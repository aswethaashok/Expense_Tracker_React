# Expense Tracker (ReactJS)

## Date: 24-05-2025

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM

```
App.js

import React, { useState } from 'react';
import './App.css';

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState('');
  const [amount, setAmount] = useState('');
  const [category, setCategory] = useState('Shopping');

  // Calculate balance, income, expenses
  const amounts = transactions.map(t => t.amount);
  const balance = amounts.reduce((acc, val) => acc + val, 0).toFixed(2);
  const income = amounts.filter(a => a > 0).reduce((acc, val) => acc + val, 0).toFixed(2);
  const expenses = amounts.filter(a => a < 0).reduce((acc, val) => acc + val, 0).toFixed(2);

  // Add transaction
  const handleSubmit = (e) => {
    e.preventDefault();
    if (!description || !amount) return;

    const newTransaction = {
      id: Date.now(),
      description,
      amount: +amount,
      category,
      date: new Date().toLocaleDateString(),
    };

    setTransactions([...transactions, newTransaction]);
    setDescription('');
    setAmount('');
  };

  // Delete transaction
  const deleteTransaction = (id) => {
    setTransactions(transactions.filter(t => t.id !== id));
  };

  return (
    <div className="app">
      <header>
        <h1> ExpenseEdge </h1>
        <p>Track your expenses in style!</p>
      </header>

      <div className="dashboard">
        <div className="balance-card">
          <h3>Your Balance</h3>
          <h2>${balance}</h2>
        </div>

        <div className="stats">
          <div className="stat-card income">
            <h4>Income 💰</h4>
            <p>+${income}</p>
          </div>
          <div className="stat-card expense">
            <h4>Expenses 🛍</h4>
            <p>-${Math.abs(expenses)}</p>
          </div>
        </div>
      </div>

      <div className="transactions">
        <h3> Recent Transactions</h3>
        {transactions.length === 0 ? (
          <p className="empty-state">No transactions yet. Add one! </p>
        ) : (
          <ul>
            {transactions.map((t) => (
              <li key={t.id} className={t.amount > 0 ? 'income-item' : 'expense-item'}>
                <div className="transaction-info">
                  <span className="category">{t.category}</span>
                  <span className="desc">{t.description}</span>
                  <span className="date">{t.date}</span>
                </div>
                <div className="transaction-amount">
                  <span className="amount">
                    {t.amount > 0 ? ' +' : '💸 -'}${Math.abs(t.amount).toFixed(2)}
                  </span>
                  <button onClick={() => deleteTransaction(t.id)}>❌</button>
                </div>
              </li>
            ))}
          </ul>
        )}
      </div>

      <div className="add-transaction">
        <h3>🌸 Add New Transaction</h3>
        <form onSubmit={handleSubmit}>
          <input
            type="text"
            placeholder="What did you spend on?"
            value={description}
            onChange={(e) => setDescription(e.target.value)}
            required
          />
          <input
            type="number"
            placeholder="Amount"
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
            required
          />
          <select value={category} onChange={(e) => setCategory(e.target.value)}>
            <option value="Shopping">🛍 Shopping</option>
            <option value="Food">🍕 Food</option>
            <option value="Bills">💡 Bills</option>
            <option value="Beauty">💄 Beauty</option>
            <option value="Entertainment">🎬 Entertainment</option>
          </select>
          <button type="submit">Add 💖</button>
        </form>
      </div>
    </div>
  );
}

export default App;

App.css

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Comic Sans MS', 'Arial Rounded MT Bold', sans-serif;
}

body {
  background: #fff5f7;
  color: #5a3d5a;
}

.app {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  background: #fff;
  border-radius: 20px;
  box-shadow: 0 5px 15px rgba(255, 105, 180, 0.1);
}

header {
  text-align: center;
  margin-bottom: 20px;
  color: #ff85a2;
}

header h1 {
  font-size: 2.5rem;
  margin-bottom: 5px;
}

header p {
  font-size: 1.1rem;
  color: #9d7b9d;
}

.dashboard {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 20px;
}

.balance-card {
  background: linear-gradient(135deg, #ff9a9e, #fad0c4);
  color: white;
  padding: 20px;
  border-radius: 15px;
  text-align: center;
  box-shadow: 0 4px 8px rgba(255, 105, 180, 0.2);
}

.balance-card h2 {
  font-size: 2.5rem;
  margin-top: 5px;
}

.stats {
  display: flex;
  gap: 10px;
}

.stat-card {
  flex: 1;
  padding: 15px;
  border-radius: 12px;
  text-align: center;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
}

.stat-card.income {
  background: #e2f3e2;
  color: #4caf50;
}

.stat-card.expense {
  background: #ffebee;
  color: #f44336;
}

.transactions {
  background: #fff;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 3px 10px rgba(255, 182, 193, 0.2);
  margin-bottom: 20px;
}

.transactions h3 {
  margin-bottom: 15px;
  color: #ff85a2;
}

.transactions ul {
  list-style: none;
}

.transactions li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px dashed #ffcce0;
}

.transactions li:last-child {
  border-bottom: none;
}

.transaction-info {
  flex: 1;
}

.transaction-amount {
  display: flex;
  align-items: center;
  gap: 10px;
}

.category {
  font-size: 0.8rem;
  background: #ffd6e7;
  color: #d45d79;
  padding: 3px 10px;
  border-radius: 10px;
  margin-right: 8px;
}

.desc {
  font-weight: 600;
  color: #5a3d5a;
}

.date {
  font-size: 0.8rem;
  color: #9d7b9d;
  margin-top: 3px;
}

.amount {
  font-weight: bold;
}

.income-item .amount {
  color: #4caf50;
}

.expense-item .amount {
  color: #f44336;
}

button {
  background: none;
  border: none;
  cursor: pointer;
  color: #ff85a2;
  font-size: 1.2rem;
}

.empty-state {
  text-align: center;
  color: #9d7b9d;
  font-style: italic;
}

.add-transaction {
  background: #fff;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 3px 10px rgba(255, 182, 193, 0.2);
}

.add-transaction h3 {
  margin-bottom: 15px;
  color: #ff85a2;
}

.add-transaction form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.add-transaction input, 
.add-transaction select {
  padding: 12px;
  border: 1px solid #ffcce0;
  border-radius: 10px;
  background: #fff9fb;
  font-size: 1rem;
}

.add-transaction button {
  background: #ff85a2;
  color: white;
  border: none;
  padding: 12px;
  border-radius: 10px;
  cursor: pointer;
  font-weight: bold;
  font-size: 1rem;
  margin-top: 5px;
}

.add-transaction button:hover {
  background: #ff6b8b;
}
```


## OUTPUT

![WhatsApp Image 2025-05-24 at 12 05 53_4904cfb3](https://github.com/user-attachments/assets/7504b5e6-aac5-425c-a91b-3a42db0f43bf)



## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
