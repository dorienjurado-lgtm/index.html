# index.html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Juego de Partes de Armas</title>
<style>
  body {
    font-family: Arial, sans-serif;
    margin: 20px;
  }
  #area-dibujo svg {
    border: 1px solid #ccc;
    max-width: 500px;
    margin-bottom: 20px;
  }
  .parte {
    cursor: pointer;
    fill: #ddd;
    stroke: #444;
    stroke-width: 1;
  }
  .parte:hover {
    fill: #aaa;
  }
  .resaltado {
    fill: yellow !important;
  }
  #resultado {
    font-weight: bold;
    margin-top: 10px;
  }
  button {
    margin-top: 10px;
  }
</style>
</head>
<body>

<h1>Juego: Identifique las partes del arma</h1>

<label for="arma-select">Seleccione un arma:</label>
<select id="arma-select">
  <option value="revolver">Revolver .38 SPL</option>
  <option value="glock">Pistola Glock 19</option>
  <option value="escopeta">Escopeta 12/70</option>
</select>

<div id="area-dibujo"></div>

<div>
  <p><strong>Pregunta:</strong> Haz clic en la parte del arma llamada <span id="pregunta-parte"></span></p>
  <p id="resultado"></p>
  <button id="siguiente-btn">Siguiente parte</button>
</div>

