# How To Fine-Tune An LLM - A Step By Step Guide
Artikel ini memberikan panduan komprehensif mengenai fine-tuning pada Large Language Model (LLM), sebuah proses adaptasi model umum menjadi ahli spesialis. Penjelasan ini mencakup perbandingan antara fine-tuning, RAG, dan prompt engineering, serta menyarankan pendekatan hibrida untuk hasil yang optimal. Fokus utama diberikan pada teknik efisien seperti LoRA dan QLoRA yang memungkinkan penggunaan perangkat keras tingkat konsumen dengan mengurangi kebutuhan memori secara signifikan melalui kuantisasi 4-bit.

## Apa itu Fine-tue?
Fine-tuning adalah bentuk transfer learning yang mengadaptasi pemahaman umum sebuah model untuk melakukan tugas-tugas yang lebih spesifik dan tertarget dengan akurasi yang lebih tinggi. Proses ini dilakukan dengan mengambil model bahasa besar yang sudah dilatih sebelumnya (pre-trained) dan melatihnya kembali secara terbatas pada dataset khusus agar performanya meningkat pada domain tertentu. Secara teknis, fine-tuning bekerja dengan menyesuaikan bobot (weights) model agar lebih sesuai dengan tugas spesifik yang diberikan

Beberapa manfaat (benefit) utama dari fine-tuning:
- **Peningkatan Akurasi dan Spesialisasi:** Fine-tuning mengubah model tujuan umum menjadi ahli spesialis yang memahami terminologi khusus domain dan unggul dalam aplikasi tertentu
- **Format Output yang Konsisten:** Memungkinkan model untuk selalu mengikuti aturan tertentu atau format data yang konsisten, seperti memastikan model selalu memberikan respons dalam format JSON
- **Mengubah Cara Bernalar:** Berbeda dengan RAG yang hanya memberikan informasi tambahan, fine-tuning dapat mengubah cara model bernalar (reasoning) dalam memproses informasi
- **Efisiensi Biaya dan Token:** Fine-tuning sering kali dapat menggantikan perintah (prompt) yang panjang dan rumit dengan instruksi yang lebih sederhana, sehingga dapat mengurangi penggunaan token dan biaya operasional
- **Adaptasi terhadap Data dan Gaya:** Organisasi dapat menciptakan solusi AI yang disesuaikan dengan data internal mereka dan mengikuti gaya penulisan tertentu
- **Efisiensi Pelatihan:** Melakukan fine-tuning jauh lebih efisien dan membutuhkan daya komputasi yang lebih kecil dibandingkan melatih model dari awal (from scratch)


## Perbedaan Antara Fine-tuning, RAG, dan Prompt Engineering

### 1. Fine-Tuning
**Mekanisme:** <br>
Merupakan bentuk transfer learning di mana model dilatih lebih lanjut pada dataset khusus untuk menyesuaikan bobot (weights) model tersebut

**Kemampuan:** <br>
Teknik ini memungkinkan model untuk memahami terminologi spesifik domain, mengikuti gaya penulisan tertentu, serta mengubah cara model bernalar (reasoning)

**Sumber Daya:** <br>
Membutuhkan investasi awal yang besar, daya komputasi tinggi (GPU), dan memori yang lebih banyak dibandingkan teknik lainnya

### 2. RAG (Retrieval-Augmented Generation)

**Mekanisme:** <br>
Teknik ini bekerja dengan menyediakan pengetahuan eksternal yang mutakhir melalui proses pengambilan (retrieval) informasi tanpa mengubah bobot model

**Kemampuan:** <br>
RAG sangat baik untuk memberikan informasi terbaru, namun tidak dapat mengubah cara model bernalar

**Kekurangan:** <br>
Performa RAG sangat bergantung pada kualitas proses pengambilan informasinya, dan bahasa yang dihasilkan model cenderung tetap bersifat umum (generalized)

### 3. Prompt Engineering

**Mekanisme:** <br>
Memberikan instruksi atau konteks tertentu langsung ke dalam input model tanpa melakukan pelatihan ulang

**Kekurangan:** <br>
Teknik ini memiliki keterbatasan pada ukuran konteks (context size), mengonsumsi banyak token, dan instruksi tersebut bisa saja tidak berfungsi lagi jika versi model berubah.

