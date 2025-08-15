# temparature-converter-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Temperature Converter</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            padding: 30px;
            width: 100%;
            max-width: 500px;
            text-align: center;
            transition: all 0.3s ease;
        }
        
        h1 {
            color: #3a4a6d;
            margin-bottom: 25px;
            font-weight: 600;
            position: relative;
            padding-bottom: 10px;
        }
        
        h1::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 3px;
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
        }
        
        .input-group {
            margin-bottom: 25px;
            text-align: left;
            width: 100%;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            color: #4a5568;
            font-weight: 500;
        }
        
        input[type="number"] {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e2e8f0;
            border-radius: 8px;
            font-size: 16px;
            transition: all 0.3s ease;
            margin-bottom: 10px;
        }
        
        input[type="number"]:focus {
            border-color: #667eea;
            outline: none;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.2);
        }
        
        .radio-group {
            display: flex;
            gap: 20px;
            margin-bottom: 25px;
        }
        
        .radio-option {
            display: flex;
            align-items: center;
            cursor: pointer;
        }
        
        .radio-option input {
            margin-right: 8px;
            width: 18px;
            height: 18px;
            cursor: pointer;
        }
        
        .btn-convert {
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
            margin-bottom: 25px;
        }
        
        .btn-convert:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }
        
        .result-container {
            background-color: #f8fafc;
            border-radius: 8px;
            padding: 20px;
            border: 1px dashed #cbd5e0;
            display: none;
        }
        
        .result-container.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }
        
        .result-value {
            font-size: 24px;
            font-weight: 600;
            color: #3a4a6d;
            margin-bottom: 5px;
        }
        
        .result-unit {
            color: #718096;
            font-size: 16px;
        }
        
        .error-message {
            color: #e53e3e;
            font-size: 14px;
            margin-top: 5px;
            display: none;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .logo {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .logo-img {
            display: inline-block;
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 40px;
            font-weight: bold;
            margin-bottom: 15px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="logo">
            <div class="logo-img">°</div>
            <h1>Temperature Converter</h1>
        </div>
        
        <div class="input-group">
            <label for="temperature">Enter Temperature:</label>
            <input type="number" id="temperature" placeholder="e.g. 32" step="0.01">
            <div id="temperature-error" class="error-message">Please enter a valid number</div>
        </div>
        
        <div class="input-group">
            <label>Temperature Unit:</label>
            <div class="radio-group">
                <div class="radio-option">
                    <input type="radio" id="celsius" name="unit" value="celsius" checked>
                    <label for="celsius">Celsius (°C)</label>
                </div>
                <div class="radio-option">
                    <input type="radio" id="fahrenheit" name="unit" value="fahrenheit">
                    <label for="fahrenheit">Fahrenheit (°F)</label>
                </div>
            </div>
        </div>
        
        <button id="convert-btn" class="btn-convert">Convert</button>
        
        <div id="result" class="result-container">
            <div class="result-value" id="result-value">0</div>
            <div class="result-unit" id="result-unit">°C</div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const temperatureInput = document.getElementById('temperature');
            const convertBtn = document.getElementById('convert-btn');
            const resultContainer = document.getElementById('result');
            const resultValue = document.getElementById('result-value');
            const resultUnit = document.getElementById('result-unit');
            const errorMessage = document.getElementById('temperature-error');
            const unitRadios = document.querySelectorAll('input[name="unit"]');
            
            function validateInput() {
                const value = temperatureInput.value.trim();
                if (value === '' || isNaN(value)) {
                    temperatureInput.style.borderColor = '#e53e3e';
                    errorMessage.style.display = 'block';
                    return false;
                } else {
                    temperatureInput.style.borderColor = '#e2e8f0';
                    errorMessage.style.display = 'none';
                    return true;
                }
            }
            
            function convertTemperature() {
                if (!validateInput()) return;
                
                const temperature = parseFloat(temperatureInput.value);
                let convertedTemp, newUnit;
                
                const isCelsiusSelected = document.getElementById('celsius').checked;
                
                if (isCelsiusSelected) {
                    // Convert Celsius to Fahrenheit
                    convertedTemp = (temperature * 9/5) + 32;
                    newUnit = '°F';
                } else {
                    // Convert Fahrenheit to Celsius
                    convertedTemp = (temperature - 32) * 5/9;
                    newUnit = '°C';
                }
                
                // Round to 2 decimal places
                convertedTemp = Math.round(convertedTemp * 100) / 100;
                
                resultValue.textContent = convertedTemp;
                resultUnit.textContent = newUnit;
                
                if (!resultContainer.classList.contains('active')) {
                    resultContainer.classList.add('active');
                }
            }
            
            temperatureInput.addEventListener('input', validateInput);
            convertBtn.addEventListener('click', convertTemperature);
            
            // Also allow conversion on Enter key press
            temperatureInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    convertTemperature();
                }
            });
            
            // Animate container on load
            setTimeout(() => {
                document.querySelector('.container').style.opacity = 1;
                document.querySelector('.container').style.transform = 'translateY(0)';
            }, 100);
        });
    </script>
</body>
</html>

