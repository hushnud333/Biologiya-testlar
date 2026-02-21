<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BioQuiz - Biologiya Testlari</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        .grade-card { transition: 0.3s; cursor: pointer; border: 2px solid transparent; }
        .grade-card:hover { border-color: #22c55e; transform: scale(1.02); }
        body { background-color: #f0fdf4; }
    </style>
</head>
<body class="p-4 md:p-8">
    <div class="max-w-4xl mx-auto">
        <header class="text-center mb-10">
            <h1 class="text-4xl font-bold text-green-700 mb-2 italic">BioQuiz</h1>
            <p class="text-green-600 font-medium">Sinfingizni tanlang va bilimingizni sinang</p>
        </header>

        <div id="grade-selection" class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-10">
            </div>

        <div id="quiz-container" class="hidden bg-white p-6 rounded-2xl shadow-xl border-t-4 border-green-500">
            <div class="flex justify-between items-center mb-6">
                <button onclick="goBack()" class="text-green-600 font-bold"><i class="fas fa-arrow-left"></i> Orqaga</button>
                <span id="grade-title" class="text-lg font-bold text-gray-700"></span>
            </div>
            <div id="question-box">
                </div>
        </div>

        <section class="mt-12 bg-white p-6 rounded-2xl shadow-lg">
            <h2 class="text-xl font-bold text-green-700 mb-4 text-center"><i class="fas fa-plus-circle"></i> Yangi savol tuzish</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <select id="new-grade" class="p-3 border rounded-lg outline-none focus:ring-2 focus:ring-green-400">
                    <option value="">Sinfni tanlang</option>
                    <option value="5">5-sinf</option><option value="6">6-sinf</option>
                    <option value="7">7-sinf</option><option value="8">8-sinf</option>
                    <option value="9">9-sinf</option><option value="10">10-sinf</option>
                    <option value="11">11-sinf</option>
                </select>
                <input type="text" id="new-q" placeholder="Savol matni" class="p-3 border rounded-lg">
                <input type="text" id="new-a" placeholder="To'g'ri javob" class="p-3 border rounded-lg bg-green-50">
                <input type="text" id="new-b" placeholder="Noto'g'ri variant 1" class="p-3 border rounded-lg">
                <input type="text" id="new-c" placeholder="Noto'g'ri variant 2" class="p-3 border rounded-lg">
                <button onclick="addCustomQuestion()" class="md:col-span-2 bg-green-600 text-white font-bold py-3 rounded-lg hover:bg-green-700">SAQLASH</button>
            </div>
        </section>
    </div>
    <script src="script.js"></script>
</body>
</html>

