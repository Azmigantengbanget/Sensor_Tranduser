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


    { "en": "Apa Perangkat Pengubah Besaran Fisik Ke Listrik?", "id": "Sensor." },
    { "en": "Apa Perangkat Pengubah Satu Bentuk Energi Ke Lainnya?", "id": "Transduser (Transducer)." },
    { "en": "Apakah Sensor Termasuk Transduser?", "id": "Ya, Sensor Adalah Jenis Transduser." },
    { "en": "Apa Besaran Fisik Yang Diukur?", "id": "Measurand (Besaran Ukur)." },
    { "en": "Apa Contoh Besaran Fisik?", "id": "Suhu, Tekanan, Cahaya, Kecepatan." },
    { "en": "Apa Output Dari Sensor?", "id": "Sinyal Listrik (Tegangan/Arus)." },
    { "en": "Apa Sensor Yang Membutuhkan Daya Eksternal?", "id": "Sensor Aktif." },
    { "en": "Apa Sensor Yang Tidak Butuh Daya Eksternal?", "id": "Sensor Pasif." },
    { "en": "Apa Contoh Sensor Pasif?", "id": "Termokopel (Thermocouple)." },
    { "en": "Apa Contoh Sensor Aktif?", "id": "Strain Gauge." },
    { "en": "Apa Sensor Berdasarkan Outputnya?", "id": "Sensor Analog Dan Digital." },
    { "en": "Apa Output Sensor Analog?", "id": "Sinyal Kontinu." },
    { "en": "Apa Output Sensor Digital?", "id": "Sinyal Diskret (Pulsa/Biner)." },
    { "en": "Apa Kedekatan Hasil Ukur Dengan Nilai Sebenarnya?", "id": "Akurasi (Accuracy)." },
    { "en": "Apa Kemampuan Memberi Hasil Sama Berulang Kali?", "id": "Presisi (Precision)." },
    { "en": "Apa Perubahan Terkecil Yang Dapat Dideteksi?", "id": "Resolusi (Resolution)." },
    { "en": "Apa Rasio Perubahan Output Per Perubahan Input?", "id": "Sensitivitas (Sensitivity)." },
    { "en": "Apa Rentang Input Yang Dapat Diukur Sensor?", "id": "Rentang Ukur (Range)." },
    { "en": "Apa Waktu Respon Sensor?", "id": "Waktu Untuk Merespon Perubahan." },
    { "en": "Apa Perubahan Lambat Output Seiring Waktu?", "id": "Drift." },
    { "en": "Apa Perbedaan Output Saat Pengukuran Naik Dan Turun?", "id": "Histeresis (Hysteresis)." },
    { "en": "Apa Seberapa Lurus Hubungan Input-Output?", "id": "Linearitas (Linearity)." },
    { "en": "Apa Sensor Pengukur Suhu?", "id": "Sensor Suhu." },
    { "en": "Apa Sensor Suhu Berbasis Efek Seebeck?", "id": "Termokopel (Thermocouple)." },
    { "en": "Apa Sambungan Pengukur Pada Termokopel?", "id": "Hot Junction (Sambungan Panas)." },
    { "en": "Apa Sambungan Referensi Pada Termokopel?", "id": "Cold Junction (Sambungan Dingin)." },
    { "en": "Apa Sensor Suhu Berbasis Resistansi Logam?", "id": "RTD (Resistance Temperature Detector)." },
    { "en": "Logam Apa Yang Umum Digunakan Di RTD?", "id": "Platina (Pt100, Pt1000)." },
    { "en": "Apa Sensor Suhu Berbasis Semikonduktor?", "id": "Termistor (Thermistor)." },
    { "en": "Apa Termistor Dengan Resistansi Turun Saat Suhu Naik?", "id": "NTC (Negative Temperature Coefficient)." },
    { "en": "Apa Termistor Dengan Resistansi Naik Saat Suhu Naik?", "id": "PTC (Positive Temperature Coefficient)." },
    { "en": "Sensor Suhu Mana Yang Paling Sensitif?", "id": "Termistor (Thermistor)." },
    { "en": "Sensor Suhu Mana Yang Paling Linear?", "id": "RTD (Resistance Temperature Detector)." },
    { "en": "Sensor Suhu Mana Yang Rentangnya Paling Lebar?", "id": "Termokopel (Thermocouple)." },
    { "en": "Apa Sensor Suhu Dalam Bentuk IC (Integrated Circuit)?", "id": "Sensor Suhu IC (LM35, DS18B20)." },
    { "en": "Apa Sensor Pengukur Tekanan?", "id": "Sensor Tekanan." },
    { "en": "Apa Prinsip Kerja Strain Gauge?", "id": "Perubahan Resistansi Akibat Regangan." },
    { "en": "Apa Rangkaian Untuk Mengukur Perubahan Resistansi Kecil?", "id": "Jembatan Wheatstone." },
    { "en": "Apa Sensor Tekanan Berbasis Kapasitansi?", "id": "Sensor Tekanan Kapasitif." },
    { "en": "Apa Sensor Tekanan Berbasis Efek Piezoelektrik?", "id": "Sensor Tekanan Piezoelektrik." },
    { "en": "Apa Sensor Pengukur Aliran Fluida?", "id": "Flowmeter (Sensor Aliran)." },
    { "en": "Apa Flowmeter Berbasis Turbin?", "id": "Turbine Flowmeter." },
    { "en": "Apa Flowmeter Berbasis Ultrasonik?", "id": "Ultrasonic Flowmeter." },
    { "en": "Apa Flowmeter Berbasis Elektromagnetik?", "id": "Electromagnetic Flowmeter." },
    { "en": "Apa Sensor Pengukur Ketinggian Cairan?", "id": "Sensor Level." },
    { "en": "Apa Sensor Level Berbasis Pelampung?", "id": "Float Switch." },
    { "en": "Apa Sensor Level Berbasis Ultrasonik?", "id": "Sensor Level Ultrasonik." },
    { "en": "Apa Sensor Level Berbasis Tekanan?", "id": "Sensor Level Tekanan Hidrostatik." },
    { "en": "Apa Sensor Pengukur Cahaya?", "id": "Sensor Cahaya." },
    { "en": "Apa Resistor Yang Nilainya Tergantung Cahaya?", "id": "LDR (Light Dependent Resistor)." },
    { "en": "Apa Dioda Yang Menghasilkan Arus Saat Kena Cahaya?", "id": "Fotodioda (Photodiode)." },
    { "en": "Apa Transistor Yang Dikontrol Oleh Cahaya?", "id": "Fototransistor (Phototransistor)." },
    { "en": "Apa Perangkat Yang Mengubah Energi Surya Menjadi Listrik?", "id": "Sel Surya (Solar Cell)." },
    { "en": "Apa Sensor Pengukur Posisi Atau Perpindahan?", "id": "Sensor Posisi." },
    { "en": "Apa Resistor Variabel Pengukur Posisi Sudut?", "id": "Potensiometer." },
    { "en": "Apa Sensor Posisi Berbasis Efek Hall?", "id": "Hall Effect Sensor." },
    { "en": "Apa Sensor Posisi Optik?", "id": "Encoder Optik." },
    { "en": "Apa Encoder Yang Memberi Posisi Absolut?", "id": "Encoder Absolut." },
    { "en": "Apa Encoder Yang Memberi Pulsa Gerakan?", "id": "Encoder Inkremental." },
    { "en": "Apa Sensor Posisi Non-kontak?", "id": "LVDT (Linear Variable Differential Transformer)." },
    { "en": "Apa Sensor Pengukur Kecepatan?", "id": "Sensor Kecepatan." },
    { "en": "Apa Generator Kecil Pengukur Kecepatan Putaran?", "id": "Tachometer Generator." },
    { "en": "Bagaimana Encoder Digunakan Untuk Mengukur Kecepatan?", "id": "Menghitung Pulsa Per Satuan Waktu." },
    { "en": "Apa Sensor Pengukur Percepatan Dan Getaran?", "id": "Akselerometer (Accelerometer)." },
    { "en": "Apa Akselerometer Berbasis Efek Piezoelektrik?", "id": "Akselerometer Piezoelektrik." },
    { "en": "Apa Akselerometer Berbasis Kapasitansi?", "id": "Akselerometer Kapasitif (MEMS)." },
    { "en": "Apa Akselerometer Berbasis MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sensor Percepatan Dalam Chip Kecil." },
    { "en": "Apa Sensor Pengukur Orientasi?", "id": "Giroskop (Gyroscope)." },
    { "en": "Apa Giroskop Berbasis MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sensor Rotasi Dalam Chip Kecil." },
    { "en": "Apa Gabungan Akselerometer Dan Giroskop?", "id": "IMU (Inertial Measurement Unit)." },
    { "en": "Apa Sensor Pengukur Medan Magnet?", "id": "Magnetometer." },
    { "en": "Apa Sensor Yang Mendeteksi Kehadiran Objek?", "id": "Sensor Jarak (Proximity Sensor)." },
    { "en": "Apa Sensor Jarak Yang Mendeteksi Logam?", "id": "Sensor Proximity Induktif." },
    { "en": "Apa Sensor Jarak Yang Mendeteksi Semua Benda?", "id": "Sensor Proximity Kapasitif." },
    { "en": "Apa Sensor Jarak Berbasis Cahaya Inframerah?", "id": "Sensor Proximity Inframerah." },
    { "en": "Apa Sensor Jarak Berbasis Gelombang Suara?", "id": "Sensor Proximity Ultrasonik." },
    { "en": "Apa Sensor Pengukur Suara?", "id": "Mikrofon (Microphone)." },
    { "en": "Apa Mikrofon Berbasis Kapasitor?", "id": "Mikrofon Kondensor." },
    { "en": "Apa Mikrofon Berbasis Induksi Elektromagnetik?", "id": "Mikrofon Dinamis." },
    { "en": "Apa Sensor Pengukur Kelembaban?", "id": "Sensor Kelembaban (Hygrometer)." },
    { "en": "Apa Sensor Kelembaban Berbasis Kapasitansi?", "id": "Sensor Kelembaban Kapasitif." },
    { "en": "Apa Sensor Kelembaban Berbasis Resistansi?", "id": "Sensor Kelembaban Resistif." },
    { "en": "Apa Sensor Pengukur Komposisi Kimia?", "id": "Sensor Gas." },
    { "en": "Apa Sensor Gas Berbasis Oksida Logam?", "id": "Metal Oxide Semiconductor Gas Sensor." },
    { "en": "Apa Sensor Pengukur pH?", "id": "Elektroda pH." },
    { "en": "Apa Efek Menghasilkan Tegangan Akibat Tekanan?", "id": "Efek Piezoelektrik." },
    { "en": "Apa Efek Menghasilkan Tegangan Akibat Suhu?", "id": "Efek Piroelektrik." },
    { "en": "Apa Sensor Gerak Berbasis Efek Piroelektrik?", "id": "Sensor PIR (Passive Infrared)." },
    { "en": "Apa Efek Menghasilkan Tegangan Akibat Medan Magnet?", "id": "Efek Hall." },
    { "en": "Apa Efek Perubahan Resistansi Akibat Regangan?", "id": "Efek Piezoresistif." },
    { "en": "Apa Sensor Berbasis Efek Piezoresistif?", "id": "Strain Gauge Semikonduktor." },
    { "en": "Apa Transduser Yang Mengubah Listrik Menjadi Gerakan?", "id": "Aktuator (Actuator)." },
    { "en": "Apa Contoh Aktuator?", "id": "Motor, Solenoida, Relai." },
    { "en": "Apa Rangkaian Pengkondisi Sinyal?", "id": "Memperkuat Dan Menyaring Sinyal Sensor." },
    { "en": "Apa Fungsi Penguat Instrumentasi?", "id": "Menguatkan Sinyal Diferensial Kecil." },
    { "en": "Apa Fungsi Filter Dalam Pengkondisian Sinyal?", "id": "Menghilangkan Derau Yang Tidak Diinginkan." },
    { "en": "Apa Proses Mengubah Sinyal Analog Ke Digital?", "id": "Digitalisasi (Digitization)." },
    { "en": "Apa Perangkat Untuk Digitalisasi?", "id": "ADC (Analog-to-Digital Converter)." },
    { "en": "Apa Itu Kalibrasi Sensor?", "id": "Menyesuaikan Output Sensor Dengan Standar." },
    { "en": "Apa Transduser Pengukur Gaya Atau Beban?", "id": "Load Cell." },
    { "en": "Apa Tipe Load Cell Yang Umum?", "id": "Strain Gauge Load Cell." },
    { "en": "Apa Transduser Pengukur Torsi?", "id": "Torque Sensor." },
    { "en": "Apa Sensor Getaran Akustik?", "id": "Sensor Akustik (Acoustic Sensor)." },
    { "en": "Apa Sensor Yang Mendeteksi Suara?", "id": "Mikrofon (Microphone)." },
    { "en": "Apa Transduser Yang Menghasilkan Suara?", "id": "Speaker Atau Buzzer." },
    { "en": "Apa Sensor Yang Mendeteksi Gelombang Ultrasonik?", "id": "Transduser Ultrasonik." },
    { "en": "Apa Fungsi Transduser Ultrasonik?", "id": "Mengukur Jarak, Mendeteksi Objek." },
    { "en": "Apa Sensor Yang Mendeteksi Gelombang Infrasonik?", "id": "Sensor Infrasonik." },
    { "en": "Apa Sensor Pengukur Keasaman (pH)?", "id": "Elektroda pH (pH Electrode)." },
    { "en": "Apa Sensor Pengukur Konduktivitas Listrik Cairan?", "id": "Conductivity Sensor." },
    { "en": "Apa Sensor Pengukur Oksigen Terlarut?", "id": "DO (Dissolved Oxygen) Sensor." },
    { "en": "Apa Sensor Pengukur Kekeruhan?", "id": "Turbidity Sensor." },
    { "en": "Apa Sensor Untuk Mendeteksi Asap?", "id": "Detektor Asap (Smoke Detector)." },
    { "en": "Apa Tipe Detektor Asap Umum?", "id": "Ionisasi Dan Fotoelektrik." },
    { "en": "Apa Sensor Untuk Mendeteksi Gas Karbon Monoksida?", "id": "CO (Carbon Monoxide) Sensor." },
    { "en": "Apa Sensor Untuk Mendeteksi Gas Mudah Terbakar?", "id": "Combustible Gas Sensor." },
    { "en": "Apa Sensor Untuk Mengukur Kelembaban Tanah?", "id": "Soil Moisture Sensor." },
    { "en": "Apa Sensor Untuk Mendeteksi Hujan?", "id": "Sensor Hujan (Rain Sensor)." },
    { "en": "Apa Sensor Untuk Mengukur Kecepatan Angin?", "id": "Anemometer." },
    { "en": "Apa Sensor Untuk Mengukur Arah Angin?", "id": "Wind Vane." },
    { "en": "Apa Sensor Untuk Mengukur Tekanan Atmosfer?", "id": "Barometer." },
    { "en": "Apa Sensor Untuk Mengukur Radiasi Matahari?", "id": "Pyranometer." },
    { "en": "Apa Sensor Untuk Mengukur Radiasi Nuklir?", "id": "Geiger Counter." },
    { "en": "Apa Tabung Dalam Geiger Counter?", "id": "Geiger-Müller Tube." },
    { "en": "Apa Sensor Berkas Cahaya?", "id": "Photoelectric Sensor." },
    { "en": "Apa Tipe Photoelectric Sensor?", "id": "Through-beam, Retro-reflective, Diffuse." },
    { "en": "Apa Sensor Yang Menggunakan Pemancar Dan Penerima Terpisah?", "id": "Through-beam Sensor." },
    { "en": "Apa Sensor Yang Menggunakan Reflektor?", "id": "Retro-reflective Sensor." },
    { "en": "Apa Sensor Yang Mendeteksi Pantulan Dari Objek?", "id": "Diffuse Sensor." },
    { "en": "Apa Sensor Untuk Membaca Kode Garis?", "id": "Barcode Scanner." },
    { "en": "Apa Sensor Untuk Membaca Warna?", "id": "Sensor Warna (Color Sensor)." },
    { "en": "Apa Sensor Untuk Membaca Sidik Jari?", "id": "Fingerprint Sensor." },
    { "en": "Apa Sensor Gambar?", "id": "Image Sensor." },
    { "en": "Apa Dua Tipe Utama Image Sensor?", "id": "CCD (Charge-Coupled Device) dan CMOS (Complementary Metal-Oxide-Semiconductor)." },
    { "en": "Apa Sensor Gambar Yang Lebih Umum Sekarang?", "id": "CMOS (Complementary Metal-Oxide-Semiconductor)." },
    { "en": "Apa Elemen Individual Dalam Sensor Gambar?", "id": "Piksel (Pixel)." },
    { "en": "Apa Transduser Untuk Mengubah Listrik Menjadi Gerakan Lurus?", "id": "Solenoida (Solenoid)." },
    { "en": "Apa Transduser Untuk Mengubah Listrik Menjadi Getaran?", "id": "Vibration Motor." },
    { "en": "Apa Material Yang Berubah Bentuk Karena Listrik?", "id": "Piezoelectric Actuator." },
    { "en": "Apa Saklar Yang Dioperasikan Medan Magnet?", "id": "Reed Switch." },
    { "en": "Apa Sensor Arus Berbasis Efek Hall?", "id": "Hall Effect Current Sensor." },
    { "en": "Apa Keuntungan Hall Effect Current Sensor?", "id": "Terisolasi Secara Elektrik (Non-invasif)." },
    { "en": "Apa Sensor Arus Berbasis Trafo?", "id": "Current Transformer (CT)." },
    { "en": "Apa Sensor Arus Berbasis Resistor Shunt?", "id": "Shunt Resistor." },
    { "en": "Apa Sensor Yang Dapat Menyentuh Dan Merasakan?", "id": "Sensor Taktil (Tactile Sensor)." },
    { "en": "Apa Sensor Untuk Mengukur Regangan?", "id": "Strain Gauge." },
    { "en": "Apa Faktor Gauge (Gauge Factor)?", "id": "Sensitivitas Dari Strain Gauge." },
    { "en": "Apa Transduser Yang Mengukur Viskositas?", "id": "Viscometer." },
    { "en": "Apa Sensor Yang Mengukur Posisi Dengan GPS (Global Positioning System)?", "id": "GPS (Global Positioning System) Receiver." },
    { "en": "Apa Sensor Untuk Komunikasi Jarak Dekat?", "id": "NFC (Near Field Communication) Sensor." },
    { "en": "Apa Sensor Untuk Membaca Kartu RFID (Radio-Frequency Identification)?", "id": "RFID (Radio-Frequency Identification) Reader." },
    { "en": "Apa Sensor Berbasis Gelombang Mikro?", "id": "Microwave Sensor." },
    { "en": "Apa Fungsi Microwave Sensor?", "id": "Mendeteksi Gerakan." },
    { "en": "Apa Perbedaan PIR (Passive Infrared) Dan Microwave Sensor?", "id": "Microwave Sensor Adalah Sensor Aktif." },
    { "en": "Apa Sensor Yang Mendeteksi Api?", "id": "Flame Detector." },
    { "en": "Bagaimana Flame Detector Bekerja?", "id": "Mendeteksi Radiasi UV (Ultra Violet) / IR (Infra Red)." },
    { "en": "Apa Sensor Untuk Mengukur Denyut Jantung?", "id": "Heart Rate Sensor." },
    { "en": "Apa Tipe Heart Rate Sensor Umum?", "id": "Optik (PPG) Dan Elektrik (ECG)." },
    { "en": "Apa Itu PPG (Photoplethysmography)?", "id": "Mengukur Perubahan Volume Darah." },
    { "en": "Apa Itu ECG (Electrocardiography)?", "id": "Mengukur Aktivitas Listrik Jantung." },
    { "en": "Apa Sensor Untuk Mengukur Saturasi Oksigen?", "id": "Pulse Oximeter." },
    { "en": "Apa Sensor Untuk Mengukur Tekanan Darah?", "id": "Blood Pressure Sensor." },
    { "en": "Apa Sensor Untuk Mengukur Suhu Tubuh?", "id": "Termometer Medis." },
    { "en": "Apa Sensor Untuk Mengukur Glukosa Darah?", "id": "Blood Glucose Meter." },
    { "en": "Apa Sensor Yang Dapat Dikenakan?", "id": "Wearable Sensor." },
    { "en": "Apa Sensor Cerdas (Smart Sensor)?", "id": "Sensor Dengan Pemrosesan Internal." },
    { "en": "Apa Sensor Nirkabel?", "id": "Sensor Yang Mengirim Data Nirkabel." },
    { "en": "Apa Jaringan Sensor Nirkabel?", "id": "WSN (Wireless Sensor Network)." },
    { "en": "Apa Simpul Dalam WSN (Wireless Sensor Network)?", "id": "Sensor Node." },
    { "en": "Apa Pengumpul Data Dalam WSN (Wireless Sensor Network)?", "id": "Gateway Atau Sink Node." },
    { "en": "Apa Itu Fusi Sensor (Sensor Fusion)?", "id": "Menggabungkan Data Dari Berbagai Sensor." },
    { "en": "Apa Tujuan Fusi Sensor?", "id": "Meningkatkan Akurasi Dan Keandalan." },
    { "en": "Apa Itu Redundansi Sensor?", "id": "Menggunakan Beberapa Sensor Sejenis." },
    { "en": "Apa Itu Sinyal Diferensial?", "id": "Informasi Dalam Selisih Dua Sinyal." },
    { "en": "Apa Keuntungan Sinyal Diferensial?", "id": "Ketahanan Terhadap Derau (Noise Immunity)." },
    { "en": "Apa Itu Sinyal Single-Ended?", "id": "Sinyal Diukur Terhadap Ground." },
    { "en": "Apa Transduser Untuk Mengukur Sudut?", "id": "Resolver." },
    { "en": "Apa Transduser Untuk Mengukur Regangan?", "id": "Strain Gauge." },
    { "en": "Apa Sensor Untuk Mengukur Ketebalan?", "id": "Thickness Gauge." },
    { "en": "Apa Sensor Untuk Mengukur Kehalusan Permukaan?", "id": "Surface Roughness Sensor." },
    { "en": "Apa Sensor Untuk Mengukur Kelembaban Relatif?", "id": "RH (Relative Humidity) Sensor." },
    { "en": "Apa Sensor Untuk Mengukur Titik Embun?", "id": "Dew Point Sensor." },
    { "en": "Apa Sensor Untuk Mengukur pH Tanah?", "id": "Soil pH Sensor." },
    { "en": "Apa Sensor Untuk Mendeteksi Sentuhan?", "id": "Touch Sensor." },
    { "en": "Apa Tipe Touch Sensor Umum?", "id": "Kapasitif Dan Resistif." },
    { "en": "Bagaimana Layar Sentuh Bekerja?", "id": "Menggunakan Matriks Sensor Sentuh." },
    { "en": "Apa Sensor Untuk Mengukur Gaya Tekan?", "id": "Force-Sensing Resistor (FSR)." },
    { "en": "Apa Sensor Yang Mendeteksi Pembengkokan?", "id": "Flex Sensor." },
    { "en": "Apa Sensor Berbasis Serat Optik?", "id": "Fiber Optic Sensor." },
    { "en": "Apa Keuntungan Fiber Optic Sensor?", "id": "Kebal Terhadap EMI (Electromagnetic Interference)." },
    { "en": "Apa Itu Kompensasi Suhu?", "id": "Mengoreksi Pengaruh Suhu Pada Sensor." },
    { "en": "Apa Itu Linearisasi?", "id": "Membuat Output Sensor Menjadi Linear." },
    { "en": "Apa Itu Amplifier?", "id": "Memperkuat Sinyal Lemah Dari Sensor." },
    { "en": "Apa Itu Filter?", "id": "Menghilangkan Frekuensi Yang Tidak Diinginkan." },
    { "en": "Apa Itu Transduser Bolak-balik (Reciprocal)?", "id": "Dapat Berfungsi Sebagai Sensor-Aktuator." },
    { "en": "Apa Contoh Transduser Bolak-balik?", "id": "Piezoelectric Crystal, Loudspeaker." },
    { "en": "Apa Itu Transduser Input?", "id": "Nama Lain Untuk Sensor." },
    { "en": "Apa Itu Transduser Output?", "id": "Nama Lain Untuk Aktuator." },
    { "en": "Apa Efek Pemanasan Diri (Self-heating)?", "id": "Sensor Menjadi Panas Akibat Arus." },
    { "en": "Apa Sensor Yang Mengukur Posisi?", "id": "Potensiometer, Encoder, LVDT." },
    { "en": "Apa Sensor Yang Mengukur Gaya?", "id": "Strain Gauge, Load Cell, FSR." },
    { "en": "Apa Sensor Yang Mengukur Suara?", "id": "Mikrofon." },
    { "en": "Apa Sensor Yang Mengukur Cahaya?", "id": "LDR, Fotodioda, Fototransistor." },
    { "en": "Apa Sensor Yang Mengukur Jarak?", "id": "Ultrasonik, Inframerah, LiDAR." },
    { "en": "Apa Sensor Yang Mengukur Kecepatan Sudut?", "id": "Giroskop, Tachometer." },
    { "en": "Apa Sensor Yang Mengukur Percepatan Linear?", "id": "Akselerometer." },
    { "en": "Apa Sensor Yang Mengukur Medan Magnet?", "id": "Hall Effect, Magnetometer, Reed Switch." },
    { "en": "Apa Sensor Yang Mengukur Kelembaban?", "id": "Hygrometer Kapasitif Dan Resistif." },
    { "en": "Apa Sensor Yang Mengukur pH?", "id": "Elektroda Gelas pH." },
    { "en": "Apa Sensor Yang Mengukur Tekanan?", "id": "Piezoresistive, Kapasitif, Piezoelektrik." },
    { "en": "Apa Sensor Yang Mengukur Aliran?", "id": "Turbin, Ultrasonik, Elektromagnetik." },
    { "en": "Apa Prinsip Dasar Termokopel?", "id": "Efek Seebeck." },
    { "en": "Apa Prinsip Dasar RTD (Resistance Temperature Detector)?", "id": "Perubahan Resistansi Logam." },
    { "en": "Apa Prinsip Dasar Termistor?", "id": "Perubahan Resistansi Semikonduktor." },
    { "en": "Apa Prinsip Dasar Strain Gauge?", "id": "Efek Piezoresistif." },
    { "en": "Apa Prinsip Dasar Sensor Piezoelektrik?", "id": "Efek Piezoelektrik." },
    { "en": "Apa Prinsip Dasar Sensor PIR (Passive Infrared)?", "id": "Efek Piroelektrik." },
    { "en": "Apa Prinsip Dasar Sensor Hall Effect?", "id": "Efek Hall." },
    { "en": "Apa Prinsip Dasar LDR (Light Dependent Resistor)?", "id": "Fotokonduktivitas." },
    { "en": "Apa Prinsip Dasar Fotodioda?", "id": "Efek Fotovoltaik." },
    { "en": "Apa Itu Kalibrasi Satu Titik?", "id": "Menyesuaikan Offset Sensor." },
    { "en": "Apa Itu Kalibrasi Dua Titik?", "id": "Menyesuaikan Offset Dan Span Sensor." },
    { "en": "Apa Itu Span Sensor?", "id": "Perbedaan Antara Batas Atas-Bawah." },
    { "en": "Apa Itu Offset Sensor?", "id": "Output Sensor Saat Input Nol." },
    { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Respon Dari 10% Ke 90%." },
    { "en": "Apa Itu Waktu Turun (Fall Time)?", "id": "Waktu Respon Dari 90% Ke 10%." },
    { "en": "Apa Itu Waktu Tunda (Delay Time)?", "id": "Waktu Awal Sebelum Sensor Merespon." },
    { "en": "Apa Itu Konstanta Waktu?", "id": "Waktu Untuk Mencapai 63.2% Nilai Akhir." },
    { "en": "Apa Itu Bandwidth Sensor?", "id": "Rentang Frekuensi Operasi Sensor." },
    { "en": "Apa Itu Derau (Noise)?", "id": "Fluktuasi Acak Pada Output Sensor." },
    { "en": "Apa Sumber Derau Umum?", "id": "Termal, Shot, Flicker (1/f)." },
    { "en": "Apa Itu SNR (Signal-to-Noise Ratio)?", "id": "Rasio Daya Sinyal Per Daya Derau." },
    { "en": "Apa Itu Pengkondisian Sinyal?", "id": "Memproses Sinyal Mentah Dari Sensor." },
    { "en": "Tahapan Apa Saja Dalam Pengkondisian Sinyal?", "id": "Amplifikasi, Filtering, Linearisasi." },
    { "en": "Apa Itu Amplifikasi?", "id": "Meningkatkan Kekuatan Sinyal." },
    { "en": "Apa Itu Atenuasi?", "id": "Melemahkan Kekuatan Sinyal." },
    { "en": "Apa Itu Isolasi?", "id": "Memisahkan Sensor Dari Sistem Pengukuran." },
    { "en": "Kenapa Isolasi Penting?", "id": "Keamanan Dan Mengurangi Ground Loop." },
    { "en": "Apa Itu Ground Loop?", "id": "Jalur Arus Tak Diinginkan Lewat Ground." },
    { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Sinyal Sensor Ke Pembawa." },
    { "en": "Apa Itu Demodulasi?", "id": "Mengekstrak Sinyal Sensor Dari Pembawa." },
    { "en": "Apa Itu Multiplexing?", "id": "Berbagi Satu ADC Untuk Banyak Sensor." },
    { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
    { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
    { "en": "Apa Itu Sistem Akuisisi Data?", "id": "DAQ (Data Acquisition System)." },
    { "en": "Apa Itu Sensor Kontak?", "id": "Sensor Yang Harus Menyentuh Objek." },
    { "en": "Apa Itu Sensor Non-Kontak?", "id": "Sensor Yang Tidak Perlu Menyentuh." },
    { "en": "Apa Itu Sensor Biner?", "id": "Outputnya Hanya ON Atau OFF." },
    { "en": "Apa Contoh Sensor Biner?", "id": "Saklar Batas (Limit Switch)." },
    { "en": "Apa Itu Sensor Intrinsik?", "id": "Besaran Ukur Langsung Mengubah Sensor." },
    { "en": "Apa Itu Sensor Ekstrinsik?", "id": "Menggunakan Efek Tak Langsung." },
    { "en": "Apa Itu Lensa Fresnel?", "id": "Digunakan Pada Sensor PIR (Passive Infrared)." },
    { "en": "Apa Itu Efek Termoelektrik?", "id": "Gabungan Efek Seebeck, Peltier, Thomson." },
    { "en": "Apa Itu Sel Peltier?", "id": "Transduser Termoelektrik (Pendingin/Pemanas)." },
    { "en": "Apa Itu Transduser Magnetostriktif?", "id": "Mengubah Energi Magnetik-Mekanis." },
    { "en": "Apa Itu Transduser Elektrostatik?", "id": "Berbasis Perubahan Kapasitansi." },
    { "en": "Apa Itu MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sensor Dan Aktuator Skala Mikro." },
    { "en": "Apa Itu Lab-on-a-Chip?", "id": "Perangkat Analisis Kimia Terintegrasi." },
    { "en": "Apa Itu Biosensor?", "id": "Sensor Yang Menggunakan Elemen Biologis." },
    { "en": "Apa Itu Enzim?", "id": "Biokatalis Yang Digunakan Dalam Biosensor." },
    { "en": "Apa Itu Antibodi?", "id": "Protein Untuk Deteksi Spesifik." },
    { "en": "Apa Itu Asam Nukleat?", "id": "DNA/RNA Untuk Sensor Genetik." },
    { "en": "Apa Itu Resonansi Plasmon Permukaan?", "id": "SPR (Surface Plasmon Resonance)." },
    { "en": "Apa Fungsi SPR (Surface Plasmon Resonance)?", "id": "Teknik Deteksi Biomolekuler." },
    { "en": "Apa Itu Nanosensor?", "id": "Sensor Berbasis Material Skala Nano." },
    { "en": "Contoh Nanosensor?", "id": "Nanotube Karbon, Quantum Dots." },
    { "en": "Apa Itu Quantum Dots?", "id": "Nanokristal Semikonduktor Pendar." },
    { "en": "Apa Itu Hidung Elektronik (E-nose)?", "id": "Array Sensor Gas Untuk Deteksi Bau." },
    { "en": "Apa Itu Lidah Elektronik (E-tongue)?", "id": "Array Sensor Cairan Untuk Deteksi Rasa." },
    { "en": "Apa Itu Sensor Suhu Inframerah?", "id": "Mengukur Suhu Dari Radiasi Termal." },
    { "en": "Apa Itu Emisivitas?", "id": "Kemampuan Objek Memancarkan Radiasi." },
    { "en": "Apa Itu Piranti Terkopel Muatan?", "id": "CCD (Charge-Coupled Device)." },
    { "en": "Apa Itu Sensor Gambar CMOS?", "id": "CIS (CMOS Image Sensor)." },
    { "en": "Apa Itu Kamera Time-of-Flight (ToF)?", "id": "Mengukur Jarak Dengan Waktu Tempuh Cahaya." },
    { "en": "Apa Itu Pencitraan Terstruktur Cahaya?", "id": "Memproyeksikan Pola Untuk Mengukur 3D." },
    { "en": "Apa Itu LIDAR (Light Detection and Ranging)?", "id": "Menggunakan Laser Untuk Memetakan Jarak." },
    { "en": "Apa Itu RADAR (Radio Detection and Ranging)?", "id": "Menggunakan Gelombang Radio." },
    { "en": "Apa Itu SONAR (Sound Navigation and Ranging)?", "id": "Menggunakan Gelombang Suara." },
    { "en": "Apa Itu Transduser Magnetoresistif?", "id": "Resistansi Berubah Akibat Medan Magnet." },
    { "en": "Jenis Transduser Magnetoresistif?", "id": "AMR, GMR, TMR." },
    { "en": "Apa Itu GMR (Giant Magnetoresistance)?", "id": "Perubahan Resistansi Sangat Besar." },
    { "en": "Di Mana GMR (Giant Magnetoresistance) Digunakan?", "id": "Head Pembaca Hard Disk." },
    { "en": "Apa Itu TMR (Tunneling Magnetoresistance)?", "id": "Digunakan Dalam Memori MRAM." },
    { "en": "Apa Itu Sensor Optik?", "id": "Sensor Yang Bekerja Berdasarkan Cahaya." },
    { "en": "Apa Itu Sensor Listrik?", "id": "Mengukur Besaran Listrik (Tegangan/Arus)." },
    { "en": "Apa Itu Sensor Mekanis?", "id": "Mengukur Besaran Mekanis (Gaya/Posisi)." },
    { "en": "Apa Itu Sensor Kimia?", "id": "Mendeteksi Kehadiran Atau Konsentrasi Zat." },
    { "en": "Apa Itu Sensor Termal?", "id": "Mengukur Suhu Atau Aliran Panas." },
    { "en": "Apa Itu Transduser Kebalikan (Inverse Transducer)?", "id": "Mengubah Listrik Menjadi Besaran Non-listrik." },
    { "en": "Contoh Transduser Kebalikan?", "id": "LED, Pemanas, Motor." },
    { "en": "Apa Itu Sensor Absolut?", "id": "Memberi Output Absolut (Encoder Absolut)." },
    { "en": "Apa Itu Sensor Relatif?", "id": "Mengukur Perubahan (Encoder Inkremental)." },
    { "en": "Apa Itu Kompensasi Sambungan Dingin?", "id": "CJC (Cold Junction Compensation) Untuk Termokopel." },
    { "en": "Apa Itu Jembatan Setengah (Half-bridge)?", "id": "Konfigurasi Jembatan Dengan Dua Elemen." },
    { "en": "Apa Itu Jembatan Penuh (Full-bridge)?", "id": "Konfigurasi Jembatan Dengan Empat Elemen." },
    { "en": "Konfigurasi Mana Yang Paling Sensitif?", "id": "Jembatan Penuh (Full-bridge)." },
    { "en": "Apa Itu Tegangan Eksitasi?", "id": "Tegangan Input Yang Diberikan Ke Jembatan." },
    { "en": "Apa Itu Sensor Ratiometrik?", "id": "Output Proporsional Dengan Tegangan Eksitasi." },
    { "en": "Apa Itu Sinyal 4-20 mA?", "id": "Standar Sinyal Arus Industri." },
    { "en": "Apa Keuntungan Sinyal Arus?", "id": "Tahan Derau, Jarak Jauh." },
    { "en": "Apa Transmitter 4-20 mA?", "id": "Mengubah Output Sensor Menjadi 4-20 mA." },
    { "en": "Apa Itu Protokol HART?", "id": "Sinyal Digital Di Atas Loop 4-20 mA." },
    { "en": "Apa Itu Fieldbus?", "id": "Jaringan Komunikasi Digital Industri." },
    { "en": "Apa Itu Sensor Nirkabel?", "id": "Mengirim Data Tanpa Kabel." },
    { "en": "Standar Komunikasi Apa Yang Dipakai Sensor Nirkabel?", "id": "ISA100, WirelessHART, Zigbee." },
    { "en": "Apa Itu Sensor Ultrasonik?", "id": "Menggunakan Gelombang Suara Frekuensi Tinggi." },
    { "en": "Apa Rentang Frekuensi Ultrasonik?", "id": "Di Atas 20 kHz." },
    { "en": "Apa Itu Waktu Tempuh (Time of Flight)?", "id": "Waktu Pulsa Pergi Dan Kembali." },
    { "en": "Bagaimana Sensor Ultrasonik Mengukur Jarak?", "id": "Berdasarkan Waktu Tempuh Gema." },
    { "en": "Apa Itu Zona Buta (Blind Zone)?", "id": "Area Dekat Sensor Tidak Terukur." },
    { "en": "Apa Itu Sudut Berkas (Beam Angle)?", "id": "Lebar Area Deteksi Sensor." },
    { "en": "Apa Itu Cross-talk?", "id": "Gangguan Antar Sensor Ultrasonik." },
    { "en": "Apa Itu Sensor Tekanan Absolut?", "id": "Mengukur Relatif Terhadap Vakum." },
    { "en": "Apa Itu Sensor Tekanan Gauge?", "id": "Mengukur Relatif Terhadap Atmosfer." },
    { "en": "Apa Itu Sensor Tekanan Diferensial?", "id": "Mengukur Selisih Dua Tekanan." },
    { "en": "Apa Itu Manometer?", "id": "Alat Ukur Tekanan." },
    { "en": "Apa Itu Tabung Pitot?", "id": "Mengukur Kecepatan Aliran Fluida." },
    { "en": "Apa Itu Tabung Venturi?", "id": "Mengukur Aliran Berdasarkan Penurunan Tekanan." },
    { "en": "Apa Itu Anemometer Kawat Panas?", "id": "Mengukur Aliran Udara Dengan Kawat Panas." },
    { "en": "Apa Itu Rotameter?", "id": "Flowmeter Dengan Pelampung Dalam Tabung." },
    { "en": "Apa Itu Coriolis Mass Flowmeter?", "id": "Mengukur Aliran Massa Secara Langsung." },
    { "en": "Apa Itu Transduser Getaran?", "id": "Akselerometer Atau Velosimeter." },
    { "en": "Apa Itu Velosimeter?", "id": "Sensor Yang Mengukur Kecepatan Getaran." },
    { "en": "Apa Itu Probe Arus Eddy?", "id": "Mengukur Perpindahan Atau Getaran Non-kontak." },
    { "en": "Apa Itu Analisis Spektrum Getaran?", "id": "Menguraikan Sinyal Getaran Ke Frekuensi." },
    { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Algoritma Untuk Analisis Spektrum." },
    { "en": "Apa Itu Resonansi Mekanis?", "id": "Frekuensi Dimana Amplitudo Getaran Maksimum." },
    { "en": "Apa Itu Redaman (Damping)?", "id": "Proses Meredam Getaran." },
    { "en": "Apa Itu Sensor Suhu Bimetal?", "id": "Dua Logam Dengan Koefisien Muai Berbeda." },
    { "en": "Di Mana Sensor Bimetal Digunakan?", "id": "Termostat Mekanis." },
    { "en": "Apa Itu Sensor Suhu Gas?", "id": "Mengukur Suhu Berdasarkan Tekanan Gas." },
    { "en": "Apa Itu Pirometer?", "id": "Mengukur Suhu Dari Radiasi Termal." },
    { "en": "Apa Itu Kamera Termal?", "id": "Mencitrakan Distribusi Suhu." },
    { "en": "Apa Itu Sensor Aliran Panas?", "id": "Mengukur Laju Transfer Panas." },
    { "en": "Apa Itu Saklar Aliran (Flow Switch)?", "id": "Mendeteksi Ada Atau Tidaknya Aliran." },
    { "en": "Apa Itu Saklar Tekanan?", "id": "Aktif Pada Level Tekanan Tertentu." },
    { "en": "Apa Itu Saklar Level?", "id": "Mendeteksi Level Cairan Tertentu." },
    { "en": "Apa Itu Saklar Suhu?", "id": "Termostat (Thermostat)." },
    { "en": "Apa Itu Sensor Magnetik?", "id": "Mendeteksi Kehadiran Atau Kekuatan Medan Magnet." },
    { "en": "Apa Itu Sensor Wiegand?", "id": "Sensor Magnetik Tanpa Daya." },
    { "en": "Apa Itu Sensor Magnetoresistif?", "id": "Resistansi Berubah Akibat Medan Magnet." },
    { "en": "Apa Itu GMR (Giant Magnetoresistance)?", "id": "Sensor Magnetoresistif Sangat Sensitif." },
    { "en": "Apa Itu SQUID (Superconducting Quantum Interference Device)?", "id": "Magnetometer Paling Sensitif." },
    { "en": "Apa Itu Fluxgate Magnetometer?", "id": "Sensor Medan Magnet DC Sensitif." },
    { "en": "Apa Itu Sensor Posisi Magnetostriktif?", "id": "Sensor Posisi Linear Akurasi Tinggi." },
    { "en": "Apa Itu Encoder Magnetik?", "id": "Sensor Posisi Menggunakan Medan Magnet." },
    { "en": "Apa Itu Saklar Efek Hall?", "id": "Saklar Digital Berbasis Efek Hall." },
    { "en": "Apa Itu Sensor Sudut?", "id": "Mengukur Orientasi Atau Kemiringan." },
    { "en": "Apa Itu Inclinometer?", "id": "Mengukur Sudut Kemiringan Relatif Gravitasi." },
    { "en": "Apa Itu Tilt Switch?", "id": "Saklar Yang Aktif Saat Miring." },
    { "en": "Apa Itu Kompas?", "id": "Menentukan Arah Mata Angin." },
    { "en": "Apa Itu AHRS (Attitude and Heading Reference System)?", "id": "Memberikan Informasi Orientasi 3D." },
    { "en": "Apa Itu Sensor Kelembaban Udara?", "id": "Hygrometer." },
    { "en": "Apa Itu Kelembaban Absolut?", "id": "Massa Uap Air Per Volume Udara." },
    { "en": "Apa Itu Kelembaban Relatif?", "id": "Rasio Uap Air Aktual Per Maksimum." },
    { "en": "Apa Itu Titik Embun (Dew Point)?", "id": "Suhu Saat Embun Mulai Terbentuk." },
    { "en": "Apa Itu Psikrometer?", "id": "Mengukur Kelembaban Dengan Termometer Basah-Kering." },
    { "en": "Apa Itu Sensor Optik?", "id": "Bekerja Berdasarkan Prinsip Optik." },
    { "en": "Apa Itu Interferometer?", "id": "Mengukur Perubahan Jarak Sangat Kecil." },
    { "en": "Apa Itu Spektrometer?", "id": "Mengukur Spektrum Cahaya." },
    { "en": "Apa Itu Kolorimeter?", "id": "Mengukur Absorbansi Cahaya." },
    { "en": "Apa Itu Opasitas Meter?", "id": "Mengukur Seberapa Buram Suatu Medium." },
    { "en": "Apa Itu Sensor Hamburan Cahaya?", "id": "Mengukur Partikel Di Udara Atau Cairan." },
    { "en": "Apa Itu Sensor Asap Fotoelektrik?", "id": "Mendeteksi Asap Dengan Hamburan Cahaya." },
    { "en": "Apa Itu Sensor Asap Ionisasi?", "id": "Mendeteksi Asap Dengan Kamar Ionisasi." },
    { "en": "Apa Itu Sensor Debu Optik?", "id": "Mengukur Konsentrasi Partikel Debu." },
    { "en": "Apa Itu Polarimeter?", "id": "Mengukur Rotasi Polarisasi Cahaya." },
    { "en": "Apa Itu Refraktometer?", "id": "Mengukur Indeks Bias Cairan." },
    { "en": "Apa Itu Sensor Kimia?", "id": "Mendeteksi Kehadiran Zat Kimia." },
    { "en": "Apa Itu Elektroda Selektif Ion?", "id": "ISE (Ion-Selective Electrode)." },
    { "en": "Apa Itu Sensor Oksida Logam?", "id": "MOS (Metal Oxide Semiconductor) Gas Sensor." },
    { "en": "Apa Itu Sensor Inframerah Non-dispersif?", "id": "NDIR (Nondispersive Infrared) Sensor." },
    { "en": "Untuk Apa Sensor NDIR (Nondispersive Infrared)?", "id": "Mengukur Konsentrasi Gas (CO2)." },
    { "en": "Apa Itu Sensor Elektrokimia?", "id": "Mengukur Gas Beracun (CO, H2S)." },
    { "en": "Apa Itu Sensor Katalitik?", "id": "Mendeteksi Gas Mudah Terbakar." },
    { "en": "Apa Itu Sensor Kelembaban Resistif?", "id": "Resistansi Berubah Sesuai Kelembaban." },
    { "en": "Apa Itu Sensor Kelembaban Kapasitif?", "id": "Kapasitansi Berubah Sesuai Kelembaban." },
    { "en": "Apa Itu Sensor Gelombang Akustik Permukaan?", "id": "SAW (Surface Acoustic Wave) Sensor." },
    { "en": "Apa Itu Mikro-keseimbangan Kristal Kuarsa?", "id": "QCM (Quartz Crystal Microbalance)." },
    { "en": "Apa Fungsi QCM (Quartz Crystal Microbalance)?", "id": "Mengukur Perubahan Massa Sangat Kecil." },
    { "en": "Apa Itu Transduser Biomedis?", "id": "Sensor Untuk Aplikasi Medis." },
    { "en": "Apa Itu Elektroda?", "id": "Konduktor Untuk Kontak Dengan Tubuh." },
    { "en": "Apa Itu Elektroda EEG (Electroencephalography)?", "id": "Mengukur Aktivitas Listrik Otak." },
    { "en": "Apa Itu Elektroda ECG (Electrocardiography)?", "id": "Mengukur Aktivitas Listrik Jantung." },
    { "en": "Apa Itu Elektroda EMG (Electromyography)?", "id": "Mengukur Aktivitas Listrik Otot." },
    { "en": "Apa Itu Sensor Laju Pernapasan?", "id": "Mengukur Frekuensi Bernapas." },
    { "en": "Apa Itu Sensor Konduktivitas Kulit?", "id": "GSR (Galvanic Skin Response) Sensor." },
    { "en": "Apa Efek Pemanasan Diri (Self-heating)?", "id": "Sensor Menjadi Panas Akibat Arus." },
    { "en": "Apa Sensor Yang Mengukur Posisi?", "id": "Potensiometer, Encoder, LVDT." },
    { "en": "Apa Sensor Yang Mengukur Gaya?", "id": "Strain Gauge, Load Cell, FSR." },
    { "en": "Apa Sensor Yang Mengukur Suara?", "id": "Mikrofon." },
    { "en": "Apa Sensor Yang Mengukur Cahaya?", "id": "LDR, Fotodioda, Fototransistor." },
    { "en": "Apa Sensor Yang Mengukur Jarak?", "id": "Ultrasonik, Inframerah, LiDAR." },
    { "en": "Apa Sensor Yang Mengukur Kecepatan Sudut?", "id": "Giroskop, Tachometer." },
    { "en": "Apa Sensor Yang Mengukur Percepatan Linear?", "id": "Akselerometer." },
    { "en": "Apa Sensor Yang Mengukur Medan Magnet?", "id": "Hall Effect, Magnetometer, Reed Switch." },
    { "en": "Apa Sensor Yang Mengukur Kelembaban?", "id": "Hygrometer Kapasitif Dan Resistif." },
    { "en": "Apa Sensor Yang Mengukur pH?", "id": "Elektroda Gelas pH." },
    { "en": "Apa Sensor Yang Mengukur Tekanan?", "id": "Piezoresistive, Kapasitif, Piezoelektrik." },
    { "en": "Apa Sensor Yang Mengukur Aliran?", "id": "Turbin, Ultrasonik, Elektromagnetik." },
    { "en": "Apa Prinsip Dasar Termokopel?", "id": "Efek Seebeck." },
    { "en": "Apa Prinsip Dasar RTD (Resistance Temperature Detector)?", "id": "Perubahan Resistansi Logam." },
    { "en": "Apa Prinsip Dasar Termistor?", "id": "Perubahan Resistansi Semikonduktor." },
    { "en": "Apa Prinsip Dasar Strain Gauge?", "id": "Efek Piezoresistif." },
    { "en": "Apa Prinsip Dasar Sensor Piezoelektrik?", "id": "Efek Piezoelektrik." },
    { "en": "Apa Prinsip Dasar Sensor PIR (Passive Infrared)?", "id": "Efek Piroelektrik." },
    { "en": "Apa Prinsip Dasar Sensor Hall Effect?", "id": "Efek Hall." },
    { "en": "Apa Prinsip Dasar LDR (Light Dependent Resistor)?", "id": "Fotokonduktivitas." },
    { "en": "Apa Prinsip Dasar Fotodioda?", "id": "Efek Fotovoltaik." },
    { "en": "Apa Itu Kalibrasi Satu Titik?", "id": "Menyesuaikan Offset Sensor." },
    { "en": "Apa Itu Kalibrasi Dua Titik?", "id": "Menyesuaikan Offset Dan Span Sensor." },
    { "en": "Apa Itu Span Sensor?", "id": "Perbedaan Antara Batas Atas-Bawah." },
    { "en": "Apa Itu Offset Sensor?", "id": "Output Sensor Saat Input Nol." },
    { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Respon Dari 10% Ke 90%." },
    { "en": "Apa Itu Waktu Turun (Fall Time)?", "id": "Waktu Respon Dari 90% Ke 10%." },
    { "en": "Apa Itu Waktu Tunda (Delay Time)?", "id": "Waktu Awal Sebelum Sensor Merespon." },
    { "en": "Apa Itu Konstanta Waktu?", "id": "Waktu Untuk Mencapai 63.2% Nilai Akhir." },
    { "en": "Apa Itu Bandwidth Sensor?", "id": "Rentang Frekuensi Operasi Sensor." },
    { "en": "Apa Itu Derau (Noise)?", "id": "Fluktuasi Acak Pada Output Sensor." },
    { "en": "Apa Sumber Derau Umum?", "id": "Termal, Shot, Flicker (1/f)." },
    { "en": "Apa Itu SNR (Signal-to-Noise Ratio)?", "id": "Rasio Daya Sinyal Per Daya Derau." },
    { "en": "Apa Itu Pengkondisian Sinyal?", "id": "Memproses Sinyal Mentah Dari Sensor." },
    { "en": "Tahapan Apa Saja Dalam Pengkondisian Sinyal?", "id": "Amplifikasi, Filtering, Linearisasi." },
    { "en": "Apa Itu Amplifikasi?", "id": "Meningkatkan Kekuatan Sinyal." },
    { "en": "Apa Itu Atenuasi?", "id": "Melemahkan Kekuatan Sinyal." },
    { "en": "Apa Itu Isolasi?", "id": "Memisahkan Sensor Dari Sistem Pengukuran." },
    { "en": "Kenapa Isolasi Penting?", "id": "Keamanan Dan Mengurangi Ground Loop." },
    { "en": "Apa Itu Ground Loop?", "id": "Jalur Arus Tak Diinginkan Lewat Ground." },
    { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Sinyal Sensor Ke Pembawa." },
    { "en": "Apa Itu Demodulasi?", "id": "Mengekstrak Sinyal Sensor Dari Pembawa." },
    { "en": "Apa Itu Multiplexing?", "id": "Berbagi Satu ADC Untuk Banyak Sensor." },
    { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
    { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
    { "en": "Apa Itu Sistem Akuisisi Data?", "id": "DAQ (Data Acquisition System)." },
    { "en": "Apa Itu Sensor Kontak?", "id": "Sensor Yang Harus Menyentuh Objek." },
    { "en": "Apa Itu Sensor Non-Kontak?", "id": "Sensor Yang Tidak Perlu Menyentuh." },
    { "en": "Apa Itu Sensor Biner?", "id": "Outputnya Hanya ON Atau OFF." },
    { "en": "Apa Contoh Sensor Biner?", "id": "Saklar Batas (Limit Switch)." },
    { "en": "Apa Itu Sensor Intrinsik?", "id": "Besaran Ukur Langsung Mengubah Sensor." },
    { "en": "Apa Itu Sensor Ekstrinsik?", "id": "Menggunakan Efek Tak Langsung." },
    { "en": "Apa Itu Lensa Fresnel?", "id": "Digunakan Pada Sensor PIR (Passive Infrared)." },
    { "en": "Apa Itu Efek Termoelektrik?", "id": "Gabungan Efek Seebeck, Peltier, Thomson." },
    { "en": "Apa Itu Sel Peltier?", "id": "Transduser Termoelektrik (Pendingin/Pemanas)." },
    { "en": "Apa Itu Transduser Magnetostriktif?", "id": "Mengubah Energi Magnetik-Mekanis." },
    { "en": "Apa Itu Transduser Elektrostatik?", "id": "Berbasis Perubahan Kapasitansi." },
    { "en": "Apa Itu MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sensor Dan Aktuator Skala Mikro." },
    { "en": "Apa Itu Lab-on-a-Chip?", "id": "Perangkat Analisis Kimia Terintegrasi." },
    { "en": "Apa Itu Biosensor?", "id": "Sensor Yang Menggunakan Elemen Biologis." },
    { "en": "Apa Itu Enzim?", "id": "Biokatalis Yang Digunakan Dalam Biosensor." },
    { "en": "Apa Itu Antibodi?", "id": "Protein Untuk Deteksi Spesifik." },
    { "en": "Apa Itu Asam Nukleat?", "id": "DNA/RNA Untuk Sensor Genetik." },
    { "en": "Apa Itu Resonansi Plasmon Permukaan?", "id": "SPR (Surface Plasmon Resonance)." },
    { "en": "Apa Fungsi SPR (Surface Plasmon Resonance)?", "id": "Teknik Deteksi Biomolekuler." },
    { "en": "Apa Itu Nanosensor?", "id": "Sensor Berbasis Material Skala Nano." },
    { "en": "Contoh Nanosensor?", "id": "Nanotube Karbon, Quantum Dots." },
    { "en": "Apa Itu Quantum Dots?", "id": "Nanokristal Semikonduktor Pendar." },
    { "en": "Apa Itu Hidung Elektronik (E-nose)?", "id": "Array Sensor Gas Untuk Deteksi Bau." },
    { "en": "Apa Itu Lidah Elektronik (E-tongue)?", "id": "Array Sensor Cairan Untuk Deteksi Rasa." },
    { "en": "Apa Itu Sensor Suhu Inframerah?", "id": "Mengukur Suhu Dari Radiasi Termal." },
    { "en": "Apa Itu Emisivitas?", "id": "Kemampuan Objek Memancarkan Radiasi." },
    { "en": "Apa Itu Piranti Terkopel Muatan?", "id": "CCD (Charge-Coupled Device)." },
    { "en": "Apa Itu Sensor Gambar CMOS?", "id": "CIS (CMOS Image Sensor)." },
    { "en": "Apa Itu Kamera Time-of-Flight (ToF)?", "id": "Mengukur Jarak Dengan Waktu Tempuh Cahaya." },
    { "en": "Apa Itu Pencitraan Terstruktur Cahaya?", "id": "Memproyeksikan Pola Untuk Mengukur 3D." },
    { "en": "Apa Itu LIDAR (Light Detection and Ranging)?", "id": "Menggunakan Laser Untuk Memetakan Jarak." },
    { "en": "Apa Itu RADAR (Radio Detection and Ranging)?", "id": "Menggunakan Gelombang Radio." },
    { "en": "Apa Itu SONAR (Sound Navigation and Ranging)?", "id": "Menggunakan Gelombang Suara." },
    { "en": "Apa Itu Transduser Magnetoresistif?", "id": "Resistansi Berubah Akibat Medan Magnet." },
    { "en": "Jenis Transduser Magnetoresistif?", "id": "AMR, GMR, TMR." },
    { "en": "Apa Itu GMR (Giant Magnetoresistance)?", "id": "Perubahan Resistansi Sangat Besar." },
    { "en": "Di Mana GMR (Giant Magnetoresistance) Digunakan?", "id": "Head Pembaca Hard Disk." },
    { "en": "Apa Itu TMR (Tunneling Magnetoresistance)?", "id": "Digunakan Dalam Memori MRAM." },
    { "en": "Apa Itu Sensor Optik?", "id": "Sensor Yang Bekerja Berdasarkan Cahaya." },
    { "en": "Apa Itu Sensor Listrik?", "id": "Mengukur Besaran Listrik (Tegangan/Arus)." },
    { "en": "Apa Itu Sensor Mekanis?", "id": "Mengukur Besaran Mekanis (Gaya/Posisi)." },
    { "en": "Apa Itu Sensor Kimia?", "id": "Mendeteksi Kehadiran Atau Konsentrasi Zat." },
    { "en": "Apa Itu Sensor Termal?", "id": "Mengukur Suhu Atau Aliran Panas." },
    { "en": "Apa Itu Transduser Kebalikan (Inverse Transducer)?", "id": "Mengubah Listrik Menjadi Besaran Non-listrik." },
    { "en": "Contoh Transduser Kebalikan?", "id": "LED, Pemanas, Motor." },
    { "en": "Apa Itu Sensor Absolut?", "id": "Memberi Output Absolut (Encoder Absolut)." },
    { "en": "Apa Itu Sensor Relatif?", "id": "Mengukur Perubahan (Encoder Inkremental)." },
    { "en": "Apa Itu Kompensasi Sambungan Dingin?", "id": "CJC (Cold Junction Compensation) Untuk Termokopel." },
    { "en": "Apa Itu Jembatan Setengah (Half-bridge)?", "id": "Konfigurasi Jembatan Dengan Dua Elemen." },
    { "en": "Apa Itu Jembatan Penuh (Full-bridge)?", "id": "Konfigurasi Jembatan Dengan Empat Elemen." },
    { "en": "Konfigurasi Mana Yang Paling Sensitif?", "id": "Jembatan Penuh (Full-bridge)." },
    { "en": "Apa Itu Tegangan Eksitasi?", "id": "Tegangan Input Yang Diberikan Ke Jembatan." },
    { "en": "Apa Itu Sensor Ratiometrik?", "id": "Output Proporsional Dengan Tegangan Eksitasi." },
    { "en": "Apa Itu Sinyal 4-20 mA?", "id": "Standar Sinyal Arus Industri." },
    { "en": "Apa Keuntungan Sinyal Arus?", "id": "Tahan Derau, Jarak Jauh." },
    { "en": "Apa Transmitter 4-20 mA?", "id": "Mengubah Output Sensor Menjadi 4-20 mA." },
    { "en": "Apa Itu Protokol HART (Highway Addressable Remote Transducer)?", "id": "Sinyal Digital Di Atas Loop 4-20 mA." },
    { "en": "Apa Itu Fieldbus?", "id": "Jaringan Komunikasi Digital Industri." },
    { "en": "Apa Itu Sensor Nirkabel?", "id": "Mengirim Data Tanpa Kabel." },
    { "en": "Standar Komunikasi Apa Yang Dipakai Sensor Nirkabel?", "id": "ISA100, WirelessHART, Zigbee." },
    { "en": "Apa Itu Sensor Ultrasonik?", "id": "Menggunakan Gelombang Suara Frekuensi Tinggi." },
    { "en": "Apa Rentang Frekuensi Ultrasonik?", "id": "Di Atas 20 kHz." },
    { "en": "Apa Itu Waktu Tempuh (Time of Flight)?", "id": "Waktu Pulsa Pergi Dan Kembali." },
    { "en": "Bagaimana Sensor Ultrasonik Mengukur Jarak?", "id": "Berdasarkan Waktu Tempuh Gema." },
    { "en": "Apa Itu Zona Buta (Blind Zone)?", "id": "Area Dekat Sensor Tidak Terukur." },
    { "en": "Apa Itu Sudut Berkas (Beam Angle)?", "id": "Lebar Area Deteksi Sensor." },
    { "en": "Apa Itu Cross-talk?", "id": "Gangguan Antar Sensor Ultrasonik." },
    { "en": "Apa Itu Sensor Tekanan Absolut?", "id": "Mengukur Relatif Terhadap Vakum." },
    { "en": "Apa Itu Sensor Tekanan Gauge?", "id": "Mengukur Relatif Terhadap Atmosfer." },
    { "en": "Apa Itu Sensor Tekanan Diferensial?", "id": "Mengukur Selisih Dua Tekanan." },
    { "en": "Apa Itu Manometer?", "id": "Alat Ukur Tekanan." },
    { "en": "Apa Itu Tabung Pitot?", "id": "Mengukur Kecepatan Aliran Fluida." },
    { "en": "Apa Itu Tabung Venturi?", "id": "Mengukur Aliran Berdasarkan Penurunan Tekanan." },
    { "en": "Apa Itu Anemometer Kawat Panas?", "id": "Mengukur Aliran Udara Dengan Kawat Panas." },
    { "en": "Apa Itu Rotameter?", "id": "Flowmeter Dengan Pelampung Dalam Tabung." },
    { "en": "Apa Itu Coriolis Mass Flowmeter?", "id": "Mengukur Aliran Massa Secara Langsung." },
    { "en": "Apa Itu Transduser Getaran?", "id": "Akselerometer Atau Velosimeter." },
    { "en": "Apa Itu Velosimeter?", "id": "Sensor Yang Mengukur Kecepatan Getaran." },
    { "en": "Apa Itu Probe Arus Eddy?", "id": "Mengukur Perpindahan Atau Getaran Non-kontak." },
    { "en": "Apa Itu Analisis Spektrum Getaran?", "id": "Menguraikan Sinyal Getaran Ke Frekuensi." },
    { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Algoritma Untuk Analisis Spektrum." },
    { "en": "Apa Itu Resonansi Mekanis?", "id": "Frekuensi Dimana Amplitudo Getaran Maksimum." },
    { "en": "Apa Itu Redaman (Damping)?", "id": "Proses Meredam Getaran." },
    { "en": "Apa Itu Sensor Suhu Bimetal?", "id": "Dua Logam Dengan Koefisien Muai Berbeda." },
    { "en": "Di Mana Sensor Bimetal Digunakan?", "id": "Termostat Mekanis." },
    { "en": "Apa Itu Sensor Suhu Gas?", "id": "Mengukur Suhu Berdasarkan Tekanan Gas." },
    { "en": "Apa Itu Pirometer?", "id": "Mengukur Suhu Dari Radiasi Termal." },
    { "en": "Apa Itu Kamera Termal?", "id": "Mencitrakan Distribusi Suhu." },
    { "en": "Apa Itu Sensor Aliran Panas?", "id": "Mengukur Laju Transfer Panas." },
    { "en": "Apa Itu Saklar Aliran (Flow Switch)?", "id": "Mendeteksi Ada Atau Tidaknya Aliran." },
    { "en": "Apa Itu Saklar Tekanan?", "id": "Aktif Pada Level Tekanan Tertentu." },
    { "en": "Apa Itu Saklar Level?", "id": "Mendeteksi Level Cairan Tertentu." },
    { "en": "Apa Itu Saklar Suhu?", "id": "Termostat (Thermostat)." },
    { "en": "Apa Itu Sensor Magnetik?", "id": "Mendeteksi Kehadiran Atau Kekuatan Medan Magnet." },
    { "en": "Apa Itu Sensor Wiegand?", "id": "Sensor Magnetik Tanpa Daya." },
    { "en": "Apa Itu Sensor Magnetoresistif?", "id": "Resistansi Berubah Akibat Medan Magnet." },
    { "en": "Apa Itu GMR (Giant Magnetoresistance)?", "id": "Sensor Magnetoresistif Sangat Sensitif." },
    { "en": "Apa Itu SQUID (Superconducting Quantum Interference Device)?", "id": "Magnetometer Paling Sensitif." },
    { "en": "Apa Itu Fluxgate Magnetometer?", "id": "Sensor Medan Magnet DC Sensitif." },
    { "en": "Apa Itu Sensor Posisi Magnetostriktif?", "id": "Sensor Posisi Linear Akurasi Tinggi." },
    { "en": "Apa Itu Encoder Magnetik?", "id": "Sensor Posisi Menggunakan Medan Magnet." },
    { "en": "Apa Itu Saklar Efek Hall?", "id": "Saklar Digital Berbasis Efek Hall." },
    { "en": "Apa Itu Sensor Sudut?", "id": "Mengukur Orientasi Atau Kemiringan." },
    { "en": "Apa Itu Inclinometer?", "id": "Mengukur Sudut Kemiringan Relatif Gravitasi." },
    { "en": "Apa Itu Tilt Switch?", "id": "Saklar Yang Aktif Saat Miring." },
    { "en": "Apa Itu Kompas?", "id": "Menentukan Arah Mata Angin." },
    { "en": "Apa Itu AHRS (Attitude and Heading Reference System)?", "id": "Memberikan Informasi Orientasi 3D." },
    { "en": "Apa Itu Sensor Kelembaban Udara?", "id": "Hygrometer." },
    { "en": "Apa Itu Kelembaban Absolut?", "id": "Massa Uap Air Per Volume Udara." },
    { "en": "Apa Itu Kelembaban Relatif?", "id": "Rasio Uap Air Aktual Per Maksimum." },
    { "en": "Apa Itu Titik Embun (Dew Point)?", "id": "Suhu Saat Embun Mulai Terbentuk." },
    { "en": "Apa Itu Psikrometer?", "id": "Mengukur Kelembaban Dengan Termometer Basah-Kering." },
    { "en": "Apa Itu Sensor Optik?", "id": "Bekerja Berdasarkan Prinsip Optik." },
    { "en": "Apa Itu Interferometer?", "id": "Mengukur Perubahan Jarak Sangat Kecil." },
    { "en": "Apa Itu Spektrometer?", "id": "Mengukur Spektrum Cahaya." },
    { "en": "Apa Itu Kolorimeter?", "id": "Mengukur Absorbansi Cahaya." },
    { "en": "Apa Itu Opasitas Meter?", "id": "Mengukur Seberapa Buram Suatu Medium." },
    { "en": "Apa Itu Sensor Hamburan Cahaya?", "id": "Mengukur Partikel Di Udara Atau Cairan." },
    { "en": "Apa Itu Sensor Asap Fotoelektrik?", "id": "Mendeteksi Asap Dengan Hamburan Cahaya." },
    { "en": "Apa Itu Sensor Asap Ionisasi?", "id": "Mendeteksi Asap Dengan Kamar Ionisasi." },
    { "en": "Apa Itu Sensor Debu Optik?", "id": "Mengukur Konsentrasi Partikel Debu." },
    { "en": "Apa Itu Polarimeter?", "id": "Mengukur Rotasi Polarisasi Cahaya." },
    { "en": "Apa Itu Refraktometer?", "id": "Mengukur Indeks Bias Cairan." },
    { "en": "Apa Itu Sensor Kimia?", "id": "Mendeteksi Kehadiran Zat Kimia." },
    { "en": "Apa Itu Elektroda Selektif Ion?", "id": "ISE (Ion-Selective Electrode)." },
    { "en": "Apa Itu Sensor Oksida Logam?", "id": "MOS (Metal Oxide Semiconductor) Gas Sensor." },
    { "en": "Apa Itu Sensor Inframerah Non-dispersif?", "id": "NDIR (Nondispersive Infrared) Sensor." },
    { "en": "Untuk Apa Sensor NDIR (Nondispersive Infrared)?", "id": "Mengukur Konsentrasi Gas (CO2)." },
    { "en": "Apa Itu Sensor Elektrokimia?", "id": "Mengukur Gas Beracun (CO, H2S)." },
    { "en": "Apa Itu Sensor Katalitik?", "id": "Mendeteksi Gas Mudah Terbakar." },
    { "en": "Apa Itu Sensor Kelembaban Resistif?", "id": "Resistansi Berubah Sesuai Kelembaban." },
    { "en": "Apa Itu Sensor Kelembaban Kapasitif?", "id": "Kapasitansi Berubah Sesuai Kelembaban." },
    { "en": "Apa Itu Sensor Gelombang Akustik Permukaan?", "id": "SAW (Surface Acoustic Wave) Sensor." },
    { "en": "Apa Itu Mikro-keseimbangan Kristal Kuarsa?", "id": "QCM (Quartz Crystal Microbalance)." },
    { "en": "Apa Fungsi QCM (Quartz Crystal Microbalance)?", "id": "Mengukur Perubahan Massa Sangat Kecil." },
    { "en": "Apa Itu Transduser Biomedis?", "id": "Sensor Untuk Aplikasi Medis." },
    { "en": "Apa Itu Elektroda?", "id": "Konduktor Untuk Kontak Dengan Tubuh." },
    { "en": "Apa Itu Elektroda EEG (Electroencephalography)?", "id": "Mengukur Aktivitas Listrik Otak." },
    { "en": "Apa Itu Elektroda ECG (Electrocardiography)?", "id": "Mengukur Aktivitas Listrik Jantung." },
    { "en": "Apa Itu Elektroda EMG (Electromyography)?", "id": "Mengukur Aktivitas Listrik Otot." },
    { "en": "Apa Itu Sensor Laju Pernapasan?", "id": "Mengukur Frekuensi Bernapas." },
    { "en": "Apa Itu Sensor Konduktivitas Kulit?", "id": "GSR (Galvanic Skin Response) Sensor." },
    { "en": "Apa Sensitivitas Sensor Terhadap Besaran Yang Bukan Target?", "id": "Sensitivitas Silang (Cross-Sensitivity)." },
    { "en": "Apa Kondisi Saat Output Sensor Tidak Naik Lagi?", "id": "Saturasi (Saturation)." },
    { "en": "Apa Kemampuan Sensor Kembali Ke Output Awal?", "id": "Repeatability." },
    { "en": "Apa Sensor Yang Mengukur Viskositas?", "id": "Viscometer." },
    { "en": "Apa Sensor Yang Mengukur Kepadatan Cairan?", "id": "Density Sensor." },
    { "en": "Apa Sensor Suara Untuk Penggunaan Di Bawah Air?", "id": "Hidrofon (Hydrophone)." },
    { "en": "Apa Sensor Getaran Untuk Survei Seismik?", "id": "Geofon (Geophone)." },
    { "en": "Apa Transduser Yang Berubah Bentuk Saat Dipanaskan?", "id": "SMA (Shape-Memory Alloy)." },
    { "en": "Apa Polimer Yang Berubah Bentuk Karena Medan Listrik?", "id": "EAP (Electroactive Polymer)." },
    { "en": "Apa Motor Yang Bergerak Karena Getaran Ultrasonik?", "id": "Motor Ultrasonik." },
    { "en": "Apa Sensor Kelembaban Berbasis Titik Embun?", "id": "Chilled Mirror Hygrometer." },
    { "en": "Apa Sensor Gas Berbasis Penyerapan Inframerah?", "id": "NDIR (Nondispersive Infrared) Sensor." },
    { "en": "Apa Sensor Gas Berbasis Sel Elektrokimia?", "id": "Electrochemical Gas Sensor." },
    { "en": "Apa Sensor Gas Yang Mendeteksi Gas Mudah Terbakar?", "id": "Pellistor (Catalytic Bead Sensor)." },
    { "en": "Apa Transistor Yang Sensitif Terhadap Ion?", "id": "ISFET (Ion-Sensitive Field-Effect Transistor)." },
    { "en": "Apa Sensor Untuk Mengukur Glukosa Darah?", "id": "Glukometer (Glucometer)." },
    { "en": "Apa Sensor Yang Mendeteksi Pelepasan Akustik?", "id": "Acoustic Emission Sensor." },
    { "en": "Apa Aktuator Yang Digunakan Di Hard Drive?", "id": "VCM (Voice Coil Motor)." },
    { "en": "Apa Transduser Untuk Membelokkan Cermin Sangat Cepat?", "id": "Galvanometer." },
    { "en": "Apa Penguat Untuk Sensor Piezoelektrik?", "id": "Penguat Muatan (Charge Amplifier)." },
    { "en": "Apa Sirkuit Untuk Demodulasi Sinyal LVDT?", "id": "Synchronous Demodulator." },
    { "en": "Apa Sensor Radiasi Yang Berpendar?", "id": "Scintillation Detector." },
    { "en": "Apa Alat Untuk Mengukur Dosis Radiasi?", "id": "Dosimeter." },
    { "en": "Apa Flowmeter Berbasis Efek Vortex?", "id": "Vortex Flowmeter." },
    { "en": "Apa Flowmeter Berbasis Perpindahan Panas?", "id": "Thermal Mass Flowmeter." },
    { "en": "Apa Arsitektur Sensor Cerdas?", "id": "Sensor, ADC (Analog-to-Digital Converter), Prosesor, Antarmuka." },
    { "en": "Apa Kemampuan Sensor Mengkalibrasi Diri?", "id": "Self-Calibration." },
    { "en": "Apa Kemampuan Sensor Mendiagnosis Diri?", "id": "Self-Diagnosis." },
    { "en": "Apa Material Keramik Umum Untuk Piezoelektrik?", "id": "PZT (Lead Zirconate Titanate)." },
    { "en": "Apa Material Untuk RTD Presisi Tinggi?", "id": "Platina (Platinum)." },
    { "en": "Apa Material Untuk Termokopel Suhu Tinggi?", "id": "Tipe K (Chromel-Alumel)." },
    { "en": "Apa Jaringan Sensor Yang Mengatur Diri?", "id": "Self-Organizing Sensor Network." },
    { "en": "Apa Topologi Jaringan Sensor?", "id": "Star, Mesh, Tree." },
    { "en": "Apa Sensor Yang Dapat Dimakan?", "id": "Edible Sensor." },
    { "en": "Apa Sensor Yang Dicetak Pada Substrat Fleksibel?", "id": "Printed Sensor." },
    { "en": "Apa Sensor Yang Ditenagai Oleh Energi Sekitar?", "id": "Self-Powered Sensor." },
    { "en": "Apa Sumber Energi Untuk Pemanenan Energi?", "id": "Getaran, Panas, Cahaya, RF." },
    { "en": "Apa Efek Fotokonduktif?", "id": "Konduktivitas Berubah Karena Cahaya." },
    { "en": "Apa Efek Fotovoltaik?", "id": "Menghasilkan Tegangan Karena Cahaya." },
    { "en": "Apa Efek Piroelektrik?", "id": "Menghasilkan Tegangan Karena Perubahan Suhu." },
    { "en": "Apa Transduser Untuk Mengukur Aliran Darah?", "id": "Doppler Flowmeter." },
    { "en": "Apa Transduser Untuk Terapi Fisik?", "id": "Ultrasound Transducer." },
    { "en": "Apa Sensor Yang Mengukur Kandungan Alkohol Napas?", "id": "Breathalyzer." },
    { "en": "Apa Sensor Ketinggian Berbasis Radar?", "id": "Radar Altimeter." },
    { "en": "Apa Sensor Yang Mendeteksi Retakan Logam?", "id": "Eddy-Current Sensor." },
    { "en": "Apa Sensor Posisi Berbasis Resolver?", "id": "Resolver." },
    { "en": "Apa Sensor Posisi Berbasis Synchro?", "id": "Synchro." },
    { "en": "Apa Itu Kompensasi Kesalahan Sensor?", "id": "Menggunakan Algoritma Untuk Memperbaiki Bacaan." },
    { "en": "Apa Itu Tabel Pencarian (Lookup Table)?", "id": "Tabel Untuk Koreksi Dan Linearisasi." },
    { "en": "Apa Itu Interpolasi?", "id": "Memperkirakan Nilai Di Antara Titik Data." },
    { "en": "Apa Itu Ekstrapolasi?", "id": "Memperkirakan Nilai Di Luar Titik Data." },
    { "en": "Apa Sensor Dengan Output Frekuensi?", "id": "Outputnya Berupa Sinyal Frekuensi." },
    { "en": "Apa Keuntungan Output Frekuensi?", "id": "Sangat Tahan Derau (Noise)." },
    { "en": "Apa Sirkuit Pengubah Tegangan-ke-Frekuensi?", "id": "VFC (Voltage-to-Frequency Converter)." },
    { "en": "Apa Sirkuit Pengubah Frekuensi-ke-Tegangan?", "id": "FVC (Frequency-to-Voltage Converter)." },
    { "en": "Apa Itu Transduser Resonansi?", "id": "Bekerja Pada Frekuensi Resonansinya." },
    { "en": "Contoh Transduser Resonansi?", "id": "Kristal Kuarsa, Resonator MEMS." },
    { "en": "Apa Faktor Kualitas (Q Factor) Sensor?", "id": "Ukuran Ketajaman Resonansi." },
    { "en": "Apa Itu Penyeteman (Tuning)?", "id": "Menyesuaikan Frekuensi Resonansi Sensor." },
    { "en": "Apa Itu Sensor Paparan Radiasi?", "id": "Dosimeter." },
    { "en": "Apa Jenis Dosimeter?", "id": "TLD, Film Badge, Kamar Ionisasi." },
    { "en": "Apa Itu Detektor Sintilasi?", "id": "Mengubah Radiasi Menjadi Kilatan Cahaya." },
    { "en": "Apa Tabung Pengganda Foton?", "id": "PMT (Photomultiplier Tube)." },
    { "en": "Apa Fungsi PMT (Photomultiplier Tube)?", "id": "Menguatkan Sinyal Cahaya Sangat Lemah." },
    { "en": "Apa Detektor Semikonduktor?", "id": "Mendeteksi Radiasi Dengan Semikonduktor." },
    { "en": "Apa Sensor Yang Mengukur Getaran Tanah?", "id": "Seismometer." },
    { "en": "Apa Sensor Yang Mendeteksi Gelombang Gravitasi?", "id": "Interferometer Laser." },
    { "en": "Apa Transduser Yang Mendinginkan Atau Memanaskan?", "id": "TEC (Thermoelectric Cooler)." },
    { "en": "Apa Sensor Yang Mengukur Arus AC/DC?", "id": "Hall-Effect Based Current Sensor." },
    { "en": "Apa Sensor Yang Mengukur Tegangan AC/DC?", "id": "Voltage Sensor." },
    { "en": "Apa Pembagi Tegangan (Voltage Divider)?", "id": "Mengurangi Tegangan Untuk Pengukuran." },
    { "en": "Apa Sensor Yang Mengukur Daya Listrik?", "id": "Power Meter." },
    { "en": "Apa Sensor Yang Mengukur Energi Listrik?", "id": "Energy Meter (kWh Meter)." },
    { "en": "Apa Itu Pengambilan Sampel (Sampling)?", "id": "Mengukur Sinyal Pada Interval Waktu." },
    { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Laju Sampling Rendah." },
    { "en": "Apa Itu Filter Anti-Aliasing?", "id": "LPF (Low-Pass Filter) Sebelum ADC." },
    { "en": "Apa Itu Kuantisasi?", "id": "Membulatkan Nilai Sampel." },
    { "en": "Apa Itu Derau Kuantisasi?", "id": "Kesalahan Akibat Pembulatan." },
    { "en": "Apa Itu Protokol I2C (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Dua Kawat." },
    { "en": "Apa Itu Protokol SPI (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Serial Cepat." },
    { "en": "Apa Itu UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial Asinkron." },
    { "en": "Apa Itu CAN (Controller Area Network) Bus?", "id": "Protokol Komunikasi Untuk Otomotif." },
    { "en": "Apa Sensor Yang Dapat Diprogram?", "id": "Sensor Dengan Pengaturan Internal." },
    { "en": "Apa Sensor Virtual?", "id": "Estimasi Besaran Dari Sensor Lain." },
    { "en": "Apa Itu Rentang Dinamis Bebas Spurious?", "id": "SFDR (Spurious-Free Dynamic Range)." },
    { "en": "Apa Itu THD (Total Harmonic Distortion)?", "id": "Ukuran Distorsi Harmonik." },
    { "en": "Apa Itu IMD (Intermodulation Distortion)?", "id": "Distorsi Akibat Interaksi Sinyal." },
    { "en": "Apa Sensor Dengan Kompensasi Digital?", "id": "Koreksi Dilakukan Secara Digital." },
    { "en": "Apa Sensor Dengan Output Digital?", "id": "Outputnya Berupa Data Biner." },
    { "en": "Apa Sensor Dengan Output Analog?", "id": "Outputnya Berupa Tegangan/Arus." },
    { "en": "Apa Sensor Dengan Output Pulsa?", "id": "Outputnya Berupa Sinyal Pulsa." },
    { "en": "Apa Transduser Yang Bergetar?", "id": "Vibrator." },
    { "en": "Apa Transduser Yang Berputar?", "id": "Motor." },
    { "en": "Apa Transduser Yang Mendorong/Menarik?", "id": "Solenoida." },
    { "en": "Apa Transduser Yang Memanaskan?", "id": "Elemen Pemanas." },
    { "en": "Apa Transduser Yang Mendinginkan?", "id": "Elemen Peltier." },
    { "en": "Apa Transduser Yang Menghasilkan Cahaya?", "id": "LED (Light-Emitting Diode)." },
    { "en": "Apa Transduser Yang Menghasilkan Suara Frekuensi Tinggi?", "id": "Transduser Ultrasonik." },
    { "en": "Apa Itu Nol Absolut?", "id": "Suhu Terendah Yang Mungkin (0 Kelvin)." },
    { "en": "Apa Sensor Suhu Yang Menggunakan Radiasi Benda Hitam?", "id": "Pirometer Inframerah." },
    { "en": "Apa Material Dengan Resistivitas Hampir Nol?", "id": "Superkonduktor." },
    { "en": "Apa Sensor Suhu Kriogenik?", "id": "Sensor Untuk Suhu Sangat Rendah." },
    { "en": "Apa Itu Dioda Silikon Sebagai Sensor Suhu?", "id": "Tegangan Maju Berubah Dengan Suhu." },
    { "en": "Apa Transduser Yang Menghasilkan Cahaya Koheren?", "id": "Dioda Laser." },
    { "en": "Apa Transduser Yang Mengubah Sudut Polarisasi?", "id": "Sel Kerr Atau Pockels." },
    { "en": "Apa Sensor Yang Mendeteksi Polarisasi Cahaya?", "id": "Polarimeter." },
    { "en": "Apa Sensor Kecepatan Berbasis Laser Doppler?", "id": "Laser Doppler Velocimeter (LDV)." },
    { "en": "Apa Sensor Getaran Berbasis Laser?", "id": "Laser Doppler Vibrometer." },
    { "en": "Apa Sensor Citra Untuk Cahaya Rendah?", "id": "EMCCD (Electron-Multiplying CCD)." },
    { "en": "Apa Sensor Citra Untuk Inframerah?", "id": "Sensor Termal (Mikrobolometer)." },
    { "en": "Apa Itu Mikrobolometer?", "id": "Sensor IR (Infra Red) Tanpa Pendingin." },
    { "en": "Apa Sensor Citra Untuk Sinar-X?", "id": "Detektor Panel Datar (Flat-Panel Detector)." },
    { "en": "Apa Sensor Yang Mendeteksi Satu Foton?", "id": "Single-Photon Avalanche Diode (SPAD)." },
    { "en": "Apa Sensor Yang Mengukur Tekanan Suara?", "id": "Sound Level Meter." },
    { "en": "Apa Jaringan Pemberat (Weighting Network) Untuk Suara?", "id": "Kurva A, C, Z-Weighting." },
    { "en": "Apa Kurva Yang Meniru Pendengaran Manusia?", "id": "A-Weighting." },
    { "en": "Apa Sensor Untuk Analisis Spektrum Suara?", "id": "Real-Time Analyzer (RTA)." },
    { "en": "Apa Sensor Yang Mendeteksi Korosi?", "id": "Corrosion Sensor." },
    { "en": "Apa Sensor Yang Mengukur Ketegangan Kabel?", "id": "Tensiometer." },
    { "en": "Apa Sensor Untuk Memantau Kesehatan Struktural?", "id": "SHM (Structural Health Monitoring) Sensor." },
    { "en": "Apa Sensor Berbasis Kisi Serat Bragg?", "id": "FBG (Fiber Bragg Grating) Sensor." },
    { "en": "Besaran Apa Yang Dapat Diukur FBG?", "id": "Regangan, Suhu, Tekanan." },
    { "en": "Apa Sensor Berbasis Hamburan Raman?", "id": "DTS (Distributed Temperature Sensing)." },
    { "en": "Apa Sensor Berbasis Hamburan Brillouin?", "id": "DSS (Distributed Strain Sensing)." },
    { "en": "Apa Itu Sensor Terdistribusi?", "id": "Mengukur Di Sepanjang Jalur Serat." },
    { "en": "Apa Itu Sensor Titik?", "id": "Mengukur Hanya Di Satu Lokasi." },
    { "en": "Apa Itu Antarmuka Sensor Digital?", "id": "Menghasilkan Output Data Digital Langsung." },
    { "en": "Apa Contoh Antarmuka Sensor Digital?", "id": "I2C, SPI, 1-Wire." },
    { "en": "Apa Itu Standar IEEE 1451?", "id": "Standar Untuk Transduser Cerdas." },
    { "en": "Apa Itu Lembar Data Deskripsi Elektronik?", "id": "TEDS (Transducer Electronic Data Sheet)." },
    { "en": "Apa Fungsi TEDS (Transducer Electronic Data Sheet)?", "id": "Menyimpan Informasi Identitas Sensor." },
    { "en": "Apa Itu Sensor Plug-and-Play?", "id": "Sensor Yang Dikenali Sistem Otomatis." },
    { "en": "Apa Itu Skala Penuh (Full Scale)?", "id": "Rentang Operasi Maksimum Sensor." },
    { "en": "Apa Itu Kesalahan Skala Penuh?", "id": "Kesalahan Dinyatakan Sebagai Persentase." },
    { "en": "Apa Itu Kesalahan Non-Linearitas?", "id": "Penyimpangan Dari Garis Lurus Ideal." },
    { "en": "Apa Itu Kesalahan Histeresis?", "id": "Perbedaan Output Saat Naik-Turun." },
    { "en": "Apa Itu Kesalahan Pengulangan?", "id": "Ketidakmampuan Memberi Output Sama." },
    { "en": "Apa Itu Koefisien Suhu?", "id": "Seberapa Besar Output Berubah Dengan Suhu." },
    { "en": "Apa Itu Koefisien Suhu Span?", "id": "Perubahan Span Dengan Suhu." },
    { "en": "Apa Itu Koefisien Suhu Offset?", "id": "Perubahan Offset Dengan Suhu." },
    { "en": "Apa Itu Pemanasan (Warm-up Time)?", "id": "Waktu Sensor Mencapai Kestabilan." },
    { "en": "Apa Itu Umur Operasi (Operating Life)?", "id": "Estimasi Masa Pakai Sensor." },
    { "en": "Apa Itu Ketahanan Getaran?", "id": "Kemampuan Bertahan Terhadap Getaran." },
    { "en": "Apa Itu Ketahanan Guncangan?", "id": "Kemampuan Bertahan Terhadap Benturan." },
    { "en": "Apa Itu Peringkat IP (Ingress Protection)?", "id": "Tingkat Proteksi Terhadap Debu-Air." },
    { "en": "Apa Itu Peringkat NEMA?", "id": "Standar Selungkup (Enclosure) Amerika." },
    { "en": "Apa Itu Keamanan Intrinsik?", "id": "Desain Aman Untuk Area Berbahaya." },
    { "en": "Apa Itu Selungkup Tahan Ledakan?", "id": "Mencegah Ledakan Internal Menyebar." },
    { "en": "Apa Itu Rasio Penolakan Mode Umum?", "id": "CMRR (Common-Mode Rejection Ratio)." },
    { "en": "Apa Itu Rasio Penolakan Catu Daya?", "id": "PSRR (Power Supply Rejection Ratio)." },
    { "en": "Apa Sirkuit Untuk Menyeimbangkan Jembatan Sensor?", "id": "Sirkuit Penyeimbang Jembatan." },
    { "en": "Apa Penguat Untuk Sinyal Frekuensi Sangat Rendah?", "id": "Chopper Amplifier." },
    { "en": "Apa Penguat Dengan Isolasi Listrik?", "id": "Isolation Amplifier." },
    { "en": "Apa Konverter Arus-ke-Tegangan?", "id": "Transimpedance Amplifier." },
    { "en": "Apa Konverter Tegangan-ke-Arus?", "id": "Transconductance Amplifier." },
    { "en": "Apa Sensor Yang Mengukur Posisi Katup?", "id": "Valve Position Sensor." },
    { "en": "Apa Sensor Yang Mengukur Kelembaban Minyak?", "id": "Oil Moisture Sensor." },
    { "en": "Apa Sensor Yang Mengukur Kualitas Udara?", "id": "Air Quality Sensor (VOC, PM2.5)." },
    { "en": "Apa Itu VOC (Volatile Organic Compound)?", "id": "Senyawa Organik Yang Mudah Menguap." },
    { "en": "Apa Itu PM2.5?", "id": "Partikel Materi Berdiameter < 2.5 Mikron." },
    { "en": "Apa Sensor Yang Mengukur Ketebalan Lapisan?", "id": "Coating Thickness Gauge." },
    { "en": "Apa Sensor Yang Mendeteksi Kebocoran Air?", "id": "Water Leakage Sensor." },
    { "en": "Apa Sensor Yang Mendeteksi Pembekuan?", "id": "Frost Sensor." },
    { "en": "Apa Sensor Yang Mengukur Indeks UV?", "id": "UV (Ultra Violet) Index Sensor." },
    { "en": "Apa Sensor Yang Mengukur Warna?", "id": "Colorimeter Sensor." },
    { "en": "Apa Sensor Yang Mengukur Tingkat Suara?", "id": "Sound Level Sensor." },
    { "en": "Apa Sensor Yang Mengukur Tekanan Ban?", "id": "TPMS (Tire Pressure Monitoring System)." },
    { "en": "Apa Sensor Yang Mengukur Posisi Pedal Gas?", "id": "Throttle Position Sensor (TPS)." },
    { "en": "Apa Sensor Yang Mengukur Aliran Udara Mesin?", "id": "MAF (Mass Airflow) Sensor." },
    { "en": "Apa Sensor Yang Mengukur Tekanan Udara Intake?", "id": "MAP (Manifold Absolute Pressure) Sensor." },
    { "en": "Apa Sensor Yang Mengukur Posisi Poros Engkol?", "id": "Crankshaft Position Sensor." },
    { "en": "Apa Sensor Yang Mengukur Posisi Poros Bubungan?", "id": "Camshaft Position Sensor." },
    { "en": "Apa Sensor Yang Mendeteksi Ketukan Mesin?", "id": "Knock Sensor." },
    { "en": "Apa Sensor Yang Mengukur Suhu Pendingin Mesin?", "id": "Engine Coolant Temperature (ECT) Sensor." },
    { "en": "Apa Sensor Yang Mengukur Suhu Udara Masuk?", "id": "Intake Air Temperature (IAT) Sensor." },
    { "en": "Apa Sensor Yang Mengukur Kandungan Oksigen Gas Buang?", "id": "Oxygen (O2) Sensor." },
    { "en": "Apa Sensor Yang Mengukur Kecepatan Roda?", "id": "Wheel Speed Sensor (Untuk ABS)." },
    { "en": "Apa Sensor Yang Mengukur Laju Yaw Kendaraan?", "id": "Yaw Rate Sensor (Untuk ESC)." },
    { "en": "Apa Sensor Yang Mengukur Sudut Kemudi?", "id": "Steering Angle Sensor." },
    { "en": "Apa Sensor Yang Mendeteksi Hujan Di Kaca Depan?", "id": "Rain Sensor." },
    { "en": "Apa Sensor Untuk Sistem Parkir?", "id": "Parking Sensor (Ultrasonic/Electromagnetic)." },
    { "en": "Apa Sensor Untuk Deteksi Titik Buta?", "id": "Blind Spot Sensor (Radar/Ultrasonic)." },
    { "en": "Apa Transduser Untuk Menggerakkan Injektor Bahan Bakar?", "id": "Fuel Injector Solenoid." },
    { "en": "Apa Transduser Untuk Menggerakkan Koil Pengapian?", "id": "Ignition Coil." },
    { "en": "Apa Transduser Untuk Mengontrol Katup EGR?", "id": "EGR (Exhaust Gas Recirculation) Valve." },
    { "en": "Apa Transduser Untuk Mengontrol Idle Mesin?", "id": "Idle Air Control (IAC) Valve." },
    { "en": "Apa Transduser Untuk Mengubah Posisi VVT?", "id": "VVT (Variable Valve Timing) Solenoid." },
    { "en": "Apa Itu Antarmuka Cerdas Untuk Sensor?", "id": "IO-Link." },
    { "en": "Apa Itu Sensor Vision?", "id": "Kamera Cerdas Dengan Pemrosesan Internal." },
    { "en": "Apa Itu Pengenalan Gestur?", "id": "Sensor Yang Mendeteksi Gerakan Tangan." },
    { "en": "Apa Itu Tomografi Proses?", "id": "Teknik Pencitraan Untuk Proses Industri." },
    { "en": "Apa Itu Tomografi Kapasitansi Listrik?", "id": "ECT (Electrical Capacitance Tomography)." },
    { "en": "Apa Itu Tomografi Impedansi Listrik?", "id": "EIT (Electrical Impedance Tomography)." },
    { "en": "Apa Itu Tomografi Resistivitas Listrik?", "id": "ERT (Electrical Resistivity Tomography)." },
    { "en": "Apa Sensor Yang Menggunakan Efek Termokromik?", "id": "Material Berubah Warna Karena Suhu." },
    { "en": "Apa Itu Efek Elektrokromik?", "id": "Material Berubah Warna Karena Listrik." },
    { "en": "Apa Itu Efek Fotokromik?", "id": "Material Berubah Warna Karena Cahaya." },
    { "en": "Apa Transduser Yang Menghasilkan Getaran Haptic?", "id": "Haptic Actuator." },
    { "en": "Jenis Haptic Actuator?", "id": "ERM (Eccentric Rotating Mass), LRA (Linear Resonant Actuator)." },
    { "en": "Apa Itu Aktuator Massa Berputar Eksentrik?", "id": "ERM (Eccentric Rotating Mass)." },
    { "en": "Apa Itu Aktuator Resonansi Linear?", "id": "LRA (Linear Resonant Actuator)." },
    { "en": "Apa Itu Pembaca Kode Batang 2D?", "id": "2D Barcode Imager." },
    { "en": "Apa Beda Scanner Dan Imager?", "id": "Imager Mengambil Gambar, Scanner Memindai Garis." },
    { "en": "Apa Itu Sensor Interferometrik?", "id": "Menggunakan Interferensi Cahaya Untuk Pengukuran." },
    { "en": "Apa Itu Giroskop Serat Optik?", "id": "FOG (Fiber Optic Gyroscope)." },
    { "en": "Apa Prinsip Kerja FOG (Fiber Optic Gyroscope)?", "id": "Efek Sagnac." },
    { "en": "Apa Itu Giroskop Cincin Laser?", "id": "RLG (Ring Laser Gyroscope)." },
    { "en": "Apa Itu Sensor Posisi Optik?", "id": "PSD (Position Sensitive Detector)." },
    { "en": "Apa Itu Sensor Spektral?", "id": "Mengukur Intensitas Cahaya Per Panjang Gelombang." },
    { "en": "Apa Itu Sensor Hiperspektral?", "id": "Mengambil Gambar Dalam Banyak Pita Spektral." },
    { "en": "Apa Itu Sensor Multispektral?", "id": "Mengambil Gambar Dalam Beberapa Pita Spektral." },
    { "en": "Apa Itu Bolometer?", "id": "Mengukur Energi Radiasi Elektromagnetik." },
    { "en": "Apa Itu Termopile?", "id": "Beberapa Termokopel Terhubung Seri." },
    { "en": "Apa Fungsi Termopile?", "id": "Sensor Suhu Inframerah Non-kontak." },
    { "en": "Apa Itu Sensor Kecepatan Udara?", "id": "Airspeed Sensor (Menggunakan Tabung Pitot)." },
    { "en": "Apa Itu Sensor Sudut Serang?", "id": "Angle of Attack (AOA) Sensor." },
    { "en": "Apa Itu Sensor Kelembaban Optik?", "id": "Mengukur Penyerapan Cahaya Oleh Uap Air." },
    { "en": "Apa Itu Sensor Efek Fotorefraktif?", "id": "Indeks Bias Berubah Karena Cahaya." },
    { "en": "Apa Itu Sensor Berbasis Metamaterial?", "id": "Sensor Dengan Sensitivitas Sangat Tinggi." },
    { "en": "Apa Itu Sensor Gelombang Permukaan?", "id": "Mendeteksi Perubahan Pada Permukaan." },
    { "en": "Apa Itu Resonator Mikro-elektromekanis?", "id": "Digunakan Dalam Sensor Resonansi." },
    { "en": "Apa Itu Titik Referensi?", "id": "Nilai Dikenal Untuk Kalibrasi." },
    { "en": "Apa Itu Standar Transfer?", "id": "Perangkat Yang Digunakan Untuk Mengkalibrasi Lainnya." },
    { "en": "Apa Itu Ketertelusuran (Traceability)?", "id": "Kemampuan Melacak Kalibrasi Ke Standar Nasional." },
    { "en": "Apa Itu Ketidakpastian Pengukuran?", "id": "Rentang Keraguan Pada Hasil Pengukuran." },
    { "en": "Apa Itu Sensor Redundan?", "id": "Beberapa Sensor Mengukur Besaran Sama." },
    { "en": "Apa Itu Pemungutan Suara (Voting) Sensor?", "id": "Memilih Bacaan Mayoritas Dari Sensor Redundan." },
    { "en": "Apa Itu Rata-rata Sinyal?", "id": "Mengurangi Derau Dengan Merata-ratakan." },
    { "en": "Apa Itu Filtering Digital?", "id": "Memproses Sampel Sensor Secara Matematis." },
    { "en": "Apa Itu Antarmuka Cerdas?", "id": "Sensor Dengan Kemampuan Komunikasi Digital." },
    { "en": "Apa Itu Antarmuka Manusia-Mesin?", "id": "HMI (Human-Machine Interface)." },
    { "en": "Apa Itu Sensor Visual?", "id": "Kamera." },
    { "en": "Apa Itu Sensor Audio?", "id": "Mikrofon." },
    { "en": "Apa Itu Sensor Haptic?", "id": "Memberikan Umpan Balik Getaran Atau Gaya." },
    { "en": "Apa Itu Layar Sentuh Resistif?", "id": "Mendeteksi Tekanan Fisik." },
    { "en": "Apa Itu Layar Sentuh Kapasitif?", "id": "Mendeteksi Gangguan Medan Listrik." },
    { "en": "Apa Itu Layar Sentuh Gelombang Akustik Permukaan?", "id": "SAW (Surface Acoustic Wave) Touchscreen." },
    { "en": "Apa Itu Layar Sentuh Inframerah?", "id": "Mendeteksi Objek Yang Menghalangi Kisi IR." },
    { "en": "Apa Itu Sensor Gaya 3D?", "id": "Mengukur Gaya Dalam Tiga Dimensi." },
    { "en": "Apa Itu Sensor Gerakan?", "id": "Mendeteksi Gerakan Fisik." },
    { "en": "Apa Itu Sensor Okupansi?", "id": "Mendeteksi Kehadiran Orang Di Ruangan." },
    { "en": "Apa Itu Tombol Tekan?", "id": "Sensor Gaya Biner." },
    { "en": "Apa Itu Saklar?", "id": "Sensor Posisi Biner." },
    { "en": "Apa Itu Joystick?", "id": "Sensor Posisi Dua Dimensi." },
    { "en": "Apa Itu Trackball?", "id": "Sensor Gerak Relatif." },
    { "en": "Apa Itu Touchpad?", "id": "Sensor Posisi Relatif Kapasitif." },
    { "en": "Apa Itu Pena Digital (Stylus)?", "id": "Sensor Posisi Absolut." },
    { "en": "Apa Itu Digitizer?", "id": "Tablet Grafis." },
    { "en": "Apa Itu Pemindai (Scanner)?", "id": "Mengubah Dokumen Fisik Menjadi Digital." },
    { "en": "Apa Itu Pembaca Kartu Magnetik?", "id": "Membaca Data Dari Garis Magnetik." },
    { "en": "Apa Itu Pembaca Kode Batang?", "id": "Membaca Pola Garis Hitam Putih." },
    { "en": "Apa Itu Pembaca Kode QR?", "id": "Membaca Matriks Data 2D." },
    { "en": "Apa Itu Pengenalan Karakter Tinta Magnetik?", "id": "MICR (Magnetic Ink Character Recognition)." },
    { "en": "Apa Itu Pengenalan Tanda Optik?", "id": "OMR (Optical Mark Recognition)." },
    { "en": "Apa Itu Pengenalan Tulisan Tangan?", "id": "HCR (Handwriting Character Recognition)." },
    { "en": "Apa Itu Pengenalan Ucapan?", "id": "Speech Recognition." },
    { "en": "Apa Itu Verifikasi Pembicara?", "id": "Mengenali Siapa Yang Berbicara." },
    { "en": "Apa Itu Sensor Elektro-optik?", "id": "Mengubah Sinar Optik Menjadi Listrik." },
    { "en": "Apa Itu Sensor Elektrokimia?", "id": "Mengukur Reaksi Kimia." },
    { "en": "Apa Itu Sensor Elektromekanis?", "id": "Mengubah Energi Mekanis-Listrik." },
    { "en": "Apa Itu Transduser Bolak-balik?", "id": "Dapat Berfungsi Sebagai Sensor Dan Aktuator." },
    { "en": "Apa Itu Linearitas?", "id": "Hubungan Lurus Antara Input Dan Output." },
    { "en": "Apa Itu Histeresis?", "id": "Ketergantungan Output Pada Sejarah Input." },
    { "en": "Apa Itu Sensitivitas Sumbu Silang?", "id": "Sensitivitas Terhadap Input Yang Tidak Diinginkan." },
    { "en": "Apa Itu Rentang Frekuensi?", "id": "Frekuensi Operasi Sensor." },
    { "en": "Apa Itu Sinyal Modulasi?", "id": "Sinyal Informasi." },
    { "en": "Apa Itu Sinyal Pembawa?", "id": "Sinyal Frekuensi Tinggi." },
    { "en": "Apa Itu Demodulator?", "id": "Mengekstrak Sinyal Informasi." },
    { "en": "Apa Itu Sirkuit Jembatan?", "id": "Mengukur Perubahan Impedansi Kecil." },
    { "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Selisih Dua Sinyal." },
    { "en": "Apa Itu Sirkuit Pengkondisi Sinyal?", "id": "SCU (Signal Conditioning Unit)." },
    { "en": "Apa Itu Penyaringan (Filtering)?", "id": "Menghilangkan Komponen Frekuensi Tak Diinginkan." },
    { "en": "Apa Itu Perataan (Averaging)?", "id": "Mengurangi Derau Dengan Rata-rata." },
    { "en": "Apa Itu Korelasi?", "id": "Mengukur Kemiripan Antara Sinyal." },
    { "en": "Apa Itu Antarmuka Sensor?", "id": "Sirkuit Yang Menghubungkan Sensor Ke Sistem." },
    { "en": "Apa Itu Protokol Sensor?", "id": "Aturan Komunikasi Untuk Sensor." },
    { "en": "Apa Itu Sensor Dengan Output Tegangan?", "id": "Outputnya Berupa Sinyal Tegangan." },
    { "en": "Apa Itu Sensor Dengan Output Arus?", "id": "Outputnya Berupa Sinyal Arus." },
    { "en": "Apa Itu Sensor Dengan Output Resistansi?", "id": "Outputnya Berupa Perubahan Resistansi." },
    { "en": "Apa Itu Sensor Dengan Output Kapasitansi?", "id": "Outputnya Berupa Perubahan Kapasitansi." },
    { "en": "Apa Itu Sensor Dengan Output Induktansi?", "id": "Outputnya Berupa Perubahan Induktansi." },
    { "en": "Apa Itu Sensor Dengan Output Frekuensi?", "id": "Outputnya Berupa Sinyal Frekuensi." },
    { "en": "Apa Itu Sensor Dengan Output Digital?", "id": "Outputnya Berupa Data Digital." },
    { "en": "Apa Itu Sensor Suhu Termal?", "id": "Termokopel, RTD, Termistor." },
    { "en": "Apa Itu Sensor Suhu Optik?", "id": "Pirometer, FBG (Fiber Bragg Grating)." },
    { "en": "Apa Itu Sensor Tekanan Mekanis?", "id": "Tabung Bourdon, Diafragma." },
    { "en": "Apa Itu Sensor Tekanan Elektronik?", "id": "Piezoresistif, Kapasitif." },
    { "en": "Apa Itu Sensor Aliran Mekanis?", "id": "Turbin, Rotameter." },
    { "en": "Apa Itu Sensor Aliran Elektronik?", "id": "Ultrasonik, Elektromagnetik, Termal." },
    { "en": "Apa Itu Transduser Gaya?", "id": "Load Cell." },
    { "en": "Apa Itu Transduser Perpindahan?", "id": "LVDT (Linear Variable Differential Transformer)." },
    { "en": "Apa Itu Transduser Percepatan?", "id": "Akselerometer." },
    { "en": "Apa Itu Transduser Kecepatan?", "id": "Tachometer." },
    { "en": "Apa Itu Transduser Akustik?", "id": "Mikrofon, Speaker, Hidrofon." },
    { "en": "Apa Itu Transduser Optoelektronik?", "id": "LED (Light-Emitting Diode), Fotodioda." },
    { "en": "Apa Itu Sensor Efek Fotokonduktif?", "id": "LDR (Light Dependent Resistor)." },
    { "en": "Apa Itu Sensor Efek Fotovoltaik?", "id": "Sel Surya, Fotodioda." },
    { "en": "Apa Itu Sensor Termoelektrik?", "id": "Termokopel." },
    { "en": "Apa Itu Sensor Piroelektrik?", "id": "Sensor PIR (Passive Infrared)." },
    { "en": "Apa Itu Sensor Piezoelektrik?", "id": "Kristal Kuarsa, PZT (Lead Zirconate Titanate)." },
    { "en": "Apa Itu Sensor Piezoresistif?", "id": "Strain Gauge Semikonduktor." },
    { "en": "Apa Itu Sensor Magnetoresistif?", "id": "Sensor GMR (Giant Magnetoresistance)." },
    { "en": "Apa Itu Sensor Magnetostriktif?", "id": "Sensor Posisi Linear." },
    { "en": "Apa Itu Sensor Efek Hall?", "id": "Mendeteksi Medan Magnet." },
    { "en": "Apa Itu Sensor Serat Optik?", "id": "Menggunakan Cahaya Dalam Serat." },
    { "en": "Apa Itu Sensor Kimia?", "id": "Mendeteksi Zat Kimia." },
    { "en": "Apa Itu Sensor Biologis?", "id": "Biosensor." },
    { "en": "Apa Itu Sensor Kelembaban?", "id": "Hygrometer." },
    { "en": "Apa Itu Sensor Tekanan?", "id": "Pressure Gauge." },
    { "en": "Apa Itu Sensor Vakum?", "id": "Vacuum Gauge (Pirani, Penning)." },
    { "en": "Apa Itu Pengukur Pirani?", "id": "Sensor Vakum Berbasis Konduktivitas Termal." },
    { "en": "Apa Itu Pengukur Penning?", "id": "Sensor Vakum Berbasis Ionisasi." },
    { "en": "Apa Itu Sensor Radiasi?", "id": "Dosimeter, Scintillator." },
    { "en": "Apa Itu Sensor Getaran?", "id": "Vibration Sensor." },
    { "en": "Apa Itu Sensor Kemiringan?", "id": "Inclinometer, Tilt Sensor." },
    { "en": "Apa Itu Sensor Magnetik?", "id": "Kompas, Magnetometer." },
    { "en": "Apa Itu Sensor Optik?", "id": "Kamera, Sensor Warna." },
    { "en": "Apa Itu Sensor Suara?", "id": "Mikrofon." },
    { "en": "Apa Itu Sensor Gaya?", "id": "Force Sensor." },
    { "en": "Apa Itu Sensor Regangan?", "id": "Strain Sensor." },
    { "en": "Apa Itu Aktuator Listrik?", "id": "Motor, Solenoida." },
    { "en": "Apa Itu Aktuator Pneumatik?", "id": "Menggunakan Udara Bertekanan." },
    { "en": "Apa Itu Aktuator Hidrolik?", "id": "Menggunakan Cairan Bertekanan." },
    { "en": "Apa Itu Aktuator Termal?", "id": "Menggunakan Ekspansi Termal." },
    { "en": "Apa Itu Aktuator Cerdas?", "id": "Aktuator Dengan Kontroler Terintegrasi." },
    { "en": "Apa Itu Sensor Sudut Putar?", "id": "Rotary Encoder." },
    { "en": "Apa Itu Sensor Perpindahan Linear?", "id": "Linear Encoder." },
    { "en": "Apa Itu Sensor Induktif?", "id": "Mendeteksi Objek Logam." },
    { "en": "Apa Itu Sensor Kapasitif?", "id": "Mendeteksi Objek Apapun." },
    { "en": "Apa Itu Sensor Fotoelektrik?", "id": "Menggunakan Sinar Cahaya." },
    { "en": "Apa Itu Sensor Visi?", "id": "Kamera Untuk Inspeksi Otomatis." },
    { "en": "Apa Itu Sensor Jarak Laser?", "id": "Mengukur Jarak Dengan Akurasi Tinggi." },
    { "en": "Apa Itu Sensor Jarak Jauh?", "id": "Remote Sensing." },
    { "en": "Apa Itu Penginderaan Jauh Aktif?", "id": "Memancarkan Sinyal Sendiri (Radar, LiDAR)." },
    { "en": "Apa Itu Penginderaan Jauh Pasif?", "id": "Mendeteksi Radiasi Alami (Kamera)." },
    { "en": "Apa Itu Sensor Gravitasi?", "id": "Gravimeter." },
    { "en": "Apa Itu Giroskop MEMS?", "id": "Sensor Rotasi Mikro." },
    { "en": "Apa Itu Akselerometer MEMS?", "id": "Sensor Percepatan Mikro." },
    { "en": "Apa Itu Mikrofon MEMS?", "id": "Mikrofon Berbasis Silikon." },
    { "en": "Apa Itu Saklar RF MEMS?", "id": "Saklar Frekuensi Radio Mekanis Mikro." },
    { "en": "Apa Itu Osilator MEMS?", "id": "Resonator Mikro Untuk Pewaktuan." },
    { "en": "Apa Itu Sensor Tekanan MEMS?", "id": "Sensor Tekanan Miniatur." },
    { "en": "Apa Itu Tinta Elektronik?", "id": "E-Ink Display." },
    { "en": "Bagaimana E-Ink Bekerja?", "id": "Partikel Bermuatan Diatur Medan Listrik." },
    { "en": "Apa Itu Tampilan Bistabil?", "id": "Mempertahankan Gambar Tanpa Daya." },
    { "en": "Apa Itu Sensor Pencitraan Kontak?", "id": "CIS (Contact Image Sensor)." },
    { "en": "Di Mana CIS (Contact Image Sensor) Digunakan?", "id": "Pemindai Dokumen (Scanner)." },
    { "en": "Apa Itu Sensor CCD (Charge-Coupled Device)?", "id": "Sensor Gambar Sensitivitas Tinggi." },
    { "en": "Apa Itu Blooming (Pada CCD)?", "id": "Bocoran Muatan Ke Piksel Tetangga." },
    { "en": "Apa Itu Smear (Pada CCD)?", "id": "Artefak Vertikal Akibat Cahaya Terang." },
    { "en": "Apa Itu Filter Bayer?", "id": "Pola Filter Warna (RGGB)." },
    { "en": "Apa Itu Demosaicing?", "id": "Merekonstruksi Warna Dari Pola Bayer." },
    { "en": "Apa Itu White Balance?", "id": "Kompensasi Warna Sumber Cahaya." },
    { "en": "Apa Itu Shutter Global?", "id": "Merekam Semua Piksel Secara Bersamaan." },
    { "en": "Apa Itu Rolling Shutter?", "id": "Merekam Piksel Baris Demi Baris." },
    { "en": "Apa Artefak Dari Rolling Shutter?", "id": "Efek Jello Pada Gerakan Cepat." },
    { "en": "Apa Itu Sensor Time-of-Flight (ToF)?", "id": "Mengukur Jarak Berdasarkan Waktu Cahaya." },
    { "en": "Apa Itu Kamera Stereo?", "id": "Menggunakan Dua Lensa Untuk Persepsi 3D." },
    { "en": "Apa Itu Sensor LiDAR?", "id": "Memindai Lingkungan Dengan Laser." },
    { "en": "Apa Itu Sensor RADAR?", "id": "Menggunakan Gelombang Radio Untuk Deteksi." },
    { "en": "Apa Itu Sensor SONAR?", "id": "Menggunakan Gelombang Suara Untuk Deteksi." },
    { "en": "Apa Itu Sensor Efek Fotorefraktif?", "id": "Indeks Bias Berubah Karena Cahaya." },
    { "en": "Apa Itu Sensor Elektroluminesen?", "id": "Bahan Bercahaya Karena Medan Listrik." },
    { "en": "Apa Itu Sensor Termoluminesen?", "id": "Bahan Bercahaya Saat Dipanaskan." },
    { "en": "Apa Itu Sensor Kimiluminesen?", "id": "Bahan Bercahaya Dari Reaksi Kimia." },
    { "en": "Apa Itu Sensor Bioluminesen?", "id": "Cahaya Dihasilkan Organisme Hidup." },
    { "en": "Apa Itu Sensor Fluoresen?", "id": "Menyerap Dan Memancarkan Kembali Cahaya." },
    { "en": "Apa Itu Sensor Fosforesen?", "id": "Memancarkan Cahaya Perlahan (Glow-in-the-dark)." },
    { "en": "Apa Itu Sensor Galvanik?", "id": "Sensor Elektrokimia Yang Menghasilkan Arus." },
    { "en": "Apa Itu Sensor Potensiometrik?", "id": "Mengukur Potensial (Tegangan) Elektroda." },
    { "en": "Apa Itu Sensor Amperometrik?", "id": "Mengukur Arus Dari Reaksi Kimia." },
    { "en": "Apa Itu Sensor Konduktometrik?", "id": "Mengukur Perubahan Konduktivitas." },
    { "en": "Apa Itu Sensor Serat Optik?", "id": "Serat Sebagai Elemen Sensor." },
    { "en": "Apa Itu Sensor Intensitas?", "id": "Mengukur Perubahan Intensitas Cahaya." },
    { "en": "Apa Itu Sensor Fasa?", "id": "Mengukur Perubahan Fasa Cahaya." },
    { "en": "Apa Itu Sensor Polarisasi?", "id": "Mengukur Perubahan Polarisasi Cahaya." },
    { "en": "Apa Itu Sensor Panjang Gelombang?", "id": "Mengukur Perubahan Panjang Gelombang." },
    { "en": "Apa Itu Interferometer Fabry-Pérot?", "id": "Sensor Berbasis Rongga Resonansi Optik." },
    { "en": "Apa Itu Interferometer Mach-Zehnder?", "id": "Sensor Berbasis Interferensi Jalur." },
    { "en": "Apa Itu Interferometer Michelson?", "id": "Digunakan Untuk Mendeteksi Gelombang Gravitasi." },
    { "en": "Apa Itu Interferometer Sagnac?", "id": "Dasar Dari Giroskop Serat Optik." },
    { "en": "Apa Itu Sensor Efek Magneto-optik?", "id": "Rotasi Polarisasi Akibat Medan Magnet." },
    { "en": "Apa Itu Efek Faraday?", "id": "Contoh Efek Magneto-optik." },
    { "en": "Apa Itu Sensor Arus Serat Optik?", "id": "Menggunakan Efek Faraday." },
    { "en": "Apa Itu Sensor Permukaan Akustik Gelombang?", "id": "SAW (Surface Acoustic Wave) Sensor." },
    { "en": "Apa Itu Sensor Bulk Acoustic Wave?", "id": "BAW (Bulk Acoustic Wave) Sensor." },
    { "en": "Apa Itu Sensor Lamb Wave?", "id": "Menggunakan Gelombang Terpandu Di Pelat Tipis." },
    { "en": "Apa Itu Sensor Viskositas Resonansi?", "id": "Mengukur Redaman Pada Resonator." },
    { "en": "Apa Itu Sensor Keausan Minyak?", "id": "Mendeteksi Partikel Logam Dalam Minyak." },
    { "en": "Apa Itu Sensor Inframerah?", "id": "Mendeteksi Radiasi Inframerah (Panas)." },
    { "en": "Apa Itu Sensor Inframerah Pasif?", "id": "PIR (Passive Infrared), Hanya Menerima." },
    { "en": "Apa Itu Sensor Inframerah Aktif?", "id": "Memancarkan Dan Menerima Sinar Inframerah." },
    { "en": "Apa Itu Termopile?", "id": "Sensor Suhu Inframerah Berbasis Termokopel." },
    { "en": "Apa Itu Bolometer?", "id": "Sensor Radiasi Termal Berbasis Resistansi." },
    { "en": "Apa Itu Sensor Piroelektrik?", "id": "Mendeteksi Perubahan Radiasi Inframerah." },
    { "en": "Apa Itu Sensor Fotokonduktif?", "id": "Konduktivitas Berubah Karena Radiasi." },
    { "en": "Apa Itu Sensor Fotovoltaik?", "id": "Menghasilkan Tegangan Dari Radiasi." },
    { "en": "Apa Itu Sensor Ultraviolet?", "id": "UV (Ultra Violet) Sensor." },
    { "en": "Apa Itu Sensor Sinar-X?", "id": "Mendeteksi Radiasi Sinar-X." },
    { "en": "Apa Itu Sensor Sinar Gamma?", "id": "Mendeteksi Radiasi Gamma." },
    { "en": "Apa Itu Dosimeter?", "id": "Mengukur Dosis Paparan Radiasi." },
    { "en": "Apa Itu Sensor Efek Compton?", "id": "Mendeteksi Hamburan Sinar Gamma." },
    { "en": "Apa Itu Sensor Efek Cherenkov?", "id": "Mendeteksi Radiasi Dari Partikel Cepat." },
    { "en": "Apa Itu Kalorimeter (Fisika Partikel)?", "id": "Mengukur Energi Partikel." },
    { "en": "Apa Itu Sensor Kawat Berpendar?", "id": "Wire Chamber Untuk Melacak Partikel." },
    { "en": "Apa Itu Detektor Drift?", "id": "Mengukur Posisi Partikel Dari Waktu Drift." },
    { "en": "Apa Itu Detektor Semikonduktor?", "id": "Mendeteksi Radiasi Dengan Dioda." },
    { "en": "Apa Itu Detektor Silikon Drift?", "id": "SDD (Silicon Drift Detector)." },
    { "en": "Apa Itu Detektor Piksel?", "id": "Array Sensor Untuk Fisika Energi Tinggi." },
    { "en": "Apa Itu Sensor Neutron?", "id": "Mendeteksi Partikel Neutron." },
    { "en": "Apa Itu Sensor Neutrino?", "id": "Detektor Sangat Besar Di Bawah Tanah." },
    { "en": "Apa Itu Sensor Gelombang Gravitasi?", "id": "Interferometer Laser Skala Besar." },
    { "en": "Apa Itu Sensor Resonansi Magnetik Nuklir?", "id": "NMR (Nuclear Magnetic Resonance) Sensor." },
    { "en": "Apa Itu Sensor Resonansi Paramagnetik Elektron?", "id": "EPR (Electron Paramagnetic Resonance) Sensor." },
    { "en": "Apa Itu Sensor Aliran Sitometri?", "id": "Menganalisis Sel Individual Dalam Aliran." },
    { "en": "Apa Itu Sensor PCR (Polymerase Chain Reaction)?", "id": "Mendeteksi Sekuens DNA Spesifik." },
    { "en": "Apa Itu Array Mikro DNA?", "id": "Mengukur Ekspresi Ribuan Gen." },
    { "en": "Apa Itu Sistem Pengurutan Gen?", "id": "Menentukan Urutan Nukleotida DNA." },
    { "en": "Apa Itu Sensor Elektrokortikografi?", "id": "ECoG (Electrocorticography)." },
    { "en": "Apa Fungsi ECoG (Electrocorticography)?", "id": "Merekam Aktivitas Otak Dari Permukaan." },
    { "en": "Apa Itu Sensor Impedansi Bioelektrik?", "id": "Mengukur Komposisi Tubuh." },
    { "en": "Apa Itu Tomografi Impedansi Listrik?", "id": "EIT (Electrical Impedance Tomography)." },
    { "en": "Apa Itu Magnetoencephalography?", "id": "MEG (Magnetoencephalography)." },
    { "en": "Apa Fungsi MEG (Magnetoencephalography)?", "id": "Mengukur Medan Magnet Otak." },
    { "en": "Apa Itu Transduser Intra-vaskular?", "id": "Sensor Yang Dimasukkan Dalam Pembuluh Darah." },
    { "en": "Apa Itu Kapsul Endoskopi?", "id": "Kamera Nirkabel Yang Dapat Ditelan." },
    { "en": "Apa Itu Transduser Pencitraan Fotoakustik?", "id": "Menggunakan Laser Dan Ultrasonik." },
    { "en": "Apa Itu Aktuator Cerdas?", "id": "Aktuator Dengan Sensor Dan Kontroler." },
    { "en": "Apa Itu Material Cerdas?", "id": "Material Yang Merespon Stimulus." },
    { "en": "Apa Itu Cairan Magnetoreologikal?", "id": "MRF (Magnetorheological Fluid)." },
    { "en": "Bagaimana MRF (Magnetorheological Fluid) Bekerja?", "id": "Viskositas Berubah Akibat Medan Magnet." },
    { "en": "Apa Itu Cairan Elektroreologikal?", "id": "ERF (Electrorheological Fluid)." },
    { "en": "Apa Itu Polimer Dielektrik Elastomer?", "id": "DEA (Dielectric Elastomer Actuator)." },
    { "en": "Apa Itu Otot Buatan Ionik?", "id": "IPMC (Ionic Polymer-Metal Composite)." },
    { "en": "Apa Itu Aktuator Termal Bimetal?", "id": "Membengkok Saat Dipanaskan." },
    { "en": "Apa Itu Aktuator Paduan Memori Bentuk?", "id": "SMA (Shape-Memory Alloy) Actuator." },
    { "en": "Apa Itu Transduser Elektrostriktif?", "id": "Mirip Piezoelektrik Tapi Efek Kuadratik." },
    { "en": "Apa Itu Transduser Magnetostriktif?", "id": "Bahan Berubah Bentuk Dalam Medan Magnet." },
    { "en": "Apa Itu Pengeras Suara Magnetostriktif?", "id": "Speaker Tanpa Konus Konvensional." },
    { "en": "Apa Itu Motor Reluktansi?", "id": "Motor Berbasis Perubahan Reluktansi." },
    { "en": "Apa Itu Motor Reluktansi Tersaklar?", "id": "SRM (Switched Reluctance Motor)." },
    { "en": "Apa Itu Bantalan Magnetik?", "id": "Menopang Benda Menggunakan Levitasi Magnetik." },
    { "en": "Apa Itu Bantalan Magnetik Aktif?", "id": "Menggunakan Elektromagnet Dan Kontrol Umpan Balik." },
    { "en": "Apa Itu Bantalan Magnetik Pasif?", "id": "Menggunakan Magnet Permanen." },
    { "en": "Apa Itu Elektrodinamik Suspensi?", "id": "EDS (Electrodynamic Suspension)." },
    { "en": "Apa Itu Elektromagnetik Suspensi?", "id": "EMS (Electromagnetic Suspension)." },
    { "en": "Apa Itu Peredam Magnetoreologikal?", "id": "Peredam Dengan Kekuatan Terkontrol." },
    { "en": "Apa Itu Kopling Magnetoreologikal?", "id": "Kopling Dengan Torsi Terkontrol." },
    { "en": "Apa Itu Sensor Virtual?", "id": "Output Dihitung Dari Sensor Lain." },
    { "en": "Apa Itu Redundansi Analitik?", "id": "Menggunakan Model Matematis Untuk Validasi." },
    { "en": "Apa Itu Deteksi Dan Isolasi Kesalahan?", "id": "FDI (Fault Detection and Isolation)." },
    { "en": "Apa Itu Observabilitas?", "id": "Kemampuan Memperkirakan Keadaan Internal." },
    { "en": "Apa Itu Kontrolabilitas?", "id": "Kemampuan Mengontrol Keadaan Internal." },
    { "en": "Apa Itu Estimator Keadaan?", "id": "Memperkirakan Variabel Yang Tidak Terukur." },
    { "en": "Apa Itu Pengamat Luenberger?", "id": "Jenis Estimator Keadaan." },
    { "en": "Apa Itu Model Ruang Keadaan?", "id": "State-Space Representation." },
    { "en": "Apa Itu Jaringan Sensor Nirkabel?", "id": "WSN (Wireless Sensor Network)." },
    { "en": "Apa Itu Jaringan Area Tubuh Nirkabel?", "id": "WBAN (Wireless Body Area Network)." },
    { "en": "Apa Itu Sinkronisasi Waktu?", "id": "Menjaga Semua Simpul Memiliki Waktu Sama." },
    { "en": "Apa Itu Protokol Sinkronisasi Jaringan?", "id": "NTP (Network Time Protocol)." },
    { "en": "Apa Itu Protokol Waktu Presisi?", "id": "PTP (Precision Time Protocol)." },
    { "en": "Apa Itu Lokalisasi?", "id": "Menentukan Posisi Simpul Sensor." },
    { "en": "Apa Itu Agregasi Data?", "id": "Menggabungkan Data Di Dalam Jaringan." },
    { "en": "Apa Itu Routing Sadar Energi?", "id": "Memilih Jalur Untuk Memperpanjang Umur Jaringan." },
    { "en": "Apa Itu Siklus Tugas (Duty Cycling)?", "id": "Menidurkan Radio Secara Periodik." },
    { "en": "Apa Itu Antarmuka Pengguna?", "id": "UI (User Interface)." },
    { "en": "Apa Itu Pengalaman Pengguna?", "id": "UX (User Experience)." },
    { "en": "Apa Itu Transduser Untuk Realitas Virtual?", "id": "Pelacak Posisi, Layar, Audio 3D." },
    { "en": "Apa Itu Pelacakan Luar-Dalam?", "id": "Sensor Di Headset Melacak Lingkungan." },
    { "en": "Apa Itu Pelacakan Dalam-Luar?", "id": "Sensor Eksternal Melacak Headset." },
    { "en": "Apa Itu Transduser Untuk Realitas Tertambah?", "id": "Kamera, Proyektor, Sensor Kedalaman." },
    { "en": "Apa Itu Sistem Kendali Umpan Balik?", "id": "Feedback Control System." },
    { "en": "Apa Itu Sistem Kendali Umpan Maju?", "id": "Feedforward Control System." },
    { "en": "Apa Itu Sistem Kendali Loop Terbuka?", "id": "Open-Loop Control System." },
    { "en": "Apa Itu Sistem Kendali Loop Tertutup?", "id": "Closed-Loop Control System." },
    { "en": "Apa Itu Sensor Dalam Sistem Kendali?", "id": "Elemen Pengukur (Umpan Balik)." },
    { "en": "Apa Itu Aktuator Dalam Sistem Kendali?", "id": "Elemen Yang Mengubah Sinyal Kontrol." },
    { "en": "Apa Itu Kontroler Dalam Sistem Kendali?", "id": "Elemen Pemroses Kesalahan." },
    { "en": "Apa Itu Setpoint?", "id": "Nilai Yang Diinginkan." },
    { "en": "Apa Itu Variabel Proses?", "id": "Besaran Aktual Yang Diukur." },
    { "en": "Apa Itu Sinyal Kesalahan (Error Signal)?", "id": "Selisih Antara Setpoint Dan Variabel Proses." },
    { "en": "Apa Itu Kontroler On-Off?", "id": "Jenis Kontroler Paling Sederhana." },
    { "en": "Apa Itu Kontroler Proporsional (P)?", "id": "Output Sebanding Dengan Kesalahan." },
    { "en": "Apa Itu Kontroler Integral (I)?", "id": "Menghilangkan Kesalahan Keadaan Tunak." },
    { "en": "Apa Itu Kontroler Derivatif (D)?", "id": "Merespon Laju Perubahan Kesalahan." },
    { "en": "Apa Itu Kontroler PID (Proportional-Integral-Derivative)?", "id": "Kontroler Umpan Balik Paling Umum." },
    { "en": "Apa Itu Tuning Kontroler?", "id": "Menyesuaikan Parameter Kontroler (P, I, D)." },
    { "en": "Apa Itu Metode Ziegler-Nichols?", "id": "Metode Klasik Untuk Tuning PID." },
    { "en": "Apa Itu Sensor Cerdas (Smart Sensor)?", "id": "Sensor Dengan Kemampuan Pemrosesan." },
    { "en": "Apa Itu Jaringan Sensor Nirkabel?", "id": "WSN (Wireless Sensor Network)." },
    { "en": "Apa Itu Sensor MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sensor Yang Dibuat Dengan Fabrikasi IC." },
    { "en": "Apa Itu Sensor Serat Optik?", "id": "FOS (Fiber Optic Sensor)." },
    { "en": "Apa Keuntungan FOS (Fiber Optic Sensor)?", "id": "Kebal EMI, Aman Di Lingkungan Berbahaya." },
    { "en": "Apa Itu Sensor Suhu Terdistribusi?", "id": "DTS (Distributed Temperature Sensing)." },
    { "en": "Apa Itu Sensor Regangan Terdistribusi?", "id": "DSS (Distributed Strain Sensing)." },
    { "en": "Apa Itu Sensor Gas Inframerah?", "id": "Mendeteksi Gas Berdasarkan Penyerapan IR." },
    { "en": "Apa Itu Sensor Kelembaban Optik?", "id": "Mengukur Kelembaban Dengan Prinsip Optik." },
    { "en": "Apa Itu Sensor Permukaan Akustik?", "id": "SAW (Surface Acoustic Wave) Sensor." },
    { "en": "Apa Itu Giroskop Resonansi?", "id": "Giroskop Berbasis Elemen Bergetar (MEMS)." },
    { "en": "Apa Itu Efek Coriolis?", "id": "Prinsip Kerja Giroskop Resonansi." },
    { "en": "Apa Itu Sensor Tekanan Resonansi?", "id": "Frekuensi Resonansi Berubah Dengan Tekanan." },
    { "en": "Apa Itu Sensor Massa Resonansi?", "id": "QCM (Quartz Crystal Microbalance)." },
    { "en": "Apa Itu Sensor Kepadatan Resonansi?", "id": "Mengukur Kepadatan Cairan." },
    { "en": "Apa Itu Sensor Kimia Resonansi?", "id": "Mendeteksi Zat Kimia Dengan Pergeseran Frekuensi." },
    { "en": "Apa Itu Sensor Kentalitas Resonansi?", "id": "Mengukur Viskositas Dengan Redaman." },
    { "en": "Apa Itu Transduser Kapasitif?", "id": "Bekerja Berdasarkan Perubahan Kapasitansi." },
    { "en": "Apa Itu Mikrofon Kapasitif?", "id": "Mikrofon Kondensor." },
    { "en": "Apa Itu Sensor Jarak Kapasitif?", "id": "Mendeteksi Perubahan Kapasitansi." },
    { "en": "Apa Itu Sensor Kelembaban Kapasitif?", "id": "Konstanta Dielektrik Berubah Dengan Kelembaban." },
    { "en": "Apa Itu Sensor Level Kapasitif?", "id": "Mengukur Ketinggian Cairan Dielektrik." },
    { "en": "Apa Itu Akselerometer Kapasitif?", "id": "Massa Bergerak Mengubah Jarak Pelat." },
    { "en": "Apa Itu Sensor Tekanan Kapasitif?", "id": "Diafragma Bergerak Mengubah Jarak Pelat." },
    { "en": "Apa Itu Transduser Induktif?", "id": "Bekerja Berdasarkan Perubahan Induktansi." },
    { "en": "Apa Itu LVDT (Linear Variable Differential Transformer)?", "id": "Sensor Posisi Linear Induktif." },
    { "en": "Apa Itu RVDT (Rotary Variable Differential Transformer)?", "id": "Sensor Posisi Sudut Induktif." },
    { "en": "Apa Itu Sensor Jarak Induktif?", "id": "Mendeteksi Logam Dengan Arus Eddy." },
    { "en": "Apa Itu Sensor Magnetostriktif?", "id": "Sifat Magnetik Berubah Dengan Regangan." },
    { "en": "Apa Itu Transduser Elektrodinamik?", "id": "Menggunakan Prinsip Gaya Lorentz." },
    { "en": "Contoh Transduser Elektrodinamik?", "id": "Speaker Dinamis, Mikrofon Dinamis." },
    { "en": "Apa Itu Shaker Elektrodinamik?", "id": "Alat Uji Getaran." },
    { "en": "Apa Itu Sensor Efek Wiegand?", "id": "Kawat Magnetik Dengan Perilaku Switching." },
    { "en": "Apa Itu Efek Villari?", "id": "Kebalikan Dari Efek Magnetostriktif." },
    { "en": "Apa Itu Sensor Efek Fotolistrik?", "id": "Tabung Pengganda Foton (PMT)." },
    { "en": "Apa Itu Sensor Ionisasi?", "id": "Mendeteksi Radiasi Pengion." },
    { "en": "Apa Itu Sensor Termal?", "id": "Mendeteksi Panas Atau Suhu." },
    { "en": "Apa Itu Sensor Mekanis?", "id": "Mendeteksi Besaran Mekanis." },
    { "en": "Apa Itu Sensor Radiasi?", "id": "Mendeteksi Radiasi Elektromagnetik." },
    { "en": "Apa Itu Transduser Pasif?", "id": "Tidak Membutuhkan Sumber Energi Eksternal." },
    { "en": "Apa Itu Transduser Aktif?", "id": "Membutuhkan Sumber Energi Eksternal." },
    { "en": "Apa Itu Sensor Laju Alir?", "id": "Flow Sensor." },
    { "en": "Apa Itu Sensor pH?", "id": "Mengukur Keasaman Atau Kebasaan." },
    { "en": "Apa Itu Sensor ORP (Oxidation-Reduction Potential)?", "id": "Mengukur Potensial Redoks." },
    { "en": "Apa Itu Sel Beban?", "id": "Load Cell." },
    { "en": "Apa Itu Sensor Torsi?", "id": "Torque Sensor." },
    { "en": "Apa Itu Sensor Gesekan?", "id": "Friction Sensor." },
    { "en": "Apa Itu Tribometer?", "id": "Alat Untuk Mengukur Gesekan." },
    { "en": "Apa Itu Sensor Ketebalan Film Tipis?", "id": "Menggunakan Kristal Kuarsa Atau Optik." },
    { "en": "Apa Itu Elipsometer?", "id": "Mengukur Sifat Lapisan Tipis." },
    { "en": "Apa Itu Sensor Posisi Laser?", "id": "Menggunakan Triangulasi Laser." },
    { "en": "Apa Itu Sensor Visi 3D?", "id": "Membangun Peta Kedalaman (Depth Map)." },
    { "en": "Apa Itu Sensor Stereovision?", "id": "Menggunakan Dua Kamera Untuk Kedalaman." },
    { "en": "Apa Itu Sensor Pencitraan Hiperspektral?", "id": "Mengambil Gambar Dalam Ratusan Warna." },
    { "en": "Apa Itu Sensor Cahaya Sekitar?", "id": "ALS (Ambient Light Sensor)." },
    { "en": "Fungsi ALS (Ambient Light Sensor)?", "id": "Menyesuaikan Kecerahan Layar Otomatis." },
    { "en": "Apa Itu Sensor Warna RGB?", "id": "Mengukur Komponen Merah, Hijau, Biru." },
    { "en": "Apa Itu Sensor Gestur?", "id": "Mendeteksi Gerakan Tangan." },
    { "en": "Apa Itu Pengkondisi Sinyal Universal?", "id": "Dapat Dikonfigurasi Untuk Berbagai Sensor." },
    { "en": "Apa Itu Transmisi Sinyal Nirkabel?", "id": "Mengirim Data Sensor Tanpa Kabel." },
    { "en": "Apa Itu Protokol LoRaWAN?", "id": "Protokol LPWAN (Low-Power Wide-Area Network)." },
    { "en": "Apa Itu Protokol NB-IoT?", "id": "Narrowband-IoT (Internet of Things)." },
    { "en": "Apa Itu Protokol Bluetooth Low Energy?", "id": "BLE (Bluetooth Low Energy)." },
    { "en": "Apa Itu Protokol Zigbee?", "id": "Standar Nirkabel Berdaya Rendah." },
    { "en": "Apa Itu Sensor Jaringan Mesh?", "id": "Sensor Dapat Meneruskan Data Sensor Lain." },
    { "en": "Apa Itu Edge Computing?", "id": "Pemrosesan Data Dilakukan Di Dekat Sensor." },
    { "en": "Apa Itu Cloud Computing?", "id": "Pemrosesan Data Dilakukan Di Server Pusat." },
    { "en": "Apa Itu Antarmuka Sensor-ke-Cloud?", "id": "Menghubungkan Sensor Langsung Ke Internet." },
    { "en": "Apa Itu Protokol MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol Populer Untuk IoT (Internet of Things)." },
    { "en": "Apa Itu Sensor Inersia?", "id": "Mengukur Gerakan (Akselerometer, Giroskop)." },
    { "en": "Apa Itu Sensor Magnetik, Inersia, Sudut?", "id": "MARG (Magnetic, Angular Rate, and Gravity)." },
    { "en": "Apa Itu Sensor Efek Fotorefraktif?", "id": "Indeks Bias Berubah Karena Cahaya." },
    { "en": "Apa Itu Sensor Berbasis Metamaterial?", "id": "Sensor Dengan Sensitivitas Sangat Tinggi." },
    { "en": "Apa Itu Sensor Gelombang Permukaan?", "id": "Mendeteksi Perubahan Pada Permukaan." },
    { "en": "Apa Itu Resonator Mikro-elektromekanis?", "id": "Digunakan Dalam Sensor Resonansi." },
    { "en": "Apa Itu Titik Referensi?", "id": "Nilai Dikenal Untuk Kalibrasi." },
    { "en": "Apa Itu Standar Transfer?", "id": "Perangkat Yang Digunakan Untuk Mengkalibrasi Lainnya." },
    { "en": "Apa Itu Ketertelusuran (Traceability)?", "id": "Kemampuan Melacak Kalibrasi Ke Standar Nasional." },
    { "en": "Apa Itu Ketidakpastian Pengukuran?", "id": "Rentang Keraguan Pada Hasil Pengukuran." },
    { "en": "Apa Itu Sensor Redundan?", "id": "Beberapa Sensor Mengukur Besaran Sama." },
    { "en": "Apa Itu Pemungutan Suara (Voting) Sensor?", "id": "Memilih Bacaan Mayoritas Dari Sensor Redundan." },
    { "en": "Apa Itu Rata-rata Sinyal?", "id": "Mengurangi Derau Dengan Merata-ratakan." },
    { "en": "Apa Itu Filtering Digital?", "id": "Memproses Sampel Sensor Secara Matematis." },
    { "en": "Apa Itu Antarmuka Cerdas?", "id": "Sensor Dengan Kemampuan Komunikasi Digital." },
    { "en": "Apa Itu Antarmuka Manusia-Mesin?", "id": "HMI (Human-Machine Interface)." },
    { "en": "Apa Itu Sensor Visual?", "id": "Kamera." },
    { "en": "Apa Itu Sensor Audio?", "id": "Mikrofon." },
    { "en": "Apa Itu Sensor Haptic?", "id": "Memberikan Umpan Balik Getaran Atau Gaya." },
    { "en": "Apa Itu Layar Sentuh Resistif?", "id": "Mendeteksi Tekanan Fisik." },
    { "en": "Apa Itu Layar Sentuh Kapasitif?", "id": "Mendeteksi Gangguan Medan Listrik." },
    { "en": "Apa Itu Layar Sentuh Gelombang Akustik Permukaan?", "id": "SAW (Surface Acoustic Wave) Touchscreen." },
    { "en": "Apa Itu Layar Sentuh Inframerah?", "id": "Mendeteksi Objek Yang Menghalangi Kisi IR." },
    { "en": "Apa Itu Sensor Gaya 3D?", "id": "Mengukur Gaya Dalam Tiga Dimensi." },
    { "en": "Apa Itu Sensor Gerakan?", "id": "Mendeteksi Gerakan Fisik." },
    { "en": "Apa Itu Sensor Okupansi?", "id": "Mendeteksi Kehadiran Orang Di Ruangan." },
    { "en": "Apa Itu Tombol Tekan?", "id": "Sensor Gaya Biner." },
    { "en": "Apa Itu Saklar?", "id": "Sensor Posisi Biner." },
    { "en": "Apa Itu Joystick?", "id": "Sensor Posisi Dua Dimensi." },
    { "en": "Apa Itu Trackball?", "id": "Sensor Gerak Relatif." },
    { "en": "Apa Itu Touchpad?", "id": "Sensor Posisi Relatif Kapasitif." },
    { "en": "Apa Itu Pena Digital (Stylus)?", "id": "Sensor Posisi Absolut." },
    { "en": "Apa Itu Digitizer?", "id": "Tablet Grafis." },
    { "en": "Apa Itu Pemindai (Scanner)?", "id": "Mengubah Dokumen Fisik Menjadi Digital." },
    { "en": "Apa Itu Pembaca Kartu Magnetik?", "id": "Membaca Data Dari Garis Magnetik." },
    { "en": "Apa Itu Pembaca Kode Batang?", "id": "Membaca Pola Garis Hitam Putih." },
    { "en": "Apa Itu Pembaca Kode QR?", "id": "Membaca Matriks Data 2D." },
    { "en": "Apa Itu Pengenalan Karakter Tinta Magnetik?", "id": "MICR (Magnetic Ink Character Recognition)." },
    { "en": "Apa Itu Pengenalan Tanda Optik?", "id": "OMR (Optical Mark Recognition)." },
    { "en": "Apa Itu Pengenalan Tulisan Tangan?", "id": "HCR (Handwriting Character Recognition)." },
    { "en": "Apa Itu Pengenalan Ucapan?", "id": "Speech Recognition." },
    { "en": "Apa Itu Verifikasi Pembicara?", "id": "Mengenali Siapa Yang Berbicara." },
    { "en": "Apa Itu Sensor Elektro-optik?", "id": "Mengubah Sinar Optik Menjadi Listrik." },
    { "en": "Apa Itu Sensor Elektrokimia?", "id": "Mengukur Reaksi Kimia." },
    { "en": "Apa Itu Sensor Elektromekanis?", "id": "Mengubah Energi Mekanis-Listrik." },
    { "en": "Apa Itu Transduser Bolak-balik?", "id": "Dapat Berfungsi Sebagai Sensor Dan Aktuator." },
    { "en": "Apa Itu Linearitas?", "id": "Hubungan Lurus Antara Input Dan Output." },
    { "en": "Apa Itu Histeresis?", "id": "Ketergantungan Output Pada Sejarah Input." },
    { "en": "Apa Itu Sensitivitas Sumbu Silang?", "id": "Sensitivitas Terhadap Input Yang Tidak Diinginkan." },
    { "en": "Apa Itu Rentang Frekuensi?", "id": "Frekuensi Operasi Sensor." },
    { "en": "Apa Itu Sinyal Modulasi?", "id": "Sinyal Informasi." },
    { "en": "Apa Itu Sinyal Pembawa?", "id": "Sinyal Frekuensi Tinggi." },
    { "en": "Apa Itu Demodulator?", "id": "Mengekstrak Sinyal Informasi." },
    { "en": "Apa Itu Sirkuit Jembatan?", "id": "Mengukur Perubahan Impedansi Kecil." },
    { "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Selisih Dua Sinyal." },
    { "en": "Apa Itu Sirkuit Pengkondisi Sinyal?", "id": "SCU (Signal Conditioning Unit)." },
    { "en": "Apa Itu Penyaringan (Filtering)?", "id": "Menghilangkan Komponen Frekuensi Tak Diinginkan." },
    { "en": "Apa Itu Perataan (Averaging)?", "id": "Mengurangi Derau Dengan Rata-rata." },
    { "en": "Apa Itu Korelasi?", "id": "Mengukur Kemiripan Antara Sinyal." },
    { "en": "Apa Itu Antarmuka Sensor?", "id": "Sirkuit Yang Menghubungkan Sensor Ke Sistem." },
    { "en": "Apa Itu Protokol Sensor?", "id": "Aturan Komunikasi Untuk Sensor." },
    { "en": "Apa Itu Sensor Dengan Output Tegangan?", "id": "Outputnya Berupa Sinyal Tegangan." },
    { "en": "Apa Itu Sensor Dengan Output Arus?", "id": "Outputnya Berupa Sinyal Arus." },
    { "en": "Apa Itu Sensor Dengan Output Resistansi?", "id": "Outputnya Berupa Perubahan Resistansi." },
    { "en": "Apa Itu Sensor Dengan Output Kapasitansi?", "id": "Outputnya Berupa Perubahan Kapasitansi." },
    { "en": "Apa Itu Sensor Dengan Output Induktansi?", "id": "Outputnya Berupa Perubahan Induktansi." },
    { "en": "Apa Itu Sensor Dengan Output Frekuensi?", "id": "Outputnya Berupa Sinyal Frekuensi." },
    { "en": "Apa Itu Sensor Dengan Output Digital?", "id": "Outputnya Berupa Data Digital." },
    { "en": "Apa Itu Sensor Suhu Termal?", "id": "Termokopel, RTD (Resistance Temperature Detector), Termistor." },
    { "en": "Apa Itu Sensor Suhu Optik?", "id": "Pirometer, FBG (Fiber Bragg Grating)." },
    { "en": "Apa Itu Sensor Tekanan Mekanis?", "id": "Tabung Bourdon, Diafragma." },
    { "en": "Apa Itu Sensor Tekanan Elektronik?", "id": "Piezoresistif, Kapasitif." },
    { "en": "Apa Itu Sensor Aliran Mekanis?", "id": "Turbin, Rotameter." },
    { "en": "Apa Itu Sensor Aliran Elektronik?", "id": "Ultrasonik, Elektromagnetik, Termal." },
    { "en": "Apa Itu Transduser Gaya?", "id": "Load Cell." },
    { "en": "Apa Itu Transduser Perpindahan?", "id": "LVDT (Linear Variable Differential Transformer)." },
    { "en": "Apa Itu Transduser Percepatan?", "id": "Akselerometer." },
    { "en": "Apa Itu Transduser Kecepatan?", "id": "Tachometer." },
    { "en": "Apa Itu Transduser Akustik?", "id": "Mikrofon, Speaker, Hidrofon." },
    { "en": "Apa Itu Transduser Optoelektronik?", "id": "LED (Light-Emitting Diode), Fotodioda." },
    { "en": "Apa Itu Sensor Efek Fotokonduktif?", "id": "LDR (Light Dependent Resistor)." },
    { "en": "Apa Itu Sensor Efek Fotovoltaik?", "id": "Sel Surya, Fotodioda." },
    { "en": "Apa Itu Sensor Termoelektrik?", "id": "Termokopel." },
    { "en": "Apa Itu Sensor Piroelektrik?", "id": "Sensor PIR (Passive Infrared)." },
    { "en": "Apa Itu Sensor Piezoelektrik?", "id": "Kristal Kuarsa, PZT (Lead Zirconate Titanate)." },
    { "en": "Apa Itu Sensor Piezoresistif?", "id": "Strain Gauge Semikonduktor." },
    { "en": "Apa Itu Sensor Magnetoresistif?", "id": "Sensor GMR (Giant Magnetoresistance)." },
    { "en": "Apa Itu Sensor Magnetostriktif?", "id": "Sensor Posisi Linear." },
    { "en": "Apa Itu Sensor Efek Hall?", "id": "Mendeteksi Medan Magnet." },
    { "en": "Apa Itu Sensor Serat Optik?", "id": "Menggunakan Cahaya Dalam Serat." },
    { "en": "Apa Itu Sensor Kimia?", "id": "Mendeteksi Zat Kimia." },
    { "en": "Apa Itu Sensor Biologis?", "id": "Biosensor." },
    { "en": "Apa Itu Sensor Kelembaban?", "id": "Hygrometer." },
    { "en": "Apa Itu Sensor Tekanan?", "id": "Pressure Gauge." },
    { "en": "Apa Itu Sensor Vakum?", "id": "Vacuum Gauge (Pirani, Penning)." },
    { "en": "Apa Itu Pengukur Pirani?", "id": "Sensor Vakum Berbasis Konduktivitas Termal." },
    { "en": "Apa Itu Pengukur Penning?", "id": "Sensor Vakum Berbasis Ionisasi." },
    { "en": "Apa Itu Sensor Radiasi?", "id": "Dosimeter, Scintillator." },
    { "en": "Apa Itu Sensor Getaran?", "id": "Vibration Sensor." },
    { "en": "Apa Itu Sensor Kemiringan?", "id": "Inclinometer, Tilt Sensor." },
    { "en": "Apa Itu Sensor Magnetik?", "id": "Kompas, Magnetometer." },
    { "en": "Apa Itu Sensor Optik?", "id": "Kamera, Sensor Warna." },
    { "en": "Apa Itu Sensor Suara?", "id": "Mikrofon." },
    { "en": "Apa Itu Sensor Gaya?", "id": "Force Sensor." },
    { "en": "Apa Itu Sensor Regangan?", "id": "Strain Sensor." },
    { "en": "Apa Itu Aktuator Listrik?", "id": "Motor, Solenoida." },
    { "en": "Apa Itu Aktuator Pneumatik?", "id": "Menggunakan Udara Bertekanan." },
    { "en": "Apa Itu Aktuator Hidrolik?", "id": "Menggunakan Cairan Bertekanan." },
    { "en": "Apa Itu Aktuator Termal?", "id": "Menggunakan Ekspansi Termal." },
    { "en": "Apa Itu Aktuator Cerdas?", "id": "Aktuator Dengan Kontroler Terintegrasi." },
    { "en": "Apa Itu Sensor Sudut Putar?", "id": "Rotary Encoder." },
    { "en": "Apa Itu Sensor Perpindahan Linear?", "id": "Linear Encoder." },
    { "en": "Apa Itu Sensor Induktif?", "id": "Mendeteksi Objek Logam." },
    { "en": "Apa Itu Sensor Kapasitif?", "id": "Mendeteksi Objek Apapun." },
    { "en": "Apa Itu Sensor Fotoelektrik?", "id": "Menggunakan Sinar Cahaya." },
    { "en": "Apa Itu Sensor Visi?", "id": "Kamera Untuk Inspeksi Otomatis." },
    { "en": "Apa Itu Sensor Jarak Laser?", "id": "Mengukur Jarak Dengan Akurasi Tinggi." },
    { "en": "Apa Itu Sensor Jarak Jauh?", "id": "Remote Sensing." },
    { "en": "Apa Itu Penginderaan Jauh Aktif?", "id": "Memancarkan Sinyal Sendiri (Radar, LiDAR)." },
    { "en": "Apa Itu Penginderaan Jauh Pasif?", "id": "Mendeteksi Radiasi Alami (Kamera)." },
    { "en": "Apa Itu Sensor Gravitasi?", "id": "Gravimeter." },
    { "en": "Apa Itu Giroskop MEMS?", "id": "Sensor Rotasi Mikro." },
    { "en": "Apa Itu Akselerometer MEMS?", "id": "Sensor Percepatan Mikro." },
    { "en": "Apa Itu Mikrofon MEMS?", "id": "Mikrofon Berbasis Silikon." },
    { "en": "Apa Itu Saklar RF MEMS?", "id": "Saklar Frekuensi Radio Mekanis Mikro." },
    { "en": "Apa Itu Osilator MEMS?", "id": "Resonator Mikro Untuk Pewaktuan." },
    { "en": "Apa Itu Sensor Tekanan MEMS?", "id": "Sensor Tekanan Miniatur." },
    { "en": "Apa Itu Tinta Elektronik?", "id": "E-Ink Display." },
    { "en": "Bagaimana E-Ink Bekerja?", "id": "Partikel Bermuatan Diatur Medan Listrik." },
    { "en": "Apa Itu Tampilan Bistabil?", "id": "Mempertahankan Gambar Tanpa Daya." },
    { "en": "Apa Itu Sensor Pencitraan Kontak?", "id": "CIS (Contact Image Sensor)." },
    { "en": "Di Mana CIS (Contact Image Sensor) Digunakan?", "id": "Pemindai Dokumen (Scanner)." },
    { "en": "Apa Itu Sensor CCD (Charge-Coupled Device)?", "id": "Sensor Gambar Sensitivitas Tinggi." },
    { "en": "Apa Itu Blooming (Pada CCD)?", "id": "Bocoran Muatan Ke Piksel Tetangga." },
    { "en": "Apa Itu Smear (Pada CCD)?", "id": "Artefak Vertikal Akibat Cahaya Terang." },
    { "en": "Apa Itu Filter Bayer?", "id": "Pola Filter Warna (RGGB)." },
    { "en": "Apa Itu Demosaicing?", "id": "Merekonstruksi Warna Dari Pola Bayer." },
    { "en": "Apa Itu White Balance?", "id": "Kompensasi Warna Sumber Cahaya." },
    { "en": "Apa Itu Shutter Global?", "id": "Merekam Semua Piksel Secara Bersamaan." },
    { "en": "Apa Itu Rolling Shutter?", "id": "Merekam Piksel Baris Demi Baris." },
    { "en": "Apa Artefak Dari Rolling Shutter?", "id": "Efek Jello Pada Gerakan Cepat." },
    { "en": "Apa Itu Sensor Time-of-Flight (ToF)?", "id": "Mengukur Jarak Berdasarkan Waktu Cahaya." },
    { "en": "Apa Itu Kamera Stereo?", "id": "Menggunakan Dua Lensa Untuk Persepsi 3D." },
    { "en": "Apa Itu Sensor LiDAR?", "id": "Memindai Lingkungan Dengan Laser." },
    { "en": "Apa Itu Sensor RADAR?", "id": "Menggunakan Gelombang Radio Untuk Deteksi." },
    { "en": "Apa Itu Sensor SONAR?", "id": "Menggunakan Gelombang Suara Untuk Deteksi." },
    { "en": "Apa Itu Sensor Efek Fotorefraktif?", "id": "Indeks Bias Berubah Karena Cahaya." },
    { "en": "Apa Itu Sensor Elektroluminesen?", "id": "Bahan Bercahaya Karena Medan Listrik." },
    { "en": "Apa Itu Sensor Termoluminesen?", "id": "Bahan Bercahaya Saat Dipanaskan." },
    { "en": "Apa Itu Sensor Kimiluminesen?", "id": "Bahan Bercahaya Dari Reaksi Kimia." },
    { "en": "Apa Itu Sensor Bioluminesen?", "id": "Cahaya Dihasilkan Organisme Hidup." },
    { "en": "Apa Itu Sensor Fluoresen?", "id": "Menyerap Dan Memancarkan Kembali Cahaya." },
    { "en": "Apa Itu Sensor Fosforesen?", "id": "Memancarkan Cahaya Perlahan (Glow-in-the-dark)." },
    { "en": "Apa Itu Sensor Galvanik?", "id": "Sensor Elektrokimia Yang Menghasilkan Arus." },
    { "en": "Apa Itu Sensor Potensiometrik?", "id": "Mengukur Potensial (Tegangan) Elektroda." },
    { "en": "Apa Itu Sensor Amperometrik?", "id": "Mengukur Arus Dari Reaksi Kimia." },
    { "en": "Apa Itu Sensor Konduktometrik?", "id": "Mengukur Perubahan Konduktivitas." },
    { "en": "Apa Itu Sensor Serat Optik?", "id": "Serat Sebagai Elemen Sensor." },
    { "en": "Apa Itu Sensor Intensitas?", "id": "Mengukur Perubahan Intensitas Cahaya." },
    { "en": "Apa Itu Sensor Fasa?", "id": "Mengukur Perubahan Fasa Cahaya." },
    { "en": "Apa Itu Sensor Polarisasi?", "id": "Mengukur Perubahan Polarisasi Cahaya." },
    { "en": "Apa Itu Sensor Panjang Gelombang?", "id": "Mengukur Perubahan Panjang Gelombang." },
    { "en": "Apa Itu Interferometer Fabry-Pérot?", "id": "Sensor Berbasis Rongga Resonansi Optik." },
    { "en": "Apa Itu Interferometer Mach-Zehnder?", "id": "Sensor Berbasis Interferensi Jalur." },
    { "en": "Apa Itu Interferometer Michelson?", "id": "Digunakan Untuk Mendeteksi Gelombang Gravitasi." },
    { "en": "Apa Itu Interferometer Sagnac?", "id": "Dasar Dari Giroskop Serat Optik." },
    { "en": "Apa Itu Sensor Efek Magneto-optik?", "id": "Rotasi Polarisasi Akibat Medan Magnet." },
    { "en": "Apa Itu Efek Faraday?", "id": "Contoh Efek Magneto-optik." },
    { "en": "Apa Itu Sensor Arus Serat Optik?", "id": "Menggunakan Efek Faraday." },
    { "en": "Apa Itu Sensor Permukaan Akustik Gelombang?", "id": "SAW (Surface Acoustic Wave) Sensor." },
    { "en": "Apa Itu Sensor Bulk Acoustic Wave?", "id": "BAW (Bulk Acoustic Wave) Sensor." },
    { "en": "Apa Itu Sensor Lamb Wave?", "id": "Menggunakan Gelombang Terpandu Di Pelat Tipis." },
    { "en": "Apa Itu Sensor Viskositas Resonansi?", "id": "Mengukur Redaman Pada Resonator." },
    { "en": "Apa Itu Sensor Keausan Minyak?", "id": "Mendeteksi Partikel Logam Dalam Minyak." },
    { "en": "Apa Itu Sensor Inframerah?", "id": "Mendeteksi Radiasi Inframerah (Panas)." },
    { "en": "Apa Itu Sensor Inframerah Pasif?", "id": "PIR (Passive Infrared), Hanya Menerima." },
    { "en": "Apa Itu Sensor Inframerah Aktif?", "id": "Memancarkan Dan Menerima Sinar Inframerah." },
    { "en": "Apa Itu Termopile?", "id": "Sensor Suhu Inframerah Berbasis Termokopel." },
    { "en": "Apa Itu Bolometer?", "id": "Sensor Radiasi Termal Berbasis Resistansi." },
    { "en": "Apa Itu Sensor Piroelektrik?", "id": "Mendeteksi Perubahan Radiasi Inframerah." },
    { "en": "Apa Itu Sensor Fotokonduktif?", "id": "Konduktivitas Berubah Karena Radiasi." },
    { "en": "Apa Itu Sensor Fotovoltaik?", "id": "Menghasilkan Tegangan Dari Radiasi." },
    { "en": "Apa Itu Sensor Ultraviolet?", "id": "UV (Ultra Violet) Sensor." },
    { "en": "Apa Itu Sensor Sinar-X?", "id": "Mendeteksi Radiasi Sinar-X." },
    { "en": "Apa Itu Sensor Sinar Gamma?", "id": "Mendeteksi Radiasi Gamma." },
    { "en": "Apa Itu Dosimeter?", "id": "Mengukur Dosis Paparan Radiasi." },
    { "en": "Apa Itu Sensor Efek Compton?", "id": "Mendeteksi Hamburan Sinar Gamma." },
    { "en": "Apa Itu Sensor Efek Cherenkov?", "id": "Mendeteksi Radiasi Dari Partikel Cepat." },
    { "en": "Apa Itu Kalorimeter (Fisika Partikel)?", "id": "Mengukur Energi Partikel." },
    { "en": "Apa Itu Sensor Kawat Berpendar?", "id": "Wire Chamber Untuk Melacak Partikel." },
    { "en": "Apa Itu Detektor Drift?", "id": "Mengukur Posisi Partikel Dari Waktu Drift." },
    { "en": "Apa Itu Detektor Semikonduktor?", "id": "Mendeteksi Radiasi Dengan Dioda." },
    { "en": "Apa Itu Detektor Silikon Drift?", "id": "SDD (Silicon Drift Detector)." },
    { "en": "Apa Itu Detektor Piksel?", "id": "Array Sensor Untuk Fisika Energi Tinggi." },
    { "en": "Apa Itu Sensor Neutron?", "id": "Mendeteksi Partikel Neutron." },
    { "en": "Apa Itu Sensor Neutrino?", "id": "Detektor Sangat Besar Di Bawah Tanah." },
    { "en": "Apa Itu Sensor Gelombang Gravitasi?", "id": "Interferometer Laser Skala Besar." },
    { "en": "Apa Itu Sensor Resonansi Magnetik Nuklir?", "id": "NMR (Nuclear Magnetic Resonance) Sensor." },
    { "en": "Apa Itu Sensor Resonansi Paramagnetik Elektron?", "id": "EPR (Electron Paramagnetic Resonance) Sensor." },
    { "en": "Apa Itu Sensor Aliran Sitometri?", "id": "Menganalisis Sel Individual Dalam Aliran." },
    { "en": "Apa Itu Sensor PCR (Polymerase Chain Reaction)?", "id": "Mendeteksi Sekuens DNA (Deoxyribonucleic Acid) Spesifik." },
    { "en": "Apa Itu Array Mikro DNA?", "id": "Mengukur Ekspresi Ribuan Gen." },
    { "en": "Apa Itu Sistem Pengurutan Gen?", "id": "Menentukan Urutan Nukleotida DNA." },
    { "en": "Apa Itu Sensor Elektrokortikografi?", "id": "ECoG (Electrocorticography)." },
    { "en": "Apa Fungsi ECoG (Electrocorticography)?", "id": "Merekam Aktivitas Otak Dari Permukaan." },
    { "en": "Apa Itu Sensor Impedansi Bioelektrik?", "id": "Mengukur Komposisi Tubuh." },
    { "en": "Apa Itu Tomografi Impedansi Listrik?", "id": "EIT (Electrical Impedance Tomography)." },
    { "en": "Apa Itu Magnetoencephalography?", "id": "MEG (Magnetoencephalography)." },
    { "en": "Apa Fungsi MEG (Magnetoencephalography)?", "id": "Mengukur Medan Magnet Otak." },
    { "en": "Apa Itu Transduser Intra-vaskular?", "id": "Sensor Yang Dimasukkan Dalam Pembuluh Darah." },
    { "en": "Apa Itu Kapsul Endoskopi?", "id": "Kamera Nirkabel Yang Dapat Ditelan." },
    { "en": "Apa Itu Transduser Pencitraan Fotoakustik?", "id": "Menggunakan Laser Dan Ultrasonik." },
    { "en": "Apa Itu Aktuator Cerdas?", "id": "Aktuator Dengan Sensor Dan Kontroler." },
    { "en": "Apa Itu Material Cerdas?", "id": "Material Yang Merespon Stimulus." },
    { "en": "Apa Itu Cairan Magnetoreologikal?", "id": "MRF (Magnetorheological Fluid)." },
    { "en": "Bagaimana MRF (Magnetorheological Fluid) Bekerja?", "id": "Viskositas Berubah Akibat Medan Magnet." },
    { "en": "Apa Itu Cairan Elektroreologikal?", "id": "ERF (Electrorheological Fluid)." },
    { "en": "Apa Itu Polimer Dielektrik Elastomer?", "id": "DEA (Dielectric Elastomer Actuator)." },
    { "en": "Apa Itu Otot Buatan Ionik?", "id": "IPMC (Ionic Polymer-Metal Composite)." },
    { "en": "Apa Itu Aktuator Termal Bimetal?", "id": "Membengkok Saat Dipanaskan." },
    { "en": "Apa Itu Aktuator Paduan Memori Bentuk?", "id": "SMA (Shape-Memory Alloy) Actuator." },
    { "en": "Apa Itu Transduser Elektrostriktif?", "id": "Mirip Piezoelektrik Tapi Efek Kuadratik." },
    { "en": "Apa Itu Transduser Magnetostriktif?", "id": "Bahan Berubah Bentuk Dalam Medan Magnet." },
    { "en": "Apa Itu Pengeras Suara Magnetostriktif?", "id": "Speaker Tanpa Konus Konvensional." },
    { "en": "Apa Itu Motor Reluktansi?", "id": "Motor Berbasis Perubahan Reluktansi." },
    { "en": "Apa Itu Motor Reluktansi Tersaklar?", "id": "SRM (Switched Reluctance Motor)." },
    { "en": "Apa Itu Bantalan Magnetik?", "id": "Menopang Benda Menggunakan Levitasi Magnetik." },
    { "en": "Apa Itu Bantalan Magnetik Aktif?", "id": "Menggunakan Elektromagnet Dan Kontrol Umpan Balik." },
    { "en": "Apa Itu Bantalan Magnetik Pasif?", "id": "Menggunakan Magnet Permanen." },
    { "en": "Apa Itu Elektrodinamik Suspensi?", "id": "EDS (Electrodynamic Suspension)." },
    { "en": "Apa Itu Elektromagnetik Suspensi?", "id": "EMS (Electromagnetic Suspension)." },
    { "en": "Apa Itu Peredam Magnetoreologikal?", "id": "Peredam Dengan Kekuatan Terkontrol." },
    { "en": "Apa Itu Kopling Magnetoreologikal?", "id": "Kopling Dengan Torsi Terkontrol." },
    { "en": "Apa Itu Sensor Virtual?", "id": "Output Dihitung Dari Sensor Lain." },
    { "en": "Apa Itu Redundansi Analitik?", "id": "Menggunakan Model Matematis Untuk Validasi." },
    { "en": "Apa Itu Deteksi Dan Isolasi Kesalahan?", "id": "FDI (Fault Detection and Isolation)." },
    { "en": "Apa Itu Observabilitas?", "id": "Kemampuan Memperkirakan Keadaan Internal." },
    { "en": "Apa Itu Kontrolabilitas?", "id": "Kemampuan Mengontrol Keadaan Internal." },
    { "en": "Apa Itu Estimator Keadaan?", "id": "Memperkirakan Variabel Yang Tidak Terukur." },
    { "en": "Apa Itu Pengamat Luenberger?", "id": "Jenis Estimator Keadaan." },
    { "en": "Apa Itu Model Ruang Keadaan?", "id": "State-Space Representation." },
    { "en": "Apa Itu Jaringan Sensor Nirkabel?", "id": "WSN (Wireless Sensor Network)." },
    { "en": "Apa Itu Jaringan Area Tubuh Nirkabel?", "id": "WBAN (Wireless Body Area Network)." },
    { "en": "Apa Itu Sinkronisasi Waktu?", "id": "Menjaga Semua Simpul Memiliki Waktu Sama." },
    { "en": "Apa Itu Protokol Sinkronisasi Jaringan?", "id": "NTP (Network Time Protocol)." },
    { "en": "Apa Itu Protokol Waktu Presisi?", "id": "PTP (Precision Time Protocol)." },
    { "en": "Apa Itu Lokalisasi?", "id": "Menentukan Posisi Simpul Sensor." },
    { "en": "Apa Itu Agregasi Data?", "id": "Menggabungkan Data Di Dalam Jaringan." },
    { "en": "Apa Itu Routing Sadar Energi?", "id": "Memilih Jalur Untuk Memperpanjang Umur Jaringan." },
    { "en": "Apa Itu Siklus Tugas (Duty Cycling)?", "id": "Menidurkan Radio Secara Periodik." },
    { "en": "Apa Itu Antarmuka Pengguna?", "id": "UI (User Interface)." },
    { "en": "Apa Itu Pengalaman Pengguna?", "id": "UX (User Experience)." },
    { "en": "Apa Itu Transduser Untuk Realitas Virtual?", "id": "Pelacak Posisi, Layar, Audio 3D." },
    { "en": "Apa Itu Pelacakan Luar-Dalam?", "id": "Sensor Di Headset Melacak Lingkungan." },
    { "en": "Apa Itu Pelacakan Dalam-Luar?", "id": "Sensor Eksternal Melacak Headset." },
    { "en": "Apa Itu Transduser Untuk Realitas Tertambah?", "id": "Kamera, Proyektor, Sensor Kedalaman." },
    { "en": "Apa Itu Sistem Kendali Umpan Balik?", "id": "Feedback Control System." },
    { "en": "Apa Itu Sistem Kendali Umpan Maju?", "id": "Feedforward Control System." },
    { "en": "Apa Itu Sistem Kendali Loop Terbuka?", "id": "Open-Loop Control System." },
    { "en": "Apa Itu Sistem Kendali Loop Tertutup?", "id": "Closed-Loop Control System." },
    { "en": "Apa Itu Sensor Dalam Sistem Kendali?", "id": "Elemen Pengukur (Umpan Balik)." },
    { "en": "Apa Itu Aktuator Dalam Sistem Kendali?", "id": "Elemen Yang Mengubah Sinyal Kontrol." },
    { "en": "Apa Itu Kontroler Dalam Sistem Kendali?", "id": "Elemen Pemroses Kesalahan." },
    { "en": "Apa Itu Setpoint?", "id": "Nilai Yang Diinginkan." },
    { "en": "Apa Itu Variabel Proses?", "id": "Besaran Aktual Yang Diukur." },
    { "en": "Apa Itu Sinyal Kesalahan (Error Signal)?", "id": "Selisih Antara Setpoint Dan Variabel Proses." },
    { "en": "Apa Itu Kontroler On-Off?", "id": "Jenis Kontroler Paling Sederhana." },
    { "en": "Apa Itu Kontroler Proporsional (P)?", "id": "Output Sebanding Dengan Kesalahan." },
    { "en": "Apa Itu Kontroler Integral (I)?", "id": "Menghilangkan Kesalahan Keadaan Tunak." },
    { "en": "Apa Itu Kontroler Derivatif (D)?", "id": "Merespon Laju Perubahan Kesalahan." },
    { "en": "Apa Itu Kontroler PID (Proportional-Integral-Derivative)?", "id": "Kontroler Umpan Balik Paling Umum." },
    { "en": "Apa Itu Tuning Kontroler?", "id": "Menyesuaikan Parameter Kontroler (P, I, D)." },
    { "en": "Apa Itu Metode Ziegler-Nichols?", "id": "Metode Klasik Untuk Tuning PID." },
    { "en": "Apa Itu Sensor Cerdas (Smart Sensor)?", "id": "Sensor Dengan Kemampuan Pemrosesan." },
    { "en": "Apa Itu Sensor MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sensor Yang Dibuat Dengan Fabrikasi IC." },
    { "en": "Apa Keuntungan FOS (Fiber Optic Sensor)?", "id": "Kebal EMI, Aman Di Lingkungan Berbahaya." },
    { "en": "Apa Itu Sensor Suhu Terdistribusi?", "id": "DTS (Distributed Temperature Sensing)." },
    { "en": "Apa Itu Sensor Regangan Terdistribusi?", "id": "DSS (Distributed Strain Sensing)." },
    { "en": "Apa Itu Sensor Gas Inframerah?", "id": "Mendeteksi Gas Berdasarkan Penyerapan IR." },
    { "en": "Apa Itu Sensor Kelembaban Optik?", "id": "Mengukur Kelembaban Dengan Prinsip Optik." },
    { "en": "Apa Itu Giroskop Resonansi?", "id": "Giroskop Berbasis Elemen Bergetar (MEMS)." },
    { "en": "Apa Itu Efek Coriolis?", "id": "Prinsip Kerja Giroskop Resonansi." },
    { "en": "Apa Itu Sensor Tekanan Resonansi?", "id": "Frekuensi Resonansi Berubah Dengan Tekanan." },
    { "en": "Apa Itu Sensor Massa Resonansi?", "id": "QCM (Quartz Crystal Microbalance)." },
    { "en": "Apa Itu Sensor Kepadatan Resonansi?", "id": "Mengukur Kepadatan Cairan." },
    { "en": "Apa Itu Sensor Kimia Resonansi?", "id": "Mendeteksi Zat Kimia Dengan Pergeseran Frekuensi." },
    { "en": "Apa Itu Sensor Kentalitas Resonansi?", "id": "Mengukur Viskositas Dengan Redaman." },
    { "en": "Apa Itu Transduser Kapasitif?", "id": "Bekerja Berdasarkan Perubahan Kapasitansi." },
    { "en": "Apa Itu Mikrofon Kapasitif?", "id": "Mikrofon Kondensor." },
    { "en": "Apa Itu Sensor Jarak Kapasitif?", "id": "Mendeteksi Perubahan Kapasitansi." },
    { "en": "Apa Itu Sensor Kelembaban Kapasitif?", "id": "Konstanta Dielektrik Berubah Dengan Kelembaban." },
    { "en": "Apa Itu Sensor Level Kapasitif?", "id": "Mengukur Ketinggian Cairan Dielektrik." },
    { "en": "Apa Itu Akselerometer Kapasitif?", "id": "Massa Bergerak Mengubah Jarak Pelat." },
    { "en": "Apa Itu Sensor Tekanan Kapasitif?", "id": "Diafragma Bergerak Mengubah Jarak Pelat." },
    { "en": "Apa Itu Transduser Induktif?", "id": "Bekerja Berdasarkan Perubahan Induktansi." },
    { "en": "Apa Itu LVDT (Linear Variable Differential Transformer)?", "id": "Sensor Posisi Linear Induktif." },
    { "en": "Apa Itu RVDT (Rotary Variable Differential Transformer)?", "id": "Sensor Posisi Sudut Induktif." },
    { "en": "Apa Itu Sensor Jarak Induktif?", "id": "Mendeteksi Logam Dengan Arus Eddy." },
    { "en": "Apa Itu Sensor Magnetostriktif?", "id": "Sifat Magnetik Berubah Dengan Regangan." },
    { "en": "Apa Itu Transduser Elektrodinamik?", "id": "Menggunakan Prinsip Gaya Lorentz." },
    { "en": "Contoh Transduser Elektrodinamik?", "id": "Speaker Dinamis, Mikrofon Dinamis." },
    { "en": "Apa Itu Shaker Elektrodinamik?", "id": "Alat Uji Getaran." },
    { "en": "Apa Itu Sensor Efek Wiegand?", "id": "Kawat Magnetik Dengan Perilaku Switching." },
    { "en": "Apa Itu Efek Villari?", "id": "Kebalikan Dari Efek Magnetostriktif." },
    { "en": "Apa Itu Sensor Efek Fotolistrik?", "id": "Tabung Pengganda Foton (PMT)." },
    { "en": "Apa Itu Sensor Ionisasi?", "id": "Mendeteksi Radiasi Pengion." },
    { "en": "Apa Itu Sensor Termal?", "id": "Mendeteksi Panas Atau Suhu." },
    { "en": "Apa Itu Sensor Mekanis?", "id": "Mendeteksi Besaran Mekanis." },
    { "en": "Apa Itu Sensor Radiasi?", "id": "Mendeteksi Radiasi Elektromagnetik." },
    { "en": "Apa Itu Transduser Pasif?", "id": "Tidak Membutuhkan Sumber Energi Eksternal." },
    { "en": "Apa Itu Transduser Aktif?", "id": "Membutuhkan Sumber Energi Eksternal." },
    { "en": "Apa Itu Sensor Laju Alir?", "id": "Flow Sensor." },
    { "en": "Apa Itu Sensor pH?", "id": "Mengukur Keasaman Atau Kebasaan." },
    { "en": "Apa Itu Sensor ORP (Oxidation-Reduction Potential)?", "id": "Mengukur Potensial Redoks." },
    { "en": "Apa Itu Sel Beban?", "id": "Load Cell." },
    { "en": "Apa Itu Sensor Torsi?", "id": "Torque Sensor." },
    { "en": "Apa Itu Sensor Gesekan?", "id": "Friction Sensor." },
    { "en": "Apa Itu Tribometer?", "id": "Alat Untuk Mengukur Gesekan." },
    { "en": "Apa Itu Sensor Ketebalan Film Tipis?", "id": "Menggunakan Kristal Kuarsa Atau Optik." },
    { "en": "Apa Itu Elipsometer?", "id": "Mengukur Sifat Lapisan Tipis." },
    { "en": "Apa Itu Sensor Posisi Laser?", "id": "Menggunakan Triangulasi Laser." },
    { "en": "Apa Itu Sensor Visi 3D?", "id": "Membangun Peta Kedalaman (Depth Map)." },
    { "en": "Apa Itu Sensor Stereovision?", "id": "Menggunakan Dua Kamera Untuk Kedalaman." },
    { "en": "Apa Itu Sensor Pencitraan Hiperspektral?", "id": "Mengambil Gambar Dalam Ratusan Warna." },
    { "en": "Apa Itu Sensor Cahaya Sekitar?", "id": "ALS (Ambient Light Sensor)." },
    { "en": "Fungsi ALS (Ambient Light Sensor)?", "id": "Menyesuaikan Kecerahan Layar Otomatis." },
    { "en": "Apa Itu Sensor Warna RGB?", "id": "Mengukur Komponen Merah, Hijau, Biru." },
    { "en": "Apa Itu Sensor Gestur?", "id": "Mendeteksi Gerakan Tangan." },
    { "en": "Apa Itu Pengkondisi Sinyal Universal?", "id": "Dapat Dikonfigurasi Untuk Berbagai Sensor." },
    { "en": "Apa Itu Transmisi Sinyal Nirkabel?", "id": "Mengirim Data Sensor Tanpa Kabel." },
    { "en": "Apa Itu Protokol LoRaWAN?", "id": "Protokol LPWAN (Low-Power Wide-Area Network)." },
    { "en": "Apa Itu Protokol NB-IoT?", "id": "Narrowband-IoT (Internet of Things)." },
    { "en": "Apa Itu Protokol Bluetooth Low Energy?", "id": "BLE (Bluetooth Low Energy)." },
    { "en": "Apa Itu Protokol Zigbee?", "id": "Standar Nirkabel Berdaya Rendah." },
    { "en": "Apa Itu Sensor Jaringan Mesh?", "id": "Sensor Dapat Meneruskan Data Sensor Lain." },
    { "en": "Apa Itu Edge Computing?", "id": "Pemrosesan Data Dilakukan Di Dekat Sensor." },
    { "en": "Apa Itu Cloud Computing?", "id": "Pemrosesan Data Dilakukan Di Server Pusat." },
    { "en": "Apa Itu Antarmuka Sensor-ke-Cloud?", "id": "Menghubungkan Sensor Langsung Ke Internet." },
    { "en": "Apa Itu Protokol MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol Populer Untuk IoT (Internet of Things)." },
    { "en": "Apa Itu Sensor Inersia?", "id": "Mengukur Gerakan (Akselerometer, Giroskop)." },
    { "en": "Apa Itu Sensor Magnetik, Inersia, Sudut?", "id": "MARG (Magnetic, Angular Rate, and Gravity)." },
    { "en": "Apa Itu Sensor Efek Fotorefraktif?", "id": "Indeks Bias Berubah Karena Cahaya." },
    { "en": "Apa Itu Sensor Berbasis Metamaterial?", "id": "Sensor Dengan Sensitivitas Sangat Tinggi." },
    { "en": "Apa Itu Sensor Gelombang Permukaan?", "id": "Mendeteksi Perubahan Pada Permukaan." },
    { "en": "Apa Itu Resonator Mikro-elektromekanis?", "id": "Digunakan Dalam Sensor Resonansi." },
    { "en": "Apa Itu Titik Referensi?", "id": "Nilai Dikenal Untuk Kalibrasi." },
    { "en": "Apa Itu Standar Transfer?", "id": "Perangkat Yang Digunakan Untuk Mengkalibrasi Lainnya." },
    { "en": "Apa Itu Ketertelusuran (Traceability)?", "id": "Kemampuan Melacak Kalibrasi Ke Standar Nasional." },
    { "en": "Apa Itu Ketidakpastian Pengukuran?", "id": "Rentang Keraguan Pada Hasil Pengukuran." },
    { "en": "Apa Itu Sensor Redundan?", "id": "Beberapa Sensor Mengukur Besaran Sama." },
    { "en": "Apa Itu Pemungutan Suara (Voting) Sensor?", "id": "Memilih Bacaan Mayoritas Dari Sensor Redundan." },
    { "en": "Apa Itu Rata-rata Sinyal?", "id": "Mengurangi Derau Dengan Merata-ratakan." },
    { "en": "Apa Itu Filtering Digital?", "id": "Memproses Sampel Sensor Secara Matematis." },
    { "en": "Apa Itu Antarmuka Cerdas?", "id": "Sensor Dengan Kemampuan Komunikasi Digital." },
    { "en": "Apa Itu Antarmuka Manusia-Mesin?", "id": "HMI (Human-Machine Interface)." },
    { "en": "Apa Itu Sensor Visual?", "id": "Kamera." },
    { "en": "Apa Itu Sensor Audio?", "id": "Mikrofon." },
    { "en": "Apa Itu Sensor Haptic?", "id": "Memberikan Umpan Balik Getaran Atau Gaya." },
    { "en": "Apa Itu Layar Sentuh Resistif?", "id": "Mendeteksi Tekanan Fisik." },
    { "en": "Apa Itu Layar Sentuh Kapasitif?", "id": "Mendeteksi Gangguan Medan Listrik." },
    { "en": "Apa Itu Layar Sentuh Gelombang Akustik Permukaan?", "id": "SAW (Surface Acoustic Wave) Touchscreen." },
    { "en": "Apa Itu Layar Sentuh Inframerah?", "id": "Mendeteksi Objek Yang Menghalangi Kisi IR." },
    { "en": "Apa Itu Sensor Gaya 3D?", "id": "Mengukur Gaya Dalam Tiga Dimensi." },
    { "en": "Apa Itu Sensor Gerakan?", "id": "Mendeteksi Gerakan Fisik." },
    { "en": "Apa Itu Sensor Okupansi?", "id": "Mendeteksi Kehadiran Orang Di Ruangan." },
    { "en": "Apa Itu Tombol Tekan?", "id": "Sensor Gaya Biner." },
    { "en": "Apa Itu Saklar?", "id": "Sensor Posisi Biner." },
    { "en": "Apa Itu Joystick?", "id": "Sensor Posisi Dua Dimensi." },
    { "en": "Apa Itu Trackball?", "id": "Sensor Gerak Relatif." },
    { "en": "Apa Itu Touchpad?", "id": "Sensor Posisi Relatif Kapasitif." },
    { "en": "Apa Itu Pena Digital (Stylus)?", "id": "Sensor Posisi Absolut." },
    { "en": "Apa Itu Digitizer?", "id": "Tablet Grafis." },
    { "en": "Apa Itu Pemindai (Scanner)?", "id": "Mengubah Dokumen Fisik Menjadi Digital." },
    { "en": "Apa Itu Pembaca Kartu Magnetik?", "id": "Membaca Data Dari Garis Magnetik." },
    { "en": "Apa Itu Pembaca Kode Batang?", "id": "Membaca Pola Garis Hitam Putih." },
    { "en": "Apa Itu Pembaca Kode QR?", "id": "Membaca Matriks Data 2D." },
    { "en": "Apa Itu Pengenalan Karakter Tinta Magnetik?", "id": "MICR (Magnetic Ink Character Recognition)." },
    { "en": "Apa Itu Pengenalan Tanda Optik?", "id": "OMR (Optical Mark Recognition)." },
    { "en": "Apa Itu Pengenalan Tulisan Tangan?", "id": "HCR (Handwriting Character Recognition)." },
    { "en": "Apa Itu Pengenalan Ucapan?", "id": "Speech Recognition." },
    { "en": "Apa Itu Verifikasi Pembicara?", "id": "Mengenali Siapa Yang Berbicara." },
    { "en": "Apa Itu Sensor Elektro-optik?", "id": "Mengubah Sinar Optik Menjadi Listrik." },
    { "en": "Apa Itu Sensor Elektrokimia?", "id": "Mengukur Reaksi Kimia." },
    { "en": "Apa Itu Sensor Elektromekanis?", "id": "Mengubah Energi Mekanis-Listrik." },
    { "en": "Apa Itu Transduser Bolak-balik?", "id": "Dapat Berfungsi Sebagai Sensor Dan Aktuator." },
    { "en": "Apa Itu Linearitas?", "id": "Hubungan Lurus Antara Input Dan Output." }



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
