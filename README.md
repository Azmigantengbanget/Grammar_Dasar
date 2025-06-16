<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [



  { "en": "Apa itu kata benda?", "id": "Nama orang, tempat, benda." },
  { "en": "Contoh kata benda?", "id": "Meja, buku, kucing, Budi." },
  { "en": "Apa itu kata kerja?", "id": "Menunjukkan suatu tindakan." },
  { "en": "Contoh kata kerja?", "id": "Makan, tidur, berlari." },
  { "en": "Apa itu kata sifat?", "id": "Menjelaskan kata benda." },
  { "en": "Contoh kata sifat?", "id": "Besar, cantik, pintar, merah." },
  { "en": "Apa itu kata ganti?", "id": "Pengganti kata benda." },
  { "en": "Kata ganti orang pertama?", "id": "Saya, aku, kami." },
  { "en": "Kata ganti orang kedua?", "id": "Kamu, Anda, kalian." },
  { "en": "Kata ganti orang ketiga?", "id": "Dia, ia, mereka." },
  { "en": "Apa fungsi subjek?", "id": "Pelaku dalam kalimat." },
  { "en": "Apa fungsi predikat?", "id": "Tindakan dalam kalimat." },
  { "en": "Apa fungsi objek?", "id": "Yang dikenai tindakan." },
  { "en": "Apa kepanjangan S-P-O-K?", "id": "Subjek, Predikat, Objek, Keterangan." },
  { "en": "Unsur utama kalimat?", "id": "Subjek dan Predikat." },
  { "en": "Tanda akhir kalimat berita?", "id": "Tanda titik (.)." },
  { "en": "Tanda akhir kalimat tanya?", "id": "Tanda tanya (?)." },
  { "en": "Tanda akhir kalimat perintah?", "id": "Tanda seru (!)." },
  { "en": "Imbuhan di awal kata?", "id": "Awalan atau prefiks." },
  { "en": "Imbuhan di akhir kata?", "id": "Akhiran atau sufiks." },
  { "en": "Imbuhan di tengah kata?", "id": "Sisipan atau infiks." },
  { "en": "Contoh awalan?", "id": "me-, ber-, di-, ter-." },
  { "en": "Contoh akhiran?", "id": "-kan, -i, -an." },
  { "en": "Imbuhan 'me-' pada 'baca'?", "id": "Menjadi 'membaca'." },
  { "en": "Imbuhan 'di-' pada 'makan'?", "id": "Menjadi 'dimakan'." },
  { "en": "Kalimat dengan imbuhan 'di-'?", "id": "Disebut kalimat pasif." },
  { "en": "Kalimat dengan imbuhan 'me-'?", "id": "Disebut kalimat aktif." },
  { "en": "'Buku' termasuk kata apa?", "id": "Kata benda (nomina)." },
  { "en": "'Menulis' termasuk kata apa?", "id": "Kata kerja (verba)." },
  { "en": "'Cepat' termasuk kata apa?", "id": "Kata sifat (adjektiva)." },
  { "en": "Kata depan penunjuk tempat?", "id": "Di, ke, dari." },
  { "en": "Perbedaan 'di' dan 'ke'?", "id": "'Di' untuk lokasi, 'ke' tujuan." },
  { "en": "Contoh kata depan?", "id": "Di, ke, dari, pada." },
  { "en": "Apa itu kata hubung?", "id": "Menghubungkan kata atau kalimat." },
  { "en": "Contoh kata hubung?", "id": "Dan, atau, tetapi." },
  { "en": "'Saya dan kamu'. Kata hubungnya?", "id": "Kata 'dan'." },
  { "en": "Apa itu sinonim?", "id": "Persamaan atau padanan kata." },
  { "en": "Sinonim kata 'pintar'?", "id": "Cerdas, pandai." },
  { "en": "Apa itu antonim?", "id": "Lawan kata." },
  { "en": "Antonim kata 'panas'?", "id": "Dingin." },
  { "en": "Fungsi tanda koma?", "id": "Pemisah unsur dalam kalimat." },
  { "en": "Bentuk dasar 'berlari'?", "id": "Kata dasarnya 'lari'." },
  { "en": "Bentuk dasar 'makanan'?", "id": "Kata dasarnya 'makan'." },
  { "en": "Imbuhan pada 'lukisan'?", "id": "Akhiran '-an'." },
  { "en": "Imbuhan pada 'pemain'?", "id": "Awalan 'pe-' dan akhiran '-an'." },
  { "en": "Kata ganti kepunyaan 'saya'?", "id": "-ku, atau 'saya'." },
  { "en": "Contoh kata ganti kepunyaan?", "id": "Bukuku, rumahmu, mobilnya." },
  { "en": "Apa itu kalimat tunggal?", "id": "Memiliki satu pola S-P." },
  { "en": "Apa itu kalimat majemuk?", "id": "Gabungan beberapa kalimat tunggal." },
  { "en": "'Ayah bekerja'. Predikatnya?", "id": "Kata 'bekerja'." },
  { "en": "'Ibu memasak nasi'. Objeknya?", "id": "Kata 'nasi'." },
  { "en": "'Adik bermain di taman'. Keterangannya?", "id": "Frasa 'di taman'." },
  { "en": "Jenis keterangan 'di taman'?", "id": "Keterangan tempat." },
  { "en": "Jenis keterangan 'kemarin sore'?", "id": "Keterangan waktu." },
  { "en": "Apa itu kata seru?", "id": "Ungkapan perasaan." },
  { "en": "Contoh kata seru?", "id": "Wah, aduh, amboi." },
  { "en": "Apa itu kata sandang?", "id": "Menemani kata benda." },
  { "en": "Contoh kata sandang?", "id": "Si, sang, para." },
  { "en": "'Sang Kancil'. Kata sandangnya?", "id": "Kata 'Sang'." },
  { "en": "Fungsi awalan 'ber-'?", "id": "Membentuk kata kerja." },
  { "en": "Contoh kata berawalan 'ber-'?", "id": "Berjalan, berenang, berpikir." },
  { "en": "Fungsi awalan 'ter-'?", "id": "Menyatakan ketidaksengajaan atau paling." },
  { "en": "Contoh 'ter-' tidak sengaja?", "id": "Terjatuh, tertidur." },
  { "en": "Contoh 'ter-' paling?", "id": "Terbaik, tertinggi, tercantik." },
  { "en": "Apa itu kata ulang?", "id": "Bentuk kata yang diulang." },
  { "en": "Contoh kata ulang?", "id": "Buku-buku, lari-lari." },
  { "en": "Arti kata ulang 'buku-buku'?", "id": "Menyatakan jumlah banyak." },
  { "en": "Arti kata ulang 'lari-lari'?", "id": "Tindakan dilakukan santai." },
  { "en": "Apa itu frasa?", "id": "Gabungan dua kata." },
  { "en": "Contoh frasa benda?", "id": "Buku baru, rumah besar." },
  { "en": "Contoh frasa kerja?", "id": "Sedang makan, akan pergi." },
  { "en": "Struktur frasa 'buku baru'?", "id": "Diterangkan-Menerangkan (DM)." },
  { "en": "Struktur frasa 'sedang makan'?", "id": "Menerangkan-Diterangkan (MD)." },
  { "en": "Penulisan 'di' sebagai awalan?", "id": "Digabung dengan kata berikutnya." },
  { "en": "Penulisan 'di' sebagai kata depan?", "id": "Dipisah dari kata berikutnya." },
  { "en": "Contoh 'di-' sebagai awalan?", "id": "Dibeli, ditulis, dibaca." },
  { "en": "Contoh 'di' sebagai kata depan?", "id": "Di rumah, di sekolah." },
  { "en": "Apa itu kalimat perintah?", "id": "Kalimat yang menyuruh." },
  { "en": "Ciri kalimat perintah?", "id": "Berakhiran 'lah' atau 'kan'." },
  { "en": "Contoh kalimat perintah?", "id": "Tutuplah pintu itu!" },
  { "en": "Apa itu kalimat larangan?", "id": "Kalimat yang melarang." },
  { "en": "Kata penanda kalimat larangan?", "id": "Jangan." },
  { "en": "Contoh kalimat larangan?", "id": "Jangan buang sampah sembarangan." },
  { "en": "'Membaca' kata kerja apa?", "id": "Kata kerja transitif." },
  { "en": "'Tidur' kata kerja apa?", "id": "Kata kerja intransitif." },
  { "en": "Apa itu verba transitif?", "id": "Kata kerja butuh objek." },
  { "en": "Apa itu verba intransitif?", "id": "Tidak memerlukan objek." },
  { "en": "Fungsi akhiran '-kan'?", "id": "Membentuk kata kerja." },
  { "en": "Contoh kata berakhiran '-kan'?", "id": "Ambilkan, belikan, buatkan." },
  { "en": "Fungsi akhiran '-i'?", "id": "Membentuk kata kerja." },
  { "en": "Contoh kata berakhiran '-i'?", "id": "Panasi, gulai, sakiti." },
  { "en": "Apa itu partikel?", "id": "Kata tugas penegas." },
  { "en": "Contoh partikel?", "id": "-lah, -kah, -tah, pun." },
  { "en": "Fungsi partikel '-kah'?", "id": "Memperjelas kalimat tanya." },
  { "en": "Fungsi partikel '-lah'?", "id": "Menghaluskan kalimat perintah." },
  { "en": "Apa itu nomina?", "id": "Istilah lain kata benda." },
  { "en": "Apa itu verba?", "id": "Istilah lain kata kerja." },
{ "en": "Apa itu kata majemuk?", "id": "Gabungan kata bermakna baru." },
  { "en": "Contoh kata majemuk?", "id": "Rumah sakit, kacamata, matahari." },
  { "en": "Penulisan 'kereta api'?", "id": "Ditulis terpisah." },
  { "en": "Penulisan 'tanggung jawab'?", "id": "Ditulis terpisah." },
  { "en": "Penulisan 'saputangan'?", "id": "Ditulis serangkai (digabung)." },
  { "en": "Imbuhan 'pe-' pada 'baca'?", "id": "Menjadi 'pembaca'." },
  { "en": "Imbuhan 'pe-' pada 'tani'?", "id": "Menjadi 'petani'." },
  { "en": "Fungsi imbuhan 'pe-'?", "id": "Membentuk nomina (pelaku)." },
  { "en": "Imbuhan 'ke-an' pada 'baik'?", "id": "Menjadi 'kebaikan'." },
  { "en": "Fungsi imbuhan 'ke-an'?", "id": "Membentuk nomina abstrak." },
  { "en": "Contoh kata berimbuhan 'ke-an'?", "id": "Kesehatan, kebersihan, keindahan." },
  { "en": "Fungsi awalan 'per-'?", "id": "Membentuk verba (kausatif)." },
  { "en": "Awalan 'per-' pada 'dalam'?", "id": "Menjadi 'perdalam'." },
  { "en": "Contoh kalimat inversi?", "id": "Pergilah ia ke pasar." },
  { "en": "Apa itu kalimat inversi?", "id": "Kalimat berpredikat di depan." },
  { "en": "Pola kalimat inversi?", "id": "Predikat - Subjek (P-S)." },
  { "en": "Kata hubung kalimat setara?", "id": "Dan, atau, tetapi, lalu." },
  { "en": "Kata hubung kalimat bertingkat?", "id": "Karena, ketika, jika, bahwa." },
  { "en": "Apa itu anak kalimat?", "id": "Klausa yang bergantung induknya." },
  { "en": "Apa itu induk kalimat?", "id": "Klausa yang dapat berdiri." },
  { "en": "'Dia menangis karena sedih'. Induknya?", "id": "'Dia menangis'." },
  { "en": "'Dia menangis karena sedih'. Anaknya?", "id": "'karena sedih'." },
  { "en": "Apa itu kalimat langsung?", "id": "Ucapan yang dikutip langsung." },
  { "en": "Tanda baca kalimat langsung?", "id": "Tanda petik dua ( \"...\" )." },
  { "en": "Contoh kalimat langsung?", "id": "\"Saya pergi,\" katanya." },
  { "en": "Apa itu kalimat tak langsung?", "id": "Kalimat yang melaporkan ucapan." },
  { "en": "Contoh kalimat tak langsung?", "id": "Dia berkata bahwa dia pergi." },
  { "en": "Awalan 'se-' berarti apa?", "id": "Satu, seluruh, atau sama." },
  { "en": "Contoh awalan 'se-'?", "id": "Serumah, sekampung, secantik." },
  { "en": "Apa itu kata bilangan?", "id": "Kata yang menyatakan jumlah." },
  { "en": "Jenis kata bilangan?", "id": "Utama dan tingkatan." },
  { "en": "Contoh kata bilangan utama?", "id": "Satu, dua, seratus." },
  { "en": "Contoh kata bilangan tingkat?", "id": "Pertama, kedua, keseratus." },
  { "en": "Fungsi huruf kapital?", "id": "Awal kalimat, nama diri." },
  { "en": "Penulisan nama hari?", "id": "Menggunakan huruf kapital." },
  { "en": "Penulisan nama kota?", "id": "Menggunakan huruf kapital." },
  { "en": "Penulisan gelar akademik?", "id": "Diawali huruf kapital." },
  { "en": "Fungsi tanda titik dua (:)?", "id": "Untuk memulai perincian." },
  { "en": "Contoh penggunaan titik dua?", "id": "Beli: buku, pensil, tas." },
  { "en": "Apa itu homonim?", "id": "Sama lafal, sama tulisan." },
  { "en": "Contoh homonim?", "id": "Bisa (dapat), bisa (racun)." },
  { "en": "Apa itu homofon?", "id": "Sama lafal, beda tulisan." },
  { "en": "Contoh homofon?", "id": "Bank (uang), bang (kakak)." },
  { "en": "Apa itu homograf?", "id": "Sama tulisan, beda lafal." },
  { "en": "Contoh homograf?", "id": "Apel (buah), apel (upacara)." },
  { "en": "Kata ulang 'meja-meja'?", "id": "Menyatakan banyak (jamak)." },
  { "en": "Kata ulang 'mondar-mandir'?", "id": "Kata ulang berubah bunyi." },
  { "en": "Nama lain kata ulang berubah bunyi?", "id": "Dwilingga salin suara." },
  { "en": "Kata ulang 'dedaunan'?", "id": "Menyatakan banyak dan beragam." },
  { "en": "Kata ulang 'lelaki'?", "id": "Kata ulang semu." },
  { "en": "Bentuk dasar 'penggunaan'?", "id": "Kata dasarnya 'guna'." },
  { "en": "Imbuhan pada 'penggunaan'?", "id": "pe-an." },
  { "en": "'Dengan cepat' keterangan apa?", "id": "Keterangan cara." },
  { "en": "'Setiap hari' keterangan apa?", "id": "Keterangan frekuensi (kekerapan)." },
  { "en": "'Sangat' termasuk kata apa?", "id": "Kata keterangan derajat." },
  { "en": "Apa itu klausa?", "id": "Kelompok kata berpola S-P." },
  { "en": "Perbedaan klausa dan kalimat?", "id": "Kalimat berintonasi akhir." },
  { "en": "Imbuhan 'memper-kan'?", "id": "Contoh: memperkenalkan." },
  { "en": "Imbuhan 'diper-kan'?", "id": "Contoh: diperkenalkan." },
  { "en": "Fungsi imbuhan 'memper-kan'?", "id": "Menjadikan objek berbuat sesuatu." },
  { "en": "Kata 'para' fungsinya?", "id": "Menyatakan jamak untuk manusia." },
  { "en": "Contoh penggunaan 'para'?", "id": "Para tamu, para siswa." },
  { "en": "'Para hadirin' salah karena?", "id": "Hadirin sudah berarti jamak." },
  { "en": "'Banyak buku-buku' salah karena?", "id": "'Banyak' sudah jamak." },
  { "en": "Bentuk benar 'banyak buku-buku'?", "id": "Banyak buku atau buku-buku." },
  { "en": "Apa itu akronim?", "id": "Singkatan berupa gabungan huruf." },
  { "en": "Contoh akronim?", "id": "SIM, KTP, UNICEF." },
  { "en": "Apa itu singkatan?", "id": "Bentuk yang dipendekkan." },
  { "en": "Contoh singkatan nama orang?", "id": "A.S. Laksana." },
  { "en": "Apa itu konjungsi temporal?", "id": "Kata hubung waktu." },
  { "en": "Contoh konjungsi temporal?", "id": "Kemudian, selanjutnya, setelah." },
  { "en": "Apa itu konjungsi kausal?", "id": "Kata hubung sebab-akibat." },
  { "en": "Contoh konjungsi kausal?", "id": "Sebab, karena, sehingga." },
  { "en": "Apa itu afiks?", "id": "Istilah lain untuk imbuhan." },
  { "en": "Proses penambahan afiks?", "id": "Disebut afiksasi." },
  { "en": "Bentuk dasar 'ketinggian'?", "id": "Kata dasarnya 'tinggi'." },
  { "en": "Imbuhan pada 'ketinggian'?", "id": "ke-an." },
  { "en": "Kata 'adalah' termasuk?", "id": "Verba kopulatif." },
  { "en": "Fungsi 'adalah' atau 'ialah'?", "id": "Penghubung subjek dan komplemen." },
  { "en": "Apa itu pelengkap?", "id": "Melengkapi predikat." },
  { "en": "'Dia bermain bola'. 'Bola'?", "id": "Objek." },
  { "en": "'Dia menjadi polisi'. 'Polisi'?", "id": "Pelengkap." },
  { "en": "Objek bisa jadi subjek?", "id": "Ya, dalam kalimat pasif." },
  { "en": "Pelengkap bisa jadi subjek?", "id": "Tidak bisa." },
  { "en": "Fungsi imbuhan '-wan'?", "id": "Menyatakan ahli atau pelaku." },
  { "en": "Contoh imbuhan '-wan'?", "id": "Wartawan, sejarawan, ilmuwan." },
  { "en": "Imbuhan untuk perempuan?", "id": "Akhiran '-wati'." },
  { "en": "Contoh imbuhan '-wati'?", "id": "Karyawati, seniwati." },
  { "en": "Apa itu pronomina?", "id": "Istilah lain kata ganti." },
  { "en": "Pronomina penunjuk?", "id": "Ini, itu." },
  { "en": "Pronomina penanya?", "id": "Apa, siapa, kapan, mana." },
  { "en": "Kata 'siapa' untuk menanyakan?", "id": "Menanyakan orang atau subjek." },
  { "en": "Kata 'apa' untuk menanyakan?", "id": "Menanyakan benda atau hal." },
{ "en": "Apa itu makna denotasi?", "id": "Makna harfiah atau sebenarnya." },
  { "en": "Apa itu makna konotasi?", "id": "Makna kiasan atau tambahan." },
  { "en": "Makna 'kambing hitam'?", "id": "Orang yang dipersalahkan." },
  { "en": "'Kambing hitam' termasuk makna?", "id": "Konotasi (makna kiasan)." },
  { "en": "Makna 'tangan kanan'?", "id": "Orang kepercayaan." },
  { "en": "Makna 'panjang tangan'?", "id": "Suka mencuri." },
  { "en": "Makna 'buah bibir'?", "id": "Bahan pembicaraan orang." },
  { "en": "Apa itu ungkapan/idiom?", "id": "Gabungan kata bermakna kiasan." },
  { "en": "Awalan 'me-' + 'kupas'?", "id": "Menjadi 'mengupas'." },
  { "en": "Awalan 'me-' + 'tulis'?", "id": "Menjadi 'menulis'." },
  { "en": "Awalan 'me-' + 'sapu'?", "id": "Menjadi 'menyapu'." },
  { "en": "Awalan 'me-' + 'pukul'?", "id": "Menjadi 'memukul'." },
  { "en": "Kapan 'me-' menjadi 'meng-'?", "id": "Saat bertemu k, g, h, vokal." },
  { "en": "Kapan 'me-' menjadi 'men-'?", "id": "Saat bertemu d, t, c, j, z." },
  { "en": "Kapan 'me-' menjadi 'meny-'?", "id": "Saat bertemu huruf 's'." },
  { "en": "Kapan 'me-' menjadi 'mem-'?", "id": "Saat bertemu b, p, f." },
  { "en": "Kapan 'me-' tetap 'me-'?", "id": "Saat bertemu l, m, n, r, w, y." },
  { "en": "Awalan 'pe-' + 'surat'?", "id": "Menjadi 'penyurat'." },
  { "en": "Awalan 'pe-' + 'gambar'?", "id": "Menjadi 'penggambar'." },
  { "en": "Awalan 'pe-' + 'baca'?", "id": "Menjadi 'pembaca'." },
  { "en": "Apa itu majas?", "id": "Gaya bahasa kiasan." },
  { "en": "Apa itu majas personifikasi?", "id": "Benda mati bertingkah manusiawi." },
  { "en": "Contoh personifikasi?", "id": "Nyiur melambai di pantai." },
  { "en": "Apa itu majas metafora?", "id": "Perbandingan langsung." },
  { "en": "Contoh metafora?", "id": "Raja siang membakar kulit." },
  { "en": "Apa itu majas simile?", "id": "Perbandingan dengan kata hubung." },
  { "en": "Contoh simile?", "id": "Wajahnya laksana bulan." },
  { "en": "Kata pembanding majas simile?", "id": "Seperti, bagai, laksana, bak." },
  { "en": "Apa itu majas hiperbola?", "id": "Gaya bahasa melebih-lebihkan." },
  { "en": "Contoh hiperbola?", "id": "Teriakannya menggelegar." },
  { "en": "Apa itu kalimat efektif?", "id": "Singkat, jelas, tidak ambigu." },
  { "en": "Apa itu pleonasme?", "id": "Penggunaan kata yang berlebihan." },
  { "en": "Contoh pleonasme?", "id": "Maju ke depan." },
  { "en": "Perbaikan 'maju ke depan'?", "id": "Cukup 'maju' saja." },
  { "en": "Perbaikan 'saling pukul-memukul'?", "id": "Saling memukul atau pukul-memukul." },
  { "en": "Fungsi huruf miring?", "id": "Menegaskan kata, istilah asing." },
  { "en": "Contoh penggunaan huruf miring?", "id": "Baca buku *Laskar Pelangi*." },
  { "en": "Fungsi tanda hubung (-)?", "id": "Menyambung unsur kata ulang." },
  { "en": "Penulisan 'ke' sebagai angka?", "id": "ke-25 (pakai tanda hubung)." },
  { "en": "Apa itu klausa adjektival?", "id": "Klausa yang menerangkan nomina." },
  { "en": "Contoh klausa adjektival?", "id": "Baju *yang bagus itu*." },
  { "en": "Apa itu klausa adverbial?", "id": "Klausa yang menerangkan verba." },
  { "en": "Konjungsi subordinatif?", "id": "Menghubungkan induk dan anak." },
  { "en": "Konjungsi 'agar' menyatakan?", "id": "Tujuan." },
  { "en": "Konjungsi 'seandainya' menyatakan?", "id": "Pengandaian." },
  { "en": "Konjungsi 'walaupun' menyatakan?", "id": "Perlawanan (konsesif)." },
  { "en": "Konjungsi korelatif?", "id": "Kata hubung berpasangan." },
  { "en": "Contoh konjungsi korelatif?", "id": "Baik... maupun..., tidak hanya..." },
  { "en": "Apa itu kalimat ambigu?", "id": "Kalimat bermakna ganda." },
  { "en": "Contoh kalimat ambigu?", "id": "Istri perwira yang ramah." },
  { "en": "Makna ganda kalimat itu?", "id": "Istri atau perwiranya ramah?" },
  { "en": "Apa itu elipsis?", "id": "Penghilangan unsur kalimat." },
  { "en": "Contoh elipsis?", "id": "Saya ke pasar, dia juga." },
  { "en": "Unsur yang dihilangkan?", "id": "Predikat 'ke pasar'." },
  { "en": "Fungsi akhiran '-isme'?", "id": "Menyatakan paham atau aliran." },
  { "en": "Contoh kata berakhiran '-isme'?", "id": "Nasionalisme, idealisme." },
  { "en": "Fungsi akhiran '-is'?", "id": "Menyatakan orang (pelaku/ahli)." },
  { "en": "Contoh kata berakhiran '-is'?", "id": "Idealis, novelis, gitaris." },
  { "en": "Fungsi akhiran '-wi'?", "id": "Menyatakan sifat atau berhubungan." },
  { "en": "Contoh kata berakhiran '-wi'?", "id": "Manusiawi, duniawi." },
  { "en": "Apa itu preposisi?", "id": "Istilah lain kata depan." },
  { "en": "Preposisi 'pada' untuk?", "id": "Menunjukkan waktu atau orang." },
  { "en": "Contoh penggunaan 'pada'?", "id": "Pada hari Minggu." },
  { "en": "Apa itu numeralia?", "id": "Istilah lain kata bilangan." },
  { "en": "Numeralia kolektif?", "id": "Kata bilangan kumpulan." },
  { "en": "Contoh numeralia kolektif?", "id": "Kedua, ketiga, berdua." },
  { "en": "'Beberapa' termasuk kata apa?", "id": "Kata bilangan tak tentu." },
  { "en": "'Seluruh' termasuk kata apa?", "id": "Kata bilangan tak tentu." },
  { "en": "Fungsi kalimat tanya retoris?", "id": "Bertanya tanpa perlu jawaban." },
  { "en": "Contoh kalimat retoris?", "id": "Siapa tak ingin bahagia?" },
  { "en": "Apa itu kata serapan?", "id": "Kata dari bahasa lain." },
  { "en": "Contoh kata serapan?", "id": "Apotek, bus, sistem, foto." },
  { "en": "Kata 'apotik' atau 'apotek'?", "id": "Yang baku 'apotek'." },
  { "en": "Kata 'nasehat' atau 'nasihat'?", "id": "Yang baku 'nasihat'." },
  { "en": "Kata 'resiko' atau 'risiko'?", "id": "Yang baku 'risiko'." },
  { "en": "Apa itu kata baku?", "id": "Kata sesuai kaidah (KBBI)." },
  { "en": "Apa itu kata tidak baku?", "id": "Kata tidak sesuai kaidah." },
  { "en": "Fungsi tanda pisah (—)?", "id": "Membatasi penyisipan kata." },
  { "en": "Fungsi tanda kurung ()?", "id": "Mengapit tambahan keterangan." },
  { "en": "'Bunga desa' adalah contoh?", "id": "Metafora." },
  { "en": "Makna 'bunga desa'?", "id": "Gadis tercantik di desa." },
  { "en": "Bentuk dasar 'memesona'?", "id": "Pesona." },
  { "en": "Bentuk dasar 'memercayai'?", "id": "Percaya." },
  { "en": "Kenapa 'p' tidak luluh?", "id": "Karena setelah awalan ada konsonan." },
  { "en": "'me-' + 'proses' menjadi?", "id": "Memproses." },
  { "en": "'me-' + 'klarifikasi' menjadi?", "id": "Mengklarifikasi." },
  { "en": "Apa itu paragraf?", "id": "Kumpulan beberapa kalimat." },
  { "en": "Unsur utama paragraf?", "id": "Gagasan utama dan penjelas." },
  { "en": "Kalimat utama ada di?", "id": "Awal (deduktif) atau akhir (induktif)." },
  { "en": "Apa itu paragraf deduktif?", "id": "Kalimat utama di awal." },
  { "en": "Apa itu paragraf induktif?", "id": "Kalimat utama di akhir." },
  { "en": "Apa itu kohesi?", "id": "Kepaduan bentuk (kata)." },
  { "en": "Apa itu koherensi?", "id": "Kepaduan makna (ide)." },
  { "en": "Paragraf baik harus punya?", "id": "Kohesi dan koherensi." },
{ "en": "Apa itu paragraf deskripsi?", "id": "Paragraf yang menggambarkan objek." },
  { "en": "Apa itu paragraf narasi?", "id": "Paragraf yang menceritakan peristiwa." },
  { "en": "Apa itu paragraf argumentasi?", "id": "Paragraf berisi pendapat & bukti." },
  { "en": "Apa itu paragraf persuasi?", "id": "Paragraf yang mengajak pembaca." },
  { "en": "Paragraf campuran adalah?", "id": "Gagasan utama awal dan akhir." },
  { "en": "Imbuhan 'per-an' pada 'dagang'?", "id": "Menjadi 'perdagangan'." },
  { "en": "Fungsi imbuhan 'per-an'?", "id": "Menyatakan hal atau hasil." },
  { "en": "Imbuhan 'ber-an' pada 'terbang'?", "id": "Menjadi 'berterbangan'." },
  { "en": "Arti imbuhan 'ber-an'?", "id": "Banyak & tidak beraturan." },
  { "en": "Contoh kata imbuhan 'ber-an'?", "id": "Berjatuhan, berlarian, bermunculan." },
  { "en": "Beda pasif 'di-' dan 'ter-'?", "id": "Sengaja vs tidak sengaja." },
  { "en": "'Buku itu terbawa olehnya'. Sengaja?", "id": "Tidak, berarti tidak sengaja." },
  { "en": "Pasif untuk orang pertama/kedua?", "id": "Menggunakan ku- atau kau-." },
  { "en": "Contoh pasif orang pertama?", "id": "Buku itu kubaca." },
  { "en": "Contoh pasif orang kedua?", "id": "Surat itu kaubaca." },
  { "en": "Majas merendahkan diri?", "id": "Majas litotes." },
  { "en": "Contoh majas litotes?", "id": "Terimalah hadiah tak berharga ini." },
  { "en": "Majas menghaluskan ucapan?", "id": "Majas eufemisme." },
  { "en": "Contoh eufemisme?", "id": "Tuna netra (buta)." },
  { "en": "Majas sindiran halus?", "id": "Majas ironi." },
  { "en": "Contoh majas ironi?", "id": "Cepat sekali kau datang (terlambat)." },
  { "en": "Majas sindiran kasar?", "id": "Majas sarkasme." },
  { "en": "Contoh majas sarkasme?", "id": "Otakmu sekecil udang!" },
  { "en": "Majas mengumpamakan manusia?", "id": "Majas asosiasi (simile)." },
  { "en": "Fungsi titik koma (;)?", "id": "Pemisah klausa setara kompleks." },
  { "en": "Fungsi tanda elipsis (...)?", "id": "Menunjukkan kalimat yang terputus." },
  { "en": "Apa itu keterangan aposisi?", "id": "Keterangan penjelas nomina." },
  { "en": "Contoh keterangan aposisi?", "id": "Jokowi, Presiden RI, ... ." },
  { "en": "Fungsi keterangan aposisi?", "id": "Memberikan informasi tambahan subjek." },
  { "en": "Apa itu ragam bahasa?", "id": "Variasi bahasa menurut pemakaian." },
  { "en": "Ragam bahasa untuk pidato?", "id": "Ragam bahasa resmi (baku)." },
  { "en": "Ragam bahasa untuk teman?", "id": "Ragam bahasa santai (nonbaku)." },
  { "en": "Penulisan 'antar kota'?", "id": "Digabung, 'antarkota'." },
  { "en": "Penulisan 'pasca sarjana'?", "id": "Digabung, 'pascasarjana'." },
  { "en": "Awalan 'antar-' dan 'pasca-'?", "id": "Termasuk bentuk terikat." },
  { "en": "Bentuk terikat ditulis?", "id": "Serangkai dengan kata berikutnya." },
  { "en": "Kesalahan 'sangat pintar sekali'?", "id": "Pemborosan kata (pleonasme)." },
  { "en": "Pilihan yang benar?", "id": "Sangat pintar atau pintar sekali." },
  { "en": "Fungsi kata 'daripada'?", "id": "Untuk membandingkan." },
  { "en": "Contoh penggunaan 'daripada'?", "id": "Apel lebih manis daripada jeruk." },
  { "en": "Fungsi kata 'dari'?", "id": "Menunjukkan asal atau bahan." },
  { "en": "Contoh penggunaan 'dari'?", "id": "Cincin terbuat dari emas." },
  { "en": "Apa itu konversi/derivasi nol?", "id": "Kata tanpa imbuhan beda kelas." },
  { "en": "Contoh konversi?", "id": "Cangkul (benda), cangkul (kerja)." },
  { "en": "Akhiran '-if' membentuk kata?", "id": "Kata sifat (adjektiva)." },
  { "en": "Contoh kata berakhiran '-if'?", "id": "Aktif, sportif, negatif." },
  { "en": "Akhiran '-al' membentuk kata?", "id": "Kata sifat (adjektiva)." },
  { "en": "Contoh kata berakhiran '-al'?", "id": "Formal, normal, legal." },
  { "en": "Akhiran '-iah' membentuk kata?", "id": "Kata sifat (adjektiva)." },
  { "en": "Contoh kata berakhiran '-iah'?", "id": "Alamiah, lahiriah, ilmiah." },
  { "en": "Apa itu ambiguitas leksikal?", "id": "Makna ganda karena kata." },
  { "en": "Contoh ambiguitas leksikal?", "id": "Kata 'bisa' (racun/dapat)." },
  { "en": "Apa itu ambiguitas struktural?", "id": "Makna ganda karena struktur." },
  { "en": "Contoh ambiguitas struktural?", "id": "Orang tua (ayah-ibu/tua)." },
  { "en": "Penulisan 'Maha Esa'?", "id": "Dipisah, Maha Esa." },
  { "en": "Penulisan 'Mahakuasa'?", "id": "Digabung, Mahakuasa." },
  { "en": "Kata 'analisa' atau 'analisis'?", "id": "Yang baku 'analisis'." },
  { "en": "Kata 'karir' atau 'karier'?", "id": "Yang baku 'karier'." },
  { "en": "Kata 'frekwensi' atau 'frekuensi'?", "id": "Yang baku 'frekuensi'." },
  { "en": "Kata 'jadual' atau 'jadwal'?", "id": "Yang baku 'jadwal'." },
  { "en": "Kata 'obyek' atau 'objek'?", "id": "Yang baku 'objek'." },
  { "en": "Bentuk jamak 'hadirin'?", "id": "Hadirin (sudah jamak)." },
  { "en": "'Para hadirin' salah karena?", "id": "Jamak bertemu jamak." },
  { "en": "Apa itu pola frasa DM?", "id": "Diterangkan - Menerangkan." },
  { "en": "Contoh pola frasa DM?", "id": "Gadis cantik." },
  { "en": "Apa itu pola frasa MD?", "id": "Menerangkan - Diterangkan." },
  { "en": "Contoh pola frasa MD?", "id": "Sangat cantik." },
  { "en": "Pola frasa bahasa Indonesia?", "id": "Umumnya berpola Diterangkan-Menerangkan." },
  { "en": "Kata ganti tak tentu?", "id": "Seseorang, sesuatu, barang siapa." },
  { "en": "Fungsi partikel 'pun'?", "id": "Menyatakan juga atau bahkan." },
  { "en": "Penulisan partikel 'pun'?", "id": "Dipisah dari kata sebelumnya." },
  { "en": "Contoh penulisan 'pun'?", "id": "Saya pun ikut pergi." },
  { "en": "Pengecualian penulisan 'pun'?", "id": "Digabung pada konjungsi." },
  { "en": "Contoh 'pun' yang digabung?", "id": "Meskipun, walaupun, adapun." },
  { "en": "Apa itu kalimat seru?", "id": "Mengungkapkan kekaguman atau keheranan." },
  { "en": "Contoh kalimat seru?", "id": "Alangkah indahnya pemandangan ini!" },
  { "en": "Kata penanda kalimat seru?", "id": "Alangkah, betapa, bukan main." },
  { "en": "Apa itu kata ganti penanya?", "id": "Kata untuk menanyakan sesuatu." },
  { "en": "Kata tanya 'mengapa'?", "id": "Untuk menanyakan sebab." },
  { "en": "Kata tanya 'bagaimana'?", "id": "Untuk menanyakan cara/keadaan." },
  { "en": "Kata tanya 'berapa'?", "id": "Untuk menanyakan jumlah." },
  { "en": "Apa itu wacana?", "id": "Satuan bahasa terlengkap." },
  { "en": "Bentuk wacana?", "id": "Bisa paragraf, surat, buku." },
  { "en": "Syarat utama sebuah wacana?", "id": "Memiliki kohesi dan koherensi." },
  { "en": "Penggunaan tanda kurung siku [...]?", "id": "Mengapit keterangan dalam kutipan." },
  { "en": "Fungsi tanda garis miring (/)?", "id": "Pengganti kata 'atau', 'per'." },
  { "en": "Contoh penggunaan garis miring?", "id": "Harga Rp5.000/lembar." },
  { "en": "Proses pembentukan 'mempertanggungjawabkan'?", "id": "me(N)- + per- + tanggung jawab + -kan." },
  { "en": "'Silahkan' atau 'silakan'?", "id": "Bentuk baku: silakan." },
  { "en": "'Merubah' atau 'mengubah'?", "id": "Bentuk baku: mengubah." },
  { "en": "Kata dasar 'mengubah'?", "id": "Ubah." },
  { "en": "Kata dasar 'mengkaji'?", "id": "Kaji." },
  { "en": "'me-' + 'kaji' jadi?", "id": "'k' luluh menjadi 'ng'." },
  { "en": "Kata dasar 'memesona'?", "id": "Pesona." },
  { "en": "'me-' + 'pesona' jadi?", "id": "'p' luluh menjadi 'm'." },
{ "en": "Kata 'praktek' atau 'praktik'?", "id": "Bentuk baku: praktik." },
  { "en": "Kata 'sistim' atau 'sistem'?", "id": "Bentuk baku: sistem." },
  { "en": "Kata 'tehnik' atau 'teknik'?", "id": "Bentuk baku: teknik." },
  { "en": "Kata 'atlit' atau 'atlet'?", "id": "Bentuk baku: atlet." },
  { "en": "Kata 'nasehat' atau 'nasihat'?", "id": "Bentuk baku: nasihat." },
  { "en": "Kata 'jaman' atau 'zaman'?", "id": "Bentuk baku: zaman." },
  { "en": "Kata 'ijin' atau 'izin'?", "id": "Bentuk baku: izin." },
  { "en": "Kata 'karir' atau 'karier'?", "id": "Bentuk baku: karier." },
  { "en": "Kata 'hakekat' atau 'hakikat'?", "id": "Bentuk baku: hakikat." },
  { "en": "Kata 'analisa' atau 'analisis'?", "id": "Bentuk baku: analisis." },
  { "en": "Kata 'kuitansi' atau 'kwitansi'?", "id": "Bentuk baku: kuitansi." },
  { "en": "Kata 'sekedar' atau 'sekadar'?", "id": "Bentuk baku: sekadar." },
  { "en": "Kata 'terlanjur' atau 'terlanjur'?", "id": "Bentuk baku: telanjur." },
  { "en": "Kata 'resiko' atau 'risiko'?", "id": "Bentuk baku: risiko." },
  { "en": "Kata 'konkrit' atau 'konkret'?", "id": "Bentuk baku: konkret." },
  { "en": "Kata 'propinsi' atau 'provinsi'?", "id": "Bentuk baku: provinsi." },
  { "en": "Kata 'grup' atau 'grup'?", "id": "Bentuk baku: grup." },
  { "en": "Kata 'komplek' atau 'kompleks'?", "id": "Bentuk baku: kompleks." },
  { "en": "Kata 'obyektif' atau 'objektif'?", "id": "Bentuk baku: objektif." },
  { "en": "Kata 'jadual' atau 'jadwal'?", "id": "Bentuk baku: jadwal." },
  { "en": "Apa itu kalimat mayor?", "id": "Kalimat lengkap minimal S-P." },
  { "en": "Apa itu kalimat minor?", "id": "Kalimat tidak lengkap polanya." },
  { "en": "Contoh kalimat minor?", "id": "Selamat pagi! Pergi!" },
  { "en": "Konjungsi antarkalimat adalah?", "id": "Penghubung antar kalimat." },
  { "en": "Contoh konjungsi antarkalimat?", "id": "Oleh karena itu, ..." },
  { "en": "Penulisan 'Danau Toba'?", "id": "Setiap awal kata kapital." },
  { "en": "Kenapa 'Danau Toba' kapital?", "id": "Merupakan nama geografi." },
  { "en": "Penulisan 'mandi di danau'?", "id": "'danau' ditulis huruf kecil." },
  { "en": "Kenapa 'danau' huruf kecil?", "id": "Bukan nama diri geografi." },
  { "en": "Penulisan 'Gubernur Jawa Barat'?", "id": "Huruf kapital (nama jabatan)." },
  { "en": "Penulisan 'menjadi gubernur'?", "id": "'gubernur' huruf kecil." },
  { "en": "Kesalahan 'agar supaya'?", "id": "Maknanya sama (pleonasme)." },
  { "en": "Kesalahan 'adalah merupakan'?", "id": "Maknanya sama (pleonasme)." },
  { "en": "Kesalahan 'disebabkan karena'?", "id": "Maknanya sama (pleonasme)." },
  { "en": "Bentuk dasar 'menyukseskan'?", "id": "Kata dasarnya 'sukses'." },
  { "en": "Imbuhan 'menyukseskan'?", "id": "me(N)-kan." },
  { "en": "Apa itu konfiks?", "id": "Imbuhan di awal & akhir." },
  { "en": "Contoh konfiks?", "id": "ke-an, pe-an, per-an." },
  { "en": "Kata ulang 'kupu-kupu'?", "id": "Kata ulang semu." },
  { "en": "Kenapa disebut ulang semu?", "id": "Karena bukan pengulangan kata." },
  { "en": "Arti ulang 'kemerah-merahan'?", "id": "Menyerupai atau agak merah." },
  { "en": "Apa itu topik dalam kalimat?", "id": "Apa yang dibicarakan." },
  { "en": "Apa itu komen dalam kalimat?", "id": "Keterangan tentang topik." },
  { "en": "'Kucing itu tidur'. Topiknya?", "id": "Kucing itu." },
  { "en": "'Kucing itu tidur'. Komennya?", "id": "Tidur." },
  { "en": "'ke samping' atau 'kesamping'?", "id": "Dipisah, 'ke samping' (arah)." },
  { "en": "'keluar' atau 'ke luar'?", "id": "Keduanya benar, beda makna." },
  { "en": "Makna 'ke luar'?", "id": "Menunjukkan arah atau tujuan." },
  { "en": "Makna 'keluar'?", "id": "Verba, bergerak dari dalam." },
  { "en": "Fungsi tanda seru (!)?", "id": "Akhir kalimat perintah/seruan." },
  { "en": "Fungsi tanda tanya (?)?", "id": "Akhir kalimat tanya." },
  { "en": "Fungsi tanda titik (.)?", "id": "Akhir kalimat berita." },
  { "en": "Pronomina persona?", "id": "Kata ganti orang." },
  { "en": "Pronomina posesif?", "id": "Kata ganti kepemilikan." },
  { "en": "Contoh pronomina posesif?", "id": "-ku, -mu, -nya, kami." },
  { "en": "Pronomina demonstratif?", "id": "Kata ganti penunjuk." },
  { "en": "Contoh pronomina demonstratif?", "id": "Ini, itu, situ, sana." },
  { "en": "Apa itu bahasa kiasan?", "id": "Bahasa yang tidak harfiah." },
  { "en": "Contoh bahasa kiasan?", "id": "Majas dan idiom." },
  { "en": "Kata 'cuma-cuma' artinya?", "id": "Gratis." },
  { "en": "'Hati-hati' termasuk kata?", "id": "Kata ulang." },
  { "en": "Makna 'hati-hati'?", "id": "Menyatakan perintah agar waspada." },
  { "en": "Kata dasar 'keterlaluan'?", "id": "Terlalu." },
  { "en": "Imbuhan 'keterlaluan'?", "id": "Konfiks ke-an." },
  { "en": "'Ibu membelikan adik baju'. Objeknya?", "id": "Adik dan baju." },
  { "en": "Objek pada 'membelikan adik'?", "id": "Adik (objek penyerta)." },
  { "en": "Objek pada 'membeli baju'?", "id": "Baju (objek penderita)." },
  { "en": "Kata 'sebaiknya' menyatakan?", "id": "Saran atau anjuran." },
  { "en": "Kata 'harus' menyatakan?", "id": "Keharusan atau kewajiban." },
  { "en": "Apa itu morfologi?", "id": "Ilmu tentang bentuk kata." },
  { "en": "Apa itu sintaksis?", "id": "Ilmu tentang susunan kalimat." },
  { "en": "Apa itu semantik?", "id": "Ilmu tentang makna kata." },
  { "en": "Apa itu fonologi?", "id": "Ilmu tentang bunyi bahasa." },
  { "en": "Unit terkecil bahasa?", "id": "Fonem (bunyi)." },
  { "en": "Unit terkecil bermakna?", "id": "Morfem (imbuhan/kata dasar)." },
  { "en": "Apa itu alomorf?", "id": "Variasi bentuk morfem." },
  { "en": "Contoh alomorf?", "id": "me-, meng-, men-, meny-." },
  { "en": "Penulisan 'Rp5.000,00'?", "id": "Benar, tanpa spasi." },
  { "en": "Fungsi tanda apostrof (')?", "id": "Menunjukkan penghilangan bagian kata." },
  { "en": "Contoh apostrof?", "id": "'kan (akan), 'lah (telah)." },
  { "en": "Kata 'aktifitas' atau 'aktivitas'?", "id": "Bentuk baku: aktivitas." },
  { "en": "Kata 'pasip' atau 'pasif'?", "id": "Bentuk baku: pasif." },
  { "en": "Kata 'azas' atau 'asas'?", "id": "Bentuk baku: asas." },
  { "en": "Kata 'duren' atau 'durian'?", "id": "Bentuk baku: durian." },
  { "en": "Kata 'ekstrim' atau 'ekstrem'?", "id": "Bentuk baku: ekstrem." },
  { "en": "Kata 'handal' atau 'andal'?", "id": "Bentuk baku: andal." },
  { "en": "Kata 'hirarki' atau 'hierarki'?", "id": "Bentuk baku: hierarki." },
  { "en": "Kata 'hipotesa' atau 'hipotesis'?", "id": "Bentuk baku: hipotesis." },
  { "en": "Kata 'kaos' atau 'kaus'?", "id": "Bentuk baku: kaus." },
  { "en": "Kata 'kategori' atau 'katagori'?", "id": "Bentuk baku: kategori." },
  { "en": "Kata 'kreatifitas' atau 'kreativitas'?", "id": "Bentuk baku: kreativitas." },
  { "en": "Kata 'kwalitas' atau 'kualitas'?", "id": "Bentuk baku: kualitas." },
  { "en": "Kata 'legalisir' atau 'legalisasi'?", "id": "Bentuk baku: legalisasi." },
{ "en": "Kata 'imbau' atau 'himbau'?", "id": "Bentuk baku: imbau." },
  { "en": "Kata 'cabe' atau 'cabai'?", "id": "Bentuk baku: cabai." },
  { "en": "Kata 'cenderamata' atau 'cinderamata'?", "id": "Bentuk baku: cenderamata." },
  { "en": "Kata 'dekret' atau 'dekrit'?", "id": "Bentuk baku: dekret." },
  { "en": "Kata 'disain' atau 'desain'?", "id": "Bentuk baku: desain." },
  { "en": "Kata 'diteil' atau 'detail'?", "id": "Bentuk baku: detail." },
  { "en": "Kata 'do'a' atau 'doa'?", "id": "Bentuk baku: doa." },
  { "en": "Kata 'dramatisir' atau 'dramatisasi'?", "id": "Bentuk baku: dramatisasi." },
  { "en": "Kata 'ekstrim' atau 'ekstrem'?", "id": "Bentuk baku: ekstrem." },
  { "en": "Kata 'elif' atau 'elit'?", "id": "Bentuk baku: elite." },
  { "en": "Kata ' Pebruari' atau 'Februari'?", "id": "Bentuk baku: Februari." },
  { "en": "Kata 'geladi' atau 'gladi'?", "id": "Bentuk baku: gladi." },
  { "en": "Kata 'jenius' atau 'jenial'?", "id": "Bentuk baku: jenial." },
  { "en": "Kata 'kangker' atau 'kanker'?", "id": "Bentuk baku: kanker." },
  { "en": "Kata 'karisma' atau 'kharisma'?", "id": "Bentuk baku: karisma." },
  { "en": "Kata 'konkrit' atau 'konkret'?", "id": "Bentuk baku: konkret." },
  { "en": "Kata 'kreatif' atau 'kreatip'?", "id": "Bentuk baku: kreatif." },
  { "en": "Kata 'kongres' atau 'konggres'?", "id": "Bentuk baku: kongres." },
  { "en": "Kata 'ma'af' atau 'maaf'?", "id": "Bentuk baku: maaf." },
  { "en": "Kata 'managemen' atau 'manajemen'?", "id": "Bentuk baku: manajemen." },
  { "en": "Beda 'jam' dan 'pukul'?", "id": "'Jam' durasi, 'pukul' waktu." },
  { "en": "Contoh penggunaan 'jam'?", "id": "Belajar selama satu jam." },
  { "en": "Contoh penggunaan 'pukul'?", "id": "Acara dimulai pukul 10.00." },
  { "en": "Apa itu morfem bebas?", "id": "Kata dasar yang bisa berdiri." },
  { "en": "Contoh morfem bebas?", "id": "Rumah, pergi, merah." },
  { "en": "Apa itu morfem terikat?", "id": "Bentuk yang tak bisa berdiri." },
  { "en": "Contoh morfem terikat?", "id": "ber-, -kan, me-, di-." },
  { "en": "Apa itu kalimat majemuk rapatan?", "id": "Kalimat yang unsurnya disingkat." },
  { "en": "Contoh kalimat majemuk rapatan?", "id": "Dia membeli buku dan pensil." },
  { "en": "Unsur yang dirapatkan?", "id": "Subjek dan predikat." },
  { "en": "Kapan angka ditulis dengan huruf?", "id": "Jika hanya satu/dua kata." },
  { "en": "Contoh angka ditulis huruf?", "id": "Dua belas orang." },
  { "en": "Kapan angka ditulis dengan angka?", "id": "Jumlah besar, perlu perincian." },
  { "en": "Contoh angka ditulis angka?", "id": "Sebanyak 250 orang." },
  { "en": "Akar kata 'pembelajaran'?", "id": "Ajar." },
  { "en": "Imbuhan pada 'pembelajaran'?", "id": "pe(N)-an dan ber-." },
  { "en": "Jenis frasa 'sangat rajin'?", "id": "Frasa adjektival (sifat)." },
  { "en": "Jenis frasa 'di pasar'?", "id": "Frasa preposisional (depan)." },
  { "en": "Penulisan 'antar' sebagai awalan?", "id": "Digabung dengan kata berikutnya." },
  { "en": "Contoh penulisan 'antar-'?", "id": "Antarnegara, antarpulau." },
  { "en": "Penulisan 'maha' sebagai awalan?", "id": "Digabung, kecuali dengan Esa." },
  { "en": "Contoh penulisan 'maha-'?", "id": "Mahasiswa, Mahakuasa." },
  { "en": "Penulisan kata sapaan?", "id": "Menggunakan huruf kapital." },
  { "en": "Contoh kata sapaan?", "id": "Selamat pagi, Pak." },
  { "en": "Fungsi koma sebelum 'tetapi'?", "id": "Memisahkan dua klausa berlawanan." },
  { "en": "Contoh koma sebelum 'tetapi'?", "id": "Itu bagus, tetapi mahal." },
  { "en": "Apa itu verba aus?", "id": "Kata kerja bantu." },
  { "en": "Contoh verba aus?", "id": "Boleh, harus, ingin, akan." },
  { "en": "Apa itu adverbia?", "id": "Istilah lain kata keterangan." },
  { "en": "Apa itu adjektiva?", "id": "Istilah lain kata sifat." },
  { "en": "Bentuk tidak baku 'hierarki'?", "id": "Hirarki." },
  { "en": "Bentuk tidak baku 'diagnosis'?", "id": "Diagnosa." },
  { "en": "Bentuk tidak baku 'efektif'?", "id": "Epektip." },
  { "en": "Bentuk tidak baku 'objek'?", "id": "Obyek." },
  { "en": "Bentuk tidak baku 'subjek'?", "id": "Subyek." },
  { "en": "Bentuk tidak baku 'metode'?", "id": "Metoda." },
  { "en": "Bentuk tidak baku 'napas'?", "id": "Nafas." },
  { "en": "Bentuk tidak baku 'paham'?", "id": "Faham." },
  { "en": "Bentuk tidak baku 'pikir'?", "id": "Fikir." },
  { "en": "Bentuk tidak baku 'jadwal'?", "id": "Jadual." },
  { "en": "Bentuk tidak baku 'kualitas'?", "id": "Kwalitas." },
  { "en": "Bentuk tidak baku 'kuantitas'?", "id": "Kwantitas." },
  { "en": "Bentuk tidak baku 'nomor'?", "id": "Nomer." },
  { "en": "Bentuk tidak baku 'persen'?", "id": "Prosen." },
  { "en": "Bentuk tidak baku 'proyek'?", "id": "Projek." },
  { "en": "Bentuk tidak baku 'saksama'?", "id": "Seksama." },
  { "en": "Bentuk tidak baku 'standar'?", "id": "Standart." },
  { "en": "Bentuk tidak baku 'survei'?", "id": "Survai." },
  { "en": "Bentuk tidak baku 'telepon'?", "id": "Telpon, tilpon." },
  { "en": "Bentuk tidak baku 'teoretis'?", "id": "Teoritis." },
  { "en": "Bentuk tidak baku 'tim'?", "id": "Team." },
  { "en": "Bentuk tidak baku 'ustaz'?", "id": "Ustad, ustadz." },
  { "en": "Bentuk tidak baku 'video'?", "id": "Vidio." },
  { "en": "Bentuk tidak baku 'vila'?", "id": "Villa." },
  { "en": "Bentuk tidak baku 'wali kota'?", "id": "Walikota (dipisah)." },
  { "en": "Penulisan 'dukacita'?", "id": "Digabung, karena kata majemuk." },
  { "en": "Penulisan 'sukacita'?", "id": "Digabung, karena kata majemuk." },
  { "en": "Penulisan 'daripada'?", "id": "Digabung, karena kata depan." },
  { "en": "Penulisan 'kepada'?", "id": "Digabung, karena kata depan." },
  { "en": "Penulisan 'bagaimana'?", "id": "Digabung, karena kata tanya." },
  { "en": "Penulisan 'kilometer'?", "id": "Digabung, karena kata majemuk." },
  { "en": "Penulisan 'kasatmata'?", "id": "Digabung, karena kata majemuk." },
  { "en": "Penulisan 'beasiswa'?", "id": "Digabung, karena kata majemuk." },
  { "en": "Penulisan 'olahraga'?", "id": "Digabung, karena kata majemuk." },
  { "en": "Penulisan 'sedangkan'?", "id": "Digabung, karena konjungsi." },
  { "en": "Penulisan 'kacamata'?", "id": "Digabung, karena sudah senyawa." },
  { "en": "Penulisan 'saputangan'?", "id": "Digabung, karena sudah senyawa." },
  { "en": "Penulisan 'tanda tangan'?", "id": "Dipisah, karena belum senyawa." },
  { "en": "Penulisan 'terima kasih'?", "id": "Dipisah, karena frasa." },
  { "en": "Penulisan 'tanggung jawab'?", "id": "Dipisah, karena frasa." },
  { "en": "Frasa atau kata majemuk?", "id": "Tergantung tingkat kepaduan makna." },
  { "en": "Unsur kalimat yang wajib?", "id": "Subjek dan Predikat." },
  { "en": "Unsur kalimat yang tak wajib?", "id": "Objek, Pelengkap, Keterangan." },
  { "en": "Struktur dasar kalimat Indonesia?", "id": "Subjek - Predikat (S-P)." }




        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
