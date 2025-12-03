=== DEFINISI PROJECT ===

- Ini adalah project SvelteKit (menggunakan Svelte 5) yang sudah include sama Tailwind
- Usahakan komponen dipisah-pisah filenya supaya bisa reusable (backend PHP maupun di file-file Svelte)

=== BACKEND ===

- Ketika mode dev, menggunakan file-file PHP di folder static melalui port lihat file [port].port.txt. Contohnya aja 2727.port.txt yang artinya adalah port untuk komunikasi adalah localhost:2727 (sebagai backend PHP yang ada di folder static). Namun, ketika sudah build, backendnya berada di root
- Backend PHP di folder static itu adalah PHP native
- Database menggunakan RedBeanPHP yang akan mengolah file static/uzumaki-naruto.db yang merupakan SQLite. Library RedBeanPHP bisa didapatkan di static/library/rb-sqlite.php
- Pakai absolute path menggunakan __DIR__ (pada backend PHP yang di folder static)
- Untuk curl harus bisa digunakan walaupun tanpa https (untuk backend)

=== FRONTEND ===

- Untuk instalasi paket Node JS, menggunakan pnpm, bukan npm
- Nggak perlu menjalankan npm run dev (di Qwen) karena aku akan menjalankannya sendiri secara manual

=== STYLING ===

- Utamakan responsive mode
- Suasana desain elegan seperti Apple
- Jangan hapus ini dari src/app.css:

@import 'tailwindcss';
@plugin '@tailwindcss/forms';
@plugin '@tailwindcss/typography';

- Untuk icon, gunakan Font Awesome

=== WHATSAPP ===

- Untuk kirim pesan menggunakan WhatsApp, tools yang digunakan adalah Sidobe

Berikut ini adalah contoh curl untuk bisa menggunakan layanan Sidobe:

Mengirim pesan ke nomor tertentu:

curl --location 'https://api.sidobe.com/wa/v1/send-message' \
  --header 'X-Secret-Key: bZYMoXDsIyjUVgognpKrdYylgwKzAOCHgHFjbdLeOJwosdtjAv' \
  --header 'Content-Type: application/json' \
  --data '{
      "phone": "+628123123123",
      "message": "example message"
  }'

Mengirim pesan ke grup:

curl --location 'https://api.sidobe.com/wa/v1/send-message' \
  --header 'X-Secret-Key: bZYMoXDsIyjUVgognpKrdYylgwKzAOCHgHFjbdLeOJwosdtjAv' \
  --header 'Content-Type: application/json' \
  --data '{
      "group_id": "123123@g.us",
      "message": "example message"
  }'

Contoh kalau berhasil mengirim pesan ke personal maupun ke grup:

{
  "is_success": true,
  "data": {
      "id": "1",
      "message_type": "TEXT",
      "status": "SUCCESS",
      "is_async": false
  }
}

Mengirim pesan dengan gambar:

curl --location 'https://api.sidobe.com/wa/v1/send-message-image' \
  --header 'X-Secret-Key: bZYMoXDsIyjUVgognpKrdYylgwKzAOCHgHFjbdLeOJwosdtjAv' \
  --header 'Content-Type: application/json' \
  --data '{
      "phone": "+628123123123",
      "message": "example message",
      "image_url": "https://sidobe.com/wp-content/uploads/2023/09/logo-2.png"
  }'

Contoh kalau berhasil mengirim pesan dengan gambar:

{
  "is_success": true,
  "data": {
      "id": "1",
      "message_type": "IMAGE",
      "status": "SUCCESS",
      "is_async": false
  }
}

Mengirim pesan dengan document:

curl --location 'https://api.sidobe.com/wa/v1/send-message-doc' \
  --header 'X-Secret-Key: bZYMoXDsIyjUVgognpKrdYylgwKzAOCHgHFjbdLeOJwosdtjAv' \
  --header 'Content-Type: application/json' \
  --data '{
      "phone": "+628123123123",
      "message": "example message",
      "document_url": "https://sidobe.com/example.pdf",
      "document_name": "Document Name Example"
  }'

Contoh kalau berhasil mengirim pesan dengan document:

{
  "is_success": true,
  "data": {
      "id": "1",
      "message_type": "DOCS",
      "status": "SUCCESS",
      "is_async": false
  }
}

Mendapatkan list grup:

curl --location 'https://api.sidobe.com/wa/v1/whatsapp-groups' \
--header 'X-Secret-Key: bZYMoXDsIyjUVgognpKrdYylgwKzAOCHgHFjbdLeOJwosdtjAv'

Contoh yang didapatkan:

{
  "is_success": true,
  "data": [
      {
          "id": "123123@g.us",
          "name": "Group Name Example",
          "description": "Group description example",
          "created_at": "2025-08-20T15:43:31+07:00",
          "owner_jid": "123123@lid",
          "owner_phone": "628512312321"
      }
  ]
}

Cek nomor WA:

