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
        #contact {
            margin-top: 20px;
        }
        #loginForm, #registerForm, #addFundsForm {
            margin-top: 20px;
            display: none; /* Skryté při načtení */
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Online Kasino</h1>
        <div class="balance" id="balance">Zůstatek: 0 Kč</div>
    </div>

    <div id="loginForm">
        <h2>Přihlášení</h2>
        <input type="text" id="loginUsername" placeholder="Uživatelské jméno">
        <input type="password" id="loginPassword" placeholder="Heslo">
        <button class="button" onclick="login()">Přihlásit se</button>
        <p>Nemáte účet? <a href="#" onclick="showRegister()">Zaregistrujte se</a></p>
    </div>

    <div id="registerForm">
        <h2>Registrace</h2>
        <input type="text" id="registerUsername" placeholder="Uživatelské jméno">
        <input type="password" id="registerPassword" placeholder="Heslo">
        <button class="button" onclick="register()">Registrovat se</button>
        <p>Již máte účet? <a href="#" onclick="showLogin()">Přihlaste se</a></p>
    </div>

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
        <p>Pro převod napište na email, napište tam svoje údaje bereme jak kreditní karty tak bankovní účty vše bude pod kontrolou, potrebujeme jeste vase uživatelske jmeno a heslo na CasinoON, nezneužijeme to: 
        <a href="mailto:capolvoking@gmail.com" style="color: #28a745;">capolvoking@gmail.com</a></p>
    </div>

    <script>
        let balance = 0; // Počáteční zůstatek
        let loggedIn = false;
        const users = JSON.parse(localStorage.getItem('users')) || {}; // Načti uživatelské údaje z localStorage
        const fundsPassword = "ytrobseknejcz7896541230mamenorobuxbruhgdsbuzjvbjschchfhagfegfhkfufaolks/////**--++5uhadzspspspspspspsps9999999999999999999999999999999554844[[paspd-09w0927364yuryo/.;,.,;pl[;[qpij1i89`90ep[;[1[p2[;ep[;qel2ke,w.e,.2q.we.2.q;;[;;>::{PPL:{P{{}+{}"; // Opravená proměnná

        // Zobrazit přihlašovací formulář
        function showLogin() {
            document.getElementById("loginForm").style.display = "block";
            document.getElementById("registerForm").style.display = "none";
            document.getElementById("addFundsForm").style.display = "none";
        }

        // Zobrazit registrační formulář
        function showRegister() {
            document.getElementById("registerForm").style.display = "block";
            document.getElementById("loginForm").style.display = "none";
            document.getElementById("addFundsForm").style.display = "none";
        }

        // Zobrazit formulář pro přidání peněz
        function showAddFunds() {
            document.getElementById("addFundsForm").style.display = "block";
        }

        // Skrýt formulář pro přidání peněz
        function hideAddFunds() {
            document.getElementById("addFundsForm").style.display = "none";
        }

        // Registrace nového uživatele
        function register() {
            const username = document.getElementById("registerUsername").value;
            const password = document.getElementById("registerPassword").value;
            if (username && password) {
                if (users[username]) {
                    alert("Toto uživatelské jméno již existuje.");
                } else {
                    users[username] = { password: password, balance: 0 }; // Uložení uživatelských údajů
                    localStorage.setItem('users', JSON.stringify(users)); // Uložení do localStorage
                    alert("Úspěšně jste zaregistrováni! Můžete se nyní přihlásit.");
                    showLogin(); // Přepni na přihlašovací formulář
                }
            } else {
                alert("Prosím vyplňte uživatelské jméno a heslo.");
            }
        }

        // Přihlášení uživatele
        function login() {
            const username = document.getElementById("loginUsername").value;
            const password = document.getElementById("loginPassword").value;
            if (users[username] && users[username].password === password) {
                loggedIn = true;
                balance = users[username].balance; // Načti zůstatek z localStorage
                document.getElementById("loginForm").style.display = "none";
                document.getElementById("registerForm").style.display = "none";
                document.getElementById("gameArea").style.display = "block";
                updateBalance();
                alert("Úspěšně jste přihlášeni!");
            } else {
                alert("Špatné uživatelské jméno nebo heslo.");
            }
        }

        // Odhlášení uživatele
        function logout() {
            if (loggedIn) {
                const username = document.getElementById("loginUsername").value; // Načti uživatelské jméno
                users[username].balance = balance; // Ulož nový zůstatek
                localStorage.setItem('users', JSON.stringify(users)); // Uložení do localStorage
                alert("Úspěšně jste se odhlásili.");
                loggedIn = false;
                balance = 0;
                updateBalance();
                showLogin(); // Zobraz přihlašovací formulář
                document.getElementById("gameArea").style.display = "none"; // Skryj herní oblast
            }
        }

        // Hraní na automatu
        function playSlot() {
            if (loggedIn && balance >= 10) {
                balance -= 10; // Snížit zůstatek o 10 Kč
                const result = Math.floor(Math.random() * 100); // Simulace výsledku
                document.getElementById("slotResult").innerText = `Výsledek: ${result}`;
                if (result === 0) {
                    balance += 50; // Vyhraje 50 Kč
                    alert("Gratulujeme, vyhráli jste 50 Kč!");
                } else {
                    alert("Prohráli jste 10 Kč.");
                }
                updateBalance(); // Aktualizuj zůstatek
            } else {
                alert("Nemáte dostatek peněz na hraní.");
            }
        }

        // Hraní rulety
        function playRoulette() {
            if (loggedIn && balance >= 15) {
                const userNumber = parseInt(document.getElementById("rouletteNumber").value);
                if (userNumber >= 0 && userNumber <= 101) {
                    balance -= 15; // Snížit zůstatek o 15 Kč
                    const winningNumber = Math.floor(Math.random() * 102); // Simulace výherního čísla
                    document.getElementById("rouletteResult").innerText = `Výherní číslo: ${winningNumber}`;
                    if (userNumber === winningNumber) {
                        balance += 100; // Vyhraje 100 Kč
                        alert("Gratulujeme, vyhráli jste 100 Kč!");
                    } else {
                        alert("Prohráli jste 15 Kč.");
                    }
                    updateBalance(); // Aktualizuj zůstatek
                } else {
                    alert("Číslo musí být mezi 0 a 101.");
                }
            } else {
                alert("Nemáte dostatek peněz na hraní.");
            }
        }

        // Přidání peněz na účet
        function addFunds() {
            const password = document.getElementById("fundsPassword").value;
            const amount = parseInt(document.getElementById("amount").value);
            if (password === fundsPassword && amount > 0) {
                balance += amount; // Přidej částku do zůstatku
                alert(`Úspěšně jste přidali ${amount} Kč!`);
                updateBalance(); // Aktualizuj zůstatek
                hideAddFunds(); // Skrýt formulář pro přidání peněz
            } else {
                alert("Nesprávné heslo nebo částka není platná.");
            }
        }

        // Aktualizace zůstatku
        function updateBalance() {
            document.getElementById("balance").innerText = `Zůstatek: ${balance} Kč`;
        }

        // Načti přihlašovací formulář při načtení stránky
        window.onload = showLogin;
    </script>
</body>
</html>
