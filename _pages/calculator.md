---
permalink: /calculator/
title: "Dosage Calculator"
author_profile: false
---

<body>
  <style>
    /* body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    } */

    #calculator {
      display: flex;
    }

    #left-section {
      flex: 1;
      padding: 20px;
    }

    #right-section {
      flex: 1;
      padding: 20px;
      font-size: 14px;
    }

    #left-section-mod {
      flex: 1;
      padding-left: 20px;
    }

    canvas {
      border: 1px solid #ddd;
      width: 100%;
    }

    /* Style for the collapsible content */
    .collapsible-content {
      display: none;
      margin-top: 10px;
    }

    /* Style for the button */
    .collapsible-btn {
      cursor: pointer;
      padding-top: 10px;
      border: none;
      text-align: center;
      outline: none;
      width: 100%;
      position: relative;
      background-color: #fff
    }
    
    /* Style for the button */
    .collapsible-btn:focus {
      outline: none;
    }

    /* Style for the V and > shapes */
    .collapsible-btn::before {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 0;
      height: 0;
      border-style: solid;
    }

    /* Style for the V shape when collapsed */
    .collapsible-btn.collapsed::before {
      border-width: 5px 5px 0 5px;
      border-color: #000 transparent transparent transparent;
    }

    /* Style for the > shape when opened */
    .collapsible-btn.expanded::before {
      border-width: 5px 5px 5px 5px;
      border-color: transparent transparent #000 transparent;
    }

    .collapsible-alert-box {
      display: none;
      padding: 10px;
      background-color: #F2F3F3; /* grey color */
      border-radius: 3px; /* Rounded corners */
      margin: 10px 0; /* Margin for spacing */
    }
  </style>
  <div id="calculator">
    <div id="left-section">
      <div id="result">Result: <span id="resultValue"></span></div>
      <canvas id="graphCanvas" width="500" height="500"></canvas>
      <div id="interactions">
        Possible Drug Interactions: <span id="calculatedInteractions"></span>
      </div>
    </div>
    <div id="right-section">
      <label for="gender">Gender:</label>
      <input type="radio" id="male" name="gender" oninput="updateGraph()" checked> Male&nbsp;&nbsp;
      <input type="radio" id="female" name="gender" oninput="updateGraph()"> Female
      
      <label for="age">Age:</label>
      <input type="number" id="age" step="1" placeholder="Enter patient's age" oninput="updateGraph()">

      <label for="height">Height:</label>
      <input type="number" id="height" step="0.1" placeholder="Enter patient's height in cm" oninput="updateGraph()">
      
      <label for="weigth">Weigth:</label>
      <input type="number" id="weigth" step="0.1" placeholder="Enter patient's weigth in kg" oninput="updateGraph()">

      <div id="bmi">Body Mass Index: <span id="calculatedBmi"></span></div>

      <button class="collapsible-btn collapsed" onclick="toggleCollapsible()"> &nbsp; </button>

      <!-- Collapsible content -->
      <div class="collapsible-content" id="collapsibleContent">
        <input type="checkbox" id="prevEvents"> Previous Acute Coronary Events
        <br>

        <input type="checkbox" id="familyHistory"> Family History of CVD
        <br>

        <input type="checkbox" id="smoking"> Smoking
        <br>
        <br>

      
        <label for="totalChol">Total Cholesterol:</label>
        <input type="number" id="totalChol" step="0.1" placeholder="Enter patient's total cholesterol in mmol/L" oninput="updateGraph()">

        <label for="ldlChol">LDL Cholesterol:</label>
        <input type="number" id="ldlChol" step="0.1" placeholder="Enter patient's LDL cholesterol in mmol/L" oninput="updateGraph()">

        <label for="bloodPressure">Blood Pressure:</label>
        <input type="number" id="bloodPressure" step="0.1" placeholder="Enter patient's blood pressure in mmHg" oninput="updateGraph()">

        <label for="liverAlt">Liver ALT and AST:</label>
        <input type="number" id="liverAlt" step="0.1" placeholder="Enter patient's liver ALT in IU/L" oninput="updateGraph()"><input type="number" id="liverAlt" step="0.1" placeholder="Enter patient's liver AST in IU/L" oninput="updateGraph()">
        <br>
        <br>

        Comorbidities:
        <br>

        <input type="checkbox" id="artherialHypertension"> Artherial Hypertension
        <br>

        <input type="checkbox" id="diabetesMellitus" onclick="toggleCollapsibleDiabetes()"> Diabetes Mellitus
        <br>
        <div class="collapsible-content" id="collapsibleContentDiabetes">
          <div id="left-section-mod">
          <label for="hba1c"> Blood Pressure:</label>
          <input type="number" id="hba1c" step="0.1" placeholder="Enter patient's HbA1c in %" oninput="updateGraph()">
          </div>
        </div>

        <input type="checkbox" id="kidneyDisease" onclick="toggleCollapsibleKidney()"> Chronic Kidney Disease
        <br>
        <div class="collapsible-content" id="collapsibleContentKidney">
          <div id="left-section-mod">
            <label for="creatinine"> Creatinine:</label>
            <input type="number" id="creatinine" step="0.1" placeholder="Enter patient's creatinine in mmol/L" oninput="updateGraph()">
            <div>Creatinine Clearance: <span id="calculatedCreatinineClearance"></span></div>
          </div>
        </div>
        <br>
        <input type="checkbox" id="medication" onclick="toggleCollapsibleMedication()"> Uses Other Medications
        <br>
        <div class="collapsible-content" id="collapsibleContentMedication">
          <div id="left-section-mod">
            <div id="input-container">
              <!-- Initial input field -->
              <div class="input-container">
                <label for="value1">Medication 1:</label>
                <input type="text" id="medValue1" name="medValue1" oninput="updateGraph()">
              </div>
            </div>
            <button onclick="addInputField()">Add Another Value</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    function updateGraph() {
      const isMale = document.getElementById('male').checked;
      const age = document.getElementById('age').value;
      const height = document.getElementById('height').value;
      const weigth = document.getElementById('weigth').value;
      const calculatedBmi = weigth / height / height * 10000;
      document.getElementById('calculatedBmi').innerText = calculatedBmi.toFixed(2);
      const creatinine = document.getElementById('creatinine').value;
      const genderCoef = isMale ? 1 : 0.85;
      const calculatedCreatinineClearance = ((140 - age) * weigth / creatinine / 814.464) * genderCoef;
      document.getElementById('calculatedCreatinineClearance').innerText = calculatedCreatinineClearance.toFixed(2);

      const med1 = document.getElementById('medValue1').value
      const atorList = ['Fenofibrate', 'Mavacamten', 'Nefazodone', 'Diltiazem', 'Ciprofibrate', 'Eryhtromycin', 'Clarithromycin', 'Clofibrate', 'Digoxin', 'Verapamil'];
      if (atorList.includes(med1)) {
        calculatedInteractions = "(Atorvastatin, " + med1 + ")"
      }
      else {
        calculatedInteractions = "Not present"
      }
      document.getElementById('calculatedInteractions').innerText = calculatedInteractions;

      const canvas = document.getElementById('graphCanvas');
      const ctx = canvas.getContext('2d');

      // Clear the canvas
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Draw gridlines and tick labels
      drawGridlines(ctx, canvas.width, canvas.height);

      // Draw the power function graph
      ctx.beginPath();
      ctx.strokeStyle = 'blue';
      ctx.lineWidth = 2;

      for (let x = 0; x <= 1; x += 0.01) {
        const y = Math.pow(baseValue, powerValue) * Math.pow(x, powerValue);
        document.getElementById('resultValue').innerText = y.toFixed(2);
        ctx.lineTo(scaleX(x), scaleY(y));
      }

      ctx.stroke();

      // Add legend
      // ctx.font = "50px serif";
      ctx.fillStyle = 'blue';
      ctx.fillText('Graph', canvas.width - 90, 20);
      ctx.fillStyle = 'gray';
      ctx.fillText('Gridlines', canvas.width - 90, 45);
    }

    function drawGridlines(ctx, width, height) {
      ctx.strokeStyle = 'rgba(200, 200, 200, 0.5)';
      ctx.lineWidth = 0.5;
      ctx.fillStyle = 'black';
      ctx.font = '20px Arial';

      // Horizontal gridlines
      for (let y = 0; y <= height; y += height / 10) {
        ctx.beginPath();
        ctx.moveTo(0, y);
        ctx.lineTo(width, y);
        ctx.stroke();

        // Tick labels
        const tickLabel = (1 - y / height).toFixed(1);
        ctx.fillText(tickLabel, 5, y - 5);
      }

      // Vertical gridlines
      for (let x = 0; x <= width; x += width / 10) {
        ctx.beginPath();
        ctx.moveTo(x, 0);
        ctx.lineTo(x, height);
        ctx.stroke();

        // Tick labels
        const tickLabel = x / width;
        ctx.fillText(tickLabel.toFixed(1), x + 5, height - 5);
      }
    }

    function scaleX(x) {
      const canvas = document.getElementById('graphCanvas');
      return x * canvas.width;
    }

    function scaleY(y) {
      const canvas = document.getElementById('graphCanvas');
      return canvas.height - y * canvas.height;
    }

    function toggleCollapsible() {
      var content = document.getElementById("collapsibleContent");
      var button = document.querySelector(".collapsible-btn");

      // Toggle the content's visibility
      if (content.style.display === "block") {
        content.style.display = "none";
        button.classList.remove("active", "expanded");
        button.classList.add("collapsed");
      } else {
        content.style.display = "block";
        button.classList.add("active", "expanded");
        button.classList.remove("collapsed");
      }
    }

    function toggleCollapsibleDiabetes() {
      var content = document.getElementById("collapsibleContentDiabetes");
      var button = document.querySelector(".diabetesMellitus");

      // Toggle the content's visibility
      if (content.style.display === "block") {
        content.style.display = "none";
        button.classList.remove("active", "expanded");
        button.classList.add("collapsed");
      } else {
        content.style.display = "block";
        button.classList.add("active", "expanded");
        button.classList.remove("collapsed");
      }
    }

    function toggleCollapsibleKidney() {
      var content = document.getElementById("collapsibleContentKidney");
      var button = document.querySelector(".kidneyDisease");

      // Toggle the content's visibility
      if (content.style.display === "block") {
        content.style.display = "none";
        button.classList.remove("active", "expanded");
        button.classList.add("collapsed");
      } else {
        content.style.display = "block";
        button.classList.add("active", "expanded");
        button.classList.remove("collapsed");
      }
    }

    function toggleCollapsibleMedication() {
      var content = document.getElementById("collapsibleContentMedication");
      var button = document.querySelector(".medication");

      // Toggle the content's visibility
      if (content.style.display === "block") {
        content.style.display = "none";
        button.classList.remove("active", "expanded");
        button.classList.add("collapsed");
      } else {
        content.style.display = "block";
        button.classList.add("active", "expanded");
        button.classList.remove("collapsed");
      }
    }

    function addInputField() {
      // Get the container div
      var container = document.getElementById('input-container');

      // Create a new input field
      var medicineCount = container.children.length + 1;
      var newInput = document.createElement('div');
      newInput.className = 'input-container';
      newInput.innerHTML = '<label for="medValue' + medicineCount + '">Medication ' + medicineCount + ':</label>' +
                           '<input type="text" id="medValue' + medicineCount + '" oninput="updateGraph()">';

      // Append the new input field to the container
      container.appendChild(newInput);
    }
  </script>
