

const IKONE = { Plaža: '🏖️', Grad: '🏙️', Priroda: '🌿', Kultura: '🏛️', Avantura: '⛰️' };

function dohvatiListu() {
  try { return JSON.parse(localStorage.getItem('bucketList')) || []; }
  catch { return []; }
}

function spremiListu(lista) {
  localStorage.setItem('bucketList', JSON.stringify(lista));
}


const POCETNE_DESTINACIJE = [
  { "id": 1, "naziv": "Santorini",   "zemlja": "Grčka",     "kontinent": "Europa",  "tip": "Plaža",    "opis": "Poznati otok s bijelim kućama i plavim kupolama iznad Egejskog mora.", "posjećeno": false },
  { "id": 2, "naziv": "Kyoto",       "zemlja": "Japan",     "kontinent": "Azija",   "tip": "Kultura",  "opis": "Grad hramova, zen vrtova i tradicionalnih čajnih ceremonija.",         "posjećeno": false },
  { "id": 3, "naziv": "Patagonia",   "zemlja": "Argentina", "kontinent": "Amerika", "tip": "Priroda",  "opis": "Netaknuta divljina na kraju svijeta s ledenjacima i planinama.",        "posjećeno": false },
  { "id": 4, "naziv": "Dubai",       "zemlja": "UAE",       "kontinent": "Azija",   "tip": "Grad",     "opis": "Futuristički grad s najvišim zgradama i luksuznim iskustvima.",         "posjećeno": true  },
  { "id": 5, "naziv": "Kilimanjaro", "zemlja": "Tanzanija", "kontinent": "Afrika",  "tip": "Avantura", "opis": "Najviši vrh Afrike — treking do 5895 metara nadmorske visine.",         "posjećeno": false },
  { "id": 6, "naziv": "Dubrovnik",   "zemlja": "Hrvatska",  "kontinent": "Europa",  "tip": "Kultura",  "opis": "Bisjer Jadrana s povijesnim zidinama i kristalno čistim morem.",        "posjećeno": true  }
];

function inicijalizirajPodatke() {
  if (dohvatiListu().length > 0) return;
  spremiListu(POCETNE_DESTINACIJE);
}

let aktivniFilter = 'Sve';

function prikaziDestinacije() {
  const lista = dohvatiListu();
  const kontejner = document.getElementById('dest-grid');
  if (!kontejner) return;

  const filtered = aktivniFilter === 'Sve'
    ? lista
    : lista.filter(d => d.kontinent === aktivniFilter);

  const label = document.getElementById('list-label');
  if (label) label.textContent = aktivniFilter === 'Sve'
    ? `Sve destinacije (${lista.length})`
    : `${aktivniFilter} (${filtered.length})`;

  if (filtered.length === 0) {
    kontejner.innerHTML = `
      <div class="empty" style="grid-column: 1/-1">
        <div class="empty-icon">🗺️</div>
        <p>Nema destinacija. Dodaj svoju prvu!</p>
      </div>`;
    return;
  }

  kontejner.innerHTML = filtered.map(d => `
    <div class="dest-card${d.posjećeno ? ' visited' : ''}">
      <div class="dest-top">
        <span class="dest-icon">${IKONE[d.tip] || '📍'}</span>
        <div class="dest-actions">
          <button class="btn-icon btn-check${d.posjećeno ? ' active' : ''}"
            onclick="togglePosjećeno(${d.id})"
            title="${d.posjećeno ? 'Označi kao neposjećeno' : 'Označi kao posjećeno'}">✓</button>
          <button class="btn-icon btn-del"
            onclick="ukloniDestinaciju(${d.id})"
            title="Ukloni">✕</button>
        </div>
      </div>
      <div class="dest-name">${d.naziv}</div>
      <div class="dest-country">${d.zemlja}</div>
      <div class="dest-desc">${d.opis}</div>
      <div class="dest-footer">
        <span class="badge badge-cont">${d.kontinent}</span>
        <span class="badge badge-tip">${d.tip}</span>
        ${d.posjećeno ? '<span class="badge badge-done">✓ Posjećeno</span>' : ''}
      </div>
    </div>
  `).join('');
}

function prikaziFiltere() {
  const lista = dohvatiListu();
  const konts = ['Sve', ...new Set(lista.map(d => d.kontinent))];
  const wrap = document.getElementById('filters');
  if (!wrap) return;

  wrap.innerHTML = konts.map(k => `
    <div class="chip${aktivniFilter === k ? ' active' : ''}" onclick="postaviFilter('${k}')">${k}</div>
  `).join('');
}

function postaviFilter(f) {
  aktivniFilter = f;
  prikaziFiltere();
  prikaziDestinacije();
}

