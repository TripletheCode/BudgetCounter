<!DOCTYPE html>
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
        }

        h1 {
            color: #3498db;
        }

        label {
            display: block;
            margin-top: 10px;
            color: #2c3e50;
        }

        input {
            padding: 5px;
            width: 200px;
            margin-bottom: 10px;
        }

        button {
            background-color: #3498db;
            color: #fff;
            padding: 8px 16px;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #2980b9;
        }

        h2 {
            color: #2c3e50;
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            background-color: #ecf0f1;
            margin: 5px;
            padding: 8px;
            border-radius: 5px;
            display: inline-block;
        }

        p {
            margin: 10px 0;
            color: #2c3e50;
        }

        button.download, button.print {
            margin-top: 20px;
        }

        .remainder {
            margin-top: 20px;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <h1>Budget Sheet</h1>

    <label for="incomeSource">Income Source:</label>
    <input type="text" id="incomeSource" placeholder="Enter income source">
    <label for="income">Income:</label>
    <input type="number" id="income" placeholder="Enter income">
    <button onclick="addIncome()">Add Income</button>

    <label for="expenseSource">Expense Source:</label>
    <input type="text" id="expenseSource" placeholder="Enter expense source">
    <label for="expense">Expense:</label>
    <input type="number" id="expense" placeholder="Enter expense">
    <button onclick="addExpense()">Add Expense</button>

    <h2>Summary</h2>
    <ul id="budgetList"></ul>

    <p>Total Income: $<span id="totalIncome">0.00</span></p>
    <p>Total Expenses: $<span id="totalExpense">0.00</span></p>

    <p class="remainder">Remainder: $<span id="remainder">0.00</span></p>

    <button class="download" onclick="downloadPage()">Download Page</button>
    <button class="print" onclick="printPage()">Print Page</button>

    <script>
        let budgetLines = [];

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

            budgetList.innerHTML = '';

            let totalIncome = 0;
            let totalExpense = 0;

            budgetLines.forEach(line => {
                const listItem = document.createElement('li');
                listItem.textContent = `${line.type.charAt(0).toUpperCase() + line.type.slice(1)} - ${line.source}: $${line.amount}`;
                budgetList.appendChild(listItem);

                if (line.type === 'income') {
                    totalIncome += parseFloat(line.amount);
                } else if (line.type === 'expense') {
                    totalExpense += parseFloat(line.amount);
                }
            });

            totalIncomeElement.textContent = totalIncome.toFixed(2);
            totalExpenseElement.textContent = totalExpense.toFixed(2);

            const remainder = totalIncome - totalExpense;
            remainderElement.textContent = remainder.toFixed(2);
        }

        function downloadPage() {
            const htmlContent = document.documentElement.outerHTML;
            const blob = new Blob([htmlContent], { type: 'text/html' });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = 'budget_sheet.html';
            document.body.appendChild(a);
            a            
            a.click();
            document.body.removeChild(a);
        }

        function printPage() {
            window.print();
        }
    </script>

</body>
</html>
