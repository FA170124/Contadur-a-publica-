<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Malla Curricular Contaduría Pública (UBA)</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f5f7fa;
      color: #333;
      margin: 20px;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
    }
    .grid {
      display: flex;
      gap: 10px;
      overflow-x: auto;
    }
    .column {
      background: #ffffff;
      border-radius: 8px;
      padding: 10px;
      min-width: 250px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .column h2 {
      font-size: 16px;
      margin-top: 0;
      border-bottom: 1px solid #ccc;
      padding-bottom: 5px;
    }
    .subject {
      padding: 8px;
      margin: 5px 0;
      border-radius: 6px;
      cursor: pointer;
      background-color: #e3f2fd;
      position: relative;
    }
    .subject.completed {
      background-color: #c8e6c9;
      text-decoration: line-through;
    }
    .subject.locked {
      background-color: #eee;
      color: #aaa;
      cursor: not-allowed;
    }
    .subject.locked::after {
      content: attr(data-tooltip);
      position: absolute;
      bottom: 100%;
      left: 50%;
      transform: translateX(-50%);
      background: #333;
      color: #fff;
      padding: 5px;
      border-radius: 4px;
      white-space: nowrap;
      font-size: 12px;
      display: none;
    }
    .subject.locked:hover::after {
      display: block;
    }
  </style>
</head>
<body>
  <h1>Malla Curricular - Contaduría Pública (UBA)</h1>
  <div class="grid" id="malla"></div>

  <script>
    const materias = [
      { ciclo: '1° Ciclo I', nombre: 'Filosofía General' },
      { ciclo: '1° Ciclo I', nombre: 'Inglés I' },
      { ciclo: '1° Ciclo I', nombre: 'Introducción a la economía I' },
      { ciclo: '1° Ciclo I', nombre: 'Matemática I' },
      { ciclo: '1° Ciclo I', nombre: 'Teoría Administrativa I' },
      { ciclo: '1° Ciclo II', nombre: 'Inglés II', prerequisitos: ['Inglés I'] },
      { ciclo: '1° Ciclo II', nombre: 'Introducción a la economía II', prerequisitos: ['Introducción a la economía I'] },
      { ciclo: '1° Ciclo II', nombre: 'Matemática II', prerequisitos: ['Matemática I'] },
      { ciclo: '1° Ciclo II', nombre: 'Sociología general' },
      { ciclo: '1° Ciclo II', nombre: 'Teoría administrativa II', prerequisitos: ['Teoría Administrativa I'] },
      // Continúa agregando todas las materias con sus dependencias...
    ];

    const estado = JSON.parse(localStorage.getItem('mallaEstado') || '{}');

    const container = document.getElementById('malla');
    const ciclos = [...new Set(materias.map(m => m.ciclo))];

    ciclos.forEach(ciclo => {
      const col = document.createElement('div');
      col.className = 'column';
      col.innerHTML = `<h2>${ciclo}</h2>`;
      materias.filter(m => m.ciclo === ciclo).forEach(m => {
        const div = document.createElement('div');
        div.className = 'subject';
        div.textContent = m.nombre;
        if (estado[m.nombre]) div.classList.add('completed');
        if (m.prerequisitos && !m.prerequisitos.every(p => estado[p])) {
          div.classList.add('locked');
          div.setAttribute('data-tooltip', 'Requiere: ' + m.prerequisitos.join(', '));
        } else {
          div.addEventListener('click', () => {
            estado[m.nombre] = !estado[m.nombre];
            localStorage.setItem('mallaEstado', JSON.stringify(estado));
            location.reload();
          });
        }
        col.appendChild(div);
      });
      container.appendChild(col);
    });
  </script>
</body>
</html>