function osvjeziStats() {
  const lista = dohvatiListu();
  const posjeceno = lista.filter(d => d.posjećeno).length;
  const ukupno = lista.length;
  const pct = ukupno ? Math.round((posjeceno / ukupno) * 100) : 0;

  const el = (id) => document.getElementById(id);
  if (el('stat-ukupno'))    el('stat-ukupno').textContent    = ukupno;
  if (el('stat-posjeceno')) el('stat-posjeceno').textContent = posjeceno;
  if (el('stat-preostalo')) el('stat-preostalo').textContent = ukupno - posjeceno;
  if (el('stat-pct'))       el('stat-pct').textContent       = pct + '%';
  if (el('progress-fill'))  el('progress-fill').style.width  = pct + '%';
}

function dodajDestinaciju(event) {
  event.preventDefault();
  const alert = document.getElementById('form-alert');

  const naziv   = document.getElementById('inp-naziv')?.value.trim();
  const zemlja  = document.getElementById('inp-zemlja')?.value.trim();
  const kont    = document.getElementById('inp-kont')?.value;
  const tip     = document.getElementById('inp-tip')?.value;
  const opis    = document.getElementById('inp-opis')?.value.trim();

  if (!naziv || !zemlja) {
    prikaziAlert(alert, 'Naziv i zemlja su obavezna polja.', 'err');
    return;
  }

  const lista = dohvatiListu();
  const novi = {
    id: Date.now(),
    naziv,
    zemlja,
    kontinent: kont,
    tip,
    opis: opis || 'Bez opisa.',
    posjećeno: false
  };

  lista.unshift(novi);
  spremiListu(lista);

  prikaziAlert(alert, '✓ Destinacija dodana!', 'ok');
  event.target.reset();

  osvjeziStats();
  prikaziFiltere();
  prikaziDestinacije();
}

function togglePosjećeno(id) {
  const lista = dohvatiListu();
  const dest = lista.find(d => d.id === id);
  if (dest) dest.posjećeno = !dest.posjećeno;
  spremiListu(lista);
  osvjeziStats();
  prikaziDestinacije();
}

function ukloniDestinaciju(id) {
  const lista = dohvatiListu().filter(d => d.id !== id);
  spremiListu(lista);
  osvjeziStats();
  prikaziFiltere();
  prikaziDestinacije();
}

function ucitajKontinente() {
  const wrap = document.getElementById('cont-grid');
  if (!wrap) return;

  const xmlString = `<?xml version="1.0" encoding="UTF-8"?>
<kontinenti>
  <kontinent ime="Europa">
    <opis>Dom bogate povijesti, kulture i raznolike prirode.</opis>
    <zanimljivost>Europa ima više od 50 zemalja na relativno malom prostoru.</zanimljivost>
  </kontinent>
  <kontinent ime="Azija">
    <opis>Najveći kontinent s nevjerojatnom raznolikošću kultura i krajolika.</opis>
    <zanimljivost>U Aziji živi više od 4.5 milijardi ljudi — 60% svjetske populacije.</zanimljivost>
  </kontinent>
  <kontinent ime="Amerika">
    <opis>Od tropskih prašuma Amazone do ledenih vrhova Patagonije.</opis>
    <zanimljivost>Amazon prašuma proizvodi 20% kisika na Zemlji.</zanimljivost>
  </kontinent>
  <kontinent ime="Afrika">
    <opis>Kolijevka civilizacije s nevjerojatnom bioraznolikošću i kulturom.</opis>
    <zanimljivost>Afrika je dom za više od 1500 različitih jezika.</zanimljivost>
  </kontinent>
  <kontinent ime="Oceanija">
    <opis>Zemlja koralnih grebena, netaknutih plaža i jedinstvene faune.</opis>
    <zanimljivost>Australija je jedina zemlja koja je ujedno i cijeli kontinent.</zanimljivost>
  </kontinent>
</kontinenti>`;

  const xml = new DOMParser().parseFromString(xmlString, 'text/xml');
  const konts = xml.querySelectorAll('kontinent');

  wrap.innerHTML = Array.from(konts).map(k => `
    <div class="cont-card">
      <div class="cont-header">
        <span class="cont-ime">${k.getAttribute('ime')}</span>
      </div>
      <p class="cont-opis">${k.querySelector('opis').textContent}</p>
      <div class="cont-fact">💡 ${k.querySelector('zanimljivost').textContent}</div>
    </div>
  `).join('');
}

function posaljiPoruku(event) {
  event.preventDefault();
  const alert = document.getElementById('kontakt-alert');
  prikaziAlert(alert, '✓ Poruka uspješno poslana!', 'ok');
  event.target.reset();
}

function prikaziAlert(el, tekst, tip) {
  if (!el) return;
  el.textContent = tekst;
  el.className = `alert alert-${tip}`;
  el.style.display = 'block';
  setTimeout(() => el.style.display = 'none', 3500);
}

function oznaktiAktivniLink() {
  const trenutna = window.location.pathname.split('/').pop() || 'index.html';
  document.querySelectorAll('.nav-links a').forEach(a => {
    a.classList.toggle('active', a.getAttribute('href') === trenutna);
  });
}

document.addEventListener('DOMContentLoaded', () => {
  inicijalizirajPodatke();
  osvjeziStats();
  prikaziFiltere();
  prikaziDestinacije();
  ucitajKontinente();
  oznaktiAktivniLink();
});
