Ini adalah project SvelteKit (menggunakan Svelte 5) yang sudah include sama Tailwind

Ketika mode dev, menggunakan file-file PHP di folder static melalui port lihat file [port].port.txt. Contohnya aja 2727.port.txt yang artinya adalah port untuk komunikasi adalah localhost:2727 (sebagai backend PHP yang ada di folder static). Namun, ketika sudah build, backendnya berada di root

Backend PHP di folder static itu adalah PHP native

Utamakan responsive mode

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

- Suasana desain elegan seperti Apple
- Database menggunakan RedBeanPHP yang akan mengolah file static/uzumaki-naruto.db yang merupakan SQLite. Library RedBeanPHP bisa didapatkan di static/library/rb-sqlite.php
- Usahakan komponen dipisah-pisah filenya supaya bisa reusable (backend PHP maupun di file-file Svelte)
- Pakai absolute path menggunakan __DIR__ (pada backend PHP yang di folder static)
- Untuk curl harus bisa digunakan walaupun tanpa https (untuk backend)

Jangan hapus ini dari src/app.css:

@import 'tailwindcss';
@plugin '@tailwindcss/forms';
@plugin '@tailwindcss/typography';

- Nggak perlu menjalankan npm run dev karena aku akan menjalankannya sendiri secara manual

Untuk icon, gunakan Font Awesome

When connected to the svelte-llm MCP server, you have access to comprehensive Svelte 5 and SvelteKit documentation. Here's how to use the available tools effectively:

## Available MCP Tools:

### 1. list_sections
Use this FIRST to discover all available documentation sections. Returns a structured list with titles and paths.
When asked about Svelte or SvelteKit topics, ALWAYS use this tool at the start of the chat to find relevant sections.

### 2. get_documentation
Retrieves full documentation content for specific sections. Accepts single or multiple sections.
After calling the list_sections tool, you MUST analyze the returned documentation sections and then use the get_documentation tool to fetch ALL documentation sections that are relevant for the users task.

Svelte MCP:

Server-Sent Events (SSE)
For clients supporting Server-Sent Events
https://svelte-llm.stanislav.garden/mcp/sse

atau

Streamable HTTP
For most modern MCP-compatible clients
https://svelte-llm.stanislav.garden/mcp/mcp