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
        .button:disabled {
            background-color: #555;
            cursor: not-allowed;
        }
        .result {
            font-size: 1.5em;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Online Kasino</h1>
        <div class="balance">Zůstatek: <span id="balance">100</span> Kč</div>
    </div>
    <button class="button" onclick="playGame()">Hraj</button>
    <div class="result" id="result"></div>

    <script>
        let balance = 100; // Počáteční zůstatek

        function playGame() {
            const bet = 10; // Sázka
            const win = Math.random() < 0.5; // 50% šance na výhru

            if (win) {
                // Generování náhodné výhry mezi 10 a 100 Kč
                const winnings = Math.floor(Math.random() * 91) + 10; // Náhodné číslo od 10 do 100
                balance += winnings; // Zvýšení zůstatku při výhře
                document.getElementById("result").innerText = "Vyhráli jste " + winnings + " Kč!";
            } else {
                balance -= bet; // Snížení zůstatku při prohře
                document.getElementById("result").innerText = "Prohráli jste " + bet + " Kč!";
            }

            // Aktualizace zůstatku na stránce
            document.getElementById("balance").innerText = balance;

            // Kontrola, zda je zůstatek kladný
            if (balance <= 0) {
                document.getElementById("result").innerText = "Nemáte dostatek peněz na hru!";
                document.querySelector(".button").disabled = true; // Zakáže tlačítko po vyčerpání zůstatku
            }
        }
    </script>
</body>
</html>
