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


    { "en": "Kata Baku Dari 'Apotik'?", "id": "Apotek" },
    { "en": "Kata Baku Dari 'Atlit'?", "id": "Atlet" },
    { "en": "Kata Baku Dari 'Analisa'?", "id": "Analisis" },
    { "en": "Kata Baku Dari 'Bis'?", "id": "Bus" },
    { "en": "Kata Baku Dari 'Cabe'?", "id": "Cabai" },
    { "en": "Kata Baku Dari 'Cuman'?", "id": "Cuma" },
    { "en": "Kata Baku Dari 'Diagnosa'?", "id": "Diagnosis" },
    { "en": "Kata Baku Dari 'Ekstrim'?", "id": "Ekstrem" },
    { "en": "Kata Baku Dari 'Pebruari'?", "id": "Februari" },
    { "en": "Kata Baku Dari 'Gubug'?", "id": "Gubuk" },
    { "en": "Kata Baku Dari 'Hadist'?", "id": "Hadis" },
    { "en": "Kata Baku Dari 'Hakekat'?", "id": "Hakikat" },
    { "en": "Kata Baku Dari 'Ijin'?", "id": "Izin" },
    { "en": "Kata Baku Dari 'Jaman'?", "id": "Zaman" },
    { "en": "Kata Baku Dari 'Karir'?", "id": "Karier" },
    { "en": "Kata Baku Dari 'Komplit'?", "id": "Komplet" },
    { "en": "Kata Baku Dari 'Kwitansi'?", "id": "Kuitansi" },
    { "en": "Kata Baku Dari 'Lobang'?", "id": "Lubang" },
    { "en": "Kata Baku Dari 'Nasehat'?", "id": "Nasihat" },
    { "en": "Kata Baku Dari 'Nafas'?", "id": "Napas" },
    { "en": "Kata Baku Dari 'Nomer'?", "id": "Nomor" },
    { "en": "Kata Baku Dari 'Obyek'?", "id": "Objek" },
    { "en": "Kata Baku Dari 'Praktek'?", "id": "Praktik" },
    { "en": "Kata Baku Dari 'Resiko'?", "id": "Risiko" },
    { "en": "Kata Baku Dari 'Rubah'?", "id": "Ubah" },
    { "en": "Kata Baku Dari 'Saos'?", "id": "Saus" },
    { "en": "Kata Baku Dari 'Sekedar'?", "id": "Sekadar" },
    { "en": "Kata Baku Dari 'Silahkan'?", "id": "Silakan" },
    { "en": "Kata Baku Dari 'Sistim'?", "id": "Sistem" },
    { "en": "Kata Baku Dari 'Subyek'?", "id": "Subjek" },
    { "en": "Kata Baku Dari 'Tehnik'?", "id": "Teknik" },
    { "en": "Kata Baku Dari 'Trampil'?", "id": "Terampil" },
    { "en": "Kata Baku Dari 'Terimakasih'?", "id": "Terima Kasih" },
    { "en": "Kata Baku Dari 'Ustad'?", "id": "Ustaz" },
    { "en": "Kata Baku Dari 'Jendral'?", "id": "Jenderal" },
    { "en": "Kata Baku Dari 'Aktip'?", "id": "Aktif" },
    { "en": "Kata Baku Dari 'Pormal'?", "id": "Formal" },
    { "en": "Kata Baku Dari 'Pasip'?", "id": "Pasif" },
    { "en": "Kata Baku Dari 'Azas'?", "id": "Asas" },
    { "en": "Kata Baku Dari 'Kongkrit'?", "id": "Konkret" },
    { "en": "Kata Baku Dari 'Antri'?", "id": "Antre" },
    { "en": "Kata Baku Dari 'Fikir'?", "id": "Pikir" },
    { "en": "Kata Baku Dari 'Photo'?", "id": "Foto" },
    { "en": "Kata Baku Dari 'Propinsi'?", "id": "Provinsi" },
    { "en": "Kata Baku Dari 'Kwalitas'?", "id": "Kualitas" },
    { "en": "Kata Baku Dari 'Hipotesa'?", "id": "Hipotesis" },
    { "en": "Kata Baku Dari 'Hisap'?", "id": "Isap" },
    { "en": "Kata Baku Dari 'Jadual'?", "id": "Jadwal" },
    { "en": "Kata Baku Dari 'Ijasah'?", "id": "Ijazah" },
    { "en": "Kata Baku Dari 'Indera'?", "id": "Indra" },
    { "en": "Kata Baku Dari 'Himbau'?", "id": "Imbau" },
    { "en": "Kata Baku Dari 'Hembus'?", "id": "Embus" },
    { "en": "Kata Baku Dari 'Elit'?", "id": "Elite" },
    { "en": "Kata Baku Dari 'Detil'?", "id": "Detail" },
    { "en": "Kata Baku Dari 'Frekwensi'?", "id": "Frekuensi" },
    { "en": "Kata Baku Dari 'Ghaib'?", "id": "Gaib" },
    { "en": "Kata Baku Dari 'Geladi'?", "id": "Gladi" },
    { "en": "Kata Baku Dari 'Kwartal'?", "id": "Kuartal" },
    { "en": "Kata Baku Dari 'Lajim'?", "id": "Lazim" },
    { "en": "Kata Baku Dari 'Lempab'?", "id": "Lembap" },
    { "en": "Kata Baku Dari 'Mangkok'?", "id": "Mangkuk" },
    { "en": "Kata Baku Dari 'Mesjid'?", "id": "Masjid" },
    { "en": "Kata Baku Dari 'Milyar'?", "id": "Miliar" },
    { "en": "Kata Baku Dari 'Nopember'?", "id": "November" },
    { "en": "Kata Baku Dari 'Original'?", "id": "Orisinal" },
    { "en": "Kata Baku Dari 'Personil'?", "id": "Personel" },
    { "en": "Kata Baku Dari 'Putera'?", "id": "Putra" },
    { "en": "Kata Baku Dari 'Rapih'?", "id": "Rapi" },
    { "en": "Kata Baku Dari 'Realita'?", "id": "Realitas" },
    { "en": "Kata Baku Dari 'Rejeki'?", "id": "Rezeki" },
    { "en": "Kata Baku Dari 'Respon'?", "id": "Respons" },
    { "en": "Kata Baku Dari 'Sahadat'?", "id": "Syahadat" },
    { "en": "Kata Baku Dari 'Shampo'?", "id": "Sampo" },
    { "en": "Kata Baku Dari 'Sorga'?", "id": "Surga" },
    { "en": "Kata Baku Dari 'Standarisasi'?", "id": "Standardisasi" },
    { "en": "Kata Baku Dari 'Telpon'?", "id": "Telepon" },
    { "en": "Kata Baku Dari 'Team'?", "id": "Tim" },
    { "en": "Kata Baku Dari 'Telor'?", "id": "Telur" },
    { "en": "Kata Baku Dari 'Tahta'?", "id": "Takhta" },
    { "en": "Kata Baku Dari 'Thema'?", "id": "Tema" },
    { "en": "Kata Baku Dari 'Trilyun'?", "id": "Triliun" },
    { "en": "Kata Baku Dari 'Tropi'?", "id": "Trofi" },
    { "en": "Kata Baku Dari 'Villa'?", "id": "Vila" },
    { "en": "Kata Baku Dari 'Walikota'?", "id": "Wali Kota" },
    { "en": "Kata Baku Dari 'Idial'?", "id": "Ideal" },
    { "en": "Kata Baku Dari 'Kreatifitas'?", "id": "Kreativitas" },
    { "en": "Kata Baku Dari 'Coklat'?", "id": "Cokelat" },
    { "en": "Kata Baku Dari 'Cidera'?", "id": "Cedera" },
    { "en": "Kata Baku Dari 'Jaman Dulu'?", "id": "Zaman Dulu" },
    { "en": "Kata Baku Dari 'Paska'?", "id": "Pasca" },
    { "en": "Kata Baku Dari 'Pondasi'?", "id": "Fondasi" },
    { "en": "Kata Baku Dari 'Frase'?", "id": "Frasa" },
    { "en": "Kata Baku Dari 'Gensek'?", "id": "Sekjen" },
    { "en": "Kata Baku Dari 'Obyektif'?", "id": "Objektif" },
    { "en": "Kata Baku Dari 'Subyektif'?", "id": "Subjektif" },
    { "en": "Kata Baku Dari 'Konkrit'?", "id": "Konkret" },
    { "en": "Kata Baku Dari 'Kwantitas'?", "id": "Kuantitas" },
    { "en": "Kata Baku Dari 'Marjin'?", "id": "Margin" },
    { "en": "Kata Baku Dari 'Metoda'?", "id": "Metode" },
    { "en": "Kata Baku Dari 'Ambulan'?", "id": "Ambulans" },
    { "en": "Kata Baku Dari 'Adzan'?", "id": "Azan" },
    { "en": "Kata Baku Dari 'Aktivitas'?", "id": "Aktivitas" },
    { "en": "Kata Baku Dari 'Amandemen'?", "id": "Amendemen" },
    { "en": "Kata Baku Dari 'Baterei'?", "id": "Baterai" },
    { "en": "Kata Baku Dari 'Berfikir'?", "id": "Berpikir" },
    { "en": "Kata Baku Dari 'Blanko'?", "id": "Blangko" },
    { "en": "Kata Baku Dari 'Cendikiawan'?", "id": "Cendekiawan" },
    { "en": "Kata Baku Dari 'Dahsyat'?", "id": "Dahsyat" },
    { "en": "Kata Baku Dari 'Debet'?", "id": "Debit" },
    { "en": "Kata Baku Dari 'Disain'?", "id": "Desain" },
    { "en": "Kata Baku Dari 'Duren'?", "id": "Durian" },
    { "en": "Kata Baku Dari 'Efektifitas'?", "id": "Efektivitas" },
    { "en": "Kata Baku Dari 'Ekspor'?", "id": "Ekspor" },
    { "en": "Kata Baku Dari 'Expres'?", "id": "Ekspres" },
    { "en": "Kata Baku Dari 'Faham'?", "id": "Paham" },
    { "en": "Kata Baku Dari 'Faksimili'?", "id": "Faksimile" },
    { "en": "Kata Baku Dari 'Handal'?", "id": "Andal" },
    { "en": "Kata Baku Dari 'Hutang'?", "id": "Utang" },
    { "en": "Kata Baku Dari 'Hierarki'?", "id": "Hierarki" },
    { "en": "Kata Baku Dari 'Insyaf'?", "id": "Insaf" },
    { "en": "Kata Baku Dari 'Intropeksi'?", "id": "Introspeksi" },
    { "en": "Kata Baku Dari 'Isteri'?", "id": "Istri" },
    { "en": "Kata Baku Dari 'Jum'at'?", "id": "Jumat" },
    { "en": "Kata Baku Dari 'Kaos'?", "id": "Kaus" },
    { "en": "Kata Baku Dari 'Khatulistiwa'?", "id": "Katulistiwa" },
    { "en": "Kata Baku Dari 'Kholesterol'?", "id": "Kolesterol" },
    { "en": "Kata Baku Dari 'Klub'?", "id": "Kelab" },
    { "en": "Kata Baku Dari 'Komoditi'?", "id": "Komoditas" },
    { "en": "Kata Baku Dari 'Komplek'?", "id": "Kompleks" },
    { "en": "Kata Baku Dari 'Koprasi'?", "id": "Koperasi" },
    { "en": "Kata Baku Dari 'Kreatif'?", "id": "Kreatif" },
    { "en": "Kata Baku Dari 'Kuatir'?", "id": "Khawatir" },
    { "en": "Kata Baku Dari 'Kyai'?", "id": "Kiai" },
    { "en": "Kata Baku Dari 'Legaliser'?", "id": "Legalisasi" },
    { "en": "Kata Baku Dari 'Managemen'?", "id": "Manajemen" },
    { "en": "Kata Baku Dari 'Milyuner'?", "id": "Miliuner" },
    { "en": "Kata Baku Dari 'Missi'?", "id": "Misi" },
    { "en": "Kata Baku Dari 'Moderen'?", "id": "Modern" },
    { "en": "Kata Baku Dari 'Motifasi'?", "id": "Motivasi" },
    { "en": "Kata Baku Dari 'Obyektif'?", "id": "Objektif" },
    { "en": "Kata Baku Dari 'Otomatis'?", "id": "Otomatis" },
    { "en": "Kata Baku Dari 'Pahlawan'?", "id": "Pahlawan" },
    { "en": "Kata Baku Dari 'Pasport'?", "id": "Paspor" },
    { "en": "Kata Baku Dari 'Prosen'?", "id": "Persen" },
    { "en": "Kata Baku Dari 'Puteri'?", "id": "Putri" },
    { "en": "Kata Baku Dari 'Roh'?", "id": "Ruh" },
    { "en": "Kata Baku Dari 'Saksama'?", "id": "Saksama" },
    { "en": "Kata Baku Dari 'Sekertaris'?", "id": "Sekretaris" },
    { "en": "Kata Baku Dari 'Sintesa'?", "id": "Sintesis" },
    { "en": "Kata Baku Dari 'Standard'?", "id": "Standar" },
    { "en": "Kata Baku Dari 'Study'?", "id": "Studi" },
    { "en": "Kata Baku Dari 'Survai'?", "id": "Survei" },
    { "en": "Kata Baku Dari 'Sutera'?", "id": "Sutra" },
    { "en": "Kata Baku Dari 'Syaraf'?", "id": "Saraf" },
    { "en": "Kata Baku Dari 'Tehnologi'?", "id": "Teknologi" },
    { "en": "Kata Baku Dari 'Telanjur'?", "id": "Terlanjur" },
    { "en": "Kata Baku Dari 'Tolerir'?", "id": "Toleransi" },
    { "en": "Kata Baku Dari 'Tradisionil'?", "id": "Tradisional" },
    { "en": "Kata Baku Dari 'Trio'?", "id": "Trio" },
    { "en": "Kata Baku Dari 'Varitas'?", "id": "Varietas" },
    { "en": "Kata Baku Dari 'Yogya'?", "id": "Yogya" },
    { "en": "Kata Baku Dari 'Akomodir'?", "id": "Akomodasi" },
    { "en": "Kata Baku Dari 'Aksesoris'?", "id": "Aksesori" },
    { "en": "Kata Baku Dari 'Al Quran'?", "id": "Alquran" },
    { "en": "Kata Baku Dari 'Atmosfir'?", "id": "Atmosfer" },
    { "en": "Kata Baku Dari 'Balsem'?", "id": "Balsam" },
    { "en": "Kata Baku Dari 'Cedera'?", "id": "Cedera" },
    { "en": "Kata Baku Dari 'Definisi'?", "id": "Definisi" },
    { "en": "Kata Baku Dari 'E-mail'?", "id": "Surel" },
    { "en": "Kata Baku Dari 'Esense'?", "id": "Esens" },
    { "en": "Kata Baku Dari 'Glukoma'?", "id": "Glaukoma" },
    { "en": "Kata Baku Dari 'Goa'?", "id": "Gua" },
    { "en": "Kata Baku Dari 'Hapal'?", "id": "Hafal" },
    { "en": "Kata Baku Dari 'Ikhlas'?", "id": "Ikhlas" },
    { "en": "Kata Baku Dari 'Import'?", "id": "Impor" },
    { "en": "Kata Baku Dari 'Intelijen'?", "id": "Intelijen" },
    { "en": "Kata Baku Dari 'Jip'?", "id": "Jip" },
    { "en": "Kata Baku Dari 'Jomblo'?", "id": "Jomlo" },
    { "en": "Kata Baku Dari 'Jubileum'?", "id": "Yubileum" },
    { "en": "Kata Baku Dari 'Judikatif'?", "id": "Yudikatif" },
    { "en": "Kata Baku Dari 'Junior'?", "id": "Yunior" },
    { "en": "Kata Baku Dari 'Justru'?", "id": "Justru" },
    { "en": "Kata Baku Dari 'Kanker'?", "id": "Kanker" },
    { "en": "Kata Baku Dari 'Kassa'?", "id": "Kasa" },
    { "en": "Kata Baku Dari 'Katagori'?", "id": "Kategori" },
    { "en": "Kata Baku Dari 'Kendor'?", "id": "Kendur" },
    { "en": "Kata Baku Dari 'Ksatria'?", "id": "Kesatria" },
    { "en": "Kata Baku Dari 'Kenalpot'?", "id": "Knalpot" },
    { "en": "Kata Baku Dari 'Koboy'?", "id": "Koboi" },
    { "en": "Kata Baku Dari 'Kholera'?", "id": "Kolera" },
    { "en": "Kata Baku Dari 'Komersil'?", "id": "Komersial" },
    { "en": "Kata Baku Dari 'Konperensi'?", "id": "Konferensi" },
    { "en": "Kata Baku Dari 'Konsekwen'?", "id": "Konsekuen" },
    { "en": "Kata Baku Dari 'Kosa Kata'?", "id": "Kosakata" },
    { "en": "Kata Baku Dari 'Kropos'?", "id": "Keropos" },
    { "en": "Kata Baku Dari 'Kwadrat'?", "id": "Kuadrat" },
    { "en": "Kata Baku Dari 'Kwalifikasi'?", "id": "Kualifikasi" },
    { "en": "Kata Baku Dari 'Afkiran'?", "id": "Apkir" },
    { "en": "Kata Baku Dari 'Alarem'?", "id": "Alarm" },
    { "en": "Kata Baku Dari 'Aquarium'?", "id": "Akuarium" },
    { "en": "Kata Baku Dari 'Azasi'?", "id": "Asasi" },
    { "en": "Kata Baku Dari 'Blesteran'?", "id": "Blasteran" },
    { "en": "Kata Baku Dari 'Capek'?", "id": "Lelah" },
    { "en": "Kata Baku Dari 'Cengkram'?", "id": "Cengkeram" },
    { "en": "Kata Baku Dari 'Dekrit'?", "id": "Dekret" },
    { "en": "Kata Baku Dari 'Diesel'?", "id": "Disel" },
    { "en": "Kata Baku Dari 'Efektip'?", "id": "Efektif" },
    { "en": "Kata Baku Dari 'Ekwivalen'?", "id": "Ekuivalen" },
    { "en": "Kata Baku Dari 'Elip'?", "id": "Elips" },
    { "en": "Kata Baku Dari 'Empas'?", "id": "Hempas" },
    { "en": "Kata Baku Dari 'Genteng'?", "id": "Genting" },
    { "en": "Kata Baku Dari 'Gips'?", "id": "Gipsum" },
    { "en": "Kata Baku Dari 'Glondong'?", "id": "Gelondong" },
    { "en": "Kata Baku Dari 'Hektar'?", "id": "Hektare" },
    { "en": "Kata Baku Dari 'Higenis'?", "id": "Higienis" },
    { "en": "Kata Baku Dari 'Idiologi'?", "id": "Ideologi" },
    { "en": "Kata Baku Dari 'Jenasah'?", "id": "Jenazah" },
    { "en": "Kata Baku Dari 'Jagad'?", "id": "Jagat" },
    { "en": "Kata Baku Dari 'Jahiliyah'?", "id": "Jahiliah" },
    { "en": "Kata Baku Dari 'Kaca Mata'?", "id": "Kacamata" },
    { "en": "Kata Baku Dari 'Kadaluarsa'?", "id": "Kedaluwarsa" },
    { "en": "Kata Baku Dari 'Kangguru'?", "id": "Kanguru" },
    { "en": "Kata Baku Dari 'Kantong'?", "id": "Kantung" },
    { "en": "Kata Baku Dari 'Kharisma'?", "id": "Karisma" },
    { "en": "Kata Baku Dari 'Ketapel'?", "id": "Katapel" },
    { "en": "Kata Baku Dari 'Kebon'?", "id": "Kebun" },
    { "en": "Kata Baku Dari 'Kedele'?", "id": "Kedelai" },
    { "en": "Kata Baku Dari 'Kemaren'?", "id": "Kemarin" },
    { "en": "Kata Baku Dari 'Korp'?", "id": "Korps" },
    { "en": "Kata Baku Dari 'Kwaci'?", "id": "Kuaci" },
    { "en": "Kata Baku Dari 'Legende'?", "id": "Legenda" },
    { "en": "Kata Baku Dari 'Lembab'?", "id": "Lembap" },
    { "en": "Kata Baku Dari 'Losin'?", "id": "Lusin" },
    { "en": "Kata Baku Dari 'Ma'na'?", "id": "Makna" },
    { "en": "Kata Baku Dari 'Mala Praktek'?", "id": "Malapraktik" },
    { "en": "Kata Baku Dari 'Mandeg'?", "id": "Mandek" },
    { "en": "Kata Baku Dari 'Mantera'?", "id": "Mantra" },
    { "en": "Kata Baku Dari 'Masal'?", "id": "Massal" },
    { "en": "Kata Baku Dari 'Mateng'?", "id": "Matang" },
    { "en": "Kata Baku Dari 'Mahzab'?", "id": "Mazhab" },
    { "en": "Kata Baku Dari 'Merk'?", "id": "Merek" },
    { "en": "Kata Baku Dari 'Monarkhi'?", "id": "Monarki" },
    { "en": "Kata Baku Dari 'Mubaligh'?", "id": "Mubalig" },
    { "en": "Kata Baku Dari 'Nampak'?", "id": "Tampak" },
    { "en": "Kata Baku Dari 'Netralisir'?", "id": "Netralisasi" },
    { "en": "Kata Baku Dari 'Netto'?", "id": "Neto" },
    { "en": "Kata Baku Dari 'Omset'?", "id": "Omzet" },
    { "en": "Kata Baku Dari 'Paragrap'?", "id": "Paragraf" },
    { "en": "Kata Baku Dari 'Pastur'?", "id": "Pastor" },
    { "en": "Kata Baku Dari 'Pleton'?", "id": "Peleton" },
    { "en": "Kata Baku Dari 'Bergedel'?", "id": "Perkedel" },
    { "en": "Kata Baku Dari 'Permak'?", "id": "Permak" },
    { "en": "Kata Baku Dari 'Piama'?", "id": "Piyama" },
    { "en": "Kata Baku Dari 'Prangko'?", "id": "Perangko" },
    { "en": "Kata Baku Dari 'Produktifitas'?", "id": "Produktivitas" },
    { "en": "Kata Baku Dari 'Projek'?", "id": "Proyek" },
    { "en": "Kata Baku Dari 'Psikotest'?", "id": "Psikotes" },
    { "en": "Kata Baku Dari 'Qasidah'?", "id": "Kasidah" },
    { "en": "Kata Baku Dari 'Radio Aktif'?", "id": "Radioaktif" },
    { "en": "Kata Baku Dari 'Rapot'?", "id": "Rapor" },
    { "en": "Kata Baku Dari 'Resiko'?", "id": "Risiko" },
    { "en": "Kata Baku Dari 'Reumatik'?", "id": "Rematik" },
    { "en": "Kata Baku Dari 'Roba'?", "id": "Raba" },
    { "en": "Kata Baku Dari 'Sanksi'?", "id": "Sanksi" },
    { "en": "Kata Baku Dari 'Santen'?", "id": "Santan" },
    { "en": "Kata Baku Dari 'Sate'?", "id": "Satai" },
    { "en": "Kata Baku Dari 'Sapu Tangan'?", "id": "Saputangan" },
    { "en": "Kata Baku Dari 'Sekor'?", "id": "Skor" },
    { "en": "Kata Baku Dari 'Selinder'?", "id": "Silinder" },
    { "en": "Kata Baku Dari 'Seprei'?", "id": "Seprai" },
    { "en": "Kata Baku Dari 'Seksofon'?", "id": "Saksofon" },
    { "en": "Kata Baku Dari 'Siklus'?", "id": "Siklus" },
    { "en": "Kata Baku Dari 'Spagheti'?", "id": "Spageti" },
    { "en": "Kata Baku Dari 'Spesies'?", "id": "Spesies" },
    { "en": "Kata Baku Dari 'Spor'?", "id": "Sepur" },
    { "en": "Kata Baku Dari 'Stel'?", "id": "Setel" },
    { "en": "Kata Baku Dari 'Stres'?", "id": "Stres" },
    { "en": "Kata Baku Dari 'Takwa'?", "id": "Taqwa" },
    { "en": "Kata Baku Dari 'Tatto'?", "id": "Tato" },
    { "en": "Kata Baku Dari 'Tauge'?", "id": "Toge" },
    { "en": "Kata Baku Dari 'TBC'?", "id": "TBC" },
    { "en": "Kata Baku Dari 'Terlentang'?", "id": "Telentang" },
    { "en": "Kata Baku Dari 'Tribun'?", "id": "Tribun" },
    { "en": "Kata Baku Dari 'Trophy'?", "id": "Trofi" },
    { "en": "Kata Baku Dari 'Tunarungu'?", "id": "Tunarungu" },
    { "en": "Kata Baku Dari 'Urin'?", "id": "Urine" },
    { "en": "Kata Baku Dari 'Vakum'?", "id": "Vakum" },
    { "en": "Kata Baku Dari 'Valuta'?", "id": "Valuta" },
    { "en": "Kata Baku Dari 'Video'?", "id": "Video" },
    { "en": "Kata Baku Dari 'Waria'?", "id": "Waria" },
    { "en": "Kata Baku Dari 'Wirausaha'?", "id": "Wirausaha" },
    { "en": "Kata Baku Dari 'Xerox'?", "id": "Salin" },
    { "en": "Kata Baku Dari 'Yudikatif'?", "id": "Yudikatif" },
    { "en": "Kata Baku Dari 'Zakar'?", "id": "Zakar" },
    { "en": "Kata Baku Dari 'Zig-zag'?", "id": "Zig-zag" },
    { "en": "Kata Baku Dari 'Akil Baligh'?", "id": "Akil Balig" },
    { "en": "Kata Baku Dari 'Almari'?", "id": "Lemari" },
    { "en": "Kata Baku Dari 'Bangker'?", "id": "Bungker" },
    { "en": "Kata Baku Dari 'Batalyon'?", "id": "Batalion" },
    { "en": "Kata Baku Dari 'Baud'?", "id": "Baut" },
    { "en": "Kata Baku Dari 'Bengep'?", "id": "Bengap" },
    { "en": "Kata Baku Dari 'Bengsin'?", "id": "Bensin" },
    { "en": "Kata Baku Dari 'Besuk'?", "id": "Jenguk" },
    { "en": "Kata Baku Dari 'Bhineka'?", "id": "Bhinneka" },
    { "en": "Kata Baku Dari 'Bilyar'?", "id": "Biliar" },
    { "en": "Kata Baku Dari 'Bonafid'?", "id": "Bonafide" },
    { "en": "Kata Baku Dari 'Capcay'?", "id": "Capcai" },
    { "en": "Kata Baku Dari 'Cengkeh'?", "id": "Cengkih" },
    { "en": "Kata Baku Dari 'Centimeter'?", "id": "Sentimeter" },
    { "en": "Kata Baku Dari 'Chef'?", "id": "Koki" },
    { "en": "Kata Baku Dari 'Cinderamata'?", "id": "Cenderamata" },
    { "en": "Kata Baku Dari 'Cockpit'?", "id": "Kokpit" },
    { "en": "Kata Baku Dari 'Da'wah'?", "id": "Dakwah" },
    { "en": "Kata Baku Dari 'Deodorant'?", "id": "Deodoran" },
    { "en": "Kata Baku Dari 'Deterjen'?", "id": "Detergen" },
    { "en": "Kata Baku Dari 'Dikir'?", "id": "Zikir" },
    { "en": "Kata Baku Dari 'Discount'?", "id": "Diskon" },
    { "en": "Kata Baku Dari 'Do'a'?", "id": "Doa" },
    { "en": "Kata Baku Dari 'Dram'?", "id": "Drum" },
    { "en": "Kata Baku Dari 'Esei'?", "id": "Esai" },
    { "en": "Kata Baku Dari 'Eskrim'?", "id": "Es Krim" },
    { "en": "Kata Baku Dari 'Etnik'?", "id": "Etnis" },
    { "en": "Kata Baku Dari 'Faqir'?", "id": "Fakir" },
    { "en": "Kata Baku Dari 'Ferma'?", "id": "Firma" },
    { "en": "Kata Baku Dari 'Flatform'?", "id": "Platform" },
    { "en": "Kata Baku Dari 'Ganjel'?", "id": "Ganjal" },
    { "en": "Kata Baku Dari 'Gledek'?", "id": "Geledek" },
    { "en": "Kata Baku Dari 'Gempor'?", "id": "Gempur" },
    { "en": "Kata Baku Dari 'Gerilia'?", "id": "Gerilya" },
    { "en": "Kata Baku Dari 'Gisi'?", "id": "Gizi" },
    { "en": "Kata Baku Dari 'Hallo'?", "id": "Halo" },
    { "en": "Kata Baku Dari 'Hepotensi'?", "id": "Hipotensi" },
    { "en": "Kata Baku Dari 'Hidentitas'?", "id": "Identitas" },
    { "en": "Kata Baku Dari 'Homogin'?", "id": "Homogen" },
    { "en": "Kata Baku Dari 'Idul Fitri'?", "id": "Idulfitri" },
    { "en": "Kata Baku Dari 'Ikhwal'?", "id": "Ihwal" },
    { "en": "Kata Baku Dari 'Index'?", "id": "Indeks" },
    { "en": "Kata Baku Dari 'Infra Merah'?", "id": "Inframerah" },
    { "en": "Kata Baku Dari 'Introgasi'?", "id": "Interogasi" },
    { "en": "Kata Baku Dari 'Jejer'?", "id": "Jajar" },
    { "en": "Kata Baku Dari 'Jalangkung'?", "id": "Jailangkung" },
    { "en": "Kata Baku Dari 'Jemaat'?", "id": "Jemaah" },
    { "en": "Kata Baku Dari 'Jenggot'?", "id": "Janggut" },
    { "en": "Kata Baku Dari 'Jerigen'?", "id": "Jeriken" },
    { "en": "Kata Baku Dari 'Jin'?", "id": "Jin" },
    { "en": "Kata Baku Dari 'Kaedah'?", "id": "Kaidah" },
    { "en": "Kata Baku Dari 'Karakterisir'?", "id": "Karakterisasi" },
    { "en": "Kata Baku Dari 'Kelakson'?", "id": "Klakson" },
    { "en": "Kata Baku Dari 'Kelelap'?", "id": "Lelap" },
    { "en": "Kata Baku Dari 'Kelenteng'?", "id": "Klenteng" },
    { "en": "Kata Baku Dari 'Kelep'?", "id": "Klep" },
    { "en": "Kata Baku Dari 'Kempes'?", "id": "Kempis" },
    { "en": "Kata Baku Dari 'Kesleo'?", "id": "Terkilir" },
    { "en": "Kata Baku Dari 'Ketchup'?", "id": "Kecap" },
    { "en": "Kata Baku Dari 'Ketimun'?", "id": "Mentimun" },
    { "en": "Kata Baku Dari 'Khutbah'?", "id": "Khotbah" },
    { "en": "Kata Baku Dari 'Client'?", "id": "Klien" },
    { "en": "Kata Baku Dari 'Komendan'?", "id": "Komandan" },
    { "en": "Kata Baku Dari 'Koment'?", "id": "Komentar" },
    { "en": "Kata Baku Dari 'Konggeres'?", "id": "Kongres" },
    { "en": "Kata Baku Dari 'Konpoy'?", "id": "Konvoi" },
    { "en": "Kata Baku Dari 'Korma'?", "id": "Kurma" },
    { "en": "Kata Baku Dari 'Kraton'?", "id": "Keraton" },
    { "en": "Kata Baku Dari 'Kuna'?", "id": "Kuno" },
    { "en": "Kata Baku Dari 'Kopon'?", "id": "Kupon" },
    { "en": "Kata Baku Dari 'Launcing'?", "id": "Peluncuran" },
    { "en": "Kata Baku Dari 'Limusin'?", "id": "Limosin" },
    { "en": "Kata Baku Dari 'Lungguh'?", "id": "Lungguh" },
    { "en": "Kata Baku Dari 'Ma'af'?", "id": "Maaf" },
    { "en": "Kata Baku Dari 'Majlis'?", "id": "Majelis" },
    { "en": "Kata Baku Dari 'Manager'?", "id": "Manajer" },
    { "en": "Kata Baku Dari 'Manejer'?", "id": "Manajer" },
    { "en": "Kata Baku Dari 'Marketing'?", "id": "Pemasaran" },
    { "en": "Kata Baku Dari 'Master'?", "id": "Mahaguru" },
    { "en": "Kata Baku Dari 'Mencontek'?", "id": "Menyontek" },
    { "en": "Kata Baku Dari 'Mensukseskan'?", "id": "Menyukseskan" },
    { "en": "Kata Baku Dari 'Menthol'?", "id": "Mentol" },
    { "en": "Kata Baku Dari 'Milyuner'?", "id": "Miliuner" },
    { "en": "Kata Baku Dari 'Mitos'?", "id": "Mitos" },
    { "en": "Kata Baku Dari 'Mubajir'?", "id": "Mubazir" },
    { "en": "Kata Baku Dari 'Nafas'?", "id": "Napas" },
    { "en": "Kata Baku Dari 'Nanas'?", "id": "Nenas" },
    { "en": "Kata Baku Dari 'Negosiasi'?", "id": "Negosiasi" },
    { "en": "Kata Baku Dari 'Nol'?", "id": "Nol" },
    { "en": "Kata Baku Dari 'Opname'?", "id": "Rawat Inap" },
    { "en": "Kata Baku Dari 'Organisir'?", "id": "Organisasi" },
    { "en": "Kata Baku Dari 'Outentik'?", "id": "Otentik" },
    { "en": "Kata Baku Dari 'Pacet'?", "id": "Pacat" },
    { "en": "Kata Baku Dari 'Pancaindera'?", "id": "Pancaindra" },
    { "en": "Kata Baku Dari 'Pararel'?", "id": "Paralel" },
    { "en": "Kata Baku Dari 'Peliara'?", "id": "Pelihara" },
    { "en": "Kata Baku Dari 'Pengrajin'?", "id": "Perajin" },
    { "en": "Kata Baku Dari 'Pralon'?", "id": "Paralon" },
    { "en": "Kata Baku Dari 'Pete'?", "id": "Petai" },
    { "en": "Kata Baku Dari 'Phobia'?", "id": "Fobia" },
    { "en": "Kata Baku Dari 'Plintir'?", "id": "Pelintir" },
    { "en": "Kata Baku Dari 'Progam'?", "id": "Program" },
    { "en": "Kata Baku Dari 'Bolpen'?", "id": "Pulpen" },
    { "en": "Kata Baku Dari 'Ramadhan'?", "id": "Ramadan" },
    { "en": "Kata Baku Dari 'Rels'?", "id": "Rel" },
    { "en": "Kata Baku Dari 'Retna'?", "id": "Retina" },
    { "en": "Kata Baku Dari 'Rokhani'?", "id": "Rohani" },
    { "en": "Kata Baku Dari 'Ronsen'?", "id": "Rontgen" },
    { "en": "Kata Baku Dari 'Route'?", "id": "Rute" },
    { "en": "Kata Baku Dari 'Saklar'?", "id": "Sakelar" },
    { "en": "Kata Baku Dari 'Sambel'?", "id": "Sambal" },
    { "en": "Kata Baku Dari 'Sembayang'?", "id": "Sembahyang" },
    { "en": "Kata Baku Dari 'Sanggama'?", "id": "Senggama" },
    { "en": "Kata Baku Dari 'Sentausa'?", "id": "Sentosa" },
    { "en": "Kata Baku Dari 'Seponsor'?", "id": "Sponsor" },
    { "en": "Kata Baku Dari 'Syetan'?", "id": "Setan" },
    { "en": "Kata Baku Dari 'Strika'?", "id": "Setrika" },
    { "en": "Kata Baku Dari 'Shalat'?", "id": "Salat" },
    { "en": "Kata Baku Dari 'Sirine'?", "id": "Sirene" },
    { "en": "Kata Baku Dari 'Sirup'?", "id": "Sirop" },
    { "en": "Kata Baku Dari 'Supir'?", "id": "Sopir" },
    { "en": "Kata Baku Dari 'Sepiritus'?", "id": "Spiritus" },
    { "en": "Kata Baku Dari 'Setasiun'?", "id": "Stasiun" },
    { "en": "Kata Baku Dari 'Sticker'?", "id": "Stiker" },
    { "en": "Kata Baku Dari 'Stock'?", "id": "Stok" },
    { "en": "Kata Baku Dari 'Setudio'?", "id": "Studio" },
    { "en": "Kata Baku Dari 'Syah'?", "id": "Sah" },
    { "en": "Kata Baku Dari 'Sahid'?", "id": "Syahid" },
    { "en": "Kata Baku Dari 'Sair'?", "id": "Syair" },
    { "en": "Kata Baku Dari 'Ta'jil'?", "id": "Takjil" },
    { "en": "Kata Baku Dari 'Tau'?", "id": "Tahu" },
    { "en": "Kata Baku Dari 'Taxi'?", "id": "Taksi" },
    { "en": "Kata Baku Dari 'Tanggungjawab'?", "id": "Tanggung Jawab" },
    { "en": "Kata Baku Dari 'Tape'?", "id": "Tapai" },
    { "en": "Kata Baku Dari 'Tarip'?", "id": "Tarif" },
    { "en": "Kata Baku Dari 'Tekat'?", "id": "Tekad" },
    { "en": "Kata Baku Dari 'Tauladan'?", "id": "Teladan" },
    { "en": "Kata Baku Dari 'Theori'?", "id": "Teori" },
    { "en": "Kata Baku Dari 'Trasi'?", "id": "Terasi" },
    { "en": "Kata Baku Dari 'Tralis'?", "id": "Terali" },
    { "en": "Kata Baku Dari 'Terong'?", "id": "Terung" },
    { "en": "Kata Baku Dari 'Terpedo'?", "id": "Torpedo" },
    { "en": "Kata Baku Dari 'Tips'?", "id": "Tip" },
    { "en": "Kata Baku Dari 'Taubat'?", "id": "Tobat" },
    { "en": "Kata Baku Dari 'Topaz'?", "id": "Topas" },
    { "en": "Kata Baku Dari 'Trenggiling'?", "id": "Tenggiling" },
    { "en": "Kata Baku Dari 'Trotoir'?", "id": "Trotoar" },
    { "en": "Kata Baku Dari 'Trek'?", "id": "Truk" },
    { "en": "Kata Baku Dari 'Tour'?", "id": "Tur" },
    { "en": "Kata Baku Dari 'Udel'?", "id": "Pusar" },
    { "en": "Kata Baku Dari 'Faksin'?", "id": "Vaksin" },
    { "en": "Kata Baku Dari 'Pecin'?", "id": "Penyedap Rasa" },
    { "en": "Kata Baku Dari 'Wujut'?", "id": "Wujud" },
    { "en": "Kata Baku Dari 'Yoghurt'?", "id": "Yogurt" },
    { "en": "Kata Baku Dari 'Zanah'?", "id": "Zina" },
    { "en": "Kata Baku Dari 'Jamrud'?", "id": "Zamrud" },
    { "en": "Kata Baku Dari 'Sebra'?", "id": "Zebra" },
    { "en": "Kata Baku Dari 'Sodiak'?", "id": "Zodiak" },
    { "en": "Kata Baku Dari 'Zone'?", "id": "Zona" },
    { "en": "Kata Baku Dari 'Pebioligi'?", "id": "Biologi" },
    { "en": "Kata Baku Dari 'Pikir'?", "id": "Pikir" },
    { "en": "Kata Baku Dari 'Pilot'?", "id": "Pilot" },
    { "en": "Kata Baku Dari 'Pinset'?", "id": "Pinset" },
    { "en": "Kata Baku Dari 'Pirsawan'?", "id": "Pemirsa" },
    { "en": "Kata Baku Dari 'Plastik'?", "id": "Plastik" },
    { "en": "Kata Baku Dari 'Polusi'?", "id": "Polusi" },
    { "en": "Kata Baku Dari 'Prasasti'?", "id": "Prasasti" },
    { "en": "Kata Baku Dari 'Premium'?", "id": "Premium" },
    { "en": "Kata Baku Dari 'Pria'?", "id": "Pria" },
    { "en": "Kata Baku Dari 'Profesional'?", "id": "Profesional" },
    { "en": "Kata Baku Dari 'Profesor'?", "id": "Profesor" },
    { "en": "Kata Baku Dari 'Protokol'?", "id": "Protokol" },
    { "en": "Kata Baku Dari 'Pulau'?", "id": "Pulau" },
    { "en": "Kata Baku Dari 'Bus'?", "id": "Bus" },
    { "en": "Kata Baku Dari 'Radio'?", "id": "Radio" },
    { "en": "Kata Baku Dari 'Raja'?", "id": "Raja" },
    { "en": "Kata Baku Dari 'Raket'?", "id": "Raket" },
    { "en": "Kata Baku Dari 'Ransel'?", "id": "Ransel" },
    { "en": "Kata Baku Dari 'Rantai'?", "id": "Rantai" },
    { "en": "Kata Baku Dari 'Rating'?", "id": "Peringkat" },
    { "en": "Kata Baku Dari 'Reaksi'?", "id": "Reaksi" },
    { "en": "Kata Baku Dari 'Rektor'?", "id": "Rektor" },
    { "en": "Kata Baku Dari 'Religi'?", "id": "Religi" },
    { "en": "Kata Baku Dari 'Rem'?", "id": "Rem" },
    { "en": "Kata Baku Dari 'Rente'?", "id": "Rente" },
    { "en": "Kata Baku Dari 'Reptil'?", "id": "Reptil" },
    { "en": "Kata Baku Dari 'Restoran'?", "id": "Restoran" },
    { "en": "Kata Baku Dari 'Revolusi'?", "id": "Revolusi" },
    { "en": "Kata Baku Dari 'Robot'?", "id": "Robot" },
    { "en": "Kata Baku Dari 'Rok'?", "id": "Rok" },
    { "en": "Kata Baku Dari 'Rokok'?", "id": "Rokok" },
    { "en": "Kata Baku Dari 'Roti'?", "id": "Roti" },
    { "en": "Kata Baku Dari 'Abjat'?", "id": "Abjad" },
    { "en": "Kata Baku Dari 'Adpokat'?", "id": "Advokat" },
    { "en": "Kata Baku Dari 'Agamais'?", "id": "Agamis" },
    { "en": "Kata Baku Dari 'Ajeg'?", "id": "Ajek" },
    { "en": "Kata Baku Dari 'Akte'?", "id": "Akta" },
    { "en": "Kata Baku Dari 'Aktuil'?", "id": "Aktual" },
    { "en": "Kata Baku Dari 'Amphibi'?", "id": "Amfibi" },
    { "en": "Kata Baku Dari 'Angpao'?", "id": "Angpau" },
    { "en": "Kata Baku Dari 'Anugrah'?", "id": "Anugerah" },
    { "en": "Kata Baku Dari 'Areobik'?", "id": "Aerobik" },
    { "en": "Kata Baku Dari 'Aritmatika'?", "id": "Aritmetika" },
    { "en": "Kata Baku Dari 'Astronot'?", "id": "Astronaut" },
    { "en": "Kata Baku Dari 'Asik'?", "id": "Asyik" },
    { "en": "Kata Baku Dari 'Audio Visual'?", "id": "Audiovisual" },
    { "en": "Kata Baku Dari 'Bale'?", "id": "Balai" },
    { "en": "Kata Baku Dari 'Baligo'?", "id": "Baliho" },
    { "en": "Kata Baku Dari 'Bandrol'?", "id": "Banderol" },
    { "en": "Kata Baku Dari 'Barzah'?", "id": "Barzakh" },
    { "en": "Kata Baku Dari 'Bass'?", "id": "Bas" },
    { "en": "Kata Baku Dari 'Bathin'?", "id": "Batin" },
    { "en": "Kata Baku Dari 'Bego'?", "id": "Bodoh" },
    { "en": "Kata Baku Dari 'Berengsek'?", "id": "Brengsek" },
    { "en": "Kata Baku Dari 'Bertanggungjawab'?", "id": "Bertanggung Jawab" },
    { "en": "Kata Baku Dari 'Beterbangan'?", "id": "Berterbangan" },
    { "en": "Kata Baku Dari 'Bhakti'?", "id": "Bakti" },
    { "en": "Kata Baku Dari 'Bilyun'?", "id": "Biliun" },
    { "en": "Kata Baku Dari 'Bongkok'?", "id": "Bungkuk" },
    { "en": "Kata Baku Dari 'Berangkas'?", "id": "Brankas" },
    { "en": "Kata Baku Dari 'Bumiputera'?", "id": "Bumiputra" },
    { "en": "Kata Baku Dari 'Buntet'?", "id": "Buntu" },
    { "en": "Kata Baku Dari 'Cantel'?", "id": "Cantol" },
    { "en": "Kata Baku Dari 'Cathering'?", "id": "Katering" },
    { "en": "Kata Baku Dari 'Celurit'?", "id": "Clurit" },
    { "en": "Kata Baku Dari 'Cericap'?", "id": "Cerecap" },
    { "en": "Kata Baku Dari 'Cermelang'?", "id": "Cemerlang" },
    { "en": "Kata Baku Dari 'Cina'?", "id": "Tionghoa" },
    { "en": "Kata Baku Dari 'Combro'?", "id": "Comro" },
    { "en": "Kata Baku Dari 'Dajjal'?", "id": "Dajal" },
    { "en": "Kata Baku Dari 'Debitor'?", "id": "Debitur" },
    { "en": "Kata Baku Dari 'Degresi'?", "id": "Digresi" },
    { "en": "Kata Baku Dari 'Depot'?", "id": "Depo" },
    { "en": "Kata Baku Dari 'Drajat'?", "id": "Derajat" },
    { "en": "Kata Baku Dari 'Diskripsi'?", "id": "Deskripsi" },
    { "en": "Kata Baku Dari 'Diskotik'?", "id": "Diskotek" },
    { "en": "Kata Baku Dari 'Destilasi'?", "id": "Distilasi" },
    { "en": "Kata Baku Dari 'Ebook'?", "id": "Buku Elektronik" },
    { "en": "Kata Baku Dari 'Ecer'?", "id": "Eceran" },
    { "en": "Kata Baku Dari 'Eksem'?", "id": "Eksim" },
    { "en": "Kata Baku Dari 'Empek-empek'?", "id": "Pempek" },
    { "en": "Kata Baku Dari 'Episod'?", "id": "Episode" },
    { "en": "Kata Baku Dari 'Family'?", "id": "Famili" },
    { "en": "Kata Baku Dari 'Paporit'?", "id": "Favorit" },
    { "en": "Kata Baku Dari 'Feminim'?", "id": "Feminin" },
    { "en": "Kata Baku Dari 'Finishing'?", "id": "Penyelesaian" },
    { "en": "Kata Baku Dari 'Fitnes'?", "id": "Kebugaran" },
    { "en": "Kata Baku Dari 'Forniture'?", "id": "Furnitur" },
    { "en": "Kata Baku Dari 'Foto Kopi'?", "id": "Fotokopi" },
    { "en": "Kata Baku Dari 'Frustasi'?", "id": "Frustrasi" },
    { "en": "Kata Baku Dari 'Gajih'?", "id": "Gaji" },
    { "en": "Kata Baku Dari 'Gelatak'?", "id": "Geletak" },
    { "en": "Kata Baku Dari 'Gembos'?", "id": "Kempis" },
    { "en": "Kata Baku Dari 'Gencat'?", "id": "Gencet" },
    { "en": "Kata Baku Dari 'Geraham'?", "id": "Graham" },
    { "en": "Kata Baku Dari 'Gemetar'?", "id": "Gemetar" },
    { "en": "Kata Baku Dari 'Geter'?", "id": "Getar" },
    { "en": "Kata Baku Dari 'Ghoib'?", "id": "Gaib" },
    { "en": "Kata Baku Dari 'Glonggong'?", "id": "Gelonggong" },
    { "en": "Kata Baku Dari 'Gono Gini'?", "id": "Gana Gini" },
    { "en": "Kata Baku Dari 'Grebek'?", "id": "Gerebek" },
    { "en": "Kata Baku Dari 'Gua'?", "id": "Gua" },
    { "en": "Kata Baku Dari 'Gudek'?", "id": "Gudeg" },
    { "en": "Kata Baku Dari 'Guncang'?", "id": "Goncang" },
    { "en": "Kata Baku Dari 'Hadroh'?", "id": "Hadrah" },
    { "en": "Kata Baku Dari 'Hempas'?", "id": "Empas" },
    { "en": "Kata Baku Dari 'Hentak'?", "id": "Entak" },
    { "en": "Kata Baku Dari 'Hepotesis'?", "id": "Hipotesis" },
    { "en": "Kata Baku Dari 'Hiteris'?", "id": "Histeris" },
    { "en": "Kata Baku Dari 'Hoby'?", "id": "Hobi" },
    { "en": "Kata Baku Dari 'Holistik'?", "id": "Holistis" },
    { "en": "Kata Baku Dari 'Horison'?", "id": "Horizon" },
    { "en": "Kata Baku Dari 'Humor'?", "id": "Humor" },
    { "en": "Kata Baku Dari 'Ideot'?", "id": "Idiot" },
    { "en": "Kata Baku Dari 'Imaginasi'?", "id": "Imajinasi" },
    { "en": "Kata Baku Dari 'Incognito'?", "id": "Inkognito" },
    { "en": "Kata Baku Dari 'Indikator'?", "id": "Indikator" },
    { "en": "Kata Baku Dari 'Inovasi'?", "id": "Inovasi" },
    { "en": "Kata Baku Dari 'Instalasi'?", "id": "Instalasi" },
    { "en": "Kata Baku Dari 'Insyaf'?", "id": "Insaf" },
    { "en": "Kata Baku Dari 'Intelejen'?", "id": "Intelijen" },
    { "en": "Kata Baku Dari 'Intens'?", "id": "Intens" },
    { "en": "Kata Baku Dari 'Intermezo'?", "id": "Intermeso" },
    { "en": "Kata Baku Dari 'Inventarisir'?", "id": "Inventarisasi" },
    { "en": "Kata Baku Dari 'Isi'?", "id": "Isi" },
    { "en": "Kata Baku Dari 'Ijin'?", "id": "Izin" },
    { "en": "Kata Baku Dari 'Jelly'?", "id": "Jeli" },
    { "en": "Kata Baku Dari 'Jogging'?", "id": "Joging" },
    { "en": "Kata Baku Dari 'Jor-joran'?", "id": "Jorjoran" },
    { "en": "Kata Baku Dari 'Jubin'?", "id": "Ubin" },
    { "en": "Kata Baku Dari 'Jupiter'?", "id": "Yupiter" },
    { "en": "Kata Baku Dari 'Kangker'?", "id": "Kanker" },
    { "en": "Kata Baku Dari 'Kansel'?", "id": "Batal" },
    { "en": "Kata Baku Dari 'Kapiten'?", "id": "Kapten" },
    { "en": "Kata Baku Dari 'Karna'?", "id": "Karena" },
    { "en": "Kata Baku Dari 'Kariawan'?", "id": "Karyawan" },
    { "en": "Kata Baku Dari 'Katel'?", "id": "Wajan" },
    { "en": "Kata Baku Dari 'Kates'?", "id": "Pepaya" },
    { "en": "Kata Baku Dari 'Katoda'?", "id": "Katode" },
    { "en": "Kata Baku Dari 'Kayangan'?", "id": "Kehayangan" },
    { "en": "Kata Baku Dari 'Kecoa'?", "id": "Kecoak" },
    { "en": "Kata Baku Dari 'Kelar'?", "id": "Selesai" },
    { "en": "Kata Baku Dari 'Kemben'?", "id": "Kemban" },
    { "en": "Kata Baku Dari 'Kemping'?", "id": "Berkemah" },
    { "en": "Kata Baku Dari 'Kenapa'?", "id": "Mengapa" },
    { "en": "Kata Baku Dari 'Kran'?", "id": "Keran" },
    { "en": "Kata Baku Dari 'Kresek'?", "id": "Kresek" },
    { "en": "Kata Baku Dari 'Kripik'?", "id": "Keripik" },
    { "en": "Kata Baku Dari 'Kerjo'?", "id": "Kerja" },
    { "en": "Kata Baku Dari 'Kesting'?", "id": "Pemilihan Peran" },
    { "en": "Kata Baku Dari 'Khatam'?", "id": "Khatam" },
    { "en": "Kata Baku Dari 'Khitan'?", "id": "Khitan" },
    { "en": "Kata Baku Dari 'Khotib'?", "id": "Khatib" },
    { "en": "Kata Baku Dari 'Kilo Meter'?", "id": "Kilometer" },
    { "en": "Kata Baku Dari 'Kiosk'?", "id": "Kios" },
    { "en": "Kata Baku Dari 'Klasmen'?", "id": "Klasemen" },
    { "en": "Kata Baku Dari 'Klausul'?", "id": "Klausula" },
    { "en": "Kata Baku Dari 'Kloropil'?", "id": "Klorofil" },
    { "en": "Kata Baku Dari 'Code'?", "id": "Kode" },
    { "en": "Kata Baku Dari 'Kolonel'?", "id": "Kolonel" },
    { "en": "Kata Baku Dari 'Komidi'?", "id": "Komedi" },
    { "en": "Kata Baku Dari 'Komfirmasi'?", "id": "Konfirmasi" },
    { "en": "Kata Baku Dari 'Kongkow'?", "id": "Kongko" },
    { "en": "Kata Baku Dari 'Konjuktur'?", "id": "Konjungtur" },
    { "en": "Kata Baku Dari 'Konpeksi'?", "id": "Konveksi" },
    { "en": "Kata Baku Dari 'Kordinir'?", "id": "Koordinasi" },
    { "en": "Kata Baku Dari 'Kos'?", "id": "Indekos" },
    { "en": "Kata Baku Dari 'Cosinus'?", "id": "Kosinus" },
    { "en": "Kata Baku Dari 'Kover'?", "id": "Sampul" },
    { "en": "Kata Baku Dari 'Krem'?", "id": "Krim" },
    { "en": "Kata Baku Dari 'Kriminil'?", "id": "Kriminal" },
    { "en": "Kata Baku Dari 'Kroscek'?", "id": "Cek Silang" },
    { "en": "Kata Baku Dari 'Krutin'?", "id": "Keratin" },
    { "en": "Kata Baku Dari 'Kudeta'?", "id": "Kudeta" },
    { "en": "Kata Baku Dari 'Kudus'?", "id": "Kudus" },
    { "en": "Kata Baku Dari 'Kuliner'?", "id": "Kuliner" },
    { "en": "Kata Baku Dari 'Kulonuwun'?", "id": "Permisi" },
    { "en": "Kata Baku Dari 'Kumandang'?", "id": "Kumandang" },
    { "en": "Kata Baku Dari 'Kumat'?", "id": "Kambuh" },
    { "en": "Kata Baku Dari 'Kumis'?", "id": "Kumis" },
    { "en": "Kata Baku Dari 'Kumpul'?", "id": "Kumpul" },
    { "en": "Kata Baku Dari 'Kuno'?", "id": "Kuno" },
    { "en": "Kata Baku Dari 'Kurban'?", "id": "Kurban" },
    { "en": "Kata Baku Dari 'Kurir'?", "id": "Kurir" },
    { "en": "Kata Baku Dari 'Kursi'?", "id": "Kursi" },
    { "en": "Kata Baku Dari 'Kursor'?", "id": "Kursor" },
    { "en": "Kata Baku Dari 'Kusta'?", "id": "Kusta" },
    { "en": "Kata Baku Dari 'Kusut'?", "id": "Kusut" },
    { "en": "Kata Baku Dari 'Kutang'?", "id": "Kutang" },
    { "en": "Kata Baku Dari 'Kutikula'?", "id": "Kutikula" },
    { "en": "Kata Baku Dari 'Kutub'?", "id": "Kutub" },
    { "en": "Kata Baku Dari 'Kuwuk'?", "id": "Kucing Hutan" },
    { "en": "Kata Baku Dari 'Laik'?", "id": "Layak" },
    { "en": "Kata Baku Dari 'Laminating'?", "id": "Laminasi" },
    { "en": "Kata Baku Dari 'Lanun'?", "id": "Perompak" },
    { "en": "Kata Baku Dari 'Lapal'?", "id": "Lafal" },
    { "en": "Kata Baku Dari 'Lasim'?", "id": "Lazim" },
    { "en": "Kata Baku Dari 'Latet'?", "id": "Letoi" },
    { "en": "Kata Baku Dari 'Latto-latto'?", "id": "Latto Latto" },
    { "en": "Kata Baku Dari 'Leptop'?", "id": "Laptop" },
    { "en": "Kata Baku Dari 'Leukimia'?", "id": "Leukemia" },
    { "en": "Kata Baku Dari 'Linier'?", "id": "Linear" },
    { "en": "Kata Baku Dari 'Liter'?", "id": "Liter" },
    { "en": "Kata Baku Dari 'Log-in'?", "id": "Masuk Log" },
    { "en": "Kata Baku Dari 'Lotion'?", "id": "Losion" },
    { "en": "Kata Baku Dari 'Lowbat'?", "id": "Baterai Lemah" },
    { "en": "Kata Baku Dari 'Loyang'?", "id": "Loyang" },
    { "en": "Kata Baku Dari 'Lumbung'?", "id": "Lumbung" },
    { "en": "Kata Baku Dari 'Ma'ag'?", "id": "Mag" },
    { "en": "Kata Baku Dari 'Madzab'?", "id": "Mazhab" },
    { "en": "Kata Baku Dari 'Maestro'?", "id": "Maestro" },
    { "en": "Kata Baku Dari 'Mafhum'?", "id": "Mafhum" },
    { "en": "Kata Baku Dari 'Maha Kuasa'?", "id": "Mahakuasa" },
    { "en": "Kata Baku Dari 'Mahardika'?", "id": "Merdeka" },
    { "en": "Kata Baku Dari 'Mahlul'?", "id": "Mahlul" },
    { "en": "Kata Baku Dari 'Main'?", "id": "Main" },
    { "en": "Kata Baku Dari 'Maker'?", "id": "Makar" },
    { "en": "Kata Baku Dari 'Makin'?", "id": "Makin" },
    { "en": "Kata Baku Dari 'Makelar'?", "id": "Makelar" },
    { "en": "Kata Baku Dari 'Maksimal'?", "id": "Maksimal" },
    { "en": "Kata Baku Dari 'Malaikat'?", "id": "Malaikat" },
    { "en": "Kata Baku Dari 'Mampet'?", "id": "Mampat" },
    { "en": "Kata Baku Dari 'Manpaat'?", "id": "Manfaat" },
    { "en": "Kata Baku Dari 'Marathon'?", "id": "Maraton" },
    { "en": "Kata Baku Dari 'Marmut'?", "id": "Marmot" },
    { "en": "Kata Baku Dari 'Martel'?", "id": "Martil" },
    { "en": "Kata Baku Dari 'Mas Kahwin'?", "id": "Maskawin" },
    { "en": "Kata Baku Dari 'Mata Hari'?", "id": "Matahari" },
    { "en": "Kata Baku Dari 'Materai'?", "id": "Meterai" },
    { "en": "Kata Baku Dari 'Matimatika'?", "id": "Matematika" },
    { "en": "Kata Baku Dari 'Membran'?", "id": "Membran" },
    { "en": "Kata Baku Dari 'Menstruasi'?", "id": "Menstruasi" },
    { "en": "Kata Baku Dari 'Mentor'?", "id": "Mentor" },
    { "en": "Kata Baku Dari 'Menu'?", "id": "Menu" },
    { "en": "Kata Baku Dari 'Nyalip'?", "id": "Menyalip" },
    { "en": "Kata Baku Dari 'Merkuri'?", "id": "Merkuri" },
    { "en": "Kata Baku Dari 'Mes'?", "id": "Mes" },
    { "en": "Kata Baku Dari 'Mesin'?", "id": "Mesin" },
    { "en": "Kata Baku Dari 'Mestika'?", "id": "Mustika" },
    { "en": "Kata Baku Dari 'Mie'?", "id": "Mi" },
    { "en": "Kata Baku Dari 'Mikropon'?", "id": "Mikrofon" },
    { "en": "Kata Baku Dari 'Minalaidin Walfaizin'?", "id": "Minalaidin Walfaizin" },
    { "en": "Kata Baku Dari 'Mebeler'?", "id": "Mebeler" },
    { "en": "Kata Baku Dari 'Mozaik'?", "id": "Mosaik" },
    { "en": "Kata Baku Dari 'Muadzin'?", "id": "Muazin" },
    { "en": "Kata Baku Dari 'Mujizat'?", "id": "Mukjizat" },
    { "en": "Kata Baku Dari 'Mulya'?", "id": "Mulia" },
    { "en": "Kata Baku Dari 'Mummy'?", "id": "Mumi" },
    { "en": "Kata Baku Dari 'Mor'?", "id": "Mur" },
    { "en": "Kata Baku Dari 'Music'?", "id": "Musik" },
    { "en": "Kata Baku Dari 'Nahkoda'?", "id": "Nakhoda" },
    { "en": "Kata Baku Dari 'Narkotik'?", "id": "Narkotika" },
    { "en": "Kata Baku Dari 'Nadar'?", "id": "Nazar" },
    { "en": "Kata Baku Dari 'Negri'?", "id": "Negeri" },
    { "en": "Kata Baku Dari 'Nekad'?", "id": "Nekat" },
    { "en": "Kata Baku Dari 'Non Aktif'?", "id": "Nonaktif" },
    { "en": "Kata Baku Dari 'Notulen'?", "id": "Notula" },
    { "en": "Kata Baku Dari 'Nusaindah'?", "id": "Nusa Indah" },
    { "en": "Kata Baku Dari 'Offset'?", "id": "Ofset" },
    { "en": "Kata Baku Dari 'Oksigin'?", "id": "Oksigen" },
    { "en": "Kata Baku Dari 'Oktav'?", "id": "Oktaf" },
    { "en": "Kata Baku Dari 'Olah Raga'?", "id": "Olahraga" },
    { "en": "Kata Baku Dari 'Olie'?", "id": "Oli" },
    { "en": "Kata Baku Dari 'Oniks'?", "id": "Oniks" },
    { "en": "Kata Baku Dari 'Orange'?", "id": "Oranye" },
    { "en": "Kata Baku Dari 'Orgen'?", "id": "Orgel" },
    { "en": "Kata Baku Dari 'Otodidak'?", "id": "Autodidak" },
    { "en": "Kata Baku Dari 'Padri'?", "id": "Paderi" },
    { "en": "Kata Baku Dari 'Palm'?", "id": "Palem" },
    { "en": "Kata Baku Dari 'Pantomime'?", "id": "Pantomim" },
    { "en": "Kata Baku Dari 'Paradighma'?", "id": "Paradigma" },
    { "en": "Kata Baku Dari 'Paramedik'?", "id": "Paramedis" },
    { "en": "Kata Baku Dari 'Parpum'?", "id": "Parfum" },
    { "en": "Kata Baku Dari 'Parkeer'?", "id": "Parkir" },
    { "en": "Kata Baku Dari 'Pas Foto'?", "id": "Pasfoto" },
    { "en": "Kata Baku Dari 'Pateri'?", "id": "Patri" },
    { "en": "Kata Baku Dari 'Pedes'?", "id": "Pedas" },
    { "en": "Kata Baku Dari 'Pebulu Tangkis'?", "id": "Pebulutangkis" },
    { "en": "Kata Baku Dari 'Peliara'?", "id": "Pelihara" },
    { "en": "Kata Baku Dari 'Pensil'?", "id": "Pensil" },
    { "en": "Kata Baku Dari 'Pentil'?", "id": "Pentil" },
    { "en": "Kata Baku Dari 'Per'?", "id": "Pegas" },
    { "en": "Kata Baku Dari 'Perek'?", "id": "Perempuan Nakal" },
    { "en": "Kata Baku Dari 'Perkosa'?", "id": "Perkosa" },
    { "en": "Kata Baku Dari 'Permisif'?", "id": "Permisif" },
    { "en": "Kata Baku Dari 'Persada'?", "id": "Persada" },
    { "en": "Kata Baku Dari 'Perseneling'?", "id": "Persneling" },
    { "en": "Kata Baku Dari 'Perseverasi'?", "id": "Perseverasi" },
    { "en": "Kata Baku Dari 'Pesing'?", "id": "Pesing" },
    { "en": "Kata Baku Dari 'Petroleum'?", "id": "Petroleum" },
    { "en": "Kata Baku Dari 'Pianis'?", "id": "Pianis" },
    { "en": "Kata Baku Dari 'Piknik'?", "id": "Piknik" },
    { "en": "Kata Baku Dari 'Pil'?", "id": "Pil" },
    { "en": "Kata Baku Dari 'Pincang'?", "id": "Pincang" },
    { "en": "Kata Baku Dari 'Pingpong'?", "id": "Pingpong" },
    { "en": "Kata Baku Dari 'Pion'?", "id": "Pion" },
    { "en": "Kata Baku Dari 'Pipa'?", "id": "Pipa" },
    { "en": "Kata Baku Dari 'Pipet'?", "id": "Pipet" },
    { "en": "Kata Baku Dari 'Piramida'?", "id": "Piramida" },
    { "en": "Kata Baku Dari 'Piranha'?", "id": "Piranha" },
    { "en": "Kata Baku Dari 'Pispot'?", "id": "Pispot" },
    { "en": "Kata Baku Dari 'Pistol'?", "id": "Pistol" },
    { "en": "Kata Baku Dari 'Pita'?", "id": "Pita" },
    { "en": "Kata Baku Dari 'Piyama'?", "id": "Piyama" },
    { "en": "Kata Baku Dari 'Plagiat'?", "id": "Plagiat" },
    { "en": "Kata Baku Dari 'Planet'?", "id": "Planet" },
    { "en": "Kata Baku Dari 'Platina'?", "id": "Platina" },
    { "en": "Kata Baku Dari 'Plaza'?", "id": "Plaza" },
    { "en": "Kata Baku Dari 'Plester'?", "id": "Plester" },
    { "en": "Kata Baku Dari 'Plin-plan'?", "id": "Plinplan" },
    { "en": "Kata Baku Dari 'Plural'?", "id": "Plural" },
    { "en": "Kata Baku Dari 'Plus'?", "id": "Plus" },
    { "en": "Kata Baku Dari 'Pluto'?", "id": "Pluto" },
    { "en": "Kata Baku Dari 'Politik'?", "id": "Politik" },
    { "en": "Kata Baku Dari 'Polio'?", "id": "Polio" },
    { "en": "Kata Baku Dari 'Polisi'?", "id": "Polisi" },
    { "en": "Kata Baku Dari 'Posessif'?", "id": "Posesif" },
    { "en": "Kata Baku Dari 'Pranoto'?", "id": "Pranata" },
    { "en": "Kata Baku Dari 'Pra Sangka'?", "id": "Prasangka" },
    { "en": "Kata Baku Dari 'Pra Sejarah'?", "id": "Prasejarah" },
    { "en": "Kata Baku Dari 'Premature'?", "id": "Prematur" },
    { "en": "Kata Baku Dari 'Proklamir'?", "id": "Proklamasi" },
    { "en": "Kata Baku Dari 'Puzel'?", "id": "Puzzle" },
    { "en": "Kata Baku Dari 'Qoriah'?", "id": "Qariah" },
    { "en": "Kata Baku Dari 'Rahasia'?", "id": "Rahasia" },
    { "en": "Kata Baku Dari 'Rajia'?", "id": "Razia" },
    { "en": "Kata Baku Dari 'Raka'at'?", "id": "Rakaat" },
    { "en": "Kata Baku Dari 'Rame'?", "id": "Ramai" },
    { "en": "Kata Baku Dari 'Rangking'?", "id": "Peringkat" },
    { "en": "Kata Baku Dari 'Ransom'?", "id": "Ransum" },
    { "en": "Kata Baku Dari 'Rasio'?", "id": "Rasio" },
    { "en": "Kata Baku Dari 'Realisir'?", "id": "Realisasi" },
    { "en": "Kata Baku Dari 'Rebo'?", "id": "Rabu" },
    { "en": "Kata Baku Dari 'Reggae'?", "id": "Rege" },
    { "en": "Kata Baku Dari 'Remunirasi'?", "id": "Remunerasi" },
    { "en": "Kata Baku Dari 'Renaissance'?", "id": "Renaisans" },
    { "en": "Kata Baku Dari 'Resepsionist'?", "id": "Resepsionis" },
    { "en": "Kata Baku Dari 'Resume'?", "id": "Ringkasan" },
    { "en": "Kata Baku Dari 'Retail'?", "id": "Ritel" },
    { "en": "Kata Baku Dari 'Riba'?", "id": "Riba" },
    { "en": "Kata Baku Dari 'Rido'?", "id": "Rida" },
    { "en": "Kata Baku Dari 'Ritma'?", "id": "Ritme" },
    { "en": "Kata Baku Dari 'Roboh'?", "id": "Rubuh" },
    { "en": "Kata Baku Dari 'Romantik'?", "id": "Romantis" },
    { "en": "Kata Baku Dari 'Rosin'?", "id": "Resin" },
    { "en": "Kata Baku Dari 'Savana'?", "id": "Sabana" },
    { "en": "Kata Baku Dari 'Sadistis'?", "id": "Sadistis" },
    { "en": "Kata Baku Dari 'Samber'?", "id": "Sambar" },
    { "en": "Kata Baku Dari 'Sampagne'?", "id": "Sampanye" },
    { "en": "Kata Baku Dari 'Sample'?", "id": "Sampel" },
    { "en": "Kata Baku Dari 'Samudera'?", "id": "Samudra" },
    { "en": "Kata Baku Dari 'Sansekerta'?", "id": "Sanskerta" },
    { "en": "Kata Baku Dari 'Satellite'?", "id": "Satelit" },
    { "en": "Kata Baku Dari 'Seblah'?", "id": "Sebelah" },
    { "en": "Kata Baku Dari 'Seismograph'?", "id": "Seismograf" },
    { "en": "Kata Baku Dari 'Sejadah'?", "id": "Sajadah" },
    { "en": "Kata Baku Dari 'Sekering'?", "id": "Sekring" },
    { "en": "Kata Baku Dari 'Skop'?", "id": "Sekop" },
    { "en": "Kata Baku Dari 'Sexy'?", "id": "Seksi" },
    { "en": "Kata Baku Dari 'Sexual'?", "id": "Seksual" },
    { "en": "Kata Baku Dari 'Eskadron'?", "id": "Skuadron" },
    { "en": "Kata Baku Dari 'Slop'?", "id": "Selop" },
    { "en": "Kata Baku Dari 'Selulose'?", "id": "Selulosa" },
    { "en": "Kata Baku Dari 'Sembrana'?", "id": "Sembrono" },
    { "en": "Kata Baku Dari 'Sentimentil'?", "id": "Sentimental" },
    { "en": "Kata Baku Dari 'Central'?", "id": "Sentral" },
    { "en": "Kata Baku Dari 'Sepakbola'?", "id": "Sepak Bola" },
    { "en": "Kata Baku Dari 'Sepiker'?", "id": "Speaker" },
    { "en": "Kata Baku Dari 'Sepit'?", "id": "Jepit" },
    { "en": "Kata Baku Dari 'Serep'?", "id": "Cadangan" },
    { "en": "Kata Baku Dari 'Sertipikat'?", "id": "Sertifikat" },
    { "en": "Kata Baku Dari 'Servis'?", "id": "Servis" },
    { "en": "Kata Baku Dari 'Setor'?", "id": "Setor" },
    { "en": "Kata Baku Dari 'Sifon'?", "id": "Sifon" },
    { "en": "Kata Baku Dari 'Sigma'?", "id": "Sigma" },
    { "en": "Kata Baku Dari 'Sih'?", "id": "Sih" },
    { "en": "Kata Baku Dari 'Sikil'?", "id": "Kaki" },
    { "en": "Kata Baku Dari 'Siklon'?", "id": "Siklon" },
    { "en": "Kata Baku Dari 'Sila'?", "id": "Sila" },
    { "en": "Kata Baku Dari 'Simbol'?", "id": "Simbol" },
    { "en": "Kata Baku Dari 'Simetri'?", "id": "Simetri" },
    { "en": "Kata Baku Dari 'Simpati'?", "id": "Simpati" },
    { "en": "Kata Baku Dari 'Simple'?", "id": "Simpel" },
    { "en": "Kata Baku Dari 'Simulasi'?", "id": "Simulasi" },
    { "en": "Kata Baku Dari 'Sinagog'?", "id": "Sinagoge" },
    { "en": "Kata Baku Dari 'Sindikat'?", "id": "Sindikat" },
    { "en": "Kata Baku Dari 'Sinonim'?", "id": "Sinonim" },
    { "en": "Kata Baku Dari 'Sinopsis'?", "id": "Sinopsis" },
    { "en": "Kata Baku Dari 'Sirep'?", "id": "Sirep" },
    { "en": "Kata Baku Dari 'Sistematis'?", "id": "Sistematis" },
    { "en": "Kata Baku Dari 'Sitir'?", "id": "Sitir" },
    { "en": "Kata Baku Dari 'Siusia'?", "id": "Sia-sia" },
    { "en": "Kata Baku Dari 'Skala'?", "id": "Skala" },
    { "en": "Kata Baku Dari 'Skema'?", "id": "Skema" },
    { "en": "Kata Baku Dari 'Sketsa'?", "id": "Sketsa" },
    { "en": "Kata Baku Dari 'Ski'?", "id": "Ski" },
    { "en": "Kata Baku Dari 'Slogan'?", "id": "Slogan" },
    { "en": "Kata Baku Dari 'Smes'?", "id": "Smash" },
    { "en": "Kata Baku Dari 'Sok'?", "id": "Sok" },
    { "en": "Kata Baku Dari 'Sol'?", "id": "Sol" },
    { "en": "Kata Baku Dari 'Solder'?", "id": "Solder" },
    { "en": "Kata Baku Dari 'Solid'?", "id": "Solid" },
    { "en": "Kata Baku Dari 'Solo'?", "id": "Solo" },
    { "en": "Kata Baku Dari 'Somay'?", "id": "Siomai" },
    { "en": "Kata Baku Dari 'Sonar'?", "id": "Sonar" },
    { "en": "Kata Baku Dari 'Sopan'?", "id": "Sopan" },
    { "en": "Kata Baku Dari 'Soto'?", "id": "Soto" },
    { "en": "Kata Baku Dari 'Spanduk'?", "id": "Spanduk" },
    { "en": "Kata Baku Dari 'Spasi'?", "id": "Spasi" },
    { "en": "Kata Baku Dari 'Spesial'?", "id": "Spesial" },
    { "en": "Kata Baku Dari 'Spesipik'?", "id": "Spesifik" },
    { "en": "Kata Baku Dari 'Sepion'?", "id": "Spion" },
    { "en": "Kata Baku Dari 'Spon'?", "id": "Spons" },
    { "en": "Kata Baku Dari 'Sportip'?", "id": "Sportif" },
    { "en": "Kata Baku Dari 'Staff'?", "id": "Staf" },
    { "en": "Kata Baku Dari 'Setempel'?", "id": "Stempel" },
    { "en": "Kata Baku Dari 'Setruk'?", "id": "Stroke" },
    { "en": "Kata Baku Dari 'Submarine'?", "id": "Kapal Selam" },
    { "en": "Kata Baku Dari 'Subtansi'?", "id": "Substansi" },
    { "en": "Kata Baku Dari 'Subtitusi'?", "id": "Substitusi" },
    { "en": "Kata Baku Dari 'Suka Cita'?", "id": "Sukacita" },
    { "en": "Kata Baku Dari 'Suka Rela'?", "id": "Sukarela" },
    { "en": "Kata Baku Dari 'Suku Cadang'?", "id": "Sukucadang" },
    { "en": "Kata Baku Dari 'Sumatera'?", "id": "Sumatra" },
    { "en": "Kata Baku Dari 'Sunnah'?", "id": "Sunah" },
    { "en": "Kata Baku Dari 'Suplay'?", "id": "Suplai" },
    { "en": "Kata Baku Dari 'Surprise'?", "id": "Kejutan" },
    { "en": "Kata Baku Dari 'Switer'?", "id": "Sweter" },
    { "en": "Kata Baku Dari 'Sahbandar'?", "id": "Syahbandar" },
    { "en": "Kata Baku Dari 'Syak Wasangka'?", "id": "Syakwasangka" },
    { "en": "Kata Baku Dari 'Sarat'?", "id": "Syarat" },
    { "en": "Kata Baku Dari 'Seh'?", "id": "Syekh" },
    { "en": "Kata Baku Dari 'Sukur'?", "id": "Syukur" },
    { "en": "Kata Baku Dari 'Tabligh'?", "id": "Tablig" },
    { "en": "Kata Baku Dari 'Tapsir'?", "id": "Tafsir" },
    { "en": "Kata Baku Dari 'Tag'?", "id": "Tagar" },
    { "en": "Kata Baku Dari 'Tahilalat'?", "id": "Tahi Lalat" },
    { "en": "Kata Baku Dari 'Tahyul'?", "id": "Takhayul" },
    { "en": "Kata Baku Dari 'Teng'?", "id": "Tank" },
    { "en": "Kata Baku Dari 'Tausiyah'?", "id": "Tausiah" },
    { "en": "Kata Baku Dari 'Theater'?", "id": "Teater" },
    { "en": "Kata Baku Dari 'Tegel'?", "id": "Ubin" },
    { "en": "Kata Baku Dari 'Terlantar'?", "id": "Telantar" },
    { "en": "Kata Baku Dari 'Telepromter'?", "id": "Teleprompter" },
    { "en": "Kata Baku Dari 'Tembako'?", "id": "Tembakau" },
    { "en": "Kata Baku Dari 'Tempoe'?", "id": "Tempo" },
    { "en": "Kata Baku Dari 'Tentra'?", "id": "Tentara" },
    { "en": "Kata Baku Dari 'Tentram'?", "id": "Tenteram" },
    { "en": "Kata Baku Dari 'Tepok'?", "id": "Tepuk" },
    { "en": "Kata Baku Dari 'Teraphy'?", "id": "Terapi" },
    { "en": "Kata Baku Dari 'Terawangan'?", "id": "Terawang" },
    { "en": "Kata Baku Dari 'Tereak'?", "id": "Teriak" },
    { "en": "Kata Baku Dari 'Terompet'?", "id": "Trompet" },
    { "en": "Kata Baku Dari 'Terorist'?", "id": "Teroris" },
    { "en": "Kata Baku Dari 'Testimonial'?", "id": "Testimoni" },
    { "en": "Kata Baku Dari 'Tapi'?", "id": "Tetapi" },
    { "en": "Kata Baku Dari 'Type'?", "id": "Tipe" },
    { "en": "Kata Baku Dari 'Titel'?", "id": "Titel" },
    { "en": "Kata Baku Dari 'Togel'?", "id": "Toto Gelap" },
    { "en": "Kata Baku Dari 'Taufan'?", "id": "Topan" },
    { "en": "Kata Baku Dari 'Tragedy'?", "id": "Tragedi" },
    { "en": "Kata Baku Dari 'Transport'?", "id": "Transpor" },
    { "en": "Kata Baku Dari 'Tri Wulan'?", "id": "Triwulan" },
    { "en": "Kata Baku Dari 'Budeg'?", "id": "Tuli" },
    { "en": "Kata Baku Dari 'Tourist'?", "id": "Turis" },
    { "en": "Kata Baku Dari 'Turnament'?", "id": "Turnamen" },
    { "en": "Kata Baku Dari 'Urang'?", "id": "Udang" },
    { "en": "Kata Baku Dari 'Ultah'?", "id": "Ulang Tahun" },
    { "en": "Kata Baku Dari 'Unik'?", "id": "Unik" },
    { "en": "Kata Baku Dari 'Unit'?", "id": "Unit" },
    { "en": "Kata Baku Dari 'Universal'?", "id": "Universal" },
    { "en": "Kata Baku Dari 'Unsur'?", "id": "Unsur" },
    { "en": "Kata Baku Dari 'Up To Date'?", "id": "Mutakhir" },
    { "en": "Kata Baku Dari 'Uria'?", "id": "Urea" },
    { "en": "Kata Baku Dari 'Urolog'?", "id": "Urolog" },
    { "en": "Kata Baku Dari 'Usia'?", "id": "Usia" },
    { "en": "Kata Baku Dari 'Usil'?", "id": "Usil" },
    { "en": "Kata Baku Dari 'Usur'?", "id": "Usur" },
    { "en": "Kata Baku Dari 'Vagina'?", "id": "Vagina" },
    { "en": "Kata Baku Dari 'Vandel'?", "id": "Panji" },
    { "en": "Kata Baku Dari 'Vanili'?", "id": "Vanili" },
    { "en": "Kata Baku Dari 'Variasi'?", "id": "Variasi" },
    { "en": "Kata Baku Dari 'Vaskuler'?", "id": "Vaskular" },
    { "en": "Kata Baku Dari 'Vegetarian'?", "id": "Vegetarian" },
    { "en": "Kata Baku Dari 'Ventilasi'?", "id": "Ventilasi" },
    { "en": "Kata Baku Dari 'Verbal'?", "id": "Verbal" },
    { "en": "Kata Baku Dari 'Verifikasi'?", "id": "Verifikasi" },
    { "en": "Kata Baku Dari 'Versi'?", "id": "Versi" },
    { "en": "Kata Baku Dari 'Vertikal'?", "id": "Vertikal" },
    { "en": "Kata Baku Dari 'Veteran'?", "id": "Veteran" },
    { "en": "Kata Baku Dari 'Veto'?", "id": "Veto" },
    { "en": "Kata Baku Dari 'Viaduk'?", "id": "Viaduk" },
    { "en": "Kata Baku Dari 'Vibrasi'?", "id": "Vibrasi" },
    { "en": "Kata Baku Dari 'Videoklip'?", "id": "Video Klip" },
    { "en": "Kata Baku Dari 'Viking'?", "id": "Viking" },
    { "en": "Kata Baku Dari 'Vinyet'?", "id": "Vinyet" },
    { "en": "Kata Baku Dari 'Viola'?", "id": "Viola" },
    { "en": "Kata Baku Dari 'Violet'?", "id": "Violet" },
    { "en": "Kata Baku Dari 'Virus'?", "id": "Virus" },
    { "en": "Kata Baku Dari 'Visa'?", "id": "Visa" },
    { "en": "Kata Baku Dari 'Visibel'?", "id": "Visibel" },
    { "en": "Kata Baku Dari 'Visi'?", "id": "Visi" },
    { "en": "Kata Baku Dari 'Visual'?", "id": "Visual" },
    { "en": "Kata Baku Dari 'Vital'?", "id": "Vital" },
    { "en": "Kata Baku Dari 'Volum'?", "id": "Volume" },
    { "en": "Kata Baku Dari 'Vonis'?", "id": "Vonis" },
    { "en": "Kata Baku Dari 'Vocer'?", "id": "Voucher" },
    { "en": "Kata Baku Dari 'Waka'?", "id": "Wakil" },
    { "en": "Kata Baku Dari 'Walau Pun'?", "id": "Walaupun" },
    { "en": "Kata Baku Dari 'Wanita'?", "id": "Wanita" },
    { "en": "Kata Baku Dari 'Wartawanwati'?", "id": "Wartawati" },
    { "en": "Kata Baku Dari 'Wassalam'?", "id": "Wasalam" },
    { "en": "Kata Baku Dari 'Was Was'?", "id": "Waswas" },
    { "en": "Kata Baku Dari 'Wat'?", "id": "Watt" },
    { "en": "Kata Baku Dari 'Welas Asih'?", "id": "Belas Kasih" },
    { "en": "Kata Baku Dari 'Wijaya Kusuma'?", "id": "Wijayakusuma" },
    { "en": "Kata Baku Dari 'Wira Swasta'?", "id": "Wiraswasta" },
    { "en": "Kata Baku Dari 'Wortol'?", "id": "Wortel" },
    { "en": "Kata Baku Dari 'Yodium'?", "id": "Iodium" },
    { "en": "Kata Baku Dari 'Yuridiksi'?", "id": "Yurisdiksi" },
    { "en": "Kata Baku Dari 'Zalin'?", "id": "Zalim" },
    { "en": "Kata Baku Dari 'Dzat'?", "id": "Zat" },
    { "en": "Kata Baku Dari 'Jiarah'?", "id": "Ziarah" },
    { "en": "Kata Baku Dari 'Zombie'?", "id": "Zombi" },
    { "en": "Kata Baku Dari 'Adap'?", "id": "Adab" },
    { "en": "Kata Baku Dari 'Adjektif'?", "id": "Adjektiva" },
    { "en": "Kata Baku Dari 'Adrenaline'?", "id": "Adrenalin" },
    { "en": "Kata Baku Dari 'Afdol'?", "id": "Afdal" },
    { "en": "Kata Baku Dari 'Ahir'?", "id": "Akhir" },
    { "en": "Kata Baku Dari 'Akherat'?", "id": "Akhirat" },
    { "en": "Kata Baku Dari 'Ahlak'?", "id": "Akhlak" },
    { "en": "Kata Baku Dari 'Aqidah'?", "id": "Akidah" },
    { "en": "Kata Baku Dari 'Akli'?", "id": "Akil" },
    { "en": "Kata Baku Dari 'Alma Mater'?", "id": "Almamater" },
    { "en": "Kata Baku Dari 'Alphabet'?", "id": "Alfabet" },
    { "en": "Kata Baku Dari 'Alternatip'?", "id": "Alternatif" },
    { "en": "Kata Baku Dari 'Amanah'?", "id": "Amanat" },
    { "en": "Kata Baku Dari 'Amateur'?", "id": "Amatir" },
    { "en": "Kata Baku Dari 'Ambigo'?", "id": "Ambigu" },
    { "en": "Kata Baku Dari 'Ambles'?", "id": "Amblas" },
    { "en": "Kata Baku Dari 'Aamin'?", "id": "Amin" },
    { "en": "Kata Baku Dari 'Amoniak'?", "id": "Amonia" },
    { "en": "Kata Baku Dari 'A Moral'?", "id": "Amoral" },
    { "en": "Kata Baku Dari 'Amper'?", "id": "Ampere" },
    { "en": "Kata Baku Dari 'Aneka Ragam'?", "id": "Anekaragam" },
    { "en": "Kata Baku Dari 'Angkot'?", "id": "Angkutan Kota" },
    { "en": "Kata Baku Dari 'Antar Bangsa'?", "id": "Antarbangsa" },
    { "en": "Kata Baku Dari 'Antar Kota'?", "id": "Antarkota" },
    { "en": "Kata Baku Dari 'Antene'?", "id": "Antena" },
    { "en": "Kata Baku Dari 'Anti Karat'?", "id": "Antikarat" },
    { "en": "Kata Baku Dari 'Apartement'?", "id": "Apartemen" },
    { "en": "Kata Baku Dari 'Aplus'?", "id": "Aplaus" },
    { "en": "Kata Baku Dari 'Arob'?", "id": "Arab" },
    { "en": "Kata Baku Dari 'Arsistek'?", "id": "Arsitek" },
    { "en": "Kata Baku Dari 'Arteris'?", "id": "Arteri" },
    { "en": "Kata Baku Dari 'Artifisial'?", "id": "Artifisial" },
    { "en": "Kata Baku Dari 'Asa'?", "id": "Asa" },
    { "en": "Kata Baku Dari 'Ashar'?", "id": "Asar" },
    { "en": "Kata Baku Dari 'Asin'?", "id": "Asin" },
    { "en": "Kata Baku Dari 'Aspal'?", "id": "Aspal" },
    { "en": "Kata Baku Dari 'Aspek'?", "id": "Aspek" },
    { "en": "Kata Baku Dari 'Assalamu'alaikum'?", "id": "Assalamualaikum" },
    { "en": "Kata Baku Dari 'Astrolog'?", "id": "Astrolog" },
    { "en": "Kata Baku Dari 'Atas Nama'?", "id": "Atas Nama" },
    { "en": "Kata Baku Dari 'Atheis'?", "id": "Ateis" },
    { "en": "Kata Baku Dari 'Aurat'?", "id": "Aurat" },
    { "en": "Kata Baku Dari 'Autobiografi'?", "id": "Autobiografi" },
    { "en": "Kata Baku Dari 'Avan'?", "id": "Avan" },
    { "en": "Kata Baku Dari 'Avokad'?", "id": "Avokad" },
    { "en": "Kata Baku Dari 'Azan'?", "id": "Azan" },
    { "en": "Kata Baku Dari 'Babad'?", "id": "Babad" },
    { "en": "Kata Baku Dari 'Babet'?", "id": "Babat" },
    { "en": "Kata Baku Dari 'Bacang'?", "id": "Bakcang" },
    { "en": "Kata Baku Dari 'Badal'?", "id": "Badal" },
    { "en": "Kata Baku Dari 'Badan'?", "id": "Badan" },
    { "en": "Kata Baku Dari 'Bae'?", "id": "Saja" },
    { "en": "Kata Baku Dari 'Bagasi'?", "id": "Bagasi" },
    { "en": "Kata Baku Dari 'Bagito'?", "id": "Begitu" },
    { "en": "Kata Baku Dari 'Bahadur'?", "id": "Bahadur" },
    { "en": "Kata Baku Dari 'Bahasa'?", "id": "Bahasa" },
    { "en": "Kata Baku Dari 'Bahenol'?", "id": "Seksi" },
    { "en": "Kata Baku Dari 'Bahtera'?", "id": "Bahtera" },
    { "en": "Kata Baku Dari 'Bait'?", "id": "Bait" },
    { "en": "Kata Baku Dari 'Baja'?", "id": "Baja" },
    { "en": "Kata Baku Dari 'Bajaj'?", "id": "Bajaj" },
    { "en": "Kata Baku Dari 'Begal'?", "id": "Begal" },
    { "en": "Kata Baku Dari 'Bak'?", "id": "Bak" },
    { "en": "Kata Baku Dari 'Bakar'?", "id": "Bakar" },
    { "en": "Kata Baku Dari 'Bakau'?", "id": "Bakau" },
    { "en": "Kata Baku Dari 'Bakmi'?", "id": "Bakmi" },
    { "en": "Kata Baku Dari 'Bako'?", "id": "Tembakau" },
    { "en": "Kata Baku Dari 'Bakteri'?", "id": "Bakteri" },
    { "en": "Kata Baku Dari 'Bakti'?", "id": "Bakti" },
    { "en": "Kata Baku Dari 'Baku'?", "id": "Baku" },
    { "en": "Kata Baku Dari 'Bala'?", "id": "Bala" },
    { "en": "Kata Baku Dari 'Balada'?", "id": "Balada" },
    { "en": "Kata Baku Dari 'Balapan'?", "id": "Balapan" },
    { "en": "Kata Baku Dari 'Ballet'?", "id": "Balet" },
    { "en": "Kata Baku Dari 'Bandal'?", "id": "Bandel" },
    { "en": "Kata Baku Dari 'Bapa'?", "id": "Bapak" },
    { "en": "Kata Baku Dari 'Babtis'?", "id": "Baptis" },
    { "en": "Kata Baku Dari 'Bar Bar'?", "id": "Barbar" },
    { "en": "Kata Baku Dari 'Baro Meter'?", "id": "Barometer" },
    { "en": "Kata Baku Dari 'Basa Basi'?", "id": "Basa-basi" },
    { "en": "Kata Baku Dari 'Basyah'?", "id": "Basah" },
    { "en": "Kata Baku Dari 'Bawak'?", "id": "Bawa" },
    { "en": "Kata Baku Dari 'Bazaar'?", "id": "Bazar" },
    { "en": "Kata Baku Dari 'Bea Siswa'?", "id": "Beasiswa" },
    { "en": "Kata Baku Dari 'Bedug'?", "id": "Beduk" },
    { "en": "Kata Baku Dari 'Begadang'?", "id": "Bergadang" },
    { "en": "Kata Baku Dari 'Gini'?", "id": "Begini" },
    { "en": "Kata Baku Dari 'Bekel'?", "id": "Bekal" },
    { "en": "Kata Baku Dari 'Backing'?", "id": "Dukungan" },
    { "en": "Kata Baku Dari 'Bela Sungkawa'?", "id": "Belasungkawa" },
    { "en": "Kata Baku Dari 'Bli'?", "id": "Beli" },
    { "en": "Kata Baku Dari 'Beloon'?", "id": "Tolol" },
    { "en": "Kata Baku Dari 'Blom'?", "id": "Belum" },
    { "en": "Kata Baku Dari 'Bener'?", "id": "Benar" },
    { "en": "Kata Baku Dari 'Bentar'?", "id": "Sebentar" },
    { "en": "Kata Baku Dari 'Brapa'?", "id": "Berapa" },
    { "en": "Kata Baku Dari 'Berdi Kari'?", "id": "Berdikari" },
    { "en": "Bentuk Baku Dari 'Beresin'?", "id": "Membereskan" },
    { "en": "Kata Baku Dari 'Kasi'?", "id": "Beri" },
    { "en": "Kata Baku Dari 'Berkwalitas'?", "id": "Berkualitas" },
    { "en": "Kata Baku Dari 'Gede'?", "id": "Besar" },
    { "en": "Kata Baku Dari 'Beta'?", "id": "Saya" },
    { "en": "Kata Baku Dari 'Biang Lala'?", "id": "Bianglala" },
    { "en": "Kata Baku Dari 'Bibel'?", "id": "Alkitab" },
    { "en": "Kata Baku Dari 'Bibliograpi'?", "id": "Bibliografi" },
    { "en": "Kata Baku Dari 'Bid'ah'?", "id": "Bidah" },
    { "en": "Kata Baku Dari 'Bikin'?", "id": "Buat" },
    { "en": "Kata Baku Dari 'Bilang'?", "id": "Berkata" },
    { "en": "Kata Baku Dari 'Bilyet'?", "id": "Bilet" },
    { "en": "Kata Baku Dari 'Binaraga'?", "id": "Binaraga" },
    { "en": "Kata Baku Dari 'Bincang'?", "id": "Bincang" },
    { "en": "Kata Baku Dari 'Binder'?", "id": "Binder" },
    { "en": "Kata Baku Dari 'Bingkai'?", "id": "Bingkai" },
    { "en": "Kata Baku Dari 'Bingung'?", "id": "Bingung" },
    { "en": "Kata Baku Dari 'Bintik'?", "id": "Bintik" },
    { "en": "Kata Baku Dari 'Biografi'?", "id": "Biografi" },
    { "en": "Kata Baku Dari 'Biola'?", "id": "Biola" },
    { "en": "Kata Baku Dari 'Bipori'?", "id": "Biopori" },
    { "en": "Kata Baku Dari 'Birama'?", "id": "Birama" },
    { "en": "Kata Baku Dari 'Birokrasi'?", "id": "Birokrasi" },
    { "en": "Kata Baku Dari 'Biru'?", "id": "Biru" },
    { "en": "Kata Baku Dari 'Bisa'?", "id": "Bisa" },
    { "en": "Kata Baku Dari 'Bisik'?", "id": "Bisik" },
    { "en": "Kata Baku Dari 'Biskuit'?", "id": "Biskuit" },
    { "en": "Kata Baku Dari 'Bismillah'?", "id": "Bismillah" },
    { "en": "Kata Baku Dari 'Bisu'?", "id": "Tuna Wicara" },
    { "en": "Kata Baku Dari 'Bius'?", "id": "Bius" },
    { "en": "Kata Baku Dari 'Blaster'?", "id": "Blaster" },
    { "en": "Kata Baku Dari 'Blender'?", "id": "Blender" },
    { "en": "Kata Baku Dari 'Blok'?", "id": "Blok" },
    { "en": "Kata Baku Dari 'Blokir'?", "id": "Blokir" },
    { "en": "Kata Baku Dari 'Blong'?", "id": "Blong" },
    { "en": "Kata Baku Dari 'Blus'?", "id": "Blus" },
    { "en": "Kata Baku Dari 'Bobot'?", "id": "Bobot" },
    { "en": "Kata Baku Dari 'Bocor'?", "id": "Bocor" },
    { "en": "Kata Baku Dari 'Bodong'?", "id": "Bodong" },
    { "en": "Kata Baku Dari 'Boikot'?", "id": "Boikot" },
    { "en": "Kata Baku Dari 'Bola'?", "id": "Bola" },
    { "en": "Kata Baku Dari 'Boleh'?", "id": "Boleh" },
    { "en": "Kata Baku Dari 'Bolos'?", "id": "Bolos" },
    { "en": "Kata Baku Dari 'Bolu'?", "id": "Bolu" },
    { "en": "Kata Baku Dari 'Bom'?", "id": "Bom" },
    { "en": "Kata Baku Dari 'Bombardir'?", "id": "Bombardir" },
    { "en": "Kata Baku Dari 'Bon'?", "id": "Bon" },
    { "en": "Kata Baku Dari 'Boneka'?", "id": "Boneka" },
    { "en": "Kata Baku Dari 'Bongkar'?", "id": "Bongkar" },
    { "en": "Kata Baku Dari 'Bonus'?", "id": "Bonus" },
    { "en": "Kata Baku Dari 'Boplang'?", "id": "Boplang" },
    { "en": "Kata Baku Dari 'Borgol'?", "id": "Borgol" },
    { "en": "Kata Baku Dari 'Boros'?", "id": "Boros" },
    { "en": "Kata Baku Dari 'Bos'?", "id": "Bos" },
    { "en": "Kata Baku Dari 'Bosan'?", "id": "Bosan" },
    { "en": "Kata Baku Dari 'Botak'?", "id": "Botak" },
    { "en": "Kata Baku Dari 'Botani'?", "id": "Botani" },
    { "en": "Kata Baku Dari 'Botol'?", "id": "Botol" },
    { "en": "Kata Baku Dari 'Boyo'?", "id": "Buaya" },
    { "en": "Kata Baku Dari 'Bra'?", "id": "Bra" },
    { "en": "Kata Baku Dari 'Branding'?", "id": "Jenama" },
    { "en": "Kata Baku Dari 'Brankas'?", "id": "Brankas" },
    { "en": "Kata Baku Dari 'Briptu'?", "id": "Briptu" },
    { "en": "Kata Baku Dari 'Brokoli'?", "id": "Brokoli" },
    { "en": "Kata Baku Dari 'Broker'?", "id": "Broker" },
    { "en": "Kata Baku Dari 'Bros'?", "id": "Bros" },
    { "en": "Kata Baku Dari 'Browser'?", "id": "Peramban" },
    { "en": "Kata Baku Dari 'Buah'?", "id": "Buah" },
    { "en": "Kata Baku Dari 'Brosur'?", "id": "Brosur" },
    { "en": "Kata Baku Dari 'Bual'?", "id": "Bual" },
    { "en": "Kata Baku Dari 'Buana'?", "id": "Buana" },
    { "en": "Kata Baku Dari 'Buaya'?", "id": "Buaya" },
    { "en": "Kata Baku Dari 'Budi Daya'?", "id": "Budidaya" },
    { "en": "Kata Baku Dari 'Budha'?", "id": "Buddha" },
    { "en": "Kata Baku Dari 'Buih'?", "id": "Buih" },
    { "en": "Kata Baku Dari 'Bujur'?", "id": "Bujur" },
    { "en": "Kata Baku Dari 'Bulet'?", "id": "Bulat" },
    { "en": "Kata Baku Dari 'Bulu Tangkis'?", "id": "Bulutangkis" },
    { "en": "Kata Baku Dari 'Bunder'?", "id": "Bundar" },
    { "en": "Kata Baku Dari 'Bunglon'?", "id": "Bunglon" },
    { "en": "Kata Baku Dari 'Bunting'?", "id": "Hamil" },
    { "en": "Kata Baku Dari 'Buta Huruf'?", "id": "Butahuruf" },
    { "en": "Kata Baku Dari 'Butek'?", "id": "Keruh" },
    { "en": "Kata Baku Dari 'Cabol'?", "id": "Cabul" },
    { "en": "Kata Baku Dari 'Caem'?", "id": "Cantik" },
    { "en": "Kata Baku Dari 'Cakra Wala'?", "id": "Cakrawala" },
    { "en": "Kata Baku Dari 'Jambang'?", "id": "Cambang" },
    { "en": "Kata Baku Dari 'Campuraduk'?", "id": "Campur Aduk" },
    { "en": "Kata Baku Dari 'Cangih'?", "id": "Canggih" },
    { "en": "Kata Baku Dari 'Cebok'?", "id": "Bercebok" },
    { "en": "Kata Baku Dari 'Cecak'?", "id": "Cicak" },
    { "en": "Kata Baku Dari 'Cedok'?", "id": "Ciduk" },
    { "en": "Kata Baku Dari 'Centil'?", "id": "Genit" },
    { "en": "Kata Baku Dari 'Ceplas Ceplos'?", "id": "Ceplas-ceplos" },
    { "en": "Kata Baku Dari 'Cerai Berai'?", "id": "Cerai-berai" },
    { "en": "Kata Baku Dari 'Ceritera'?", "id": "Cerita" },
    { "en": "Kata Baku Dari 'Cilik'?", "id": "Kecil" },
    { "en": "Kata Baku Dari 'Cita Cita'?", "id": "Cita-cita" },
    { "en": "Kata Baku Dari 'Copyright'?", "id": "Hak Cipta" },
    { "en": "Kata Baku Dari 'Coret Moret'?", "id": "Coret-moret" },
    { "en": "Kata Baku Dari 'Cowok'?", "id": "Laki-laki" },
    { "en": "Kata Baku Dari 'Daku'?", "id": "Aku" },
    { "en": "Kata Baku Dari 'Dandan'?", "id": "Berdandan" },
    { "en": "Kata Baku Dari 'Dengkul'?", "id": "Dengkul" },
    { "en": "Kata Baku Dari 'Didepan'?", "id": "Di Depan" },
    { "en": "Kata Baku Dari 'Dikursi'?", "id": "Di Kursi" },
    { "en": "Kata Baku Dari 'Dimana'?", "id": "Di Mana" },
    { "en": "Kata Baku Dari 'Dipersilahkan'?", "id": "Dipersilakan" },
    { "en": "Kata Baku Dari 'Dipinggir'?", "id": "Di Pinggir" },
    { "en": "Kata Baku Dari 'Dolar'?", "id": "Dolar" },
    { "en": "Kata Baku Dari 'Doping'?", "id": "Doping" },
    { "en": "Kata Baku Dari 'Draf'?", "id": "Draf" },
    { "en": "Kata Baku Dari 'Drama'?", "id": "Drama" },
    { "en": "Kata Baku Dari 'Drastis'?", "id": "Drastis" },
    { "en": "Kata Baku Dari 'Dribel'?", "id": "Dribel" },
    { "en": "Kata Baku Dari 'Duafa'?", "id": "Duafa" },
    { "en": "Kata Baku Dari 'Dubur'?", "id": "Dubur" },
    { "en": "Kata Baku Dari 'Duda'?", "id": "Duda" },
    { "en": "Kata Baku Dari 'Dukun'?", "id": "Dukun" },
    { "en": "Kata Baku Dari 'Dukuh'?", "id": "Dukuh" },
    { "en": "Kata Baku Dari 'Dungu'?", "id": "Dungu" },
    { "en": "Kata Baku Dari 'Dunia'?", "id": "Dunia" },
    { "en": "Kata Baku Dari 'Duplikat'?", "id": "Duplikat" },
    { "en": "Kata Baku Dari 'Duri'?", "id": "Duri" },
    { "en": "Kata Baku Dari 'Durjana'?", "id": "Durjana" },
    { "en": "Kata Baku Dari 'Dusta'?", "id": "Dusta" },
    { "en": "Kata Baku Dari 'Dusun'?", "id": "Dusun" },
    { "en": "Kata Baku Dari 'Duta'?", "id": "Duta" },
    { "en": "Kata Baku Dari 'Ebi'?", "id": "Ebi" },
    { "en": "Kata Baku Dari 'Ebonit'?", "id": "Ebonit" },
    { "en": "Kata Baku Dari 'Efisien'?", "id": "Efisien" },
    { "en": "Kata Baku Dari 'Egaliter'?", "id": "Egaliter" },
    { "en": "Kata Baku Dari 'Egois'?", "id": "Egois" },
    { "en": "Kata Baku Dari 'Eja'?", "id": "Eja" },
    { "en": "Kata Baku Dari 'Ejek'?", "id": "Ejek" },
    { "en": "Kata Baku Dari 'Ekor'?", "id": "Ekor" },
    { "en": "Kata Baku Dari 'Eksemplar'?", "id": "Eksemplar" },
    { "en": "Kata Baku Dari 'Eksim'?", "id": "Eksim" },
    { "en": "Kata Baku Dari 'Eksis'?", "id": "Eksis" },
    { "en": "Kata Baku Dari 'Ekslusif'?", "id": "Eksklusif" },
    { "en": "Kata Baku Dari 'Eksperimen'?", "id": "Eksperimen" },
    { "en": "Kata Baku Dari 'Ekspo'?", "id": "Ekspo" },
    { "en": "Kata Baku Dari 'Ekstrakurikuler'?", "id": "Ekstrakurikuler" },
    { "en": "Kata Baku Dari 'Ekuador'?", "id": "Ekuador" },
    { "en": "Kata Baku Dari 'Elang'?", "id": "Elang" },
    { "en": "Kata Baku Dari 'Elastis'?", "id": "Elastis" },
    { "en": "Kata Baku Dari 'Elektronik'?", "id": "Elektronik" },
    { "en": "Kata Baku Dari 'Eling'?", "id": "Ingat" },
    { "en": "Kata Baku Dari 'Elite'?", "id": "Elite" },
    { "en": "Kata Baku Dari 'Embargo'?", "id": "Embargo" },
    { "en": "Kata Baku Dari 'Embarkasi'?", "id": "Embarkasi" },
    { "en": "Kata Baku Dari 'Ember'?", "id": "Ember" },
    { "en": "Kata Baku Dari 'Embrional'?", "id": "Embrional" },
    { "en": "Kata Baku Dari 'Embus'?", "id": "Embus" },
    { "en": "Kata Baku Dari 'Emigrasi'?", "id": "Emigrasi" },
    { "en": "Kata Baku Dari 'Emosi'?", "id": "Emosi" },
    { "en": "Kata Baku Dari 'Empati'?", "id": "Empati" },
    { "en": "Kata Baku Dari 'Empedu'?", "id": "Empedu" },
    { "en": "Kata Baku Dari 'Empiris'?", "id": "Empiris" },
    { "en": "Kata Baku Dari 'Empuk'?", "id": "Empuk" },
    { "en": "Kata Baku Dari 'Enak'?", "id": "Enak" },
    { "en": "Kata Baku Dari 'Encer'?", "id": "Encer" },
    { "en": "Kata Baku Dari 'Endap'?", "id": "Endap" },
    { "en": "Kata Baku Dari 'Engas'?", "id": "Engas" },
    { "en": "Kata Baku Dari 'Ogah'?", "id": "Enggan" },
    { "en": "Kata Baku Dari 'Ensiklopedi'?", "id": "Ensiklopedia" },
    { "en": "Kata Baku Dari 'Ensim'?", "id": "Enzim" },
    { "en": "Kata Baku Dari 'Erotik'?", "id": "Erotis" },
    { "en": "Kata Baku Dari 'Estapet'?", "id": "Estafet" },
    { "en": "Kata Baku Dari 'Estetik'?", "id": "Estetika" },
    { "en": "Kata Baku Dari 'Euphoria'?", "id": "Euforia" },
    { "en": "Kata Baku Dari 'Fable'?", "id": "Fabel" },
    { "en": "Kata Baku Dari 'Falsafah'?", "id": "Filsafat" },
    { "en": "Kata Baku Dari 'Fantastik'?", "id": "Fantastis" },
    { "en": "Kata Baku Dari 'Pardu'?", "id": "Fardu" },
    { "en": "Kata Baku Dari 'Pasilitas'?", "id": "Fasilitas" },
    { "en": "Kata Baku Dari 'Ferry'?", "id": "Feri" },
    { "en": "Kata Baku Dari 'Fiktip'?", "id": "Fiktif" },
    { "en": "Kata Baku Dari 'File'?", "id": "Berkas" },
    { "en": "Kata Baku Dari 'Pilem'?", "id": "Film" },
    { "en": "Kata Baku Dari 'Finish'?", "id": "Finis" },
    { "en": "Kata Baku Dari 'Sirik'?", "id": "Iri" },
    { "en": "Kata Baku Dari 'Fisiotrapi'?", "id": "Fisioterapi" },
    { "en": "Kata Baku Dari 'Pitnah'?", "id": "Fitnah" },
    { "en": "Kata Baku Dari 'Plu'?", "id": "Flu" },
    { "en": "Kata Baku Dari 'Pokus'?", "id": "Fokus" },
    { "en": "Kata Baku Dari 'Folklore'?", "id": "Cerita Rakyat" },
    { "en": "Kata Baku Dari 'Ponem'?", "id": "Fonem" },
    { "en": "Kata Baku Dari 'Pormulir'?", "id": "Formulir" },
    { "en": "Kata Baku Dari 'Photograper'?", "id": "Fotografer" },
    { "en": "Kata Baku Dari 'Fotosintesa'?", "id": "Fotosintesis" },
    { "en": "Kata Baku Dari 'Prambusia'?", "id": "Frambusia" },
    { "en": "Kata Baku Dari 'Fungsionil'?", "id": "Fungsional" },
    { "en": "Kata Baku Dari 'Cewek'?", "id": "Gadis" },
    { "en": "Kata Baku Dari 'Gantirugi'?", "id": "Ganti Rugi" },
    { "en": "Kata Baku Dari 'Gara Gara'?", "id": "Gara-gara" },
    { "en": "Kata Baku Dari 'Gatel'?", "id": "Gatal" },
    { "en": "Kata Baku Dari 'Glagapan'?", "id": "Gelagapan" },
    { "en": "Kata Baku Dari 'Gendut'?", "id": "Gemuk" },
    { "en": "Kata Baku Dari 'Gencatansenjata'?", "id": "Gencatan Senjata" },
    { "en": "Kata Baku Dari 'Genius'?", "id": "Jenius" },
    { "en": "Kata Baku Dari 'Gerakjalan'?", "id": "Gerak Jalan" },
    { "en": "Kata Baku Dari 'Grimis'?", "id": "Gerimis" },
    { "en": "Kata Baku Dari 'Getoktular'?", "id": "Getok Tular" },
    { "en": "Kata Baku Dari 'Giduan'?", "id": "Panduan" },
    { "en": "Kata Baku Dari 'Giga'?", "id": "Giga" },
    { "en": "Kata Baku Dari 'Gigi'?", "id": "Gigi" },
    { "en": "Kata Baku Dari 'Gigit'?", "id": "Gigit" },
    { "en": "Kata Baku Dari 'Gila'?", "id": "Gila" },
    { "en": "Kata Baku Dari 'Giling'?", "id": "Giling" },
    { "en": "Kata Baku Dari 'Gilir'?", "id": "Gilir" },
    { "en": "Kata Baku Dari 'Ginjal'?", "id": "Ginjal" },
    { "en": "Kata Baku Dari 'Gips'?", "id": "Gips" },
    { "en": "Kata Baku Dari 'Girah'?", "id": "Gairah" },
    { "en": "Kata Baku Dari 'Gitar'?", "id": "Gitar" },
    { "en": "Kata Baku Dari 'Global'?", "id": "Global" },
    { "en": "Kata Baku Dari 'Gladiator'?", "id": "Gladiator" },
    { "en": "Kata Baku Dari 'Gletser'?", "id": "Gletser" },
    { "en": "Kata Baku Dari 'Gol'?", "id": "Gol" },
    { "en": "Kata Baku Dari 'Golok'?", "id": "Golok" },
    { "en": "Kata Baku Dari 'Gong'?", "id": "Gong" },
    { "en": "Kata Baku Dari 'Gotong Royong'?", "id": "Gotong Royong" },
    { "en": "Kata Baku Dari 'Goyah'?", "id": "Goyah" },
    { "en": "Kata Baku Dari 'Grafik'?", "id": "Grafik" },
    { "en": "Kata Baku Dari 'Gramatika'?", "id": "Gramatika" },
    { "en": "Kata Baku Dari 'Granat'?", "id": "Granat" },
    { "en": "Kata Baku Dari 'Gratis'?", "id": "Gratis" },
    { "en": "Kata Baku Dari 'Grosir'?", "id": "Grosir" },
    { "en": "Kata Baku Dari 'Gua'?", "id": "Gua" },
    { "en": "Kata Baku Dari 'Gubernur'?", "id": "Gubernur" },
    { "en": "Kata Baku Dari 'Gugah'?", "id": "Gugah" },
    { "en": "Kata Baku Dari 'Gugat'?", "id": "Gugat" },
    { "en": "Kata Baku Dari 'Gugur'?", "id": "Gugur" },
    { "en": "Kata Baku Dari 'Gula'?", "id": "Gula" },
    { "en": "Kata Baku Dari 'Gulai'?", "id": "Gulai" },
    { "en": "Kata Baku Dari 'Gulat'?", "id": "Gulat" },
    { "en": "Kata Baku Dari 'Gulita'?", "id": "Gulita" },
    { "en": "Kata Baku Dari 'Gulung'?", "id": "Gulung" },
    { "en": "Kata Baku Dari 'Guna'?", "id": "Guna" },
    { "en": "Kata Baku Dari 'Gundah'?", "id": "Gundah" },
    { "en": "Kata Baku Dari 'Gundik'?", "id": "Gundik" },
    { "en": "Kata Baku Dari 'Gunting'?", "id": "Gunting" },
    { "en": "Kata Baku Dari 'Guntur'?", "id": "Guntur" },
    { "en": "Kata Baku Dari 'Gunung'?", "id": "Gunung" },
    { "en": "Kata Baku Dari 'Gurami'?", "id": "Gurami" },
    { "en": "Kata Baku Dari 'Gurun'?", "id": "Gurun" },
    { "en": "Kata Baku Dari 'Gurita'?", "id": "Gurita" },
    { "en": "Kata Baku Dari 'Guru'?", "id": "Guru" },
    { "en": "Kata Baku Dari 'Gusar'?", "id": "Gusar" },
    { "en": "Kata Baku Dari 'Gusur'?", "id": "Gusur" },
    { "en": "Kata Baku Dari 'Habeas Corpus'?", "id": "Habeas Corpus" },
    { "en": "Kata Baku Dari 'Habitat'?", "id": "Habitat" },
    { "en": "Kata Baku Dari 'Hadiah'?", "id": "Hadiah" },
    { "en": "Kata Baku Dari 'Haid'?", "id": "Haid" },
    { "en": "Kata Baku Dari 'Hajar'?", "id": "Hajar" },
    { "en": "Kata Baku Dari 'Hajat'?", "id": "Hajat" },
    { "en": "Kata Baku Dari 'Hak'?", "id": "Hak" },
    { "en": "Kata Baku Dari 'Hakikat'?", "id": "Hakikat" },
    { "en": "Kata Baku Dari 'Hakim'?", "id": "Hakim" },
    { "en": "Kata Baku Dari 'Halal Bihalal'?", "id": "Halalbihalal" },
    { "en": "Kata Baku Dari 'Alang'?", "id": "Halang" },
    { "en": "Kata Baku Dari 'Halusinasyen'?", "id": "Halusinasi" },
    { "en": "Kata Baku Dari 'Amba'?", "id": "Hamba" },
    { "en": "Kata Baku Dari 'Ancur'?", "id": "Hancur" },
    { "en": "Kata Baku Dari 'Anduk'?", "id": "Handuk" },
    { "en": "Kata Baku Dari 'Angat'?", "id": "Hangat" },
    { "en": "Kata Baku Dari 'Angus'?", "id": "Hangus" },
    { "en": "Kata Baku Dari 'Anyut'?", "id": "Hanyut" },
    { "en": "Kata Baku Dari 'Apus'?", "id": "Hapus" },
    { "en": "Kata Baku Dari 'Harfiyah'?", "id": "Harfiah" },
    { "en": "Kata Baku Dari 'Hasad'?", "id": "Dengki" },
    { "en": "Kata Baku Dari 'Hasyis'?", "id": "Hasis" },
    { "en": "Kata Baku Dari 'Hati Hati'?", "id": "Hati-hati" },
    { "en": "Kata Baku Dari 'Aus'?", "id": "Haus" },
    { "en": "Kata Baku Dari 'Hawanafsu'?", "id": "Hawa Nafsu" },
    { "en": "Kata Baku Dari 'Hidayat'?", "id": "Hidayah" },
    { "en": "Kata Baku Dari 'Hikmat'?", "id": "Hikmah" },
    { "en": "Kata Baku Dari 'Ilang'?", "id": "Hilang" },
    { "en": "Kata Baku Dari 'Itung'?", "id": "Hitung" },
    { "en": "Kata Baku Dari 'Hormone'?", "id": "Hormon" },
    { "en": "Kata Baku Dari 'Horror'?", "id": "Horor" },
    { "en": "Kata Baku Dari 'Ujan'?", "id": "Hujan" },
    { "en": "Kata Baku Dari 'Icing'?", "id": "Gula-gula" },
    { "en": "Kata Baku Dari 'Identipikasi'?", "id": "Identifikasi" },
    { "en": "Kata Baku Dari 'Idul Adha'?", "id": "Iduladha" },
    { "en": "Kata Baku Dari 'Igloo'?", "id": "Iglo" },
    { "en": "Kata Baku Dari 'Ihsan'?", "id": "Ihsan" },
    { "en": "Kata Baku Dari 'Ihtiar'?", "id": "Ikhtiar" },
    { "en": "Kata Baku Dari 'Icon'?", "id": "Ikon" },
    { "en": "Kata Baku Dari 'Ikutin'?", "id": "Mengikuti" },
    { "en": "Kata Baku Dari 'Illusi'?", "id": "Ilusi" },
    { "en": "Kata Baku Dari 'Imingiming'?", "id": "Iming-iming" },
    { "en": "Kata Baku Dari 'Imperialisma'?", "id": "Imperialisme" },
    { "en": "Kata Baku Dari 'Influensa'?", "id": "Influenza" },
    { "en": "Kata Baku Dari 'Instant'?", "id": "Instan" },
    { "en": "Kata Baku Dari 'Institute'?", "id": "Institut" },
    { "en": "Kata Baku Dari 'Instrument'?", "id": "Instrumen" },
    { "en": "Kata Baku Dari 'Insya Allah'?", "id": "Insyaallah" },
    { "en": "Kata Baku Dari 'Interkoneksi'?", "id": "Antarkoneksi" },
    { "en": "Kata Baku Dari 'International'?", "id": "Internasional" },
    { "en": "Kata Baku Dari 'Interpretasi'?", "id": "Interpretasi" },
    { "en": "Kata Baku Dari 'Interupsi'?", "id": "Interupsi" },
    { "en": "Kata Baku Dari 'Interview'?", "id": "Wawancara" },
    { "en": "Kata Baku Dari 'Inti'?", "id": "Inti" },
    { "en": "Kata Baku Dari 'Intro'?", "id": "Intro" },
    { "en": "Kata Baku Dari 'Intuisi'?", "id": "Intuisi" },
    { "en": "Kata Baku Dari 'Invasi'?", "id": "Invasi" },
    { "en": "Kata Baku Dari 'Investasi'?", "id": "Investasi" },
    { "en": "Kata Baku Dari 'Irama'?", "id": "Irama" },
    { "en": "Kata Baku Dari 'Irasional'?", "id": "Irasional" },
    { "en": "Kata Baku Dari 'Irigasi'?", "id": "Irigasi" },
    { "en": "Kata Baku Dari 'Isi'?", "id": "Isi" },
    { "en": "Kata Baku Dari 'Islami'?", "id": "Islami" },
    { "en": "Kata Baku Dari 'Isolasi'?", "id": "Isolasi" },
    { "en": "Kata Baku Dari 'Isolatip'?", "id": "Selotip" },
    { "en": "Kata Baku Dari 'Isotop'?", "id": "Isotop" },
    { "en": "Kata Baku Dari 'Istana'?", "id": "Istana" },
    { "en": "Kata Baku Dari 'Istimewa'?", "id": "Istimewa" },
    { "en": "Kata Baku Dari 'Istiqlal'?", "id": "Istiqlal" },
    { "en": "Kata Baku Dari 'Istirahat'?", "id": "Istirahat" },
    { "en": "Kata Baku Dari 'Istri'?", "id": "Istri" },
    { "en": "Kata Baku Dari 'Isu'?", "id": "Isu" },
    { "en": "Kata Baku Dari 'Isya'?", "id": "Isya" },
    { "en": "Kata Baku Dari 'Itikad'?", "id": "Iktikad" },
    { "en": "Kata Baku Dari 'Itu'?", "id": "Itu" },
    { "en": "Kata Baku Dari 'Izin'?", "id": "Izin" },
    { "en": "Kata Baku Dari 'Jababeka'?", "id": "Jababeka" },
    { "en": "Kata Baku Dari 'Jabar'?", "id": "Jawa Barat" },
    { "en": "Kata Baku Dari 'Jadul'?", "id": "Zaman Dulu" },
    { "en": "Kata Baku Dari 'Jaga'?", "id": "Jaga" },
    { "en": "Kata Baku Dari 'Jagat'?", "id": "Jagat" },
    { "en": "Kata Baku Dari 'Jago'?", "id": "Jago" },
    { "en": "Kata Baku Dari 'Jagoan'?", "id": "Jagoan" },
    { "en": "Kata Baku Dari 'Jagung'?", "id": "Jagung" },
    { "en": "Kata Baku Dari 'Jahannam'?", "id": "Jahanam" },
    { "en": "Kata Baku Dari 'Jahiliyah'?", "id": "Jahiliah" },
    { "en": "Kata Baku Dari 'Jahit'?", "id": "Jahit" },
    { "en": "Kata Baku Dari 'Jaipong'?", "id": "Jaipong" },
    { "en": "Kata Baku Dari 'Jaja'?", "id": "Jaja" },
    { "en": "Kata Baku Dari 'Jajak'?", "id": "Jajak" },
    { "en": "Kata Baku Dari 'Jajan'?", "id": "Jajan" },
    { "en": "Kata Baku Dari 'Jajar'?", "id": "Jajar" },
    { "en": "Kata Baku Dari 'Jaket'?", "id": "Jaket" },
    { "en": "Kata Baku Dari 'Jaksa'?", "id": "Jaksa" },
    { "en": "Kata Baku Dari 'Jala'?", "id": "Jala" },
    { "en": "Kata Baku Dari 'Jalan'?", "id": "Jalan" },
    { "en": "Kata Baku Dari 'Jalang'?", "id": "Jalang" },
    { "en": "Kata Baku Dari 'Jalar'?", "id": "Jalar" },
    { "en": "Kata Baku Dari 'Jali'?", "id": "Jali" },
    { "en": "Kata Baku Dari 'Jalin'?", "id": "Jalin" },
    { "en": "Kata Baku Dari 'Jalu'?", "id": "Jalu" },
    { "en": "Kata Baku Dari 'Jalur'?", "id": "Jalur" },
    { "en": "Kata Baku Dari 'Halal Bihalal'?", "id": "Halalbihalal" },
    { "en": "Kata Baku Dari 'Hati Hati'?", "id": "Hati-hati" },
    { "en": "Kata Baku Dari 'Hawanafsu'?", "id": "Hawa Nafsu" },
    { "en": "Kata Baku Dari 'Idul Adha'?", "id": "Iduladha" },
    { "en": "Kata Baku Dari 'Jam Berapa'?", "id": "Pukul Berapa" },
    { "en": "Kata Baku Dari 'Ga Usah'?", "id": "Tidak Perlu" },
    { "en": "Kata Baku Dari 'Jatoh'?", "id": "Jatuh" },
    { "en": "Kata Baku Dari 'Elek'?", "id": "Jelek" },
    { "en": "Kata Baku Dari 'Jengkelin'?", "id": "Menjengkelkan" },
    { "en": "Kata Baku Dari 'Macem'?", "id": "Macam" },
    { "en": "Kata Baku Dari 'Jepun'?", "id": "Jepang" },
    { "en": "Kata Baku Dari 'Kapok'?", "id": "Jera" },
    { "en": "Kata Baku Dari 'Jerihpayah'?", "id": "Jerih Payah" },
    { "en": "Kata Baku Dari 'Jiji'?", "id": "Jijik" },
    { "en": "Kata Baku Dari 'Jiran'?", "id": "Tetangga" },
    { "en": "Kata Baku Dari 'Jiwit'?", "id": "Cubit" },
    { "en": "Kata Baku Dari 'Jualbeli'?", "id": "Jual Beli" },
    { "en": "Kata Baku Dari 'Judes'?", "id": "Galak" },
    { "en": "Kata Baku Dari 'Ketemu'?", "id": "Bertemu" },
    { "en": "Kata Baku Dari 'Jutek'?", "id": "Galak" },
    { "en": "Kata Baku Dari 'Kaen'?", "id": "Kain" },
    { "en": "Kata Baku Dari 'Kakak Tua'?", "id": "Kakaktua" },
    { "en": "Kata Baku Dari 'Kakilima'?", "id": "Kaki Lima" },
    { "en": "Kata Baku Dari 'Kala Jengking'?", "id": "Kalajengking" },
    { "en": "Kata Baku Dari 'Kalo'?", "id": "Kalau" },
    { "en": "Kata Baku Dari 'Kolbu'?", "id": "Kalbu" },
    { "en": "Kata Baku Dari 'Kameramen'?", "id": "Juru Kamera" },
    { "en": "Kata Baku Dari 'Kampas'?", "id": "Kanvas" },
    { "en": "Kata Baku Dari 'Kampret'?", "id": "Kelelawar Kecil" },
    { "en": "Kata Baku Dari 'Kangen'?", "id": "Rindu" },
    { "en": "Kata Baku Dari 'Kampak'?", "id": "Kapak" },
    { "en": "Kata Baku Dari 'Kapasitet'?", "id": "Kapasitas" },
    { "en": "Kata Baku Dari 'Kapital'?", "id": "Kapital" },
    { "en": "Kata Baku Dari 'Kapling'?", "id": "Kaveling" },
    { "en": "Kata Baku Dari 'Kapsul'?", "id": "Kapsul" },
    { "en": "Kata Baku Dari 'Karakter'?", "id": "Karakter" },
    { "en": "Kata Baku Dari 'Karambol'?", "id": "Karambol" },
    { "en": "Kata Baku Dari 'Karate'?", "id": "Karate" },
    { "en": "Kata Baku Dari 'Karbit'?", "id": "Karbit" },
    { "en": "Kata Baku Dari 'Karbol'?", "id": "Karbol" },
    { "en": "Kata Baku Dari 'Karbon'?", "id": "Karbon" },
    { "en": "Kata Baku Dari 'Kardus'?", "id": "Kardus" },
    { "en": "Kata Baku Dari 'Karier'?", "id": "Karier" },
    { "en": "Kata Baku Dari 'Karies'?", "id": "Karies" },
    { "en": "Kata Baku Dari 'Karikatur'?", "id": "Karikatur" },
    { "en": "Kata Baku Dari 'Karpet'?", "id": "Karpet" },
    { "en": "Kata Baku Dari 'Kartel'?", "id": "Kartel" },
    { "en": "Kata Baku Dari 'Kartu'?", "id": "Kartu" },
    { "en": "Kata Baku Dari 'Karun'?", "id": "Karun" },
    { "en": "Kata Baku Dari 'Karunia'?", "id": "Karunia" },
    { "en": "Kata Baku Dari 'Karya'?", "id": "Karya" },
    { "en": "Kata Baku Dari 'Kasat Mata'?", "id": "Kasa Mata" },
    { "en": "Kata Baku Dari 'Kash'?", "id": "Tunai" },
    { "en": "Kata Baku Dari 'Kasino'?", "id": "Kasino" },
    { "en": "Kata Baku Dari 'Kasur'?", "id": "Kasur" },
    { "en": "Kata Baku Dari 'Katalog'?", "id": "Katalog" },
    { "en": "Kata Baku Dari 'Kate'?", "id": "Kate" },
    { "en": "Kata Baku Dari 'Katedral'?", "id": "Katedral" },
    { "en": "Kata Baku Dari 'Kaya'?", "id": "Kaya" },
    { "en": "Kata Baku Dari 'Kayu'?", "id": "Kayu" },
    { "en": "Kata Baku Dari 'Ke Atas'?", "id": "Keatas" },
    { "en": "Kata Baku Dari 'Ke Bawah'?", "id": "Kebawah" },
    { "en": "Kata Baku Dari 'Kebiri'?", "id": "Kebiri" },
    { "en": "Kata Baku Dari 'Kecam'?", "id": "Kecam" },
    { "en": "Kata Baku Dari 'Kecap'?", "id": "Kecap" },
    { "en": "Kata Baku Dari 'Kecil'?", "id": "Kecil" },
    { "en": "Kata Baku Dari 'Kecu'?", "id": "Kecu" },
    { "en": "Kata Baku Dari 'Kecuali'?", "id": "Kecuali" },
    { "en": "Kata Baku Dari 'Kecup'?", "id": "Kecup" },
    { "en": "Kata Baku Dari 'Kedai'?", "id": "Kedai" },
    { "en": "Kata Baku Dari 'Kedip'?", "id": "Kedip" },
    { "en": "Kata Baku Dari 'Kejar'?", "id": "Kejar" },
    { "en": "Kata Baku Dari 'Keji'?", "id": "Keji" },
    { "en": "Kata Baku Dari 'Kejora'?", "id": "Kejora" },
    { "en": "Kata Baku Dari 'Keju'?", "id": "Keju" },
    { "en": "Kata Baku Dari 'Kejut'?", "id": "Kejut" },
    { "en": "Kata Baku Dari 'Kekal'?", "id": "Kekal" },
    { "en": "Kata Baku Dari 'Kekang'?", "id": "Kekang" },
    { "en": "Kata Baku Dari 'Kekar'?", "id": "Kekar" },
    { "en": "Kata Baku Dari 'Kelahi'?", "id": "Kelahi" },
    { "en": "Kata Baku Dari 'Kelakar'?", "id": "Kelakar" },
    { "en": "Kata Baku Dari 'Kelambu'?", "id": "Kelambu" },
    { "en": "Kata Baku Dari 'Kelamin'?", "id": "Kelamin" },
    { "en": "Kata Baku Dari 'Kelam'?", "id": "Kelam" },
    { "en": "Kata Baku Dari 'Kelana'?", "id": "Kelana" },
    { "en": "Kata Baku Dari 'Kelapa'?", "id": "Kelapa" },
    { "en": "Kata Baku Dari 'Kelar'?", "id": "Kelar" },
    { "en": "Kata Baku Dari 'Kelas'?", "id": "Kelas" },
    { "en": "Kata Baku Dari 'Kelasi'?", "id": "Kelasi" },
    { "en": "Kata Baku Dari 'Kelebet'?", "id": "Kelebet" },
    { "en": "Kata Baku Dari 'Kelelawar'?", "id": "Kelelawar" },
    { "en": "Kata Baku Dari 'Kelelahan'?", "id": "Kelelahan" },
    { "en": "Kata Baku Dari 'Kelereng'?", "id": "Kelereng" },
    { "en": "Kata Baku Dari 'Kelinci'?", "id": "Kelinci" },
    { "en": "Kata Baku Dari 'Kelingking'?", "id": "Kelingking" }



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
