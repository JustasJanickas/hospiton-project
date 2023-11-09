---
permalink: /about/
title: "About"
---

Tempor velit sint sunt ipsum tempor enim ad qui ullamco. Est dolore anim ad velit duis dolore minim sunt aliquip amet commodo labore. Ut eu pariatur aute ea aute excepteur laborum. Esse ea esse excepteur minim mollit qui cillum excepteur ex dolore magna. Labore deserunt fugiat incididunt incididunt sint ea. Consequat dolore aute laboris quis proident quis non et est consectetur ex eiusmod sit culpa.

Cupidatat ea do et in excepteur in. Ad nostrud ut est esse eu duis ea sunt eiusmod. Aliquip tempor veniam sint elit fugiat. Velit incididunt laboris amet incididunt labore dolore irure velit excepteur commodo deserunt laborum. Consectetur eu fugiat veniam veniam Lorem labore magna eiusmod. Ea occaecat reprehenderit pariatur consectetur minim labore ut aliquip.

<style>
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
    <h2>Simple Calculator</h2>
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
</script>