<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BioQuiz - Biology Learning Platform</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap');
        body { font-family: 'Poppins', sans-serif; background-color: #f0fdf4; }
        .grade-card { transition: all 0.3s ease; cursor: pointer; }
        .grade-card:hover { transform: translateY(-5px); box-shadow: 0 10px 15px -3px rgba(34, 197, 94, 0.2); }
        .hidden { display: none; }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-4xl mx-auto">
        <header class="text-center mb-10">
            <div class="inline-block p-3 bg-green-100 rounded-full mb-4">
                <i class="fas fa-dna text-3xl text-green-600"></i>
            </div>
            <h1 class="text-4xl font-bold text-green-800">BioQuiz</h1>
            <p class="text-green-600 italic">Master Biology from Grade 5 to 11</p>
        </header>

        <div id="grade-screen">
            <h2 class="text-xl font-semibold mb-6 text-gray-700 text-center">Select Your Grade</h2>
            <div id="grade-grid" class="grid grid-cols-2 md:grid-cols-4 gap-4">
                </div>
        </div>

        <div id="quiz-screen" class="hidden bg-white p-6 rounded-3xl shadow-2xl border-b-8 border-green-500">
            <div class="flex justify-between items-center mb-8">
                <button onclick="showGrades()" class="text-green-600 font-semibold"><i class="fas fa-chevron-left"></i> Back</button>
                <span id="active-grade-name" class="bg-green-100 text-green-700 px-4 py-1 rounded-full font-bold text-sm"></span>
            </div>
            
            <div id="quiz-content">
                </div>

            <div id="result-screen" class="hidden text-center">
                <i class="fas fa-trophy text-6xl text-yellow-500 mb-4"></i>
                <h3 class="text-2xl font-bold mb-2">Quiz Completed!</h3>
                <p id="score-text" class="text-xl mb-6"></p>
                <button onclick="showGrades()" class="bg-green-600 text-white px-8 py-3 rounded-xl font-bold">Try Another Grade</button>
            </div>
        </div>

        <section class="mt-16 bg-white p-8 rounded-3xl shadow-lg border border-green-100">
            <h2 class="text-2xl font-bold text-green-800 mb-6"><i class="fas fa-pen-nib mr-2"></i> Create Your Own Question</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <select id="input-grade" class="p-4 border border-gray-200 rounded-xl outline-none focus:ring-2 focus:ring-green-400">
                    <option value="">Choose Grade</option>
                    <option value="5">Grade 5</option><option value="6">Grade 6</option>
                    <option value="7">Grade 7</option><option value="8">Grade 8</option>
                    <option value="9">Grade 9</option><option value="10">Grade 10</option>
                    <option value="11">Grade 11</option>
                </select>
                <input type="text" id="input-q" placeholder="Enter Question" class="p-4 border border-gray-200 rounded-xl">
                <input type="text" id="input-correct" placeholder="Correct Answer" class="p-4 border border-green-200 rounded-xl bg-green-50">
                <input type="text" id="input-opt1" placeholder="Wrong Option 1" class="p-4 border border-gray-200 rounded-xl">
                <input type="text" id="input-opt2" placeholder="Wrong Option 2" class="p-4 border border-gray-200 rounded-xl">
                <button onclick="saveUserQuestion()" class="md:col-span-2 bg-green-700 text-white font-bold py-4 rounded-xl hover:bg-green-800 transition">
                    <i class="fas fa-save mr-2"></i> SAVE QUESTION
                </button>
            </div>
        </section>
    </div>

    <script>
        // 1. DATABASE: Initial 10 questions for each grade
        const defaultQuestions = {
            "5": [
                { q: "What is the study of plants called?", a: "Botany", o: ["Zoology", "Botany", "Ecology"] },
                { q: "What part of the plant absorbs water?", a: "Roots", o: ["Leaves", "Flowers", "Roots"] },
                { q: "Which part of the cell is the brain?", a: "Nucleus", o: ["Nucleus", "Cell Wall", "Cytoplasm"] },
                { q: "What gas do plants release?", a: "Oxygen", o: ["Carbon Dioxide", "Oxygen", "Nitrogen"] },
                { q: "Which tool is used to see tiny cells?", a: "Microscope", o: ["Telescope", "Microscope", "Binoculars"] },
                { q: "What is a baby plant inside a seed?", a: "Embryo", o: ["Embryo", "Root", "Stem"] },
                { q: "Plants make food using sunlight in...", a: "Photosynthesis", o: ["Photosynthesis", "Digestion", "Breathing"] },
                { q: "Green color in leaves comes from...", a: "Chlorophyll", o: ["Chlorophyll", "Water", "Iron"] },
                { q: "Which is a flowering plant?", a: "Rose", o: ["Rose", "Moss", "Fern"] },
                { q: "Animals that only eat plants are...", a: "Herbivores", o: ["Carnivores", "Omnivores", "Herbivores"] }
            ],
            // Grades 6-11 can follow the same structure...
            "8": [
                { q: "How many chambers in the human heart?", a: "4", o: ["2", "3", "4"] },
                { q: "What carries oxygen in the blood?", a: "Red Blood Cells", o: ["White Blood Cells", "Plasma", "Red Blood Cells"] },
                { q: "Largest organ of the human body?", a: "Skin", o: ["Liver", "Heart", "Skin"] },
                { q: "How many bones in an adult human?", a: "206", o: ["206", "300", "150"] },
                { q: "Main organ for breathing?", a: "Lungs", o: ["Heart", "Stomach", "Lungs"] },
                { q: "What controls the whole body?", a: "Brain", o: ["Heart", "Brain", "Spinal Cord"] },
                { q: "Which vitamin comes from sunlight?", a: "Vitamin D", o: ["Vitamin A", "Vitamin C", "Vitamin D"] },
                { q: "Blood sugar is controlled by...", a: "Insulin", o: ["Insulin", "Saliva", "Bile"] },
                { q: "Where does digestion start?", a: "Mouth", o: ["Stomach", "Mouth", "Intestine"] },
                { q: "Which is a waste product of breathing?", a: "CO2", o: ["O2", "CO2", "N2"] }
            ]
        };

        // 2. INITIALIZATION
        let currentGradeQuestions = [];
        let score = 0;

        const gradeGrid = document.getElementById('grade-grid');
        for(let i=5; i<=11; i++) {
            gradeGrid.innerHTML += `
                <div onclick="startQuiz('${i}')" class="grade-card bg-white p-6 rounded-2xl shadow-sm text-center border-b-4 border-green-200">
                    <div class="text-2xl font-bold text-green-700">Grade ${i}</div>
                    <div class="text-xs text-gray-500 uppercase mt-2 font-semibold">Biology</div>
                </div>
            `;
        }

        // 3. CORE FUNCTIONS
        function saveUserQuestion() {
            const grade = document.getElementById('input-grade').value;
            const question = document.getElementById('input-q').value;
            const correct = document.getElementById('input-correct').value;
            const opt1 = document.getElementById('input-opt1').value;
            const opt2 = document.getElementById('input-opt2').value;

            if(!grade || !question || !correct) {
                alert("Please fill in the question and correct answer!");
                return;
            }

            // Save to LocalStorage
            let stored = JSON.parse(localStorage.getItem('userBioData')) || {};
            if(!stored[grade]) stored[grade] = [];
            
            stored[grade].push({
                q: question,
                a: correct,
                o: [correct, opt1, opt2].sort(() => Math.random() - 0.5)
            });

            localStorage.setItem('userBioData', JSON.stringify(stored));
            alert("Success! Question saved to Grade " + grade);
            location.reload(); // Refresh to update list
        }

        function startQuiz(grade) {
            document.getElementById('grade-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            document.getElementById('active-grade-name').innerText = "Grade " + grade;

            // Load Default + User Questions
            const userStored = JSON.parse(localStorage.getItem('userBioData')) || {};
            const gradeDefaults = defaultQuestions[grade] || [];
            const gradeUsers = userStored[grade] || [];
            
            currentGradeQuestions = [...gradeDefaults, ...gradeUsers].slice(0, 10); // Max 10
            
            renderQuestions();
        }

        function renderQuestions() {
            const content = document.getElementById('quiz-content');
            score = 0;
            
            if(currentGradeQuestions.length === 0) {
                content.innerHTML = "<p class='text-center py-10'>No questions found for this grade. Please add some below!</p>";
                return;
            }

            content.innerHTML = currentGradeQuestions.map((item, index) => `
                <div class="mb-8 p-4 rounded-xl border border-gray-100 shadow-sm bg-gray-50">
                    <p class="font-bold text-gray-800 text-lg mb-4">${index + 1}. ${item.q}</p>
                    <div class="grid grid-cols-1 gap-3">
                        ${item.o.map(opt => `
                            <button onclick="handleAnswer(this, '${opt}', '${item.a}')" 
                                class="ans-btn text-left p-4 bg-white border-2 border-gray-200 rounded-xl hover:border-green-500 transition font-medium">
                                ${opt}
                            </button>
                        `).join('')}
                    </div>
                </div>
            `).join('') + `<button onclick="finishQuiz()" class="w-full bg-black text-white py-4 rounded-xl font-bold mt-4 shadow-lg">SUBMIT ANSWERS</button>`;
        }

        function handleAnswer(btn, chosen, correct) {
            // Disable other buttons in the same question
            const parent = btn.parentElement;
            const buttons = parent.querySelectorAll('.ans-btn');
            buttons.forEach(b => {
                b.disabled = true;
                b.classList.add('opacity-50');
            });

            if(chosen === correct) {
                btn.classList.replace('border-gray-200', 'border-green-500');
                btn.classList.add('bg-green-100', 'text-green-800', 'opacity-100');
                score++;
            } else {
                btn.classList.replace('border-gray-200', 'border-red-500');
                btn.classList.add('bg-red-100', 'text-red-800', 'opacity-100');
            }
        }

        function finishQuiz() {
            document.getElementById('quiz-content').classList.add('hidden');
            const result = document.getElementById('result-screen');
            result.classList.remove('hidden');
            document.getElementById('score-text').innerText = `You got ${score} out of ${currentGradeQuestions.length} correct!`;
        }

        function showGrades() {
            location.reload();
        }
    </script>
</body>
</html>
