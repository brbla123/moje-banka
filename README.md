<!DOCTYPE html>
<html lang="cs">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Moje Banka</title>
  <link rel="icon" href="https://cdn-icons-png.flaticon.com/512/190/190411.png" type="image/png">
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #6dd5fa, #2980b9);
      min-height: 100vh;
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .container {
      background: white;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      width: 300px;
      text-align: center;
      animation: fadeIn 1s ease-in-out;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(-20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    input, button, select {
      width: 90%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 1em;
    }
    button {
      background-color: #3498db;
      color: white;
      border: none;
      cursor: pointer;
      transition: background 0.3s;
    }
    button:hover {
      background-color: #2980b9;
    }
    .hidden {
      display: none;
    }
    .balance {
      font-size: 2em;
      margin: 20px 0;
    }
    .history {
      text-align: left;
      max-height: 150px;
      overflow-y: auto;
      margin-top: 20px;
    }
    .dark-mode {
      background: #1e272e;
      color: white;
    }
  </style>
</head>
<body>

<div class="container" id="loginPage">
  <h2>Přihlášení</h2>
  <input type="text" id="username" placeholder="Uživatelské jméno">
  <input type="password" id="password" placeholder="Heslo">
  <button onclick="login()">Přihlásit se</button>
</div>

<div class="container hidden" id="bankPage">
  <h2>Moje Banka</h2>
  <div>
    <label for="accountSelect">Účet:</label>
    <select id="accountSelect" onchange="switchAccount()">
      <option value="current">Běžný účet</option>
      <option value="savings">Spořící účet</option>
    </select>
  </div>
  <div class="balance" id="balance">Zůstatek: 10 000 Kč</div>
  <input type="number" id="amount" placeholder="Částka k odeslání">
  <input type="text" id="recipient" placeholder="Příjemce">
  <button onclick="sendMoney()">Odeslat</button>
  <button onclick="toggleDarkMode()">Přepnout režim</button>
  <button onclick="logout()">Odhlásit se</button>
  <div class="history" id="history">
    <h3>Historie převodů</h3>
    <ul id="historyList"></ul>
  </div>
</div>

<script>
  let accounts = {
    current: 10000,
    savings: 50000
  };
  let currentAccount = "current";

  function login() {
    const username = document.getElementById('username').value.trim();
    const password = document.getElementById('password').value.trim();
    if (username && password) {
      document.getElementById('loginPage').classList.add('hidden');
      document.getElementById('bankPage').classList.remove('hidden');
      updateBalance();
    } else {
      alert('Zadejte uživatelské jméno a heslo!');
    }
  }

  function logout() {
    document.getElementById('loginPage').classList.remove('hidden');
    document.getElementById('bankPage').classList.add('hidden');
    document.getElementById('username').value = '';
    document.getElementById('password').value = '';
    document.getElementById('historyList').innerHTML = '';
  }

  function updateBalance() {
    const balanceEl = document.getElementById('balance');
    balanceEl.innerText = `Zůstatek: ${accounts[currentAccount].toLocaleString('cs-CZ')} Kč`;
  }

  function sendMoney() {
    const amount = parseFloat(document.getElementById('amount').value);
    const recipient = document.getElementById('recipient').value.trim();
    if (isNaN(amount) || amount <= 0 || recipient === "") {
      alert('Zadejte správnou částku a příjemce.');
      return;
    }
    if (amount > accounts[currentAccount]) {
      alert('Nemáte dostatek peněz.');
      return;
    }
    accounts[currentAccount] -= amount;
    updateBalance();
    addHistory(`Odesláno ${amount.toLocaleString('cs-CZ')} Kč příjemci ${recipient}`);
    document.getElementById('amount').value = '';
    document.getElementById('recipient').value = '';
  }

  function addHistory(text) {
    const list = document.getElementById('historyList');
    const item = document.createElement('li');
    item.innerText = text;
    list.prepend(item);
  }

  function switchAccount() {
    currentAccount = document.getElementById('accountSelect').value;
    updateBalance();
  }

  function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
  }
</script>

</body>
</html>