<script>
  const armasDatos = {
    revolver: {
      partes: [
        {id:"cañon", nombre:"Caño"},
        {id:"boca", nombre:"Boca de cañón"},
        {id:"carcasa", nombre:"Carcón"},
        {id:"alza", nombre:"Alza"},
        {id:"muesca", nombre:"Muesca"},
        {id:"puente", nombre:"Puente"},
        {id:"gatillo", nombre:"Gatillo"},
        {id:"guardamonte", nombre:"Guardamonte"},
        {id:"martillo", nombre:"Martillo"},
        {id:"cargador", nombre:"Cacha de revólver"}
        // Podrías agregar más si quieres
      ],
      dibujar: () => {
        // SVG simplificado para revolver - rectángulos y círculos
        return `
        <svg viewBox="0 0 300 150" width="500" height="250" >
          <rect id="cañon" class="parte" x="180" y="50" width="100" height="20" />
          <rect id="boca" class="parte" x="280" y="50" width="20" height="20" />
          <rect id="carcasa" class="parte" x="80" y="60" width="100" height="50" />
          <rect id="alza" class="parte" x="140" y="30" width="20" height="10" />
          <rect id="muesca" class="parte" x="120" y="25" width="10" height="20" />
          <rect id="puente" class="parte" x="80" y="50" width="30" height="10" />
          <circle id="gatillo" class="parte" cx="70" cy="110" r="15" />
          <rect id="guardamonte" class="parte" x="50" y="90" width="40" height="20" />
          <rect id="martillo" class="parte" x="70" y="20" width="20" height="20" />
          <rect id="cargador" class="parte" x="20" y="110" width="50" height="30" />
        </svg>
        `;
      }
    },
    glock: {
      partes: [
        {id:"cañon", nombre:"Boca (cañón)"},
        {id:"punto_mira", nombre:"Punto de mira"},
        {id:"corredera", nombre:"Corredera"},
        {id:"calibre", nombre:"Calibre"},
        {id:"alza", nombre:"Alza"},
        {id:"palanca_retorno", nombre:"Palanca de retención de la corredera"},
        {id:"gatillo", nombre:"Gatillo"},
        {id:"seguro_gatillo", nombre:"Seguro de gatillo"},
        {id:"guardamonte", nombre:"Guardamonte"},
        {id:"cargador", nombre:"Cargador"}
      ],
      dibujar: () => {
        // SVG simplificado para pistola Glock
        return `
        <svg viewBox="0 0 300 120" width="500" height="200">
          <rect id="cañon" class="parte" x="180" y="30" width="100" height="20" />
          <circle id="punto_mira" class="parte" cx="270" cy="25" r="6" />
          <rect id="corredera" class="parte" x="150" y="25" width="110" height="30" />
          <text x="210" y="70" font-size="10" fill="#444">9x19</text>
          <rect id="alza" class="parte" x="210" y="15" width="20" height="10" />
          <rect id="palanca_retorno" class="parte" x="130" y="45" width="20" height="10" />
          <circle id="gatillo" class="parte" cx="90" cy="80" r="15" />
          <rect id="seguro_gatillo" class="parte" x="80" y="70" width="20" height="10" />
          <rect id="guardamonte" class="parte" x="40" y="70" width="40" height="20" />
          <rect id="cargador" class="parte" x="10" y="90" width="60" height="40" />
        </svg>
        `;
      }
    },
    escopeta: {
      partes: [
        {id:"cañon", nombre:"Cañón"},
        {id:"culata", nombre:"Culata"},
        {id:"cantorera", nombre:"Cantorera"},
        {id:"destrabador", nombre:"Destrabador"},
        {id:"guardamonte", nombre:"Guardamonte"},
        {id:"disparador", nombre:"Disparador"},
        {id:"seguro", nombre:"Seguro"},
        {id:"ventana_eyeccion", nombre:"Ventana de eyección"},
        {id:"tubo_abastecimiento", nombre:"Tubo de abastecimiento"},
        {id:"alza_guion", nombre:"Alza y guión"}
      ],
      dibujar: () => {
        // SVG simplificado para escopeta
        return `
        <svg viewBox="0 0 350 120" width="500" height="200">
          <rect id="cañon" class="parte" x="250" y="40" width="90" height="20" />
          <polygon id="culata" class="parte" points="10,80 50,40 80,60 60,100" />
          <rect id="cantorera" class="parte" x="10" y="70" width="15" height="10" />
          <rect id="destrabador" class="parte" x="120" y="70" width="20" height="20" />
          <rect id="guardamonte" class="parte" x="95" y="80" width="30" height="15" />
          <circle id="disparador" class="parte" cx="110" cy="70" r="10" />
          <rect id="seguro" class="parte" x="130" y="50" width="15" height="10" />
          <rect id="ventana_eyeccion" class="parte" x="220" y="25" width="30" height="10" />
          <rect id="tubo_abastecimiento" class="parte" x="200" y="60" width="150" height="10" />
          <rect id="alza_guion" class="parte" x="270" y="30" width="30" height="10" />
        </svg>
        `;
      }
    }
  };

  let armaActual = 'revolver';
  let partesArma = [];
  let parteActual = null;
  const areaDibujo = document.getElementById('area-dibujo');
  const preguntaParte = document.getElementById('pregunta-parte');
  const resultado = document.getElementById('resultado');
  const armaSelect = document.getElementById('arma-select');
  const siguienteBtn = document.getElementById('siguiente-btn');

  function cargarArma(arma) {
    armaActual = arma;
    areaDibujo.innerHTML = armasDatos[arma].dibujar();
    partesArma = armasDatos[arma].partes.slice();
    resultado.textContent = '';
    siguienteBtn.disabled = false;
    siguientePregunta();
    agregarEventosPartes();
  }

  function siguientePregunta() {
    if(partesArma.length === 0) {
      preguntaParte.textContent = "¡Has identificado todas las partes! 🎉";
      siguienteBtn.disabled = true;
      return;
    }
    const idx = Math.floor(Math.random() * partesArma.length);
    parteActual = partesArma[idx];
    preguntaParte.textContent = parteActual.nombre;
    resultado.textContent = '';
    destacarParte(null); // Quitar resaltado
  }

  function agregarEventosPartes() {
    const partesSVG = document.querySelectorAll('#area-dibujo .parte');
    partesSVG.forEach(el => {
      el.addEventListener('click', () => {
        if(!parteActual) return;
        if(el.id === parteActual.id) {
          resultado.textContent = "¡Correcto!";
          partesArma = partesArma.filter(p => p.id !== parteActual.id);
          destacarParte(el.id);
          parteActual = null;
        } else {
          resultado.textContent = "Incorrecto, intenta de nuevo.";
          destacarParte(null);
        }
      });
    });
  }

  function destacarParte(id) {
    document.querySelectorAll('.parte').forEach(el => {
      el.classList.remove('resaltado');
    });
    if(id) {
      const elem = document.getElementById(id);
      if(elem) elem.classList.add('resaltado');
    }
  }

  armaSelect.addEventListener('change', e => {
    cargarArma(e.target.value);
  });

  siguienteBtn.addEventListener('click', () => {
    siguientePregunta();
  });

  // Cargar arma inicial
  cargarArma(armaActual);

</script>

</body>
</html>
