# amellia-genie-electrique
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Amellia Nkulu - Portfolio & Galerie</title>

<style>
:root {
  --bg: #f2f2f2;
  --card: #ffffff;
  --text: #000;
  --btn: #007bff;
  --border: #ddd;
}

body.dark {
  --bg: #121212;
  --card: #1e1e1e;
  --text: #ffffff;
  --btn: #2196f3;
  --border: #444;
}

body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: var(--bg);
  color: var(--text);
  transition: all 0.3s ease;
}

h1, h2 { text-align: center; }

.container {
  max-width: 850px;
  margin: 20px auto;
  background: var(--card);
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
}

.bio-text {
  line-height: 1.6;
  font-size: 1.1em;
}

input, select, textarea {
  width: 100%;
  padding: 10px;
  margin-top: 8px;
  border: 1px solid var(--border);
  border-radius: 5px;
  background: var(--card);
  color: var(--text);
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 12px;
  background: var(--btn);
  color: white;
  border: none;
  margin-top: 15px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}

button:hover { opacity: 0.85; }

.toggle-btn {
  position: fixed;
  top: 10px;
  right: 10px;
  width: auto;
  z-index: 100;
  padding: 8px 15px;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 15px;
  margin-top: 20px;
}

.photo-card {
  background: var(--card);
  padding: 10px;
  border-radius: 10px;
  border: 1px solid var(--border);
  text-align: center;
}

.photo-card img {
  width: 100%;
  height: 130px;
  object-fit: cover;
  border-radius: 5px;
  cursor: pointer;
}

.table-wrapper {
  overflow-x: auto;
  margin-top: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
  min-width: 1400px; /* Largeur pour accommoder les emails */
}

td {
  border: 1px solid var(--border);
  min-width: 100px;
}

table input {
  border: none;
  text-align: center;
  padding: 8px;
  width: 100%;
  font-size: 0.9em;
}

#viewer {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.9);
  display: none;
  justify-content: center;
  align-items: center;
  z-index: 2000;
}

#viewer img { max-width: 90%; max-height: 90%; border: 3px solid white; }
</style>
</head>

<body>

<button class="toggle-btn" onclick="toggleDark()">🌓 Mode Sombre</button>

<div class="container">
  <h1>Amellia Nkulu</h1>
  <h2>Biographie</h2>
  <div class="bio-text">
    <p>
      Je m’appelle <strong>Amellia Nkulu</strong>, née à <strong>Lubumbashi</strong> le <strong>26 décembre 2007</strong>. 
      J’ai débuté mon parcours scolaire très tôt, en commençant la maternelle Laborat dès l’âge de 2 ans.
    </p>
    <p>
      Après avoir achevé mes études maternelles, j’ai poursuivi mon parcours <strong>au CS Inshallah</strong>, où j’ai effectué six années d’études primaires. 
      Par la suite, j’ai intégré l’<strong>Institut Technique Salama</strong>, dans la filière électricité, ce qui m’a permis d’acquérir une solide base technique.
    </p>
    <p>
      Actuellement, je suis étudiante à l’<strong>Université de Lubumbashi (UNILU)</strong>, à l’<strong>ESI</strong>, où je me spécialise en <strong>génie électrique</strong>. 
      Motivée et passionnée par les technologies électriques, je poursuis mes études avec l’objectif de contribuer activement au développement du secteur énergétique.
    </p>
  </div>
</div>

<div class="container">
  <h3>Ajouter une photo</h3>
  <label>Tranche d'âge :</label>
  <select id="ageSelect">
    <option value="0-5">0 à 5 ans</option>
    <option value="6-9">6 à 9 ans</option>
    <option value="10-14">10 à 14 ans</option>
    <option value="15-19">15 à 19 ans</option>
    <option value="20+">20 ans et plus</option>
  </select>
  <input type="file" id="fileInput" accept="image/*">
  <button onclick="addPhoto()">Enregistrer dans la galerie</button>
</div>

<div id="galleryContainer"></div>