curl --location 'https://api.sidobe.com/wa/v1/utilities/check-number' \
  --header 'X-Secret-Key: bZYMoXDsIyjUVgognpKrdYylgwKzAOCHgHFjbdLeOJwosdtjAv' \
  --header 'Content-Type: application/json' \
  --data '{
      "phone": "+628123123123"
  }'

Contoh kalau nomor WA terdaftar:

{
  "is_success": true,
  "data": {
      "is_registered": true
  }
}

=== AI ===

- Untuk AI, menggunakan Gemini dengan key AIzaSyB0ZdbSRI9TiHr6vJeYpdSvee6C11rTrTI dengan contoh curl:

curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent" \
  -H 'Content-Type: application/json' \
  -H 'X-goog-api-key: AIzaSyB0ZdbSRI9TiHr6vJeYpdSvee6C11rTrTI' \
  -X POST \
  -d '{
    "contents": [
      {
        "parts": [
          {
            "text": "Explain how AI works in a few words"
          }
        ]
      }
    ]
  }'

=== Backend Anti-Duplicate Insert (WAJIB untuk semua endpoint POST) ===

* Semua endpoint PHP yang melakukan insert **harus** memakai pola cek duplikasi berikut:

  ```php
  // Cek apakah data yang sama sudah pernah ada
  $exists = R::findOne('nama_tabel', ' field1 = ? AND field2 = ? ', [$field1, $field2]);

  if ($exists) {
      echo json_encode([
          "success" => false,
          "reason"  => "duplicate"
      ]);
      exit;
  }
  ```

* Jika data **tidak ada**, baru boleh menjalankan:

  ```php
  $bean = R::dispense('nama_tabel');
  $bean->field1 = $field1;
  $bean->field2 = $field2;
  R::store($bean);
  ```

* Semua endpoint PHP **wajib mengembalikan JSON**, terutama bila menemukan duplikasi:

  ```json
  { "success": false, "reason": "duplicate" }
  ```

* Untuk tabel yang memerlukan keunikan absolut, **wajib** menambahkan unique index SQLite:

  ```php
  R::exec('CREATE UNIQUE INDEX IF NOT EXISTS idx_nama ON nama_tabel(field1, field2)');
  ```

=== Frontend Svelte 5 (Anti Double Submit / Anti Double Request) ===

Karena Svelte 5 **tidak menggunakan `on:click` / `on:submit`**, semua request harus mengikuti pola berikut:

### **1. Gunakan “run” atau “bind:value” lalu submit melalui action sendiri**

Aturan:

* Gunakan rune `$state` untuk status loading.
* Gunakan `<form method="POST">` + `use:enhance` atau manual `fetch`.
* Setelah form dijalankan sekali, **disable submit** sampai selesai.

### Pola Standar Anti-Duplicate di Svelte 5:

```svelte
<script>
  let loading = $state(false);

  // ini dipanggil otomatis oleh enhance ketika user submit
  async function handle({ formData }) {
    if (loading) return;       // anti double
    loading = true;

    const res = await fetch('/api/input.php', {
      method: 'POST',
      body: formData
    });

    const json = await res.json();

    if (json.reason === 'duplicate') {
      alert("Data sudah ada, tidak disimpan ulang.");
    }

    loading = false;
  }
</script>

<form use:enhance={handle}>
  <!-- input di sini -->
  <button disabled={loading}>
    {loading ? "Menyimpan..." : "Simpan"}
  </button>
</form>
```

### **Peraturan:**

* GPT **tidak boleh** membuat form dengan `on:click`, `on:submit`, atau event DOM model lama.
* Semua submit harus memakai:

  * `use:enhance={handler}`
  * atau membuat **action sendiri** tetapi tetap memastikan:

    **Jika `loading === true`, request tidak boleh dikirim ulang.**

=== Rules Tambahan untuk AI ===

* Setiap kali AI membuat file Svelte yang melibatkan form atau penyimpanan, AI harus:

  * Membuat variabel loading dengan `$state`.
  * Membuat pengecekan `if (loading) return`.
  * Men-disable tombol submit ketika loading.
  * Menampilkan handling "duplicate" jika backend mengirim `reason: duplicate`.

* AI **tidak boleh menghasilkan** kode yang mengirim request 2x (misal: fetch + default form submit bersamaan).

* AI harus menganggap bahwa backend PHP *selalu* berada di:

  * dev: port pada file `.port.txt`
  * build: root backend

=== Upload Gambar ===

Untuk halaman-halaman tertentu yang ada fitur upload gambar, berarti pas pilih gambar itu, dia otomatis terupload ke folder temporary image dan di frontendnya itu otomatis tampilkan preview gambarnya. Terus, di form tersebut ketika submit, maka gambarnya dipindahkan ke folder image. 

Ketika mengedit atau menghapus record, maka secara otomatis juga berpengaruh pada gambar. Misalnya aja jika mengedit form dengan cara mengganti gambar, berarti kan gambar sebelumnya dihapus dan diganti dengan yang baru. Lalu, jika menghapus record, maka gambar juga dihapus