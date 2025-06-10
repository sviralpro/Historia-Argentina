<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Historia Argentina - Juego de Preguntas</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #74b9ff 0%, #0984e3 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .quiz-container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            padding: 40px;
            max-width: 800px;
            width: 100%;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .quiz-container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 5px;
            background: linear-gradient(90deg, #74b9ff, #0984e3, #00b894);
        }

        .header {
            margin-bottom: 30px;
        }

        .header h1 {
            color: #2d3436;
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }

        .header p {
            color: #636e72;
            font-size: 1.1rem;
        }

        .question-counter {
            background: linear-gradient(135deg, #fd79a8, #e84393);
            color: white;
            padding: 10px 20px;
            border-radius: 25px;
            font-weight: bold;
            margin-bottom: 20px;
            display: inline-block;
        }

        .question-card {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 30px;
            margin-bottom: 30px;
            border-left: 5px solid #0984e3;
            text-align: left;
            transition: transform 0.3s ease;
        }

        .question-card:hover {
            transform: translateY(-5px);
        }

        .question-number {
            background: #0984e3;
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-bottom: 15px;
        }

        .question-text {
            font-size: 1.3rem;
            color: #2d3436;
            line-height: 1.6;
            margin-bottom: 20px;
        }

        .answer-space {
            border: 2px dashed #ddd;
            border-radius: 10px;
            padding: 20px;
            background: white;
            min-height: 80px;
            position: relative;
        }

        .answer-space::before {
            content: '✍️ Escribe tu respuesta aquí...';
            color: #b2bec3;
            font-style: italic;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            pointer-events: none;
        }

        .controls {
            display: flex;
            justify-content: space-between;
            margin-top: 30px;
            gap: 20px;
        }

        .btn {
            padding: 12px 30px;
            border: none;
            border-radius: 25px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #0984e3, #74b9ff);
            color: white;
        }

        .btn-secondary {
            background: linear-gradient(135deg, #00b894, #00cec9);
            color: white;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .progress-bar {
            width: 100%;
            height: 10px;
            background: #ddd;
            border-radius: 5px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #0984e3, #00b894);
            transition: width 0.5s ease;
            border-radius: 5px;
        }

        .completion-message {
            background: linear-gradient(135deg, #00b894, #00cec9);
            color: white;
            padding: 30px;
            border-radius: 15px;
            margin-top: 20px;
            text-align: center;
        }

        .completion-message h2 {
            font-size: 2rem;
            margin-bottom: 10px;
        }

        @media (max-width: 768px) {
            .quiz-container {
                padding: 20px;
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            .question-text {
                font-size: 1.1rem;
            }
            
            .controls {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div class="header">
            <h1>🇦🇷 Historia Argentina</h1>
            <p>Juego de Preguntas - 17 de Junio</p>
        </div>

        <div class="progress-bar">
            <div class="progress-fill" id="progressFill"></div>
        </div>

        <div class="question-counter">
            Pregunta <span id="currentQuestion">1</span> de <span id="totalQuestions">8</span>
        </div>

        <div class="question-card" id="questionCard">
            <div class="question-number" id="questionNumber">1</div>
            <div class="question-text" id="questionText"></div>
            <div class="answer-space" id="answerSpace"></div>
        </div>

        <div class="controls">
            <button class="btn btn-secondary" id="prevBtn" onclick="previousQuestion()" disabled>
                ← Anterior
            </button>
            <button class="btn btn-primary" id="nextBtn" onclick="nextQuestion()">
                Siguiente →
            </button>
        </div>

        <div class="completion-message" id="completionMessage" style="display: none;">
            <h2>🎉 ¡Excelente trabajo!</h2>
            <p>Has completado todas las preguntas de Historia Argentina.</p>
            <p>¡Muy bien por tu dedicación al estudio!</p>
        </div>
    </div>

    <script>
        const questions = [
            "¿Cuáles eran las causas de la crisis del sistema colonial español en el Río de la Plata durante el período colonial?",
            "¿Cómo se inició el proceso de independencia de las colonias americanas, qué acontecimientos determinaron su llegada? ¿Qué fue lo que motivó a los Integralistas?",
            "¿Qué objetivo tenía cada uno de los congresos argentinos que se realizaron en Buenos Aires entre 1810 y 1860?",
            "¿Qué influencia y personalidad tuvo José de San Martín en el proceso de independencia de las colonias de Argentina? ¿Qué tipo de liderazgo ejerció?",
            "¿Qué sucedió el 17 de junio de 1821? ¿Dónde se desarrolló la batalla de Güemes? ¿En qué contexto se desarrolló?",
            "¿Quién terminó a Güemes? ¿Por qué se pondría?",
            "¿Qué provincias defendió el Norte y Salta?",
            "¿Cuál fue la vestimenta que lo caracterizaba? ¿A qué clase social pertenecía?"
        ];

        let currentQuestionIndex = 0;
        const totalQuestions = questions.length;

        function displayQuestion() {
            document.getElementById('questionText').textContent = questions[currentQuestionIndex];
            document.getElementById('questionNumber').textContent = currentQuestionIndex + 1;
            document.getElementById('currentQuestion').textContent = currentQuestionIndex + 1;
            document.getElementById('totalQuestions').textContent = totalQuestions;
            
            // Update progress bar
            const progress = ((currentQuestionIndex + 1) / totalQuestions) * 100;
            document.getElementById('progressFill').style.width = progress + '%';
            
            // Update button states
            document.getElementById('prevBtn').disabled = currentQuestionIndex === 0;
            document.getElementById('nextBtn').disabled = currentQuestionIndex === totalQuestions - 1;
            
            if (currentQuestionIndex === totalQuestions - 1) {
                document.getElementById('nextBtn').textContent = 'Finalizar ✓';
            } else {
                document.getElementById('nextBtn').textContent = 'Siguiente →';
            }
        }

        function nextQuestion() {
            if (currentQuestionIndex < totalQuestions - 1) {
                currentQuestionIndex++;
                displayQuestion();
            } else {
                // Show completion message
                document.getElementById('questionCard').style.display = 'none';
                document.getElementById('completionMessage').style.display = 'block';
                document.querySelector('.controls').style.display = 'none';
                document.querySelector('.question-counter').style.display = 'none';
            }
        }

        function previousQuestion() {
            if (currentQuestionIndex > 0) {
                currentQuestionIndex--;
                displayQuestion();
            }
        }

        // Initialize the quiz
        displayQuestion();

        // Add keyboard navigation
        document.addEventListener('keydown', function(event) {
            if (event.key === 'ArrowLeft' && currentQuestionIndex > 0) {
                previousQuestion();
            } else if (event.key === 'ArrowRight' && currentQuestionIndex < totalQuestions - 1) {
                nextQuestion();
            }
        });
    </script>
</body>
</html>
