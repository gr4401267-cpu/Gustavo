<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Painel Headtrick 100% Full Vermelho</title>
  <style>
    * { margin:0; padding:0; box-sizing:border-box; }
    body {
      font-family: Arial, sans-serif;
      background: #000000;           /* fundo preto puro */
      height: 100vh;
      overflow: hidden;
      touch-action: none;            /* melhor drag no celular */
    }

    /* Ícone flutuante centralizado */
    #floating-icon {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 70px;
      height: 70px;
      background: #FF0000;
      border-radius: 50%;
      box-shadow: 0 0 25px rgba(255,0,0,0.7);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 40px;
      font-weight: bold;
      cursor: pointer;
      user-select: none;
      z-index: 9999;
      transition: all 0.2s;
    }

    #floating-icon:active {
      transform: translate(-50%, -50%) scale(0.92);
    }

    /* Painel principal */
    #panel {
      position: absolute;
      width: 320px;
      background: rgba(17,17,17,0.90); /* semi-preto com transparência */
      border: 2px solid #FF0000;
      border-radius: 12px;
      color: white;
      padding: 16px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.8);
      z-index: 10000;
      display: none;               /* começa escondido */
      user-select: none;
      touch-action: none;
    }

    #title-bar {
      background: #FF0000;
      padding: 10px;
      text-align: center;
      font-size: 17px;
      font-weight: bold;
      border-radius: 8px 8px 0 0;
      cursor: move;
      margin: -16px -16px 16px -16px;
    }

    #subtitle {
      text-align: center;
      color: #FFCC00;
      font-size: 14px;
      margin-bottom: 20px;
    }

    .option {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin: 12px 0;
      font-size: 16px;
    }

    .switch {
      position: relative;
      display: inline-block;
      width: 50px;
      height: 26px;
    }

    .switch input { opacity: 0; width: 0; height: 0; }

    .slider {
      position: absolute;
      cursor: pointer;
      top: 0; left: 0; right: 0; bottom: 0;
      background-color: #555;
      transition: .4s;
      border-radius: 26px;
    }

    .slider:before {
      position: absolute;
      content: "";
      height: 20px;
      width: 20px;
      left: 3px;
      bottom: 3px;
      background-color: white;
      transition: .4s;
      border-radius: 50%;
    }

    input:checked + .slider { background-color: #FF0000; }

    input:checked + .slider:before { transform: translateX(24px); }

    #fov-section { margin-top: 20px; }

    #seekbar-fov {
      width: 100%;
      height: 8px;
      background: #444;
      border-radius: 4px;
      outline: none;
      appearance: none;
    }

    #seekbar-fov::-webkit-slider-thumb {
      appearance: none;
      width: 20px;
      height: 20px;
      background: #FF0000;
      border-radius: 50%;
      cursor: pointer;
    }

    #fov-value {
      text-align: center;
      color: #FFCC00;
      font-size: 18px;
      margin-top: 8px;
    }

    #close-btn {
      width: 100%;
      padding: 12px;
      background: #FF0000;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      margin-top: 20px;
      cursor: pointer;
    }

    /* Novo: Círculo vermelho FoV na tela */
    #fov-circle {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      border: 2px solid #FF0000;
      border-radius: 50%;
      pointer-events: none;        /* não interfere em cliques */
      display: none;
      z-index: 9998;               /* abaixo do painel e ícone */
      box-shadow: 0 0 15px rgba(255,0,0,0.5);
    }
  </style>
