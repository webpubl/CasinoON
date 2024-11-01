<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Kasino</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #222;
            color: #fff;
            text-align: center;
            margin: 0;
            padding: 20px;
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .balance {
            font-size: 1.5em;
        }
        .button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            margin: 10px;
        }
        .result {
            margin-top: 20px;
            font-size: 1.2em;
        }
        #contact, #loginForm, #registerForm, #addFundsForm {
            margin-top: 20px;
            display: none;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Online Kasino</h1>
        <div class="balance" id="balance">Zůstatek: 0 Kč</div>
    </div>

    <!-- Přihlašovací formulář -->
    <div id="loginForm">
        <h2>Přihlášení</h2>
        <input type="text" id="loginUsername" placeholder="Uživatelské jméno">
        <input type="password" id="loginPassword" placeholder="Heslo">
        <button class="button" onclick="login()">Přihlásit se</button>
        <p>Nemáte účet? <a href="#" onclick="showRegister()">Zaregistrujte se</a></p>
    </div>

    <!-- Registrační formulář -->
    <div id="registerForm">
        <h2>Registrace</h2>
        <input type="text" id="registerUsername" placeholder="Uživatelské jméno">
        <input type="password" id="registerPassword" placeholder="Heslo">
        <button class="button" onclick="register()">Registrovat se</button>
        <p>Již máte účet? <a href="#" onclick="showLogin()">Přihlaste se</a></p>
    </div>

    <!-- Herní oblast -->
    <div id="gameArea" style="display: none;">
        <h2>Hra na Automatu</h2>
        <button class="button" onclick="playSlot()">Hráj Automat (10 Kč)</button>
        <div id="slotResult" class="result"></div>
        
        <h2>Hra na Ruletě</h2>
        <input type="number" id="rouletteNumber" placeholder="Vyber číslo (0-101)" min="0" max="101">
        <button class="button" onclick="playRoulette()">Hráj Ruletu (15 Kč)</button>
        <div id="rouletteResult" class="result"></div>
        
        <h2>Přidat peníze</h2>
        <button class="button" onclick="showAddFunds()">Přidej peníze</button>
        <button class="button" onclick="logout()">Odhlásit se</button>
    </div>

    <!-- Formulář pro přidání peněz -->
    <div id="addFundsForm">
        <h2>Přidat peníze na účet</h2>
        <input type="password" id="fundsPassword" placeholder="Zadejte heslo">
        <input type="number" id="amount" placeholder="Kolik chcete přidat?" min="1">
        <button class="button" onclick="addFunds()">Přidat</button>
        <button class="button" onclick="hideAddFunds()">Zpět</button>
    </div>

    <!-- Kontakt -->
    <div id="contact">
        <h2>Převést Peníze</h2>
        <p>Pro převod napište na email, napište tam svoje údaje bereme jak kreditní karty tak bankovní účty vše bude pod kontrolou, potrebujeme jeste vase uživatelske jméno a heslo na CasinoON, nezneužijeme to: 
            <a href="mailto:capolvoking@gmail.com" style="color: #28a745;">capolvoking@gmail.com</a>
        </p>
    </div>

    <script>
        let balance = 0;
        let loggedIn = false;
        const users = JSON.parse(localStorage.getItem('users')) || {};
        const fundsPassword = "CCCoNNcAssINO001";

        function showLogin() {
            document.getElementById("loginForm").style.display = "block";
            document.getElementById("registerForm").style.display = "none";
            document.getElementById("addFundsForm").style.display = "none";
        }

        function showRegister() {
            document.getElementById("registerForm").style.display = "block";
            document.getElementById("loginForm").style.display = "none";
            document.getElementById("addFundsForm").style.display = "none";
        }

        function showAddFunds() {
            document.getElementById("addFundsForm").style.display = "block";
        }

        function hideAddFunds() {
            document.getElementById("addFundsForm").style.display = "none";
        }

        function register() {
            const username = document.getElementById("registerUsername").value;
            const password = document.getElementById("registerPassword").value;
            if (username && password) {
                if (users[username]) {
                    alert("Toto uživatelské jméno již existuje.");
                } else {
                    users[username] = { password: password, balance: 0 };
                    localStorage.setItem('users', JSON.stringify(users));
                    alert("Úspěšně jste zaregistrováni! Můžete se nyní přihlásit.");
                    showLogin();
                }
            } else {
                alert("Prosím vyplňte uživatelské jméno a heslo.");
            }
        }

        function login() {
            const username = document.getElementById("loginUsername").value;
            const password = document.getElementById("loginPassword").value;
            if (users[username] && users[username].password === password) {
                loggedIn = true;
                balance = users[username].balance;
                document.getElementById("loginForm").style.display = "none";
                document.getElementById("registerForm").style.display = "none";
                document.getElementById("gameArea").style.display = "block";
                updateBalance();
            } else {
                alert("Špatné uživatelské jméno nebo heslo.");
            }
        }

        function logout() {
            if (loggedIn) {
                const username = document.getElementById("loginUsername").value;
                users[username].balance = balance;
                localStorage.setItem('users', JSON.stringify(users));
                loggedIn = false;
                balance = 0;
                updateBalance();
                showLogin();
                document.getElementById("gameArea").style.display = "none";
            }
        }

        function playSlot() {
            if (loggedIn && balance >= 10) {
                balance -= 10;
                const result = Math.floor(Math.random() * 100);
                document.getElementById("slotResult").innerText = `Výsledek: ${result}`;
                if (result < 20) {
                    balance += 50;
                }
                updateBalance();
            }
        }

        function playRoulette() {
            if (loggedIn && balance >= 15) {
                const chosenNumber = parseInt(document.getElementById("rouletteNumber").value);
                if (chosenNumber >= 0 && chosenNumber <= 101) {
                    balance -= 15;
                    const result = Math.floor(Math.random() * 102);
                    document.getElementById("rouletteResult").innerText = `Výsledek: ${result}`;
                    if (result === chosenNumber) {
                        balance += 150;
                    }
                    updateBalance();
                }
            }
        }

        function addFunds() {
            const amount = parseInt(document.getElementById("amount").value);
            const enteredPassword = document.getElementById("fundsPassword").value;
            if (enteredPassword === fundsPassword && amount > 0) {
                balance += amount;
                updateBalance();
                hideAddFunds();
            }
        }

        function updateBalance() {
            document.getElementById("balance").innerText = `Zůstatek: ${balance} Kč`;
        }

        showLogin();
    </script>
</body>
</html>
