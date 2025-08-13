<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Expense Tracker</title>
    <link rel="manifest" href="manifest.json">
    <style>
        :root {
            --primary-color: #4a6fa5;
            --secondary-color: #166088;
            --accent-color: #4fc3f7;
            --background-color: #f5f5f5;
            --card-color: #ffffff;
            --text-color: #333333;
            --error-color: #e74c3c;
            --success-color: #2ecc71;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--background-color);
            color: var(--text-color);
            line-height: 1.6;
            padding: 0;
            margin: 0;
            font-size: 16px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 1rem;
        }

        header {
            background-color: var(--primary-color);
            color: white;
            padding: 1rem 0;
            text-align: center;
            margin-bottom: 1.5rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            position: relative;
        }

        h1 {
            font-size: 1.8rem;
        }

        .language-selector {
            position: absolute;
            top: 1rem;
            right: 1rem;
        }

        .language-selector select {
            padding: 0.3rem;
            border-radius: 4px;
            border: none;
        }

        .card {
            background-color: var(--card-color);
            border-radius: 8px;
            padding: 1.5rem;
            margin-bottom: 1.5rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .form-group {
            margin-bottom: 1rem;
        }

        label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
        }

        input, select {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 1rem;
        }

        button {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 0.75rem 1.5rem;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1rem;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: var(--secondary-color);
        }

        button.secondary {
            background-color: #6c757d;
        }

        button.danger {
            background-color: var(--error-color);
        }

        button.success {
            background-color: var(--success-color);
        }

        .btn-group {
            display: flex;
            gap: 0.5rem;
            margin-top: 1rem;
        }

        .btn-group button {
            flex: 1;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 1rem;
        }

        th, td {
            padding: 0.75rem;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        th {
            background-color: #f8f9fa;
            font-weight: 600;
        }

        tr:hover {
            background-color: #f5f5f5;
        }

        .action-buttons {
            display: flex;
            gap: 0.5rem;
        }

        .action-buttons button {
            padding: 0.5rem;
        }

        .chart-container {
            position: relative;
            height: 300px;
            margin-bottom: 1rem;
        }

        .filter-group {
            display: flex;
            gap: 1rem;
            margin-bottom: 1rem;
            flex-wrap: wrap;
        }

        .filter-group .form-group {
            flex: 1;
            min-width: 150px;
        }

        .total-display {
            font-size: 1.2rem;
            font-weight: 600;
            margin: 1rem 0;
            text-align: right;
        }

        .no-expenses {
            text-align: center;
            padding: 2rem;
            color: #6c757d;
        }

        .tabs {
            display: flex;
            margin-bottom: 1rem;
            border-bottom: 1px solid #ddd;
        }

        .tab {
            padding: 0.75rem 1.5rem;
            cursor: pointer;
            border-bottom: 3px solid transparent;
        }

        .tab.active {
            border-bottom: 3px solid var(--primary-color);
            font-weight: 600;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        @media (max-width: 600px) {
            .container {
                padding: 0.5rem;
            }

            .card {
                padding: 1rem;
            }

            .action-buttons {
                flex-direction: column;
            }

            .action-buttons button {
                width: 100%;
            }

            .filter-group {
                flex-direction: column;
                gap: 0.5rem;
            }

            .btn-group {
                flex-direction: column;
            }

            th, td {
                padding: 0.5rem;
                font-size: 0.9rem;
            }

            .tabs {
                overflow-x: auto;
                white-space: nowrap;
                padding-bottom: 5px;
            }
        }
      .offline-notification {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #ff9800;
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
            z-index: 1000;
            display: none;
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <div class="language-selector">
                <select id="language-select">
                    <option value="en">English</option>
                    <option value="ta">தமிழ் (Tamil)</option>
                    <option value="es">Español (Spanish)</option>
                    <option value="fr">Français (French)</option>
                    <option value="de">Deutsch (German)</option>
                </select>
            </div>
            <h1 id="app-title">Expense Tracker</h1>
        </div>
    </header>

    <div class="container">
        <div class="tabs">
            <div class="tab active" data-tab="expenses" id="expenses-tab-label">Expenses</div>
            <div class="tab" data-tab="charts" id="charts-tab-label">Charts</div>
        </div>

        <div class="tab-content active" id="expenses-tab">
            <div class="card">
                <h2 id="add-expense-title">Add New Expense</h2>
                <form id="expense-form">
                    <div class="form-group">
                        <label for="expense-description" id="description-label">Description</label>
                        <input type="text" id="expense-description" required>
                    </div>
                    <div class="form-group">
                        <label for="expense-amount" id="amount-label">Amount</label>
                        <input type="number" id="expense-amount" step="0.01" min="0" required>
                    </div>
                    <div class="form-group">
                        <label for="expense-currency" id="currency-label">Currency</label>
                        <select id="expense-currency" required>
                            <option value="USD">Dollar ($)</option>
                            <option value="INR">Rupees (₹)</option>
                            <option value="CNY">Yuan (¥)</option>
                            <option value="MYR">Ringgit (RM)</option>
                            <option value="RUB">Ruble (₽)</option>
                            <option value="EUR">Euro (€)</option>
                            <option value="GBP">Pound (£)</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="expense-category" id="category-label">Category</label>
                        <select id="expense-category" required>
                            <option value="">Select a category</option>
                            <option value="Food">Food</option>
                            <option value="Transport">Transport</option>
                            <option value="Housing">Housing</option>
                            <option value="Entertainment">Entertainment</option>
                            <option value="Utilities">Utilities</option>
                            <option value="Healthcare">Healthcare</option>
                            <option value="Shopping">Shopping</option>
                            <option value="Other">Other</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="expense-date" id="date-label">Date</label>
                        <input type="date" id="expense-date" required>
                    </div>
                    <div class="btn-group">
                        <button type="submit" id="save-expense" class="success">Save Expense</button>
                        <button type="button" id="cancel-edit" class="secondary" style="display: none;">Cancel</button>
                    </div>
                </form>
            </div>

            <div class="card">
                <h2 id="filter-title">Filter Expenses</h2>
                <div class="filter-group">
                    <div class="form-group">
                        <label for="search-description" id="search-label">Search</label>
                        <input type="text" id="search-description" placeholder="Search descriptions">
                    </div>
                    <div class="form-group">
                        <label for="filter-category" id="filter-category-label">Category</label>
                        <select id="filter-category">
                            <option value="">All Categories</option>
                            <option value="Food">Food</option>
                            <option value="Transport">Transport</option>
                            <option value="Housing">Housing</option>
                            <option value="Entertainment">Entertainment</option>
                            <option value="Utilities">Utilities</option>
                            <option value="Healthcare">Healthcare</option>
                            <option value="Shopping">Shopping</option>
                            <option value="Other">Other</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="filter-date-from" id="from-label">From</label>
                        <input type="date" id="filter-date-from">
                    </div>
                    <div class="form-group">
                        <label for="filter-date-to" id="to-label">To</label>
                        <input type="date" id="filter-date-to">
                    </div>
                </div>
                <div class="btn-group">
                    <button type="button" id="apply-filters" class="success">Apply Filters</button>
                    <button type="button" id="reset-filters" class="secondary">Reset</button>
                    <button type="button" id="export-csv" class="secondary">Export to CSV</button>
                    <button type="button" id="backup-data" class="secondary">Backup</button>
                </div>
            </div>

            <div class="card">
                <h2 id="expense-list-title">Expense List</h2>
                <div class="total-display">
                    <span id="total-label">Total:</span> <span id="total-amount">$0.00</span>
                </div>
                <div id="expense-list-container">
                    <table id="expense-list">
                        <thead>
                            <tr>
                                <th id="date-header">Date</th>
                                <th id="description-header">Description</th>
                                <th id="category-header">Category</th>
                                <th id="amount-header">Amount</th>
                                <th id="actions-header">Actions</th>
                            </tr>
                        </thead>
                        <tbody id="expense-list-body">
                            <!-- Expenses will be loaded here -->
                        </tbody>
                    </table>
                    <div id="no-expenses" class="no-expenses">
                        No expenses found. Add your first expense!
                    </div>
                </div>
            </div>
        </div>

        <div class="tab-content" id="charts-tab">
            <div class="card">
                <h2 id="monthly-chart-title">Monthly Expenses</h2>
                <div class="chart-container">
                    <canvas id="monthly-chart"></canvas>
                </div>
            </div>
            <div class="card">
                <h2 id="yearly-chart-title">Yearly Expenses by Category</h2>
                <div class="chart-container">
                    <canvas id="yearly-chart"></canvas>
                </div>
            </div>
        </div>
    </div>

    <div class="offline-notification" id="offline-notification">
        You are currently offline. Changes will be saved locally.
    </div>
    <input type="file" id="restore-file" accept=".json" style="display: none;">

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // Language translations
        const translations = {
            en: {
                appTitle: "Expense Tracker",
                expensesTab: "Expenses",
                chartsTab: "Charts",
                addExpenseTitle: "Add New Expense",
                descriptionLabel: "Description",
                amountLabel: "Amount",
                currencyLabel: "Currency",
                categoryLabel: "Category",
                dateLabel: "Date",
                saveExpense: "Save Expense",
                cancel: "Cancel",
                filterTitle: "Filter Expenses",
                searchLabel: "Search",
                filterCategoryLabel: "Category",
                fromLabel: "From",
                toLabel: "To",
                applyFilters: "Apply Filters",
                resetFilters: "Reset",
                exportCSV: "Export to CSV",
                backup: "Backup",
                expenseListTitle: "Expense List",
                totalLabel: "Total:",
                dateHeader: "Date",
                descriptionHeader: "Description",
                categoryHeader: "Category",
                amountHeader: "Amount",
                actionsHeader: "Actions",
                edit: "Edit",
                delete: "Delete",
                noExpenses: "No expenses found. Add your first expense!",
                monthlyChartTitle: "Monthly Expenses",
                yearlyChartTitle: "Yearly Expenses by Category",
                offlineNotification: "You are currently offline. Changes will be saved locally.",
                confirmDelete: "Are you sure you want to delete this expense?",
                invalidFields: "Please fill in all fields correctly",
                noExpensesExport: "No expenses to export with current filters",
                backupSuccess: "Backup created successfully!",
                restoreSuccess: "Expenses restored successfully!",
                invalidBackup: "Invalid backup file format",
                restoreFailed: "Failed to restore expenses",
                storageFull: "Storage is full. Please delete some expenses."
            },