<div class="container">
  <h3>Fiche d'informations (6x16)</h3>
  <div class="table-wrapper">
    <table id="mainTable"></table>
  </div>
</div>

<div id="viewer" onclick="this.style.display='none'">
  <img id="viewerImg">
</div>

<script>
// Données de base
let photosData = JSON.parse(localStorage.getItem("amellia_gallery")) || {};
let tableData = JSON.parse(localStorage.getItem("amellia_table")) || [];

const prenomsHasard = ["Arsel", "Daniella", "Felix", "Gloria", "Isaac", "Lise", "Merveille", "Plamedi", "Sarah", "Nathan", "Esther", "Joël"];

// Informations à insérer
const mesInfos = ["097000001", "Amellia", "Nkulu", "Golf Météo", "amelliankulu@icloud.com"];

function initTable() {
  const table = document.getElementById("mainTable");
  table.innerHTML = "";
  
  for (let r = 0; r < 6; r++) {
    const row = document.createElement("tr");
    tableData[r] = tableData[r] || [];
    
    for (let c = 0; c < 16; c++) {
      const cell = document.createElement("td");
      const input = document.createElement("input");
      
      // Remplissage intelligent
      if (!tableData[r][c]) {
        if (r === 0 && c < mesInfos.length) {
          // Ligne 1 : Vos informations
          tableData[r][c] = mesInfos[c];
        } else {
          // Reste : Prénoms au hasard
          tableData[r][c] = prenomsHasard[Math.floor(Math.random() * prenomsHasard.length)];
        }
      }
      
      input.value = tableData[r][c];
      input.oninput = () => { 
        tableData[r][c] = input.value; 
        localStorage.setItem("amellia_table", JSON.stringify(tableData)); 
      };
      cell.appendChild(input);
      row.appendChild(cell);
    }
    table.appendChild(row);
  }
  localStorage.setItem("amellia_table", JSON.stringify(tableData));
}

// Gestion des photos
function addPhoto() {
  const age = document.getElementById("ageSelect").value;
  const file = document.getElementById("fileInput").files[0];
  if (!file) return alert("Veuillez choisir une image.");

  const reader = new FileReader();
  reader.onload = (e) => {
    if (!photosData[age]) photosData[age] = [];
    photosData[age].push({ src: e.target.result, desc: "" });
    localStorage.setItem("amellia_gallery", JSON.stringify(photosData));
    renderGallery();
  };
  reader.readAsDataURL(file);
}

function renderGallery() {
  const container = document.getElementById("galleryContainer");
  container.innerHTML = "";
  for (let age in photosData) {
    const section = document.createElement("div");
    section.className = "container";
    section.innerHTML = `<h3>Âge : ${age} ans</h3><div class="gallery-grid"></div>`;
    const grid = section.querySelector(".gallery-grid");

    photosData[age].forEach((p, i) => {
      const card = document.createElement("div");
      card.className = "photo-card";
      card.innerHTML = `
        <img src="${p.src}" onclick="openZoom('${p.src}')">
        <textarea oninput="updateDesc('${age}',${i},this.value)" placeholder="Description...">${p.desc}</textarea>
        <button style="background:#ff4d4d; padding:5px; font-size:12px;" onclick="deletePhoto('${age}',${i})">Supprimer</button>
      `;
      grid.appendChild(card);
    });
    container.appendChild(section);
  }
}

function updateDesc(age, i, val) { 
  photosData[age][i].desc = val; 
  localStorage.setItem("amellia_gallery", JSON.stringify(photosData)); 
}

function deletePhoto(age, i) { 
  if(confirm("Supprimer cette photo ?")) {
    photosData[age].splice(i,1); 
    if(!photosData[age].length) delete photosData[age]; 
    localStorage.setItem("amellia_gallery", JSON.stringify(photosData));
    renderGallery();
  }
}

function openZoom(src) { 
  document.getElementById("viewerImg").src = src; 
  document.getElementById("viewer").style.display = "flex"; 
}

function toggleDark() { document.body.classList.toggle("dark"); }

// Lancement
renderGallery();
initTable();
</script>

</body>
</html>
