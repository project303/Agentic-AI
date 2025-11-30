
# 🧩 **Fitur-Fitur Flowise (Penjelasan Singkat)**

Flowise adalah platform **no-code AI builder** yang memudahkan siapa pun membangun **chatbot, AI agent, automations, dan workflow LLM** tanpa menulis kode. Berikut penjelasan fitur-fiturnya:


## **1. Chatflow**

Digunakan untuk membuat **alur percakapan** seperti:

* Chatbot
* AI Assistant
* Customer service bot
* RAG chatbot
* Model Q&A
* Summarizer

Kita cukup menarik & menyusun node seperti LLM, prompt, tool, dan input/output.

---

## **2. Agentflow**

Fitur untuk membuat **alur yang berisi banyak agent** yang dapat:

* bekerja sama,
* membagi tugas,
* menggunakan tools yang berbeda,
* saling memberikan output untuk menyelesaikan pekerjaan kompleks.

Ideal untuk **agentic AI**, misalnya:

* agent researcher
* agent planner
* agent writer
* agent evaluator

Semua bekerja otomatis berdasarkan workflow.

---

## **3. Assistant**

Fitur khusus yang memungkinkan pembuatan **Assistant** mirip seperti ChatGPT’s Assistants atau OpenAI Assistants API.

Assistant:

* bisa memiliki memori,
* bisa menggunakan tool,
* bisa menjalankan file/pipeline,
* bisa menjadi “AI Worker” dalam aplikasi kita.

---

## **4. Marketplaces**

Tempat untuk:

* mencari **template siap pakai**,
* seperti RAG chatbot, data extraction bot, AI agent, sentiment analysis, dll
* dan **meng-clone** template tersebut untuk disesuaikan dengan kebutuhan kita.

Mempermudah pemula agar tidak perlu membuat workflow dari nol.

---

## **5. Tools**

Flowise mendukung:

* memilih tools dari marketplace
* atau membuat **tools buatan sendiri**

Tools bisa berupa:

* API fetcher
* Web browser tool
* Database query tool
* File operations
* Custom function

Tools membuat agent bisa **mengambil data**, **menjalankan aksi**, bukan cuma menjawab teks.

---

## **6. Credentials**

Tempat menyimpan **API Keys** atau credential aman untuk:

* OpenAI
* Groq
* Gemini
* OpenRouter
* Weaviate
* Pinecone
* Supabase
* dan banyak lainnya

Agar flow bisa terhubung dengan layanan eksternal tanpa menyimpan kunci rahasia di node.

---

## **7. Variables**

Mengatur **global variables** yang dapat dipakai di seluruh flow.
Contoh:

* nama perusahaan
* domain website
* prompt dasar
* path folder dataset
* URL API

Fungsinya mirip environment variables pada aplikasi.

---

## **8. API Keys**

Menu khusus untuk mengatur API Keys milik Flowise (bukan model).

Ini digunakan untuk:

* memanggil Chatflow lewat API
* menghubungkan Flowise dengan aplikasi kita
* mengamankan endpoint agar tidak bisa diakses sembarang orang

---

## **9. Document Stores**

Tempat mengatur **basis knowledge** untuk RAG.
Kita bisa mengatur:

* folder dokumen
* vector database
* embeddings
* indexing
* pencarian (retrieval)

Flowise mendukung banyak document store seperti:

* ChromaDB
* Weaviate
* Pinecone
* LanceDB
* Supabase

Digunakan untuk membuat **chatbot yang bisa menjawab berdasarkan dokumen kita**.

---

# 🎯 **Kesimpulan Singkat**

Flowise menyediakan fitur lengkap untuk membangun sistem AI:

| Fitur               | Fungsi Utama                        |
| ------------------- | ----------------------------------- |
| **Chatflow**        | Buat chatbot / AI assistant no-code |
| **Agentflow**       | Buat workflow multi-agent           |
| **Assistant**       | Buat AI worker mirip Assistants API |
| **Marketplaces**    | Template siap pakai                 |
| **Tools**           | Aksi tambahan, custom tools         |
| **Credentials**     | Simpan API key aman                 |
| **Variables**       | Variabel global                     |
| **API Keys**        | Akses API flow                      |
| **Document Stores** | Knowledge base RAG                  |


