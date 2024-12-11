<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kalkulator Sederhana</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f3f4f6;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .calculator {
      background-color: #ffffff;
      border-radius: 15px;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
      padding: 20px;
      width: 260px;
      transition: background-color 0.3s;
    }

    .display {
      width: 100%;
      height: 50px;
      text-align: right;
      background-color: #f8f9fa;
      border: 1px solid #ddd;
      border-radius: 10px;
      font-size: 1.8em;
      padding: 10px;
      box-sizing: border-box;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .button-row {
      display: flex;
      flex-wrap: wrap;
      margin-top: 10px;
    }

    .button {
      width: 55px;
      height: 55px;
      background-color: #f1f1f1;
      border: 1px solid #ddd;
      border-radius: 10px;
      font-size: 1.8em;
      margin: 5px;
      cursor: pointer;
      transition: background-color 0.2s;
    }

    .button:hover {
      background-color: #e0e0e0;
    }

    .button.operator {
      background-color: #ff9f00;
      color: white;
    }

    .button.operator:hover {
      background-color: #e68900;
    }

    .button.equal {
      background-color: #4CAF50;
      color: white;
      flex-grow: 2;
    }

    .button.equal:hover {
      background-color: #45a049;
    }

    .button.delete, .button.clear {
      background-color: #607d8b;
      color: white;
      font-size: 1.2em;
      width: 55px; /* Ukuran kembali ke semula */
      height: 55px; /* Ukuran kembali ke semula */
    }

    .button.delete:hover, .button.clear:hover {
      background-color: #455a64;
    }

    .button-zero {
      flex-grow: 2;
    }

    .color-picker {
      display: flex;
      justify-content: space-evenly;
      margin-top: 10px;
    }

    .color-btn {
      width: 30px;
      height: 30px;
      border-radius: 50%;
      border: none;
      cursor: pointer;
      transition: transform 0.3s;
    }

    .color-btn:hover {
      transform: scale(1.2);
    }

  </style>
</head>
<body>

<div class="calculator" id="calculator">
  <input id="display" class="display" type="text" readonly />
  <div class="button-row">
    <button class="button clear" onclick="clearDisplay()">C</button>
    <button class="button delete" onclick="deleteLast()">DEL</button>
    <button class="button operator" onclick="appendToDisplay('+')">+</button>
  </div>
  <div class="button-row">
    <button class="button" onclick="appendToDisplay('1')">1</button>
    <button class="button" onclick="appendToDisplay('2')">2</button>
    <button class="button" onclick="appendToDisplay('3')">3</button>
    <button class="button operator" onclick="appendToDisplay('-')">-</button>
  </div>
  <div class="button-row">
    <button class="button" onclick="appendToDisplay('4')">4</button>
    <button class="button" onclick="appendToDisplay('5')">5</button>
    <button class="button" onclick="appendToDisplay('6')">6</button>
    <button class="button operator" onclick="appendToDisplay('x')">x</button>
  </div>
  <div class="button-row">
    <button class="button" onclick="appendToDisplay('7')">7</button>
    <button class="button" onclick="appendToDisplay('8')">8</button>
    <button class="button" onclick="appendToDisplay('9')">9</button>
    <button class="button operator" onclick="appendToDisplay('/')">/</button>
  </div>
  <div class="button-row">
    <button class="button button-zero" onclick="appendToDisplay('0')">0</button>
    <button class="button" onclick="appendToDisplay('.')">.</button>
    <button class="button equal" onclick="calculate()">=</button>
  </div>
  <div class="color-picker">
    <button class="color-btn" style="background-color: pink;" onclick="changeCalculatorColor('pink')"></button>
    <button class="color-btn" style="background-color: green;" onclick="changeCalculatorColor('green')"></button>
    <button class="color-btn" style="background-color: blue;" onclick="changeCalculatorColor('blue')"></button>
    <button class="color-btn" style="background-color: orange;" onclick="changeCalculatorColor('orange')"></button>
    <button class="color-btn" style="background-color: black;" onclick="changeCalculatorColor('black')"></button>
    <button class="color-btn" style="background-color: #B0B0B0;" onclick="changeCalculatorColor('cloudWhite')"></button> <!-- Putih Mendung -->
  </div>
</div>

<script>
  function appendToDisplay(value) {
    let display = document.getElementById("display");
    const lastChar = display.value.charAt(display.value.length - 1);

    // Jika tombol yang ditekan adalah operator
    if (value === "+" || value === "-" || value === "x" || value === "/") {
      // Jika karakter terakhir adalah operator dan yang ditekan operator yang sama
      if (lastChar === value) {
        return;
      }
      
      // Jika karakter terakhir adalah operator, ganti dengan operator baru
      if (lastChar === "+" || lastChar === "-" || lastChar === "x" || lastChar === "/") {
        display.value = display.value.slice(0, -1); // Hapus operator terakhir
      }
    }
    display.value += value;
  }

  function clearDisplay() {
    document.getElementById("display").value = '';
  }

  function deleteLast() {
    let display = document.getElementById("display");
    display.value = display.value.slice(0, -1);
  }

  function calculate() {
    let display = document.getElementById("display");
    let expression = display.value.replace(/x/g, '*'); // Ganti 'x' dengan '*' untuk operasi perkalian
    try {
      display.value = eval(expression);
    } catch {
      display.value = 'Yang bener aja goblok!'; // Pesan kesalahan
      display.style.fontSize = '1em'; // Ukuran font lebih kecil untuk menyesuaikan teks
    }
  }

  function changeCalculatorColor(color) {
    let calculator = document.getElementById("calculator");
    switch (color) {
      case "pink":
        calculator.style.backgroundColor = "#ff66b2";
        break;
      case "green":
        calculator.style.backgroundColor = "#4CAF50";
        break;
      case "blue":
        calculator.style.backgroundColor = "#2196F3";
        break;
      case "orange":
        calculator.style.backgroundColor = "#FF5722";
        break;
      case "black":
        calculator.style.backgroundColor = "#333333";
        break;
      case "cloudWhite":
        calculator.style.backgroundColor = "#B0B0B0"; // Putih mendung
        break;
    }
  }
</script>

</body>
</html>
