<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Quebra-Cabeça Simples</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
    }
    .tabuleiro {
      width: 300px;
      height: 300px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 2px;
    }
    .peca {
      width: 100px;
      height: 100px;
      border: 1px solid #ccc;
      background-size: 300px 300px;
      cursor: move;
    }
  </style>
</head>
<body>

  <h1>Quebra-Cabeça</h1>
  <p>Arraste as peças para montar a imagem!</p>

  <div class="tabuleiro" id="tabuleiro"></div>

  <script>
    const tabuleiro = document.getElementById("tabuleiro");
    const pecas = [];

    // Criar as peças com posições embaralhadas
    for (let i = 0; i < 9; i++) {
      const peca = document.createElement("div");
      peca.classList.add("peca");
      peca.draggable = true;
      peca.dataset.posicao = i;
      pecas.push(peca);
    }

    // Embaralhar
    pecas.sort(() => Math.random() - 0.5);

    // Aplicar imagem e posições
    pecas.forEach((peca, i) => {
      const x = (peca.dataset.posicao % 3) * -100;
      const y = Math.floor(peca.dataset.posicao / 3) * -100;
      peca.style.backgroundImage = "url('https://i.imgur.com/3YVZiwY.jpg')";
      peca.style.backgroundPosition = `${x}px ${y}px`;
      tabuleiro.appendChild(peca);
    });

    let dragged;

    tabuleiro.addEventListener("dragstart", e => {
      if (e.target.classList.contains("peca")) {
        dragged = e.target;
      }
    });

    tabuleiro.addEventListener("dragover", e => {
      e.preventDefault();
    });

    tabuleiro.addEventListener("drop", e => {
      if (e.target.classList.contains("peca")) {
        const cloneA = dragged.cloneNode(true);
        const cloneB = e.target.cloneNode(true);
        tabuleiro.replaceChild(cloneA, e.target);
        tabuleiro.replaceChild(cloneB, dragged);
        addDragListeners(); // reinicia eventos
      }
    });

    function addDragListeners() {
      document.querySelectorAll(".peca").forEach(peca => {
        peca.addEventListener("dragstart", e => {
          dragged = e.target;
        });
      });
    }

    addDragListeners();
  </script>

</body>
</html>
