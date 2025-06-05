# analizador-funciones
A page that makes it easy to understand all types of functions, with their steps.
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>Analizador de Funciones</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.8.0/math.min.js"></script>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    input, button { padding: 8px; margin: 5px; }
    #graph { width: 100%; height: 400px; }
    table { border-collapse: collapse; margin-top: 20px; }
    td, th { border: 1px solid #ccc; padding: 6px 12px; }
  </style>
</head>
<body>
  <h1>Analizador de Funciones</h1>
  <label for="funcion">Escribe una función (en x):</label>
  <input type="text" id="funcion" value="x^2 - 4x + 3" />
  <button onclick="analizarFuncion()">Analizar</button>

  <div id="graph"></div>
  <div id="tabla-valores"></div>
  <div id="analisis"></div>

  <script>
    function analizarFuncion() {
      const exprInput = document.getElementById("funcion").value;
      const expr = math.parse(exprInput).compile();

      const xVals = [];
      const yVals = [];
      let minY = Infinity, maxY = -Infinity;

      for (let x = -10; x <= 10; x += 1) {
        const y = expr.evaluate({ x });
        xVals.push(x);
        yVals.push(y);
        if (y < minY) minY = y;
        if (y > maxY) maxY = y;
      }

      // Graficar
      Plotly.newPlot("graph", [{ x: xVals, y: yVals, type: 'scatter', mode: 'lines+markers' }], { margin: { t: 10 } });

      // Tabla de valores
      let tablaHtml = '<h2>Tabla de Valores</h2><table><tr><th>x</th><th>f(x)</th></tr>';
      for (let i = 0; i < xVals.length; i++) {
        tablaHtml += `<tr><td>${xVals[i]}</td><td>${yVals[i].toFixed(2)}</td></tr>`;
      }
      tablaHtml += '</table>';
      document.getElementById("tabla-valores").innerHTML = tablaHtml;

      // Análisis básico
      const dominio = "( -\u221E , +\u221E )";
      const rango = `( ${minY.toFixed(2)} , ${maxY.toFixed(2)} )`;

      const analisisHtml = `
        <h2>Análisis</h2>
        <p><strong>Dominio:</strong> ${dominio}</p>
        <p><strong>Rango:</strong> ${rango}</p>
        <p><strong>Valor mínimo:</strong> ${minY.toFixed(2)}</p>
        <p><strong>Valor máximo:</strong> ${maxY.toFixed(2)}</p>
      `;

      document.getElementById("analisis").innerHTML = analisisHtml;
    }
  </script>
</body>
</html>
