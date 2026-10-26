<!DOCTYPE html>
<html lang="mn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Пайтон хэлний нөхцөлт шалгалт ба логик холбоос - Тест</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Fira+Code:wght@400;600&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                        mono: ['Fira Code', 'monospace'],
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col font-sans antialiased selection:bg-blue-500 selection:text-white">

    <!-- Header -->
    <header class="border-b border-slate-800 bg-slate-900/80 backdrop-blur sticky top-0 z-50">
        <div class="max-w-4xl mx-auto px-4 py-4 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-blue-600/20 border border-blue-500/30 flex items-center justify-center text-blue-400">
                    <i class="fa-brands fa-python text-2xl"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg text-white">Пайтон Програмчлал</h1>
                    <p class="text-xs text-slate-400">Харьцуулах ба Логик оператор - Онлайн Тест</p>
                </div>
            </div>
            <div id="userInfoBadge" class="hidden text-right text-xs text-slate-300 bg-slate-800 border border-slate-700 px-3 py-1.5 rounded-lg">
                <span id="badgeName" class="font-bold text-blue-400"></span> (<span id="badgeClass"></span>)
            </div>
        </div>
    </header>

    <main class="flex-grow flex items-center justify-center p-4">
        <div class="max-w-3xl w-full">

            <!-- STEP 1: LOGIN / START SCREEN -->
            <div id="loginScreen" class="bg-slate-800/80 border border-slate-700/80 rounded-2xl p-6 sm:p-10 shadow-2xl backdrop-blur-sm">
                <div class="text-center max-w-md mx-auto mb-8">
                    <div class="inline-flex p-3 rounded-2xl bg-blue-500/10 text-blue-400 mb-4 border border-blue-500/20">
                        <i class="fa-solid fa-graduation-cap text-3xl"></i>
                    </div>
                    <h2 class="text-2xl font-extrabold text-white mb-2">Мэдлэгээ шалгацгаая!</h2>
                    <p class="text-slate-400 text-sm">Нөхцөлт шалгалт, харьцуулах болон логик операторуудын сэдвээр 10 асуултад хариулан мэдлэгээ сориорой.</p>
                </div>

                <form id="startForm" onsubmit="startQuiz(event)" class="max-w-md mx-auto space-y-4">
                    <div>
                        <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-2">Сурагчийн Нэр</label>
                        <div class="relative">
                            <i class="fa-solid fa-user absolute left-4 top-1/2 -translate-y-1/2 text-slate-500"></i>
                            <input type="text" id="studentName" required placeholder="Ж: Бат-Эрдэнэ" class="w-full bg-slate-900/90 border border-slate-700 rounded-xl pl-11 pr-4 py-3 text-white placeholder-slate-500 focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 transition">
                        </div>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-2">Анги, Бүлэг</label>
                        <div class="relative">
                            <i class="fa-solid fa-users absolute left-4 top-1/2 -translate-y-1/2 text-slate-500"></i>
                            <input type="text" id="studentClass" required placeholder="Ж: 10А" class="w-full bg-slate-900/90 border border-slate-700 rounded-xl pl-11 pr-4 py-3 text-white placeholder-slate-500 focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 transition">
                        </div>
                    </div>

                    <div class="bg-blue-950/40 border border-blue-800/40 rounded-xl p-4 text-xs text-blue-200/80 space-y-1">
                        <p class="font-semibold text-blue-400 mb-1"><i class="fa-solid fa-circle-info mr-1"></i> Санамж:</p>
                        <p>• Нийт 10 асуулттай.</p>
                        <p>• Асуулт бүр 1 зөв хариулттай.</p>
                        <p>• Тест дуусахад үр дүн шууд харагдана.</p>
                    </div>

                    <button type="submit" class="w-full bg-blue-600 hover:bg-blue-500 text-white font-bold py-3.5 px-6 rounded-xl shadow-lg shadow-blue-600/30 transition duration-200 flex items-center justify-center space-x-2">
                        <span>Тест эхлүүлэх</span>
                        <i class="fa-solid fa-arrow-right"></i>
                    </button>
                </form>
            </div>

            <!-- STEP 2: QUIZ SCREEN -->
            <div id="quizScreen" class="hidden space-y-6">
                <!-- Progress & Stats -->
                <div class="bg-slate-800/80 border border-slate-700/80 rounded-xl p-4 flex flex-col sm:flex-row items-center justify-between gap-4">
                    <div class="w-full sm:w-2/3">
                        <div class="flex justify-between text-xs font-semibold mb-1">
                            <span class="text-slate-400">Явц</span>
                            <span class="text-blue-400" id="progressText">1 / 10</span>
                        </div>
                        <div class="w-full bg-slate-900 rounded-full h-2.5 overflow-hidden">
                            <div id="progressBar" class="bg-blue-500 h-2.5 rounded-full transition-all duration-300" style="width: 10%"></div>
                        </div>
                    </div>
                    <div class="flex items-center space-x-2 text-xs font-mono bg-slate-900/80 px-3 py-2 rounded-lg border border-slate-700/50">
                        <i class="fa-regular fa-clock text-blue-400"></i>
                        <span id="timerText" class="text-slate-200">00:00</span>
                    </div>
                </div>

                <!-- Question Card -->
                <div class="bg-slate-800/80 border border-slate-700/80 rounded-2xl p-6 sm:p-8 shadow-2xl relative overflow-hidden">
                    <span id="questionTag" class="inline-block px-3 py-1 rounded-md bg-blue-500/10 text-blue-400 border border-blue-500/20 text-xs font-semibold mb-4">Асуулт 1</span>
                    <h3 id="questionText" class="text-lg sm:text-xl font-bold text-white mb-6 leading-relaxed"></h3>
                    
                    <!-- Code snippet block (optional, rendered dynamically) -->
                    <div id="codeSnippetContainer" class="hidden mb-6">
                        <pre class="bg-slate-950 border border-slate-800 rounded-xl p-4 font-mono text-sm text-emerald-400 overflow-x-auto"><code id="codeSnippet"></code></pre>
                    </div>

                    <!-- Options -->
                    <div id="optionsContainer" class="space-y-3"></div>
                </div>

                <!-- Navigation Buttons -->
                <div class="flex justify-between items-center pt-2">
                    <button id="prevBtn" onclick="prevQuestion()" class="px-5 py-2.5 rounded-xl border border-slate-700 text-slate-300 font-semibold text-sm hover:bg-slate-800 disabled:opacity-30 disabled:cursor-not-allowed transition">
                        <i class="fa-solid fa-chevron-left mr-2"></i> Өмнөх
                    </button>
                    <button id="nextBtn" onclick="nextQuestion()" class="px-6 py-2.5 rounded-xl bg-blue-600 text-white font-semibold text-sm hover:bg-blue-500 shadow-lg shadow-blue-600/20 transition">
                        Дараах <i class="fa-solid fa-chevron-right ml-2"></i>
                    </button>
                    <button id="submitBtn" onclick="finishQuiz()" class="hidden px-6 py-2.5 rounded-xl bg-emerald-600 text-white font-semibold text-sm hover:bg-emerald-500 shadow-lg shadow-emerald-600/20 transition">
                        Дуусгах ба Дүн харах <i class="fa-solid fa-check ml-2"></i>
                    </button>
                </div>
            </div>

            <!-- STEP 3: RESULT SCREEN -->
            <div id="resultScreen" class="hidden space-y-6">
                <div class="bg-slate-800/80 border border-slate-700/80 rounded-2xl p-6 sm:p-10 text-center shadow-2xl">
                    <div id="scoreBadge" class="w-24 h-24 mx-auto rounded-full flex items-center justify-center text-4xl mb-4 border-4">
                        <i id="scoreIcon" class="fa-solid"></i>
                    </div>

                    <h2 class="text-2xl font-black text-white mb-1">Тест Амжилттай Дууслаа!</h2>
                    <p class="text-slate-400 text-sm mb-6"><span id="resStudentName" class="text-white font-semibold"></span> сурагч (<span id="resStudentClass" class="text-blue-400"></span>)</p>

                    <div class="grid grid-cols-2 sm:grid-cols-3 gap-4 max-w-lg mx-auto mb-8">
                        <div class="bg-slate-900/80 border border-slate-700/50 p-4 rounded-xl">
                            <div class="text-xs text-slate-400 mb-1">Оноо</div>
                            <div id="scoreText" class="text-2xl font-extrabold text-blue-400">0 / 10</div>
                        </div>
                        <div class="bg-slate-900/80 border border-slate-700/50 p-4 rounded-xl">
                            <div class="text-xs text-slate-400 mb-1">Амжилт</div>
                            <div id="percentageText" class="text-2xl font-extrabold text-emerald-400">0%</div>
                        </div>
                        <div class="col-span-2 sm:col-span-1 bg-slate-900/80 border border-slate-700/50 p-4 rounded-xl">
                            <div class="text-xs text-slate-400 mb-1">Зарцуулсан цаг</div>
                            <div id="timeSpentText" class="text-2xl font-extrabold text-purple-400 font-mono">00:00</div>
                        </div>
                    </div>

                    <div class="flex flex-col sm:flex-row justify-center gap-4">
                        <button onclick="toggleReview()" class="px-6 py-3 rounded-xl border border-slate-700 bg-slate-800 hover:bg-slate-700 text-white font-semibold text-sm transition">
                            <i class="fa-solid fa-list-check mr-2 text-blue-400"></i> Хариултаа шалгах
                        </button>
                        <button onclick="restartQuiz()" class="px-6 py-3 rounded-xl bg-blue-600 hover:bg-blue-500 text-white font-semibold text-sm shadow-lg shadow-blue-600/30 transition">
                            <i class="fa-solid fa-rotate-right mr-2"></i> Дахин өгөх
                        </button>
                    </div>
                </div>

                <!-- Detailed Answer Review -->
                <div id="reviewContainer" class="hidden space-y-4">
                    <h3 class="text-lg font-bold text-white mb-2 flex items-center">
                        <i class="fa-solid fa-square-poll-vertical mr-2 text-blue-400"></i> Дэлгэрэнгүй тайлан
                    </h3>
                    <div id="reviewList" class="space-y-4"></div>
                </div>
            </div>

        </div>
    </main>

    <script>
        // 10 Python Conditions & Logical Operators Questions
        const quizQuestions = [
            {
                id: 1,
                question: "Пайтон хэлэнд хоёр утга 'тэнцүү эсэхийг' шалгахдаа ямар оператор ашигладаг вэ?",
                code: null,
                options: [
                    "=",
                    "==",
                    "===",
                    "is equal"
                ],
                correct: 1,
                explanation: "Пайтонд '=' нь утга олгох оператор бөгөөд харин '==' нь хоёр утга тэнцүү эсэхийг шалгах харьцуулах оператор юм."
            },
            {
                id: 2,
                question: "Дараах кодын үр дүн ямар байх вэ?",
                code: "x = 10\ny = 15\nprint(x >= y)",
                options: [
                    "True",
                    "False",
                    "SyntaxError",
                    "10 >= 15"
                ],
                correct: 1,
                explanation: "10 нь 15-аас их эсвэл тэнцүү биш тул харьцуулалтын үр дүнд Булеан (Boolean) 'False' утга буцна."
            },
            {
                id: 3,
                question: "Логик 'and' операторын хувьд аль мэдэгдэл нь ЗӨВ бэ?",
                code: null,
                options: [
                    "Аль нэг нөхцөл биелж байвал True гарна.",
                    "Холбогдсон БҮХ нөхцөл True байж байж үр дүн True гарна.",
                    "Нөхцөлийг эсрэгээр нь эргүүлнэ.",
                    "Бүх нөхцөл False байх үед л True гарна."
                ],
                correct: 1,
                explanation: "'and' (Логик БА) оператор нь холбогдсон бүх нөхцөлүүд үнэн (True) байх тохиолдолд л True буцаана."
            },
            {
                id: 4,
                question: "Дараах илэрхийллийн бодолтын үр дүн юу гарах вэ?",
                code: "print((5 > 3) or (10 == 20))",
                options: [
                    "False",
                    "True",
                    "None",
                    "Error"
                ],
                correct: 1,
                explanation: "(5 > 3) нь True, (10 == 20) нь False байна. 'or' оператор нь аль нэг нөхцөл л True бол үр дүнг True болгодог."
            },
            {
                id: 5,
                question: "Логик 'not' операторын үүрэг юу вэ?",
                code: "print(not (7 > 2))",
                options: [
                    "True утга буцаана.",
                    "Нөхцөлийн логик үр дүнг эсрэгээр нь эргүүлнэ (True-г False болгоно).",
                    "Хоёр утгыг хооронд нь нэмнэ.",
                    "Тоог сөрөг тэмдэгтэй болгоно."
                ],
                correct: 1,
                explanation: "(7 > 2) нь True бөгөөд 'not True' нь False утгыг буцаана. 'not' оператор нь үр дүнг эсрэгээр нь эргүүлдэг."
            },
            {
                id: 6,
                question: "Пайтон хэлэнд 'тэнцүү биш' гэсэн харьцуулах операторыг хэрхэн бичдэг вэ?",
                code: null,
                options: [
                    "<>",
                    "not=",
                    "!=",
                    "=/="
                ],
                correct: 2,
                explanation: "Пайтонд тэнцүү биш гэдгийг '!=' оператороор шалгадаг."
            },
            {
                id: 7,
                question: "Тэмдэгт мөр эсвэл жагсаалт дотор тухайн утга агуулагдаж байгааг шалгах ямар оператор байдаг вэ?",
                code: "text = 'Python'\nprint('P' in text)",
                options: [
                    "has",
                    "contains",
                    "in",
                    "find"
                ],
                correct: 2,
                explanation: "'in' болон 'not in' нь Гишүүн оператор (Membership operator) бөгөөд жагсаалт болон тэмдэгт мөрөөс хайлт хийхэд ашиглагддаг."
            },
            {
                id: 8,
                question: "Дараах кодын гаралт юу байх вэ?",
                code: "a = 5\nb = 10\nc = 15\nprint((a < b) and (b < c))",
                options: [
                    "True",
                    "False",
                    "5 < 10 < 15",
                    "Error"
                ],
                correct: 0,
                explanation: "(5 < 10) нь True, (10 < 15) нь True байна. Хоёр нөхцөл хоёулаа True тул 'and' холбоосын үр дүн True гарна."
            },
            {
                id: 9,
                question: "Логик операторуудын биелэх дараалал (Precedence) ямар байдаг вэ?",
                code: null,
                options: [
                    "Эхлээд or, дараа нь and, төгсгөлд нь not",
                    "Эхлээд and, дараа нь or, төгсгөлд нь not",
                    "Эхлээд not, дараа нь and, төгсгөлд нь or",
                    "Бүх оператор ижил эрэмбэтэй"
                ],
                correct: 2,
                explanation: "Логик операторуудын дараалал нь 1-рт 'not', 2-рт 'and', 3-рт 'or' байдаг."
            },
            {
                id: 10,
                question: "Дараах кодын үр дүн юу гарах вэ?",
                code: "lang = 'Python'\nprint('java' not in lang)",
                options: [
                    "True",
                    "False",
                    "SyntaxError",
                    "None"
                ],
                correct: 0,
                explanation: "'java' тэмдэгт мөр 'Python' гэдэг үгэн дотор байхгүй нь ҮНЭН тул 'not in' оператор True буцаана."
            }
        ];

        // App State Variables
        let currentQuestionIndex = 0;
        let userAnswers = new Array(quizQuestions.length).fill(null);
        let studentName = "";
        let studentClass = "";
        let timerSeconds = 0;
        let timerInterval = null;

        // START QUIZ
        function startQuiz(event) {
            event.preventDefault();
            studentName = document.getElementById('studentName').value.trim();
            studentClass = document.getElementById('studentClass').value.trim();

            if (!studentName || !studentClass) return;

            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('quizScreen').classList.remove('hidden');

            document.getElementById('badgeName').innerText = studentName;
            document.getElementById('badgeClass').innerText = studentClass;
            document.getElementById('userInfoBadge').classList.remove('hidden');

            startTimer();
            renderQuestion();
        }

        // TIMER
        function startTimer() {
            timerInterval = setInterval(() => {
                timerSeconds++;
                const mins = Math.floor(timerSeconds / 60).toString().padStart(2, '0');
                const secs = (timerSeconds % 60).toString().padStart(2, '0');
                document.getElementById('timerText').innerText = `${mins}:${secs}`;
            }, 1000);
        }

        function stopTimer() {
            clearInterval(timerInterval);
        }

        // RENDER QUESTION
        function renderQuestion() {
            const q = quizQuestions[currentQuestionIndex];

            // Progress Update
            document.getElementById('questionTag').innerText = `Асуулт ${currentQuestionIndex + 1} / ${quizQuestions.length}`;
            document.getElementById('progressText').innerText = `${currentQuestionIndex + 1} / ${quizQuestions.length}`;
            const pct = ((currentQuestionIndex + 1) / quizQuestions.length) * 100;
            document.getElementById('progressBar').style.width = `${pct}%`;

            // Question & Code Snippet
            document.getElementById('questionText').innerText = q.question;

            const codeContainer = document.getElementById('codeSnippetContainer');
            const codeSnippet = document.getElementById('codeSnippet');
            if (q.code) {
                codeSnippet.innerText = q.code;
                codeContainer.classList.remove('hidden');
            } else {
                codeContainer.classList.add('hidden');
            }

            // Options
            const optionsContainer = document.getElementById('optionsContainer');
            optionsContainer.innerHTML = '';

            q.options.forEach((opt, idx) => {
                const isSelected = userAnswers[currentQuestionIndex] === idx;
                const optBtn = document.createElement('button');
                optBtn.type = 'button';
                optBtn.onclick = () => selectOption(idx);
                optBtn.className = `w-full text-left p-4 rounded-xl border font-semibold text-sm transition flex items-center justify-between ${
                    isSelected 
                        ? 'bg-blue-600/20 border-blue-500 text-white shadow-lg ring-1 ring-blue-500' 
                        : 'bg-slate-900/60 border-slate-700/80 text-slate-300 hover:bg-slate-800 hover:border-slate-600'
                }`;

                optBtn.innerHTML = `
                    <div class="flex items-center space-x-3">
                        <span class="w-7 h-7 rounded-lg flex items-center justify-center text-xs font-bold ${
                            isSelected ? 'bg-blue-500 text-white' : 'bg-slate-800 text-slate-400'
                        }">
                            ${String.fromCharCode(65 + idx)}
                        </span>
                        <span>${escapeHtml(opt)}</span>
                    </div>
                    ${isSelected ? '<i class="fa-solid fa-circle-check text-blue-400 text-lg"></i>' : ''}
                `;
                optionsContainer.appendChild(optBtn);
            });

            // Buttons state
            document.getElementById('prevBtn').disabled = currentQuestionIndex === 0;

            if (currentQuestionIndex === quizQuestions.length - 1) {
                document.getElementById('nextBtn').classList.add('hidden');
                document.getElementById('submitBtn').classList.remove('hidden');
            } else {
                document.getElementById('nextBtn').classList.remove('hidden');
                document.getElementById('submitBtn').classList.add('hidden');
            }
        }

        function selectOption(index) {
            userAnswers[currentQuestionIndex] = index;
            renderQuestion();
        }

        function prevQuestion() {
            if (currentQuestionIndex > 0) {
                currentQuestionIndex--;
                renderQuestion();
            }
        }

        function nextQuestion() {
            if (currentQuestionIndex < quizQuestions.length - 1) {
                currentQuestionIndex++;
                renderQuestion();
            }
        }

        // FINISH & CALCULATE
        function finishQuiz() {
            // Check if any unanswered
            const unanswered = userAnswers.filter(a => a === null).length;
            if (unanswered > 0) {
                if (!confirm(`Танд хариулаагүй ${unanswered} асуулт байна. Тестийг дуусгах уу?`)) {
                    return;
                }
            }

            stopTimer();

            // Calculate Score
            let correctCount = 0;
            quizQuestions.forEach((q, idx) => {
                if (userAnswers[idx] === q.correct) {
                    correctCount++;
                }
            });

            const scorePct = Math.round((correctCount / quizQuestions.length) * 100);

            // Hide Quiz, Show Results
            document.getElementById('quizScreen').classList.add('hidden');
            document.getElementById('resultScreen').classList.remove('hidden');

            document.getElementById('resStudentName').innerText = studentName;
            document.getElementById('resStudentClass').innerText = studentClass;
            document.getElementById('scoreText').innerText = `${correctCount} / ${quizQuestions.length}`;
            document.getElementById('percentageText').innerText = `${scorePct}%`;

            const mins = Math.floor(timerSeconds / 60).toString().padStart(2, '0');
            const secs = (timerSeconds % 60).toString().padStart(2, '0');
            document.getElementById('timeSpentText').innerText = `${mins}:${secs}`;

            // Score Badge styling
            const badge = document.getElementById('scoreBadge');
            const icon = document.getElementById('scoreIcon');

            if (scorePct >= 80) {
                badge.className = "w-24 h-24 mx-auto rounded-full flex items-center justify-center text-4xl mb-4 border-4 bg-emerald-500/10 border-emerald-500 text-emerald-400";
                icon.className = "fa-solid fa-trophy";
            } else if (scorePct >= 50) {
                badge.className = "w-24 h-24 mx-auto rounded-full flex items-center justify-center text-4xl mb-4 border-4 bg-blue-500/10 border-blue-500 text-blue-400";
                icon.className = "fa-solid fa-thumbs-up";
            } else {
                badge.className = "w-24 h-24 mx-auto rounded-full flex items-center justify-center text-4xl mb-4 border-4 bg-amber-500/10 border-amber-500 text-amber-400";
                icon.className = "fa-solid fa-book-open";
            }

            buildReviewList();
        }

        // BUILD REVIEW
        function buildReviewList() {
            const reviewList = document.getElementById('reviewList');
            reviewList.innerHTML = '';

            quizQuestions.forEach((q, idx) => {
                const userAns = userAnswers[idx];
                const isCorrect = userAns === q.correct;

                const card = document.createElement('div');
                card.className = `p-5 rounded-2xl border bg-slate-800/80 space-y-3 ${
                    isCorrect ? 'border-emerald-500/40' : 'border-rose-500/40'
                }`;

                let optionsHtml = '';
                q.options.forEach((opt, optIdx) => {
                    let optStyle = "text-slate-400 bg-slate-900/40 border-slate-800";
                    let iconHtml = '';

                    if (optIdx === q.correct) {
                        optStyle = "text-emerald-300 bg-emerald-950/40 border-emerald-500/50 font-semibold";
                        iconHtml = '<i class="fa-solid fa-check text-emerald-400"></i>';
                    } else if (optIdx === userAns && !isCorrect) {
                        optStyle = "text-rose-300 bg-rose-950/40 border-rose-500/50 font-semibold";
                        iconHtml = '<i class="fa-solid fa-xmark text-rose-400"></i>';
                    }

                    optionsHtml += `
                        <div class="p-3 rounded-lg border text-xs sm:text-sm flex items-center justify-between ${optStyle}">
                            <span><b>${String.fromCharCode(65 + optIdx)}.</b> ${escapeHtml(opt)}</span>
                            ${iconHtml}
                        </div>
                    `;
                });

                card.innerHTML = `
                    <div class="flex items-start justify-between gap-2">
                        <div class="font-bold text-sm sm:text-base text-white">
                            <span class="text-blue-400 mr-1">${idx + 1}.</span> ${q.question}
                        </div>
                        <span class="px-2.5 py-1 rounded-md text-xs font-bold shrink-0 ${
                            isCorrect ? 'bg-emerald-500/10 text-emerald-400 border border-emerald-500/30' : 'bg-rose-500/10 text-rose-400 border border-rose-500/30'
                        }">
                            ${isCorrect ? 'Зөв' : 'Буруу'}
                        </span>
                    </div>

                    ${q.code ? `<pre class="bg-slate-950 border border-slate-800 rounded-lg p-3 font-mono text-xs text-emerald-400 overflow-x-auto"><code>${q.code}</code></pre>` : ''}

                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-2 pt-1">
                        ${optionsHtml}
                    </div>

                    <div class="bg-slate-900/90 border border-slate-700/50 rounded-xl p-3 text-xs text-slate-300 mt-2">
                        <span class="font-bold text-blue-400"><i class="fa-solid fa-lightbulb mr-1"></i> Тайлбар:</span> ${q.explanation}
                    </div>
                `;

                reviewList.appendChild(card);
            });
        }

        function toggleReview() {
            const container = document.getElementById('reviewContainer');
            container.classList.toggle('hidden');
        }

        function restartQuiz() {
            currentQuestionIndex = 0;
            userAnswers = new Array(quizQuestions.length).fill(null);
            timerSeconds = 0;

            document.getElementById('resultScreen').classList.add('hidden');
            document.getElementById('reviewContainer').classList.add('hidden');
            document.getElementById('loginScreen').classList.remove('hidden');
            document.getElementById('userInfoBadge').classList.add('hidden');
        }

        function escapeHtml(str) {
            return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;");
        }
    </script>
</body>
</html>
