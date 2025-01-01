# Estimate.github.io
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Calculadora de Precios</title>
    <style>
        * {
            -webkit-tap-highlight-color: transparent;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            margin: 0;
            padding: 16px;
            background-color: #f5f5f5;
            -webkit-text-size-adjust: 100%;
        }

        .calculator {
            background-color: white;
            padding: 16px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            max-width: 100%;
            margin: 0 auto;
        }

        h2 {
            margin: 0 0 20px 0;
            font-size: 1.5rem;
            color: #1c1c1e;
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            font-size: 0.9rem;
            color: #1c1c1e;
        }

        input {
            width: 100%;
            padding: 12px;
            border: 1px solid #d1d1d6;
            border-radius: 10px;
            box-sizing: border-box;
            font-size: 1.1rem;
            -webkit-appearance: none;
            appearance: none;
            background-color: #f5f5f5;
            color: #1c1c1e;
        }

        input:focus {
            outline: none;
            border-color: #007aff;
            box-shadow: 0 0 0 2px rgba(0,122,255,0.2);
        }

        .result {
            margin-top: 24px;
            padding: 16px;
            background-color: #f5f5f5;
            border-radius: 12px;
        }

        .result-line {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            font-size: 0.95rem;
        }

        .total {
            margin-top: 12px;
            padding-top: 12px;
            border-top: 2px solid #d1d1d6;
            font-weight: 700;
            font-size: 1.1rem;
            color: #007aff;
        }

        @supports (-webkit-touch-callout: none) {
            input {
                font-size: 16px;
            }
            
            .calculator {
                margin-bottom: 40px;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <h2>Calculadora de Precios</h2>
        
        <div class="input-group">
            <label for="originalPrice">Precio Original ($)</label>
            <input type="number" id="originalPrice" step="0.01" value="0" inputmode="decimal">
        </div>

        <div class="input-group">
            <label for="taxes">Impuestos (%)</label>
            <input type="number" id="taxes" step="0.01" value="0" inputmode="decimal">
        </div>

        <div class="input-group">
            <label for="viviTaxes">Vivi Impuestos (%)</label>
            <input type="number" id="viviTaxes" step="0.01" value="0" inputmode="decimal">
        </div>

        <div class="input-group">
            <label for="shipping">Envío ($)</label>
            <input type="number" id="shipping" step="0.01" value="0" inputmode="decimal">
        </div>

        <div class="result">
            <div class="result-line">
                <span>Precio Original:</span>
                <span id="showOriginalPrice">$0.00</span>
            </div>
            <div class="result-line">
                <span>Monto de Impuestos:</span>
                <span id="showTaxesAmount">$0.00</span>
            </div>
            <div class="result-line">
                <span>Monto de Vivi Impuestos:</span>
                <span id="showViviTaxesAmount">$0.00</span>
            </div>
            <div class="result-line">
                <span>Subtotal (× 21):</span>
                <span id="showSubtotal">$0.00</span>
            </div>
            <div class="result-line">
                <span>Envío:</span>
                <span id="showShipping">$0.00</span>
            </div>
            <div class="result-line total">
                <span>Total Final:</span>
                <span id="showTotal">$0.00</span>
            </div>
        </div>
    </div>

    <script>
        const formatCurrency = (number) => {
            return new Intl.NumberFormat('en-US', {
                style: 'currency',
                currency: 'USD'
            }).format(number);
        };

        const calculateAll = () => {
            const originalPrice = parseFloat(document.getElementById('originalPrice').value) || 0;
            const taxes = parseFloat(document.getElementById('taxes').value) || 0;
            const viviTaxes = parseFloat(document.getElementById('viviTaxes').value) || 0;
            const shipping = parseFloat(document.getElementById('shipping').value) || 0;

            const taxesAmount = originalPrice * (taxes / 100);
            const priceWithTaxes = originalPrice + taxesAmount;
            const viviTaxesAmount = priceWithTaxes * (viviTaxes / 100);
            const priceWithAllTaxes = priceWithTaxes + viviTaxesAmount;
            const subtotal = priceWithAllTaxes * 21;
            const total = subtotal + shipping;

            document.getElementById('showOriginalPrice').textContent = formatCurrency(originalPrice);
            document.getElementById('showTaxesAmount').textContent = formatCurrency(taxesAmount);
            document.getElementById('showViviTaxesAmount').textContent = formatCurrency(viviTaxesAmount);
            document.getElementById('showSubtotal').textContent = formatCurrency(subtotal);
            document.getElementById('showShipping').textContent = formatCurrency(shipping);
            document.getElementById('showTotal').textContent = formatCurrency(total);
        };

        // Event listeners
        const inputs = ['originalPrice', 'taxes', 'viviTaxes', 'shipping'];
        inputs.forEach(id => {
            const input = document.getElementById(id);
            input.addEventListener('input', calculateAll);
            input.addEventListener('focus', function() {
                setTimeout(() => {
                    this.scrollIntoView({ behavior: 'smooth', block: 'center' });
                }, 300);
            });
        });

        calculateAll();
    </script>
</body>
</html>