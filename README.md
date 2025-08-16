meu-jogo-aventura/
│
├── index.html
├── css/
│   └── style.css
├── js/
│   └── game.js
└── README.md
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aventura em Texto</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <div id="game-container">
        <h1>Aventura em Texto</h1>
        <div id="status-bar">
            <span id="vida">❤️ Vida: 100</span>
            <span id="energia">⚡ Energia: 50</span>
            <span id="ouro">💰 Ouro: 0</span>
            <span id="dia">📅 Dia: 1</span>
        </div>
        <div id="game-text"></div>
        <div id="choices"></div>
        <div id="inventory"><strong>Inventário:</strong> <span id="items"></span></div>
        <div id="history"><strong>Histórico:</strong> <span id="log"></span></div>
        <div id="controls">
            <button onclick="voltarCena()">🔙 Voltar</button>
            <button onclick="salvarJogo()">💾 Salvar</button>
            <button onclick="carregarJogo()">📂 Carregar</button>
            <button onclick="reiniciarJogo()">🔄 Reiniciar</button>
        </div>
    </div>
    <script src="js/game.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background-color: #111;
    color: white;
    text-align: center;
    margin: 0;
    padding: 0;
}

#game-container {
    max-width: 800px;
    margin: auto;
    padding: 20px;
}

#status-bar {
    background-color: #222;
    padding: 10px;
    margin-bottom: 15px;
}

#choices button {
    background-color: #444;
    border: none;
    padding: 10px;
    margin: 5px;
    color: white;
    cursor: pointer;
    border-radius: 5px;
}

#choices button:hover {
    background-color: #666;
}
let jogo = {
    vida: 100,
    energia: 50,
    ouro: 0,
    dia: 1,
    inventario: [],
    historico: [],
    cenaAtual: "inicio"
};

function mostrarCena(texto, opcoes) {
    document.getElementById("game-text").innerHTML = texto;
    let botoes = "";
    opcoes.forEach(o => {
        botoes += `<button onclick="${o.acao}">${o.texto}</button>`;
    });
    document.getElementById("choices").innerHTML = botoes;
    atualizarStatus();
}

function atualizarStatus() {
    document.getElementById("vida").innerText = `❤️ Vida: ${jogo.vida}`;
    document.getElementById("energia").innerText = `⚡ Energia: ${jogo.energia}`;
    document.getElementById("ouro").innerText = `💰 Ouro: ${jogo.ouro}`;
    document.getElementById("dia").innerText = `📅 Dia: ${jogo.dia}`;
    document.getElementById("items").innerText = jogo.inventario.join(", ") || "Nenhum";
    document.getElementById("log").innerText = jogo.historico.join(" ➡ ") || "Nenhum";
}

function inicio() {
    jogo.cenaAtual = "inicio";
    jogo.historico.push("Início");
    mostrarCena(
        "Você acorda em uma pequena vila. Há uma estrada para o norte e uma taverna próxima.",
        [
            { texto: "Seguir para o norte", acao: "floresta()" },
            { texto: "Entrar na taverna", acao: "taverna()" }
        ]
    );
}

function floresta() {
    jogo.cenaAtual = "floresta";
    jogo.historico.push("Floresta");
    jogo.energia -= 10;
    mostrarCena(
        "Você entra na floresta e ouve o som de água corrente.",
        [
            { texto: "Seguir o som", acao: "rio()" },
            { texto: "Voltar para a vila", acao: "inicio()" }
        ]
    );
}

function taverna() {
    jogo.cenaAtual = "taverna";
    jogo.historico.push("Taverna");
    jogo.ouro -= 5;
    mostrarCena(
        "Na taverna, você encontra um mercador e um aventureiro.",
        [
            { texto: "Conversar com o mercador", acao: "mercador()" },
            { texto: "Falar com o aventureiro", acao: "aventureiro()" }
        ]
    );
}

function mercador() {
    jogo.ouro += 10;
    jogo.inventario.push("Mapa Misterioso");
    mostrarCena(
        "O mercador lhe vende um mapa misterioso.",
        [{ texto: "Voltar para a vila", acao: "inicio()" }]
    );
}

function aventureiro() {
    jogo.vida -= 20;
    mostrarCena(
        "O aventureiro o desafia para um duelo e você se machuca.",
        [{ texto: "Voltar para a vila", acao: "inicio()" }]
    );
}

function rio() {
    jogo.energia += 10;
    mostrarCena(
        "Você encontra um rio e recupera energia.",
        [{ texto: "Voltar para a floresta", acao: "floresta()" }]
    );
}

function voltarCena() {
    if (jogo.historico.length > 1) {
        jogo.historico.pop();
        let cenaAnterior = jogo.historico.pop();
        window[cenaAnterior.toLowerCase()]();
    }
}

function salvarJogo() {
    localStorage.setItem("jogoSalvo", JSON.stringify(jogo));
}

function carregarJogo() {
    let salvo = localStorage.getItem("jogoSalvo");
    if (salvo) {
        jogo = JSON.parse(salvo);
        window[jogo.cenaAtual]();
    }
}

function reiniciarJogo() {
    jogo = {
        vida: 100,
        energia: 50,
        ouro: 0,
        dia: 1,
        inventario: [],
        historico: [],
        cenaAtual: "inicio"
    };
    inicio();
}

inicio();
# Aventura em Texto (HTML + CSS + JS)

Um jogo de aventura interativo em texto, inspirado no projeto da Alura **"Algoritmos: Criando uma Aventura"**.

## 🎮 Como Jogar
Abra o `index.html` no navegador ou hospede via GitHub Pages.  
Faça escolhas clicando nos botões para avançar na história.

## 📂 Estrutura do Projeto
