<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Pinezka</title>
  <style>
    /* ====== Reset & zmienne ====== */
    :root {
      --bg: #0f1216;
      --panel: #141922;
      --text: #e6e8ea;
      --muted: #9aa3ad;
      --accent: #4db6ff;
      --accent-2: #ff7aa2;
      --border: #232a36;
      --ok: #36d399;
      --warn: #ffcf5c;
      --danger: #ff5c7a;
      --radius: 12px;
      --shadow: 0 8px 24px rgba(0,0,0,0.25);
      --font-size: 16px;
    }
    [data-theme="light"] {
      --bg: #f6f7fb;
      --panel: #ffffff;
      --text: #1b2430;
      --muted: #5c6b7a;
      --accent: #1f7aff;
      --accent-2: #ff4d6d;
      --border: #e7eaf0;
      --shadow: 0 8px 24px rgba(0,0,0,0.08);
    }
    * { box-sizing: border-box; }
    html, body { height: 100%; }
    body {
      margin: 0;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, "Helvetica Neue", Arial, "Noto Sans", "Apple Color Emoji", "Segoe UI Emoji";
      background: var(--bg);
      color: var(--text);
      font-size: var(--font-size);
    }

    /* ====== Layout ====== */
    .app {
      display: grid;
      grid-template-columns: 300px 1fr;
      grid-template-rows: 64px 1fr;
      grid-template-areas:
        "header header"
        "sidebar main";
      gap: 16px;
      padding: 16px;
      min-height: 100vh;
    }
    header {
      grid-area: header;
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 12px 16px;
    }
    .brand { display: flex; align-items: center; gap: 12px; }
    .logo {
      width: 36px; height: 36px;
    }
    .brand h1 { font-size: 18px; margin: 0; }
    .clock { font-variant-numeric: tabular-nums; color: var(--muted); }

    aside {
      grid-area: sidebar;
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 16px;
      display: flex;
      flex-direction: column;
      gap: 16px;
      position: sticky;
      top: 16px;
      height: calc(100vh - 96px);
      overflow: auto;
    }
    main {
      grid-area: main;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
    }

    /* ====== Karty ====== */
    .card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 16px;
      display: flex;
      flex-direction: column;
      gap: 12px;
      min-height: 280px;
    }
    .card h2 {
      margin: 0;
      font-size: 18px;
    }
    .row { display: flex; gap: 8px; flex-wrap: wrap; }
    .input, .select, .button {
      border: 1px solid var(--border);
      background: transparent;
      color: var(--text);
      padding: 10px 12px;
      border-radius: 10px;
      outline: none;
    }
    .input:focus, .select:focus { border-color: var(--accent); }
    .button {
      cursor: pointer;
      background: linear-gradient(180deg, rgba(255,255,255,0.05), rgba(0,0,0,0.05));
    }
    .button.primary { background: var(--accent); color: #fff; border: none; }
    .button.danger { background: var(--danger); color: #fff; border: none; }
    .button.warn { background: var(--warn); color: #1b2430; border: none; }
    .tag {
      display: inline-flex; align-items: center; gap: 6px;
      background: rgba(77,182,255,0.12);
      color: var(--accent);
      border: 1px dashed var(--accent);
      border-radius: 999px;
      padding: 6px 10px;
      font-size: 12px;
    }

    /* ====== Lista notatek ====== */
    .notes-list { display: grid; gap: 10px; grid-auto-rows: minmax(80px, auto); }
    .note {
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 10px;
      background: rgba(255,255,255,0.03);
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 8px;
    }
    .note.pinned { border-color: var(--accent); box-shadow: inset 0 0 0 1px var(--accent); }
    .note-title { font-weight: 600; }
    .note-meta { color: var(--muted); font-size: 12px; }
    .note-actions { display: flex; gap: 6px; }

    /* ====== Galeria ====== */
    .gallery-controls { display: flex; gap: 8px; flex-wrap: wrap; }
    .dropzone {
      border: 2px dashed var(--border);
      border-radius: 12px;
      padding: 16px;
      text-align: center;
      color: var(--muted);
    }
    .dropzone.drag { border-color: var(--accent); color: var(--accent); }
    .gallery.grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
      gap: 10px;
    }
    .gallery.list { display: grid; gap: 10px; }
    .photo {
      border: 1px solid var(--border);
      border-radius: 10px;
      overflow: hidden;
      background: rgba(255,255,255,0.03);
      display: flex; flex-direction: column;
    }
    .photo img { width: 100%; height: 160px; object-fit: cover; background: #0b0e12; }
    .photo .caption {
      padding: 8px; display: flex; align-items: center; gap: 8px;
    }
    .photo .caption input {
      flex: 1; background: transparent; border: 1px dashed var(--border);
      border-radius: 8px; padding: 6px 8px; color: var(--text);
    }

    /* ====== Przybornik ====== */
    .tool {
      border: 1px dashed var(--border);
      border-radius: 10px;
      padding: 10px;
      display: grid; gap: 8px;
    }
    .meter {
      height: 10px; border-radius: 999px; background: #222838; overflow: hidden;
    }
    .meter > div { height: 100%; background: var(--accent); width: 0%; transition: width 0.3s; }

    /* ====== Responsywność ====== */
    @media (max-width: 900px) {
      .app { grid-template-columns: 1fr; grid-template-areas: "header" "main"; }
      aside { position: static; height: auto; }
      main { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
  <div class="app" id="app">
    <header>
      <div class="brand">
        <!-- Prosty SVG pinezki -->
        <svg class="logo" viewBox="0 0 64 64" aria-hidden="true">
          <defs>
            <linearGradient id="g" x1="0" x2="1">
              <stop offset="0" stop-color="#4db6ff"/>
              <stop offset="1" stop-color="#ff7aa2"/>
            </linearGradient>
          </defs>
          <circle cx="32" cy="20" r="14" fill="url(#g)" />
          <path d="M32 34 L28 50 L32 44 L36 50 Z" fill="#ff7aa2" opacity="0.85"/>
        </svg>
        <h1>Pinezka</h1>
      </div>
      <div class="row">
        <span class="clock" id="clock">--:--</span>
        <select class="select" id="themeSelect" aria-label="Motyw">
          <option value="dark">Ciemny</option>
          <option value="light">Jasny</option>
        </select>
        <select class="select" id="fontSelect" aria-label="Rozmiar tekstu">
          <option value="14">Mały</option>
          <option value="16" selected>Średni</option>
          <option value="18">Duży</option>
        </select>
      </div>
    </header>

    <aside>
      <h2>Przybornik</h2>
      <div class="tool">
        <strong>Motyw:</strong>
        <div class="row">
          <button class="button" data-theme="dark">Ciemny</button>
          <button class="button" data-theme="light">Jasny</button>
        </div>

        <strong>Widok galerii:</strong>
        <div class="row">
          <button class="button" id="viewGrid">Siatka</button>
          <button class="button" id="viewList">Lista</button>
        </div>

        <strong>Eksport/Import:</strong>
        <div class="row">
          <button class="button primary" id="exportBtn">Eksportuj JSON</button>
          <label class="button" style="cursor:pointer;">
            Importuj JSON
            <input type="file" id="importInput" accept=".json" style="display:none" />
          </label>
          <button class="button warn" id="clearAllBtn">Wyczyść wszystko</button>
        </div>

        <strong>Pomodoro:</strong>
        <div class="row">
          <button class="button" id="pomodoroStart">Start (25 min)</button>
          <button class="button" id="pomodoroStop">Stop</button>
        </div>
        <div class="meter"><div id="pomodoroMeter"></div></div>
        <small class="note-meta" id="pomodoroLabel">Gotowy.</small>
      </div>

      <div class="tool">
        <strong>Szybkie szablony notatek:</strong>
        <div class="row">
          <button class="button" data-template="Lista zadań">Lista zadań</button>
          <button class="button" data-template="Pomysł projektu">Pomysł projektu</button>
          <button class="button" data-template="Notatka ze spotkania">Notatka ze spotkania</button>
        </div>
      </div>
    </aside>

    <main>
      <!-- Notatnik -->
      <section class="card" id="notesCard">
        <h2>Notatnik</h2>
        <div class="row">
          <input class="input" id="noteTitle" placeholder="Tytuł..." />
          <input class="input" id="noteTags" placeholder="Tagi (np. praca, foto)..." />
        </div>
        <textarea class="input" id="noteBody" placeholder="Twoja notatka..." rows="5"></textarea>
        <div class="row">
          <button class="button primary" id="addNoteBtn">Dodaj notatkę</button>
          <button class="button" id="updateNoteBtn" disabled>Aktualizuj</button>
          <button class="button warn" id="resetFormBtn">Wyczyść formularz</button>
        </div>
        <div class="row">
          <input class="input" id="searchNotes" placeholder="Szukaj w notatkach..." />
        </div>
        <div class="notes-list" id="notesList"></div>
      </section>

      <!-- Zdjęcia -->
      <section class="card" id="photosCard">
        <h2>Zdjęcia</h2>
        <div class="gallery-controls">
          <div class="row">
            <label class="button" style="cursor:pointer;">
              Wgraj zdjęcia
              <input type="file" id="photoInput" accept="image/*" multiple style="display:none" />
            </label>
            <button class="button" id="deleteAllPhotos">Usuń wszystkie</button>
          </div>
        </div>
        <div class="dropzone" id="dropzone">Przeciągnij i upuść zdjęcia tutaj</div>
        <div class="gallery grid" id="gallery"></div>
      </section>
    </main>
  </div>

  <script>
    // ====== Util ======
    const $ = (sel, root = document) => root.querySelector(sel);
    const $$ = (sel, root = document) => Array.from(root.querySelectorAll(sel));
    const storage = {
      get(key, fallback) {
        try { const v = localStorage.getItem(key); return v ? JSON.parse(v) : fallback; }
        catch(e) { return fallback; }
      },
      set(key, val) { localStorage.setItem(key, JSON.stringify(val)); }
    };

    // ====== Stan aplikacji ======
    const state = {
      notes: storage.get('pinezka.notes', []),   // [{id, title, body, tags[], pinned, createdAt, updatedAt}]
      photos: storage.get('pinezka.photos', []), // [{id, dataUrl, caption, createdAt}]
      ui: storage.get('pinezka.ui', { theme: 'dark', fontSize: 16, galleryView: 'grid' }),
      editingNoteId: null,
      pomodoro: { running: false, start: 0, duration: 25 * 60 * 1000 }
    };

    // ====== Inicjalizacja UI ======
    function initUI() {
      document.body.dataset.theme = state.ui.theme;
      document.documentElement.style.setProperty('--font-size', state.ui.fontSize + 'px');
      $('#themeSelect').value = state.ui.theme;
      $('#fontSelect').value = String(state.ui.fontSize);
      renderClock();
      setInterval(renderClock, 1000);
      updatePomodoro(0);
    }

    function renderClock() {
      const now = new Date();
      const h = String(now.getHours()).padStart(2, '0');
      const m = String(now.getMinutes()).padStart(2, '0');
      $('#clock').textContent = `${h}:${m}`;
    }

    // ====== Notatki ======
    function renderNotes() {
      const list = $('#notesList');
      list.innerHTML = '';
      const q = ($('#searchNotes').value || '').toLowerCase().trim();

      let notes = [...state.notes];
      if (q) {
        notes = notes.filter(n =>
          (n.title + ' ' + n.body + ' ' + (n.tags || []).join(' ')).toLowerCase().includes(q)
        );
      }
      notes.sort((a, b) => (b.pinned - a.pinned) || (b.updatedAt - a.updatedAt));

      for (const n of notes) {
        const el = document.createElement('div');
        el.className = 'note' + (n.pinned ? ' pinned' : '');
        el.innerHTML = `
          <div>
            <div class="note-title">${escapeHTML(n.title || 'Bez tytułu')}</div>
            <div class="note-meta">
              ${formatDate(n.updatedAt)} • ${n.tags && n.tags.length ? 'Tagi: ' + n.tags.map(t => `<span class="tag">${escapeHTML(t)}</span>`).join(' ') : 'Brak tagów'}
            </div>
            <div class="note-body">${escapeHTML(n.body || '').replace(/\n/g, '<br>')}</div>
          </div>
          <div class="note-actions">
            <button class="button" data-action="edit">Edytuj</button>
            <button class="button" data-action="pin">${n.pinned ? 'Odepnij' : 'Przypnij'}</button>
            <button class="button danger" data-action="delete">Usuń</button>
          </div>
        `;
        el.querySelector('[data-action="edit"]').onclick = () => editNote(n.id);
        el.querySelector('[data-action="pin"]').onclick = () => togglePin(n.id);
        el.querySelector('[data-action="delete"]').onclick = () => deleteNote(n.id);
        list.appendChild(el);
      }
      storage.set('pinezka.notes', state.notes);
    }

    function addNote() {
      const title = $('#noteTitle').value.trim();
      const body = $('#noteBody').value.trim();
      const tags = ($('#noteTags').value || '').split(',').map(s => s.trim()).filter(Boolean);

      if (!title && !body) return alert('Wpisz treść lub tytuł notatki.');

      const now = Date.now();
      const note = {
        id: cryptoRandomId(),
        title, body, tags,
        pinned: false,
        createdAt: now,
        updatedAt: now
      };
      state.notes.push(note);
      resetNoteForm();
      renderNotes();
    }

    function editNote(id) {
      const note = state.notes.find(n => n.id === id);
      if (!note) return;
      state.editingNoteId = id;
      $('#noteTitle').value = note.title || '';
      $('#noteBody').value = note.body || '';
      $('#noteTags').value = (note.tags || []).join(', ');
      $('#addNoteBtn').disabled = true;
      $('#updateNoteBtn').disabled = false;
    }

    function updateNote() {
      const note = state.notes.find(n => n.id === state.editingNoteId);
      if (!note) return;
      note.title = $('#noteTitle').value.trim();
      note.body = $('#noteBody').value.trim();
      note.tags = ($('#noteTags').value || '').split(',').map(s => s.trim()).filter(Boolean);
      note.updatedAt = Date.now();
      state.editingNoteId = null;
      $('#addNoteBtn').disabled = false;
      $('#updateNoteBtn').disabled = true;
      resetNoteForm();
      renderNotes();
    }

    function togglePin(id) {
      const note = state.notes.find(n => n.id === id);
      if (!note) return;
      note.pinned = !note.pinned;
      note.updatedAt = Date.now();
      renderNotes();
    }

    function deleteNote(id) {
      if (!confirm('Usunąć tę notatkę?')) return;
      state.notes = state.notes.filter(n => n.id !== id);
      renderNotes();
    }

    function resetNoteForm() {
      $('#noteTitle').value = '';
      $('#noteBody').value = '';
      $('#noteTags').value = '';
      $('#addNoteBtn').disabled = false;
      $('#updateNoteBtn').disabled = true;
    }

    // ====== Zdjęcia ======
    function renderPhotos() {
      const gallery = $('#gallery');
      gallery.className = 'gallery ' + state.ui.galleryView;
      gallery.innerHTML = '';
      for (const p of state.photos) {
        const el = document.createElement('div');
        el.className = 'photo';
        el.innerHTML = `
          <img src="${p.dataUrl}" alt="Photo" />
          <div class="caption">
            <input value="${escapeHTML(p.caption || '')}" placeholder="Podpis..." />
            <button class="button danger">Usuń</button>
          </div>
        `;
        el.querySelector('input').oninput = (e) => {
          p.caption = e.target.value;
          storage.set('pinezka.photos', state.photos);
        };
        el.querySelector('.button.danger').onclick = () => {
          if (!confirm('Usunąć to zdjęcie?')) return;
          state.photos = state.photos.filter(x => x.id !== p.id);
          renderPhotos();
        };
        gallery.appendChild(el);
      }
      storage.set('pinezka.photos', state.photos);
    }

    function addPhotos(files) {
      if (!files || !files.length) return;
      const tasks = Array.from(files).map(file => new Promise((resolve) => {
        const reader = new FileReader();
        reader.onload = () => resolve({ id: cryptoRandomId(), dataUrl: reader.result, caption: '', createdAt: Date.now() });
        reader.readAsDataURL(file);
      }));
      Promise.all(tasks).then(items => {
        state.photos.push(...items);
        renderPhotos();
      });
    }

    // ====== Eksport/Import/Wyczyść ======
    function exportData() {
      const data = {
        notes: state.notes,
        photos: state.photos,
        ui: state.ui
      };
      const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'pinezka-export.json';
      a.click();
      URL.revokeObjectURL(url);
    }
    function importData(file) {
      const reader = new FileReader();
      reader.onload = () => {
        try {
          const data = JSON.parse(reader.result);
          state.notes = Array.isArray(data.notes) ? data.notes : [];
          state.photos = Array.isArray(data.photos) ? data.photos : [];
          state.ui = Object.assign({ theme: 'dark', fontSize: 16, galleryView: 'grid' }, data.ui || {});
          storage.set('pinezka.notes', state.notes);
          storage.set('pinezka.photos', state.photos);
          storage.set('pinezka.ui', state.ui);
          initUI();
          renderNotes();
          renderPhotos();
          alert('Import zakończony.');
        } catch (e) {
          alert('Nieprawidłowy plik JSON.');
        }
      };
      reader.readAsText(file);
    }
    function clearAll() {
      if (!confirm('Wyczyścić wszystkie dane (notatki, zdjęcia, ustawienia)?')) return;
      state.notes = [];
      state.photos = [];
      state.ui = { theme: 'dark', fontSize: 16, galleryView: 'grid' };
      storage.set('pinezka.notes', state.notes);
      storage.set('pinezka.photos', state.photos);
      storage.set('pinezka.ui', state.ui);
      initUI();
      renderNotes();
      renderPhotos();
    }

    // ====== Pomodoro ======
    let pomodoroTimer = null;
    function startPomodoro() {
      if (state.pomodoro.running) return;
      state.pomodoro.running = true;
      state.pomodoro.start = Date.now();
      $('#pomodoroLabel').textContent = 'W toku: 25 min';
      tickPomodoro();
      pomodoroTimer = setInterval(tickPomodoro, 500);
    }
    function stopPomodoro() {
      state.pomodoro.running = false;
      clearInterval(pomodoroTimer);
      updatePomodoro(0);
      $('#pomodoroLabel').textContent = 'Gotowy.';
    }
    function tickPomodoro() {
      const elapsed = Date.now() - state.pomodoro.start;
      const ratio = Math.min(elapsed / state.pomodoro.duration, 1);
      updatePomodoro(ratio);
      if (ratio >= 1) {
        stopPomodoro();
        alert('Sesja Pomodoro zakończona! Czas na przerwę.');
      } else {
        const remainingMs = state.pomodoro.duration - elapsed;
        const mm = Math.floor(remainingMs / 60000);
        const ss = Math.floor((remainingMs % 60000) / 1000);
        $('#pomodoroLabel').textContent = `Pozostało: ${String(mm).padStart(2,'0')}:${String(ss).padStart(2,'0')}`;
      }
    }
    function updatePomodoro(ratio) {
      $('#pomodoroMeter').style.width = (ratio * 100).toFixed(1) + '%';
    }

    // ====== Motyw/widok/rozmiar ======
    function setTheme(theme) {
      state.ui.theme = theme;
      document.body.dataset.theme = theme;
      $('#themeSelect').value = theme;
      storage.set('pinezka.ui', state.ui);
    }
    function setFontSize(px) {
      state.ui.fontSize = px;
      document.documentElement.style.setProperty('--font-size', px + 'px');
      $('#fontSelect').value = String(px);
      storage.set('pinezka.ui', state.ui);
    }
    function setGalleryView(view) {
      state.ui.galleryView = view;
      renderPhotos();
      storage.set('pinezka.ui', state.ui);
    }

    // ====== Eventy ======
    function bindEvents() {
      // Notatki
      $('#addNoteBtn').onclick = addNote;
      $('#updateNoteBtn').onclick = updateNote;
      $('#resetFormBtn').onclick = resetNoteForm;
      $('#searchNotes').oninput = renderNotes;

      // Szablony
      $$('.tool [data-template]').forEach(btn => {
        btn.onclick = () => {
          const t = btn.dataset.template;
          $('#noteTitle').value = t;
          $('#noteBody').value = `• Punkt 1\n• Punkt 2\n• Punkt 3`;
          $('#noteTags').value = 'szablon';
        };
      });

      // Zdjęcia
      $('#photoInput').onchange = (e) => addPhotos(e.target.files);
      const dz = $('#dropzone');
      ['dragenter','dragover'].forEach(ev => dz.addEventListener(ev, (e) => { e.preventDefault(); dz.classList.add('drag'); }));
      ['dragleave','drop'].forEach(ev => dz.addEventListener(ev, (e) => { e.preventDefault(); dz.classList.remove('drag'); }));
      dz.addEventListener('drop', (e) => {
        if (e.dataTransfer && e.dataTransfer.files) addPhotos(e.dataTransfer.files);
      });
      $('#deleteAllPhotos').onclick = () => {
        if (!confirm('Usunąć wszystkie zdjęcia?')) return;
        state.photos = [];
        renderPhotos();
      };

      // Przybornik
      $('#exportBtn').onclick = exportData;
      $('#importInput').onchange = (e) => e.target.files[0] && importData(e.target.files[0]);
      $('#clearAllBtn').onclick = clearAll;

      $$('.tool [data-theme]').forEach(btn => btn.onclick = () => setTheme(btn.dataset.theme));
      $('#themeSelect').onchange = (e) => setTheme(e.target.value);
      $('#fontSelect').onchange = (e) => setFontSize(parseInt(e.target.value, 10));
      $('#viewGrid').onclick = () => setGalleryView('grid');
      $('#viewList').onclick = () => setGalleryView('list');

      // Pomodoro
      $('#pomodoroStart').onclick = startPomodoro;
      $('#pomodoroStop').onclick = stopPomodoro;

      // Klawisze skrótów
      document.addEventListener('keydown', (e) => {
        if (e.ctrlKey && e.key.toLowerCase() === 's') { e.preventDefault(); addNote(); }
        if (e.ctrlKey && e.key.toLowerCase() === 'f') { e.preventDefault(); $('#searchNotes').focus(); }
      });
    }

    // ====== Pomocnicze ======
    function formatDate(ts) {
      const d = new Date(ts);
      const dd = String(d.getDate()).padStart(2,'0');
      const mm = String(d.getMonth()+1).padStart(2,'0');
      const yyyy = d.getFullYear();
      const hh = String(d.getHours()).padStart(2,'0');
      const min = String(d.getMinutes()).padStart(2,'0');
      return `${dd}.${mm}.${yyyy} ${hh}:${min}`;
    }
    function escapeHTML(str) {
      return String(str).replace(/[&<>"']/g, s => ({
        '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'
      }[s]));
    }
    function cryptoRandomId() {
      const arr = new Uint8Array(12);
      crypto.getRandomValues(arr);
      return Array.from(arr).map(b => b.toString(16).padStart(2,'0')).join('');
    }

    // ====== Start ======
    initUI();
    bindEvents();
    renderNotes();
    renderPhotos();
  </script>
</body>
</html>