### 4. Pendekatan Hibrida
Dalam artikel ini menyarankan bahwa cara terbaik untuk mendapatkan performa optimal adalah dengan menggabungkan fine-tuning dan RAG
- Fine-tuning membantu model memahami domain dan cara berkomunikasi secara mendalam
- sementara RAG menyediakan data eksternal terbaru yang tidak ada dalam data pelatihan awal model tersebut

##  Tahapan Utama Fine-tuning LLM

### Environment Set up
Konfigurasi notenook yang digunakan:
- Python 3.11.1
- Windows 10
- GPU Model: NVIDIA GeForce RTX 4060 Laptop GPU
- CUDA Version: 12.7

### Install dan Aktifasi Paket Python
```
python -m venv fintechexplained
fintechexplained/bin/activate
```

```
# Install PyTorch 2.5.1
pip install torch==2.5.1 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Install compatible Triton
pip install triton==2.1.0

# Install transformers
pip install transformers==4.44.0

# Reinstall Unsloth
pip install --no-deps "unsloth[cu121-torch251] @ git+https://github.com/unslothai/unsloth.git"
pip install "unsloth[cu121-torch251] @ git+https://github.com/unslothai/unsloth.git"
```

**Hugging Face** dan **unsloth** digunakan untuk membantu melakukan fine-tuning LLM secara hemat biaya.
- Ekosistem Hugging Face menyediakan alat yang komprehensif untuk fine-tuning, seperti pustaka **transformers** dan **PEFT** untuk _parameter-efficient fine-tuning_
- Unsloth adalah kerangka kerja (framework) yang dioptimalkan untuk pelatihan yang lebih cepat dan lebih hemat memori. Alat ini melakukan patch pada komputer Anda untuk memungkinkan fine-tuning gratis yang 2x lebih cepat, dan **Unsloth Zoo** memastikan pelatihan berjalan lebih cepat

### Tahapan
Berikut adalah rincian dari tahapan-tahapan tersebut:
1. **Menetapkan Tujuan (Setting Objective):** Langkah pertama adalah menentukan dengan jelas apa yang ingin dicapai, misalnya melatih model agar selalu memberikan respons dalam format tertentu seperti JSON
2. **Memilih Model Dasar (Choose Base Model):** Memilih model pre-trained yang paling mendekati kebutuhan, seperti Llama, GPT, atau T5
. Penting untuk mempertimbangkan ukuran model agar seimbang antara performa dan kemampuan komputasi yang tersedia
3. **Mengonfigurasi Teknik Fine-Tuning:** Memilih antara Full Fine-Tuning (memperbarui semua parameter) atau Parameter-Efficient Fine-Tuning (PEFT)
. Teknik PEFT seperti LoRA atau QLoRA lebih disarankan karena dapat mengurangi penggunaan memori GPU hingga lebih dari 3 kali lipat dengan hanya melatih sebagian kecil parameter tambahan
4. **Menyiapkan Data Pelatihan dan Evaluasi:** Mengumpulkan contoh spesifik domain yang berkualitas tinggi dan memformatnya, misalnya sebagai percakapan antara sistem, pengguna, dan asisten
5. **Melatih Model (Train Your Model):** Menjalankan proses pelatihan dengan mengatur hyperparameters seperti learning rate (biasanya kecil, antara 1e-5 hingga 5e-5), batch size, dan jumlah epochs (biasanya 2-5 cukup)
. Selama proses ini, metrik seperti kurva loss harus dipantau
6. **Menyimpan Model:** Setelah pelatihan selesai, simpan adapter LoRA dan model yang telah digabungkan (merged model) agar dapat digunakan kembali
7. **Mengevaluasi Model:** Menguji model yang telah dilatih menggunakan data evaluasi untuk memastikan bahwa model tersebut benar-benar memenuhi tujuan awal, seperti validasi akurasi format JSON yang dihasilkan
8. **Menggunakan Model yang Telah Dilatih:** Memuat model dari penyimpanan dan menggunakannya untuk tugas inferensi nyata atau demonstrasi
.
Proses ini secara keseluruhan bertujuan untuk menyesuaikan bobot model agar lebih cocok dengan tugas tertentu, yang sering kali lebih efektif dan efisien daripada hanya mengandalkan teknik seperti prompt engineering atau RAG saja

## Parameter-Efficient Fine-Tuning (PEFT)
**Parameter-Efficient Fine-Tuning (PEFT)** adalah teknik fine-tuning yang hanya memperbarui **sebagian kecil parameter** model sementara sebagian besar parameter lainnya tetap "dibekukan" (_frozen_)

