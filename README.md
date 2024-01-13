<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Budget Sheet</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 20px;
            text-align: center;
            background-color: #17202A; /* Dark Blue Background */
            color: #fff;
        }

        h1, h2, label, p, li, .remainder, .instructions {
            color: #fff;
        }

        input {
            padding: 10px;
            width: 200px;
            margin-bottom: 20px;
            border: 1px solid #3498db;
            border-radius: 5px;
            background-color: #34495e;
            color: #ecf0f1;
        }

        button, select {
            background-color: #3498db;
            color: #fff;
            padding: 12px 24px;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
            border-radius: 5px;
        }

        button:hover, select:hover {
            background-color: #2980b9;
        }

        h1 {
            color: #3498db;
        }

        /* Dark mode styles */
        body.dark-mode {
            background-color: #17202A;
            color: #fff;
        }

        body.dark-mode button,
        body.dark-mode select,
        body.dark-mode input {
            background-color: #34495e;
            color: #ecf0f1;
        }

        body.dark-mode button:hover,
        body.dark-mode select:hover {
            background-color: #2c3e50;
        }

        /* Layout adjustments */
        .container {
            max-width: 800px;
            margin: 0 auto;
            text-align: left;
            padding: 20px;
            border-radius: 10px;
            background-color: #2c3e50; /* Slightly lighter blue */
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
        }

        .section {
            margin-bottom: 30px;
        }

        .summary {
            margin-top: 20px;
            list-style: none;
            padding: 0;
        }

        .summary li {
            border-bottom: 1px solid #34495e;
            padding: 10px 0;
        }

        .summary li:last-child {
            border-bottom: none;
        }

        /* Responsive adjustments */
        @media (max-width: 600px) {
            input {
                width: 100%;
            }
        }

        /* Clear button styles */
        .clear-btn {
            background-color: #e74c3c;
            color: #fff;
            padding: 12px 24px;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
            border-radius: 5px;
        }

        .clear-btn:hover {
            background-color: #c0392b;
        }

        /* Download and Print buttons styles */
        .download, .print {
            margin-right: 10px;
        }

        /* Instructions styles */
        .instructions {
            font-size: 14px;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Budget Sheet</h1>

        <div class="instructions">
            <p>Enter your income and expenses below. Select the time frame for calculation:</p>
        </div>

        <label for="timeframe">Select Timeframe:</label>
        <select id="timeframe" onchange="updateSummary()">
            <option value="weekly">Weekly</option>
            <option value="biweekly">Bi-weekly</option>
            <option value="monthly">Monthly</option>
            <option value="annual">Annual</option>
        </select>

        <button class="clear-btn" onclick="clearEntries()">Clear Entries</button>

        <div class="section">
            <label for="incomeSource">Income Source:</label>
            <input type="text" id="incomeSource" placeholder="Enter income source">
            <label for="income">Income:</label>
            <input type="number" id="income" placeholder="Enter income">
            <button onclick="addIncome()">Add Income</button>
        </div>

        <div class="section">
            <label for="expenseSource">Expense Source:</label>
            <input type="text" id="expenseSource" placeholder="Enter expense source">
            <label for="expense">Expense:</label>
            <input type="number" id="expense" placeholder="Enter expense">
            <button onclick="addExpense()">Add Expense</button>
        </div>

        <div class="section">
            <h2>Summary</h2>
            <ul id="budgetList" class="summary"></ul>

            <p>Total Income: $<span id="totalIncome">0.00</span></p>
            <p>Total Expenses: $<span id="totalExpense">0.00</span></p>

            <p class="remainder">Remainder: $<span id="remainder">0.00</span></p>
        </div>

        <div class="section">
            <button class="download" onclick="downloadPage()">Download Page</button>
            <button class="print" onclick="printPage()">Print Page</button>
        </div>
    </div>

    <script>
        let budgetLines = [];
        let timeframe = 'weekly';

        function addIncome() {
            addLine("income");
        }

        function addExpense() {
            addLine("expense");
        }

        function addLine(type) {
            const sourceInput = document.getElementById(`${type}Source`);
            const amountInput = document.getElementById(type);

            const source = sourceInput.value.trim();
            const amount = parseFloat(amountInput.value);

            if (source !== '' && !isNaN(amount)) {
                const line = {
                    type: type,
                    source: source,
                    amount: amount.toFixed(2),
                };

                budgetLines.push(line);
                updateSummary();
                sourceInput.value = '';
                amountInput.value = '';
            }
        }

        function updateSummary() {
            const budgetList = document.getElementById('budgetList');
            const totalIncomeElement = document.getElementById('totalIncome');
            const totalExpenseElement = document.getElementById('totalExpense');
            const remainderElement = document.getElementById('remainder');
            const timeframeSelect = document.getElementById('timeframe');

            timeframe = timeframeSelect.value;

            budgetList.innerHTML =           '';

            let totalIncome = 0;
            let totalExpense = 0;

            budgetLines.forEach(line => {
                const listItem = document.createElement('li');
                listItem.textContent = `${line.type.charAt(0).toUpperCase() + line.type.slice(1)} - ${line.source}: $${calculateAmount(line.amount)}`;
                budgetList.appendChild(listItem);

                if (line.type === 'income') {
                    totalIncome += parseFloat(calculateAmount(line.amount));
                } else if (line.type === 'expense') {
                    totalExpense += parseFloat(calculateAmount(line.amount));
                }
            });

            totalIncomeElement.textContent = calculateAmount(totalIncome.toFixed(2));
            totalExpenseElement.textContent = calculateAmount(totalExpense.toFixed(2));

            const remainder = totalIncome - totalExpense;
            remainderElement.textContent = calculateAmount(remainder.toFixed(2));
        }

        function calculateAmount(amount) {
            if (timeframe === 'weekly') {
                return amount;
            } else if (timeframe === 'biweekly') {
                return (parseFloat(amount) * 2).toFixed(2);
            } else if (timeframe === 'monthly') {
                return (parseFloat(amount) * 4.33).toFixed(2);
            } else if (timeframe === 'annual') {
                return (parseFloat(amount) * 52).toFixed(2);
            }
            return amount;
        }

        function clearEntries() {
            budgetLines = [];
            updateSummary();
        }

        function downloadPage() {
            const htmlContent = document.documentElement.outerHTML;
            const blob = new Blob([htmlContent], { type: 'text/html' });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = 'budget_sheet.html';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
        }

        function printPage() {
            window.print();
        }
    </script>

</body>
</html>



</body>
</html>


