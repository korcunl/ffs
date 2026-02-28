<!doctype html>
<html lang="tr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>FFS Portal (Internal)</title>
  <style>
    :root{
      --bg:#0b0f17;
      --panel:#0f1624;
      --panel2:#0c1320;
      --text:#e8eefc;
      --muted:#a7b3d1;
      --line:rgba(255,255,255,.10);
      --accent:#5aa9ff;
      --danger:#ff6b6b;
      --ok:#3ddc97;
      --shadow: 0 10px 30px rgba(0,0,0,.35);
      --radius: 16px;
      --radius2: 12px;
      --mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
      --sans: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, "Noto Sans", "Helvetica Neue", "Apple Color Emoji", "Segoe UI Emoji";
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: var(--sans);
      background: radial-gradient(1200px 600px at 10% 10%, rgba(90,169,255,.12), transparent 60%),
                  radial-gradient(900px 500px at 90% 20%, rgba(61,220,151,.08), transparent 55%),
                  var(--bg);
      color:var(--text);
    }
    a{color:inherit; text-decoration:none}

    /* Layout */
    .app{min-height:100%; display:grid; grid-template-columns: 280px 1fr;}

    /* Sidebar */
    .sidebar{
      border-right: 1px solid var(--line);
      background: linear-gradient(180deg, rgba(255,255,255,.03), transparent 30%), var(--panel2);
      padding: 18px;
      position: sticky;
      top:0;
      height:100vh;
    }
    .brand{
      display:flex;
      align-items:center;
      gap:10px;
      padding: 10px 10px 14px;
      border-bottom: 1px solid var(--line);
      margin-bottom: 14px;
    }
    .logo{
      width: 36px;
      height: 36px;
      border-radius: 10px;
      background: linear-gradient(135deg, rgba(90,169,255,.9), rgba(61,220,151,.85));
      box-shadow: var(--shadow);
    }
    .brand h1{font-size:14px; margin:0; letter-spacing:.2px;}
    .brand .sub{font-size:12px; color:var(--muted)}

    .navTitle{font-size:12px; color:var(--muted); margin: 16px 10px 8px;}
    .nav{display:flex; flex-direction:column; gap:6px;}
    .nav button{
      all:unset;
      cursor:pointer;
      display:flex;
      justify-content:space-between;
      align-items:center;
      gap:10px;
      padding: 10px 12px;
      border-radius: 12px;
      border:1px solid transparent;
      background: rgba(255,255,255,.02);
    }
    .nav button:hover{border-color: var(--line); background: rgba(255,255,255,.03)}
    .nav button.active{border-color: rgba(90,169,255,.45); background: rgba(90,169,255,.10)}
    .pill{font-family: var(--mono); font-size:11px; color:var(--muted)}

    .sidebarFooter{
      position:absolute;
      left:18px;
      right:18px;
      bottom:18px;
      padding-top: 14px;
      border-top: 1px solid var(--line);
      color: var(--muted);
      font-size: 12px;
      display:flex;
      justify-content:space-between;
      gap:10px;
    }

    /* Main */
    .main{padding: 18px 22px 28px;}

    /* Topbar */
    .topbar{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:14px;
      padding: 14px 16px;
      border: 1px solid var(--line);
      border-radius: var(--radius);
      background: rgba(255,255,255,.02);
      box-shadow: var(--shadow);
    }
    .topbar .left{display:flex; flex-direction:column; gap:2px;}
    .topbar .title{font-size:16px; font-weight:650; letter-spacing:.2px;}
    .topbar .hint{font-size:12px; color:var(--muted)}
    .topbar .right{display:flex; align-items:center; gap:10px;}

    .chip{
      display:inline-flex;
      align-items:center;
      gap:8px;
      padding: 8px 10px;
      border: 1px solid var(--line);
      border-radius: 999px;
      background: rgba(255,255,255,.02);
      font-size:12px;
      color: var(--muted);
    }
    .chip .dot{width:8px; height:8px; border-radius:999px; background: var(--ok)}

    .btn{
      cursor:pointer;
      border: 1px solid var(--line);
      background: rgba(255,255,255,.02);
      color: var(--text);
      padding: 10px 12px;
      border-radius: 12px;
      font-weight: 600;
      letter-spacing: .2px;
    }
    .btn:hover{border-color: rgba(255,255,255,.2)}
    .btn.primary{border-color: rgba(90,169,255,.55); background: rgba(90,169,255,.12)}
    .btn.danger{border-color: rgba(255,107,107,.55); background: rgba(255,107,107,.10)}

    /* Cards */
    .grid{display:grid; grid-template-columns: 1.2fr .8fr; gap:14px; margin-top:14px;}
    @media (max-width: 980px){
      .app{grid-template-columns: 1fr}
      .sidebar{position:relative; height:auto}
      .sidebarFooter{position:relative; left:0; right:0; bottom:0; margin-top:12px}
      .grid{grid-template-columns: 1fr}
    }

    .card{
      border: 1px solid var(--line);
      background: rgba(255,255,255,.02);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 14px;
    }
    .card h2{margin:0 0 8px; font-size:14px; letter-spacing:.2px;}
    .card p{margin: 6px 0; color: var(--muted); font-size: 13px; line-height: 1.45;}

    /* Form */
    .formRow{display:grid; grid-template-columns: 1fr 1fr; gap:10px; margin-top:10px;}
    @media (max-width: 760px){.formRow{grid-template-columns:1fr}}

    label{display:block; font-size:12px; color:var(--muted); margin: 10px 0 6px;}
    input, select, textarea{
      width:100%;
      padding: 10px 12px;
      border-radius: 12px;
      border: 1px solid var(--line);
      background: rgba(10,16,28,.6);
      color: var(--text);
      outline:none;
    }
    input:focus, select:focus, textarea:focus{border-color: rgba(90,169,255,.55); box-shadow: 0 0 0 4px rgba(90,169,255,.12)}
    .help{font-size:12px; color:var(--muted); margin-top:8px;}

    /* Result */
    .resultBox{
      margin-top: 12px;
      border-radius: 14px;
      border: 1px dashed rgba(255,255,255,.18);
      padding: 12px;
      background: rgba(255,255,255,.015);
      font-family: var(--mono);
      font-size: 12px;
      white-space: pre-wrap;
    }

    /* Login overlay */
    .overlay{
      position:fixed;
      inset:0;
      background: rgba(6,10,16,.72);
      backdrop-filter: blur(8px);
      display:flex;
      align-items:center;
      justify-content:center;
      padding: 18px;
      z-index: 999;
    }
    .login{
      width: 100%;
      max-width: 420px;
      border: 1px solid var(--line);
      background: rgba(255,255,255,.03);
      border-radius: 20px;
      box-shadow: var(--shadow);
      padding: 16px;
    }
    .login h2{margin: 6px 0 2px; font-size: 16px}
    .login .sub{color: var(--muted); font-size: 12px; margin: 0 0 10px;}
    .login .row{display:grid; gap:10px; margin-top:10px;}
    .login .actions{display:flex; justify-content:space-between; align-items:center; gap:10px; margin-top:12px;}
    .login .warn{font-size:12px; color: rgba(255,107,107,.9); min-height: 16px;}

    /* Small table */
    table{width:100%; border-collapse:collapse; margin-top:8px; overflow:hidden; border-radius: 12px;}
    th, td{padding: 10px; border-bottom: 1px solid var(--line); font-size: 12px;}
    th{color: var(--muted); font-weight:600; text-align:left; background: rgba(255,255,255,.02)}
    tr:last-child td{border-bottom:none}
  </style>
