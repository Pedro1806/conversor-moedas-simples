<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Conversor de Moedas</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>💱 Conversor de Moedas</h1>
        <div class="form-group">
            <label for="amount">Valor:</label>
            <input type="number" id="amount" placeholder="Digite o valor">
        </div>
        <div class="form-group">
            <label for="from">De:</label>
            <select id="from"></select>
        </div>
        <div class="form-group">
            <label for="to">Para:</label>
            <select id="to"></select>
        </div>
        <button id="convert">Converter</button>
        <p id="result"></p>
    </div>
    <script src="script.js"></script>

   body {
    font-family: Arial, sans-serif;
    background: #f5f5f5;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.container {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    width: 300px;
}

h1 {
    text-align: center;
    margin-bottom: 20px;
}

.form-group {
    margin-bottom: 15px;
}

label {
    display: block;
    margin-bottom: 5px;
}

input, select {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

button {
    width: 100%;
    padding: 10px;
    background: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background: #45a049;
}

#result {
    text-align: center;
    margin-top: 15px;
    font-weight: bold;
}

const fromCurrency = document.getElementById("from");
const toCurrency = document.getElementById("to");
const amount = document.getElementById("amount");
const result = document.getElementById("result");
const convertBtn = document.getElementById("convert");

const apiURL = "https://api.exchangerate.host/latest";

// Preenche as opções de moedas
fetch(apiURL)
    .then(res => res.json())
    .then(data => {
        const currencies = Object.keys(data.rates);
        currencies.forEach(currency => {
            let optionFrom = document.createElement("option");
            let optionTo = document.createElement("option");
            optionFrom.value = optionTo.value = currency;
            optionFrom.textContent = optionTo.textContent = currency;
            fromCurrency.appendChild(optionFrom);
            toCurrency.appendChild(optionTo);
        });
        fromCurrency.value = "USD";
        toCurrency.value = "BRL";
    });

convertBtn.addEventListener("click", () => {
    const from = fromCurrency.value;
    const to = toCurrency.value;
    const amountValue = amount.value;

    if (!amountValue || amountValue <= 0) {
        result.textContent = "Digite um valor válido.";
        return;
    }

    fetch(`${apiURL}?base=${from}&symbols=${to}`)
        .then(res => res.json())
        .then(data => {
            const rate = data.rates[to];
            const converted = (amountValue * rate).toFixed(2);
            result.textContent = `${amountValue} ${from} = ${converted} ${to}`;
        })
        .catch(() => {
            result.textContent = "Erro ao buscar taxas de câmbio.";
        });
});

</body>
</html>
