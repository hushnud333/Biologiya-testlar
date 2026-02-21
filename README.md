<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BioQuiz - Maktab Biologiya Testlari</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #f7fee7; }
        .card-green { background: white; border-bottom: 4px solid #16a34a; transition: 0.3s; cursor: pointer; }
        .card-green:hover { transform: translateY(-5px); border-color: #15803d; }
        .hidden { display: none; }
    </style>
</head>
<body class="p-4">

    <div class="max-w-2xl mx-auto">
        <header class="text-center py-8">
            <h1 class="text-4xl font-extrabold text-green-700 italic">BioQuiz</h1>
            <p class="text-green-600 font-medium">Sinfingizni tanlang va bilimingizni sinang</p>
        </header>

        <div id="grade-screen">
            <div id="grade-grid" class="grid grid-cols-2 gap-4">
                </div>
        </div>

        <div id="quiz-screen" class="hidden bg-white p-6 rounded-3xl shadow-xl border border-green-100">
            <div class="flex justify-between items-center mb-6">
                <button onclick="home()" class="text-green-600 font-bold"><i class="fas fa-home"></i> Bosh sahifa</button>
                <span id="active-title" class="bg-green-100 text-green-700 px-3 py-1 rounded-full text-xs font-bold uppercase"></span>
            </div>
            
            <div id="quiz-content"></div>

            <div id="result-box" class="hidden text-center py-10">
                <div class="text-5xl mb-4">🏆</div>
                <h2 class="text-2xl font-bold text-gray-800">Test Yakunlandi!</h2>
                <p id="score-text" class="text-xl text-green-600 font-bold mt-2"></p>
                <button onclick="home()" class="mt-6 bg-green-600 text-white px-10 py-3 rounded-xl font-bold shadow-lg">Qaytadan boshlash</button>
            </div>
        </div>

        <div class="mt-12 bg-white p-6 rounded-3xl shadow-lg border-t-4 border-green-500">
            <h3 class="font-bold text-green-800 text-lg mb-4 text-center"><i class="fas fa-plus"></i> Yangi Savol Qo'shish</h3>
            <div class="space-y-3">
                <select id="in-grade" class="w-full p-3 border rounded-xl outline-none focus:ring-2 focus:ring-green-400">
                    <option value="5">5-sinf</option><option value="6">6-sinf</option>
                    <option value="7">7-sinf</option><option value="8">8-sinf</option>
                    <option value="9">9-sinf</option><option value="10">10-sinf</option>
                    <option value="11">11-sinf</option>
                </select>
                <input type="text" id="in-q" placeholder="Savol matni" class="w-full p-3 border rounded-xl">
                <input type="text" id="in-a" placeholder="To'g'ri javob" class="w-full p-3 border rounded-xl bg-green-50">
                <input type="text" id="in-b" placeholder="Noto'g'ri javob 1" class="w-full p-3 border rounded-xl">
                <input type="text" id="in-c" placeholder="Noto'g'ri javob 2" class="w-full p-3 border rounded-xl">
                <button onclick="saveQuestion()" class="w-full bg-green-700 text-white font-bold py-3 rounded-xl hover:bg-green-800 transition">SAQLASH</button>
            </div>
        </div>
    </div>

    <script>
        const bioData = {
            "5": [
                {q: "Botanika nimani o'rganadi?", a: "O'simliklarni", o: ["Hayvonlarni", "O'simliklarni", "Hujayrani"]},
                {q: "Hujayraning energetik markazi?", a: "Mitoxondriya", o: ["Yadro", "Mitoxondriya", "Xloroplast"]},
                {q: "O'simlikka yashil rang beruvchi pigment?", a: "Xlorofill", o: ["Xlorofill", "Globin", "Karotin"]},
                {q: "Ildizning vazifasi nima?", a: "Suv shimish", o: ["Fotosintez", "Suv shimish", "Meva berish"]},
                {q: "Hujayra devori nimalarda bo'ladi?", a: "O'simliklarda", o: ["Hayvonlarda", "O'simliklarda", "Odamda"]},
                {q: "Mikroskopda ko'rinadigan eng kichik birlik?", a: "Hujayra", o: ["A'zo", "To'qima", "Hujayra"]},
                {q: "Ildiz turlariga nima kirmaydi?", a: "Poya", o: ["Asosiy", "Poya", "Yon"]},
                {q: "Guldasta qismlaridan birini ayting?", a: "Gultojibarg", o: ["Gultojibarg", "Tikan", "Ildizmeva"]},
                {q: "Mevaning necha turi bor?", a: "2", o: ["2", "4", "5"]},
                {q: "Zamburug'lar qanday oziqlanadi?", a: "Tayyor organik", o: ["Fotosintez", "Tayyor organik", "Anorganik"]}
            ],
            "6": [
                {q: "O'simliklar dunyosi necha guruhga bo'linadi?", a: "2", o: ["2", "3", "5"]},
                {q: "Dunyodagi eng katta o'simlik?", a: "Sekvoya", o: ["Eman", "Sekvoya", "Archa"]},
                {q: "Suvo'tlar qayerda yashaydi?", a: "Suvda", o: ["Tuproqda", "Suvda", "Havoda"]},
                {q: "Yopiq urug'li o'simlik nima deyiladi?", a: "Gulli", o: ["Gulli", "Ignabargli", "Sporali"]},
                {q: "Madaniy o'simlikni toping?", a: "Bug'doy", o: ["Shuvoq", "Bug'doy", "Qamish"]},
                {q: "Lola qaysi oilaga kiradi?", a: "Loldoshlar", o: ["Karamdoshlar", "Loldoshlar", "Murakkabguldoshlar"]},
                {q: "Fotosintez qaysi organda bo'ladi?", a: "Barg", o: ["Ildiz", "Barg", "Meva"]},
                {q: "Daraxtning yog'ochlik qismi nima?", a: "Ksilema", o: ["Ksilema", "Floema", "Epidermis"]},
                {q: "Dorivor o'simlik qaysi?", a: "Na'matak", o: ["Yantoq", "Na'matak", "G'o'za"]},
                {q: "O'simliklarning nafas olish organi?", a: "Barcha qismlari", o: ["Faqat barg", "Barcha qismlari", "Faqat ildiz"]}
            ],
            "7": [
                {q: "Zoologiya nimani o'rganadi?", a: "Hayvonlarni", o: ["O'simliklarni", "Hayvonlarni", "Qushlarni"]},
                {q: "Bir hujayrali hayvonni toping?", a: "Amyoba", o: ["Baliq", "Amyoba", "Chigirtka"]},
                {q: "Baliqlar nima orqali nafas oladi?", a: "Jabra", o: ["O'pka", "Jabra", "Teri"]},
                {q: "Sudralib yuruvchilar qaysi?", a: "Ilonlar", o: ["Ilonlar", "Baqa", "Qurbaqa"]},
                {q: "Eng katta hayvon?", a: "Ko'k kit", o: ["Fil", "Ko'k kit", "Sher"]},
                {q: "Hasharotlarning necha oyog'i bor?", a: "6", o: ["4", "6", "8"]},
                {q: "Issiq qonli hayvon qaysi?", a: "Qushlar", o: ["Baliqlar", "Baqa", "Qushlar"]},
                {q: "O'rgimchaksimonlar oyog'i necha juft?", a: "4 juft", o: ["3 juft", "4 juft", "5 juft"]},
                {q: "Yuragi 4 kamerali hayvon?", a: "Timsoh", o: ["Ilon", "Timsoh", "Kaltakesak"]},
                {q: "Asalari necha ko'zli?", a: "5", o: ["2", "3", "5"]}
            ],
            "8": [
                {q: "Odamda nechta qon guruhi bor?", a: "4", o: ["2", "3", "4"]},
                {q: "Yurak bir minutda necha marta uradi?", a: "70-75", o: ["60-65", "70-75", "90-100"]},
                {q: "Eng katta suyak qaysi?", a: "Son suyagi", o: ["Boldir", "Son suyagi", "Qovurg'a"]},
                {q: "O'pka nimani chiqaradi?", a: "CO2", o: ["O2", "CO2", "N2"]},
                {q: "Bosh miya necha qismdan iborat?", a: "5", o: ["3", "4", "5"]},
                {q: "Odamda nechta qovurg'a bor?", a: "12 juft", o: ["10 juft", "12 juft", "14 juft"]},
                {q: "Ko'zning rangdor pardasi?", a: "Kamalak parda", o: ["Gavhar", "To'r parda", "Kamalak parda"]},
                {q: "Qonni quyultiruvchi element?", a: "Trombotsit", o: ["Eritrotsit", "Trombotsit", "Leykotsit"]},
                {q: "Vitamin C yetishmasa nima bo'ladi?", a: "Singa", o: ["Raxit", "Singa", "Anemiya"]},
                {q: "Ovqat hazm qilish qayerda tugaydi?", a: "Yo'g'on ichakda", o: ["Oshqozonda", "Ingichka ichakda", "Yo'g'on ichakda"]}
            ],
            "9": [
                {q: "Genetika asoschisi kim?", a: "G. Mendel", o: ["Darvin", "Mendel", "Lamarck"]},
                {q: "Hujayra nazariyasi mualliflari?", a: "Shleyden va Shvann", o: ["Virxov", "Shleyden va Shvann", "Guk"]},
                {q: "DNK kashfiyotchilari?", a: "Uotson va Krik", o: ["Uotson va Krik", "Morgan", "Mendel"]},
                {q: "Hujayra bo'linishi turlari?", a: "Mitoz va Meyoz", o: ["Sitoz", "Mitoz va Meyoz", "Fagoz"]},
                {q: "Xromosomalar qayerda joylashgan?", a: "Yadroda", o: ["Sitoplazmada", "Yadroda", "Ribosomada"]},
                {q: "Oqsillar sintezi qayerda bo'ladi?", a: "Ribosomada", o: ["Ribosomada", "Lizosomada", "Yadroda"]},
                {q: "Evolyutsiya asoschisi?", a: "Ch. Darvin", o: ["Lamarck", "Ch. Darvin", "Linney"]},
                {q: "Fotosintezning yorug'lik bosqichi qayerda bo'ladi?", a: "Tilaokidda", o: ["Stromada", "Tilaokidda", "Yadroda"]},
                {q: "ATP nima?", a: "Energiya manbai", o: ["Gormon", "Energiya manbai", "Ferment"]},
                {q: "Meyozda hujayra necha marta bo'linadi?", a: "2 marta", o: ["1 marta", "2 marta", "4 marta"]}
            ],
            "10-11": [
                {q: "Antropogenez nima?", a: "Odam rivojlanishi", o: ["O'simlik", "Odam rivojlanishi", "Yer paydo bo'lishi"]},
                {q: "Biogenetik qonun muallifi?", a: "Gekkel va Myuller", o: ["Mendel", "Gekkel va Myuller", "Morgan"]},
                {q: "Populyatsiya nima?", a: "Tur guruhlari", o: ["Yakka organizm", "Tur guruhlari", "Biotsenoz"]},
                {q: "Ekologiya terminini kim kiritgan?", a: "E. Gekkel", o: ["Darvin", "E. Gekkel", "Lamarck"]},
                {q: "Moddalar almashinuvi?", a: "Metabolizm", o: ["Katabolizm", "Metabolizm", "Anabolizm"]},
                {q: "Gomeostaz nima?", a: "Doimiylik", o: ["O'zgaruvchanlik", "Doimiylik", "O'sish"]},
                {q: "Transkripsiya qayerda bo'ladi?", a: "Yadroda", o: ["Ribosomada", "Yadroda", "Sitoplazmada"]},
                {q: "Gidrosfera nima?", a: "Suv qobig'i", o: ["Tosh", "Suv qobig'i", "Havo"]},
                {q: "Mutatsiya nima?", a: "Irsiy o'zgarish", o: ["O'sish", "Irsiy o'zgarish", "Nafas olish"]},
                {q: "Biosferaning tirik moddasi?", a: "Barcha organizmlar", o: ["Suv", "Barcha organizmlar", "Tog'lar"]}
            ]
        };

        const grid = document.getElementById('grade-grid');
        ["5", "6", "7", "8", "9", "10-11"].forEach(grade => {
            grid.innerHTML += `
                <div onclick="startQuiz('${grade}')" class="card-green p-6 rounded-2xl text-center shadow-sm">
                    <div class="text-2xl font-bold text-green-700">${grade}-sinf</div>
                    <div class="text-xs text-gray-400 mt-1 uppercase">Biologiya</div>
                </div>
            `;
        });

        function saveQuestion() {
            const grade = document.getElementById('in-grade').value;
            const q = document.getElementById('in-q').value;
            const a = document.getElementById('in-a').value;
            const b = document.getElementById('in-b').value;
            const c = document.getElementById('in-c').value;

            if(!q || !a) return alert("Savol va javobni to'ldiring!");

            let localDB = JSON.parse(localStorage.getItem('myBioDB')) || {};
            if(!localDB[grade]) localDB[grade] = [];
            
            localDB[grade].push({q, a, o: [a, b, c].sort(() => Math.random() - 0.5)});
            localStorage.setItem('myBioDB', JSON.stringify(localDB));
            
            alert("Saqlandi! Endi " + grade + "-sinf testida bu savol chiqadi.");
            location.reload();
        }

        let score = 0;
        let total = 0;

        function startQuiz(grade) {
            document.getElementById('grade-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            document.getElementById('active-title').innerText = grade + "-sinf Testi";
            
            const localDB = JSON.parse(localStorage.getItem('myBioDB')) || {};
            const qList = [...(bioData[grade] || []), ...(localDB[grade] || [])].slice(0, 10);
            
            total = qList.length;
            score = 0;
            const content = document.getElementById('quiz-content');
            content.classList.remove('hidden');
            document.getElementById('result-box').classList.add('hidden');

            content.innerHTML = qList.map((item, i) => `
                <div class="mb-8 p-4 bg-green-50 rounded-xl">
                    <p class="font-bold text-gray-800 mb-4">${i+1}. ${item.q}</p>
                    <div class="space-y-2">
                        ${item.o.map(opt => `
                            <button onclick="check(this, '${opt}', '${item.a}')" class="q-btn w-full text-left p-3 bg-white border rounded-lg hover:border-green-500 transition">
                                ${opt}
                            </button>
                        `).join('')}
                    </div>
                </div>
            `).join('') + `<button onclick="finish()" class="w-full bg-black text-white py-4 rounded-xl font-bold shadow-lg">NATIJANI KO'RISH</button>`;
        }

        function check(btn, val, correct) {
            const btns = btn.parentElement.querySelectorAll('.q-btn');
            btns.forEach(b => b.disabled = true);
            
            if(val === correct) {
                btn.classList.add('bg-green-500', 'text-white');
                score++;
            } else {
                btn.classList.add('bg-red-500', 'text-white');
            }
        }

        function finish() {
            document.getElementById('quiz-content').classList.add('hidden');
            document.getElementById('result-box').classList.remove('hidden');
            document.getElementById('score-text').innerText = `${total} tadan ${score} ta to'g'ri!`;
        }

        function home() {
            location.reload();
        }
    </script>
</body>
</html>