</head>

<body>
  <!-- LOGIN (Client-side only demo) -->
  <div class="overlay" id="loginOverlay" aria-hidden="false">
    <div class="login" role="dialog" aria-modal="true" aria-label="Login">
      <div style="display:flex; gap:10px; align-items:center;">
        <div class="logo" aria-hidden="true"></div>
        <div>
          <h2>FFS Portal</h2>
          <div class="sub">Internal use • Minimal demo template</div>
        </div>
      </div>

      <div class="row">
        <div>
          <label for="u">Kullanıcı Adı</label>
          <input id="u" autocomplete="username" placeholder="korcan" />
        </div>
        <div>
          <label for="p">Şifre</label>
          <input id="p" type="password" autocomplete="current-password" placeholder="••••••••" />
        </div>
      </div>

      <div class="actions">
        <button class="btn primary" id="loginBtn">Giriş</button>
        <div style="flex:1"></div>
        <div class="warn" id="loginWarn"></div>
      </div>

      <p class="help">
        Not: Bu sayfa yalnızca arayüz şablonudur. Gerçek güvenli giriş için sunucu tarafı doğrulama gerekir.
      </p>
    </div>
  </div>

  <div class="app" id="app" style="filter: blur(0px);">
    <!-- SIDEBAR NAV -->
    <aside class="sidebar">
      <div class="brand">
        <div class="logo" aria-hidden="true"></div>
        <div>
          <h1>FFS Portal</h1>
          <div class="sub">In-service • Fitness-for-Service</div>
        </div>
      </div>

      <div class="navTitle">Modüller</div>
      <nav class="nav" id="nav">
        <button data-route="home" class="active">
          <span>Anasayfa</span>
          <span class="pill">/home</span>
        </button>
        <button data-route="api579">
          <span>API 579 / ASME FFS-1</span>
          <span class="pill">/api579</span>
        </button>
        <button data-route="b318g">
          <span>ASME B31G</span>
          <span class="pill">/b31g</span>
        </button>
        <button data-route="pcc2">
          <span>ASME PCC-2 (Kompozit)</span>
          <span class="pill">/pcc2</span>
        </button>
        <button data-route="settings">
          <span>Ayarlar</span>
          <span class="pill">/settings</span>
        </button>
      </nav>

      <div class="sidebarFooter">
        <span>v0.1</span>
        <span style="font-family:var(--mono)">local-demo</span>
      </div>
    </aside>

    <!-- MAIN CONTENT -->
    <main class="main">
      <div class="topbar">
        <div class="left">
          <div class="title" id="pageTitle">Anasayfa</div>
          <div class="hint" id="pageHint">Modül seçerek ilgili hesap ekranına geçin.</div>
        </div>
        <div class="right">
          <span class="chip"><span class="dot"></span><span id="userChip">Signed in</span></span>
          <button class="btn" id="logoutBtn" title="Log out">Çıkış</button>
        </div>
      </div>

      <div id="view">
        <!-- Views injected here -->
      </div>
    </main>
  </div>

  <template id="tpl-home">
    <div class="grid">
      <section class="card">
        <h2>Hızlı Başlangıç</h2>
        <p>Bu şablon; minimalist UI, navbar/side-nav, route mantığı ve örnek bir hesap formu içerir.</p>
        <p>Hedef: API 579 (Part 2/5/9), ASME B31G ve PCC-2 kompozit sarım hesap ekranlarını tek çatı altında toplamak.</p>

        <h2 style="margin-top:14px">Yapılacaklar (Öneri)</h2>
        <table>
          <thead><tr><th>Modül</th><th>Kapsam</th><th>Durum</th></tr></thead>
          <tbody>
            <tr><td>API 579</td><td>Part 2, 5, 9 ekranları + input doğrulama</td><td>Placeholder</td></tr>
            <tr><td>B31G</td><td>Corrosion assessment (basit/modified)</td><td>Placeholder</td></tr>
            <tr><td>PCC-2</td><td>Composite repair sizing + limit kontrolleri</td><td>Placeholder</td></tr>
          </tbody>
        </table>
      </section>

      <aside class="card">
        <h2>Notlar</h2>
        <p><b>Güvenlik:</b> Bu demo, tarayıcı içinde (client-side) “göstermelik” login yapar. Gerçekte kullanıcı adı/şifre doğrulamasını sunucu tarafında yapmalısın.</p>
        <p><b>Mimari öneri:</b> UI (bu sayfa) + API (Node/Express, FastAPI vb.) + hesap motoru (Python/JS) şeklinde ayır.</p>
        <p><b>Veri:</b> Her hesap için bir JSON schema tanımlayıp input validation’ı standardize et.</p>
      </aside>
    </div>
  </template>

  <template id="tpl-api579">
    <div class="grid">
      <section class="card">
        <h2>API 579 / ASME FFS-1</h2>
        <p>Örnek: Part seçimi + placeholder hesap ekranı. Buradaki hesaplar DEMO amaçlıdır.</p>

        <div class="formRow">
          <div>
            <label for="partSel">Part</label>
            <select id="partSel">
              <option value="p2">Part 2 — General Metal Loss</option>
              <option value="p5">Part 5 — Local Metal Loss</option>
              <option value="p9">Part 9 — Crack-Like Flaws</option>
            </select>
          </div>
          <div>
            <label for="units">Units</label>
            <select id="units">
              <option value="si">SI (mm, MPa)</option>
              <option value="us">US (in, psi)</option>
            </select>
          </div>
        </div>

        <label for="designP">Design Pressure</label>
        <input id="designP" type="number" step="any" placeholder="Örn: 7.5" />

        <div class="formRow">
          <div>
            <label for="od">OD</label>
            <input id="od" type="number" step="any" placeholder="Örn: 1422.4" />
          </div>
          <div>
            <label for="t">t (Nominal)</label>
            <input id="t" type="number" step="any" placeholder="Örn: 19.45" />
          </div>
        </div>

        <div style="display:flex; gap:10px; margin-top:12px; flex-wrap:wrap;">
          <button class="btn primary" id="runApi579">Çalıştır (Demo)</button>
          <button class="btn" id="resetApi579">Sıfırla</button>
        </div>

        <div class="resultBox" id="api579Out">Çıktı burada görünecek…</div>

        <p class="help">Not: Bu ekranda şimdilik yalnızca örnek bir “nominal hoop stress” hesaplanır. Gerçek API 579 implementasyonu için ilgili Part algoritmalarını ekle.</p>
      </section>

      <aside class="card">
        <h2>Geliştirme Notu</h2>
        <p><b>Part bazlı ekranlar:</b> Part 2/5/9 için ayrı alt-view komponenti yapıp, input setini Part’a göre değiştir.</p>
        <p><b>Doğrulama:</b> Limit kontrolleri ve domain checks (negatif kalınlık, OD&lt;=0 vb.) zorunlu.</p>
        <p><b>Rapor:</b> PDF export için server-side render veya client-side (pdf-lib) kullanılabilir.</p>
      </aside>
    </div>
  </template>

  <template id="tpl-b31g">
    <div class="grid">
      <section class="card">
        <h2>ASME B31G</h2>
        <p>Placeholder ekran. Buraya corrosion-length/depth girdileri ve B31G/Modified B31G hesaplarını ekleyebilirsin.</p>

        <label>Corrosion Length (L)</label>
        <input type="number" step="any" placeholder="[value]" />

        <div class="formRow">
          <div>
            <label>Max Depth (d)</label>
            <input type="number" step="any" placeholder="[value]" />
          </div>
          <div>
            <label>t (Nominal)</label>
            <input type="number" step="any" placeholder="[value]" />
          </div>
        </div>

        <div style="display:flex; gap:10px; margin-top:12px; flex-wrap:wrap;">
          <button class="btn primary">Çalıştır (yakında)</button>
          <button class="btn">Sıfırla</button>
        </div>

        <div class="resultBox">Henüz implement edilmedi.</div>
      </section>

      <aside class="card">
        <h2>Not</h2>
        <p>B31G modülünde; material grade, SMYS, design factor, location class ve MAOP ilişkisini ayrı bir “input set” olarak tasarlamak iyi olur.</p>
      </aside>
    </div>
  </template>

  <template id="tpl-pcc2">
    <div class="grid">
      <section class="card">
        <h2>ASME PCC-2 (Kompozit Sarım)</h2>
        <p>Placeholder ekran. Buraya wrap thickness/length, laminate properties, allowable strain/stress ve safety checks eklenebilir.</p>

        <div class="formRow">
          <div>
            <label>Repair Type</label>
            <select>
              <option>Type A — Reinforcement</option>
              <option>Type B — Pressure Containment</option>
            </select>
          </div>
          <div>
            <label>Fiber System</label>
            <select>
              <option>GFRP</option>
              <option>CFRP</option>
              <option>Other</option>
            </select>
          </div>
        </div>

        <label>Design Pressure</label>
        <input type="number" step="any" placeholder="[value]" />

        <div style="display:flex; gap:10px; margin-top:12px; flex-wrap:wrap;">
          <button class="btn primary">Çalıştır (yakında)</button>
          <button class="btn">Sıfırla</button>
        </div>

        <div class="resultBox">Henüz implement edilmedi.</div>
      </section>

      <aside class="card">
        <h2>Not</h2>
        <p>Kompozit sarım için üretici datasheet değerlerini (E, allowable strain, curing) modül seviyesinde zorunlu input yapmanı öneririm.</p>
      </aside>
    </div>
  </template>

  <template id="tpl-settings">
    <div class="grid">
      <section class="card">
        <h2>Ayarlar</h2>
        <p>Basit profil ve uygulama ayarları için placeholder.</p>

        <div class="formRow">
          <div>
            <label>Display Name</label>
            <input id="dispName" placeholder="Korcan" />
          </div>
          <div>
            <label>Default Units</label>
            <select id="defUnits">
              <option value="si">SI</option>
              <option value="us">US</option>
            </select>
          </div>
        </div>

        <div style="display:flex; gap:10px; margin-top:12px; flex-wrap:wrap;">
          <button class="btn primary" id="saveSettings">Kaydet</button>
        </div>

        <div class="resultBox" id="settingsOut">—</div>
      </section>

      <aside class="card">
        <h2>Uygulama Notu</h2>
        <p>Gerçek uygulamada bu ayarlar sunucuda kullanıcı profiline yazılmalı (DB + auth).</p>
      </aside>
    </div>
  </template>

  <script>
    // ----------------------------
    // Minimal client-side router
    // ----------------------------
    const state = {
      authed: false,
      user: null,
      route: 'home'
    };

    const routes = {
      home: { title: 'Anasayfa', hint: 'Modül seçerek ilgili hesap ekranına geçin.', tpl: 'tpl-home' },
      api579: { title: 'API 579 / ASME FFS-1', hint: 'Part 2 / Part 5 / Part 9 seçimli hesap ekranı.', tpl: 'tpl-api579' },
      b318g: { title: 'ASME B31G', hint: 'Corrosion assessment modülü (placeholder).', tpl: 'tpl-b31g' },
      pcc2: { title: 'ASME PCC-2 (Kompozit)', hint: 'Composite repair hesap ekranı (placeholder).', tpl: 'tpl-pcc2' },
      settings: { title: 'Ayarlar', hint: 'Kullanıcı profili ve varsayılanlar.', tpl: 'tpl-settings' }
    };

    const $ = (sel, root=document) => root.querySelector(sel);
    const $$ = (sel, root=document) => Array.from(root.querySelectorAll(sel));

    function setActiveNav(route){
      $$('#nav button').forEach(b => b.classList.toggle('active', b.dataset.route === route));
    }

    function mount(route){
      state.route = route;
      const r = routes[route] || routes.home;
      $('#pageTitle').textContent = r.title;
      $('#pageHint').textContent = r.hint;
      setActiveNav(route);

      const tpl = document.getElementById(r.tpl);
      const node = tpl.content.cloneNode(true);
      const view = $('#view');
      view.innerHTML = '';
      view.appendChild(node);

      // attach per-view handlers
      if(route === 'api579') attachApi579Handlers();
      if(route === 'settings') attachSettingsHandlers();
    }

    // ----------------------------
    // Demo login (NOT secure)
    // ----------------------------
    function fakeLogin(u, p){
      // IMPORTANT: This is only for UI demo.
      // Replace with server-side auth.
      return (u && p && p.length >= 4);
    }

    function showOverlay(show){
      const el = $('#loginOverlay');
      el.style.display = show ? 'flex' : 'none';
      el.setAttribute('aria-hidden', show ? 'false' : 'true');
    }

    function onLogin(){
      const u = $('#u').value.trim();
      const p = $('#p').value;
      const ok = fakeLogin(u,p);
      if(!ok){
        $('#loginWarn').textContent = 'Hatalı giriş (demo). Şifre en az 4 karakter.';
        return;
      }
      state.authed = true;
      state.user = u;
      $('#userChip').textContent = u;
      $('#loginWarn').textContent = '';
      showOverlay(false);
      mount('home');
    }

    function onLogout(){
      state.authed = false;
      state.user = null;
      $('#userChip').textContent = 'Signed in';
      $('#u').value = '';
      $('#p').value = '';
      showOverlay(true);
    }

    // ----------------------------
    // API 579 demo calc
    // ----------------------------
    function attachApi579Handlers(){
      const runBtn = $('#runApi579');
      const resetBtn = $('#resetApi579');
      const out = $('#api579Out');

      runBtn?.addEventListener('click', () => {
        const units = $('#units').value;
        const part = $('#partSel').value;
        const P = parseFloat($('#designP').value);
        const OD = parseFloat($('#od').value);
        const t = parseFloat($('#t').value);

        if(!isFinite(P) || !isFinite(OD) || !isFinite(t) || P<=0 || OD<=0 || t<=0){
          out.textContent = 'Input error: P, OD, t > 0 olmalı.';
          return;
        }

        // Demo nominal hoop stress (thin-wall approx): sigma_h = P*D/(2t)
        // Units: if SI => MPa if P in MPa, D & t in mm
        //       if US => psi if P in psi, D & t in in
        const sigma = (P * OD) / (2 * t);

        out.textContent =
`Selected: ${part.toUpperCase()}\n`+
`Units: ${units.toUpperCase()}\n`+
`P = ${P}\nOD = ${OD}\nt = ${t}\n\n`+
`Nominal Hoop Stress (demo) = P*OD/(2t) = ${sigma.toFixed(3)} ${units==='si' ? 'MPa' : 'psi'}\n\n`+
`TODO: Replace with full API 579 algorithms for the selected Part.`;
      });

      resetBtn?.addEventListener('click', () => {
        $('#designP').value = '';
        $('#od').value = '';
        $('#t').value = '';
        out.textContent = 'Çıktı burada görünecek…';
      });
    }

    // ----------------------------
    // Settings demo
    // ----------------------------
    function attachSettingsHandlers(){
      const disp = $('#dispName');
      const units = $('#defUnits');
      const out = $('#settingsOut');
      const btn = $('#saveSettings');

      // preload
      disp.value = state.user || '';

      btn?.addEventListener('click', () => {
        const name = disp.value.trim() || state.user || 'user';
        out.textContent = `Saved (demo)\nDisplay Name: ${name}\nDefault Units: ${units.value.toUpperCase()}\n\nNot: Bu değerler şu an sadece tarayıcı hafızasında.`;
      });
    }

    // ----------------------------
    // Boot
    // ----------------------------
    $$('#nav button').forEach(btn => {
      btn.addEventListener('click', () => mount(btn.dataset.route));
    });

    $('#loginBtn').addEventListener('click', onLogin);
    $('#logoutBtn').addEventListener('click', onLogout);

    // Allow Enter to login
    ['u','p'].forEach(id => {
      $('#'+id).addEventListener('keydown', (e) => {
        if(e.key === 'Enter') onLogin();
      });
    });

    // initial
    showOverlay(true);
    mount('home');
  </script>
</body>
</html>

