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
            background: #fff;
            border-radius: 20px;
            box-shadow: 0 10px 50px rgba(0, 0, 0, 0.3);
            padding: 20px;
            width: 320px;
            overflow: hidden;
        }

        .display {
            margin-bottom: 20px;
        }

        #display {
            width: 100%;
            padding: 20px;
            font-size: 32px;
            text-align: right;
            border: 2px solid #667eea;
            border-radius: 10px;
            background: #f5f5f5;
            color: #333;
            font-weight: 500;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        .btn {
            padding: 20px;
            font-size: 20px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            background: #f0f0f0;
            color: #333;
            font-weight: 600;
            transition: all 0.2s ease;
        }

        .btn:hover {
            background: #e0e0e0;
            transform: scale(1.05);
        }

        .btn:active {
            transform: scale(0.95);
        }

        .btn.operator {
            background: #667eea;
            color: white;
        }

        .btn.operator:hover {
            background: #5568d3;
        }

        .btn.clear {
            background: #ff6b6b;
            color: white;
            grid-column: span 2;
        }

        .btn.clear:hover {
            background: #ee5a52;
        }

        .btn.delete {
            background: #ffa94d;
            color: white;
            grid-column: span 2;
        }

        .btn.delete:hover {
            background: #ff922b;
        }

        .btn.equals {
            background: #51cf66;
            color: white;
            grid-column: span 2;
        }

        .btn.equals:hover {
            background: #37b24d;
        }

        .btn.zero {
            grid-column: span 2;
        }

        @media (max-width: 400px) {
            .calculator {
                width: 100%;
                padding: 15px;
            }

            .btn {
                padding: 15px;
                font-size: 18px;
            }

            #display {
                font-size: 24px;
                padding: 15px;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display">
            <input type="text" id="display" value="0" readonly>
        </div>
        <div class="buttons">
            <button class="btn clear" onclick="clearDisplay()">C</button>
            <button class="btn operator" onclick="appendOperator('/')">/</button>
            <button class="btn operator" onclick="appendOperator('*')">*</button>
            <button class="btn delete" onclick="deleteLast()">DEL</button>
            
            <button class="btn" onclick="appendNumber('7')">7</button>
            <button class="btn" onclick="appendNumber('8')">8</button>
            <button class="btn" onclick="appendNumber('9')">9</button>
            <button class="btn operator" onclick="appendOperator('-')">-</button>
            
            <button class="btn" onclick="appendNumber('4')">4</button>
            <button class="btn" onclick="appendNumber('5')">5</button>
            <button class="btn" onclick="appendNumber('6')">6</button>
            <button class="btn operator" onclick="appendOperator('+')">+</button>
            
            <button class="btn" onclick="appendNumber('1')">1</button>
            <button class="btn" onclick="appendNumber('2')">2</button>
            <button class="btn" onclick="appendNumber('3')">3</button>
            <button class="btn operator" onclick="appendOperator('.')">.  </button>
            
            <button class="btn zero" onclick="appendNumber('0')">0</button>
            <button class="btn equals" onclick="calculate()">=</button>
        </div>
    </div>

    <script>
        const displayInput = document.getElementById('display');

        function appendNumber(num) {
            if (displayInput.value === '0' && num !== '.') {
                displayInput.value = num;
            } else if (num === '.' && displayInput.value.includes('.')) {
                return;
            } else {
                displayInput.value += num;
            }
        }

        function appendOperator(op) {
            const lastChar = displayInput.value[displayInput.value.length - 1];
            
            if (['+', '-', '*', '/'].includes(lastChar)) {
                return;
            }
            
            if (displayInput.value === '') {
                return;
            }
            
            displayInput.value += op;
        }

        function clearDisplay() {
            displayInput.value = '0';
        }

        function deleteLast() {
            if (displayInput.value.length === 1) {
                displayInput.value = '0';
            } else {
                displayInput.value = displayInput.value.slice(0, -1);
            }
        }

        function calculate() {
            try {
                const result = eval(displayInput.value);
                displayInput.value = result;
            } catch (error) {
                displayInput.value = 'Error';
                setTimeout(() => {
                    displayInput.value = '0';
                }, 1500);
            }
        }

        // Keyboard support
        document.addEventListener('keydown', (e) => {
            if (e.key >= '0' && e.key <= '9') {
                appendNumber(e.key);
            } else if (['+', '-', '*', '/'].includes(e.key)) {
                appendOperator(e.key);
            } else if (e.key === '.') {
                appendNumber('.');
            } else if (e.key === 'Enter' || e.key === '=') {
                calculate();
            } else if (e.key === 'Backspace') {
                deleteLast();
            } else if (e.key === 'c' || e.key === 'C') {
                clearDisplay();
            }
        });
    </script>
</body>
</html>
