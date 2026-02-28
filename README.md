# calculator
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        .calculator {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            padding: 20px;
            width: 320px;
        }

        .display {
            background: #333;
            color: #fff;
            font-size: 32px;
            padding: 20px;
            border-radius: 10px;
            text-align: right;
            margin-bottom: 20px;
            word-wrap: break-word;
            word-break: break-all;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: flex-end;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            padding: 20px;
            font-size: 20px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s;
            font-weight: 600;
        }

        button:hover {
            transform: scale(1.05);
        }

        button:active {
            transform: scale(0.98);
        }

        .number, .decimal {
            background: #f0f0f0;
            color: #333;
        }

        .number:hover, .decimal:hover {
            background: #e0e0e0;
        }

        .operator {
            background: #667eea;
            color: white;
        }

        .operator:hover {
            background: #5568d3;
        }

        .equals {
            background: #48bb78;
            color: white;
            grid-column: span 2;
        }

        .equals:hover {
            background: #38a169;
        }

        .clear {
            background: #f56565;
            color: white;
            grid-column: span 2;
        }

        .clear:hover {
            background: #e53e3e;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="operator" onclick="appendOperator('/')">/</button>
            <button class="operator" onclick="appendOperator('*')">*</button>
            
            <button class="number" onclick="appendNumber('7')">7</button>
            <button class="number" onclick="appendNumber('8')">8</button>
            <button class="number" onclick="appendNumber('9')">9</button>
            <button class="operator" onclick="appendOperator('-')">-</button>
            
            <button class="number" onclick="appendNumber('4')">4</button>
            <button class="number" onclick="appendNumber('5')">5</button>
            <button class="number" onclick="appendNumber('6')">6</button>
            <button class="operator" onclick="appendOperator('+')">+</button>
            
            <button class="number" onclick="appendNumber('1')">1</button>
            <button class="number" onclick="appendNumber('2')">2</button>
            <button class="number" onclick="appendNumber('3')">3</button>
            <button class="operator" onclick="appendOperator('=')">=</button>
            
            <button class="number" onclick="appendNumber('0')" style="grid-column: span 2;">0</button>
            <button class="decimal" onclick="appendDecimal()">.</button>
            <button class="operator" onclick="deleteLast()">←</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let currentInput = '0';
        let previousInput = ''; 
        let operator = null;
        let shouldResetDisplay = false;

        function updateDisplay() {
            display.textContent = currentInput;
        }

        function appendNumber(number) {
            if (shouldResetDisplay) {
                currentInput = number;
                shouldResetDisplay = false;
            } else {
                if (currentInput === '0') {
                    currentInput = number;
                } else {
                    currentInput += number;
                }
            }
            updateDisplay();
        }

        function appendDecimal() {
            if (shouldResetDisplay) {
                currentInput = '0.';
                shouldResetDisplay = false;
            } else if (!currentInput.includes('.')) {
                currentInput += '.';
            }
            updateDisplay();
        }

        function appendOperator(op) {
            if (op === '=') {
                calculate();
            } else {
                if (operator !== null) {
                    calculate();
                }
                previousInput = currentInput;
                operator = op;
                shouldResetDisplay = true;
            }
        }

        function calculate() {
            if (operator === null || shouldResetDisplay) {
                return;
            }

            let result;
            const prev = parseFloat(previousInput);
            const current = parseFloat(currentInput);

            switch (operator) {
                case '+':
                    result = prev + current;
                    break;
                case '-':
                    result = prev - current;
                    break;
                case '*':
                    result = prev * current;
                    break;
                case '/':
                    result = current !== 0 ? prev / current : 'Error';
                    break;
                default:
                    return;
            }

            currentInput = result.toString();
            operator = null;
            shouldResetDisplay = true;
            updateDisplay();
        }

        function clearDisplay() {
            currentInput = '0';
            previousInput = '';
            operator = null;
            shouldResetDisplay = false;
            updateDisplay();
        }

        function deleteLast() {
            if (currentInput.length > 1) {
                currentInput = currentInput.slice(0, -1);
            } else {
                currentInput = '0';
            }
            updateDisplay();
        }
    </script>
</body>
</html>
