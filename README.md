<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Milliy Sertifikat - Javoblar Varaqasi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        body { background-color: #f8fafc; font-family: 'Inter', sans-serif; -webkit-tap-highlight-color: transparent; }
        .answer-row { display: flex; align-items: center; justify-content: space-between; background: white; padding: 10px 15px; border-radius: 12px; margin-bottom: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.02); }
        .opt-btn { width: 40px; height: 40px; border: 2px solid #e2e8f0; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; color: #64748b; transition: 0.2s; }
        .opt-btn.selected { background-color: #10b981; border-color: #10b981; color: white; transform: scale(1.1); }
        .sticky-header { position: sticky; top: 0; z-index: 50; background: rgba(248, 250, 252, 0.9); backdrop-filter: blur(10px); }
    </style>
</head>
<body class="pb-24">

    <div class="max-w-md mx-auto p-4">
        <div class="sticky-header py-4 border-b mb-6">
            <div class="flex justify-between items-center">
                <h1 class="text-xl font-extrabold text-slate-800">BIOLOGIYA</h1>
                <span class="bg-blue-100 text-blue-700 px-3 py-1 rounded-full text-xs font-bold">35 TALIK TEST</span>
            </div>
            <p class="text-xs text-slate-500 mt-1">Javoblarni tanlang va yakunlash tugmasini bosing</p>
        </div>

        <div id="answer-sheet">
            </div>

        <div class="fixed bottom-0 left-0 right-0 p-4 bg-white border-t shadow-lg flex gap-3">
            <button onclick="resetAll()" class="w-1/3 bg-slate-100 text-slate-600 py-4 rounded-2xl font-bold">TOZALASH</button>
            <button onclick="submitTest()" class="w-2/3 bg-green-600 text-white py-4 rounded-2xl font-bold shadow-green-200 shadow-xl">YAKUNLASH</button>
        </div>
    </div>

    <script>
        const sheet = document.getElementById('answer-sheet');
        let results = {};

        // 35 ta savol qatorini yaratish
        for (let i = 1; i <= 35; i++) {
            sheet.innerHTML += `
                <div class="answer-row">
                    <span class="w-8 font-bold text-slate-400">${i}.</span>
                    <div class="flex gap-3">
                        ${['A', 'B', 'C', 'D'].map(opt => `
                            <div onclick="selectOpt(${i}, '${opt}')" id="btn-${i}-${opt}" class="opt-btn">${opt}</div>
                        `).join('')}
                    </div>
                </div>
            `;
        }

        function selectOpt(qNum, opt) {
            // Avvalgi tanlovni o'chirish
            ['A', 'B', 'C', 'D'].forEach(o => {
                document.getElementById(`btn-${qNum}-${o}`).classList.remove('selected');
            });
            
            // Yangisini tanlash
            document.getElementById(`btn-${qNum}-${opt}`).classList.add('selected');
            results[qNum] = opt;
            
            // Haptic feedback (vibro) agar qo'llab quvvatlasa
            if (window.navigator.vibrate) window.navigator.vibrate(10);
        }

        function resetAll() {
            if(confirm("Barcha javoblarni o'chirmoqchimisiz?")) {
                location.reload();
            }
        }

        function submitTest() {
            const count = Object.keys(results).length;
            if (count < 35) {
                if(!confirm(`Siz hali 35 ta savoldan faqat ${count} tasini belgiladingiz. Davom etamizmi?`)) return;
            }

            // Natijalarni Telegramga yuborish
            // Agar Telegram Mini App ichida bo'lsa:
            if (window.Telegram && window.Telegram.WebApp) {
                window.Telegram.WebApp.sendData(JSON.stringify(results));
            } else {
                alert("Test yakunlandi! Natijalaringiz qabul qilindi.");
                console.log("Results:", results);
            }
        }

        // Telegram WebApp interfeysini sozlash
        if (window.Telegram && window.Telegram.WebApp) {
            window.Telegram.WebApp.ready();
            window.Telegram.WebApp.expand();
        }
    </script>
</body>
</html>