</body>

<!-- <style>
  #calculator-container {
    text-align: center;
    margin: 50px;
  }

  h2 {
    color: #333;
  }

  #calculator {
    width: 400px;
    margin: 0 auto;
    background-color: #f5f5f5;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }

  #display {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
    font-size: 1.5em;
    text-align: right;
  }

  button {
    width: 70px;
    height: 40px;
    font-size: 1em;
    margin: 5px;
    cursor: pointer;
    border: none;
    border-radius: 5px;
    background-color: #4caf50;
    color: #fff;
  }

  button:active {
    background-color: #45a049;
  }
</style>

<div id="calculator-container">
  <div id="calculator">
    <input type="text" id="display" readonly>
    <br>
    <button onclick="clearDisplay()">C</button>
    <button onclick="appendToDisplay('1')">1</button>
    <button onclick="appendToDisplay('2')">2</button>
    <button onclick="appendToDisplay('+')">+</button>
    <br>
    <button onclick="appendToDisplay('3')">3</button>
    <button onclick="appendToDisplay('4')">4</button>
    <button onclick="appendToDisplay('5')">5</button>
    <button onclick="appendToDisplay('-')">-</button>
    <br>
    <button onclick="appendToDisplay('6')">6</button>
    <button onclick="appendToDisplay('7')">7</button>
    <button onclick="appendToDisplay('8')">8</button>
    <button onclick="appendToDisplay('*')">*</button>
    <br>
    <button onclick="appendToDisplay('9')">9</button>
    <button onclick="appendToDisplay('0')">0</button>
    <button onclick="calculate()">=</button>
    <button onclick="appendToDisplay('/')">/</button>
  </div>
</div>

<script>
  let display = document.getElementById('display')
  let currentInput = ''
  let justEvaluated = false

  function appendToDisplay(value) {
    if (justEvaluated && /[0-9]/.test(value)) {
      currentInput = value;
    } else if (!justEvaluated && /[0-9]/.test(value)) {
      currentInput += value;
    } else if (['+', '-', '*', '/'].includes(value)) {
      currentInput += ` ${value} `;
    }
    justEvaluated = false
    display.value = currentInput
  }

  function clearDisplay() {
    currentInput = ''
    justEvaluated = false
    display.value = ''
  }

  function calculate() {
    try {
      display.value = eval(currentInput)
      currentInput = display.value
      justEvaluated = true
    } catch (error) {
      display.value = 'Error'
      currentInput = ''
    }
  }
</script> -->