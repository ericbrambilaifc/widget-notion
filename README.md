<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Distribuição de Matérias - PRF/PF</title>
  <style>
    :root {
      --bg-color: #ffffff;
      --text-color: #37352f;
      --btn-bg: #ebeced;
      --btn-hover: #dfe1e3;
      --muted-color: #787774;
      --border-color: #e9e9e7;
      --menu-bg: #ffffff;
      --menu-shadow: 0 4px 12px rgba(15, 15, 15, 0.1), 0 0 0 1px rgba(15, 15, 15, 0.05);
      --danger-color: #eb5757;
      --danger-bg: rgba(235, 87, 87, 0.1);
    }

    @media (prefers-color-scheme: dark) {
      :root {
        --bg-color: #191919;
        --text-color: #ffffffcf;
        --btn-bg: #2f2f2f;
        --btn-hover: #3f3f3f;
        --muted-color: #9b9a97;
        --border-color: #2f2f2f;
        --menu-bg: #252525;
        --menu-shadow: 0 4px 12px rgba(0, 0, 0, 0.4), 0 0 0 1px rgba(255, 255, 255, 0.08);
        --danger-color: #ff6b6b;
        --danger-bg: rgba(255, 107, 107, 0.12);
      }
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
      background-color: var(--bg-color);
      color: var(--text-color);
      padding: 12px 16px;
      user-select: none;
    }

    .container {
      width: 100%;
      max-width: 340px;
    }

    .list {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 14px;
      padding: 4px 6px;
      margin: 0 -6px;
      border-radius: 5px;
      transition: background-color 0.1s ease;
    }

    .row:hover {
      background-color: rgba(128, 128, 128, 0.06);
    }

    .subject-info {
      display: flex;
      align-items: center;
      gap: 6px;
      flex: 1;
      min-width: 0;
      padding-right: 8px;
    }

    .subject-name {
      font-weight: 400;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .count-badge {
      font-size: 12px;
      color: var(--muted-color);
      flex-shrink: 0;
    }

    .controls {
      display: flex;
      align-items: center;
      gap: 6px;
      flex-shrink: 0;
    }

    button {
      background-color: var(--btn-bg);
      color: var(--text-color);
      border: none;
      border-radius: 5px;
      padding: 4px 9px;
      font-size: 13px;
      font-weight: 500;
      cursor: pointer;
      transition: background-color 0.15s ease;
    }

    button:hover {
      background-color: var(--btn-hover);
    }

    button:active {
      transform: scale(0.96);
    }

    .percentage {
      min-width: 44px;
      text-align: right;
      font-variant-numeric: tabular-nums;
      margin-left: 6px;
    }

    /* Área de adicionar matéria */
    .add-section {
      margin-top: 8px;
    }

    .add-trigger {
      background: transparent;
      color: var(--muted-color);
      padding: 5px 8px;
      font-size: 13px;
      display: inline-flex;
      align-items: center;
      gap: 4px;
    }

    .add-trigger:hover {
      background-color: var(--btn-bg);
      color: var(--text-color);
    }

    .add-form {
      display: none;
      align-items: center;
      gap: 6px;
      margin-top: 4px;
      width: 100%;
    }

    .add-form.active {
      display: flex;
    }

    .add-input {
      flex: 1;
      min-width: 0;
      background: var(--bg-color);
      color: var(--text-color);
      border: 1px solid var(--border-color);
      border-radius: 5px;
      padding: 5px 8px;
      font-size: 13px;
      font-family: inherit;
      outline: none;
    }

    .add-input:focus {
      border-color: var(--muted-color);
    }

    .add-submit {
      flex-shrink: 0;
      padding: 5px 10px;
    }

    .footer {
      margin-top: 12px;
      padding-top: 8px;
      border-top: 1px solid var(--border-color);
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 12px;
      color: var(--muted-color);
    }

    .reset-btn {
      background: none;
      padding: 3px 7px;
      font-size: 11px;
      color: var(--muted-color);
    }

    .reset-btn:hover {
      background-color: var(--btn-bg);
      color: var(--text-color);
    }

    /* Menu Dropdown (Botão Direito) */
    .context-menu {
      position: fixed;
      display: none;
      background-color: var(--menu-bg);
      box-shadow: var(--menu-shadow);
      border-radius: 6px;
      padding: 4px;
      z-index: 1000;
      min-width: 145px;
    }

    .context-menu.visible {
      display: block;
    }

    .context-item {
      display: flex;
      align-items: center;
      gap: 8px;
      width: 100%;
      background: none;
      border: none;
      padding: 6px 10px;
      font-size: 13px;
      text-align: left;
      border-radius: 4px;
      color: var(--danger-color);
      cursor: pointer;
    }

    .context-item:hover {
      background-color: var(--danger-bg);
    }
  </style>
</head>
<body>

  <div class="container">
    <div class="list" id="subject-list"></div>

    <div class="add-section">
      <button type="button" class="add-trigger" id="show-add-btn">+ Nova matéria</button>
      <div class="add-form" id="add-form">
        <input type="text" class="add-input" id="new-subject-input" placeholder="Nome da matéria...">
        <button type="button" class="add-submit" id="confirm-add-btn">Adicionar</button>
      </div>
    </div>

    <div class="footer">
      <span id="total-display">Total: 0 blocos</span>
      <button type="button" class="reset-btn" id="reset-btn">Resetar (0%)</button>
    </div>
  </div>

  <!-- Dropdown de contexto -->
  <div class="context-menu" id="context-menu">
    <button type="button" class="context-item" id="delete-item-btn">
      <span>🗑</span> Excluir matéria
    </button>
  </div>

  <script>
    const defaultSubjects = [
      { id: 'port', name: 'Português', count: 4 },
      { id: 'transito', name: 'Leg. Trânsito', count: 4 },
      { id: 'penal', name: 'Direito Penal', count: 3 },
      { id: 'rodizio', name: 'Rodízio', count: 3 },
      { id: 'rlm', name: 'RLM', count: 2 },
      { id: 'adm', name: 'Direito Adm.', count: 1 },
      { id: 'const', name: 'Direito Const.', count: 1 },
      { id: 'info', name: 'Informática', count: 1 }
    ];

    const STORAGE_KEY = 'notion_prf_widget_v3';

    function loadData() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (raw) return JSON.parse(raw);
      } catch (e) {
        // Ignora bloqueio de storage em iframes
      }
      return JSON.parse(JSON.stringify(defaultSubjects));
    }

    let subjects = loadData();
    let selectedIndex = null;

    function save() {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(subjects));
      } catch (e) {
        // Mantém funcionando em memória se o navegador bloquear cookies/storage de terceiros
      }
    }

    function updateCount(index, delta) {
      if (subjects[index] && subjects[index].count + delta >= 0) {
        subjects[index].count += delta;
        save();
        render();
      }
    }

    function resetToZero() {
      subjects = subjects.map(sub => ({ ...sub, count: 0 }));
      save();
      render();
    }

    function openAddForm() {
      const btn = document.getElementById('show-add-btn');
      const form = document.getElementById('add-form');
      const input = document.getElementById('new-subject-input');

      btn.style.display = 'none';
      form.classList.add('active');
      input.value = '';
      setTimeout(() => input.focus(), 10);
    }

    function closeAddForm() {
      const btn = document.getElementById('show-add-btn');
      const form = document.getElementById('add-form');

      form.classList.remove('active');
      btn.style.display = 'inline-flex';
    }

    function addSubject() {
      const input = document.getElementById('new-subject-input');
      const name = input.value.trim();
      if (name) {
        subjects.push({
          id: 'sub_' + Date.now(),
          name: name,
          count: 0
        });
        save();
        render();
      }
      closeAddForm();
    }

    function openContextMenu(event, index) {
      event.preventDefault();
      selectedIndex = index;

      const menu = document.getElementById('context-menu');
      menu.classList.add('visible');

      const menuWidth = 150;
      const menuHeight = 40;
      const x = Math.min(event.clientX, window.innerWidth - menuWidth - 8);
      const y = Math.min(event.clientY, window.innerHeight - menuHeight - 8);

      menu.style.left = `${Math.max(4, x)}px`;
      menu.style.top = `${Math.max(4, y)}px`;
    }

    function closeContextMenu() {
      document.getElementById('context-menu').classList.remove('visible');
      selectedIndex = null;
    }

    function deleteSelectedSubject() {
      if (selectedIndex !== null) {
        subjects.splice(selectedIndex, 1);
        save();
        render();
      }
      closeContextMenu();
    }

    // Eventos vinculados via addEventListener (mais seguro em embeds)
    document.getElementById('show-add-btn').addEventListener('click', openAddForm);
    document.getElementById('confirm-add-btn').addEventListener('click', addSubject);
    document.getElementById('reset-btn').addEventListener('click', resetToZero);
    document.getElementById('delete-item-btn').addEventListener('click', deleteSelectedSubject);

    document.getElementById('new-subject-input').addEventListener('keydown', (event) => {
      if (event.key === 'Enter') {
        event.preventDefault();
        addSubject();
      } else if (event.key === 'Escape') {
        closeAddForm();
      }
    });

    document.addEventListener('click', (e) => {
      if (!e.target.closest('#context-menu')) {
        closeContextMenu();
      }
    });

    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') {
        closeContextMenu();
      }
    });

    function render() {
      const listEl = document.getElementById('subject-list');
      const totalEl = document.getElementById('total-display');
      
      const totalBlocks = subjects.reduce((sum, item) => sum + item.count, 0);
      totalEl.textContent = `Total: ${totalBlocks} ${totalBlocks === 1 ? 'bloco' : 'blocos'}`;

      listEl.innerHTML = '';

      subjects.forEach((sub, index) => {
        const pct = totalBlocks > 0 ? Math.round((sub.count / totalBlocks) * 100) : 0;

        const row = document.createElement('div');
        row.className = 'row';
        row.addEventListener('contextmenu', (e) => openContextMenu(e, index));

        row.innerHTML = `
          <div class="subject-info" title="${sub.name}">
            <span class="subject-name">${sub.name}</span>
            <span class="count-badge">(${sub.count})</span>
          </div>
          <div class="controls">
            <button type="button" class="btn-plus">+1</button>
            <button type="button" class="btn-minus">-1</button>
            <span class="percentage">${pct}%</span>
          </div>
        `;

        row.querySelector('.btn-plus').addEventListener('click', () => updateCount(index, 1));
        row.querySelector('.btn-minus').addEventListener('click', () => updateCount(index, -1));

        listEl.appendChild(row);
      });
    }

    render();
  </script>
</body>
</html>
