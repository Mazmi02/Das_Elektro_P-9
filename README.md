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


  { "en": "Apa Itu Gerbang (Gate) Logika?", "id": "Komponen Dasar Sirkuit Logika Digital." },
  { "en": "Apa Itu Aljabar (Algebra) Boolean?", "id": "Matematika Logika Biner (0/1)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Digital." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Perangkat Penyimpanan Data Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil (Hilang Tanpa Daya)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil (Data Tetap)." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Pengubah Sinyal Listrik Ke Aksi." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Jaringan Pembangkitan Hingga Distribusi." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Tegangan Arus Bolak-Balik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Konversi Daya Semikonduktor." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Output Pengaruhi Input." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Hasil Ukur Nilai Benar." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Hasil Ukur Sama." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Visualisasi Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Kombinasi Listrik, Magnet." },
  { "en": "Apa Itu Gelombang (Wave) Elektromagnetik?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Komunikasi (Communication)?", "id": "Proses Pertukaran Informasi." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Antar Perangkat." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Pengenal Logis Unik Di Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Mudah Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Sulit Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Konduktivitas Antara." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Searah Arus Semikonduktor." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Penguat/Saklar Semikonduktor." },
  { "en": "Apa Itu Hukum (Law) Ohm?", "id": "Hubungan V = I * R." },
  { "en": "Apa Itu Hukum (Law) Arus Kirchhoff?", "id": "Jumlah Arus Masuk Titik Nol." },
  { "en": "Apa Itu Hukum (Law) Tegangan Kirchhoff?", "id": "Jumlah Tegangan Loop Tertutup Nol." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Transfer Energi Listrik (Watt)." },
  { "en": "Apa Rumus (Formula) Daya Listrik?", "id": "P = V * I = I² * R = V² / R." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Komponen Penyimpan Energi Medan Magnet." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Total Hambatan Rangkaian AC." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Kapasitor/Induktor Di AC." },
  { "en": "Apa Itu Respons (Response) Frekuensi?", "id": "Perilaku Rangkaian Vs Frekuensi." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Frekuensi Reaktansi Saling Menghilangkan." },
  { "en": "Apa Itu Filter Lolos Band (Band Pass)?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Stop Band (Band Stop)?", "id": "Menolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Penguatan (Gain) Desibel (dB)?", "id": "Ukuran Logaritmik Rasio Daya/Amplitudo." },
  { "en": "Apa Itu Sistem (System) Linear?", "id": "Sistem Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Transformasi (Transform) Laplace?", "id": "Alat Analisis Sistem Domain-s." },
  { "en": "Apa Itu Transformasi (Transform) Z?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem?", "id": "Kemampuan Sistem Kembali Normal." },
  { "en": "Apa Itu Sirkuit (Circuit) Digital?", "id": "Sirkuit Bekerja Level Logika Biner." },
  { "en": "Apa Itu Tabel (Table) Kebenaran?", "id": "Menunjukkan Output Semua Kombinasi Input." },
  { "en": "Apa Itu Logika (Logic) Kombinasional?", "id": "Output Hanya Tergantung Input Saat Ini." },
  { "en": "Apa Itu Logika (Logic) Sekuensial?", "id": "Output Tergantung Input, Keadaan Internal." },
  { "en": "Apa Itu Sinyal (Signal) Clock?", "id": "Sinyal Sinkronisasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Tepi (Edge) Naik Clock?", "id": "Transisi Sinyal Clock Dari Rendah Ke Tinggi." },
  { "en": "Apa Itu Tepi (Edge) Turun Clock?", "id": "Transisi Sinyal Clock Dari Tinggi Ke Rendah." },
  { "en": "Apa Itu Memori (Memory) Statis (SRAM)?", "id": "Memori RAM (Random Access Memory) Berbasis Flip-Flop." },
  { "en": "Apa Itu Memori (Memory) Dinamis (DRAM)?", "id": "Memori RAM (Random Access Memory) Berbasis Kapasitor." },
  { "en": "Apa Perlu Refresh (Refresh) DRAM?", "id": "Ya, Kapasitor Kehilangan Muatan." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "Chip Logika Dapat Dikonfigurasi Ulang." },
  { "en": "Apa Itu ASIC (Application Specific Integrated Circuit)?", "id": "Chip Dirancang Khusus Aplikasi." },
  { "en": "Apa Itu Prosesor (Processor) Sinyal Digital (DSP)?", "id": "Mikroprosesor Optimasi Operasi DSP." },
  { "en": "Apa Itu Konverter (Converter) Analog Ke Digital?", "id": "ADC (Analog-to-Digital Converter)." },
  { "en": "Apa Itu Konverter (Converter) Digital Ke Analog?", "id": "DAC (Digital-to-Analog Converter)." },
  { "en": "Apa Itu Resolusi (Resolution) ADC/DAC?", "id": "Jumlah Bit Output/Input Digital." },
  { "en": "Apa Itu Laju (Rate) Sampling ADC?", "id": "Frekuensi Pengambilan Sampel Analog." },
  { "en": "Apa Itu Motor (Motor) Stepper?", "id": "Motor Bergerak Dalam Langkah Sudut." },
  { "en": "Apa Itu Motor (Motor) Servo?", "id": "Motor Kontrol Posisi Loop Tertutup." },
  { "en": "Apa Itu Sistem (System) Tenaga Tiga Fasa?", "id": "Sistem AC Tiga Gelombang Terpisah 120°." },
  { "en": "Apa Keuntungan (Advantage) Tiga Fasa?", "id": "Efisiensi Transmisi, Daya Konstan." },
  { "en": "Apa Hubungan (Connection) Bintang (Y)?", "id": "Ujung Sama Tiga Fasa Terhubung Netral." },
  { "en": "Apa Hubungan (Connection) Delta (Δ)?", "id": "Ujung Fasa Terhubung Fasa Berikutnya." },
  { "en": "Apa Itu Tegangan (Voltage) Fasa?", "id": "Tegangan Antara Satu Fasa, Netral." },
  { "en": "Apa Itu Tegangan (Voltage) Line?", "id": "Tegangan Antara Dua Fasa Berbeda." },
  { "en": "Hubungan V_Line, V_Fasa Di Bintang?", "id": "V_Line = √3 * V_Fasa." },
  { "en": "Hubungan I_Line, I_Fasa Di Delta?", "id": "I_Line = √3 * I_Fasa." },
  { "en": "Apa Itu Daya (Power) Kompleks?", "id": "Representasi Fasor Daya AC (P+jQ)." },
  { "en": "Apa Itu Segitiga (Triangle) Daya?", "id": "Representasi Grafis Hubungan P, Q, S." },
  { "en": "Apa Itu Kompensasi (Compensation) Daya Reaktif?", "id": "Menambah Kapasitor/Induktor Perbaiki PF." },
  { "en": "Apa Itu Analisis (Analysis) Gangguan Sistem Tenaga?", "id": "Menghitung Arus, Tegangan Saat Gangguan." },
  { "en": "Apa Itu Komponen (Component) Simetris?", "id": "Metode Analisis Gangguan Asimetris." },
  { "en": "Apa Tiga Komponen (Component) Simetris?", "id": "Urutan Positif, Negatif, Nol." },
  { "en": "Apa Itu Stabilitas (Stability) Sistem Tenaga?", "id": "Kemampuan Sistem Kembali Normal." },
  { "en": "Apa Jenis (Type) Stabilitas Utama?", "id": "Sudut Rotor, Tegangan, Frekuensi." },
  { "en": "Apa Itu Stabilitas (Stability) Sudut Rotor?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas (Stability) Tegangan?", "id": "Kemampuan Sistem Jaga Level Tegangan." },
  { "en": "Apa Itu Stabilitas (Stability) Frekuensi?", "id": "Kemampuan Sistem Jaga Frekuensi Konstan." },
  { "en": "Apa Itu Kendali (Control) Beban Frekuensi (LFC)?", "id": "Mengatur Pembangkitan Jaga Frekuensi." },
  { "en": "Apa Itu Kendali (Control) Tegangan Otomatis (AVR)?", "id": "Mengatur Eksitasi Jaga Tegangan Generator." },
  { "en": "Apa Itu FACTS (Flexible AC Transmission Systems)?", "id": "Peralatan Elektronika Daya Kontrol Transmisi." },
  { "en": "Apa Itu HVDC (High Voltage Direct Current)?", "id": "Transmisi Daya Arus Searah Jarak Jauh." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Jarak Jauh." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Jaringan Listrik Modern Digitalisasi." },
  { "en": "Apa Itu Pengukuran (Measurement) Fase Sinkron (PMU)?", "id": "Phasor Measurement Unit (PMU)." },
  { "en": "Apa Fungsi PMU (Phasor Measurement Unit)?", "id": "Pemantauan Keadaan Sistem Real-Time." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Kemampuan Mesin Tiru Kecerdasan Manusia." },
  { "en": "Apa Itu Pembelajaran Mesin (ML)?", "id": "Sub-Bidang AI (Artificial Intelligence) Belajar Dari Data." },
  { "en": "Apa Itu Deep Learning (DL)?", "id": "Sub-Bidang ML (Machine Learning) Jaringan Saraf Dalam." },
  { "en": "Apa Tiga Jenis Pembelajaran Mesin?", "id": "Terawasi, Tak Terawasi, Penguatan." },
  { "en": "Apa Itu Pembelajaran Terawasi (Supervised)?", "id": "Belajar Dari Data Berlabel (Input-Output)." },
  { "en": "Apa Itu Pembelajaran Tak Terawasi (Unsupervised)?", "id": "Belajar Pola Dari Data Tanpa Label." },
  { "en": "Apa Itu Pembelajaran Penguatan (Reinforcement)?", "id": "Belajar Lewat Coba Salah (Reward/Punishment)." },
  { "en": "Apa Itu Klasifikasi (Classification)?", "id": "Tugas Prediksi Kategori Diskrit." },
  { "en": "Apa Itu Regresi (Regression)?", "id": "Tugas Prediksi Nilai Kontinu." },
  { "en": "Apa Itu Clustering (Pengelompokan)?", "id": "Tugas Mengelompokkan Data Mirip." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Terinspirasi Otak Biologis." },
  { "en": "Apa Itu Neuron (Neuron) Tiruan?", "id": "Unit Pemroses Dasar Jaringan Saraf." },
  { "en": "Apa Itu Fungsi Aktivasi (Activation Function)?", "id": "Fungsi Non-Linear Output Neuron." },
  { "en": "Contoh Fungsi Aktivasi (Activation Function)?", "id": "Sigmoid, ReLU (Rectified Linear Unit), Tanh." },
  { "en": "Apa Itu Backpropagation (Propagasi Balik)?", "id": "Algoritma Pelatihan Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Overfitting (Overfitting)?", "id": "Model Terlalu Cocok Data Latih." },
  { "en": "Apa Itu Underfitting (Underfitting)?", "id": "Model Terlalu Sederhana Tangkap Pola." },
  { "en": "Apa Itu Regularisasi (Regularization)?", "id": "Teknik Mengurangi Overfitting." },
  { "en": "Apa Itu Convolutional Neural Network (CNN)?", "id": "Jaringan Saraf Khusus Pengolahan Citra." },
  { "en": "Apa Itu Recurrent Neural Network (RNN)?", "id": "Jaringan Saraf Data Sekuensial (Teks, Waktu)." },
  { "en": "Apa Itu Pemrosesan Bahasa Alami (NLP)?", "id": "Natural Language Processing (NLP)." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Kemampuan Komputer Interpretasi Gambar/Video." },
  { "en": "Apa Itu Sistem Rekomendasi (Recommender System)?", "id": "Memprediksi Preferensi Pengguna." },
  { "en": "Apa Itu Sistem Pakar (Expert System)?", "id": "AI (Artificial Intelligence) Tiru Penalaran Pakar Manusia." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Desain, Konstruksi, Operasi Robot." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung Internet." },
  { "en": "Apa Itu Big Data (Big Data)?", "id": "Volume Data Sangat Besar, Kompleks." },
  { "en": "Apa Tiga V Utama Big Data?", "id": "Volume (Volume), Velocity (Kecepatan), Variety (Variasi)." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Penyediaan Sumber Daya Komputasi Internet." },
  { "en": "Apa Itu IaaS (Infrastructure as a Service)?", "id": "Infrastruktur IT (VM, Storage) Sewaan." },
  { "en": "Apa Itu PaaS (Platform as a Service)?", "id": "Platform Pengembangan Aplikasi Sewaan." },
  { "en": "Apa Itu SaaS (Software as a Service)?", "id": "Perangkat Lunak Aplikasi Sewaan (Web)." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Perlindungan Sistem Digital Dari Serangan." },
  { "en": "Apa Itu Kriptografi (Cryptography)?", "id": "Ilmu Pengamanan Informasi (Enkripsi)." },
  { "en": "Apa Itu Blockchain (Blockchain)?", "id": "Buku Besar Digital Terdistribusi, Aman." },
  { "en": "Apa Itu Mata Uang Kripto (Cryptocurrency)?", "id": "Mata Uang Digital Kriptografi." },
  { "en": "Contoh Mata Uang Kripto (Cryptocurrency)?", "id": "Bitcoin (BTC), Ethereum (ETH)." },
  { "en": "Apa Itu Penambangan (Mining) Kripto?", "id": "Proses Verifikasi Transaksi, Tambah Blok." },
  { "en": "Apa Itu Kontrak Pintar (Smart Contract)?", "id": "Program Otomatis Eksekusi Blockchain." },
  { "en": "Apa Itu Realitas Virtual (VR)?", "id": "Simulasi Lingkungan Imersif Komputer." },
  { "en": "Apa Itu Realitas Tertambah (AR)?", "id": "Menambahkan Elemen Digital Dunia Nyata." },
  { "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?", "id": "Komputasi Berbasis Prinsip Kuantum." },
  { "en": "Apa Itu Qubit (Qubit)?", "id": "Unit Dasar Informasi Kuantum." },
  { "en": "Apa Sifat Unik Qubit (Qubit)?", "id": "Superposisi Dan Keterikatan (Entanglement)." },
  { "en": "Apa Itu Superposisi (Superposition)?", "id": "Qubit (Qubit) Bisa 0, 1, Keduanya Sekaligus." },
  { "en": "Apa Itu Keterikatan (Entanglement)?", "id": "Korelasi Kuantum Antar Qubit." },
  { "en": "Potensi Aplikasi Komputasi Kuantum?", "id": "Penemuan Obat, Material, Kriptografi." },
  { "en": "Apa Itu Nanoteknologi (Nanotechnology)?", "id": "Manipulasi Materi Skala Nanometer." },
  { "en": "Apa Ukuran Nanometer (nm)?", "id": "Satu Per Miliar Meter." },
  { "en": "Apa Itu Material Nano (Nanomaterial)?", "id": "Material Dimensi Skala Nano." },
  { "en": "Contoh Material Nano (Nanomaterial)?", "id": "Tabung Nano Karbon, Grafena, Titik Kuantum." },
  { "en": "Apa Itu Grafena (Graphene)?", "id": "Lapisan Tunggal Atom Karbon (Sangat Kuat)." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot)?", "id": "Nanokristal Semikonduktor Sifat Optik Unik." },
  { "en": "Apa Itu Bioteknologi (Biotechnology)?", "id": "Pemanfaatan Sistem Biologis Teknologi." },
  { "en": "Apa Itu Rekayasa Genetika (Genetic Engineering)?", "id": "Manipulasi Langsung Gen Organisme." },
  { "en": "Apa Itu CRISPR (Clustered Regularly Interspaced Short Palindromic Repeats)?", "id": "Teknologi Penyuntingan Gen." },
  { "en": "Apa Itu Kloning (Cloning)?", "id": "Membuat Salinan Genetik Identik." },
  { "en": "Apa Itu Sel Punca (Stem Cell)?", "id": "Sel Belum Terdiferensiasi Jadi Spesifik." },
  { "en": "Potensi Terapi Sel Punca (Stem Cell)?", "id": "Regenerasi Jaringan, Pengobatan Penyakit." },
  { "en": "Apa Itu Bioinformatika (Bioinformatics)?", "id": "Aplikasi Komputasi Analisis Data Biologis." },
  { "en": "Apa Itu Genomika (Genomics)?", "id": "Studi Genom Lengkap Organisme." },
  { "en": "Apa Itu Proteomika (Proteomics)?", "id": "Studi Protein Lengkap Organisme." },
  { "en": "Apa Itu Energi Nuklir (Nuclear Energy)?", "id": "Energi Dilepaskan Reaksi Nuklir." },
  { "en": "Apa Itu Fisi Nuklir (Nuclear Fission)?", "id": "Pembelahan Inti Atom Berat." },
  { "en": "Apa Itu Fusi Nuklir (Nuclear Fusion)?", "id": "Penggabungan Inti Atom Ringan." },
  { "en": "Bahan Bakar Reaktor Fisi Umum?", "id": "Uranium (U-235)." },
  { "en": "Bahan Bakar Reaksi Fusi Matahari?", "id": "Hidrogen Menjadi Helium." },
  { "en": "Tantangan Reaktor Fusi (Fusion Reactor)?", "id": "Mencapai Suhu, Tekanan Sangat Tinggi." },
  { "en": "Apa Itu Limbah Radioaktif (Radioactive Waste)?", "id": "Material Sisa Radioaktif Reaksi Nuklir." },
  { "en": "Apa Itu Ilmu Material (Materials Science)?", "id": "Studi Hubungan Struktur, Sifat Material." },
  { "en": "Apa Jenis Utama Material?", "id": "Logam, Keramik, Polimer, Komposit." },
  { "en": "Apa Itu Logam (Metal)?", "id": "Konduktif, Kuat, Ulet." },
  { "en": "Apa Itu Keramik (Ceramic)?", "id": "Keras, Rapuh, Tahan Panas, Isolator." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Molekul Rantai Panjang (Plastik, Karet)." },
  { "en": "Apa Itu Komposit (Composite)?", "id": "Gabungan Dua Material Berbeda Sifat." },
  { "en": "Contoh Material Komposit (Composite)?", "id": "Fiberglass (Serat Kaca Dalam Plastik)." },
  { "en": "Apa Itu Kekuatan (Strength) Material?", "id": "Kemampuan Tahan Beban Tanpa Patah." },
  { "en": "Apa Itu Kekerasan (Hardness) Material?", "id": "Ketahanan Terhadap Goresan, Indentasi." },
  { "en": "Apa Itu Keuletan (Ductility) Material?", "id": "Kemampuan Deformasi Plastis Tanpa Patah." },
  { "en": "Apa Itu Kerapuhan (Brittleness) Material?", "id": "Mudah Patah Tanpa Deformasi Signifikan." },
  { "en": "Apa Itu Ketangguhan (Toughness) Material?", "id": "Kemampuan Serap Energi Sebelum Patah." },
  { "en": "Apa Itu Kelelahan (Fatigue) Material?", "id": "Kegagalan Akibat Beban Siklik Berulang." },
  { "en": "Apa Itu Rayapan (Creep) Material?", "id": "Deformasi Lambat Beban Konstan Suhu Tinggi." },
  { "en": "Apa Itu Korosi (Corrosion)?", "id": "Kerusakan Material Akibat Reaksi Kimia Lingkungan." },
  { "en": "Bagaimana Mencegah Korosi (Corrosion)?", "id": "Pelapisan, Proteksi Katodik, Paduan." },
  { "en": "Apa Itu Diagram Fasa (Phase Diagram)?", "id": "Peta Fasa Stabil Material Vs Suhu/Tekanan." },
  { "en": "Apa Itu Perlakuan Panas (Heat Treatment)?", "id": "Pemanasan, Pendinginan Terkontrol Ubah Sifat." },
  { "en": "Contoh Proses Perlakuan Panas (Heat Treatment)?", "id": "Annealing, Quenching, Tempering." },
  { "en": "Apa Itu Annealing (Annealing)?", "id": "Pemanasan Diikuti Pendinginan Lambat (Melunakkan)." },
  { "en": "Apa Itu Quenching (Quenching)?", "id": "Pendinginan Cepat (Mengeraskan Baja)." },
  { "en": "Apa Itu Tempering (Tempering)?", "id": "Pemanasan Ulang Baja Keras (Kurangi Kerapuhan)." },
  { "en": "Apa Itu Paduan (Alloy)?", "id": "Campuran Logam Dengan Logam/Non-Logam Lain." },
  { "en": "Contoh Paduan (Alloy)?", "id": "Baja (Besi+Karbon), Kuningan (Tembaga+Seng)." },
  { "en": "Apa Itu Mekatronika (Mechatronics)?", "id": "Integrasi Sistem Mekanik, Elektronik, Komputer." },
  { "en": "Apa Itu Termodinamika (Thermodynamics)?", "id": "Studi Hubungan Panas, Kerja, Energi." },
  { "en": "Apa Itu Perpindahan Panas (Heat Transfer)?", "id": "Pergerakan Energi Termal." },
  { "en": "Apa Tiga Mode Perpindahan Panas?", "id": "Konduksi, Konveksi, Radiasi." },
  { "en": "Apa Itu Mekanika Fluida (Fluid Mechanics)?", "id": "Studi Perilaku Fluida (Cairan, Gas)." },
  { "en": "Apa Itu Viskositas (Viscosity)?", "id": "Ukuran Ketahanan Fluida Mengalir." },
  { "en": "Apa Itu Aliran Laminar (Laminar Flow)?", "id": "Aliran Fluida Halus, Teratur." },
  { "en": "Apa Itu Aliran Turbulen (Turbulent Flow)?", "id": "Aliran Fluida Kacau, Berputar." },
  { "en": "Apa Itu Bilangan Reynolds (Reynolds Number)?", "id": "Parameter Prediksi Jenis Aliran Fluida." },
  { "en": "Apa Itu Aerodinamika (Aerodynamics)?", "id": "Studi Gerakan Udara, Interaksinya Benda." },
  { "en": "Apa Itu Hidrodinamika (Hydrodynamics)?", "id": "Studi Gerakan Cairan." },
  { "en": "Apa Itu Elektronika Biomedis (Biomedical Electronics)?", "id": "Aplikasi Elektronika Dalam Bidang Medis." },
  { "en": "Apa Itu Sinyal Biomedis (Biomedical Signal)?", "id": "Sinyal Listrik Dihasilkan Tubuh Manusia." },
  { "en": "Contoh Sinyal Biomedis (Biomedical Signal)?", "id": "ECG (Electrocardiogram), EEG (Electroencephalogram), EMG (Electromyogram)." },
  { "en": "Apa Itu ECG (Electrocardiogram)?", "id": "Perekaman Aktivitas Listrik Jantung." },
  { "en": "Apa Itu EEG (Electroencephalogram)?", "id": "Perekaman Aktivitas Listrik Otak." },
  { "en": "Apa Itu EMG (Electromyogram)?", "id": "Perekaman Aktivitas Listrik Otot." },
  { "en": "Apa Itu Elektroda (Electrode) Biomedis?", "id": "Sensor Konduktif Merekam Sinyal Tubuh." },
  { "en": "Jenis Elektroda (Electrode) Biomedis?", "id": "Permukaan (Surface), Jarum (Needle), Mikro (Micro)." },
  { "en": "Bahan Umum Elektroda (Electrode) Permukaan?", "id": "Perak-Perak Klorida (Ag/AgCl)." },
  { "en": "Apa Fungsi Gel (Gel) Elektroda?", "id": "Mengurangi Impedansi Kontak Kulit." },
  { "en": "Apa Itu Penguat Biopotensial (Biopotential Amplifier)?", "id": "Penguat Khusus Sinyal Biomedis." },
  { "en": "Syarat Penguat Biopotensial (Biopotential Amplifier)?", "id": "CMRR Tinggi, Impedansi Input Tinggi." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Itu Isolasi (Isolation) Medis?", "id": "Pemisahan Elektrikal Pasien Dari Jaringan Listrik." },
  { "en": "Mengapa Isolasi (Isolation) Medis Penting?", "id": "Mencegah Risiko Sengatan Listrik Pasien." },
  { "en": "Apa Itu Arus Bocor (Leakage Current) Medis?", "id": "Arus Kecil Mengalir Ke Pasien." },
  { "en": "Apa Itu Defibrilator (Defibrillator)?", "id": "Alat Kejut Listrik Hentikan Fibrilasi Jantung." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Perangkat Implant Stimulasi Detak Jantung." },
  { "en": "Apa Itu Pencitraan Medis (Medical Imaging)?", "id": "Teknik Visualisasi Bagian Dalam Tubuh." },
  { "en": "Apa Itu Sinar-X (X-Ray) Konvensional?", "id": "Pencitraan Tulang Atenuasi Sinar-X." },
  { "en": "Apa Itu CT Scan (Computed Tomography)?", "id": "Pencitraan Sinar-X Tiga Dimensi (3D)." },
  { "en": "Apa Itu MRI (Magnetic Resonance Imaging)?", "id": "Pencitraan Medan Magnet, Gelombang Radio." },
  { "en": "Keunggulan MRI (Magnetic Resonance Imaging)?", "id": "Detail Jaringan Lunak Sangat Baik." },
  { "en": "Apa Itu Ultrasonografi (Ultrasound)?", "id": "Pencitraan Menggunakan Gelombang Suara Tinggi." },
  { "en": "Prinsip Ultrasonografi (Ultrasound)?", "id": "Pantulan Gelombang Suara Antar Jaringan." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect) Ultrasonografi?", "id": "Mengukur Kecepatan Aliran Darah." },
  { "en": "Apa Itu Kedokteran Nuklir (Nuclear Medicine)?", "id": "Pencitraan Fungsional Menggunakan Radioisotop." },
  { "en": "Apa Itu Pencitraan PET (Positron Emission Tomography)?", "id": "Deteksi Anihilasi Positron (Metabolisme)." },
  { "en": "Apa Itu Robotika Medis (Medical Robotics)?", "id": "Penggunaan Robot Dalam Prosedur Medis." },
  { "en": "Contoh Robot Bedah (Surgical Robot)?", "id": "Sistem Robot Da Vinci." },
  { "en": "Apa Itu Nanoteknologi (Nanotechnology)?", "id": "Teknologi Skala Ukuran Nanometer." },
  { "en": "Apa Itu Nanometer (nm)?", "id": "Satu Per Miliar Meter (10⁻⁹ m)." },
  { "en": "Apa Itu Nanomaterial (Nanomaterial)?", "id": "Material Struktur Skala Nanometer." },
  { "en": "Contoh Nanomaterial (Nanomaterial)?", "id": "Tabung Nano Karbon, Grafena, Titik Kuantum." },
  { "en": "Apa Itu Tabung Nano Karbon (Carbon Nanotube)?", "id": "Silinder Grafena Gulung (Sangat Kuat)." },
  { "en": "Apa Itu Grafena (Graphene)?", "id": "Lapisan Tunggal Atom Karbon (2D)." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot)?", "id": "Nanokristal Semikonduktor Pendar Cahaya Warna." },
  { "en": "Apa Itu Nanomedicine (Nanomedicine)?", "id": "Aplikasi Nanoteknologi Bidang Medis." },
  { "en": "Apa Itu Material Cerdas (Smart Material)?", "id": "Material Respon Terhadap Stimulus Eksternal." },
  { "en": "Apa Itu Paduan Memori Bentuk (SMA)?", "id": "Kembali Bentuk Asli Jika Dipanaskan." },
  { "en": "Apa Itu Bahan Piezoelektrik (Piezoelectric Material)?", "id": "Hasilkan Listrik Diberi Tekanan Mekanis." },
  { "en": "Apa Itu Bahan Termokromik (Thermochromic Material)?", "id": "Berubah Warna Berdasarkan Suhu." },
  { "en": "Apa Itu Bahan Fotokromik (Photochromic Material)?", "id": "Berubah Warna Berdasarkan Cahaya." },
  { "en": "Apa Itu Fotonik (Photonics)?", "id": "Ilmu Teknologi Cahaya (Foton)." },
  { "en": "Apa Itu Laser (Laser)?", "id": "Sumber Cahaya Koheren, Monokromatik." },
  { "en": "Apa Itu Serat Optik (Optical Fiber)?", "id": "Media Pemandu Cahaya Transmisi Data." },
  { "en": "Prinsip Kerja Serat Optik (Optical Fiber)?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Optoelektronika (Optoelectronics)?", "id": "Perangkat Elektronik Berbasis Cahaya." },
  { "en": "Apa Itu Bioteknologi (Biotechnology)?", "id": "Pemanfaatan Organisme, Sistem Biologi Teknologi." },
  { "en": "Apa Itu Rekayasa Genetika (Genetic Engineering)?", "id": "Manipulasi Materi Genetik Organisme." },
  { "en": "Apa Itu PCR (Polymerase Chain Reaction)?", "id": "Teknik Penggandaan Segmen DNA." },
  { "en": "Apa Itu Kloning (Cloning)?", "id": "Pembuatan Salinan Identik Genetik." },
  { "en": "Apa Itu Sel Punca (Stem Cell)?", "id": "Sel Belum Terdiferensiasi." },
  { "en": "Apa Itu Bioinformatika (Bioinformatics)?", "id": "Komputasi Untuk Analisis Data Biologi." },
  { "en": "Apa Itu Fisika Modern (Modern Physics)?", "id": "Fisika Abad 20 (Relativitas, Kuantum)." },
  { "en": "Apa Itu Teori Relativitas (Theory of Relativity) Khusus?", "id": "Teori Einstein Gerak Kecepatan Tinggi." },
  { "en": "Apa Postulat Relativitas Khusus (Special Relativity)?", "id": "Hukum Fisika Sama, Kecepatan Cahaya Konstan." },
  { "en": "Apa Itu Dilatasi Waktu (Time Dilation)?", "id": "Waktu Bergerak Lambat Kecepatan Tinggi." },
  { "en": "Apa Itu Kontraksi Panjang (Length Contraction)?", "id": "Panjang Tampak Memendek Arah Gerak." },
  { "en": "Apa Itu E=mc² (E equals mc squared)?", "id": "Kesetaraan Massa Dan Energi." },
  { "en": "Apa Itu Teori Relativitas (Theory of Relativity) Umum?", "id": "Teori Einstein Gravitasi (Kelengkungan Ruangwaktu)." },
  { "en": "Apa Itu Mekanika Kuantum (Quantum Mechanics)?", "id": "Fisika Perilaku Skala Atomik." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Besaran Fisik Hanya Nilai Diskrit." },
  { "en": "Apa Itu Foton (Photon)?", "id": "Partikel Kuantum Cahaya." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel (Wave-Particle Duality)?", "id": "Partikel Punya Sifat Gelombang, Sebaliknya." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Batas Akurasi Ukur Posisi, Momentum." },
  { "en": "Apa Itu Fungsi Gelombang (Wave Function) (Ψ)?", "id": "Deskripsi Keadaan Sistem Kuantum." },
  { "en": "Apa Itu Persamaan Schrödinger (Schrödinger Equation)?", "id": "Persamaan Evolusi Fungsi Gelombang." },
  { "en": "Apa Itu Kucing Schrödinger (Schrödinger's Cat)?", "id": "Eksperimen Pikiran Ilustrasi Superposisi." },
  { "en": "Apa Itu Superposisi (Superposition) Kuantum?", "id": "Sistem Bisa Banyak Keadaan Sekaligus." },
  { "en": "Apa Itu Keterikatan Kuantum (Quantum Entanglement)?", "id": "Korelasi Kuantum Antar Partikel Terpisah." },
  { "en": "Apa Itu Spin (Spin) Partikel?", "id": "Momentum Sudut Intrinsik Kuantum." },
  { "en": "Apa Itu Fermion (Fermion)?", "id": "Partikel Spin Setengah Bulat (Elektron)." },
  { "en": "Apa Itu Boson (Boson)?", "id": "Partikel Spin Bulat (Foton)." },
  { "en": "Apa Itu Prinsip Eksklusi Pauli (Pauli Exclusion Principle)?", "id": "Dua Fermion Identik Tak Bisa Keadaan Sama." },
  { "en": "Apa Itu Statistik Fermi-Dirac (Fermi-Dirac Statistics)?", "id": "Statistik Distribusi Energi Fermion." },
  { "en": "Apa Itu Statistik Bose-Einstein (Bose-Einstein Statistics)?", "id": "Statistik Distribusi Energi Boson." },
  { "en": "Apa Itu Fisika Atom (Atomic Physics)?", "id": "Studi Struktur, Sifat Atom." },
  { "en": "Apa Itu Tingkat Energi (Energy Level) Atom?", "id": "Orbit Elektron Energi Terkuantisasi." },
  { "en": "Apa Itu Bilangan Kuantum (Quantum Number)?", "id": "Angka Deskripsi Keadaan Elektron Atom." },
  { "en": "Apa Itu Fisika Nuklir (Nuclear Physics)?", "id": "Studi Inti Atom." },
  { "en": "Apa Itu Inti Atom (Atomic Nucleus)?", "id": "Pusat Atom (Proton, Neutron)." },
  { "en": "Apa Itu Nukleon (Nucleon)?", "id": "Partikel Penyusun Inti (Proton/Neutron)." },
  { "en": "Apa Itu Gaya Nuklir Kuat (Strong Nuclear Force)?", "id": "Gaya Ikat Nukleon Dalam Inti." },
  { "en": "Apa Itu Gaya Nuklir Lemah (Weak Nuclear Force)?", "id": "Gaya Terlibat Peluruhan Radioaktif." },
  { "en": "Apa Itu Radioaktivitas (Radioactivity)?", "id": "Peluruhan Spontan Inti Tak Stabil." },
  { "en": "Apa Itu Peluruhan Alfa (Alpha Decay)?", "id": "Emisi Partikel Alfa (Inti Helium)." },
  { "en": "Apa Itu Peluruhan Beta (Beta Decay)?", "id": "Emisi Elektron Atau Positron Dari Inti." },
  { "en": "Apa Itu Peluruhan Gama (Gamma Decay)?", "id": "Emisi Foton Energi Tinggi (Gama)." },
  { "en": "Apa Itu Waktu Paruh (Half-Life)?", "id": "Waktu Separuh Inti Radioaktif Meluruh." },
  { "en": "Apa Itu Aktivitas (Activity) Radioaktif?", "id": "Laju Peluruhan Inti Radioaktif." },
  { "en": "Apa Satuan Aktivitas (Activity) Radioaktif?", "id": "Becquerel (Bq) Atau Curie (Ci)." },
  { "en": "Apa Itu Fisi Nuklir (Nuclear Fission)?", "id": "Pembelahan Inti Berat Menjadi Lebih Ringan." },
  { "en": "Apa Itu Fusi Nuklir (Nuclear Fusion)?", "id": "Penggabungan Inti Ringan Menjadi Lebih Berat." },
  { "en": "Apa Itu Reaksi Berantai (Chain Reaction)?", "id": "Reaksi Fisi Hasilkan Neutron Picu Fisi Lanjutan." },
  { "en": "Apa Itu Reaktor Nuklir (Nuclear Reactor)?", "id": "Tempat Reaksi Fisi Terkendali." },
  { "en": "Apa Itu Moderator (Moderator) Reaktor?", "id": "Bahan Memperlambat Neutron (Air, Grafit)." },
  { "en": "Apa Itu Batang Kendali (Control Rod)?", "id": "Bahan Penyerap Neutron Kontrol Laju Reaksi." },
  { "en": "Apa Itu Fisika Partikel (Particle Physics)?", "id": "Studi Partikel Elementer, Interaksinya." },
  { "en": "Apa Itu Model Standar (Standard Model)?", "id": "Teori Partikel Elementer, Gaya Fundamental." },
  { "en": "Apa Itu Quark (Quark)?", "id": "Partikel Dasar Penyusun Hadron." },
  { "en": "Apa Itu Hadron (Hadron)?", "id": "Partikel Terbuat Dari Quark (Proton, Neutron)." },
  { "en": "Apa Itu Lepton (Lepton)?", "id": "Partikel Dasar (Elektron, Neutrino)." },
  { "en": "Apa Itu Antimateri (Antimatter)?", "id": "Partikel Muatan Berlawanan Materi Biasa." },
  { "en": "Apa Itu Positron (Positron)?", "id": "Antipartikel Dari Elektron." },
  { "en": "Apa Itu Anihilasi (Annihilation)?", "id": "Materi, Antimateri Bertemu Hasilkan Energi." },
  { "en": "Apa Itu Boson (Boson) Gauge?", "id": "Partikel Perantara Gaya Fundamental." },
  { "en": "Apa Perantara (Carrier) Gaya Elektromagnetik?", "id": "Foton (Photon)." },
  { "en": "Apa Perantara (Carrier) Gaya Nuklir Kuat?", "id": "Gluon (Gluon)." },
  { "en": "Apa Perantara (Carrier) Gaya Nuklir Lemah?", "id": "Boson W Dan Z." },
  { "en": "Apa Itu Graviton (Graviton)?", "id": "Partikel Hipotetis Perantara Gravitasi." },
  { "en": "Apa Itu Boson Higgs (Higgs Boson)?", "id": "Partikel Terkait Pemberian Massa Partikel Lain." },
  { "en": "Apa Itu Akselerator Partikel (Particle Accelerator)?", "id": "Mesin Percepat Partikel Energi Tinggi." },
  { "en": "Contoh Akselerator Partikel (Particle Accelerator) Terkenal?", "id": "LHC (Large Hadron Collider) Di CERN." },
  { "en": "Apa Itu Kosmologi (Cosmology)?", "id": "Studi Asal Usul, Evolusi Alam Semesta." },
  { "en": "Apa Itu Teori Big Bang (Big Bang)?", "id": "Teori Awal Mula Alam Semesta." },
  { "en": "Apa Itu Radiasi Latar Belakang Kosmik (CMB)?", "id": "Sisa Panas Radiasi Big Bang." },
  { "en": "Apa Itu Pergeseran Merah (Redshift) Kosmologis?", "id": "Pergeseran Cahaya Galaksi Jauh (Ekspansi)." },
  { "en": "Apa Hukum Hubble (Hubble's Law)?", "id": "Kecepatan Galaksi Menjauh Proporsional Jarak." },
  { "en": "Apa Itu Materi Gelap (Dark Matter)?", "id": "Materi Tak Terlihat (Efek Gravitasi)." },
  { "en": "Apa Itu Energi Gelap (Dark Energy)?", "id": "Energi Hipotetis Penyebab Ekspansi Dipercepat." },
  { "en": "Apa Itu Relativitas (Relativity) Umum?", "id": "Teori Gravitasi Einstein (Kelengkungan Ruangwaktu)." },
  { "en": "Apa Itu Lubang Hitam (Black Hole)?", "id": "Objek Gravitasi Sangat Kuat." },
  { "en": "Apa Itu Horison Peristiwa (Event Horizon)?", "id": "Batas Lubang Hitam Tak Bisa Kembali." },
  { "en": "Apa Itu Gelombang Gravitasi (Gravitational Wave)?", "id": "Riak Dalam Jaringan Ruangwaktu." },
  { "en": "Apa Itu Teknik Digital (Digital Engineering)?", "id": "Desain, Implementasi Sistem Digital." },
  { "en": "Apa Itu Sistem Biner (Binary System)?", "id": "Sistem Bilangan Basis Dua (0, 1)." },
  { "en": "Apa Itu Bit (Bit)?", "id": "Satuan Terkecil Informasi Digital." },
  { "en": "Apa Itu Byte (Byte)?", "id": "Delapan Bit Data." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Gerbang (Gate) Apa Fungsi AND (Dan)?", "id": "Output 1 Jika Semua Input 1." },
  { "en": "Gerbang (Gate) Apa Fungsi OR (Atau)?", "id": "Output 1 Jika Ada Input 1." },
  { "en": "Gerbang (Gate) Apa Fungsi NOT (Bukan)?", "id": "Membalikkan Nilai Logika Input." },
  { "en": "Gerbang (Gate) Apa Fungsi NAND (Bukan DAN)?", "id": "Kebalikan Dari Gerbang AND." },
  { "en": "Gerbang (Gate) Apa Fungsi NOR (Bukan ATAU)?", "id": "Kebalikan Dari Gerbang OR." },
  { "en": "Gerbang (Gate) Apa Fungsi XOR (Eksklusif ATAU)?", "id": "Output 1 Jika Input Berbeda." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Aturan Matematis Logika Biner." },
  { "en": "Apa Itu Teorema De Morgan (De Morgan's Theorem)?", "id": "Aturan Hubungan NAND/NOR Ke AND/OR." },
  { "en": "Apa Itu Peta Karnaugh (Karnaugh Map)?", "id": "Metode Grafis Penyederhanaan Logika." },
  { "en": "Apa Itu Sirkuit Kombinasional (Combinational Circuit)?", "id": "Output Tergantung Input Saat Ini." },
  { "en": "Contoh Sirkuit Kombinasional (Combinational Circuit)?", "id": "Encoder, Decoder, Multiplexer, Adder." },
  { "en": "Apa Itu Half Adder (Penjumlah Setengah)?", "id": "Menjumlahkan Dua Bit (Hasil Sum, Carry)." },
  { "en": "Apa Itu Full Adder (Penjumlah Penuh)?", "id": "Menjumlahkan Tiga Bit (Dengan Carry In)." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Memilih Satu Dari Banyak Input." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Mengarahkan Satu Input Ke Banyak Output." },
  { "en": "Apa Itu Decoder (Decoder)?", "id": "Mengaktifkan Satu Output Berdasarkan Input Biner." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Mengubah Input Aktif Ke Kode Biner." },
  { "en": "Apa Itu Sirkuit Sekuensial (Sequential Circuit)?", "id": "Output Tergantung Input, Keadaan Internal." },
  { "en": "Elemen Apa Penyimpan Keadaan (State)?", "id": "Flip-Flop Atau Latch." },
  { "en": "Apa Itu Sinyal Clock (Clock Signal)?", "id": "Sinyal Sinkronisasi Rangkaian Sekuensial." },
  { "en": "Apa Beda Latch Dan Flip-Flop?", "id": "Latch (Sensitif Level), Flip-Flop (Sensitif Tepi)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) SR?", "id": "Elemen Memori Set-Reset Dasar." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) D?", "id": "Menyimpan Data (D) Pada Tepi Clock." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) JK?", "id": "Flip-Flop Universal (Set, Reset, Toggle)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) T?", "id": "Toggle (Membalik Keadaan) Pada Tepi Clock." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data Biner." },
  { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Menggeser Data Bit Per Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Sekuensial Mencacah Pulsa." },
  { "en": "Apa Itu Counter (Pencacah) Sinkron?", "id": "Semua Flip-Flop Di-Clock Bersamaan." },
  { "en": "Apa Itu Counter (Pencacah) Asinkron?", "id": "Output Flip-Flop Memicu Flip-Flop Berikutnya." },
  { "en": "Nama Lain Counter (Pencacah) Asinkron?", "id": "Ripple Counter (Pencacah Riak)." },
  { "en": "Apa Itu Modulus (Modulus) Counter?", "id": "Jumlah Total Keadaan (State) Counter." },
  { "en": "Apa Itu Counter (Pencacah) Dekade?", "id": "Counter Modulus Sepuluh (0-9)." },
  { "en": "Apa Itu Memori (Memory) Semikonduktor?", "id": "Perangkat Penyimpanan Data Berbasis IC." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Baca Tulis Akses Acak (Volatil)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Baca (Non-Volatil)." },
  { "en": "Apa Itu SRAM (Static RAM)?", "id": "RAM (Random Access Memory) Berbasis Flip-Flop (Cepat)." },
  { "en": "Apa Itu DRAM (Dynamic RAM)?", "id": "RAM (Random Access Memory) Berbasis Kapasitor (Perlu Refresh)." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Memori Non-Volatil Tipe EEPROM." },
  { "en": "Apa Itu Arsitektur Komputer (Computer Architecture)?", "id": "Desain Konseptual Sistem Komputer." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Unit Pemroses Pusat (Otak Komputer)." },
  { "en": "Apa Bagian Utama CPU (Central Processing Unit)?", "id": "ALU (Arithmetic Logic Unit), Control Unit, Register." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Unit Aritmetika Dan Logika." },
  { "en": "Apa Itu Control Unit (Unit Kendali)?", "id": "Mengatur Operasi Komponen Komputer." },
  { "en": "Apa Itu Register (Register) CPU?", "id": "Lokasi Penyimpanan Cepat Internal CPU." },
  { "en": "Apa Itu Program Counter (PC)?", "id": "Register Penunjuk Alamat Instruksi Berikutnya." },
  { "en": "Apa Itu Instruction Register (IR)?", "id": "Register Penyimpan Instruksi Saat Dijalankan." },
  { "en": "Apa Itu Siklus Instruksi (Instruction Cycle)?", "id": "Proses Ambil, Dekode, Eksekusi Instruksi." },
  { "en": "Apa Itu Bus (Bus) Sistem?", "id": "Jalur Komunikasi Antar Komponen Komputer." },
  { "en": "Apa Tiga Jenis Bus (Bus) Sistem?", "id": "Bus Alamat, Data, Kontrol." },
  { "en": "Apa Itu Bus Alamat (Address Bus)?", "id": "Menentukan Lokasi Memori Atau I/O." },
  { "en": "Apa Itu Bus Data (Data Bus)?", "id": "Mentransfer Data Antar Komponen." },
  { "en": "Apa Itu Bus Kontrol (Control Bus)?", "id": "Sinyal Pengatur Operasi (Read/Write)." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip IC." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip IC." },
  { "en": "Apa Beda Mikroprosesor, Mikrokontroler?", "id": "Mikrokontroler Termasuk Memori, I/O Peripheral." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Dedikasi Fungsi Khusus." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Transfer Data Satu Bit Per Waktu." },
  { "en": "Contoh Antarmuka Serial (Serial Interface)?", "id": "UART (Universal Asynchronous Receiver-Transmitter), SPI (Serial Peripheral Interface), I2C." },
  { "en": "Apa Itu Komunikasi Paralel (Parallel Communication)?", "id": "Transfer Data Beberapa Bit Bersamaan." },
  { "en": "Apa Itu Input/Output (I/O) Port?", "id": "Antarmuka Koneksi Komputer Perangkat Luar." },
  { "en": "Apa Itu Polling (Polling) I/O?", "id": "CPU (Central Processing Unit) Periodik Cek Status I/O." },
  { "en": "Apa Itu Interrupt (Interupsi) I/O?", "id": "Perangkat I/O Beri Sinyal CPU." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Transfer Data I/O Memori Tanpa CPU." },
  { "en": "Apa Itu Sistem Operasi (Operating System)?", "id": "Perangkat Lunak Pengelola Sumber Daya Komputer." },
  { "en": "Apa Itu Kernel (Kernel)?", "id": "Inti Program Sistem Operasi." },
  { "en": "Apa Itu Proses (Process)?", "id": "Program Yang Sedang Dieksekusi." },
  { "en": "Apa Itu Thread (Thread)?", "id": "Unit Eksekusi Terkecil Dalam Proses." },
  { "en": "Apa Itu Manajemen Memori (Memory Management)?", "id": "Alokasi, Dealokasi Memori Proses." },
  { "en": "Apa Itu Memori Virtual (Virtual Memory)?", "id": "Penggunaan Disk Perluas Memori Fisik." },
  { "en": "Apa Itu Paging (Paging)?", "id": "Membagi Memori Jadi Blok Ukuran Tetap." },
  { "en": "Apa Itu Sistem Berkas (File System)?", "id": "Struktur Organisasi Berkas Di Penyimpanan." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Pemrograman Aplikasi." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "Sistem Operasi Respon Waktu Deterministik." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Mode Deplesi?", "id": "Transistor MOSFET (Metal-Oxide-Semiconductor FET) Normally ON." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Mode Peningkatan?", "id": "Transistor MOSFET (Metal-Oxide-Semiconductor FET) Normally OFF." },
  { "en": "Apa Itu Arus Drain Saturasi (IDSS)?", "id": "Arus Drain Maksimum MOSFET Deplesi (Vgs=0)." },
  { "en": "Apa Itu Tegangan Pinch-Off (Vp) MOSFET?", "id": "Tegangan Gate Mematikan Saluran (Deplesi)." },
  { "en": "Apa Itu Tegangan Threshold (Vt) MOSFET?", "id": "Tegangan Gate Minimum Aktifkan Saluran (Enhancement)." },
  { "en": "Daerah Apa MOSFET Bekerja Saklar?", "id": "Daerah Cutoff (Off), Triode (On)." },
  { "en": "Daerah Apa MOSFET Bekerja Penguat?", "id": "Daerah Saturasi (Aktif)." },
  { "en": "Apa Keunggulan MOSFET Dibanding BJT?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Logika Digital Kombinasi NMOS, PMOS." },
  { "en": "Apa Keuntungan Utama Logika CMOS?", "id": "Disipasi Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Inverter (Inverter) CMOS?", "id": "Gerbang NOT (NOT Gate) CMOS." },
  { "en": "Apa Itu Gerbang NAND (NAND Gate) CMOS?", "id": "Gerbang NAND (NAND Gate) CMOS." },
  { "en": "Apa Itu Gerbang NOR (NOR Gate) CMOS?", "id": "Gerbang NOR (NOR Gate) CMOS." },
  { "en": "Apa Itu Gerbang Transmisi (Transmission Gate)?", "id": "Saklar Analog CMOS (NMOS Paralel PMOS)." },
  { "en": "Apa Itu Latch-Up (Latch-Up) CMOS?", "id": "Struktur Parasitik SCR Sebabkan Short." },
  { "en": "Apa Itu Penguat Common Source (Common Source)?", "id": "Konfigurasi Penguat FET (Field Effect Transistor) Fasa Terbalik." },
  { "en": "Apa Itu Penguat Common Drain (Common Drain)?", "id": "Konfigurasi Pengikut Source (Source Follower)." },
  { "en": "Apa Itu Penguat Common Gate (Common Gate)?", "id": "Konfigurasi Impedansi Input Rendah." },
  { "en": "Apa Itu Beban Aktif (Active Load)?", "id": "Menggunakan Transistor Sebagai Beban Penguat." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Rangkaian Menyalin Arus Referensi." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Mode Sama." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Penguatan, Mengurangi Distorsi." },
  { "en": "Apa Itu Umpan Balik Positif (Positive Feedback)?", "id": "Menyebabkan Osilasi (Dipakai Osilator)." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Penguat Op-Amp Output Terbalik Fasa." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Penguat Op-Amp Output Sefasa." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Buffer (Buffer) Op-Amp Gain Satu." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Kriteria Osilasi Barkhausen (Barkhausen Criterion)?", "id": "Gain Loop = 1, Fasa Loop = 0°." },
  { "en": "Apa Itu Osilator (Oscillator) RC?", "id": "Osilator (Oscillator) Jaringan Resistor-Kapasitor." },
  { "en": "Apa Itu Osilator (Oscillator) LC?", "id": "Osilator (Oscillator) Jaringan Induktor-Kapasitor." },
  { "en": "Apa Itu Osilator (Oscillator) Kristal?", "id": "Osilator (Oscillator) Frekuensi Sangat Stabil." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter (Filter) Menggunakan Op-Amp." },
  { "en": "Apa Itu Filter (Filter) Butterworth?", "id": "Filter (Filter) Respon Passband Datar." },
  { "en": "Apa Itu Filter (Filter) Chebyshev?", "id": "Filter (Filter) Roll-Off Curam, Riak Passband." },
  { "en": "Apa Itu Timer (Timer) 555?", "id": "IC (Integrated Circuit) Serbaguna Timer, Osilator." },
  { "en": "Mode Astabil (Astable Mode) IC 555?", "id": "Sebagai Osilator Gelombang Kotak." },
  { "en": "Mode Monostabil (Monostable Mode) IC 555?", "id": "Sebagai Timer Satu Kali Tembak." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Pengubah Arus AC Ke DC." },
  { "en": "Apa Itu Filter (Filter) Kapasitor?", "id": "Kapasitor Penghalus Riak Tegangan DC." },
  { "en": "Apa Itu Regulator (Regulator) Tegangan?", "id": "Menjaga Tegangan Output DC Konstan." },
  { "en": "Apa Itu Regulator (Regulator) Linear?", "id": "Regulator (Regulator) Efisiensi Rendah, Riak Rendah." },
  { "en": "Apa Itu Regulator (Regulator) Switching (SMPS)?", "id": "Regulator (Regulator) Efisiensi Tinggi, Riak Tinggi." },
  { "en": "Apa Itu Konverter (Converter) Buck?", "id": "Regulator (Regulator) Switching Penurun Tegangan." },
  { "en": "Apa Itu Konverter (Converter) Boost?", "id": "Regulator (Regulator) Switching Penaik Tegangan." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Komponen Pengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor (Motor) Induksi?", "id": "Motor AC (Alternating Current) Asinkron." },
  { "en": "Apa Itu Generator (Generator) Sinkron?", "id": "Pembangkit Listrik AC (Alternator)." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Jaringan Listrik Pembangkit, Transmisi, Distribusi." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Terhadap Gangguan." },
  { "en": "Apa Itu Pemutus (Breaker) Sirkuit?", "id": "Saklar Otomatis Pemutus Arus Gangguan." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keselamatan." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Aplikasi Semikonduktor Kontrol Daya." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Rangkaian Pengubah Daya DC Ke AC." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Sistem Pengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Output Digunakan Mengatur Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Industri Otomatisasi." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran Listrik." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Sinyal Listrik." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur V, A, Ω Serbaguna." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Listrika Dan Magnetik." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Komunikasi (Communication) Data?", "id": "Transfer Informasi Digital Antar Perangkat." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Global Komputer." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Jaringan." },
  { "en": "Apa Itu IP Address (Alamat IP)?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Perangkat Penghubung Perangkat Satu LAN." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Standar Jaringan Kabel LAN." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Standar Jaringan Nirkabel LAN." },
  { "en": "Apa Itu Bluetooth (Bluetooth)?", "id": "Komunikasi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Mudah Hantarkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Sulit Hantarkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Konduktivitas Dapat Diatur." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Searah Arus." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Penguat/Saklar." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Sirkuit Elektronik Dalam Chip." },
  { "en": "Apa Itu Digital (Digital)?", "id": "Berbasis Nilai Diskrit (0, 1)." },
  { "en": "Apa Itu Analog (Analog)?", "id": "Berbasis Nilai Kontinu." },
  { "en": "Apa Itu Sinyal (Signal)?", "id": "Besaran Fisik Membawa Informasi." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Sinyal Acak Tak Diinginkan." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Seleksi Frekuensi." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Operasi." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Hasil Pengukuran." },
  { "en": "Apa Itu Resolusi (Resolution)?", "id": "Perubahan Terkecil Terdeteksi." },
  { "en": "Apa Itu Sensor (Sensor) Suhu?", "id": "Mengukur Derajat Panas Dingin." },
  { "en": "Apa Itu Sensor (Sensor) Cahaya?", "id": "Mengukur Intensitas Cahaya." },
  { "en": "Apa Itu Sensor (Sensor) Jarak?", "id": "Mengukur Jarak Ke Objek." },
  { "en": "Apa Itu Motor (Motor) DC?", "id": "Penggerak Putar Arus Searah." },
  { "en": "Apa Itu Motor (Motor) AC?", "id": "Penggerak Putar Arus Bolak-Balik." },
  { "en": "Apa Itu Relay (Relay)?", "id": "Saklar Elektromekanis." },
  { "en": "Apa Itu Keselamatan (Safety) Listrik?", "id": "Pencegahan Bahaya Akibat Listrik." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih (Melebur)." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit (Bisa Reset)." },
  { "en": "Apa Itu Ground (Ground) Fault?", "id": "Arus Bocor Ke Tanah." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Penyearah Arus." },
  { "en": "Apa Fungsi Utama Dioda (Diode)?", "id": "Melewatkan Arus Satu Arah." },
  { "en": "Apa Nama Terminal Positif Dioda?", "id": "Anoda (Anode)." },
  { "en": "Apa Nama Terminal Negatif Dioda?", "id": "Katoda (Cathode)." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Kondisi Dioda Menghantarkan Arus." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Kondisi Dioda Menghambat Arus." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) (Vf)?", "id": "Jatuh Tegangan Saat Dioda Konduksi." },
  { "en": "Berapa Vf (Tegangan Maju) Dioda Silikon?", "id": "Sekitar Nol Koma Tujuh Volt (0.7V)." },
  { "en": "Apa Itu Arus Bocor (Leakage Current) Dioda?", "id": "Arus Kecil Saat Bias Mundur." },
  { "en": "Apa Itu Tegangan Breakdown (Breakdown Voltage)?", "id": "Tegangan Mundur Maksimum Dioda." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Pengubah AC Ke DC." },
  { "en": "Apa Itu Penyearah Setengah Gelombang (Half-Wave)?", "id": "Menggunakan Satu Dioda." },
  { "en": "Apa Itu Penyearah Gelombang Penuh (Full-Wave)?", "id": "Menggunakan Dua Atau Empat Dioda." },
  { "en": "Apa Itu Dioda Bridge (Bridge Rectifier)?", "id": "Empat Dioda Penyearah Gelombang Penuh." },
  { "en": "Apa Fungsi Kapasitor Filter (Filter Capacitor)?", "id": "Menghaluskan Riak Tegangan DC." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Dioda Regulator Tegangan Mundur." },
  { "en": "Apa Aplikasi Utama Dioda Zener?", "id": "Menstabilkan Tegangan Output." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Memancarkan Cahaya." },
  { "en": "Apa Fungsi Resistor Seri LED?", "id": "Membatasi Arus Listrik Lewat LED." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Peka Terhadap Cahaya." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Semikonduktor Penguat/Saklar." },
  { "en": "Apa Jenis Transistor (Transistor) Utama?", "id": "BJT (Bipolar Junction Transistor) Dan FET (Field Effect Transistor)." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Transistor Bipolar Terkontrol Arus." },
  { "en": "Apa Tiga Terminal BJT (Bipolar Junction Transistor)?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Apa Itu Transistor NPN (NPN Transistor)?", "id": "Tipe Transistor BJT (Lapisan P Tipis)." },
  { "en": "Apa Itu Transistor PNP (PNP Transistor)?", "id": "Tipe Transistor BJT (Lapisan N Tipis)." },
  { "en": "Apa Itu Penguatan Arus (Beta) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Tiga Daerah Operasi BJT?", "id": "Cutoff, Aktif, Dan Saturasi." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Berfungsi Penguat?", "id": "Saat Berada Di Daerah Aktif." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Berfungsi Saklar?", "id": "Saat Cutoff (Off) Atau Saturasi (On)." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Transistor Efek Medan Terkontrol Tegangan." },
  { "en": "Apa Tiga Terminal FET (Field Effect Transistor)?", "id": "Gate (Gerbang), Drain (Saluran), Source (Sumber)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Jenis FET Gerbang Terisolasi Oksida." },
  { "en": "Apa Keunggulan MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Karakteristik Op-Amp (Operational Amplifier) Ideal?", "id": "Gain Tak Hingga, Input Z Tak Hingga." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Penguatan Op-Amp." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Berlawanan Fasa Dengan Input." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Op-Amp Dengan Gain Satu." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Sebagai Penyangga (Buffer) Impedansi." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Itu Filter (Filter) Elektronik?", "id": "Rangkaian Selektif Frekuensi." },
  { "en": "Apa Itu Filter Lolos Rendah (Low Pass)?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Tinggi (High Pass)?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Logika Biner (Binary Logic)?", "id": "Sistem Logika (0 Dan 1)." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Untuk Logika Digital." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Level Tegangan AC." },
  { "en": "Apa Prinsip Kerja Transformator (Transformer)?", "id": "Induksi Mutual Elektromagnetik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Mengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem Tenaga (Power System) Listrik?", "id": "Jaringan Listrik Pembangkit Hingga Konsumen." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Aman Ke Potensial Bumi." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih (Putus)." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Pelindung Arus Lebih (Bisa Reset)." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Menampilkan Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Penghantar Listrik Yang Baik." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Penghambat Listrik Yang Baik." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Antara Konduktor, Isolator." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Aliran Daya Listrik." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Rangkaian Pengubah DC Ke AC." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Loop Tertutup (Closed Loop) Kontrol?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Deteksi Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Penggerak Mekanis." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Logika Industri." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Alamat Logis Perangkat Di Jaringan." },
  { "en": "Apa Itu Medan Elektromagnetik (Electromagnetic Field)?", "id": "Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Getaran Medan Listrik, Magnet Merambat." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Listrik Statis (Static Electricity)?", "id": "Akumulasi Muatan Listrik Diam." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Hambatan Spesifik Suatu Material." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Kemudahan Material Hantarkan Listrik." },
  { "en": "Apa Itu Bahan Dielektrik (Dielectric Material)?", "id": "Bahan Isolator Dalam Kapasitor." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Medan Listrik Maksimum Sebelum Tembus." },
  { "en": "Apa Itu Bahan Magnetik (Magnetic Material)?", "id": "Bahan Berinteraksi Dengan Medan Magnet." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Ketergantungan Magnetisasi Riwayat Medan." },
  { "en": "Apa Itu Efek Hall (Hall Effect)?", "id": "Tegangan Timbul Konduktor Medan Magnet." },
  { "en": "Apa Itu Efek Termoelektrik (Thermoelectric Effect)?", "id": "Konversi Langsung Panas Ke Listrik." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Listrik Dihasilkan Tekanan Mekanis." },
  { "en": "Apa Itu Efek Fotolistrik (Photoelectric Effect)?", "id": "Elektron Dipancarkan Akibat Cahaya." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Perangkat Pengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Resistansi Nol Suhu Rendah." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Rangkaian Elektronik Dalam Chip Kecil." },
  { "en": "Apa Itu Fabrikasi (Fabrication) IC?", "id": "Proses Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit IC." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Impuritas Atur Konduktivitas." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Dasar Komponen Semikonduktor Dioda." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Area Sekitar Sambungan Kurang Muatan." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) (Vf) Dioda?", "id": "Jatuh Tegangan Saat Konduksi." },
  { "en": "Apa Itu Penyearah (Rectifier) Jembatan?", "id": "Empat Dioda Penyearah Gelombang Penuh." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Dioda Penstabil Tegangan Mundur." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Intensitas Cahaya." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Penguat/Saklar Terkontrol Arus Basis." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Penguat/Saklar Terkontrol Tegangan Gate." },
  { "en": "Apa Itu Penguat (Amplifier) Op-Amp?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis, Interpretasi, Manipulasi Sinyal." },
  { "en": "Apa Itu Sinyal (Signal) Waktu Kontinu?", "id": "Sinyal Terdefinisi Setiap Saat Waktu." },
  { "en": "Apa Itu Sinyal (Signal) Waktu Diskrit?", "id": "Sinyal Terdefinisi Hanya Waktu Tertentu." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Mengubah Sinyal Kontinu Ke Diskrit." },
  { "en": "Apa Itu Teorema (Theorem) Sampling Nyquist?", "id": "Laju Sampling > 2x Frekuensi Max." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Frekuensi Akibat Sampling Kurang." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Pembulatan Nilai Sampel Ke Level Diskrit." },
  { "en": "Apa Itu Kesalahan (Error) Kuantisasi?", "id": "Selisih Antara Nilai Asli, Kuantisasi." },
  { "en": "Apa Itu Sistem (System) LTI?", "id": "Linear Time-Invariant." },
  { "en": "Apa Itu Respon (Response) Impuls?", "id": "Output Sistem LTI Untuk Input Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Output Sistem LTI." },
  { "en": "Apa Itu Transformasi (Transform) Fourier?", "id": "Mengubah Sinyal Ke Domain Frekuensi." },
  { "en": "Apa Itu Spektrum (Spectrum) Frekuensi?", "id": "Representasi Amplitudo/Fasa Vs Frekuensi." },
  { "en": "Apa Itu DFT (Discrete Fourier Transform)?", "id": "Transformasi Fourier Sinyal Diskrit." },
  { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Algoritma Cepat Hitung DFT." },
  { "en": "Apa Itu Filter (Filter) Digital?", "id": "Memodifikasi Spektrum Sinyal Digital." },
  { "en": "Apa Itu Filter (Filter) FIR?", "id": "Finite Impulse Response." },
  { "en": "Apa Itu Filter (Filter) IIR?", "id": "Infinite Impulse Response." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu LAN (Local Area Network)?", "id": "Jaringan Area Lokal." },
  { "en": "Apa Itu WAN (Wide Area Network)?", "id": "Jaringan Area Luas." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Global." },
  { "en": "Apa Itu Model (Model) OSI?", "id": "Model Referensi Tujuh Lapisan." },
  { "en": "Apa Itu Model (Model) TCP/IP?", "id": "Model Arsitektur Internet." },
  { "en": "Apa Itu Subnet Mask (Mask Subnet)?", "id": "Memisahkan Jaringan, Host Address." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Penerjemah Nama Domain Ke IP." },
  { "en": "Apa Itu DHCP (Dynamic Host Configuration Protocol)?", "id": "Konfigurasi IP Otomatis." },
  { "en": "Apa Itu MAC Address (Alamat MAC)?", "id": "Alamat Fisik Perangkat Keras." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Menghubungkan Perangkat Satu LAN." },
  { "en": "Apa Itu Router (Router)?", "id": "Menghubungkan Jaringan Berbeda." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Pengaman Penyaring Lalu Lintas." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Teknologi Jaringan Kabel LAN." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan Nirkabel LAN." },
  { "en": "Apa Itu TCP (Transmission Control Protocol)?", "id": "Protokol Transport Andal." },
  { "en": "Apa Itu UDP (User Datagram Protocol)?", "id": "Protokol Transport Cepat, Tidak Andal." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Transfer Data Web." },
  { "en": "Apa Itu HTTPS (HTTP Secure)?", "id": "HTTP (Hypertext Transfer Protocol) Dengan Enkripsi." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Koneksi Aman Jaringan Publik." },
  { "en": "Apa Itu NAT (Network Address Translation)?", "id": "Mengubah IP Privat Ke Publik." },
  { "en": "Apa Itu Keamanan (Security) Siber?", "id": "Perlindungan Sistem Digital." },
  { "en": "Apa Itu Malware (Malicious Software)?", "id": "Perangkat Lunak Jahat." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Mengubah Data Jadi Tak Terbaca." },
  { "en": "Apa Itu Tanda Tangan (Signature) Digital?", "id": "Otentikasi, Integritas Pesan." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Sifat Antara." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Sirkuit Elektronik Chip." },
  { "en": "Apa Itu Hukum (Law) Ohm?", "id": "V = I * R." },
  { "en": "Apa Itu Hukum (Law) Kirchhoff?", "id": "Hukum Arus Dan Tegangan." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Energi Listrik." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Penyimpan Energi Magnetik." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total AC." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Kapasitor/Induktor." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Frekuensi Reaktansi Saling Hapus." },
  { "en": "Apa Itu Penguatan (Gain) Desibel?", "id": "Ukuran Logaritmik Rasio." },
  { "en": "Apa Itu Transformasi (Transform) Laplace?", "id": "Alat Analisis Sistem." },
  { "en": "Apa Itu Transformasi (Transform) Z?", "id": "Analisis Sistem Diskrit." },
  { "en": "Apa Itu Sirkuit (Circuit) Digital?", "id": "Sirkuit Logika Biner." },
  { "en": "Apa Itu Gerbang (Gate) Logika?", "id": "Elemen Logika Dasar." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Memori Satu Bit." },
  { "en": "Apa Itu Register (Register)?", "id": "Penyimpanan Data Sementara." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Pencacah Urutan Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil Cepat." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Chip." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Detektor Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Penggerak Berbasis Sinyal." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Tegangan AC." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem (System) Tenaga?", "id": "Jaringan Listrik Luas." },
  { "en": "Apa Itu Proteksi (Protection) Sistem?", "id": "Pengamanan Sistem Tenaga." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Daya Semikonduktor." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "DC Ke AC." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Output Pengaruhi Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Industri." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Kuantifikasi Besaran." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Dekat Nilai Benar." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Hasil Ukur Konsisten." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Dasar." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Visualisasi Sinyal Waktu." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Listrik Magnet." },
  { "en": "Apa Itu Gelombang (Wave) Elektromagnetik?", "id": "Perambatan Medan EM." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Pancar Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Informasi Ke Pembawa." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing) Biomedis?", "id": "Analisis Sinyal Tubuh (ECG)." },
  { "en": "Apa Itu Pencitraan (Imaging) Medis?", "id": "Visualisasi Dalam Tubuh (X-Ray)." },
  { "en": "Apa Itu Nanoteknologi (Nanotechnology)?", "id": "Teknologi Skala Nano." },
  { "en": "Apa Itu Bioteknologi (Biotechnology)?", "id": "Teknologi Sistem Biologi." },
  { "en": "Apa Itu Fisika (Physics) Modern?", "id": "Relativitas Dan Kuantum." },
  { "en": "Apa Itu Kecerdasan (Intelligence) Buatan (AI)?", "id": "Mesin Tiru Kecerdasan." },
  { "en": "Apa Itu Pembelajaran (Learning) Mesin (ML)?", "id": "AI Belajar Dari Data." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Tentang Robot." },
  { "en": "Apa Itu Internet (Internet) of Things (IoT)?", "id": "Jaringan Perangkat Terhubung." },
  { "en": "Apa Itu Big Data (Big Data)?", "id": "Data Volume Sangat Besar." },
  { "en": "Apa Itu Komputasi (Computing) Awan?", "id": "Layanan Komputasi Lewat Internet." },
  { "en": "Apa Itu IaaS (Infrastructure as a Service)?", "id": "Infrastruktur IT Sewaan (VM)." },
  { "en": "Apa Itu PaaS (Platform as a Service)?", "id": "Platform Aplikasi Sewaan." },
  { "en": "Apa Itu SaaS (Software as a Service)?", "id": "Perangkat Lunak Sewaan (Web)." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Penyaring Lalu Lintas Jaringan." },
  { "en": "Apa Itu Otentikasi (Authentication)?", "id": "Verifikasi Identitas Pengguna." },
  { "en": "Apa Itu Resistansi (Resistance) Total Seri?", "id": "Jumlah Total Semua Resistor." },
  { "en": "Apa Rumus Resistansi (Resistance) Total Paralel?", "id": "Satu Per Jumlah Satu Per R." },
  { "en": "Apa Itu Arus (Current) Rangkaian Seri?", "id": "Sama Di Setiap Titik Rangkaian." },
  { "en": "Apa Itu Tegangan (Voltage) Rangkaian Paralel?", "id": "Sama Di Setiap Cabang Paralel." },
  { "en": "Apa Itu Hukum (Law) Ohm?", "id": "V Sama Dengan I Kali R." },
  { "en": "Apa Itu Hukum (Law) Arus Kirchhoff?", "id": "Jumlah Arus Masuk Simpul Nol." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Energi Listrik (Watt)." },
  { "en": "Apa Rumus (Formula) Daya Listrik?", "id": "P Sama Dengan V Kali I." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Penyimpan Energi Medan Listrik." },
  { "en": "Apa Satuan (Unit) Kapasitansi?", "id": "Farad (F)." },
  { "en": "Apa Rumus Kapasitansi (Capacitance) Total Seri?", "id": "Satu Per Jumlah Satu Per C." },
  { "en": "Apa Rumus Kapasitansi (Capacitance) Total Paralel?", "id": "Jumlah Total Semua Kapasitansi." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Penyimpan Energi Medan Magnet." },
  { "en": "Apa Satuan (Unit) Induktansi?", "id": "Henry (H)." },
  { "en": "Apa Rumus Induktansi (Inductance) Total Seri?", "id": "Jumlah Total Semua Induktansi." },
  { "en": "Apa Rumus Induktansi (Inductance) Total Paralel?", "id": "Satu Per Jumlah Satu Per L." },
  { "en": "Apa Itu Reaktansi (Reactance) Kapasitif?", "id": "Hambatan Kapasitor Terhadap AC." },
  { "en": "Apa Itu Reaktansi (Reactance) Induktif?", "id": "Hambatan Induktor Terhadap AC." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total Rangkaian AC." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Kondisi Reaktansi Saling Menghilangkan." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Penyearah Arus." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Pengubah Sinyal AC Ke DC." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Meningkatkan Kekuatan Sinyal." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Gelombang Periodik." },
  { "en": "Apa Itu Gerbang (Gate) Logika?", "id": "Dasar Sirkuit Digital." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Satu Bit." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Ubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Ubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem (System) Tenaga?", "id": "Jaringan Listrik Skala Besar." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Potensi Tanah." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Konversi Daya Listrik." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Output Mempengaruhi Input." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur V, A, Ω." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Visualisasi Sinyal Listrik." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu IP Address (Alamat IP)?", "id": "Alamat Logis Perangkat." },
  { "en": "Apa Itu Router (Router)?", "id": "Penghubung Antar Jaringan." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Penghantar Listrik." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Penghambat Listrik." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Antara Konduktor Isolator." },
  { "en": "Apa Itu Sirkuit (Circuit) Terpadu (IC)?", "id": "Rangkaian Elektronik Chip." },
  { "en": "Apa Itu Sinyal (Signal) Analog?", "id": "Sinyal Nilai Kontinu." },
  { "en": "Apa Itu Sinyal (Signal) Digital?", "id": "Sinyal Nilai Diskrit." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Gangguan Acak Sinyal." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Sinyal." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Dekat Nilai Sebenarnya." },
  { "en": "Apa Itu Sensor (Sensor) Suhu?", "id": "Mengukur Suhu." },
  { "en": "Apa Itu Motor (Motor) DC?", "id": "Penggerak Putar DC." },
  { "en": "Apa Itu Motor (Motor) AC?", "id": "Penggerak Putar AC." },
  { "en": "Apa Itu Keselamatan (Safety) Listrik?", "id": "Pencegahan Bahaya Listrik." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih Putus." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit Otomatis." },
  { "en": "Apa Itu Resistor (Resistor) Film?", "id": "Resistor Lapisan Tipis Resistif." },
  { "en": "Apa Itu Resistor (Resistor) Komposisi Karbon?", "id": "Resistor Campuran Karbon Perekat." },
  { "en": "Apa Itu Resistor (Resistor) Variabel?", "id": "Resistansi Dapat Diubah (Potensiometer)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Keramik?", "id": "Kapasitor Dielektrik Keramik." },
  { "en": "Apa Itu Kapasitor (Capacitor) Elektrolit?", "id": "Kapasitor Polar Kapasitansi Tinggi." },
  { "en": "Apa Itu Kapasitor (Capacitor) Tantalum?", "id": "Jenis Kapasitor Elektrolit Padat." },
  { "en": "Apa Itu Kapasitor (Capacitor) Film?", "id": "Kapasitor Dielektrik Plastik Film." },
  { "en": "Apa Itu Induktor (Induktor) Inti Udara?", "id": "Induktor Tanpa Inti Magnetik." },
  { "en": "Apa Itu Induktor (Induktor) Inti Ferit?", "id": "Induktor Inti Bahan Ferit (HF)." },
  { "en": "Apa Itu Induktor (Induktor) Inti Besi?", "id": "Induktor Inti Besi Laminasi (LF)." },
  { "en": "Apa Itu Transformator (Transformer) Inti Besi?", "id": "Transformator Frekuensi Rendah." },
  { "en": "Apa Itu Transformator (Transformer) Inti Ferit?", "id": "Transformator Frekuensi Tinggi (SMPS)." },
  { "en": "Apa Itu Dioda (Diode) Penyearah?", "id": "Dioda Konversi AC Ke DC." },
  { "en": "Apa Itu Dioda (Diode) Sinyal?", "id": "Dioda Arus Kecil Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Dioda Regulator Tegangan Mundur." },
  { "en": "Apa Itu Dioda (Diode) Schottky?", "id": "Dioda Cepat Tegangan Rendah." },
  { "en": "Apa Itu Dioda (Diode) Varactor (Varicap)?", "id": "Dioda Kapasitansi Variabel." },
  { "en": "Apa Itu Transistor (Transistor) NPN?", "id": "Tipe BJT (Bipolar Junction Transistor) Umum." },
  { "en": "Apa Itu Transistor (Transistor) PNP?", "id": "Tipe BJT (Bipolar Junction Transistor) Komplementer NPN." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) N-Channel?", "id": "Tipe MOSFET (Metal-Oxide-Semiconductor FET) Umum." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) P-Channel?", "id": "Tipe MOSFET (Metal-Oxide-Semiconductor FET) Komplementer N-Channel." },
  { "en": "Apa Itu Thyristor (Thyristor) (SCR)?", "id": "Saklar Daya Semikonduktor Terkunci." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Saklar Daya AC Dua Arah." },
  { "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Pemicu TRIAC (Triode for Alternating Current)." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Saklar Daya Tinggi (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu Sirkuit Terpadu (IC) Linear?", "id": "IC (Integrated Circuit) Proses Sinyal Analog." },
  { "en": "Contoh IC (Integrated Circuit) Linear?", "id": "Op-Amp, Regulator Tegangan, Timer 555." },
  { "en": "Apa Itu Sirkuit Terpadu (IC) Digital?", "id": "IC (Integrated Circuit) Proses Sinyal Digital." },
  { "en": "Contoh IC (Integrated Circuit) Digital?", "id": "Gerbang Logika, Mikroprosesor, Memori." },
  { "en": "Apa Itu IC (Integrated Circuit) Sinyal Campuran?", "id": "IC (Integrated Circuit) Gabungkan Analog, Digital." },
  { "en": "Contoh IC (Integrated Circuit) Sinyal Campuran?", "id": "ADC (Analog-to-Digital Converter), DAC (Digital-to-Analog Converter)." },
  { "en": "Apa Itu Keluarga Logika (Logic Family) TTL?", "id": "Transistor-Transistor Logic." },
  { "en": "Apa Itu Keluarga Logika (Logic Family) CMOS?", "id": "Complementary Metal-Oxide-Semiconductor." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "IC (Integrated Circuit) Logika Terprogram." },
  { "en": "Apa Itu CPLD (Complex Programmable Logic Device)?", "id": "Perangkat Logika Terprogram Kompleks." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Perangkat Konversi Energi." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Hasilkan Gerakan/Aksi." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor)?", "id": "Mengukur Panas Atau Dingin." },
  { "en": "Jenis Sensor Suhu (Temperature Sensor)?", "id": "Termokopel, RTD (Resistance Temperature Detector), Termistor." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Efek Seebeck." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Resistansi Logam." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Semikonduktor Peka Suhu." },
  { "en": "Apa Itu Sensor Tekanan (Pressure Sensor)?", "id": "Mengukur Tekanan Fluida." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor)?", "id": "Mengukur Laju Aliran Fluida." },
  { "en": "Apa Itu Sensor Level (Level Sensor)?", "id": "Mengukur Ketinggian Cairan/Padatan." },
  { "en": "Apa Itu Sensor Jarak (Distance Sensor)?", "id": "Mengukur Jarak Ke Objek." },
  { "en": "Apa Itu Sensor Proximity (Proximity Sensor)?", "id": "Mendeteksi Keberadaan Objek Dekat." },
  { "en": "Apa Itu Sensor Cahaya (Light Sensor)?", "id": "Mendeteksi Intensitas Cahaya." },
  { "en": "Apa Itu Sensor Gerak (Motion Sensor)?", "id": "Mendeteksi Pergerakan Objek." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Mengukur Percepatan Linear." },
  { "en": "Apa Itu Giroskop (Gyroscope)?", "id": "Mengukur Kecepatan Sudut (Rotasi)." },
  { "en": "Apa Itu IMU (Inertial Measurement Unit)?", "id": "Gabungan Akselerometer Dan Giroskop." },
  { "en": "Apa Itu Magnetometer (Magnetometer)?", "id": "Mengukur Medan Magnet (Kompas)." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Pembangkit Listrik (Power Plant)?", "id": "Tempat Energi Listrik Dibangkitkan." },
  { "en": "Apa Itu Transmisi Listrik (Power Transmission)?", "id": "Penyaluran Daya Jarak Jauh (HV)." },
  { "en": "Apa Itu Distribusi Listrik (Power Distribution)?", "id": "Penyaluran Daya Ke Konsumen (MV/LV)." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Pengubah Tegangan, Switching." },
  { "en": "Apa Itu Transformator Daya (Power Transformer)?", "id": "Transformator Kapasitas Besar Gardu Induk." },
  { "en": "Apa Itu Pemutus Tenaga (Circuit Breaker)?", "id": "Saklar Pelindung Arus Gangguan." },
  { "en": "Apa Itu Sistem Proteksi (Protection System)?", "id": "Sistem Pengaman Jaringan Listrik." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Perangkat Deteksi Gangguan." },
  { "en": "Apa Itu Trafo Arus (CT)?", "id": "Mengukur Arus Tinggi Untuk Proteksi." },
  { "en": "Apa Itu Trafo Tegangan (PT)?", "id": "Mengukur Tegangan Tinggi Untuk Proteksi." },
  { "en": "Apa Itu Gangguan (Fault) Sistem Tenaga?", "id": "Kondisi Abnormal (Hubung Singkat)." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Kontak Antar Fasa/Tanah Impedansi Rendah." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Karakteristik Gelombang Listrik Ideal." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Fundamental." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Ukuran Efisiensi Penggunaan Daya." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Sumber Energi Alam Tak Habis." },
  { "en": "Contoh Energi Terbarukan (Renewable Energy)?", "id": "Surya, Angin, Air, Panas Bumi." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Mengubah Cahaya Matahari Jadi Listrik." },
  { "en": "Apa Itu Inverter (Inverter) Surya?", "id": "Mengubah DC Panel Surya Ke AC." },
  { "en": "Apa Itu MPPT (Maximum Power Point Tracking)?", "id": "Optimasi Daya Output Panel Surya." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Mengubah Energi Angin Jadi Listrik." },
  { "en": "Apa Itu PLTA (Pembangkit Listrik Tenaga Air)?", "id": "Menggunakan Energi Potensial Air." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Semikonduktor Kontrol Daya." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Konverter AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Konverter DC Ke AC." },
  { "en": "Apa Itu Konverter DC-DC (DC-DC Converter)?", "id": "Pengubah Level Tegangan DC." },
  { "en": "Apa Itu Thyristor (Thyristor) / SCR?", "id": "Saklar Daya Semikonduktor Terkunci." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Saklar Daya Kecepatan Tinggi Populer." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Rata-Rata Daya." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Kontrol Loop Terbuka (Open Loop)?", "id": "Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Kontrol Loop Tertutup (Closed Loop)?", "id": "Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Output Mempengaruhi Input." },
  { "en": "Apa Itu Kontroler PID (PID Controller)?", "id": "Kontroler Proporsional-Integral-Derivatif." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka Grafis Operator Mesin." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Terpusat." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Transfer Informasi Digital." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Alamat Fisik Perangkat Keras." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Penghubung Perangkat Satu LAN." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Pengkodean Data Agar Aman." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Interaksi Listrik Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Proses Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Proses Ekstraksi Informasi Dari Pembawa." },
  { "en": "Apa Itu Spektrum Frekuensi (Frequency Spectrum)?", "id": "Rentang Frekuensi Sinyal." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis, Modifikasi Sinyal." },
  { "en": "Apa Itu Sinyal Analog (Analog Signal)?", "id": "Sinyal Dengan Nilai Kontinu." },
  { "en": "Apa Itu Sinyal Digital (Digital Signal)?", "id": "Sinyal Dengan Nilai Diskrit." },
  { "en": "Apa Itu Konversi Analog-Digital (ADC)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Konversi Digital-Analog (DAC)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Memproses Sinyal Digital Ubah Spektrum." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Mengubah Sinyal Domain Waktu Ke Frekuensi." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Tentang Robot." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Gerakan Independen." },
  { "en": "Apa Itu Kinematika (Kinematics) Robot?", "id": "Studi Geometri Gerakan Robot." },
  { "en": "Apa Itu Dinamika (Dynamics) Robot?", "id": "Studi Gerakan Robot Memperhitungkan Gaya." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Kemampuan Komputer Menginterpretasi Gambar." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Ukur Visualisasi Bentuk Gelombang." },
  { "en": "Apa Sumbu Vertikal (Vertical Axis) Osiloskop?", "id": "Tegangan (Volt)." },
  { "en": "Apa Sumbu Horizontal (Horizontal Axis) Osiloskop?", "id": "Waktu (Time)." },
  { "en": "Apa Fungsi Tombol Volt/Div?", "id": "Mengatur Skala Tegangan Vertikal." },
  { "en": "Apa Fungsi Tombol Time/Div?", "id": "Mengatur Skala Waktu Horizontal." },
  { "en": "Apa Itu Pemicuan (Triggering) Osiloskop?", "id": "Menstabilkan Tampilan Gelombang Berulang." },
  { "en": "Apa Itu Mode AC Coupling (Kopling AC)?", "id": "Hanya Menampilkan Komponen AC Sinyal." },
  { "en": "Apa Itu Mode DC Coupling (Kopling DC)?", "id": "Menampilkan Komponen AC Dan DC Sinyal." },
  { "en": "Apa Fungsi Probe (Probe) Osiloskop?", "id": "Menghubungkan Rangkaian Ke Input Osiloskop." },
  { "en": "Apa Arti Probe (Probe) 10X?", "id": "Melemahkan Sinyal 10 Kali Lipat." },
  { "en": "Mengapa Menggunakan Probe (Probe) 10X?", "id": "Impedansi Input Tinggi, Beban Rendah." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Osiloskop?", "id": "Frekuensi Maksimum Dapat Diukur Akurat." },
  { "en": "Apa Itu Osiloskop Digital (DSO)?", "id": "Digital Storage Oscilloscope (DSO)." },
  { "en": "Apa Keuntungan Osiloskop Digital (DSO)?", "id": "Penyimpanan, Pengukuran Otomatis, Analisis." },
  { "en": "Apa Itu Laju Sampel (Sample Rate) DSO?", "id": "Kecepatan ADC (Analog-to-Digital Converter) Mengambil Sampel." },
  { "en": "Besaran Apa Diukur Multimeter (Multimeter)?", "id": "Tegangan, Arus, Dan Resistansi." },
  { "en": "Bagaimana Cara Mengukur Tegangan (Voltage)?", "id": "Hubungkan Paralel Dengan Komponen." },
  { "en": "Bagaimana Cara Mengukur Arus (Current)?", "id": "Hubungkan Seri Dengan Rangkaian." },
  { "en": "Bagaimana Cara Mengukur Resistansi (Resistance)?", "id": "Ukur Komponen (Daya Rangkaian Mati)." },
  { "en": "Apa Itu True RMS (True RMS) Multimeter?", "id": "Mengukur Nilai Efektif Akurat Non-Sinus." },
  { "en": "Apa Itu Tang Ampere (Clamp Meter)?", "id": "Mengukur Arus Tanpa Memutus Sirkuit." },
  { "en": "Prinsip Kerja Tang Ampere (Clamp Meter)?", "id": "Induksi Magnetik (Transformator Arus)." },
  { "en": "Apa Itu Generator Sinyal (Signal Generator)?", "id": "Alat Penghasil Sinyal Uji." },
  { "en": "Bentuk Gelombang Apa Dihasilkan Generator Sinyal?", "id": "Sinus, Kotak, Segitiga, Gigi Gergaji." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Menampilkan Sinyal Domain Frekuensi." },
  { "en": "Sumbu Horizontal (Horizontal Axis) Penganalisis Spektrum?", "id": "Frekuensi (Frequency)." },
  { "en": "Sumbu Vertikal (Vertical Axis) Penganalisis Spektrum?", "id": "Amplitudo (Daya, dBm)." },
  { "en": "Apa Itu Catu Daya (Power Supply) DC?", "id": "Sumber Tegangan Searah Teregulasi." },
  { "en": "Apa Itu Mode CV (Constant Voltage) Catu Daya?", "id": "Menjaga Tegangan Output Konstan." },
  { "en": "Apa Itu Mode CC (Constant Current) Catu Daya?", "id": "Menjaga Arus Output Konstan." },
  { "en": "Apa Itu LCR Meter (LCR Meter)?", "id": "Mengukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Apa Itu Pengujian Isolasi (Insulation Test)?", "id": "Mengukur Resistansi Isolasi Sangat Tinggi." },
  { "en": "Alat Apa Untuk Pengujian Isolasi?", "id": "Megohmmeter (Megger)." },
  { "en": "Apa Itu Pengujian Pembumian (Earth Test)?", "id": "Mengukur Resistansi Sistem Pembumian." },
  { "en": "Apa Itu Transformasi Bintang-Delta (Y-Δ)?", "id": "Konversi Ekuivalen Jaringan Resistor/Impedansi." },
  { "en": "Apa Itu Teorema Superposisi (Superposition Theorem)?", "id": "Analisis Rangkaian Linear Multi Sumber." },
  { "en": "Apa Itu Teorema Thevenin (Thevenin's Theorem)?", "id": "Sederhanakan Rangkaian Jadi Vth, Rth Seri." },
  { "en": "Apa Itu Tegangan Thevenin (Vth)?", "id": "Tegangan Rangkaian Terbuka Terminal Beban." },
  { "en": "Apa Itu Resistansi Thevenin (Rth)?", "id": "Resistansi Ekuivalen Dilihat Dari Terminal." },
  { "en": "Apa Itu Teorema Norton (Norton's Theorem)?", "id": "Sederhanakan Rangkaian Jadi In, Rn Paralel." },
  { "en": "Apa Itu Arus Norton (In)?", "id": "Arus Hubung Singkat Terminal Beban." },
  { "en": "Apa Hubungan Rth Dan Rn?", "id": "Resistansi Thevenin Sama Resistansi Norton." },
  { "en": "Apa Hubungan Vth, In, Rth?", "id": "Vth = In * Rth." },
  { "en": "Apa Teorema Transfer Daya Maksimum?", "id": "RL = Rth (DC), ZL = Zth* (AC)." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Perilaku Rangkaian Terhadap Input Frekuensi." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Logaritmik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Ukuran Tajamnya Puncak Resonansi." },
  { "en": "Apa Itu Jaringan Dua Port (Two-Port Network)?", "id": "Rangkaian Listrik Dua Pasang Terminal." },
  { "en": "Parameter Apa Jaringan Dua Port?", "id": "Parameter Z, Y, h, ABCD, S." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda Sambungan P-N (P-N Junction)?", "id": "Komponen Semikonduktor Searah Arus." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Area Sekitar Sambungan Miskin Muatan." },
  { "en": "Apa Itu Potensial Penghalang (Barrier Potential)?", "id": "Tegangan Internal Daerah Deplesi." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Pengubah AC Ke DC." },
  { "en": "Apa Itu Clipper (Clipping Circuit)?", "id": "Rangkaian Pemotong Level Tegangan." },
  { "en": "Apa Itu Clamper (Clamping Circuit)?", "id": "Rangkaian Penggeser Level DC." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Regulator Tegangan Bias Mundur." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Perangkat Penguat/Saklar Terkontrol Arus." },
  { "en": "Apa Itu Penguatan Beta (β) BJT?", "id": "Rasio Arus Kolektor, Basis." },
  { "en": "Apa Itu Titik Q (Q-Point) Transistor?", "id": "Titik Operasi DC Transistor." },
  { "en": "Apa Itu Penguat Common Emitter (Common Emitter)?", "id": "Penguat BJT (Bipolar Junction Transistor) Paling Umum." },
  { "en": "Apa Itu Penguat Common Collector (Common Collector)?", "id": "Pengikut Emitor (Emitter Follower)." },
  { "en": "Apa Itu Penguat Common Base (Common Base)?", "id": "Penguat Impedansi Input Rendah." },
  { "en": "Apa Itu Transistor FET (Field Effect Transistor)?", "id": "Perangkat Penguat/Saklar Terkontrol Tegangan." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET (Field Effect Transistor) Gerbang Terisolasi." },
  { "en": "Apa Itu Penguat Common Source (Common Source)?", "id": "Penguat FET (Field Effect Transistor) Paling Umum." },
  { "en": "Apa Itu Penguat Common Drain (Common Drain)?", "id": "Pengikut Source (Source Follower)." },
  { "en": "Apa Itu Penguat Common Gate (Common Gate)?", "id": "Penguat Impedansi Input Rendah." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Terbalik Fasa 180 Derajat." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Sirkuit Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Operasi Logika." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Penyimpan Data." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Pemilih Data Digital." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Distributor Data Digital." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Jaringan Listrik Skala Besar." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Konversi Daya Listrik." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Pengubah Level Tegangan DC." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Pengaturan Perilaku Sistem." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu Komunikasi (Communication) Data?", "id": "Transfer Informasi Digital." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Interaksi Listrik, Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan EM." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Ke Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Perangkat Pengubah Energi Satu Bentuk Lain." },
  { "en": "Apakah Sensor (Sensor) Termasuk Transduser (Transducer)?", "id": "Ya, Sensor Adalah Jenis Transduser." },
  { "en": "Apakah Aktuator (Actuator) Termasuk Transduser (Transducer)?", "id": "Ya, Aktuator Juga Transduser." },
  { "en": "Apa Itu Sensor (Sensor) Suhu?", "id": "Mengukur Besaran Suhu Panas Dingin." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Berbasis Efek Seebeck." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Resistansi Logam." },
  { "en": "Logam Apa Umum Dipakai RTD?", "id": "Platina (Platinum) (Contoh: Pt100)." },
  { "en": "Apa Itu Sensor (Sensor) Inframerah Suhu?", "id": "Mengukur Suhu Non-Kontak (Radiasi)." },
  { "en": "Apa Itu Sensor (Sensor) Tekanan?", "id": "Mengukur Tekanan Fluida (Gas/Cair)." },
  { "en": "Apa Itu Strain Gauge (Strain Gauge)?", "id": "Sensor Mengukur Regangan Mekanis." },
  { "en": "Apa Aplikasi Strain Gauge (Strain Gauge)?", "id": "Sel Beban (Load Cell) Timbangan." },
  { "en": "Apa Itu Sensor (Sensor) Aliran?", "id": "Mengukur Laju Aliran Fluida." },
  { "en": "Apa Itu Sensor (Sensor) Level?", "id": "Mengukur Ketinggian Cairan Dalam Tangki." },
  { "en": "Apa Itu Sensor (Sensor) Jarak?", "id": "Mengukur Jarak Ke Objek Target." },
  { "en": "Jenis Sensor (Sensor) Jarak Non-Kontak?", "id": "Ultrasonik, Inframerah, Laser." },
  { "en": "Apa Itu Sensor (Sensor) Proximity Induktif?", "id": "Mendeteksi Benda Logam Dekat." },
  { "en": "Apa Itu Sensor (Sensor) Proximity Kapasitif?", "id": "Mendeteksi Benda Logam/Non-Logam Dekat." },
  { "en": "Apa Itu Sensor (Sensor) Cahaya?", "id": "Mendeteksi Intensitas Cahaya." },
  { "en": "Contoh Sensor (Sensor) Cahaya?", "id": "LDR (Light Dependent Resistor), Fotodioda, Fototransistor." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistansi Berubah Tergantung Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Cahaya (Bias Mundur)." },
  { "en": "Apa Itu Fototransistor (Phototransistor)?", "id": "Transistor Basis Dikontrol Cahaya." },
  { "en": "Apa Itu Sensor (Sensor) Efek Hall?", "id": "Mendeteksi Keberadaan Medan Magnet." },
  { "en": "Aplikasi Sensor (Sensor) Efek Hall?", "id": "Sensor Posisi, Kecepatan, Arus." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Sensor Posisi Sudut Putaran." },
  { "en": "Apa Itu Encoder (Encoder) Inkremental?", "id": "Menghasilkan Pulsa Relatif Gerakan." },
  { "en": "Apa Itu Encoder (Encoder) Absolut?", "id": "Memberikan Kode Posisi Unik Mutlak." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Mengukur Percepatan Linear, Getaran." },
  { "en": "Apa Itu Pengkondisian (Conditioning) Sinyal?", "id": "Memproses Sinyal Sensor Agar Siap." },
  { "en": "Apa Saja Proses Pengkondisian (Conditioning) Sinyal?", "id": "Penguatan, Penyaringan, Linierisasi." },
  { "en": "Apa Itu Penguatan (Amplification) Sinyal?", "id": "Meningkatkan Amplitudo Sinyal Lemah." },
  { "en": "Apa Itu Penyaringan (Filtering) Sinyal?", "id": "Menghilangkan Komponen Frekuensi Tak Diinginkan." },
  { "en": "Apa Itu Linierisasi (Linearization) Sinyal?", "id": "Membuat Respon Sinyal Menjadi Linear." },
  { "en": "Apa Itu Isolasi (Isolation) Sinyal?", "id": "Memisahkan Elektrikal Input, Output." },
  { "en": "Apa Itu Sinyal (Signal) Analog 4-20mA?", "id": "Standar Transmisi Sinyal Arus Industri." },
  { "en": "Keuntungan Sinyal (Signal) 4-20mA?", "id": "Kebal Noise, Deteksi Kabel Putus (0mA)." },
  { "en": "Apa Itu Sinyal (Signal) Analog 0-10V?", "id": "Standar Transmisi Sinyal Tegangan Industri." },
  { "en": "Apa Itu Sistem (System) Akuisisi Data (DAQ)?", "id": "Sistem Pengumpulan Data Sinyal Analog." },
  { "en": "Apa Komponen (Component) Utama DAQ?", "id": "Sensor, Pengkondisi, Multiplexer, ADC, Komputer." },
  { "en": "Apa Fungsi Multiplexer (Multiplexer) DAQ?", "id": "Memilih Satu Kanal Sensor Bergantian." },
  { "en": "Apa Fungsi ADC (Analog-to-Digital Converter) Dalam DAQ?", "id": "Mengubah Sinyal Sensor Ke Digital." },
  { "en": "Apa Itu Laju (Rate) Sampling DAQ?", "id": "Seberapa Cepat Data Diambil Per Kanal." },
  { "en": "Apa Itu Resolusi (Resolution) DAQ?", "id": "Tingkat Detail Pengukuran (Bit ADC)." },
  { "en": "Apa Itu Aktuator (Actuator) Elektromekanis?", "id": "Penggerak Berbasis Prinsip Elektromagnetik." },
  { "en": "Contoh Aktuator (Actuator) Elektromekanis?", "id": "Relay, Solenoida, Motor Listrik." },
  { "en": "Apa Itu Solenoida (Solenoid)?", "id": "Aktuator Gerakan Linear (Tarik/Dorong)." },
  { "en": "Apa Itu Motor DC (Direct Current) Ber-sikat?", "id": "Motor DC (Direct Current) Komutasi Mekanis." },
  { "en": "Apa Itu Motor DC (Direct Current) Tanpa Sikat (BLDC)?", "id": "Motor DC (Direct Current) Komutasi Elektronik." },
  { "en": "Apa Itu Motor (Motor) Servo?", "id": "Motor Kontrol Posisi Umpan Balik." },
  { "en": "Apa Itu Aktuator (Actuator) Piezoelektrik?", "id": "Penggerak Presisi Tinggi Efek Piezo." },
  { "en": "Apa Itu Aktuator (Actuator) Pneumatik?", "id": "Penggerak Menggunakan Udara Bertekanan." },
  { "en": "Apa Itu Aktuator (Actuator) Hidrolik?", "id": "Penggerak Menggunakan Cairan Bertekanan." },
  { "en": "Apa Itu Kontrol (Control) Loop Terbuka?", "id": "Kontrol Tanpa Umpan Balik Output." },
  { "en": "Apa Itu Kontrol (Control) Loop Tertutup?", "id": "Kontrol Menggunakan Umpan Balik Output." },
  { "en": "Apa Itu Variabel (Variable) Proses (PV)?", "id": "Besaran Fisik Yang Dikontrol." },
  { "en": "Apa Itu Setpoint (Setpoint) (SP)?", "id": "Nilai Target Diinginkan Untuk PV." },
  { "en": "Apa Itu Kesalahan (Error) (e)?", "id": "Selisih Antara SP Dan PV." },
  { "en": "Apa Itu Kontroler (Controller)?", "id": "Memproses Error, Hasilkan Sinyal Kontrol." },
  { "en": "Apa Itu Elemen (Element) Kontrol Final?", "id": "Aktuator Yang Mempengaruhi Proses." },
  { "en": "Apa Itu Kontrol (Control) On-Off?", "id": "Kontroler Sederhana (Output 0% Atau 100%)." },
  { "en": "Apa Itu Histeresis (Hysteresis) Kontrol On-Off?", "id": "Mencegah Switching Terlalu Cepat (Chattering)." },
  { "en": "Apa Itu Kontrol (Control) Proporsional (P)?", "id": "Output Proporsional Terhadap Error." },
  { "en": "Apa Itu Gain (Gain) Proporsional (Kp)?", "id": "Faktor Penguatan Aksi Proporsional." },
  { "en": "Apa Itu Offset (Offset) Kontrol Proporsional?", "id": "Error Keadaan Tunak Yang Tersisa." },
  { "en": "Apa Itu Kontrol (Control) Integral (I)?", "id": "Output Proporsional Akumulasi Error." },
  { "en": "Apa Fungsi Aksi Integral (I)?", "id": "Menghilangkan Offset (Error Keadaan Tunak)." },
  { "en": "Apa Itu Waktu Integral (Integral Time) (Ti)?", "id": "Parameter Penentu Kecepatan Aksi Integral." },
  { "en": "Apa Itu Kontrol (Control) Derivatif (D)?", "id": "Output Proporsional Laju Perubahan Error." },
  { "en": "Apa Fungsi Aksi Derivatif (D)?", "id": "Memberi Efek Antisipasi (Prediktif)." },
  { "en": "Apa Itu Waktu Derivatif (Derivative Time) (Td)?", "id": "Parameter Penentu Kekuatan Aksi Derivatif." },
  { "en": "Apa Kerugian Aksi Derivatif (D)?", "id": "Memperkuat Noise Frekuensi Tinggi." },
  { "en": "Apa Itu Kontroler (Controller) PID?", "id": "Gabungan Aksi Proporsional, Integral, Derivatif." },
  { "en": "Apa Itu Penalaan (Tuning) PID?", "id": "Proses Penentuan Parameter Kp, Ti, Td." },
  { "en": "Apa Itu Respon (Response) Transien?", "id": "Perilaku Sistem Menuju Keseimbangan Baru." },
  { "en": "Apa Itu Respon (Response) Keadaan Tunak?", "id": "Perilaku Sistem Setelah Stabil." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Kontrol?", "id": "Kemampuan Kembali Seimbang Setelah Gangguan." },
  { "en": "Apa Itu Osilasi (Oscillation) Sistem Kontrol?", "id": "Output Berayun Terus Menerus." },
  { "en": "Apa Itu Overshoot (Limpasan)?", "id": "Output Melebihi Setpoint Sesaat." },
  { "en": "Apa Itu Waktu Tunda (Dead Time) Proses?", "id": "Waktu Tunda Awal Respon Proses." },
  { "en": "Apa Itu Kontrol (Control) Umpan Maju (Feedforward)?", "id": "Mengantisipasi Gangguan Sebelum Mempengaruhi Output." },
  { "en": "Apa Itu Kontrol (Control) Cascade?", "id": "Loop Kontrol Bersarang (Master, Slave)." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Digital Industri Kuat." },
  { "en": "Bahasa Pemrograman PLC (Programmable Logic Controller) Utama?", "id": "Ladder Logic (Logika Tangga)." },
  { "en": "Apa Itu Siklus Scan (Scan Cycle) PLC?", "id": "Baca Input -> Eksekusi Program -> Tulis Output." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka Grafis Operator." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Teknologi Desain, Operasi Robot." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Sumbu Gerak Independen." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Kemampuan Mesin Meniru Manusia." },
  { "en": "Apa Itu Pembelajaran Mesin (ML)?", "id": "AI (Artificial Intelligence) Belajar Dari Data." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Transistor Efek Medan Gerbang Terisolasi." },
  { "en": "Apa Terminal Utama MOSFET?", "id": "Gate, Drain, Source." },
  { "en": "Fungsi Gate (Gerbang) MOSFET?", "id": "Mengontrol Aliran Arus Drain-Source." },
  { "en": "Apa Itu Mode Peningkatan (Enhancement)?", "id": "MOSFET Normalnya Mati (OFF)." },
  { "en": "Apa Itu Mode Deplesi (Depletion)?", "id": "MOSFET Normalnya Hidup (ON)." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Teknologi Kombinasi NMOS Dan PMOS." },
  { "en": "Keuntungan Utama CMOS?", "id": "Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Penguat Common Source?", "id": "Konfigurasi Penguat Dasar MOSFET." },
  { "en": "Apa Itu Penguat Common Drain?", "id": "Pengikut Source (Source Follower)." },
  { "en": "Apa Itu Penguat Common Gate?", "id": "Konfigurasi Input Impedansi Rendah." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Menyalin Arus Referensi." },
  { "en": "Apa Itu Beban Aktif (Active Load)?", "id": "Menggunakan Transistor Sebagai Beban." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Gain Sangat Tinggi Serbaguna." },
  { "en": "Karakteristik Op-Amp Ideal?", "id": "Gain Tak Hingga, Input Z Tak Hingga." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Menstabilkan Penguatan Op-Amp." },
  { "en": "Apa Itu Penguat Inverting?", "id": "Output Berbeda Fasa 180°." },
  { "en": "Apa Itu Penguat Non-Inverting?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut Tegangan?", "id": "Buffer (Buffer) Gain Satu." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Sama." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Penghasil Sinyal Periodik." },
  { "en": "Apa Kriteria Osilasi Barkhausen?", "id": "Gain Loop 1, Fasa Loop 0°." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter Menggunakan Op-Amp." },
  { "en": "Apa Itu Timer (Timer) 555?", "id": "IC (Integrated Circuit) Untuk Pewaktuan, Osilasi." },
  { "en": "Apa Itu Regulator (Regulator) Tegangan?", "id": "Menjaga Tegangan Output Konstan." },
  { "en": "Apa Itu Regulator (Regulator) Switching?", "id": "Regulator Efisiensi Tinggi (Buck/Boost)." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Dasar Sirkuit Logika Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Logika Biner." },
  { "en": "Apa Itu Register (Register)?", "id": "Penyimpanan Data Sementara Digital." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Penyimpanan Informasi Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil Baca Tulis." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil Hanya Baca." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Chip." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Digital Ke Analog." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Tegangan Arus AC." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Dari Gangguan." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Potensi Bumi." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Pengubah Daya DC Ke AC." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Output Mempengaruhi Input Kontrol." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Benar." },
  { "en": "Apa Itu Medan Elektromagnetik (Electromagnetic Field)?", "id": "Medan Kombinasi Listrik, Magnet." },
  { "en": "Apa Itu Bahan Konduktor (Conductor Material)?", "id": "Bahan Penghantar Listrik." },
  { "en": "Apa Itu Bahan Isolator (Insulator Material)?", "id": "Bahan Penghambat Listrik." },
  { "en": "Apa Itu Bahan Semikonduktor (Semiconductor Material)?", "id": "Bahan Sifat Antara." },
  { "en": "Apa Itu Hukum Ohm (Ohm's Law)?", "id": "V = I * R." },
  { "en": "Apa Itu Hukum Kirchhoff (Kirchhoff's Laws)?", "id": "Hukum Arus Dan Tegangan." },
  { "en": "Apa Itu Daya Listrik (Electric Power)?", "id": "Laju Energi Listrik." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Non-Resistif AC." },
  { "en": "Apa Itu Sinyal Analog (Analog Signal)?", "id": "Sinyal Nilai Kontinu." },
  { "en": "Apa Itu Sinyal Digital (Digital Signal)?", "id": "Sinyal Nilai Diskrit." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Analisis Frekuensi Sinyal." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Robot." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Mesin Tiru Kecerdasan Manusia." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Perlindungan Sistem Digital." },
  { "en": "Apa Itu Blockchain (Blockchain)?", "id": "Buku Besar Digital Terdistribusi." },
  { "en": "Apa Itu Nanoteknologi (Nanotechnology)?", "id": "Teknologi Skala Nanometer." },
  { "en": "Apa Itu Fisika Modern (Modern Physics)?", "id": "Relativitas Dan Kuantum." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Sumber Energi Alam Berkelanjutan." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Pengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Pengubah Energi Angin Listrik." },
  { "en": "Apa Itu Penyimpanan Energi (Energy Storage)?", "id": "Menyimpan Energi Listrik (Baterai)." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Jaringan Listrik Digital Komunikasi." },
  { "en": "Apa Itu Kendaraan Listrik (Electric Vehicle)?", "id": "Kendaraan Penggerak Motor Listrik." },
  { "en": "Apa Itu Pencitraan Medis (Medical Imaging)?", "id": "Visualisasi Dalam Tubuh (MRI, CT)." },
  { "en": "Apa Itu Elektronika Biomedis (Biomedical Electronics)?", "id": "Elektronika Aplikasi Medis." },
  { "en": "Apa Itu Sensor Biomedis (Biomedical Sensor)?", "id": "Sensor Ukur Sinyal Tubuh (ECG)." },
  { "en": "Apa Itu Sistem Kontrol (Control System) Digital?", "id": "Sistem Kontrol Menggunakan Komputer Digital." },
  { "en": "Apa Itu Pencuplikan (Sampling) Sinyal Kontrol?", "id": "Mengubah Sinyal Analog Ke Diskrit Waktu." },
  { "en": "Apa Itu Kuantisasi (Quantization) Sinyal Kontrol?", "id": "Mengubah Nilai Sampel Ke Diskrit Amplitudo." },
  { "en": "Apa Itu Transformasi-Z (Z-Transform)?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Fungsi Transfer (Transfer Function) Diskrit H(z)?", "id": "Rasio Output(z) Terhadap Input(z)." },
  { "en": "Apa Itu Pole (Kutub) Dan Zero (Nol) Sistem Diskrit?", "id": "Akar Penyebut Dan Pembilang H(z)." },
  { "en": "Dimana Pole (Kutub) Sistem Diskrit Stabil?", "id": "Di Dalam Lingkaran Satuan (Unit Circle)." },
  { "en": "Apa Itu Lingkaran Satuan (Unit Circle)?", "id": "Lingkaran Jari-Jari Satu Bidang-Z." },
  { "en": "Apa Itu Metode Diskritisasi (Discretization)?", "id": "Mengubah Kontroler Kontinu Ke Diskrit." },
  { "en": "Contoh Metode Diskritisasi (Discretization)?", "id": "Euler, Tustin (Transformasi Bilinear)." },
  { "en": "Apa Itu Kontroler PID (PID Controller) Digital?", "id": "Implementasi Algoritma PID Secara Digital." },
  { "en": "Apa Itu Penahan Orde Nol (Zero-Order Hold)?", "id": "Model Efek DAC (Digital-to-Analog Converter) Menahan Nilai." },
  { "en": "Apa Kepanjangan ZOH (Zero-Order Hold)?", "id": "Penahan Orde Nol." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Jaringan Pembangkitan, Transmisi, Distribusi." },
  { "en": "Apa Itu Pembangkit Listrik (Power Plant)?", "id": "Fasilitas Konversi Energi Menjadi Listrik." },
  { "en": "Apa Itu Transmisi (Transmission) Listrik?", "id": "Penyaluran Daya Besar Jarak Jauh (HV)." },
  { "en": "Apa Itu Distribusi (Distribution) Listrik?", "id": "Penyaluran Daya Ke Konsumen (MV/LV)." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Pengubah Tegangan, Switching Jaringan." },
  { "en": "Apa Itu Pemutus Tenaga (Circuit Breaker)?", "id": "Saklar Pelindung Pemutus Arus Gangguan." },
  { "en": "Apa Itu Pemisah (Disconnector/Isolator)?", "id": "Saklar Pemisah Tanpa Beban Arus." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Kontak Impedansi Rendah Antar Konduktor." },
  { "en": "Apa Itu Arus Hubung Singkat (Short Circuit Current)?", "id": "Arus Sangat Tinggi Saat Gangguan." },
  { "en": "Apa Itu Studi Hubung Singkat (Short Circuit Study)?", "id": "Perhitungan Arus Gangguan Maksimum." },
  { "en": "Apa Itu Sistem Pembumian (Grounding System)?", "id": "Koneksi Sistem Ke Tanah." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Karakteristik Ideal Tegangan, Arus." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Komponen Frekuensi Kelipatan Fundamental." },
  { "en": "Apa Penyebab Utama Harmonisa (Harmonics)?", "id": "Beban Non-Linear (Elektronika Daya)." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Daya Nyata Terhadap Daya Semu." },
  { "en": "Apa Itu Koreksi Faktor Daya (Power Factor Correction)?", "id": "Memperbaiki Faktor Daya (Pasang Kapasitor)." },
  { "en": "Apa Itu Studi Aliran Daya (Load Flow Study)?", "id": "Analisis Aliran Daya, Tegangan Jaringan." },
  { "en": "Apa Itu Bus (Bus) Referensi (Slack Bus)?", "id": "Bus Referensi Tegangan, Fasa (Aliran Daya)." },
  { "en": "Apa Itu Bus (Bus) Beban (Load Bus) (PQ Bus)?", "id": "Bus Diketahui Daya Aktif, Reaktif." },
  { "en": "Apa Itu Bus (Bus) Generator (Generator Bus) (PV Bus)?", "id": "Bus Diketahui Daya Aktif, Tegangan." },
  { "en": "Apa Itu Matriks Admitansi (Admittance Matrix) Bus (Ybus)?", "id": "Representasi Jaringan Analisis Aliran Daya." },
  { "en": "Metode Solusi Aliran Daya (Load Flow)?", "id": "Gauss-Seidel, Newton-Raphson." },
  { "en": "Apa Itu Stabilitas (Stability) Sistem Tenaga?", "id": "Kemampuan Sistem Kembali Normal Pasca Gangguan." },
  { "en": "Apa Itu Stabilitas Sudut Rotor (Rotor Angle)?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas Tegangan (Voltage Stability)?", "id": "Kemampuan Sistem Mempertahankan Tegangan." },
  { "en": "Apa Itu Stabilitas Frekuensi (Frequency Stability)?", "id": "Kemampuan Sistem Mempertahankan Frekuensi." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Energi Sumber Alam Berkelanjutan." },
  { "en": "Apa Itu Sel Fotovoltaik (Photovoltaic Cell)?", "id": "Pengubah Langsung Cahaya Matahari Ke Listrik." },
  { "en": "Apa Itu Panel Surya (Solar Panel)?", "id": "Kumpulan Modul Fotovoltaik." },
  { "en": "Apa Itu MPPT (Maximum Power Point Tracking)?", "id": "Optimasi Daya Maksimum Panel Surya." },
  { "en": "Apa Itu Sistem On-Grid (On-Grid System) Surya?", "id": "Sistem Terhubung Ke Jaringan PLN." },
  { "en": "Apa Itu Sistem Off-Grid (Off-Grid System) Surya?", "id": "Sistem Mandiri Dengan Penyimpanan Baterai." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Mengubah Energi Kinetik Angin Ke Listrik." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air (PLTA)?", "id": "Menggunakan Energi Potensial/Kinetik Air." },
  { "en": "Apa Itu Penyimpanan Energi (Energy Storage)?", "id": "Menyimpan Energi Listrik Untuk Nanti." },
  { "en": "Contoh Penyimpanan Energi (Energy Storage)?", "id": "Baterai, Penyimpanan Pompa Hidro, Superkapasitor." },
  { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Komunikasi Digital." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Listrik Komunikasi Dua Arah." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Mengelola Pola Konsumsi Listrik." },
  { "en": "Apa Itu Kendaraan Listrik (Electric Vehicle) (EV)?", "id": "Kendaraan Penggerak Motor Listrik." },
  { "en": "Apa Itu Pengisian (Charging) Kendaraan Listrik?", "id": "Proses Pengisian Baterai Kendaraan Listrik." },
  { "en": "Apa Itu Pengisian (Charging) AC?", "id": "Pengisian Arus Bolak-Balik (Lebih Lambat)." },
  { "en": "Apa Itu Pengisian (Charging) DC Cepat?", "id": "Pengisian Arus Searah (Sangat Cepat)." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Kembalikan Daya Ke Jaringan." },
  { "en": "Apa Itu Elektronika Biomedis (Biomedical Electronics)?", "id": "Elektronika Aplikasi Dunia Medis." },
  { "en": "Apa Itu Sinyal Biomedis (Biomedical Signal)?", "id": "Sinyal Listrik Fisiologis Tubuh." },
  { "en": "Apa Itu ECG (Electrocardiogram)?", "id": "Perekaman Sinyal Listrik Jantung." },
  { "en": "Apa Itu EEG (Electroencephalogram)?", "id": "Perekaman Sinyal Listrik Otak." },
  { "en": "Apa Itu EMG (Electromyogram)?", "id": "Perekaman Sinyal Listrik Otot." },
  { "en": "Apa Itu Elektroda (Electrode) Biomedis?", "id": "Sensor Kontak Tubuh Perekaman Sinyal." },
  { "en": "Apa Itu Penguat Biopotensial (Biopotential Amplifier)?", "id": "Penguat Sinyal Biomedis (CMRR Tinggi)." },
  { "en": "Apa Itu Pencitraan Medis (Medical Imaging)?", "id": "Teknik Visualisasi Organ Tubuh." },
  { "en": "Contoh Pencitraan Medis (Medical Imaging)?", "id": "Sinar-X, CT Scan, MRI, Ultrasound." },
  { "en": "Apa Itu Sinar-X (X-Ray)?", "id": "Pencitraan Atenuasi Radiasi Elektromagnetik." },
  { "en": "Apa Itu CT Scan (Computed Tomography)?", "id": "Pencitraan Sinar-X Tiga Dimensi." },
  { "en": "Apa Itu Ultrasonografi (Ultrasound)?", "id": "Pencitraan Pantulan Gelombang Suara." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Stimulator Listrik Ritme Jantung." },
  { "en": "Apa Itu Defibrilator (Defibrillator)?", "id": "Alat Kejut Listrik Hentikan Aritmia." },
  { "en": "Apa Itu Nanomaterial (Nanomaterial)?", "id": "Material Struktur Skala Nano." },
  { "en": "Apa Itu Nanosensor (Nanosensor)?", "id": "Sensor Berbasis Prinsip Skala Nano." },
  { "en": "Apa Itu Nanoelektronika (Nanoelectronics)?", "id": "Perangkat Elektronik Skala Nanometer." },
  { "en": "Apa Itu Fotonik (Photonics)?", "id": "Ilmu Teknologi Berbasis Cahaya (Foton)." },
  { "en": "Apa Itu Laser (Laser)?", "id": "Sumber Cahaya Koheren Monokromatik." },
  { "en": "Apa Itu Serat Optik (Optical Fiber)?", "id": "Pemandu Cahaya Transmisi Data." },
  { "en": "Apa Itu Optoelektronika (Optoelectronics)?", "id": "Perangkat Elektronik Interaksi Cahaya." },
  { "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?", "id": "Komputasi Berbasis Fenomena Kuantum." },
  { "en": "Apa Itu Qubit (Qubit)?", "id": "Unit Informasi Kuantum." },
  { "en": "Apa Sifat Unik Qubit (Qubit)?", "id": "Superposisi Dan Keterikatan." },
  { "en": "Apa Itu Superkonduktivitas (Superconductivity)?", "id": "Hilangnya Resistansi Listrik Total." },
  { "en": "Kapan Superkonduktivitas (Superconductivity) Terjadi?", "id": "Pada Suhu Sangat Rendah (Kritis)." },
  { "en": "Apa Itu Efek Meissner (Meissner Effect)?", "id": "Penolakan Medan Magnet Superkonduktor." },
  { "en": "Aplikasi Superkonduktor (Superconductor)?", "id": "MRI (Magnetic Resonance Imaging), Maglev, Pembatas Arus." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Desain, Operasi Robot." },
  { "en": "Apa Itu Robot (Robot) Industri?", "id": "Manipulator Otomatis Pabrik." },
  { "en": "Apa Itu Robot (Robot) Bergerak?", "id": "Robot Dapat Berpindah Tempat." },
  { "en": "Apa Itu Derajat Kebebasan (DOF)?", "id": "Jumlah Gerakan Independen Robot." },
  { "en": "Apa Itu Dinamika (Dynamics) Robot?", "id": "Studi Gerakan Robot Terkait Gaya." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Kemampuan Komputer Interpretasi Citra." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Kemampuan Mesin Meniru Kecerdasan." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Inspirasi Otak." },
  { "en": "Apa Itu Deep Learning (Pembelajaran Mendalam)?", "id": "ML (Machine Learning) Jaringan Saraf Dalam." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Layanan Komputasi Lewat Internet." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) N-Channel?", "id": "Transistor MOSFET (Metal-Oxide-Semiconductor FET) Kanal N." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) P-Channel?", "id": "Transistor MOSFET (Metal-Oxide-Semiconductor FET) Kanal P." },
  { "en": "Mode Apa Yang Paling Umum Untuk MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Mode Peningkatan (Enhancement Mode)." },
  { "en": "Apa Itu Daerah Cut-Off (Cut-Off Region) MOSFET?", "id": "Transistor Mati (Off), Tidak Ada Arus." },
  { "en": "Apa Itu Daerah Triode (Triode Region) MOSFET?", "id": "Bertindak Seperti Resistor Terkontrol Tegangan." },
  { "en": "Apa Nama Lain Daerah Triode (Triode Region) MOSFET?", "id": "Daerah Linear (Linear Region)." },
  { "en": "Apa Itu Daerah Saturasi (Saturation Region) MOSFET?", "id": "Transistor Aktif, Arus Konstan (Penguat)." },
  { "en": "Apa Fungsi Utama JFET (Junction Field Effect Transistor)?", "id": "Penguat Sinyal, Saklar Impedansi Tinggi." },
  { "en": "Apa Itu Tegangan Pinch-Off (Pinch-Off Voltage) JFET?", "id": "Tegangan Gate Mematikan Arus Drain." },
  { "en": "Apa Itu Garis Beban (Load Line) AC Penguat?", "id": "Garis Kerja Penguat Sinyal AC." },
  { "en": "Apa Itu Respons Frekuensi Rendah (Low-Frequency Response)?", "id": "Pengaruh Kapasitor Kopling, Bypass." },
  { "en": "Apa Itu Respons Frekuensi Tinggi (High-Frequency Response)?", "id": "Pengaruh Kapasitansi Internal Transistor." },
  { "en": "Apa Itu Penguat Umpan Balik (Feedback Amplifier)?", "id": "Penguat Dengan Sebagian Output Kembali Input." },
  { "en": "Apa Itu Umpan Balik Positif (Positive Feedback)?", "id": "Sinyal Umpan Balik Memperkuat Input." },
  { "en": "Dimana Umpan Balik Positif (Positive Feedback) Digunakan?", "id": "Rangkaian Osilator, Schmitt Trigger." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Sinyal Umpan Balik Melawan Input." },
  { "en": "Apa Manfaat Umpan Balik Negatif (Negative Feedback)?", "id": "Stabilitas, Kurangi Distorsi, Atur Gain." },
  { "en": "Sebutkan Empat Topologi Umpan Balik (Feedback Topology)?", "id": "Seri-Shunt, Shunt-Seri, Seri-Seri, Shunt-Shunt." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Antara Dua Input." },
  { "en": "Apa Itu Sinyal Mode Bersama (Common-Mode Signal)?", "id": "Sinyal Yang Sama Di Kedua Input." },
  { "en": "Apa Itu Sinyal Mode Diferensial (Differential-Mode Signal)?", "id": "Sinyal Yang Berbeda Di Kedua Input." },
  { "en": "Apa Itu Beban Aktif (Active Load)?", "id": "Mengganti Resistor Beban Dengan Transistor." },
  { "en": "Apa Keuntungan Beban Aktif (Active Load)?", "id": "Mendapatkan Penguatan Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Rangkaian Cascode (Cascode Amplifier)?", "id": "Gabungan Common Emitter, Common Base." },
  { "en": "Apa Keuntungan Rangkaian Cascode (Cascode Amplifier)?", "id": "Bandwidth (Lebar Pita) Sangat Lebar." },
  { "en": "Apa Itu Resistansi Negatif (Negative Resistance)?", "id": "Karakteristik Arus Turun Saat Tegangan Naik." },
  { "en": "Komponen Apa Punya Resistansi Negatif (Negative Resistance)?", "id": "Dioda Tunnel, UJT (Unijunction Transistor)." },
  { "en": "Apa Itu Rangkaian Pelipat Tegangan (Voltage Multiplier)?", "id": "Menghasilkan Tegangan DC Kelipatan AC." },
  { "en": "Apa Itu Rangkaian Pengganda Tegangan (Voltage Doubler)?", "id": "Menghasilkan Tegangan DC Dua Kali Puncak." },
  { "en": "Apa Itu Rangkaian Penjepit (Clamper Circuit)?", "id": "Rangkaian Penggeser Level DC Sinyal." },
  { "en": "Apa Nama Lain Rangkaian Penjepit (Clamper Circuit)?", "id": "Pemulih DC (DC Restorer)." },
  { "en": "Apa Itu Rangkaian Pemotong (Clipper Circuit)?", "id": "Rangkaian Pemotong Sebagian Sinyal." },
  { "en": "Apa Nama Lain Rangkaian Pemotong (Clipper Circuit)?", "id": "Limiter (Pembatas)." },
  { "en": "Apa Itu Rangkaian Saklar Analog (Analog Switch)?", "id": "Saklar Elektronik Melewatkan Sinyal Analog." },
  { "en": "Contoh Komponen Saklar Analog (Analog Switch)?", "id": "JFET (Junction Field Effect Transistor), MOSFET (Metal-Oxide-Semiconductor FET), IC 4066." },
  { "en": "Apa Itu Rangkaian Sample and Hold (S&H)?", "id": "Mencuplik, Menahan Tegangan Sinyal Analog." },
  { "en": "Dimana Rangkaian Sample and Hold (S&H) Digunakan?", "id": "Pada Masukan ADC (Analog-to-Digital Converter)." },
  { "en": "Apa Itu Resolusi (Resolution) ADC (Analog-to-Digital Converter)?", "id": "Jumlah Bit Output Digital." },
  { "en": "Apa Itu Flash ADC (Flash Analog-to-Digital Converter)?", "id": "Jenis ADC (Analog-to-Digital Converter) Paling Cepat." },
  { "en": "Apa Itu Successive Approximation ADC (SAR ADC)?", "id": "Jenis ADC (Analog-to-Digital Converter) Paling Umum." },
  { "en": "Apa Itu Dual-Slope ADC (Dual-Slope Analog-to-Digital Converter)?", "id": "Jenis ADC (Analog-to-Digital Converter) Lambat, Akurasi Tinggi." },
  { "en": "Apa Itu Sigma-Delta ADC (Sigma-Delta Analog-to-Digital Converter)?", "id": "Jenis ADC (Analog-to-Digital Converter) Resolusi Sangat Tinggi." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) R-2R Ladder?", "id": "DAC (Digital-to-Analog Converter) Menggunakan Jaringan Resistor R-2R." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) Weighted Resistor?", "id": "DAC (Digital-to-Analog Converter) Resistor Terbobot Biner." },
  { "en": "Apa Itu Monotonisitas (Monotonicity) DAC?", "id": "Output Selalu Naik Sesuai Input." },
  { "en": "Apa Itu Linearitas (Linearity) ADC (Analog-to-Digital Converter) / DAC (Digital-to-Analog Converter)?", "id": "Ukuran Kedekatan Respon Garis Lurus." },
  { "en": "Apa Itu Sistem Bilangan Heksadesimal (Hexadecimal)?", "id": "Sistem Bilangan Basis 16." },
  { "en": "Simbol Apa Yang Digunakan Heksadesimal (Hexadecimal)?", "id": "Angka 0-9 Dan Huruf A-F." },
  { "en": "Apa Itu Kode ASCII (American Standard Code for Information Interchange)?", "id": "Standar Pengkodean Karakter Teks." },
  { "en": "Apa Itu Gerbang Logika Universal (Universal Logic Gate)?", "id": "Gerbang Pembentuk Semua Gerbang Lain." },
  { "en": "Gerbang Apa Yang Termasuk Gerbang Universal?", "id": "NAND (Not AND) Dan NOR (Not OR)." },
  { "en": "Apa Itu Propagasi Tunda (Propagation Delay) Gerbang Logika?", "id": "Waktu Tunda Input Berubah Ke Output." },
  { "en": "Apa Itu Disipasi Daya (Power Dissipation) Gerbang Logika?", "id": "Konsumsi Daya Rata-Rata Gerbang." },
  { "en": "Apa Itu Logika Positif (Positive Logic)?", "id": "Level Tinggi Adalah 1, Rendah 0." },
  { "en": "Apa Itu Logika Negatif (Negative Logic)?", "id": "Level Tinggi Adalah 0, Rendah 1." },
  { "en": "Apa Itu Buffer (Penyangga) Logika?", "id": "Penguat Arus (Driver), Non-Inverting." },
  { "en": "Apa Itu Buffer Tri-State (Tri-State Buffer)?", "id": "Buffer Dengan Tiga Keadaan Output." },
  { "en": "Apa Tiga Keadaan Output Tri-State?", "id": "Tinggi (High), Rendah (Low), Impedansi Tinggi (Hi-Z)." },
  { "en": "Apa Fungsi Keadaan Impedansi Tinggi (Hi-Z)?", "id": "Melepas Output Dari Bus (Jalur)." },
  { "en": "Apa Itu Open Collector (Kolektor Terbuka) Output?", "id": "Output Transistor Tanpa Resistor Pull-Up." },
  { "en": "Apa Keuntungan Output Open Collector (Kolektor Terbuka)?", "id": "Dapat Dihubungkan Bersama (Wired-AND)." },
  { "en": "Apa Itu Open Drain (Drain Terbuka) Output?", "id": "Sama Seperti Open Collector (Kolektor Terbuka), Tapi MOSFET." },
  { "en": "Apa Itu Resistor Pull-Up (Pull-Up Resistor)?", "id": "Resistor Ke VCC (Tegangan Positif) Jaga Level Tinggi." },
  { "en": "Apa Itu Resistor Pull-Down (Pull-Down Resistor)?", "id": "Resistor Ke Ground Jaga Level Rendah." },
  { "en": "Apa Itu Rangkaian Aritmetika (Arithmetic Circuit)?", "id": "Rangkaian Operasi Matematika." },
  { "en": "Apa Itu Subtractor (Pengurang) Digital?", "id": "Rangkaian Pengurang Bilangan Biner." },
  { "en": "Bagaimana Membuat Subtractor (Pengurang) Menggunakan Adder (Penjumlah)?", "id": "Menggunakan Metode Komplemen Dua." },
  { "en": "Apa Itu Magnitude Comparator (Pembanding Besaran)?", "id": "Rangkaian Pembanding Dua Bilangan Biner." },
  { "en": "Apa Itu Clock Skew (Kemiringan Clock)?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu Clock Jitter (Gemetar Clock)?", "id": "Variasi Periode Sinyal Clock." },
  { "en": "Apa Itu Counter (Pencacah) Modulus?", "id": "Jumlah Keadaan (State) Counter." },
  { "en": "Apa Itu Counter (Pencacah) Mod-10 (MOD-10)?", "id": "Pencacah Dekade (0 Sampai 9)." },
  { "en": "Apa Itu Counter (Pencacah) BCD (Binary Coded Decimal)?", "id": "Nama Lain Counter (Pencacah) Mod-10." },
  { "en": "Apa Itu Ring Counter (Pencacah Cincin)?", "id": "Register Geser Dengan Output Kembali Input." },
  { "en": "Apa Itu Johnson Counter (Pencacah Johnson)?", "id": "Ring Counter (Pencacah Cincin) Dengan Output Terbalik." },
  { "en": "Apa Itu Dekoder (Decoder) 7-Segment?", "id": "Mengubah Kode BCD (Binary Coded Decimal) Ke Tampilan 7-Segment." },
  { "en": "Apa Itu Tampilan 7-Segment (7-Segment Display)?", "id": "Tampilan LED (Light Emitting Diode) Untuk Angka." },
  { "en": "Apa Itu Common Anode (Anoda Bersama) 7-Segment?", "id": "Semua Anoda LED Terhubung Bersama (Ke VCC)." },
  { "en": "Apa Itu Common Cathode (Katoda Bersama) 7-Segment?", "id": "Semua Katoda LED Terhubung Bersama (Ke Ground)." },
  { "en": "Apa Itu Arsitektur Memori (Memory Architecture)?", "id": "Organisasi Sel Memori." },
  { "en": "Apa Itu Sel Memori (Memory Cell)?", "id": "Unit Dasar Penyimpan Satu Bit." },
  { "en": "Apa Itu Waktu Akses (Access Time) Memori?", "id": "Waktu Baca Data Dari Memori." },
  { "en": "Apa Itu Waktu Siklus (Cycle Time) Memori?", "id": "Waktu Total Operasi Memori (Baca/Tulis)." },
  { "en": "Apa Kepanjangan SDRAM (Synchronous DRAM)?", "id": "DRAM (Dynamic RAM) Sinkron (Mengikuti Clock)." },
  { "en": "Apa Kepanjangan DDR (Double Data Rate) SDRAM?", "id": "SDRAM (Synchronous DRAM) Transfer Data Dua Kali." },
  { "en": "Apa Kepanjangan NVRAM (Non-Volatile RAM)?", "id": "RAM (Random Access Memory) Non-Volatil (Baterai Cadangan)." },
  { "en": "Apa Itu Memori FIFO (First-In First-Out)?", "id": "Memori Antrian (Data Masuk Pertama, Keluar Pertama)." },
  { "en": "Apa Itu Memori LIFO (Last-In First-Out)?", "id": "Memori Tumpukan (Stack) (Data Masuk Terakhir, Keluar Pertama)." },
  { "en": "Apa Nama Lain Memori LIFO (Last-In First-Out)?", "id": "Stack (Tumpukan)." },
  { "en": "Apa Itu Bahasa Assembly (Assembly Language)?", "id": "Bahasa Pemrograman Level Rendah (Mnemonik)." },
  { "en": "Apa Itu Kompilator (Compiler)?", "id": "Penerjemah Bahasa Tinggi Ke Mesin." },
  { "en": "Apa Itu Interpreter (Interpreter)?", "id": "Penerjemah Bahasa Baris Per Baris." },
  { "en": "Apa Itu Assembler (Perakit)?", "id": "Penerjemah Bahasa Assembly Ke Mesin." },
  { "en": "Apa Itu Debugger (Debugger)?", "id": "Alat Bantu Mencari Kesalahan (Bug) Program." },
  { "en": "Apa Itu Diagram Waktu (Timing Diagram)?", "id": "Grafik Sinyal Digital Terhadap Waktu." },
  { "en": "Apa Itu Mesin Keadaan (State Machine)?", "id": "Model Komputasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Diagram Keadaan (State Diagram)?", "id": "Representasi Grafis Mesin Keadaan." },
  { "en": "Apa Itu Keadaan (State) Dalam Mesin Keadaan?", "id": "Kondisi Operasi Sistem Saat Itu." },
  { "en": "Apa Itu Transisi (Transition) Mesin Keadaan?", "id": "Perpindahan Antar Keadaan (State)." },
  { "en": "Apa Itu Mesin Moore (Moore Machine)?", "id": "Output Mesin Keadaan Tergantung Keadaan (State) Saja." },
  { "en": "Apa Itu Mesin Mealy (Mealy Machine)?", "id": "Output Mesin Keadaan Tergantung Keadaan, Input." },
  { "en": "Apa Itu Sirkuit (Circuit) Rangkaian Terbuka?", "id": "Jalur Listrik Tidak Terhubung Lengkap." },
  { "en": "Apa Arus (Current) Pada Sirkuit Terbuka?", "id": "Arus Mengalir Bernilai Nol." },
  { "en": "Apa Itu Sirkuit (Circuit) Hubungan Singkat?", "id": "Jalur Listrik Resistansi Sangat Rendah." },
  { "en": "Apa Tegangan (Voltage) Pada Hubungan Singkat Ideal?", "id": "Tegangan Bernilai Nol Volt." },
  { "en": "Apa Bahaya (Danger) Hubungan Singkat?", "id": "Arus Sangat Tinggi, Kebakaran." },
  { "en": "Apa Itu Efisiensi (Efficiency) Sistem?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Mengapa Efisiensi (Efficiency) Tidak Bisa 100 Persen?", "id": "Selalu Ada Energi Hilang (Panas)." },
  { "en": "Apa Itu Rugi (Loss) Tembaga?", "id": "Rugi Daya Akibat Resistansi Kawat (I²R)." },
  { "en": "Apa Itu Rugi (Loss) Inti?", "id": "Rugi Histeresis Dan Arus Eddy." },
  { "en": "Apa Itu Arus Eddy (Eddy Current)?", "id": "Arus Berputar Di Inti Konduktif." },
  { "en": "Bagaimana Mengurangi Rugi Arus Eddy (Eddy Current)?", "id": "Menggunakan Inti Berlaminasi (Berlapis)." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Keterlambatan Magnetisasi Bahan Inti." },
  { "en": "Bahan Inti (Core) Apa Rugi Histeresis Rendah?", "id": "Bahan Magnet Lunak (Soft Magnetic)." },
  { "en": "Apa Itu Fluks (Flux) Magnetik?", "id": "Ukuran Total Medan Magnet." },
  { "en": "Apa Satuan Fluks (Flux) Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Kerapatan (Density) Fluks Magnet?", "id": "Fluks Magnet Per Satuan Luas." },
  { "en": "Apa Satuan Kerapatan (Density) Fluks Magnet?", "id": "Tesla (T)." },
  { "en": "Apa Itu Gaya Gerak Magnet (MMF)?", "id": "Penyebab Timbulnya Fluks Magnet." },
  { "en": "Apa Satuan Gaya Gerak Magnet (MMF)?", "id": "Ampere-Lilit (Ampere-Turns)." },
  { "en": "Apa Itu Reluktansi (Reluctance) Magnetik?", "id": "Hambatan Terhadap Aliran Fluks Magnet." },
  { "en": "Apa Hukum Ohm (Ohm's Law) Rangkaian Magnetik?", "id": "Fluks = MMF / Reluktansi." },
  { "en": "Apa Itu Permeabilitas (Permeability) Magnetik?", "id": "Kemampuan Bahan Hantarkan Fluks Magnet." },
  { "en": "Apa Itu Bahan (Material) Feromagnetik?", "id": "Bahan Permeabilitas Sangat Tinggi (Besi)." },
  { "en": "Apa Itu Saturasi (Saturation) Magnetik?", "id": "Kondisi Inti Jenuh Medan Magnet." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik) Induksi?", "id": "Tegangan Timbul Akibat Perubahan Fluks." },
  { "en": "Apa Bunyi Hukum Faraday (Faraday's Law) Induksi?", "id": "GGL Proporsional Laju Perubahan Fluks." },
  { "en": "Apa Bunyi Hukum Lenz (Lenz's Law)?", "id": "Arah Arus Induksi Melawan Perubahan." },
  { "en": "Apa Itu Induktansi (Inductance) Diri?", "id": "GGL Induksi Diri Sendiri (Self-Inductance)." },
  { "en": "Apa Itu Induktansi (Inductance) Mutual?", "id": "GGL Induksi Akibat Kumparan Lain." },
  { "en": "Aplikasi Utama Induktansi (Inductance) Mutual?", "id": "Transformator (Transformer)." },
  { "en": "Apa Itu Energi (Energy) Tersimpan Induktor?", "id": "Energi Medan Magnet (½LI²)." },
  { "en": "Apa Itu Energi (Energy) Tersimpan Kapasitor?", "id": "Energi Medan Listrik (½CV²)." },
  { "en": "Apa Itu Resistor (Resistor) Film Karbon?", "id": "Resistor Umum Lapisan Karbon." },
  { "en": "Apa Itu Resistor (Resistor) Film Logam?", "id": "Resistor Presisi Lapisan Logam." },
  { "en": "Apa Itu Resistor (Resistor) Kawat Lilit?", "id": "Resistor Lilitan Kawat (Daya Tinggi)." },
  { "en": "Apa Itu Resistor (Resistor) SMD?", "id": "Surface Mount Device (Pemasangan Permukaan)." },
  { "en": "Apa Itu Kode (Code) Resistor SMD?", "id": "Kode Angka (Contoh: 103 = 10kΩ)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Keramik?", "id": "Kapasitor Non-Polar Frekuensi Tinggi." },
  { "en": "Apa Itu Kapasitor (Capacitor) Elektrolit (Elco)?", "id": "Kapasitor Polar Nilai Tinggi." },
  { "en": "Apa Itu Kapasitor (Capacitor) Tantalum?", "id": "Kapasitor Elektrolit Padat (Lebih Stabil)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Film?", "id": "Kapasitor Non-Polar Dielektrik Film Plastik." },
  { "en": "Apa Itu Kapasitor (Capacitor) Variabel (Varco)?", "id": "Kapasitansi Dapat Diatur Mekanis." },
  { "en": "Aplikasi Kapasitor (Capacitor) Variabel (Varco)?", "id": "Penala (Tuner) Radio." },
  { "en": "Apa Itu Dioda (Diode) Ideal?", "id": "Saklar Sempurna Satu Arah." },
  { "en": "Apa Itu Tegangan (Voltage) Lutut Dioda?", "id": "Tegangan Minimum Mulai Konduksi." },
  { "en": "Apa Itu Arus (Current) Bocor Dioda?", "id": "Arus Kecil Saat Bias Mundur." },
  { "en": "Apa Itu Tegangan (Voltage) Puncak Balik (PIV)?", "id": "Tegangan Mundur Maksimum Dioda." },
  { "en": "Apa Itu Dioda (Diode) Penyearah?", "id": "Mengubah Arus AC Menjadi DC." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Menstabilkan Tegangan Pada Breakdown." },
  { "en": "Apa Itu Dioda (Diode) Schottky?", "id": "Dioda Cepat, Jatuh Tegangan Rendah." },
  { "en": "Apa Itu Dioda (Diode) Varactor (Varicap)?", "id": "Kapasitansi Dioda Terkendali Tegangan." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Bipolar Junction Transistor." },
  { "en": "Bagaimana Transistor BJT (Bipolar Junction Transistor) Dikontrol?", "id": "Oleh Arus Basis (Current Controlled)." },
  { "en": "Apa Itu Penguatan (Gain) Arus BJT?", "id": "Beta (β) Atau hFE." },
  { "en": "Apa Itu Daerah (Region) Cutoff BJT?", "id": "Transistor Mati (Saklar Terbuka)." },
  { "en": "Apa Itu Daerah (Region) Saturasi BJT?", "id": "Transistor Jenuh (Saklar Tertutup)." },
  { "en": "Apa Itu Daerah (Region) Aktif BJT?", "id": "Transistor Sebagai Penguat Linear." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Field Effect Transistor." },
  { "en": "Bagaimana Transistor FET (Field Effect Transistor) Dikontrol?", "id": "Oleh Tegangan Gate (Voltage Controlled)." },
  { "en": "Apa Keunggulan Utama FET (Field Effect Transistor)?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu JFET (Junction FET)?", "id": "FET (Field Effect Transistor) Dengan Sambungan PN Gate." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET (Field Effect Transistor) Dengan Gerbang Terisolasi Oksida." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Enhancement Mode?", "id": "Perlu Tegangan Gate Untuk Konduksi." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Depletion Mode?", "id": "Konduksi Tanpa Tegangan Gate." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Logika Kombinasi NMOS Dan PMOS." },
  { "en": "Keuntungan Utama Logika CMOS (Complementary MOS)?", "id": "Konsumsi Daya Statis Rendah." },
  { "en": "Apa Itu Thyristor (Thyristor)?", "id": "Keluarga Saklar Semikonduktor Empat Lapis." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Saklar DC Terkendali Gerbang." },
  { "en": "Bagaimana Mematikan (Turn Off) SCR?", "id": "Arus Anoda Dibawah Arus Holding." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Saklar AC Dua Arah Terkendali." },
  { "en": "Apa Itu DIAC (Diode for AC)?", "id": "Pemicu (Trigger) Untuk TRIAC." },
  { "en": "Apa Itu UJT (Unijunction Transistor)?", "id": "Transistor Pemicu Osilator Relaksasi." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Gabungan MOSFET (Input) BJT (Output)." },
  { "en": "Keunggulan IGBT (Insulated Gate Bipolar Transistor)?", "id": "Handle Daya Besar, Switching Cepat." },
  { "en": "Apa Itu Penguat (Amplifier) Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Karakteristik (Characteristic) Op-Amp Ideal?", "id": "Gain Infinite, Input Z Infinite, Output Z Nol." },
  { "en": "Apa Itu Umpan Balik (Feedback) Negatif?", "id": "Menstabilkan Gain Op-Amp." },
  { "en": "Apa Itu Penguat (Amplifier) Inverting?", "id": "Output Berbeda Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat (Amplifier) Non-Inverting?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut (Follower) Tegangan?", "id": "Buffer (Buffer) Gain Satu." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter Menggunakan Op-Amp (Bisa Gain)." },
  { "en": "Apa Itu Osilator (Oscillator) Jembatan Wien?", "id": "Penghasil Gelombang Sinus Murni." },
  { "en": "Apa Itu Resolusi (Resolution) ADC/DAC?", "id": "Jumlah Bit Representasi Digital." },
  { "en": "Apa Itu Laju (Rate) Sampling?", "id": "Seberapa Cepat Sinyal Diukur (ADC)." },
  { "en": "Apa Teorema (Theorem) Sampling Nyquist?", "id": "Fs Harus Minimal 2x Fmax." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Akibat Laju Sampling Kurang." },
  { "en": "Apa Itu Filter (Filter) Anti-Aliasing?", "id": "Filter Low-Pass Sebelum ADC." },
  { "en": "Apa Itu Multiplexer (Multiplexer) Analog?", "id": "Saklar Elektronik Pilih Sinyal Analog." },
  { "en": "Apa Itu Sample And Hold (S&H)?", "id": "Rangkaian Tahan Tegangan Input ADC." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Sirkuit Logika Digital." },
  { "en": "Apa Itu Aljabar (Algebra) Boolean?", "id": "Matematika Untuk Logika Biner." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Memilih Satu Dari Banyak Input Ke Output Tunggal." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Mengarahkan Input Tunggal Ke Salah Satu Output." },
  { "en": "Apa Itu Decoder (Decoder)?", "id": "Mengaktifkan Satu Output Berdasarkan Kode Biner." },
  { "en": "Apa Itu Half Adder (Penjumlah Setengah)?", "id": "Menjumlahkan Dua Bit Tunggal." },
  { "en": "Apa Itu Full Adder (Penjumlah Penuh)?", "id": "Menjumlahkan Tiga Bit (Termasuk Carry In)." },
  { "en": "Apa Itu Ripple Carry Adder (Penjumlah Bawa Riak)?", "id": "Adder Multi-Bit Carry Berpropagasi Berurutan." },
  { "en": "Apa Itu Lookahead Carry Adder (Penjumlah Bawa Lihat Depan)?", "id": "Adder Multi-Bit Propagasi Carry Lebih Cepat." },
  { "en": "Apa Itu Arithmetic Logic Unit (ALU)?", "id": "Unit Aritmetika Dan Logika Dalam CPU." },
  { "en": "Apa Kepanjangan ALU (Arithmetic Logic Unit)?", "id": "Unit Aritmetika Logika." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Perangkat Penyimpanan Informasi Digital." },
  { "en": "Apa Itu Volatile Memory (Memori Volatil)?", "id": "Data Hilang Saat Daya Mati." },
  { "en": "Apa Itu Non-Volatile Memory (Memori Non-Volatil)?", "id": "Data Tetap Ada Tanpa Daya." },
  { "en": "Contoh Memori (Memory) Volatil?", "id": "RAM (Random Access Memory) (SRAM, DRAM)." },
  { "en": "Contoh Memori (Memory) Non-Volatil?", "id": "ROM (Read Only Memory), Flash, Hard Disk." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Baca Tulis Akses Acak." },
  { "en": "Apa Itu SRAM (Static RAM)?", "id": "RAM (Random Access Memory) Cepat, Berbasis Flip-Flop." },
  { "en": "Apa Itu DRAM (Dynamic RAM)?", "id": "RAM (Random Access Memory) Kepadatan Tinggi, Perlu Refresh." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Bisa Dibaca." },
  { "en": "Apa Itu PROM (Programmable ROM)?", "id": "ROM (Read Only Memory) Dapat Diprogram Sekali." },
  { "en": "Apa Itu EPROM (Erasable PROM)?", "id": "PROM (Programmable ROM) Dapat Dihapus Sinar UV." },
  { "en": "Apa Itu EEPROM (Electrically Erasable PROM)?", "id": "EPROM (Erasable PROM) Dapat Dihapus Secara Elektrik." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Jenis EEPROM (Electrically Erasable PROM) Hapus Blok." },
  { "en": "Apa Itu SSD (Solid State Drive)?", "id": "Penyimpanan Non-Volatil Berbasis Memori Flash." },
  { "en": "Apa Itu Hard Disk Drive (HDD)?", "id": "Penyimpanan Non-Volatil Berbasis Piringan Magnetik." },
  { "en": "Apa Itu Cache Memory (Memori Cache)?", "id": "Memori Kecil Cepat Antara CPU, RAM." },
  { "en": "Apa Tujuan Cache Memory (Memori Cache)?", "id": "Mempercepat Akses Data Yang Sering Digunakan." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Dalam Chip." },
  { "en": "Apa Itu Arsitektur Von Neumann (Von Neumann Architecture)?", "id": "Data, Instruksi Disimpan Memori Sama." },
  { "en": "Apa Itu Arsitektur Harvard (Harvard Architecture)?", "id": "Memori Terpisah Untuk Data, Instruksi." },
  { "en": "Apa Itu Instruction Set Architecture (ISA)?", "id": "Set Instruksi Dipahami Mikroprosesor." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computer)?", "id": "Arsitektur Set Instruksi Sederhana." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computer)?", "id": "Arsitektur Set Instruksi Kompleks." },
  { "en": "Apa Itu Pipelining (Pipelining) Instruksi?", "id": "Eksekusi Instruksi Bertahap Tumpang Tindih." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Chip (CPU, RAM, ROM, I/O)." },
  { "en": "Apa Beda Mikroprosesor, Mikrokontroler?", "id": "Mikrokontroler Lebih Terintegrasi (All-in-One)." },
  { "en": "Aplikasi Mikrokontroler (Microcontroller)?", "id": "Sistem Tertanam (Embedded System)." },
  { "en": "Apa Itu GPIO (General Purpose Input Output)?", "id": "Pin Mikrokontroler Konfigurasi Input/Output." },
  { "en": "Apa Itu Timer/Counter (Pewaktu/Pencacah) Mikrokontroler?", "id": "Modul Pewaktuan, Pencacahan Internal." },
  { "en": "Apa Itu Interrupt (Interupsi)?", "id": "Sinyal Eksternal/Internal Ganggu Eksekusi Normal." },
  { "en": "Apa Fungsi Interrupt (Interupsi)?", "id": "Respon Cepat Terhadap Kejadian Asinkron." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Timer Pencegah Sistem Hang (Reset Otomatis)." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication) UART?", "id": "Komunikasi Asinkron Dua Kawat (TX, RX)." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication) SPI?", "id": "Komunikasi Sinkron Empat Kawat (MOSI, MISO, SCLK, SS)." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication) I2C?", "id": "Komunikasi Sinkron Dua Kawat (SDA, SCL)." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Dedikasi Fungsi Tertentu." },
  { "en": "Apa Itu Real-Time System (Sistem Waktu Nyata)?", "id": "Sistem Respon Terhadap Event Batas Waktu." },
  { "en": "Apa Itu Hard Real-Time (Waktu Nyata Keras)?", "id": "Kegagalan Penuhi Batas Waktu Fatal." },
  { "en": "Apa Itu Soft Real-Time (Waktu Nyata Lunak)?", "id": "Kegagalan Batas Waktu Kurangi Kualitas." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "Sistem Operasi Sistem Waktu Nyata." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "Chip Logika Dapat Diprogram Ulang." },
  { "en": "Apa Itu Bahasa Deskripsi Perangkat Keras (HDL)?", "id": "Hardware Description Language (HDL)." },
  { "en": "Contoh Bahasa HDL (Hardware Description Language)?", "id": "VHDL (VHSIC HDL), Verilog." },
  { "en": "Apa Itu Sintesis (Synthesis) HDL?", "id": "Proses Konversi Kode HDL Ke Rangkaian Logika." },
  { "en": "Apa Itu Place and Route (Penempatan Dan Routing)?", "id": "Proses Implementasi Fisik Desain FPGA/ASIC." },
  { "en": "Apa Itu ASIC (Application Specific Integrated Circuit)?", "id": "Chip Dirancang Khusus Aplikasi Tertentu." },
  { "en": "Apa Keuntungan ASIC (Application Specific Integrated Circuit) Dibanding FPGA?", "id": "Kinerja Lebih Tinggi, Daya Lebih Rendah." },
  { "en": "Apa Kerugian ASIC (Application Specific Integrated Circuit) Dibanding FPGA?", "id": "Biaya Desain Tinggi, Tidak Fleksibel." },
  { "en": "Apa Itu SoC (System on Chip)?", "id": "Integrasi Komponen Sistem Lengkap Chip Tunggal." },
  { "en": "Apa Itu IP (Intellectual Property) Core?", "id": "Blok Desain Siap Pakai Untuk SoC." },
  { "en": "Apa Itu EDA (Electronic Design Automation)?", "id": "Perangkat Lunak Bantu Desain Elektronik." },
  { "en": "Apa Itu Verifikasi (Verification) Desain?", "id": "Memastikan Desain Berfungsi Benar." },
  { "en": "Apa Itu Simulasi (Simulation) Desain?", "id": "Menjalankan Model Desain Di Komputer." },
  { "en": "Apa Itu Emulasi (Emulation) Perangkat Keras?", "id": "Menjalankan Desain Di Hardware Khusus (FPGA)." },
  { "en": "Apa Itu Design for Testability (DFT)?", "id": "Teknik Desain Memudahkan Pengujian Chip." },
  { "en": "Contoh Teknik DFT (Design for Testability)?", "id": "Scan Chain, BIST (Built-In Self-Test)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Standar Antarmuka Pengujian (Boundary Scan)." },
  { "en": "Apa Itu Pengukuran (Measurement) Dan Instrumentasi?", "id": "Ilmu Teknik Pengukuran Besaran Fisik." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Pengukuran Yang Sama." },
  { "en": "Apa Itu Resolusi (Resolution)?", "id": "Perubahan Terkecil Dapat Diukur." },
  { "en": "Apa Itu Kesalahan (Error) Pengukuran?", "id": "Selisih Antara Nilai Ukur, Nilai Benar." },
  { "en": "Apa Itu Kesalahan Sistematik (Systematic Error)?", "id": "Kesalahan Konsisten (Bias)." },
  { "en": "Apa Itu Kesalahan Acak (Random Error)?", "id": "Kesalahan Fluktuatif Tak Terduga." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Membandingkan Alat Ukur Dengan Standar." },
  { "en": "Apa Itu Ketidakpastian (Uncertainty) Pengukuran?", "id": "Rentang Keraguan Nilai Hasil Ukur." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Konversi Besaran Fisik Ke Sinyal." },
  { "en": "Apa Itu Pengkondisian (Conditioning) Sinyal?", "id": "Memproses Sinyal Sensor Agar Sesuai." },
  { "en": "Apa Itu Akuisisi (Acquisition) Data (DAQ)?", "id": "Pengumpulan Data Sinyal Ke Komputer." },
  { "en": "Apa Itu Multimeter Digital (DMM)?", "id": "Alat Ukur Listrik V, A, Ω Digital." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Generator Sinyal (Signal Generator)?", "id": "Penghasil Sinyal Uji Elektronik." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Menampilkan Komponen Frekuensi Sinyal." },
  { "en": "Apa Itu Penganalisis Logika (Logic Analyzer)?", "id": "Menganalisis Sinyal Digital Multi Kanal." },
  { "en": "Apa Itu LCR Meter (LCR Meter)?", "id": "Pengukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Apa Itu Antarmuka (Interface) GPIB/IEEE-488?", "id": "Standar Komunikasi Instrumen Pengukuran." },
  { "en": "Apa Itu LabVIEW (Laboratory Virtual Instrument Engineering Workbench)?", "id": "Perangkat Lunak Pengembangan Grafis Instrumentasi." },
  { "en": "Apa Itu Instrumentasi (Instrumentation) Virtual?", "id": "Penggunaan Komputer Pengganti Instrumen Fisik." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Kombinasi Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Persamaan (Equations) Maxwell?", "id": "Hukum Dasar Elektromagnetisme Klasik." },
  { "en": "Apa Itu Gelombang (Wave) Elektromagnetik?", "id": "Perambatan Gangguan Medan Elektromagnetik." },
  { "en": "Apa Itu Kecepatan (Speed) Cahaya (c)?", "id": "Kecepatan Gelombang EM Di Vakum." },
  { "en": "Apa Itu Spektrum (Spectrum) Elektromagnetik?", "id": "Rentang Frekuensi Gelombang EM." },
  { "en": "Sebutkan Urutan Spektrum (Spectrum) EM?", "id": "Radio, Mikro, Inframerah, Tampak, UV, X, Gamma." },
  { "en": "Apa Itu Polarisasi (Polarization) Gelombang?", "id": "Orientasi Osilasi Medan Listrik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Konverter Antara Gelombang Terpandu, Bebas." },
  { "en": "Apa Itu Pola (Pattern) Radiasi Antena?", "id": "Distribusi Spasial Daya Dipancarkan." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan Dibanding Isotropik." },
  { "en": "Apa Itu Saluran (Line) Transmisi?", "id": "Struktur Pemandu Perambatan Gelombang EM." },
  { "en": "Contoh Saluran (Line) Transmisi?", "id": "Kabel Koaksial, Mikrostrip, Pemandu Gelombang." },
  { "en": "Apa Itu Impedansi (Impedance) Karakteristik?", "id": "Impedansi Khas Saluran Transmisi." },
  { "en": "Apa Itu Pencocokan (Matching) Impedansi?", "id": "Membuat Impedansi Beban Sama Saluran." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Alat Grafis Analisis Saluran Transmisi." },
  { "en": "Apa Itu Radar (Radio Detection and Ranging)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Apa Itu Komunikasi (Communication) Optik?", "id": "Transmisi Informasi Menggunakan Cahaya." },
  { "en": "Media Apa Komunikasi (Communication) Optik Utama?", "id": "Serat Optik (Fiber Optic)." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Pemisahan Informasi Dari Sinyal Pembawa." },
  { "en": "Jenis Modulasi (Modulation) Analog Dasar?", "id": "AM (Amplitude Modulation), FM (Frequency Modulation), PM (Phase Modulation)." },
  { "en": "Jenis Modulasi (Modulation) Digital Dasar?", "id": "ASK (Amplitude Shift Keying), FSK (Frequency Shift Keying), PSK (Phase Shift Keying)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Sinyal?", "id": "Rentang Frekuensi Ditempati Sinyal." },
  { "en": "Apa Itu Kapasitas (Capacity) Kanal Shannon?", "id": "Batas Laju Data Maksimum Teoretis." },
  { "en": "Apa Itu SNR (Signal-to-Noise Ratio)?", "id": "Rasio Daya Sinyal Terhadap Derau." },
  { "en": "Apa Itu BER (Bit Error Rate)?", "id": "Rasio Jumlah Bit Error Terhadap Total." },
  { "en": "Apa Itu Pengkodean (Coding) Kanal?", "id": "Menambah Redundansi Deteksi/Koreksi Error." },
  { "en": "Apa Itu Interleaving (Interleaving)?", "id": "Mengacak Urutan Bit Atasi Burst Error." },
  { "en": "Apa Itu Multipath Fading (Redaman Multipath)?", "id": "Fluktuasi Sinyal Akibat Lintasan Ganda." },
  { "en": "Apa Itu Keanekaragaman (Diversity)?", "id": "Teknik Mengurangi Efek Fading." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Sistem Multi Antena Pemancar Penerima." },
  { "en": "Apa Itu OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Modulasi Multi Pembawa Orthogonal." },
  { "en": "Apa Itu Akses (Access) Ganda?", "id": "Berbagi Kanal Banyak Pengguna." },
  { "en": "Jenis Akses (Access) Ganda?", "id": "FDMA, TDMA, CDMA, OFDMA." },
  { "en": "Apa Itu Jaringan (Network) Seluler?", "id": "Jaringan Nirkabel Berbasis Sel." },
  { "en": "Apa Itu Komunikasi (Communication) Satelit?", "id": "Komunikasi Menggunakan Satelit Orbit." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Navigasi Satelit Global." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Komunikasi Antar Perangkat." },
  { "en": "Apa Itu Model (Model) OSI?", "id": "Model Referensi Tujuh Lapisan Jaringan." },
  { "en": "Apa Itu Model (Model) TCP/IP?", "id": "Model Arsitektur Jaringan Internet." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Itu Alamat (Address) MAC?", "id": "Alamat Fisik Perangkat Keras." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Teknologi Jaringan LAN Kabel." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan LAN Nirkabel." },
  { "en": "Apa Itu Router (Router)?", "id": "Penghubung Antar Jaringan Berbeda." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Sistem Keamanan Penyaring Lalu Lintas." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Output Pengaruhi Input Kontrol." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Reproduktifitas Hasil Pengukuran." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik V, A, Ω." },
  { "en": "Apa Itu Gelombang (Wave) Elektromagnetik?", "id": "Perambatan Energi Medan EM." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Sifat Antara Konduktor, Isolator." },
  { "en": "Apa Itu Pemrosesan (Processing) Sinyal?", "id": "Analisis, Modifikasi Sinyal." },
  { "en": "Apa Itu Transformasi (Transform) Fourier?", "id": "Analisis Frekuensi Sinyal." },
  { "en": "Apa Itu Kecerdasan (Intelligence) Buatan (AI)?", "id": "Mesin Tiru Kecerdasan Manusia." },
  { "en": "Apa Itu Pembelajaran (Learning) Mesin (ML)?", "id": "AI (Artificial Intelligence) Belajar Dari Data." },
  { "en": "Apa Itu Internet (Internet) of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung." },
  { "en": "Apa Itu Energi (Energy) Terbarukan?", "id": "Sumber Energi Alam Berkelanjutan." },
  { "en": "Apa Itu Sel (Cell) Surya?", "id": "Pengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Turbin (Turbine) Angin?", "id": "Pengubah Energi Angin Listrik." },
  { "en": "Apa Itu Penyimpanan (Storage) Energi?", "id": "Menyimpan Energi Listrik (Baterai)." },
  { "en": "Apa Itu Kendaraan (Vehicle) Listrik?", "id": "Kendaraan Penggerak Motor Listrik." },
  { "en": "Apa Itu Pencitraan (Imaging) Medis?", "id": "Visualisasi Dalam Tubuh (MRI, CT)." },
  { "en": "Apa Itu Elektronika (Electronics) Biomedis?", "id": "Elektronika Aplikasi Medis." },
  { "en": "Apa Itu Sensor (Sensor) Biomedis?", "id": "Sensor Ukur Sinyal Tubuh (ECG)." },
  { "en": "Apa Itu Sinyal (Signal) ECG?", "id": "Rekaman Aktivitas Listrik Jantung." },
  { "en": "Apa Itu Sinyal (Signal) EEG?", "id": "Rekaman Aktivitas Listrik Otak." },
  { "en": "Apa Itu Sinyal (Signal) EMG?", "id": "Rekaman Aktivitas Listrik Otot." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Perangkat Stimulasi Ritme Jantung." },
  { "en": "Apa Itu Defibrilator (Defibrillator)?", "id": "Alat Kejut Listrik Atasi Aritmia." },
  { "en": "Apa Itu Teknik (Engineering) Material?", "id": "Studi Sifat, Aplikasi Material." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Molekul Rantai Panjang (Plastik)." },
  { "en": "Apa Itu Komposit (Composite)?", "id": "Gabungan Dua Material Berbeda." },
  { "en": "Apa Itu Kekuatan (Strength) Tarik?", "id": "Kemampuan Tahan Gaya Tarik." },
  { "en": "Apa Itu Keuletan (Ductility)?", "id": "Kemampuan Deformasi Plastis." },
  { "en": "Apa Itu Kerapuhan (Brittleness)?", "id": "Sifat Mudah Patah Tanpa Deformasi." },
  { "en": "Apa Itu Korosi (Corrosion)?", "id": "Kerusakan Material Reaksi Kimia." },
  { "en": "Apa Itu Mekatronika (Mechatronics)?", "id": "Integrasi Mekanik, Elektronik, Kontrol." },
  { "en": "Apa Itu Perpindahan (Heat Transfer) Panas?", "id": "Pergerakan Energi Termal." },
  { "en": "Mode Perpindahan (Heat Transfer) Panas?", "id": "Konduksi, Konveksi, Radiasi." },
  { "en": "Apa Itu Mekanika (Mechanics) Fluida?", "id": "Studi Perilaku Fluida." }


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
