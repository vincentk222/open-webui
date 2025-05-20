<script>
import { onMount } from 'svelte';

onMount(() => {
  const EXAMPLES = {
    "Basique 3D": [
      { id: "webgl_geometry_cube", name: "Cube 3D", src: "https://threejs.org/examples/webgl_geometry_cube.html", code: "https://github.com/mrdoob/three.js/blob/master/examples/webgl_geometry_cube.html" },
      { id: "webgl_geometry_sphere", name: "Sphère 3D", src: "https://threejs.org/examples/webgl_geometry_sphere.html", code: "https://github.com/mrdoob/three.js/blob/master/examples/webgl_geometry_sphere.html" }
    ],
    "Effets avancés": [
      { id: "webgl_postprocessing_glitch", name: "Effet Glitch", src: "https://threejs.org/examples/webgl_postprocessing_glitch.html", code: "https://github.com/mrdoob/three.js/blob/master/examples/webgl_postprocessing_glitch.html" },
      { id: "webgl_materials_envmaps", name: "Environnement Map", src: "https://threejs.org/examples/webgl_materials_envmaps.html", code: "https://github.com/mrdoob/three.js/blob/master/examples/webgl_materials_envmaps.html" }
    ]
  };

  const listDiv = document.getElementById('exampleList');
  const searchInput = document.getElementById('searchInput');
  const exampleIframe = document.getElementById('exampleIframe');
  const srcButton = document.getElementById('srcButton');
  const exampleName = document.getElementById('exampleName');

  let allCards = [];

  function renderList(filter = "") {
    listDiv.innerHTML = '';
    allCards = [];
    for (const cat in EXAMPLES) {
      const filtered = EXAMPLES[cat].filter(ex => (ex.name + ' ' + ex.id).toLowerCase().includes(filter.toLowerCase()));
      if (filtered.length === 0) continue;
      const title = document.createElement('div');
      title.className = "category-title";
      title.textContent = cat;
      listDiv.appendChild(title);
      for (const example of filtered) {
        const card = document.createElement('div');
        card.className = 'example-card';
        card.dataset.exId = example.id;
        card.innerHTML = `<div class="title">${example.name}</div><div style="font-size:0.9em;color:#8bd;">${example.id}</div>`;
        card.onclick = () => selectExample(example, card);
        listDiv.appendChild(card);
        allCards.push(card);
      }
    }
  }

  let selectedCard = null;
  function selectExample(example, card) {
    allCards.forEach(c => c.classList.remove('selected'));
    card.classList.add('selected');
    selectedCard = card;
    exampleName.textContent = example.name;
    exampleIframe.src = example.src;
    srcButton.href = example.code;
    srcButton.style.display = '';
  }

  searchInput.addEventListener('input', () => {
    renderList(searchInput.value);
    exampleIframe.srcdoc = `
    <div style='height:100%;display:flex;align-items:center;justify-content:center;color:#999; font-size:1.4em;'>
      Sélectionnez un exemple dans la colonne de gauche
    </div>`;
    exampleName.textContent = 'Sélectionnez un exemple';
    srcButton.style.display = 'none';
  });

  renderList();
});
</script>

<svelte:head>
  <title>Artefact: Navigateur d'exemples three.js</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
</svelte:head>

<div id="container">
  <div id="panel">
    <div id="header"><h1>three.js exemples</h1></div>
    <div id="search">
      <input id="searchInput" type="text" placeholder="Filtrer..." autocomplete="off" />
    </div>
    <div id="exampleList"></div>
  </div>

  <div id="main-view">
    <div id="controls">
      <span id="exampleName">Sélectionnez un exemple</span>
      <a id="srcButton" rel="noopener" target="_blank">Voir le code source</a>
    </div>
    <iframe id="exampleIframe" srcdoc="
      <div style='height:100%;display:flex;align-items:center;justify-content:center;color:#999; font-size:1.4em;'>
        Sélectionnez un exemple dans la colonne de gauche
      </div>
    " allowfullscreen></iframe>
  </div>
</div>

<style>
  :global(html, body) {
    height: 100%;
    margin: 0;
    font-family: sans-serif;
    background: #222;
    color: #eee;
  }
  #container {
    display: flex;
    height: 100vh;
  }
  #panel {
    width: 320px;
    background: #23272b;
    border-right: 1px solid #111;
    display: flex;
    flex-direction: column;
    padding: 0 0 1em 0;
  }
  #header {
    padding: 1em;
    border-bottom: 1px solid #444;
  }
  #header h1 { margin: 0; font-size: 1.5em; }
  #search {
    padding: 0.7em 1em 0.5em 1em;
  }
  #searchInput {
    width: 95%;
    padding: 0.3em;
    background: #191c1f;
    border: 1px solid #333;
    color: #eee;
    border-radius: 3px;
  }
  #exampleList {
    flex: 1;
    overflow: auto;
    padding: 0 1em;
  }
  .example-card {
    background: #181a1d;
    margin: 0.5em 0;
    padding: 0.5em 1em;
    border-radius: 4px;
    cursor: pointer;
    transition: background 0.2s;
  }
  .example-card.selected {
    background: #444985;
    color: #fff;
  }
  .example-card .title {
    font-weight: bold;
  }
  #main-view {
    flex: 1;
    display: flex;
    flex-direction: column;
  }
  #controls {
    padding: 0.5em 1em;
    background: #181c20;
    border-bottom: 1px solid #333;
    display: flex;
    align-items: center;
    gap: 1em;
  }
  #srcButton {
    display: none;
    background: #4b90ff;
    color: #fff;
    border: none;
    border-radius: 3px;
    padding: 0.3em 1em;
    text-decoration: none;
    font-size: 1em;
    cursor: pointer;
  }
  #exampleIframe {
    flex: 1;
    width: 100%;
    border: none;
    background: #222;
  }
  .category-title {
    margin: 1em 0 0.15em 0;
    color: #ffea5d;
    font-size: 1.07em;
    font-weight: bold;
    letter-spacing: 0.05em;
  }
</style>
