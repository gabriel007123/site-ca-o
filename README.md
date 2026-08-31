<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HUD - Homem-Aranha</title>

  <style>
    /* =========================================================
       CONFIGURAÇÕES GERAIS
       ========================================================= */
    :root {
      --hud-scale: 1;
      --red: #ff1635;
      --red-dark: #720015;
      --blue: #147dff;
      --blue-dark: #082e6e;
      --white: #f4f7ff;
      --gold: #ffd32a;
      --panel: rgba(7, 10, 16, 0.94);
    }

    * {
      box-sizing: border-box;
    }

    html,
    body {
      width: 100%;
      min-height: 100%;
      margin: 0;
    }

    body {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;

      background:
        radial-gradient(circle at 20% 50%, rgba(255, 0, 35, 0.08), transparent 30%),
        radial-gradient(circle at 80% 50%, rgba(0, 100, 255, 0.08), transparent 30%),
        #030508;

      font-family:
        Impact,
        Haettenschweiler,
        "Arial Narrow Bold",
        "Arial Narrow",
        Arial,
        sans-serif;
    }

    /* =========================================================
       HUD PRINCIPAL
       ========================================================= */
    .hud {
      position: relative;
      width: 580px;
      height: 265px;
      transform: scale(var(--hud-scale));
      transform-origin: center;

      color: var(--white);
      background:
        radial-gradient(
          circle at 18% 40%,
          rgba(255, 0, 45, 0.16),
          transparent 35%
        ),
        radial-gradient(
          circle at 83% 45%,
          rgba(0, 100, 255, 0.17),
          transparent 37%
        ),
        linear-gradient(
          105deg,
          rgba(27, 4, 10, 0.98) 0%,
          rgba(10, 12, 18, 0.98) 45%,
          rgba(5, 12, 25, 0.98) 100%
        );

      border: 1px solid rgba(130, 145, 165, 0.65);

      clip-path: polygon(
        0 12px,
        12px 0,
        calc(100% - 18px) 0,
        100% 18px,
        100% calc(100% - 13px),
        calc(100% - 14px) 100%,
        14px 100%,
        0 calc(100% - 14px)
      );

      box-shadow:
        0 0 3px rgba(255, 255, 255, 0.3),
        -8px 0 25px rgba(255, 0, 35, 0.24),
        8px 0 25px rgba(0, 110, 255, 0.24),
        inset 0 0 35px rgba(0, 0, 0, 0.75);

      isolation: isolate;
      animation: hudGlow 5s ease-in-out infinite;
    }

    /* Borda tecnológica colorida */
    .hud::before {
      content: "";
      position: absolute;
      inset: 0;
      z-index: -1;
      padding: 2px;

      background:
        linear-gradient(
          90deg,
          var(--red) 0%,
          #e5092f 28%,
          #673b58 50%,
          #1555ba 72%,
          var(--blue) 100%
        );

      clip-path: polygon(
        0 12px,
        12px 0,
        calc(100% - 18px) 0,
        100% 18px,
        100% calc(100% - 13px),
        calc(100% - 14px) 100%,
        14px 100%,
        0 calc(100% - 14px)
      );

      -webkit-mask:
        linear-gradient(#000 0 0) content-box,
        linear-gradient(#000 0 0);

      -webkit-mask-composite: xor;
      mask-composite: exclude;

      filter:
        drop-shadow(0 0 4px rgba(255, 0, 40, 0.7))
        drop-shadow(0 0 7px rgba(0, 110, 255, 0.45));
    }

    /* Linhas e detalhes decorativos */
    .hud::after {
      content: "";
      position: absolute;
      inset: 7px;
      z-index: -1;
      pointer-events: none;

      background:
        linear-gradient(
          135deg,
          transparent 0 7%,
          rgba(255,255,255,0.035) 7.1%,
          transparent 7.3% 92%,
          rgba(255,255,255,0.035) 92.1%,
          transparent 92.3%
        ),
        repeating-linear-gradient(
          90deg,
          transparent 0 37px,
          rgba(255,255,255,0.018) 38px,
          transparent 39px 75px
        );

      opacity: 0.75;
    }

    /* =========================================================
       ELEMENTOS DECORATIVOS
       ========================================================= */
    .tech-line {
      position: absolute;
      pointer-events: none;
      opacity: 0.55;
    }

    .tech-line.top-left {
      left: 0;
      top: 7px;
      width: 150px;
      height: 1px;
      background: linear-gradient(
        90deg,
        var(--red),
        rgba(255, 0, 40, 0)
      );
      box-shadow: 0 0 5px var(--red);
    }

    .tech-line.top-right {
      right: 0;
      top: 9px;
      width: 180px;
      height: 1px;
      background: linear-gradient(
        90deg,
        rgba(0, 110, 255, 0),
        var(--blue)
      );
      box-shadow: 0 0 5px var(--blue);
    }

    .tech-line.bottom {
      left: 30px;
      right: 30px;
      bottom: 9px;
      height: 1px;
      background:
        linear-gradient(
          90deg,
          transparent,
          rgba(255, 20, 50, 0.55),
          rgba(70, 80, 100, 0.3),
          rgba(0, 120, 255, 0.55),
          transparent
        );
    }

    /* Pequenos recortes laterais */
    .corner {
      position: absolute;
      width: 22px;
      height: 22px;
      pointer-events: none;
    }

    .corner::before,
    .corner::after {
      content: "";
      position: absolute;
      background: rgba(180, 190, 205, 0.55);
    }

    .corner.tl {
      top: 5px;
      left: 5px;
    }

    .corner.tl::before {
      width: 14px;
      height: 1px;
      left: 0;
      top: 4px;
    }

    .corner.tl::after {
      width: 1px;
      height: 14px;
      left: 4px;
      top: 0;
    }

    .corner.br {
      right: 5px;
      bottom: 5px;
    }

    .corner.br::before {
      width: 14px;
      height: 1px;
      right: 0;
      bottom: 4px;
    }

    .corner.br::after {
      width: 1px;
      height: 14px;
      right: 4px;
      bottom: 0;
    }

    /* =========================================================
       CONTEÚDO
       ========================================================= */
    .hud-content {
      position: absolute;
      inset: 0;
      padding: 14px 24px 18px;
    }

    .header {
      display: flex;
      align-items: flex-start;
      justify-content: space-between;
      gap: 15px;
    }

    /* =========================================================
       TÍTULO
       ========================================================= */
    .title {
      margin: 0;
      color: #fff;

      font-size: 31px;
      line-height: 1;
      letter-spacing: 0.7px;
      font-weight: 900;

      text-shadow:
        2px 2px 0 #000,
        0 0 4px rgba(255, 255, 255, 0.15);
    }

    .title::after {
      content: "";
      display: block;
      width: 125px;
      height: 2px;
      margin-top: 5px;

      background: linear-gradient(
        90deg,
        var(--red),
        rgba(255, 0, 40, 0)
      );

      box-shadow: 0 0 6px var(--red);
    }

    /* =========================================================
       INDICADOR DE TECLAS
       ========================================================= */
    .controls {
      min-width: 185px;
      padding: 6px 10px 5px;

      color: #e9edf7;
      background:
        linear-gradient(
          180deg,
          rgba(34, 42, 55, 0.9),
          rgba(8, 12, 18, 0.95)
        );

      border: 1px solid rgba(130, 150, 180, 0.65);
      border-radius: 2px;

      font-family: Arial, sans-serif;
      font-size: 10px;
      font-weight: 700;
      letter-spacing: 0.4px;
      text-align: center;

      box-shadow:
        inset 0 0 7px rgba(0, 0, 0, 0.8),
        0 0 6px rgba(20, 100, 180, 0.12);

      white-space: nowrap;
    }

    /* =========================================================
       BARRA DE PROGRESSO
       ========================================================= */
    .progress-row {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-top: 19px;
    }

    .progress {
      position: relative;
      width: 310px;
      height: 30px;
      padding: 3px;

      overflow: hidden;

      background:
        linear-gradient(
          180deg,
          #11161d,
          #252b32 48%,
          #0c1015
        );

      border: 1px solid #65707b;
      border-radius: 3px;

      box-shadow:
        inset 0 2px 5px rgba(0, 0, 0, 0.9),
        inset 0 -1px 2px rgba(255, 255, 255, 0.08),
        0 0 4px rgba(0, 0, 0, 0.8);
    }

    .progress::before {
      content: "";
      position: absolute;
      inset: 2px;
      pointer-events: none;

      background:
        repeating-linear-gradient(
          135deg,
          transparent 0 12px,
          rgba(255, 255, 255, 0.035) 13px,
          transparent 14px
        );
    }

    .progress-fill {
      position: relative;
      width: 80%;
      height: 100%;

      overflow: hidden;

      background:
        linear-gradient(
          180deg,
          #ff4a59 0%,
          #f51835 38%,
          #b90019 100%
        );

      box-shadow:
        inset 0 1px rgba(255, 255, 255, 0.35),
        inset 0 -3px 5px rgba(80, 0, 0, 0.45),
        0 0 7px rgba(255, 0, 35, 0.75);

      transition: width 0.35s ease;
    }

    .progress-fill::after {
      content: "";
      position: absolute;
      top: 0;
      left: -50%;
      width: 50%;
      height: 100%;

      background:
        linear-gradient(
          90deg,
          transparent,
          rgba(255,255,255,0.45),
          transparent
        );

      animation: progressShine 3.5s linear infinite;
    }

    .progress-value {
      min-width: 40px;

      font-size: 25px;
      line-height: 1;
      font-weight: 900;

      text-shadow:
        2px 2px 0 #000,
        0 0 5px rgba(255,255,255,0.15);
    }

    /* =========================================================
       LINHA DE STATUS
       ========================================================= */
    .status-row {
      display: flex;
      align-items: center;
      justify-content: space-between;

      width: 100%;
      margin-top: 15px;
      padding-right: 8px;
    }

    .status {
      display: flex;
      align-items: center;
      gap: 9px;

      font-size: 19px;
      line-height: 1;
      font-weight: 900;
      letter-spacing: 0.2px;
      white-space: nowrap;

      text-shadow: 2px 2px 0 #000;
    }

    .coins {
      color: #ffe34a;
      text-shadow:
        2px 2px 0 #3a2600,
        0 0 7px rgba(255, 207, 20, 0.25);
    }

    .points {
      color: #f5f6fa;
    }

    /* =========================================================
       MOEDA CSS
       ========================================================= */
    .coin {
      position: relative;

      width: 25px;
      height: 25px;
      flex: 0 0 25px;

      border-radius: 50%;

      background:
        radial-gradient(
          circle at 32% 27%,
          #fff4a0 0 8%,
          #ffe33d 20%,
          #f6a900 62%,
          #9a5700 100%
        );

      border: 2px solid #ffd52d;

      box-shadow:
        inset 0 0 0 1px rgba(255, 255, 255, 0.35),
        inset -3px -3px 5px rgba(100, 50, 0, 0.45),
        0 0 7px rgba(255, 190, 0, 0.5);

      animation: coinFloat 2.8s ease-in-out infinite;
    }

    .coin::before {
      content: "";
      position: absolute;
      inset: 5px;

      border-radius: 50%;

      border: 1px solid rgba(140, 78, 0, 0.75);
    }

    .coin::after {
      content: "★";
      position: absolute;

      left: 50%;
      top: 50%;

      transform: translate(-50%, -52%);

      color: #9b5900;
      font-family: Arial, sans-serif;
      font-size: 8px;
      font-weight: 900;
    }

    /* =========================================================
       HABILIDADE INFERIOR
       ========================================================= */
    .ability-row {
      position: absolute;
      left: 24px;
      right: 24px;
      bottom: 20px;

      display: flex;
      align-items: center;
      gap: 10px;

      font-size: 20px;
      font-weight: 900;
      letter-spacing: 0.2px;

      text-shadow:
        2px 2px 0 #000,
        0 0 5px rgba(255,255,255,0.1);
    }

    /* Ícone circular Venom */
    .venom-icon {
      position: relative;

      width: 28px;
      height: 28px;
      flex: 0 0 28px;

      border-radius: 50%;

      background:
        radial-gradient(
          circle at 32% 25%,
          #777e86 0%,
          #3c4249 25%,
          #161a1f 63%,
          #050608 100%
        );

      border: 1px solid #a5adb5;

      box-shadow:
        inset 0 1px 2px rgba(255,255,255,0.35),
        inset -3px -4px 7px rgba(0,0,0,0.8),
        0 0 6px rgba(170,190,210,0.28);
    }

    .venom-icon::before {
      content: "";
      position: absolute;

      left: 6px;
      top: 8px;

      width: 15px;
      height: 8px;

      border-radius: 50% 50% 45% 45%;

      background:
        radial-gradient(
          ellipse at center,
          #f0f3f6 0 20%,
          #858d95 22% 34%,
          #16191d 36%
        );

      transform: rotate(-8deg);

      opacity: 0.8;
    }

    .venom-icon::after {
      content: "";
      position: absolute;

      left: 12px;
      top: 12px;

      width: 5px;
      height: 10px;

      border-left: 1px solid rgba(255,255,255,0.5);
      border-right: 1px solid rgba(255,255,255,0.25);

      transform: skewX(-12deg);
    }

    /* =========================================================
       DETALHES TECNOLÓGICOS
       ========================================================= */
    .circuit {
      position: absolute;
      opacity: 0.3;
      pointer-events: none;
    }

    .circuit.one {
      right: 26px;
      bottom: 48px;
      width: 65px;
      height: 35px;

      border-right: 1px solid rgba(30,130,255,0.35);
      border-bottom: 1px solid rgba(30,130,255,0.35);
    }

    .circuit.one::before,
    .circuit.one::after {
      content: "";
      position: absolute;

      background: rgba(30,130,255,0.5);
    }

    .circuit.one::before {
      right: 10px;
      top: -10px;
      width: 1px;
      height: 45px;
    }

    .circuit.one::after {
      right: 0;
      top: 9px;
      width: 25px;
      height: 1px;
    }

    .circuit.two {
      left: 26px;
      bottom: 52px;
      width: 75px;
      height: 25px;

      border-left: 1px solid rgba(255,30,50,0.32);
      border-bottom: 1px solid rgba(255,30,50,0.32);
    }

    .circuit.two::before {
      content: "";
      position: absolute;

      left: 15px;
      bottom: 7px;

      width: 45px;
      height: 1px;

      background: rgba(255,30,50,0.5);
    }

    .scanline {
      position: absolute;
      left: 0;
      right: 0;
      top: 48%;

      height: 1px;

      background: linear-gradient(
        90deg,
        transparent,
        rgba(255,255,255,0.025),
        transparent
      );

      pointer-events: none;
    }

    /* =========================================================
       ANIMAÇÕES
       ========================================================= */
    @keyframes hudGlow {
      0%,
      100% {
        filter: brightness(0.98);
      }

      50% {
        filter: brightness(1.035);
      }
    }

    @keyframes progressShine {
      0% {
        left: -55%;
      }

      100% {
        left: 120%;
      }
    }

    @keyframes coinFloat {
      0%,
      100% {
        transform: translateY(0) rotateY(0deg);
      }

      50% {
        transform: translateY(-1px) rotateY(12deg);
      }
    }

    /* =========================================================
       RESPONSIVIDADE
       ========================================================= */
    @media (max-width: 620px) {
      :root {
        --hud-scale: 0.9;
      }
    }

    @media (max-width: 540px) {
      :root {
        --hud-scale: 0.78;
      }
    }

    @media (max-width: 430px) {
      :root {
        --hud-scale: 0.62;
      }
    }
  </style>
</head>

<body>

  <!-- =======================================================
       HUD
       ======================================================== -->
  <main class="hud" id="gameHUD">

    <!-- Elementos decorativos da moldura -->
    <div class="tech-line top-left"></div>
    <div class="tech-line top-right"></div>
    <div class="tech-line bottom"></div>

    <div class="corner tl"></div>
    <div class="corner br"></div>

    <div class="circuit one"></div>
    <div class="circuit two"></div>
    <div class="scanline"></div>

    <section class="hud-content">

      <!-- Cabeçalho -->
      <header class="header">

        <h1 class="title">
          HOMEM-ARANHA
        </h1>

        <div class="controls">
          A/D ou ←/→ = andar&nbsp;&nbsp; | &nbsp;V
        </div>

      </header>

      <!-- Barra de progresso -->
      <div class="progress-row">

        <div
          class="progress"
          role="progressbar"
          aria-valuemin="0"
          aria-valuemax="5"
          aria-valuenow="4"
        >
          <div
            class="progress-fill"
            id="progressFill"
          ></div>
        </div>

        <div
          class="progress-value"
          id="progressValue"
        >
          4/5
        </div>

      </div>

      <!-- Moedas e pontos -->
      <div class="status-row">

        <div class="status coins">

          <span
            class="coin"
            aria-hidden="true"
          ></span>

          <span id="coinsText">
            Moedas: 1/10
          </span>

        </div>

        <div class="status points">
          <span id="pointsText">
            Pontos: 100
          </span>
        </div>

      </div>

      <!-- Habilidade -->
      <div class="ability-row">

        <span
          class="venom-icon"
          aria-hidden="true"
        ></span>

        <span>
          TEIA PRETA: VENOM
        </span>

      </div>

    </section>
  </main>

  <script>
    /* =========================================================
       ESTADO DO HUD
       ========================================================= */

    let points = 100;
    let coins = 1;

    let progressCurrent = 4;
    let progressMax = 5;

    /* =========================================================
       ELEMENTOS DOM
       ========================================================= */

    const coinsText = document.getElementById("coinsText");
    const pointsText = document.getElementById("pointsText");
    const progressFill = document.getElementById("progressFill");
    const progressValue = document.getElementById("progressValue");
    const progressBar = document.querySelector(".progress");

    /* =========================================================
       ATUALIZAÇÃO GERAL DO HUD
       ========================================================= */

    function updateHUD() {
      /* Limita moedas ao intervalo válido */
      coins = Math.max(0, Math.min(coins, 10));

      /* Impede valores inválidos na progressão */
      progressMax = Math.max(1, progressMax);
      progressCurrent = Math.max(
        0,
        Math.min(progressCurrent, progressMax)
      );

      /* Atualiza pontos */
      pointsText.textContent = `Pontos: ${points}`;

      /* Atualiza moedas */
      coinsText.textContent = `Moedas: ${coins}/10`;

      /* Calcula percentual */
      const percentage =
        (progressCurrent / progressMax) * 100;

      /* Atualiza barra */
      progressFill.style.width = `${percentage}%`;

      /* Atualiza contador */
      progressValue.textContent =
        `${progressCurrent}/${progressMax}`;

      /* Atualiza atributos de acessibilidade */
      progressBar.setAttribute(
        "aria-valuenow",
        progressCurrent
      );

      progressBar.setAttribute(
        "aria-valuemax",
        progressMax
      );
    }

    /* =========================================================
       ADICIONAR MOEDA
       ========================================================= */

    function addCoin() {
      if (coins < 10) {
        coins += 1;
      }

      updateHUD();
    }

    /* =========================================================
       ADICIONAR PONTOS
       ========================================================= */

    function addPoints(amount) {
      const value = Number(amount);

      if (!Number.isFinite(value)) {
        return;
      }

      points += value;

      updateHUD();
    }

    /* =========================================================
       ALTERAR PROGRESSO
       ========================================================= */

    function setProgress(current, max) {
      const newCurrent = Number(current);
      const newMax = Number(max);

      if (
        !Number.isFinite(newCurrent) ||
        !Number.isFinite(newMax) ||
        newMax <= 0
      ) {
        return;
      }

      progressMax = newMax;
      progressCurrent = newCurrent;

      updateHUD();
    }

    /* =========================================================
       CONTROLES
       ========================================================= */

    function walkLeft() {
      console.log("Ação: andar para esquerda");
    }

    function walkRight() {
      console.log("Ação: andar para direita");
    }

    function venomAction() {
      console.log("Ação: Teia Preta - Venom");
    }

    document.addEventListener("keydown", function(event) {

      /*
       * Ignora teclas repetidas quando o usuário
       * mantém uma tecla pressionada.
       */
      if (event.repeat) {
        return;
      }

      const key = event.key.toLowerCase();

      /* A ou seta esquerda */
      if (key === "a" || event.key === "ArrowLeft") {
        event.preventDefault();
        walkLeft();
        return;
      }

      /* D ou seta direita */
      if (key === "d" || event.key === "ArrowRight") {
        event.preventDefault();
        walkRight();
        return;
      }

      /* V */
      if (key === "v") {
        event.preventDefault();
        venomAction();
      }
    });

    /* =========================================================
       INICIALIZAÇÃO
       ========================================================= */

    updateHUD();

    /*
     * As funções ficam disponíveis globalmente para que possam
     * ser chamadas futuramente por outros scripts ou pelo jogo.
     *
     * Exemplos:
     *
     * addCoin();
     * addPoints(100);
     * setProgress(4, 5);
     */
    window.updateHUD = updateHUD;
    window.addCoin = addCoin;
    window.addPoints = addPoints;
    window.setProgress = setProgress;
  </script>

</body>
</html>

