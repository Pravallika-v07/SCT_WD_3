
Pravallika Reddy <pravallikadotcom2005@gmail.com>
3:18 PM (40 minutes ago)
to me

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Easy Quiz Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      display: flex;
      justify-content: center;
      padding: 30px;
    }

    .quiz-container {
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      width: 100%;
      max-width: 600px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    h1 {
      text-align: center;
      color: #333;
    }

    button {
      margin-top: 20px;
      padding: 10px 20px;
      background: #28a745;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    button:hover {
      background: #218838;
    }

    label {
      display: block;
      margin: 10px 0;
    }

    input[type="text"] {
      width: 100%;
      padding: 8px;
      margin-top: 10px;
    }

    #result {
      text-align: center;
      font-size: 1.2em;
      margin-top: 20px;
      color: #333;
    }
  </style>
</head>
<body>
  <div class="quiz-container">
    <h1>Easy Quiz Game</h1>
    <div id="quiz-box"></div>
    <button onclick="nextQuestion()">Next</button>
    <div id="result"></div>
  </div>

  <script>
    const quizData = [
      {
        type: 'single',
        question: "Which animal is known as the 'King of the Jungle'?",
        options: ["Elephant", "Tiger", "Lion", "Giraffe"],
        correct: "Lion"
      },
      {
        type: 'multi',
        question: "Select all colors from the list:",
        options: ["Apple", "Red", "Blue", "Chair"],
        correct: ["Red", "Blue"]
      },
      {
        type: 'fill',
        question: "The color of the sky on a clear day is ____.",
        correct: "blue"
      },
      {
        type: 'single',
        question: "How many legs does a spider have?",
        options: ["4", "6", "8", "10"],
        correct: "8"
      },
      {
        type: 'fill',
        question: "2 + 2 equals ____.",
        correct: "4"
      }
    ];

    let current = 0;
    let score = 0;

    function renderQuestion() {
      const quizBox = document.getElementById("quiz-box");
      const q = quizData[current];
      quizBox.innerHTML = `<p><strong>Q${current + 1}:</strong> ${q.question}</p>`;

      if (q.type === 'single') {
        q.options.forEach(opt => {
          quizBox.innerHTML += `<label><input type="radio" name="answer" value="${opt}"> ${opt}</label>`;
        });
      } else if (q.type === 'multi') {
        q.options.forEach(opt => {
          quizBox.innerHTML += `<label><input type="checkbox" name="answer" value="${opt}"> ${opt}</label>`;
        });
      } else if (q.type === 'fill') {
        quizBox.innerHTML += `<input type="text" id="fillInput" placeholder="Your answer">`;
      }
    }

    function nextQuestion() {
      const q = quizData[current];
      let userAnswer;

      if (q.type === 'single') {
        const selected = document.querySelector('input[name="answer"]:checked');
        if (!selected) return alert("Please select an answer.");
        userAnswer = selected.value;
        if (userAnswer === q.correct) score++;
      } else if (q.type === 'multi') {
        const selected = Array.from(document.querySelectorAll('input[name="answer"]:checked')).map(e => e.value);
        if (selected.length === 0) return alert("Please select at least one option.");
        if (arraysEqual(selected, q.correct)) score++;
      } else if (q.type === 'fill') {
        const input = document.getElementById("fillInput").value.trim();
        if (!input) return alert("Please enter your answer.");
        if (input.toLowerCase() === q.correct.toLowerCase()) score++;
      }

      current++;
      if (current < quizData.length) {
        renderQuestion();
      } else {
        document.getElementById("quiz-box").innerHTML = "";
        document.querySelector("button").style.display = "none";
        document.getElementById("result").innerHTML = `<h2>Your Score: ${score}/${quizData.length}</h2>`;
      }
    }

    function arraysEqual(a, b) {
      return a.sort().join() === b.sort().join();
    }

    // Start quiz
    renderQuestion();
  </script>
</body>
</html>