</head>
<body>

  <!-- Botão central -->
  <div id="floating-icon">+</div>

  <!-- Painel (visível quando aberto) -->
  <div id="panel">
    <div id="title-bar">Painel Headtrick 100% Full Vermelho</div>
    <div id="subtitle"> @gustavo0526_</div>

    <div class="option">
      <span>Aim Lock</span>
      <label class="switch">
        <input type="checkbox" id="chk-aimlock">
        <span class="slider"></span>
      </label>
    </div>

    <div class="option">
      <span>No Recoil</span>
      <label class="switch">
        <input type="checkbox" id="chk-norecoil">
        <span class="slider"></span>
      </label>
    </div>

    <div class="option">
      <span>Auxílio de mira</span>
      <label class="switch">
        <input type="checkbox" id="chk-auxilio">
        <span class="slider"></span>
      </label>
    </div>

    <div class="option">
      <span>Desenhar Círculo FoV</span>
      <label class="switch">
        <input type="checkbox" id="chk-fovcircle">
        <span class="slider"></span>
      </label>
    </div>

    <div id="fov-section">
      <div>Regular Aim - FoV</div>
      <input type="range" min="0" max="100" value="10" id="seekbar-fov">
      <div id="fov-value">1.0</div>
    </div>

    <button id="close-btn">Fechar Painel</button>
  </div>

  <!-- Novo: Elemento para o círculo vermelho -->
  <div id="fov-circle"></div>

  <script>
    const icon = document.getElementById('floating-icon');
    const panel = document.getElementById('panel');
    const titleBar = document.getElementById('title-bar');
    const closeBtn = document.getElementById('close-btn');
    const fovCircle = document.getElementById('fov-circle');
    const chkFovCircle = document.getElementById('chk-fovcircle');
    const seekbar = document.getElementById('seekbar-fov');
    const fovValue = document.getElementById('fov-value');

    let isDragging = false;
    let currentX = 0, currentY = 0, initialX, initialY;

    // Toggle painel com ícone central
    icon.addEventListener('click', () => {
      if (panel.style.display === 'block') {
        panel.style.display = 'none';
        icon.textContent = '+';
      } else {
        panel.style.display = 'block';
        icon.textContent = '×';
        // Centraliza o painel na primeira vez
        if (!panel.dataset.positioned) {
          panel.style.left = (window.innerWidth / 2 - 160) + 'px';
          panel.style.top = (window.innerHeight / 3) + 'px'; // um pouco acima do centro
          panel.dataset.positioned = 'true';
        }
      }
    });

    // Fechar com botão
    closeBtn.addEventListener('click', () => {
      panel.style.display = 'none';
      icon.textContent = '+';
    });

    // Drag do painel pela barra de título
    titleBar.addEventListener('mousedown', startDrag);
    titleBar.addEventListener('touchstart', startDrag, { passive: false });

    function startDrag(e) {
      if (e.type === 'touchstart') {
        initialX = e.touches[0].clientX - currentX;
        initialY = e.touches[0].clientY - currentY;
      } else {
        initialX = e.clientX - currentX;
        initialY = e.clientY - currentY;
      }
      isDragging = true;
      document.addEventListener('mousemove', drag);
      document.addEventListener('touchmove', drag, { passive: false });
      document.addEventListener('mouseup', stopDrag);
      document.addEventListener('touchend', stopDrag);
    }

    function drag(e) {
      if (isDragging) {
        e.preventDefault();
        if (e.type === 'touchmove') {
          currentX = e.touches[0].clientX - initialX;
          currentY = e.touches[0].clientY - initialY;
        } else {
          currentX = e.clientX - initialX;
          currentY = e.clientY - initialY;
        }
        panel.style.left = currentX + 'px';
        panel.style.top = currentY + 'px';
      }
    }

    function stopDrag() {
      isDragging = false;
      document.removeEventListener('mousemove', drag);
      document.removeEventListener('touchmove', drag);
      document.removeEventListener('mouseup', stopDrag);
      document.removeEventListener('touchend', stopDrag);
    }

    // Switches - logs + simulação (adicione lógica real aqui)
    document.querySelectorAll('input[type="checkbox"]').forEach(chk => {
      chk.addEventListener('change', (e) => {
        const label = e.target.closest('.option').querySelector('span').textContent;
        const status = e.target.checked ? 'ATIVADO' : 'DESATIVADO';
        console.log(`${label}: ${status}`);
        // alert(`${label}: ${status}`); // descomente se quiser popup

        // Específico para FoV Circle
        if (e.target.id === 'chk-fovcircle') {
          if (e.target.checked) {
            fovCircle.style.display = 'block';
            updateFovCircle(); // Atualiza tamanho inicial
          } else {
            fovCircle.style.display = 'none';
          }
        }
      });
    });

    // Slider FoV
    seekbar.addEventListener('input', () => {
      const val = 0.5 + (seekbar.value / 100) * 4.5;
      fovValue.textContent = val.toFixed(1);
      console.log(`FoV: ${val.toFixed(1)}`);
      updateFovCircle(val); // Atualiza o círculo em tempo real
    });

    // Função para atualizar o tamanho do círculo baseado no FoV
    function updateFovCircle(val = parseFloat(fovValue.textContent)) {
      if (chkFovCircle.checked) {
        const radius = val * 100; // Ajuste o multiplicador pra tamanho real (ex: val * 100 pixels)
        fovCircle.style.width = `${radius * 2}px`;
        fovCircle.style.height = `${radius * 2}px`;
      }
    }
  </script>
</body>
</html>
