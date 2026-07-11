


# Professional Expense Tracker

A responsive full-stack expense management web application built with **HTML, CSS, JavaScript, PHP, and MySQL**. It helps users record daily expenses, manage monthly budgets, automate recurring expenses, filter transactions, export data, and analyse spending through interactive charts.

## Project Overview

The Professional Expense Tracker provides a simple dashboard for monitoring personal spending. Expense data is stored in a MySQL database and accessed through a PHP API. The frontend dynamically updates totals, charts, reports, and transaction lists without reloading the page.

The application uses the Indian Rupee symbol (`₹`) for displaying monetary values.

## Features

- Add, edit, and delete expenses
- Store expense description, amount, date, category, and payment source
- Set and update a monthly budget
- Display the remaining monthly budget
- Show budget usage through a progress bar
- Highlight warning and over-budget conditions
- Create weekly, monthly, or yearly recurring expenses
- Automatically process due recurring expenses
- Manage and delete recurring expense records
- Filter expenses by category
- Filter expenses by payment source
- Search expenses using description keywords
- View current-month expense totals
- Export all expenses to a CSV file
- Generate reports for a selected date range
- View category-wise spending in a doughnut chart
- View daily spending in a line chart
- Compare category totals using a bar chart
- Switch between light and dark themes
- Responsive interface for desktop and mobile screens

## Technology Stack

| Technology | Purpose |
|---|---|
| HTML5 | Application structure |
| CSS3 | Custom styling and responsive design |
| JavaScript | Client-side logic and dynamic rendering |
| jQuery | DOM manipulation and event handling |
| Bootstrap 5 | Responsive layout, components, forms, and modals |
| Chart.js | Expense charts and report visualisations |
| PHP | Backend API and server-side processing |
| MySQL | Persistent storage for expenses, budgets, and recurring records |
| Font Awesome | Interface icons |
| Google Fonts | Inter font family |

## Application Architecture

```text
Browser Interface
       |
       | Fetch API / JSON
       v
    api.php
       |
       | MySQLi prepared statements
       v
 MySQL Database
```

The frontend communicates with `api.php` using asynchronous HTTP requests. The PHP API performs database operations through `db_connect.php` and returns JSON responses.

## Project Structure

```text
expense-tracker/
├── index.html          # Main dashboard and application interface
├── style.css           # Light/dark theme and component styling
├── script.js           # Frontend logic, API calls, charts, filters, and export
├── api.php             # PHP API for expenses, reports, budgets, and recurring data
├── db_connect.php      # MySQL database connection
└── README.md           # Project documentation
```

## Prerequisites

Install the following before running the project:

- Git
- XAMPP, WAMP, MAMP, or another PHP and MySQL server
- PHP 7.4 or later
- MySQL or MariaDB
- A modern web browser
- Internet access for CDN resources such as Bootstrap, jQuery, Chart.js, Font Awesome, and Google Fonts

## Installation and Setup

### 1. Clone the repository

```bash
git clone https://github.com/deveshmokhariya/expense-tracker.git
```

### 2. Move the project into the web-server directory

For XAMPP on Windows, place the project inside:

```text
C:\xampp\htdocs\expense-tracker
```

You can also clone it directly:

```bash
cd C:\xampp\htdocs
git clone https://github.com/deveshmokhariya/expense-tracker.git
```

### 3. Start the local server

Open the XAMPP Control Panel and start:

- Apache
- MySQL

### 4. Create the database

Open phpMyAdmin:

```text
http://localhost/phpmyadmin
```

Create a database named:

```text
expense_tracker
```

Then select the database and run the following SQL:

```sql
CREATE DATABASE IF NOT EXISTS expense_tracker;
USE expense_tracker;

CREATE TABLE IF NOT EXISTS expenses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    description VARCHAR(255) NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    category VARCHAR(50) NOT NULL,
    source VARCHAR(50) NOT NULL,
    date DATE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS recurring_expenses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    description VARCHAR(255) NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    category VARCHAR(50) NOT NULL,
    source VARCHAR(50) NOT NULL,
    frequency VARCHAR(20) NOT NULL,
    startDate DATE NOT NULL,
    lastAdded DATE DEFAULT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS settings (
    setting_key VARCHAR(100) PRIMARY KEY,
    setting_value VARCHAR(255) NOT NULL
);

INSERT INTO settings (setting_key, setting_value)
VALUES ('monthlyBudget', '0')
ON DUPLICATE KEY UPDATE setting_value = setting_value;
```

### 5. Configure the database connection

The default settings in `db_connect.php` are designed for a standard XAMPP installation:

```php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "expense_tracker";
```

Update these values when your MySQL username, password, host, or database name is different.

### 6. Open the application

Visit:

```text
http://localhost/expense-tracker/
```

Do not open `index.html` directly using a `file:///` address because the PHP backend requires a web server.

## How to Use

### Add an Expense

1. Click **Add New Expense**.
2. Enter the expense description.
3. Enter the amount.
4. Select the date.
5. Select a category.
6. Select the payment source.
7. Click **Add Expense**.

### Edit or Delete an Expense

Use the edit or delete button displayed beside a transaction in the expense table.

### Set a Monthly Budget

1. Click **Set Budget**.
2. Enter the monthly budget amount.
3. Save the budget.

The dashboard displays the remaining amount and changes the progress-bar state as spending approaches or exceeds the budget.

### Add a Recurring Expense

