html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Laundromat Scheduling</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
        }
        h1, h2 {
            color: #333;
        }
        .section {
            background: white;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
        }
        input, select, textarea {
            width: 100%;
            padding: 8px;
            box-sizing: border-box;
        }
        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            padding: 10px;
            border: 1px solid #ddd;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        .total {
            font-weight: bold;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Welcome to My Laundromat</h1>

    <!-- Customer Pickup Scheduling Section -->
    <div class="section">
        <h2>Schedule a Pickup</h2>
        <div class="form-group">
            <label for="pickup-date">Pick a Date:</label>
            <input type="date" id="pickup-date" required>
        </div>
        <div class="form-group">
            <label for="pickup-time">Pick a Time:</label>
            <input type="time" id="pickup-time" required>
        </div>
        <div class="form-group">
            <label for="customer-name">Your Name:</label>
            <input type="text" id="customer-name" required>
        </div>
        <div class="form-group">
            <label for="customer-phone">Your Phone Number:</label>
            <input type="tel" id="customer-phone" required>
        </div>
        <button onclick="schedulePickup()">Schedule Pickup</button>
    </div>

    <!-- Employee Order Management Section -->
    <div class="section">
        <h2>Employee Order Log</h2>
        <div class="form-group">
            <label for="emp-name">Customer Name:</label>
            <input type="text" id="emp-name">
        </div>
        <div class="form-group">
            <label for="emp-phone">Phone Number:</label>
            <input type="tel" id="emp-phone">
        </div>
        <div class="form-group">
            <label for="emp-payment">Payment Method:</label>
            <select id="emp-payment">
                <option value="cash">Cash</option>
                <option value="cashapp">CashApp</option>
            </select>
        </div>
        <div class="form-group">
            <label for="emp-balance">Balance Owed (if any):</label>
            <input type="number" id="emp-balance" step="0.01" min="0" value="0">
        </div>
        <div class="form-group">
            <label for="emp-dropoff">Drop-off Time:</label>
            <input type="time" id="emp-dropoff">
        </div>
        <div class="form-group">
            <label for="emp-washer">Washer Machine #:</label>
            <input type="text" id="emp-washer">
        </div>
        <div class="form-group">
            <label for="emp-washer-cost">Washer Cost ($):</label>
            <input type="number" id="emp-washer-cost" step="0.01" min="0" value="0">
        </div>
        <div class="form-group">
            <label for="emp-washer-start">Washer Start Time:</label>
            <input type="time" id="emp-washer-start">
        </div>
        <div class="form-group">
            <label for="emp-dryer">Dryer Machine #:</label>
            <input type="text" id="emp-dryer">
        </div>
        <div class="form-group">
            <label for="emp-dryer-cost">Dryer Cost ($):</label>
            <input type="number" id="emp-dryer-cost" step="0.01" min="0" value="0">
        </div>
        <div class="form-group">
            <label for="emp-dryer-start">Dryer Start Time:</label>
            <input type="time" id="emp-dryer-start">
        </div>
        <div class="form-group">
            <label for="emp-notes">Special Notes:</label>
            <textarea id="emp-notes" rows="3"></textarea>
        </div>
        <button onclick="addOrder()">Add Order</button>

        <!-- Employee Order Table -->
        <table id="order-table">
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Phone</th>
                    <th>Payment</th>
                    <th>Balance</th>
                    <th>Drop-off</th>
                    <th>Washer #</th>
                    <th>Washer Cost</th>
                    <th>Washer Start</th>
                    <th>Dryer #</th>
                    <th>Dryer Cost</th>
                    <th>Dryer Start</th>
                    <th>Notes</th>
                    <th>Total</th>
                </tr>
            </thead>
            <tbody id="order-table-body"></tbody>
        </table>
        <div class="total" id="grand-total">Grand Total: $0.00</div>
    </div>

    <script>
        // Customer Pickup Scheduling
        function schedulePickup() {
            const date = document.getElementById('pickup-date').value;
            const time = document.getElementById('pickup-time').value;
            const name = document.getElementById('customer-name').value;
            const phone = document.getElementById('customer-phone').value;

            if (!date || !time || !name || !phone) {
                alert("Please fill out all fields!");
                return;
            }

            const subject = `Pickup Request from ${name}`;
            const body = `Name: ${name}\nPhone: ${phone}\nPickup Date: ${date}\nPickup Time: ${time}`;
            const mailtoLink = `mailto:your-email@example.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
            window.location.href = mailtoLink;
        }

        // Employee Order Management
        let grandTotal = 0;

        function addOrder() {
            const name = document.getElementById('emp-name').value;
            const phone = document.getElementById('emp-phone').value;
            const payment = document.getElementById('emp-payment').value;
            const balance = parseFloat(document.getElementById('emp-balance').value) || 0;
            const dropoff = document.getElementById('emp-dropoff').value;
            const washer = document.getElementById('emp-washer').value;
            const washerCost = parseFloat(document.getElementById('emp-washer-cost').value) || 0;
            const washerStart = document.getElementById('emp-washer-start').value;
            const dryer = document.getElementById('emp-dryer').value;
            const dryerCost = parseFloat(document.getElementById('emp-dryer-cost').value) || 0;
            const dryerStart = document.getElementById('emp-dryer-start').value;
            const notes = document.getElementById('emp-notes').value;

            if (!name || !phone || !dropoff) {
                alert("Please fill out at least the name, phone, and drop-off time!");
                return;
            }

            const total = washerCost + dryerCost;
            grandTotal += total;

            const tableBody = document.getElementById('order-table-body');
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${name}</td>
                <td>${phone}</td>
                <td>${payment}</td>
                <td>$${balance.toFixed(2)}</td>
                <td>${dropoff}</td>
                <td>${washer}</td>
                <td>$${washerCost.toFixed(2)}</td>
                <td>${washerStart}</td>
                <td>${dryer}</td>
                <td>$${dryerCost.toFixed(2)}</td>
                <td>${dryerStart}</td>
                <td>${notes}</td>
                <td>$${total.toFixed(2)}</td>
            `;
            tableBody.appendChild(row);

            document.getElementById('grand-total').textContent = `Grand Total: $${grandTotal.toFixed(2)}`;

            // Clear the form
            document.querySelectorAll('#emp-name, #emp-phone, #emp-balance, #emp-dropoff, #emp-washer, #emp-washer-cost, #emp-washer-start, #emp-dryer, #emp-dryer-cost, #emp-dryer-start, #emp-notes')
                .forEach(input => input.value = '');
            document.getElementById('emp-balance').value = '0';
            document.getElementById('emp-washer-cost').value = '0';
            document.getElementById('emp-dryer-cost').value = '0';
        }
    </script>
</body>
</html>
