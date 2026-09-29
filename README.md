<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz de Matemática - E.M.E.F. Rita Paula de Brito</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
</head>
<body class="bg-slate-100 min-h-screen flex flex-col justify-between font-sans text-slate-800">

    <!-- Cabeçalho Principal -->
    <header class="bg-indigo-700 text-white shadow-lg p-4">
        <div class="max-w-4xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-2">
            <div>
                <h1 class="text-xl font-bold tracking-wide flex items-center gap-2">
                    <i class="fa-solid font-bold fa-school"></i> E.M.E.F. Rita Paula de Brito
                </h1>
                <p class="text-xs text-indigo-200">Avaliação Interativa de Matemática (20 Questões • 0,5 pt/cada)</p>
            </div>
            <button id="btn-portal-prof" onclick="abrirModalProfessor()" class="bg-indigo-600 hover:bg-indigo-800 text-xs text-white px-3 py-2 rounded-lg transition border border-indigo-400 flex items-center gap-1.5 shadow">
                <i class="fa-solid fa-user-tie"></i> Área do Professor
            </button>
        </div>
    </header>

    <!-- Conteúdo Principal -->
    <main class="max-w-2xl mx-auto w-full p-4 flex-grow flex items-center justify-center">

        <!-- 1. TELA INICIAL: ID DO ALUNO -->
        <div id="screen-welcome" class="bg-white w-full rounded-2xl shadow-xl p-6 sm:p-8 border border-slate-200">
            <div class="text-center mb-6">
                <div class="inline-flex items-center justify-center w-16 h-16 rounded-full bg-indigo-100 text-indigo-600 mb-3 text-2xl shadow-inner">
                    <i class="fa-solid fa-calculator"></i>
                </div>
                <h2 class="text-2xl font-bold text-slate-800">Quiz de Matemática</h2>
                <p class="text-slate-500 text-sm mt-1">Identifique-se para dar início à prova</p>
            </div>

            <form onsubmit="iniciarQuiz(event)" class="space-y-4">
                <div>
                    <label for="student-fullname" class="block text-sm font-semibold text-slate-700 mb-1">
                        Nome completo do aluno:
                    </label>
                    <input type="text" id="student-fullname" required
                        placeholder="Digite o nome completo (ex: Gabriel Silva Santos)"
                        class="w-full px-4 py-3 rounded-xl border border-slate-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition text-slate-800">
                    <p id="error-name" class="text-red-500 text-xs mt-1 hidden">Por favor, digite seu nome e pelo menos um sobrenome.</p>
                </div>

                <div class="bg-indigo-50 border border-indigo-100 rounded-xl p-4 text-xs text-indigo-900 space-y-1">
                    <p class="font-bold flex items-center gap-1"><i class="fa-solid fa-circle-info"></i> Informações do Teste:</p>
                    <ul class="list-disc list-inside space-y-0.5 text-indigo-800">
                        <li><strong>Total de questões:</strong> 20 perguntas</li>
                        <li><strong>Valor de cada questão:</strong> 0,5 ponto</li>
                        <li><strong>Nota Máxima:</strong> 10,0 pontos</li>
                        <li>Explicação exibida ao confirmar cada resposta.</li>
                    </ul>
                </div>

                <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3.5 px-4 rounded-xl shadow-lg hover:shadow-xl transition transform active:scale-[0.98]">
                    Iniciar Quiz <i class="fa-solid fa-arrow-right ml-1"></i>
                </button>
            </form>
        </div>

        <!-- 2. TELA DAS QUESTÕES (1 por página) -->
        <div id="screen-quiz" class="bg-white w-full rounded-2xl shadow-xl p-6 sm:p-8 border border-slate-200 hidden">
            <!-- Barra de Progresso e Topo -->
            <div class="flex justify-between items-center mb-2 text-sm text-slate-500 font-medium">
                <span>Questão <strong id="current-q-num" class="text-indigo-600">1</strong> de 20</span>
                <span id="student-display-name" class="text-xs bg-slate-100 px-2.5 py-1 rounded-full text-slate-600 font-semibold truncate max-w-[180px]"></span>
            </div>
            
            <div class="w-full bg-slate-200 h-2.5 rounded-full overflow-hidden mb-6">
                <div id="progress-bar" class="bg-indigo-600 h-full transition-all duration-300" style="width: 5%;"></div>
            </div>

            <!-- Pergunta Atual -->
            <div class="mb-6">
                <span id="q-category" class="inline-block px-2.5 py-0.5 text-xs font-semibold rounded-full bg-indigo-100 text-indigo-700 mb-2">Categoria</span>
                <h3 id="q-text" class="text-xl font-bold text-slate-800 leading-snug">Texto da Pergunta</h3>
            </div>

            <!-- Opções de Resposta -->
            <div id="options-container" class="space-y-3 mb-6">
                <!-- Injetado dinamicamente -->
            </div>

            <!-- Caixa de Explicação -->
            <div id="explanation-box" class="hidden mb-6 p-4 rounded-xl border transition-all">
                <h4 id="explanation-title" class="font-bold text-sm mb-1 flex items-center gap-1.5"></h4>
                <p id="explanation-text" class="text-sm leading-relaxed"></p>
            </div>

            <!-- Ações -->
            <div class="flex justify-end">
                <button id="btn-action" onclick="processarAcao()" disabled
                    class="bg-slate-300 text-slate-500 font-bold py-3 px-6 rounded-xl transition cursor-not-allowed">
                    Confirmar Resposta
                </button>
            </div>
        </div>

        <!-- 3. TELA DE RESULTADO DO ALUNO -->
        <div id="screen-result" class="bg-white w-full rounded-2xl shadow-xl p-6 sm:p-8 border border-slate-200 text-center hidden">
            <div class="inline-flex items-center justify-center w-20 h-20 rounded-full bg-green-100 text-green-600 mb-4 text-3xl shadow-inner">
                <i class="fa-solid fa-trophy"></i>
            </div>

            <h2 class="text-2xl font-bold text-slate-800">Quiz Concluído!</h2>
            <p class="text-slate-500 text-sm mt-1">Parabéns, <span id="res-student-name" class="font-bold text-slate-700"></span>!</p>

            <div class="my-6 p-6 bg-slate-50 rounded-2xl border border-slate-200 inline-block w-full">
                <div class="text-xs font-semibold text-slate-400 uppercase tracking-wider mb-1">Sua Nota Final</div>
                <div id="res-score-grade" class="text-5xl font-black text-indigo-600 mb-2">0,0</div>
                <div class="text-sm text-slate-600">
                    Você acertou <strong id="res-correct-count" class="text-green-600">0</strong> de <strong>20</strong> questões.
                </div>
            </div>

            <div class="flex flex-col sm:flex-row gap-3 justify-center">
                <button onclick="reiniciarQuiz()" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 px-6 rounded-xl transition shadow">
                    Refazer Quiz
                </button>
                <button onclick="abrirModalProfessor()" class="bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold py-3 px-6 rounded-xl transition">
                    Acessar Painel
                </button>
            </div>
        </div>

    </main>

    <!-- Rodapé -->
    <footer class="bg-white border-t border-slate-200 py-3 text-center text-xs text-slate-500">
        E.M.E.F. Rita Paula de Brito &bull; Sistema de Avaliação Interativa de Matemática
    </footer>

    <!-- MODAL: LOGIN / PAINEL DO PROFESSOR -->
    <div id="modal-professor" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden">
        <div class="bg-white rounded-2xl shadow-2xl max-w-2xl w-full max-h-[90vh] flex flex-col overflow-hidden border border-slate-200">
            
            <!-- Modal Header -->
            <div class="bg-indigo-700 text-white px-6 py-4 flex justify-between items-center">
                <h3 class="font-bold text-lg flex items-center gap-2">
                    <i class="fa-solid fa-user-shield"></i> Painel do Professor
                </h3>
                <button onclick="fecharModalProfessor()" class="text-indigo-200 hover:text-white text-xl">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <!-- View 1: Formulário de Login do Professor -->
            <div id="prof-login-view" class="p-6 sm:p-8 space-y-4">
                <p class="text-sm text-slate-600">Acesse com suas credenciais para visualizar as notas dos alunos.</p>
                
                <form onsubmit="autenticarProfessor(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs font-bold text-slate-700 uppercase mb-1">Nome de Usuário:</label>
                        <input type="text" id="prof-username" required placeholder="Digite seu usuário"
                            class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-indigo-500 focus:outline-none text-sm">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-slate-700 uppercase mb-1">Senha de Acesso:</label>
                        <input type="password" id="prof-password" required placeholder="Digite sua senha"
                            class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-indigo-500 focus:outline-none text-sm">
                    </div>
                    <p id="prof-login-error" class="text-red-500 text-xs hidden">Usuário ou senha incorretos.</p>
                    
                    <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 rounded-xl transition">
                        Entrar no Painel
                    </button>
                </form>
            </div>

            <!-- View 2: Dashboard/Relatório das Notas -->
            <div id="prof-dashboard-view" class="p-6 flex-grow flex flex-col overflow-hidden hidden">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-2 mb-4">
                    <div>
                        <h4 class="font-bold text-slate-800">Histórico de Alunos e Notas</h4>
                        <p class="text-xs text-slate-500">Cada questão correta equivale a 0,5 ponto.</p>
                    </div>
                    <div class="flex gap-2">
                        <button onclick="limparHistorico()" class="text-xs bg-red-50 hover:bg-red-100 text-red-600 px-3 py-1.5 rounded-lg font-semibold transition border border-red-200">
                            <i class="fa-solid fa-trash"></i> Limpar Dados
                        </button>
                        <button onclick="logoutProfessor()" class="text-xs bg-slate-100 hover:bg-slate-200 text-slate-600 px-3 py-1.5 rounded-lg font-semibold transition">
                            Sair
                        </button>
                    </div>
                </div>

                <!-- Campo de Pesquisa -->
                <div class="mb-3">
                    <input type="text" id="search-student" oninput="renderizarTabelaAlunos()" placeholder="Buscar por nome do aluno..." 
                        class="w-full px-3 py-2 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-1 focus:ring-indigo-500">
                </div>

                <!-- Tabela de Alunos -->
                <div class="overflow-y-auto flex-grow border border-slate-200 rounded-xl">
                    <table class="w-full text-left text-sm">
                        <thead class="bg-slate-50 text-slate-600 uppercase text-[11px] sticky top-0 border-b border-slate-200">
                            <tr>
                                <th class="py-2.5 px-4 font-bold">Aluno</th>
                                <th class="py-2.5 px-4 font-bold text-center">Acertos</th>
                                <th class="py-2.5 px-4 font-bold text-center">Nota Final</th>
                                <th class="py-2.5 px-4 font-bold text-right">Data/Hora</th>
                            </tr>
                        </thead>
                        <tbody id="student-table-body" class="divide-y divide-slate-100 text-slate-700">
                            <!-- Injetado dinamicamente -->
                        </tbody>
                    </table>
                </div>
            </div>

        </div>
    </div>

    <!-- Script de Funcionamento -->
    <script>
        // Banco de Questões (20 Questões)
        const questions = [
            {
                category: "Adição",
                question: "Qual é o resultado da soma de 348 + 475?",
                options: ["813", "823", "833", "843"],
                answer: 1,
                explanation: "348 + 475 = 823. Somando as unidades (8+5=13), dezenas (4+7+1=12) e centenas (3+4+1=8)."
            },
            {
                category: "Subtração",
                question: "Calcule: 1.000 - 467",
                options: ["533", "543", "633", "537"],
                answer: 0,
                explanation: "1000 - 467 = 533."
            },
            {
                category: "Multiplicação",
                question: "Qual o valor do produto de 18 x 14?",
                options: ["242", "252", "262", "272"],
                answer: 1,
                explanation: "18 x 14 = 252."
            },
            {
                category: "Elevação (Potenciação)",
                question: "Qual o resultado de 3⁴ (3 elevado à quarta potência)?",
                options: ["12", "27", "81", "243"],
                answer: 2,
                explanation: "3⁴ = 3 x 3 x 3 x 3 = 81."
            },
            {
                category: "Raiz Quadrada",
                question: "Qual é a raíz quadrada de 169 (√169)?",
                options: ["11", "12", "13", "14"],
                answer: 2,
                explanation: "√169 = 13, pois 13 x 13 = 169."
            },
            {
                category: "Adição",
                question: "Calcule a soma: 1.250 + 875",
                options: ["2.025", "2.125", "2.225", "2.175"],
                answer: 1,
                explanation: "1250 + 875 = 2125."
            },
            {
                category: "Subtração",
                question: "Resolva: 850 - 385",
                options: ["455", "465", "475", "485"],
                answer: 1,
                explanation: "850 - 385 = 465."
            },
            {
                category: "Multiplicação",
                question: "Quanto é 25 x 16?",
                options: ["380", "400", "420", "450"],
                answer: 1,
                explanation: "25 x 16 = 400."
            },
            {
                category: "Elevação (Potenciação)",
                question: "Qual o valor de 2⁵ (2 elevado à quinta potência)?",
                options: ["10", "16", "32", "64"],
                answer: 2,
                explanation: "2⁵ = 2 x 2 x 2 x 2 x 2 = 32."
            },
            {
                category: "Raiz Quadrada",
                question: "Qual é a raíz quadrada de 225 (√225)?",
                options: ["15", "25", "35", "45"],
                answer: 0,
                explanation: "√225 = 15, pois 15 x 15 = 225."
            },
            {
                category: "Adição",
                question: "Qual o resultado de 567 + 389?",
                options: ["946", "956", "966", "976"],
                answer: 1,
                explanation: "567 + 389 = 956."
            },
            {
                category: "Subtração",
                question: "Calcule: 500 - 238",
                options: ["252", "262", "272", "282"],
                answer: 1,
                explanation: "500 - 238 = 262."
            },
            {
                category: "Multiplicação",
                question: "Quanto é 15 x 15?",
                options: ["205", "215", "225", "235"],
                answer: 2,
                explanation: "15 x 15 = 225."
            },
            {
                category: "Elevação (Potenciação)",
                question: "Qual o valor de 12² (12 elevado ao quadrado)?",
                options: ["24", "120", "144", "168"],
                answer: 2,
                explanation: "12² = 12 x 12 = 144."
            },
            {
                category: "Raiz Quadrada",
                question: "Qual é a raiz quadrada de 400 (√400)?",
                options: ["10", "20", "30", "40"],
                answer: 1,
                explanation: "√400 = 20, pois 20 x 20 = 400."
            },
            {
                category: "Adição",
                question: "Resolva a soma de três parcelas: 120 + 250 + 340",
                options: ["690", "700", "710", "720"],
                answer: 2,
                explanation: "120 + 250 + 340 = 710."
            },
            {
                category: "Subtração",
                question: "Se você tem R$ 150,00 e gasta R$ 87,50, quanto resta?",
                options: ["R$ 61,50", "R$ 62,50", "R$ 63,50", "R$ 64,50"],
                answer: 1,
                explanation: "150,00 - 87,50 = 62,50."
            },
            {
                category: "Multiplicação",
                question: "Qual é o valor da expressão (6 x 7) + 18?",
                options: ["50", "60", "70", "80"],
                answer: 1,
                explanation: "6 x 7 = 42; 42 + 18 = 60."
            },
            {
                category: "Elevação (Potenciação)",
                question: "Qual é o resultado de 10³ (10 elevado ao cubo)?",
                options: ["30", "100", "1.000", "10.000"],
                answer: 2,
                explanation: "10³ = 10 x 10 x 10 = 1.000."
            },
            {
                category: "Raiz Quadrada",
                question: "Qual o valor da expressão: √144 + √25?",
                options: ["15", "17", "19", "21"],
                answer: 1,
                explanation: "√144 = 12 e √25 = 5. Portanto, 12 + 5 = 17."
            }
        ];

        // Estado da Aplicação
        let currentQuestionIndex = 0;
        let score = 0;
        let selectedOptionIndex = null;
        let isAnswered = false;
        let studentFullName = "";

        // Iniciar Quiz
        function iniciarQuiz(event) {
            event.preventDefault();
            const inputName = document.getElementById('student-fullname').value.trim();
            const errorElement = document.getElementById('error-name');

            if (!inputName.includes(" ") || inputName.length < 5) {
                errorElement.classList.remove('hidden');
                return;
            }
            errorElement.classList.add('hidden');

            studentFullName = inputName;
            currentQuestionIndex = 0;
            score = 0;

            document.getElementById('screen-welcome').classList.add('hidden');
            document.getElementById('screen-quiz').classList.remove('hidden');
            document.getElementById('student-display-name').textContent = studentFullName;

            carregarQuestao();
        }

        // Carregar Pergunta Atual
        function carregarQuestao() {
            isAnswered = false;
            selectedOptionIndex = null;

            const q = questions[currentQuestionIndex];

            document.getElementById('current-q-num').textContent = currentQuestionIndex + 1;
            const progressPercent = ((currentQuestionIndex + 1) / questions.length) * 100;
            document.getElementById('progress-bar').style.width = `${progressPercent}%`;

            document.getElementById('q-category').textContent = q.category;
            document.getElementById('q-text').textContent = q.question;

            const container = document.getElementById('options-container');
            container.innerHTML = '';

            q.options.forEach((opt, idx) => {
                const btn = document.createElement('button');
                btn.className = `w-full text-left p-4 rounded-xl border border-slate-200 hover:border-indigo-300 hover:bg-indigo-50/50 transition flex items-center justify-between text-slate-700 font-medium`;
                btn.onclick = () => selecionarOpcao(idx);
                btn.id = `opt-${idx}`;
                btn.innerHTML = `
                    <span>${opt}</span>
                    <span class="w-6 h-6 rounded-full border border-slate-300 flex items-center justify-center text-xs check-icon"></span>
                `;
                container.appendChild(btn);
            });

            document.getElementById('explanation-box').className = 'hidden mb-6 p-4 rounded-xl border transition-all';
            const btnAction = document.getElementById('btn-action');
            btnAction.disabled = true;
            btnAction.className = 'bg-slate-300 text-slate-500 font-bold py-3 px-6 rounded-xl transition cursor-not-allowed';
            btnAction.textContent = 'Confirmar Resposta';
        }

        // Seleção de Opção
        function selecionarOpcao(index) {
            if (isAnswered) return;

            selectedOptionIndex = index;

            questions[currentQuestionIndex].options.forEach((_, idx) => {
                const btn = document.getElementById(`opt-${idx}`);
                if (idx === index) {
                    btn.className = `w-full text-left p-4 rounded-xl border-2 border-indigo-600 bg-indigo-50 text-indigo-900 font-semibold flex items-center justify-between shadow-sm`;
                    btn.querySelector('.check-icon').className = `w-6 h-6 rounded-full bg-indigo-600 text-white flex items-center justify-center text-xs`;
                    btn.querySelector('.check-icon').innerHTML = `<i class="fa-solid fa-check"></i>`;
                } else {
                    btn.className = `w-full text-left p-4 rounded-xl border border-slate-200 text-slate-700 flex items-center justify-between opacity-70`;
                    btn.querySelector('.check-icon').className = `w-6 h-6 rounded-full border border-slate-300 flex items-center justify-center text-xs`;
                    btn.querySelector('.check-icon').innerHTML = ``;
                }
            });

            const btnAction = document.getElementById('btn-action');
            btnAction.disabled = false;
            btnAction.className = 'bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 px-6 rounded-xl transition shadow-lg';
        }

        // Processar Resposta
        function processarAcao() {
            if (!isAnswered) {
                isAnswered = true;
                const q = questions[currentQuestionIndex];
                const isCorrect = selectedOptionIndex === q.answer;

                if (isCorrect) score++;

                q.options.forEach((_, idx) => {
                    const btn = document.getElementById(`opt-${idx}`);
                    btn.onclick = null;

                    if (idx === q.answer) {
                        btn.className = `w-full text-left p-4 rounded-xl border-2 border-green-500 bg-green-50 text-green-900 font-bold flex items-center justify-between`;
                        btn.querySelector('.check-icon').className = `w-6 h-6 rounded-full bg-green-500 text-white flex items-center justify-center text-xs`;
                        btn.querySelector('.check-icon').innerHTML = `<i class="fa-solid fa-check"></i>`;
                    } else if (idx === selectedOptionIndex && !isCorrect) {
                        btn.className = `w-full text-left p-4 rounded-xl border-2 border-red-500 bg-red-50 text-red-900 font-bold flex items-center justify-between`;
                        btn.querySelector('.check-icon').className = `w-6 h-6 rounded-full bg-red-500 text-white flex items-center justify-center text-xs`;
                        btn.querySelector('.check-icon').innerHTML = `<i class="fa-solid fa-xmark"></i>`;
                    } else {
                        btn.className = `w-full text-left p-4 rounded-xl border border-slate-200 text-slate-400 opacity-40 flex items-center justify-between`;
                    }
                });

                const expBox = document.getElementById('explanation-box');
                const expTitle = document.getElementById('explanation-title');
                const expText = document.getElementById('explanation-text');

                expBox.classList.remove('hidden');
                expText.textContent = q.explanation;

                if (isCorrect) {
                    expBox.className = 'mb-6 p-4 rounded-xl border border-green-200 bg-green-50 text-green-900';
                    expTitle.innerHTML = `<i class="fa-solid fa-circle-check text-green-600"></i> Resposta Correta! (+0,5 pt)`;
                } else {
                    expBox.className = 'mb-6 p-4 rounded-xl border border-red-200 bg-red-50 text-red-900';
                    expTitle.innerHTML = `<i class="fa-solid fa-circle-xmark text-red-600"></i> Resposta Incorreta!`;
                }

                const btnAction = document.getElementById('btn-action');
                if (currentQuestionIndex < questions.length - 1) {
                    btnAction.textContent = 'Próxima Questão';
                } else {
                    btnAction.textContent = 'Ver Resultado Final';
                }
            } else {
                currentQuestionIndex++;
                if (currentQuestionIndex < questions.length) {
                    carregarQuestao();
                } else {
                    finalizarQuiz();
                }
            }
        }

        function finalizarQuiz() {
            document.getElementById('screen-quiz').classList.add('hidden');
            document.getElementById('screen-result').classList.remove('hidden');

            const finalGrade = (score * 0.5).toFixed(1);

            document.getElementById('res-student-name').textContent = studentFullName;
            document.getElementById('res-score-grade').textContent = finalGrade.replace('.', ',');
            document.getElementById('res-correct-count').textContent = score;

            salvarResultadoAluno(studentFullName, score, finalGrade);
        }

        function reiniciarQuiz() {
            document.getElementById('screen-result').classList.add('hidden');
            document.getElementById('screen-welcome').classList.remove('hidden');
            document.getElementById('student-fullname').value = '';
        }

        function salvarResultadoAluno(nome, acertos, nota) {
            const historico = JSON.parse(localStorage.getItem('quiz_math_results') || '[]');
            const novoRegistro = {
                id: Date.now(),
                nome: nome,
                acertos: acertos,
                nota: nota,
                data: new Date().toLocaleString('pt-BR')
            };
            historico.unshift(novoRegistro);
            localStorage.setItem('quiz_math_results', JSON.stringify(historico));
        }

        function abrirModalProfessor() {
            document.getElementById('modal-professor').classList.remove('hidden');
        }

        function fecharModalProfessor() {
            document.getElementById('modal-professor').classList.add('hidden');
        }

        function autenticarProfessor(event) {
            event.preventDefault();
            const user = document.getElementById('prof-username').value;
            const pass = document.getElementById('prof-password').value;
            const errorMsg = document.getElementById('prof-login-error');

            if (user === 'Jhon777' && pass === 'jhonbonitao') {
                errorMsg.classList.add('hidden');
                document.getElementById('prof-login-view').classList.add('hidden');
                document.getElementById('prof-dashboard-view').classList.remove('hidden');
                renderizarTabelaAlunos();
            } else {
                errorMsg.classList.remove('hidden');
            }
        }

        function logoutProfessor() {
            document.getElementById('prof-dashboard-view').classList.add('hidden');
            document.getElementById('prof-login-view').classList.remove('hidden');
            document.getElementById('prof-username').value = '';
            document.getElementById('prof-password').value = '';
        }

        function renderizarTabelaAlunos() {
            const historico = JSON.parse(localStorage.getItem('quiz_math_results') || '[]');
            const tbody = document.getElementById('student-table-body');
            const searchTerm = document.getElementById('search-student').value.toLowerCase();

            tbody.innerHTML = '';

            const filtrados = historico.filter(item => item.nome.toLowerCase().includes(searchTerm));

            if (filtrados.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="4" class="py-6 text-center text-slate-400 text-xs">
                            Nenhum registro de aluno encontrado.
                        </td>
                    </tr>
                `;
                return;
            }

            filtrados.forEach(item => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50 transition";
                tr.innerHTML = `
                    <td class="py-3 px-4 font-semibold text-slate-800">${item.nome}</td>
                    <td class="py-3 px-4 text-center text-slate-600">${item.acertos}/20</td>
                    <td class="py-3 px-4 text-center">
                        <span class="inline-block px-2.5 py-1 rounded-full text-xs font-bold ${parseFloat(item.nota) >= 6 ? 'bg-green-100 text-green-700' : 'bg-amber-100 text-amber-700'}">
                            ${item.nota.replace('.', ',')}
                        </span>
                    </td>
                    <td class="py-3 px-4 text-right text-xs text-slate-400">${item.data}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function limparHistorico() {
            if (confirm("Tem certeza que deseja apagar o histórico de notas de todos os alunos?")) {
                localStorage.removeItem('quiz_math_results');
                renderizarTabelaAlunos();
            }
        }
    </script>
</body>
</html>
