<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Expense Tracker</title>

  <style>
    body {
      margin: 0;
      font-family: Arial;
      background: #1f1f2e;
      color: white;
    }

    .container {
      max-width: 500px;
      margin: auto;
      padding: 15px;
    }

    h1 {
      text-align: center;
    }

    .card {
      background: #2c2c3e;
      padding: 15px;
      border-radius: 10px;
      margin-bottom: 10px;
    }

    input, select {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      margin-bottom: 10px;
      border-radius: 6px;
      border: none;
    }

    button {
      width: 100%;
      padding: 10px;
      border: none;
      border-radius: 6px;
      background: #4f46e5;
      color: white;
      cursor: pointer;
    }

    .income { color: lightgreen; }
    .expense { color: red; }

    ul { list-style: none; padding: 0; }

    li {
      background: #3a3a50;
      padding: 10px;
      margin-top: 5px;
      border-radius: 6px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .delete {
      background: red;
      border: none;
      color: white;
      padding: 5px 8px;
      border-radius: 5px;
      cursor: pointer;
    }

    img {
      width: 100%;
      border-radius: 10px;
      margin-bottom: 10px;
    }
  </style>
</head>

<body>

<div class="container">

  <img src="https://images.unsplash.com/photo-1554224155-6726b3ff858f">

  <h1>Expense Tracker</h1>

  <div class="card">
    <h3>Balance: $<span id="balance">0</span></h3>
    <h4 class="income">Income: $<span id="income">0</span></h4>
    <h4 class="expense">Expense: $<span id="expense">0</span></h4>
  </div>

  <div class="card">
    <input type="text" id="text" placeholder="Enter description">

    <select id="category">
      <option>Petrol</option>
      <option>Food</option>
      <option>Rent</option>
      <option>Other</option>
    </select>

    <input type="number" id="amount" placeholder="Enter amount">

    <button onclick="addTransaction()">Add Transaction</button>
  </div>

  <div class="card">
    <h3>History</h3>
    <ul id="list"></ul>
  </div>

</div>

<script>
let transactions = JSON.parse(localStorage.getItem("transactions")) || [];

function updateUI() {
  const list = document.getElementById("list");
  list.innerHTML = "";

  let balance = 0, income = 0, expense = 0;

  transactions.forEach((t, index) => {
    const li = document.createElement("li");

    li.innerHTML = `
      ${t.text} (${t.category}) 
      <span>${t.amount}</span>
      <button class="delete" onclick="removeTransaction(${index})">X</button>
    `;

    list.appendChild(li);

    balance += t.amount;
    if (t.amount > 0) income += t.amount;
    else expense += t.amount;
  });

  document.getElementById("balance").innerText = balance;
  document.getElementById("income").innerText = income;
  document.getElementById("expense").innerText = Math.abs(expense);

  localStorage.setItem("transactions", JSON.stringify(transactions));
}

function addTransaction() {
  const text = document.getElementById("text").value;
  const category = document.getElementById("category").value;
  const amount = Number(document.getElementById("amount").value);

  if (!text || !amount) return;

  transactions.push({ text, category, amount });

  document.getElementById("text").value = "";
  document.getElementById("amount").value = "";

  updateUI();
}

function removeTransaction(index) {
  transactions.splice(index, 1);
  updateUI();
}

updateUI();
</script>

</body>
</html>