1. Open the **Add New Expense** form.
2. Enter the expense details.
3. Select **This is a recurring expense**.
4. Choose weekly, monthly, or yearly frequency.
5. Save the expense.

The backend checks recurring records when the application loads and creates expenses that have become due.

### Filter Transactions

Available filters include:

- Category
- Payment source
- Description keyword

Click **Reset** to remove all active filters.

### Generate Reports

1. Open the **Reports** section.
2. Choose a start date.
3. Choose an end date.
4. Click **Generate Report**.

The application displays:

- Spending over time
- Category comparison

### Export Expense Data

Click **Export** to download all recorded expenses as a CSV file.

## Expense Categories

The current categories are:

- Food
- Transport
- Shopping
- Utilities
- Entertainment
- Health
- Other

## Payment Sources

The current payment-source options are:

- Cash
- Credit Card
- Bank Transfer
- UPI
- Other

## API Actions

The application uses `api.php` with an `action` query parameter.

| Action | Method | Purpose |
|---|---|---|
| `dashboard` | GET | Fetch expenses, recurring records, and monthly budget |
| `add_expense` | POST | Add a new expense |
| `update_expense` | POST | Update an existing expense |
| `delete_expense` | DELETE | Delete an expense by ID |
| `add_recurring` | POST | Add a recurring expense |
| `delete_recurring` | DELETE | Delete a recurring expense |
| `set_budget` | POST | Save or update the monthly budget |
| `get_reports` | GET | Fetch expenses between two dates |
| `process_recurring` | POST | Create due expenses from recurring records |

Example dashboard request:

```text
api.php?action=dashboard
```

Example report request:

```text
api.php?action=get_reports&start=2026-01-01&end=2026-01-31
```

## Data Flow

1. The browser loads `index.html`.
2. `script.js` sends a request to process recurring expenses.
3. The frontend requests dashboard data from `api.php`.
4. PHP reads the required records from MySQL.
5. The server returns the information as JSON.
6. JavaScript updates the dashboard, transaction table, budget status, and charts.
7. User actions send additional requests for create, update, delete, report, or budget operations.

## Charts and Reports

The application uses Chart.js to display:

### Expenses by Category

A doughnut chart summarising the current month's expenses by category.

### Spending Over Time

A line chart showing daily spending during the selected report period.

### Category Comparison

A bar chart comparing total spending across expense categories.

## Theme Support

The light or dark theme selection is stored in browser `localStorage`, so the selected theme remains active when the application is opened again in the same browser.

## Responsive Design

Bootstrap's responsive grid system is used to adapt:

- Dashboard cards
- Filters
- Expense tables
- Forms and modals
- Charts
- Reports

to different screen sizes.

## Troubleshooting

### The application says it cannot connect to the backend

Check that:

- Apache is running
- MySQL is running
- The project is inside the server's document root
- `api.php` and `db_connect.php` are in the same folder
- The database name is `expense_tracker`
- The database credentials are correct

### Database connection failed

Update the credentials in `db_connect.php`:

```php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "expense_tracker";
```

### Charts are not visible

Check that:

- The browser has internet access
- Chart.js loaded successfully from the CDN
- The browser console does not contain JavaScript errors
- Expense data exists for the selected date range

### The page opens but saving does not work

Make sure you opened the application through:

```text
http://localhost/expense-tracker/
```

and not by double-clicking `index.html`.

### Recurring expenses are not being generated

Check that:

- The recurring record has a valid start date
- Its frequency is `weekly`, `monthly`, or `yearly`
- The system date is correct
- The `lastAdded` column exists in the `recurring_expenses` table

## Security Considerations

This project is currently suitable for learning and local use. Before deploying it publicly, consider adding:

- User authentication
- Authorisation for all API operations
- Server-side input validation
- CSRF protection
- Rate limiting
- Secure session handling
- Environment variables for database credentials
- HTTPS
- Restricted CORS rules
- Error logging without exposing database details
- Database backup and recovery procedures

Do not commit real production database passwords to a public repository.

## Current Limitations

- No user registration or login system
- All data belongs to a single shared database
- Currency is displayed in Indian Rupees
- The project depends on external CDN resources
- No automated test suite is included
- No database migration or SQL file is currently included in the repository
- Designed primarily for local PHP/MySQL environments

## Future Enhancements

- User accounts and secure login
- Separate data for each user
- Income tracking
- Savings-goal management
- Multiple currencies
- Custom categories
- Receipt image uploads
- Monthly and yearly comparison dashboards
- PDF report export
- Data import from CSV
- Email budget alerts
- Progressive Web App support
- Offline data synchronisation
- Automated testing
- Docker-based deployment
- Environment-variable configuration
- REST API validation and improved error handling

## Contributing

Contributions are welcome.

1. Fork the repository.
2. Create a feature branch.

```bash
git checkout -b feature/your-feature-name
```

3. Commit your changes.

```bash
git commit -m "Add your feature"
```

4. Push the branch.

```bash
git push origin feature/your-feature-name
```

5. Open a pull request.

## Author

**Devesh Mokhariya**

- GitHub: [deveshmokhariya](https://github.com/deveshmokhariya)
- LinkedIn: [Devesh Mokhariya](https://www.linkedin.com/in/devesh-mokhariya-91bbb6281/)

## Repository

[Expense Tracker on GitHub](https://github.com/deveshmokhariya/expense-tracker)

---

If this project is useful, consider giving the repository a star.
