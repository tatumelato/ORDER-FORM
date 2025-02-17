<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Restaurant Order Form</title>
    <style>
        :root {
            --primary-red: #ff1a1a;
            --light-red: #ffeded;
        }

        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: var(--light-red);
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        h1 {
            color: var(--primary-red);
            text-align: center;
            margin-bottom: 30px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            color: var(--primary-red);
            font-weight: bold;
        }

        select, input, textarea {
            width: 100%;
            padding: 10px;
            border: 2px solid var(--primary-red);
            border-radius: 5px;
            margin-top: 5px;
        }

        .price-input {
            width: calc(100% - 24px);
            padding-left: 20px;
        }

        .price-wrapper {
            position: relative;
        }

        .currency-symbol {
            position: absolute;
            left: 10px;
            top: 50%;
            transform: translateY(-50%);
            color: #666;
        }

        .button-3d {
            background: var(--primary-red);
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            transform: translateY(0);
            transition: all 0.2s;
            box-shadow: 0 4px 6px rgba(255, 26, 26, 0.2);
            width: 100%;
            margin-bottom: 10px;
        }

        .button-3d:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 8px rgba(255, 26, 26, 0.3);
        }

        .button-3d:active {
            transform: translateY(1px);
            box-shadow: 0 2px 4px rgba(255, 26, 26, 0.2);
        }

        .confirmation-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            width: 90%;
            max-width: 400px;
        }

        .confirmation-buttons {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-top: 20px;
        }

        .confirmation-buttons button {
            width: 120px;
        }

        .meal-inputs {
            display: flex;
            gap: 10px;
        }

        .meal-name {
            flex: 2;
        }

        .meal-price {
            flex: 1;
        }

        .order-summary {
            background-color: #f8f8f8;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
        }

        @media (max-width: 480px) {
            .meal-inputs {
                flex-direction: column;
                gap: 5px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Place Your Order</h1>
        <form id="orderForm">
            <div class="form-group">
                <label>Customer Name:</label>
                <input type="text" id="customerName" required>
            </div>

            <div class="form-group">
                <label>Meal Details:</label>
                <div class="meal-inputs">
                    <div class="meal-name">
                        <input type="text" id="mealName" placeholder="Enter meal name" required>
                    </div>
                    <div class="meal-price">
                        <div class="price-wrapper">
                            <span class="currency-symbol">R</span>
                            <input type="number" id="mealPrice" class="price-input" placeholder="Price" step="0.01" required>
                        </div>
                    </div>
                </div>
            </div>

            <div class="form-group">
                <label>Service Type:</label>
                <select id="serviceType" required>
                    <option value="">Select service type...</option>
                    <option value="delivery">Delivery</option>
                    <option value="collection">Collection</option>
                    <option value="eatin">Eat In</option>
                </select>
            </div>

            <div id="addressSection" class="form-group" style="display: none;">
                <label for="address">Delivery Address:</label>
                <textarea id="address" rows="3"></textarea>
            </div>

            <div id="timeSection" class="form-group" style="display: none;">
                <label for="time">Preferred Time:</label>
                <input type="time" id="time">
            </div>

            <div id="eggSection" class="form-group">
                <label for="eggType">Egg Preference (Optional):</label>
                <select id="eggType">
                    <option value="">Select egg type...</option>
                    <option value="scrambled">Scrambled</option>
                    <option value="fried">Fried</option>
                    <option value="poached">Poached</option>
                </select>
            </div>

            <div class="form-group">
                <label>Payment Method:</label>
                <select id="paymentMethod" required>
                    <option value="">Select payment method...</option>
                    <option value="cash">Cash</option>
                    <option value="card">Card</option>
                </select>
            </div>

            <div id="changeSection" class="form-group" style="display: none;">
                <label for="cashAmount">Cash Amount (R):</label>
                <div class="price-wrapper">
                    <span class="currency-symbol">R</span>
                    <input type="number" id="cashAmount" class="price-input" step="0.01">
                </div>
                <p id="changeAmount" style="color: var(--primary-red);"></p>
            </div>

            <button type="submit" class="button-3d">Review Order</button>
        </form>
    </div>

    <div class="confirmation-modal" id="confirmationModal">
        <div class="modal-content">
            <h2>Order Summary</h2>
            <div class="order-summary" id="orderSummary"></div>
            <p>Would you like to submit this order?</p>
            <div class="confirmation-buttons">
                <button class="button-3d" onclick="submitOrder('yes')">YES</button>
                <button class="button-3d" onclick="submitOrder('no')" style="background-color: #666;">NO</button>
            </div>
        </div>
    </div>

    <script>
        const form = document.getElementById('orderForm');
        const mealPrice = document.getElementById('mealPrice');
        const serviceType = document.getElementById('serviceType');
        const addressSection = document.getElementById('addressSection');
        const timeSection = document.getElementById('timeSection');
        const paymentMethod = document.getElementById('paymentMethod');
        const changeSection = document.getElementById('changeSection');
        const cashAmount = document.getElementById('cashAmount');
        const changeAmount = document.getElementById('changeAmount');
        const modal = document.getElementById('confirmationModal');
        
        let orderNumber = Math.floor(Math.random() * 9000) + 1000;

        // Show/hide sections based on selections
        serviceType.addEventListener('change', () => {
            addressSection.style.display = serviceType.value === 'delivery' ? 'block' : 'none';
            timeSection.style.display = ['collection', 'eatin'].includes(serviceType.value) ? 'block' : 'none';
        });

        paymentMethod.addEventListener('change', () => {
            changeSection.style.display = paymentMethod.value === 'cash' ? 'block' : 'none';
        });

        // Calculate change
        cashAmount.addEventListener('input', () => {
            const price = parseFloat(mealPrice.value);
            const cash = parseFloat(cashAmount.value);
            if (!isNaN(cash) && !isNaN(price)) {
                const change = cash - price;
                changeAmount.textContent = `Change: R${change.toFixed(2)}`;
            }
        });
        
        form.addEventListener('submit', (e) => {
            e.preventDefault();
            displayOrderSummary();
            modal.style.display = 'flex';
        });

        function displayOrderSummary() {
            const customerName = document.getElementById('customerName').value;
            const mealName = document.getElementById('mealName').value;
            const mealPriceValue = document.getElementById('mealPrice').value;
            const serviceTypeText = serviceType.value;
            const addressText = document.getElementById('address').value;
            const timeText = document.getElementById('time').value;
            const eggTypeText = document.getElementById('eggType').value;
            const paymentMethodText = paymentMethod.value;
            const cashAmountText = cashAmount.value;

            const summaryHTML = `
                <p><strong>Order #${orderNumber}</strong></p>
                <p><strong>Customer:</strong> ${customerName}</p>
                <p><strong>Meal:</strong> ${mealName}</p>
                <p><strong>Price:</strong> R${mealPriceValue}</p>
                <p><strong>Service:</strong> ${serviceTypeText}</p>
                ${serviceTypeText === 'delivery' ? `<p><strong>Address:</strong> ${addressText}</p>` : ''}
                ${['collection', 'eatin'].includes(serviceTypeText) ? `<p><strong>Time:</strong> ${timeText}</p>` : ''}
                ${eggTypeText ? `<p><strong>Egg Preference:</strong> ${eggTypeText}</p>` : ''}
                <p><strong>Payment:</strong> ${paymentMethodText}</p>
                ${paymentMethodText === 'cash' ? `
                    <p><strong>Cash Amount:</strong> R${cashAmountText}</p>
                    <p><strong>Change Required:</strong> R${(cashAmountText - parseFloat(mealPriceValue)).toFixed(2)}</p>
                ` : ''}
            `;

            document.getElementById('orderSummary').innerHTML = summaryHTML;
        }

        function submitOrder(choice) {
            if (choice === 'yes') {
                const customerName = document.getElementById('customerName').value;
                const mealName = document.getElementById('mealName').value;
                const mealPriceValue = document.getElementById('mealPrice').value;
                const serviceTypeText = serviceType.value;
                const addressText = document.getElementById('address').value;
                const timeText = document.getElementById('time').value;
                const eggTypeText = document.getElementById('eggType').value;
                const paymentMethodText = paymentMethod.value;
                const cashAmountText = cashAmount.value;

                const message = `New Order #${orderNumber}:\n
Customer: ${customerName}
Meal: ${mealName}
Price: R${mealPriceValue}
Service: ${serviceTypeText}
${serviceTypeText === 'delivery' ? `Address: ${addressText}` : ''}
${['collection', 'eatin'].includes(serviceTypeText) ? `Time: ${timeText}` : ''}
${eggTypeText ? `Egg Preference: ${eggTypeText}` : ''}
Payment: ${paymentMethodText}
${paymentMethodText === 'cash' ? `Cash Amount: R${cashAmountText}
Change Required: R${(cashAmountText - parseFloat(mealPriceValue)).toFixed(2)}` : ''}`;

                const encodedMessage = encodeURIComponent(message);
                window.open(`https://wa.me/27625928143?text=${encodedMessage}`, '_blank');
                
                // Reset form and generate new order number
                form.reset();
                orderNumber = Math.floor(Math.random() * 9000) + 1000;
            }
            
            modal.style.display = 'none';
        }
    </script>
</body>
</html>
