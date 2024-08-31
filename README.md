# SmartWatch Project

## Descrição

Este projeto é uma implementação de um relógio de pulso estilizado usando HTML, CSS e JavaScript. O relógio exibe a hora e a data atual em um formato estilizado, com um design moderno. O código utiliza animações e estilizações CSS para criar um efeito visual atraente.

## Estrutura do Projeto

- **index.html**: O arquivo HTML principal que estrutura o conteúdo do relógio.
- **styles.css**: O arquivo CSS que define o estilo e a aparência do relógio.
- **main.js**: O arquivo JavaScript que atualiza o relógio em tempo real.

## Implementação

### HTML

O arquivo `index.html` contém a estrutura básica da página e inclui o elemento onde o relógio será exibido:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>SmartWatch</title>
</head>
<body>
    <div class="timepiece"></div>

    <script src="main.js"></script>
</body>
</html>
```

# CSS

O arquivo `styles.css` define o estilo do relógio. Ele usa variáveis CSS para cores e estiliza o elemento `.timepiece` para parecer com um relógio de pulso moderno. O código inclui propriedades como bordas, sombras e gradientes para criar um efeito visual atraente.

```css
.timepiece {
  --watch-border-color: #333;
  --watch-background-color: #f0f0f0;
  --screen-background-color: #000;
  font-size: 4.5vmin;
  font-family: 'Courier New', monospace;
  color: #fff;
  padding: 2em 1.5em;
  border: 2px solid var(--watch-border-color);
  border-radius: 2em;
  box-shadow:
    inset 0 0 0 .5em black,
    inset 0 0 1rem 1em rgba(255, 255, 255, 0.25);
  background-color: var(--screen-background-color);
  background-image: linear-gradient(to bottom right, #fff0 50%, rgba(255, 255, 255, 0.25));
  position: relative;
}

.timepiece::before, .timepiece::after {
  content: "";
  position: absolute;
  background-color: var(--watch-border-color);
  z-index: -1;
}

.timepiece::after {
  inset: -.5em 20%;
  border-radius: .5em;
  background-image: 
    linear-gradient(#fff0, rgba(0, 0, 0, 0.5) .5em calc(100% - .5em), #fff0);
}

.timepiece::before {
  inset: -50vh 25%;
  background-image: 
    repeating-linear-gradient(#fff0 0 .3em, rgba(0, 0, 0, 0.125) .3em .5em, #fff0 .5em .8em),
    radial-gradient(circle, #fff0, rgba(0, 0, 0, 0.25) 50%);
}

.keyword  { color: #ddca7e; }
.def      { color: #809bbd; }
.operator { color: #cccccc; }
.property { color: #9a8297; }
.string   { color: #96b38a; }
.number   { color: #d0782a; }

* { box-sizing: border-box; }
body { 
  margin: 0;
  padding: 1em;
  min-height: 100vh;
  background-color: #1d1e22;
  display: grid;
  place-items: center;
  overflow: hidden;
}
```

# JavaScript

O arquivo `main.js` atualiza o relógio em tempo real. Ele usa a função `clock` para obter a hora atual e formatar a exibição. A função `clock` é chamada repetidamente usando `requestAnimationFrame` para manter o relógio atualizado.

```javascript
clock();

function clock() {
  const date = new Date();
  const indent = 2;
  const clockObj = {
    am_pm: date.getHours() >= 12 ? 'pm' : 'am',
    hours: date.getHours() % 12 || 12,
    minutes: date.getMinutes(),
    seconds: date.getSeconds(),
    day: date.toLocaleDateString('en-us', { weekday: 'long' }),
    date: date.getDate(),
    month: date.toLocaleDateString('en-us', { month: 'long' }),
    year: date.getFullYear(),
  };
  const entryFormat = ([key, val]) => {
    return `${'&nbsp'.repeat(indent)}<span class="property">${key}</span>: ${valFormat(val)}`;
  };
  const valFormat = (val) => {
    if (typeof val === 'number') return `<span class="number">${val}</span>`;
    else if (typeof val === 'string') return `<span class="string">"${val}"</span>`;
  };
  document.querySelector(".timepiece").innerHTML = `
    <span class="keyword">const</span> <span class="def">clock</span> = {<br>
    ${Object.entries(clockObj).reduce((str, entry) => str + entryFormat(entry) + ',<br>', '')}};`;
  
  requestAnimationFrame(clock);
}
```