### 1. LoRA (Low-Rank Adaptation)
LoRA adalah salah satu teknik PEFT paling populer karena efektivitas dan efisiensinya
. Cara kerjanya melibatkan tiga langkah utama:
- Membekukan bobot pre-trained: Parameter asli model tidak diubah
- Menambahkan matriks yang dapat dilatih: Matriks dekomposisi peringkat rendah (low-rank decomposition matrices) dimasukkan ke dalam lapisan tertentu
- Mempelajari adaptasi: Hanya matriks kecil ini yang diperbarui selama proses pelatihan

**Manfaat utama LoRA** meliputi pengurangan parameter yang dapat dilatih hingga **10.000 kali lipat** dan penurunan kebutuhan memori GPU hingga lebih dari **3 kali lipat**

### 2. QLoRA (Quantized LoRA)
Dalam panduan ini, penulis secara spesifik mengonfigurasi QLoRA

**QLoRA** menggabungkan teknik LoRA dengan **kuantisasi 4-bit**, yang memberikan penghematan memori hingga **75%**. Hal ini memungkinkan model besar (seperti model dengan 65 miliar parameter) untuk dilatih pada GPU kelas konsumen tanpa kehilangan performa yang signifikan

### 3. Konfigurasi Teknis PEFT yang Digunakan
Dalam kode praktisnya, penulis menggunakan pustaka Unsloth dan Hugging Face PEFT dengan konfigurasi berikut:
- **Rank (r) = 16:** Digunakan untuk adaptasi peringkat rendah
- **LoRA Alpha = 16:** Faktor penskalaan untuk adaptasi tersebut
- **Target Modules:** Penyesuaian dilakukan pada bagian spesifik model seperti q_proj, k_proj, v_proj, o_proj, serta gerbang dan proyeksi lainnya
- **Gradient Checkpointing:** Menggunakan mode "unsloth" untuk menghemat memori selama pelatihan
.
### 4. Teknik PEFT Lainnya
Selain LoRA dan QLoRA, sumber juga menyebutkan beberapa teknik PEFT lain, meskipun tidak digunakan dalam latihan utama, yaitu
- **Adapter Layers:** Modul jaringan saraf kecil yang dimasukkan di antara lapisan transformer.
- **Prefix Tuning:** Menambahkan trainable prefix tokens pada urutan input.
- **Prompt Tuning:** Mempelajari embedding prompt yang optimal untuk tugas tertentu.
- **BitFit:** Hanya memperbarui istilah bias dalam model.


## Penutup
Sebagai penutup, artikel ini menekankan bahwa fine-tuning merupakan metode transfer learning yang paling efektif untuk mengubah model bahasa besar (LLM) tujuan umum menjadi ahli spesialis yang mampu menangani tugas tertentu dengan akurasi tinggi. <br>
Berikut adalah beberapa poin kunci sebagai kesimpulan akhir:
- Efisiensi dan Aksesibilitas: Dengan menggunakan teknik Parameter-Efficient Fine-Tuning (PEFT) seperti QLoRA, kebutuhan parameter yang perlu dilatih dapat dikurangi hingga 10.000 kali lipat dan penggunaan memori GPU ditekan hingga 75%.  Hal ini memungkinkan pelatihan model canggih dilakukan secara hemat biaya pada perangkat keras kelas konsumen
- Kualitas di Atas Kuantitas: Keberhasilan fine-tuning sangat bergantung pada kualitas data daripada volume data yang besar. Fokus pada contoh yang relevan dan pembersihan data dari gangguan (noise) sangat krusial untuk generalisasi model yang lebih baik
- Pendekatan Hibrida adalah Solusi Terbaik: Meskipun fine-tuning sangat unggul dalam mengubah cara model bernalar dan gaya komunikasinya, penggabungan dengan teknik RAG (Retrieval-Augmented Generation) adalah strategi optimal. Fine-tuning memberikan keahlian domain, sementara RAG menyediakan pengetahuan eksternal yang paling mutakhir
- Alur Kerja yang Terstruktur: Panduan delapan langkah yang dipaparkan—mulai dari penetapan tujuan hingga penggunaan model hasil pelatihan—memberikan kerangka kerja teknis yang solid bagi pengembang untuk membangun solusi AI yang andal dan konsisten dalam format output-nya (seperti JSON)



## Todo:
- buat scriptnya
