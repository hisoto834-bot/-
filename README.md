# -
ЭкоФото: сфотографируйте мусор, умирающие деревья или сжигание отходов заводом. Приложение запишет дату, время и GPS-место для жалобы в экологическую службу.
Ниже полный код одним файлом, это версия `ecofoto-seo.html`. Скопируйте всё, вставьте в текстовый файл и сохраните как `index.html`.

```html
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>ЭкоФото — фиксация загрязнений</title>
<meta name="description" content="ЭкоФото: сфотографируйте мусор, умирающие деревья или сжигание отходов заводом. Приложение запишет дату, время и GPS-место для жалобы в экологическую службу.">
<meta name="robots" content="index, follow">
<meta name="theme-color" content="#2e7d32">
<meta property="og:type" content="website">
<meta property="og:title" content="ЭкоФото — фиксация загрязнений">
<meta property="og:description" content="Фото, дата и GPS-место загрязнения для жалобы в экологическую службу.">
<meta property="og:locale" content="ru_RU">
<style>
:root{--bg:#f4f7f4;--card:#fff;--tx:#1d2a1d;--mut:#667;--ac:#2e7d32;--bd:#d8e2d8;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#121712;--card:#1b231b;--tx:#e8f0e8;--mut:#9ab;--ac:#66bb6a;--bd:#2c382c}}
:root[data-theme="dark"]{--bg:#121712;--card:#1b231b;--tx:#e8f0e8;--mut:#9ab;--ac:#66bb6a;--bd:#2c382c}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--tx);font:16px/1.4 system-ui,sans-serif}
main{max-width:520px;margin:0 auto;padding:16px}
h1{font-size:22px;margin:0 0 12px}
.card{background:var(--card);border:1px solid var(--bd);border-radius:14px;padding:14px;margin-bottom:14px}
label{display:block;font-size:14px;color:var(--mut);margin:10px 0 4px}
select,textarea,input[type=text]{width:100%;padding:10px;border-radius:10px;border:1px solid var(--bd);background:var(--bg);color:var(--tx);font:inherit}
button,.btn{display:block;width:100%;padding:14px;border:0;border-radius:12px;background:var(--ac);color:#fff;font:600 16px system-ui;text-align:center;cursor:pointer;margin-top:12px}
button.sec{background:transparent;color:var(--ac);border:1px solid var(--ac)}
#preview{width:100%;border-radius:10px;margin-top:10px;display:none}
.meta{font-size:13px;color:var(--mut);margin-top:6px}
.item{display:flex;gap:10px;margin-bottom:12px}
.item img{width:84px;height:84px;object-fit:cover;border-radius:10px}
.item b{display:block}
.item small{color:var(--mut);display:block}
.row{display:flex;gap:8px}
.row button{margin-top:6px;padding:8px;font-size:13px}
#mapbtn{position:fixed;right:14px;bottom:calc(14px + env(safe-area-inset-bottom,0px));width:48px;height:48px;border-radius:50%;padding:0;margin:0;font-size:22px;z-index:5;box-shadow:0 2px 8px rgba(0,0,0,.3)}
#mappanel{position:fixed;left:0;right:0;bottom:0;max-height:80%;background:var(--card);border-top:1px solid var(--bd);border-radius:16px 16px 0 0;padding:14px;z-index:6;display:none;overflow:auto;padding-bottom:calc(14px + env(safe-area-inset-bottom,0px))}
#mebadge{position:fixed;top:calc(10px + env(safe-area-inset-top,0px));right:10px;max-width:70%;background:var(--card);color:var(--tx);border:2px solid #6a1b9a;border-radius:12px;padding:8px 10px;font-size:13px;line-height:1.3;z-index:7;display:none;box-shadow:0 2px 8px rgba(0,0,0,.3)}
#mebadge a{color:#6a1b9a;font-weight:600}
</style>
</head>
<body>
<main>
<h1>🌿 ЭкоФото</h1>
<div class="card" id="reg" style="display:none">
  <b>Регистрация</b>
  <label>ФИО</label>
  <input type="text" id="rname" placeholder="Иванов Иван Иванович">
  <label>ИИН (12 цифр)</label>
  <input type="text" id="riin" inputmode="numeric" maxlength="12" placeholder="123456789012">
  <label>Телефон</label>
  <input type="text" id="rphone" inputmode="tel" placeholder="+7 700 000 00 00">
  <div class="meta" id="rerr" style="color:#c62828"></div>
  <button id="rsave">Зарегистрироваться</button>
  <div class="meta">Данные хранятся только на этом устройстве и никуда не отправляются.</div>
  <div style="margin-top:14px;padding-top:12px;border-top:1px solid var(--bd)">
    <b>Для чего это приложение</b>
    <div class="meta" style="font-size:14px">Фотофиксация загрязнений: мусор, умирающие деревья, сжигание отходов заводами. Снимайте, и приложение запишет дату, время и место, чтобы отправить жалобу в службу.</div>
    <details style="margin-top:8px">
      <summary style="color:var(--ac);font-weight:600;cursor:pointer">Подробнее</summary>
      <div class="meta" style="font-size:14px;margin-top:8px">
        <p><b>Что умеет:</b> делает фото, определяет GPS, собирает отчёты, показывает мини-карту и ваше местоположение, отправляет заявление по почте или в мессенджер.</p>
        <p><b>Зачем ИИН и ФИО:</b> чтобы в заявлении были данные заявителя. Без этого многие службы не принимают обращение.</p>
        <p><b>Где хранятся данные:</b> только в браузере вашего телефона. На сервер они не попадают. ИИН проверяется только на корректность номера, принадлежность вам не проверяется.</p>
        <p><b>Как пользоваться:</b> 1) зарегистрируйтесь; 2) нажмите «Сделать фото»; 3) выберите тип, укажите адрес; 4) нажмите «Сохранить» и «Отправить».</p>
        <p><b>Срочно:</b> при пожаре или опасном дыме звоните 112.</p>
      </div>
    </details>
  </div>
</div>
<div class="meta" id="userbar" style="margin:0 0 10px"></div>
<button class="sec" id="logout" style="display:none;margin:0 0 14px">Выйти / сменить данные</button>
<div class="card">
  <label class="btn" for="file" style="margin-top:0">📷 Сделать фото</label>
  <input id="file" type="file" accept="image/*" capture="environment" hidden>
  <img id="preview" alt="Превью">
  <div class="meta" id="meta">Фото ещё не сделано</div>
  <label>Тип нарушения</label>
  <select id="type">
    <option>Свалка / мусор</option>
    <option>Умирающее / сухое дерево</option>
    <option>Сжигание мусора / дым завода</option>
    <option>Другое</option>
  </select>
  <label>Координаты (если GPS не работает)</label>
  <input type="text" id="coords" placeholder="51.1282, 71.4304 или ссылка из 2ГИС">
  <label>Адрес / описание места</label>
  <input type="text" id="addr" placeholder="Например: ул. Заводская, у забора завода">
  <label>Комментарий</label>
  <textarea id="note" rows="2" placeholder="Что происходит, как давно, направление ветра"></textarea>
  <button id="save">💾 Сохранить отчёт</button>
</div>
<div class="card">
  <b>Мои отчёты</b>
  <div id="list" style="margin-top:10px"></div>
  <label>E-mail службы, куда отправлять отчёты</label>
  <input type="text" id="mailto" inputmode="email" placeholder="example@ecology.gov.kz">
  <a class="btn" href="tel:112" style="background:#c62828">📞 Срочно: позвонить 112</a>
  <button class="sec" id="export">⬇️ Скачать текст отчётов</button>
</div>
</main>
<button id="mapbtn" style="display:none" title="Мини-карта">🗺</button>
<div id="mebadge"></div>
<div id="mappanel">
  <div style="display:flex;justify-content:space-between;align-items:center">
    <b>Мини-карта отчётов</b>
    <button class="sec" id="mapclose" style="width:auto;margin:0;padding:6px 14px">✕</button>
  </div>
  <button id="locbtn" style="margin:10px 0 0">📍 Где я нахожусь</button>
  <div class="row" style="margin-top:8px">
    <button class="sec" id="zin" style="font-size:20px;padding:6px">＋</button>
    <button class="sec" id="zout" style="font-size:20px;padding:6px">－</button>
    <button class="sec" id="zreset" style="font-size:14px;padding:6px">⟲ Сбросить</button>
  </div>
  <svg id="mapsvg" viewBox="0 0 300 220" style="width:100%;background:var(--bg);border:1px solid var(--bd);border-radius:10px;margin-top:10px"></svg>
  <div class="meta" id="mapinfo">Нажмите на значок</div>
  <img id="mapimg" alt="" style="display:none;width:100%;border-radius:10px;margin-top:8px">
  <div class="meta">🗑️ мусор/свалка · 🥀 умирающее дерево · 🔥 сжигание/дым · ⚠️ другое · 🟣 я</div>
  <a id="maplink" class="btn" style="display:none" target="_blank" rel="noopener">Открыть в Google Maps</a>
  <a id="maplink2" class="btn" style="display:none;background:#1ba136" target="_blank" rel="noopener">Открыть в 2ГИС</a>
</div>
<script>
var reports = [], cur = null, mem = [];
try { reports = JSON.parse(localStorage.getItem("eco") || "[]"); } catch (e) { reports = []; }

function persist() {
  try { localStorage.setItem("eco", JSON.stringify(reports)); }
  catch (e) { alert("Не хватило места для хранения. Удалите старые отчёты."); }
}
function shrink(file, cb) {
  var r = new FileReader();
  r.onload = function () {
    var img = new Image();
    img.onload = function () {
      var s = Math.min(1, 1000 / Math.max(img.width, img.height));
      var c = document.createElement("canvas");
      c.width = img.width * s; c.height = img.height * s;
      c.getContext("2d").drawImage(img, 0, 0, c.width, c.height);
      cb(c.toDataURL("image/jpeg", 0.7));
    };
    img.src = r.result;
  };
  r.readAsDataURL(file);
}
function fmt(t) { return new Date(t).toLocaleString("ru-RU"); }

document.getElementById("file").onchange = function (e) {
  var f = e.target.files[0];
  if (!f) return;
  cur = { time: Date.now(), lat: null, lon: null };
  var m = document.getElementById("meta");
  m.textContent = "Дата: " + fmt(cur.time) + " · определяю место…";
  shrink(f, function (d) {
    cur.img = d;
    var p = document.getElementById("preview");
    p.src = d; p.style.display = "block";
  });
  try {
    navigator.geolocation.getCurrentPosition(function (pos) {
      cur.lat = pos.coords.latitude; cur.lon = pos.coords.longitude;
      m.textContent = "Дата: " + fmt(cur.time) + " · GPS: " + cur.lat.toFixed(5) + ", " + cur.lon.toFixed(5);
    }, function () {
      m.textContent = "Дата: " + fmt(cur.time) + " · GPS недоступен, укажите адрес";
    }, { enableHighAccuracy: true, timeout: 10000 });
  } catch (err) {
    m.textContent = "Дата: " + fmt(cur.time) + " · GPS недоступен, укажите адрес";
  }
};

document.getElementById("save").onclick = function () {
  if (!cur || !cur.img) { alert("Сначала сделайте фото"); return; }
  var mc = parseCoords(document.getElementById("coords").value);
  if ((cur.lat === null || cur.lat === undefined) && mc) { cur.lat = mc.lat; cur.lon = mc.lon; }
  document.getElementById("coords").value = "";
  cur.type = document.getElementById("type").value;
  cur.addr = document.getElementById("addr").value;
  cur.note = document.getElementById("note").value;
  reports.unshift(cur);
  var justSaved = cur;
  persist();
  cur = null;
  document.getElementById("preview").style.display = "none";
  document.getElementById("note").value = "";
  document.getElementById("meta").textContent = "Отчёт сохранён. Можно снять ещё.";
  render();
  toast("Отчёт сохранён. Нажмите «📨 Отправить» у отчёта, чтобы дать его службе.");
};

function render() {
  var l = document.getElementById("list");
  l.innerHTML = "";
  if (!reports.length) { l.textContent = "Пока пусто"; return; }
  reports.forEach(function (r, i) {
    var d = document.createElement("div");
    d.className = "item";
    var img = document.createElement("img"); img.src = r.img;
    var t = document.createElement("div");
    var b = document.createElement("b"); b.textContent = r.type;
    var s1 = document.createElement("small"); s1.textContent = fmt(r.time);
    var s2 = document.createElement("small");
    s2.textContent = (r.lat ? r.lat.toFixed(5) + ", " + r.lon.toFixed(5) : "") + " " + (r.addr || "");
    var s3 = document.createElement("small"); s3.textContent = r.note || "";
    var row = document.createElement("div"); row.className = "row";
    var del = document.createElement("button"); del.className = "sec"; del.textContent = "Удалить";
    del.onclick = function () { reports.splice(i, 1); persist(); render(); };
    var snd = document.createElement("button"); snd.textContent = "📨 Отправить";
    snd.onclick = function () { sendReport(r); };
    row.appendChild(snd);
    row.appendChild(del);
    [b, s1, s2, s3, row].forEach(function (x) { t.appendChild(x); });
    d.appendChild(img); d.appendChild(t);
    l.appendChild(d);
  });
}

document.getElementById("export").onclick = function () {
  if (!reports.length) return;
  var hdr = user ? "Заявитель: " + user.name + "\nИИН: " + user.iin + "\nТелефон: " + (user.phone || "-") + "\n\n" : "";
  var txt = hdr + reports.map(function (r, i) {
    return (i + 1) + ". " + r.type + "\nДата: " + fmt(r.time) +
      "\nGPS: " + (r.lat ? r.lat + ", " + r.lon : "нет") +
      "\nАдрес: " + (r.addr || "-") + "\nКомментарий: " + (r.note || "-") + "\n";
  }).join("\n");
  var w = window.open("", "_blank");
  if (w) { w.document.write("<pre>" + txt.replace(/</g, "&lt;") + "</pre>"); }
  else { alert(txt); }
};
var user = null;
try { user = JSON.parse(localStorage.getItem("ecouser") || "null"); } catch (e) { user = null; }

function checkIin(s) {
  if (!/^\d{12}$/.test(s)) return false;
  var d = s.split("").map(Number);
  var mm = +s.slice(2, 4), dd = +s.slice(4, 6);
  if (mm < 1 || mm > 12 || dd < 1 || dd > 31) return false;
  if (d[6] < 1 || d[6] > 6) return false;
  var w1 = [1,2,3,4,5,6,7,8,9,10,11], w2 = [3,4,5,6,7,8,9,10,11,1,2], i, c = 0;
  for (i = 0; i < 11; i++) c += d[i] * w1[i];
  c %= 11;
  if (c === 10) {
    c = 0;
    for (i = 0; i < 11; i++) c += d[i] * w2[i];
    c %= 11;
    if (c === 10) return false;
  }
  return c === d[11];
}

function showState() {
  var cards = document.querySelectorAll(".card");
  for (var i = 0; i < cards.length; i++) {
    if (cards[i].id === "reg") cards[i].style.display = user ? "none" : "block";
    else cards[i].style.display = user ? "block" : "none";
  }
  document.getElementById("userbar").textContent = user ? "👤 " + user.name + " · ИИН " + user.iin.slice(0, 6) + "******" : "";
  document.getElementById("logout").style.display = user ? "block" : "none";
  document.getElementById("mapbtn").style.display = user ? "block" : "none";
}

document.getElementById("rsave").onclick = function () {
  var name = document.getElementById("rname").value.trim();
  var iin = document.getElementById("riin").value.trim();
  var phone = document.getElementById("rphone").value.trim();
  var err = document.getElementById("rerr");
  if (name.split(/\s+/).length < 2) { err.textContent = "Укажите фамилию и имя"; return; }
  if (!checkIin(iin)) { err.textContent = "ИИН неверный: нужно 12 цифр, проверьте номер"; return; }
  err.textContent = "";
  user = { name: name, iin: iin, phone: phone };
  try { localStorage.setItem("ecouser", JSON.stringify(user)); } catch (e) {}
  showState();
};

document.getElementById("logout").onclick = function () {
  var lb = document.getElementById("logout");
  if (!lb.dataset.sure) {
    lb.dataset.sure = "1";
    lb.textContent = "Нажмите ещё раз: данные регистрации удалятся (отчёты останутся)";
    setTimeout(function () { delete lb.dataset.sure; lb.textContent = "Выйти / сменить данные"; }, 5000);
    return;
  }
  delete lb.dataset.sure;
  lb.textContent = "Выйти / сменить данные";
  user = null;
  try { localStorage.removeItem("ecouser"); } catch (e) {}
  showState();
};

try { document.getElementById("mailto").value = localStorage.getItem("ecomail") || ""; } catch (e) {}

function reportText(r) {
  return "Заявление о загрязнении окружающей среды\n" +
    (user ? "Заявитель: " + user.name + "\nИИН: " + user.iin + "\nТелефон: " + (user.phone || "-") + "\n" : "") +
    "\nТип: " + r.type + "\nДата и время: " + fmt(r.time) +
    "\nGPS: " + (r.lat ? r.lat + ", " + r.lon + " (https://maps.google.com/?q=" + r.lat + "," + r.lon + ")" : "нет") +
    "\nАдрес: " + (r.addr || "-") + "\nКомментарий: " + (r.note || "-");
}

function sendReport(r) {
  var text = reportText(r);
  var mail = document.getElementById("mailto").value.trim();
  try { localStorage.setItem("ecomail", mail); } catch (e) {}
  function viaMail() {
    if (!mail) { alert("Укажите e-mail службы под списком отчётов, затем нажмите «Отправить» ещё раз."); return; }
    alert("Откроется почта с текстом заявления. Фото прикрепите вручную из галереи телефона.");
    location.href = "mailto:" + mail + "?subject=" + encodeURIComponent("Загрязнение: " + r.type) + "&body=" + encodeURIComponent(text);
  }
  try {
    var p = r.img.split(","), bin = atob(p[1]), arr = new Uint8Array(bin.length);
    for (var i = 0; i < bin.length; i++) arr[i] = bin.charCodeAt(i);
    var f = new File([arr], "foto.jpg", { type: "image/jpeg" });
    if (navigator.canShare && navigator.canShare({ files: [f] })) {
      navigator.share({ title: "Загрязнение", text: text, files: [f] }).catch(function (e) {
        if (!e || e.name !== "AbortError") viaMail();
      });
    } else { viaMail(); }
  } catch (e) { viaMail(); }
}

function parseCoords(s) {
  s = s || "";
  var m = s.match(/(-?\d{1,3}\.\d+)\s*[,;\s]\s*(-?\d{1,3}\.\d+)/);
  if (!m) return null;
  var a = parseFloat(m[1]), b = parseFloat(m[2]);
  if (s.indexOf("2gis") >= 0 || a > 56) { var t = a; a = b; b = t; }
  if (Math.abs(a) <= 90 && Math.abs(b) <= 180) return { lat: a, lon: b };
  return null;
}

function toast(m) {
  var t = document.getElementById("toast");
  if (!t) {
    t = document.createElement("div"); t.id = "toast";
    t.style.cssText = "position:fixed;left:12px;right:12px;bottom:80px;background:#222;color:#fff;padding:12px;border-radius:10px;z-index:20;font-size:14px;text-align:center";
    document.body.appendChild(t);
  }
  t.textContent = m; t.style.display = "block";
  clearTimeout(t._h);
  t._h = setTimeout(function () { t.style.display = "none"; }, 4500);
}
window.alert = function (m) { toast(m); };

var myPos = null;

function showBadge() {
  var b = document.getElementById("mebadge");
  while (b.firstChild) b.removeChild(b.firstChild);
  var near = null, best = 1e9, k = Math.cos(myPos.lat * Math.PI / 180);
  reports.forEach(function (r) {
    if (r.lat === null || r.lat === undefined || !r.addr) return;
    var dx = (r.lon - myPos.lon) * k * 111320, dy = (r.lat - myPos.lat) * 110540;
    var d = Math.sqrt(dx * dx + dy * dy);
    if (d < best) { best = d; near = r; }
  });
  var t1 = document.createElement("b"); t1.textContent = "📍 Я здесь";
  var t2 = document.createElement("div"); t2.textContent = myPos.lat.toFixed(5) + ", " + myPos.lon.toFixed(5);
  var t3 = document.createElement("div");
  if (near && best < 500) t3.textContent = "Адрес рядом (~" + Math.round(best) + " м): " + near.addr;
  else t3.textContent = "Адреса рядом нет в отчётах. Откройте карту.";
  var l = document.createElement("a");
  l.href = "https://maps.google.com/?q=" + myPos.lat + "," + myPos.lon;
  l.target = "_blank"; l.rel = "noopener"; l.textContent = "Адрес на Google Maps";
  var l2 = document.createElement("a");
  l2.href = "https://2gis.kz/?m=" + myPos.lon + "," + myPos.lat + "/17";
  l2.target = "_blank"; l2.rel = "noopener"; l2.textContent = "Адрес в 2ГИС";
  [t1, t2, t3, l, document.createElement("br"), l2].forEach(function (x) { b.appendChild(x); });
  b.style.display = "block";
}

function locate() {
  var info = document.getElementById("mapinfo");
  info.textContent = "Определяю ваше местоположение…";
  try {
    navigator.geolocation.getCurrentPosition(function (pos) {
      myPos = { lat: pos.coords.latitude, lon: pos.coords.longitude };
      drawMap();
      info.textContent = "Вы здесь: " + myPos.lat.toFixed(5) + ", " + myPos.lon.toFixed(5) + " (точность ±" + Math.round(pos.coords.accuracy) + " м)";
      var a = document.getElementById("maplink");
      a.href = "https://maps.google.com/?q=" + myPos.lat + "," + myPos.lon;
      var b2 = document.getElementById("maplink2");
      b2.href = "https://2gis.kz/?m=" + myPos.lon + "," + myPos.lat + "/17";
      b2.textContent = "Открыть моё место в 2ГИС";
      b2.style.display = "block";
      a.textContent = "Открыть моё место в Google Maps";
      showBadge();
      a.style.display = "block";
    }, function (err) {
      var mp = parseCoords(document.getElementById("coords").value);
      if (mp) {
        myPos = mp; drawMap(); showBadge();
        info.textContent = "Показано место из поля «Координаты» (GPS недоступен).";
        return;
      }
      var c = err && err.code;
      info.textContent = c === 1 ? "Геолокация запрещена или заблокирована браузером внутри этой страницы. Ниже есть запасной способ." :
        c === 2 ? "Местоположение недоступно. Включите GPS (геолокацию) в настройках телефона." :
        "Время ожидания истекло. Выйдите на открытое место или нажмите «Где я нахожусь» ещё раз.";
      var g = document.getElementById("maplink");
      g.href = "https://www.google.com/maps";
      g.textContent = "Открыть Google Maps (нажмите «Моё местоположение»)";
      g.style.display = "block";
      var g2 = document.getElementById("maplink2");
      g2.href = "https://2gis.kz/";
      g2.textContent = "Открыть 2ГИС и найти себя";
      g2.style.display = "block";
    }, { enableHighAccuracy: false, timeout: 15000, maximumAge: 60000 });
  } catch (e) {
    info.textContent = "Геолокация недоступна в этом браузере.";
  }
}

function typeIcon(t) {
  if (t.indexOf("Сжигание") === 0) return "🔥";
  if (t.indexOf("Умирающее") === 0) return "🥀";
  if (t.indexOf("Свалка") === 0) return "🗑️";
  return "⚠️";
}

function typeColor(t) {
  if (t.indexOf("Сжигание") === 0) return "#c62828";
  if (t.indexOf("Умирающее") === 0) return "#2e7d32";
  if (t.indexOf("Свалка") === 0) return "#8d6e63";
  return "#1565c0";
}

function drawMap() {
  var svg = document.getElementById("mapsvg"), NS = "http://www.w3.org/2000/svg";
  while (svg.firstChild) svg.removeChild(svg.firstChild);
  applyVB();
  var pts = reports.filter(function (r) { return r.lat !== null && r.lat !== undefined; });
  var all = pts.slice();
  if (myPos) all.push(myPos);
  var info = document.getElementById("mapinfo");
  document.getElementById("maplink").style.display = "none";
  document.getElementById("maplink2").style.display = "none";
  if (!all.length) { info.textContent = "Нет отчётов с GPS. После съёмки разрешите доступ к геолокации."; return; }
  info.textContent = "На карте отмечено мест: " + pts.length + (reports.length > pts.length ? " (без места: " + (reports.length - pts.length) + " — укажите координаты)" : "") + ". Нажмите на значок.";
  document.getElementById("mapimg").style.display = "none";
  var minLat = 90, maxLat = -90, minLon = 180, maxLon = -180;
  all.forEach(function (r) {
    minLat = Math.min(minLat, r.lat); maxLat = Math.max(maxLat, r.lat);
    minLon = Math.min(minLon, r.lon); maxLon = Math.max(maxLon, r.lon);
  });
  var k = Math.cos(((minLat + maxLat) / 2) * Math.PI / 180);
  var spanX = (maxLon - minLon) * k, spanY = maxLat - minLat;
  var span = Math.max(spanX, spanY, 0.004);
  var offX = (span - spanX) / 2, offY = (span - spanY) / 2;
  for (var g = 1; g < 6; g++) {
    var gl = document.createElementNS(NS, "line");
    gl.setAttribute("x1", g * 50); gl.setAttribute("x2", g * 50);
    gl.setAttribute("y1", 0); gl.setAttribute("y2", 220);
    gl.setAttribute("stroke", "#8884"); svg.appendChild(gl);
    var gh = document.createElementNS(NS, "line");
    gh.setAttribute("y1", g * 36); gh.setAttribute("y2", g * 36);
    gh.setAttribute("x1", 0); gh.setAttribute("x2", 300);
    gh.setAttribute("stroke", "#8884"); svg.appendChild(gh);
  }
  var north = document.createElementNS(NS, "text");
  north.setAttribute("x", 8); north.setAttribute("y", 16);
  north.setAttribute("fill", "#888"); north.setAttribute("font-size", "12");
  north.textContent = "С ↑"; svg.appendChild(north);
  pts.forEach(function (r) {
    var x = 30 + ((r.lon - minLon) * k + offX) / span * 240;
    var y = 190 - ((r.lat - minLat) + offY) / span * 160;
    var c = document.createElementNS(NS, "circle");
    c.setAttribute("cx", x); c.setAttribute("cy", y); c.setAttribute("r", 14);
    c.setAttribute("fill", "#ffffff"); c.setAttribute("stroke", typeColor(r.type));
    c.setAttribute("stroke-width", 3); c.style.cursor = "pointer";
    c.onclick = function () {
      info.textContent = typeIcon(r.type) + " " + r.type + " · " + fmt(r.time) + (r.addr ? " · " + r.addr : "") + (r.note ? " · " + r.note : "");
      var mi = document.getElementById("mapimg");
      mi.src = r.img; mi.style.display = "block";
      var a = document.getElementById("maplink");
      a.href = "https://maps.google.com/?q=" + r.lat + "," + r.lon;
      a.textContent = "Открыть в Google Maps";
      var a2 = document.getElementById("maplink2");
      a2.href = "https://2gis.kz/?m=" + r.lon + "," + r.lat + "/17";
      a2.textContent = "Открыть в 2ГИС";
      a2.style.display = "block";
      a.style.display = "block";
    };
    svg.appendChild(c);
    var ic = document.createElementNS(NS, "text");
    ic.setAttribute("x", x); ic.setAttribute("y", y + 5);
    ic.setAttribute("text-anchor", "middle"); ic.setAttribute("font-size", "15");
    ic.style.pointerEvents = "none"; ic.textContent = typeIcon(r.type);
    svg.appendChild(ic);
  });
  if (myPos) {
    var mx = 30 + ((myPos.lon - minLon) * k + offX) / span * 240;
    var my = 190 - ((myPos.lat - minLat) + offY) / span * 160;
    var halo = document.createElementNS(NS, "circle");
    halo.setAttribute("cx", mx); halo.setAttribute("cy", my); halo.setAttribute("r", 16);
    halo.setAttribute("fill", "#6a1b9a44"); svg.appendChild(halo);
    var me = document.createElementNS(NS, "circle");
    me.setAttribute("cx", mx); me.setAttribute("cy", my); me.setAttribute("r", 7);
    me.setAttribute("fill", "#6a1b9a"); me.setAttribute("stroke", "#fff"); me.setAttribute("stroke-width", 2);
    svg.appendChild(me);
    var lb = document.createElementNS(NS, "text");
    lb.setAttribute("x", mx + 12); lb.setAttribute("y", my - 12);
    lb.setAttribute("fill", "#6a1b9a"); lb.setAttribute("font-size", "13"); lb.setAttribute("font-weight", "bold");
    lb.textContent = "Я"; svg.appendChild(lb);
  }
}

document.getElementById("mapbtn").onclick = function () {
  resetVB();
  drawMap();
  locate();
  document.getElementById("mappanel").style.display = "block";
};
document.getElementById("locbtn").onclick = locate;

var vb = { x: 0, y: 0, w: 300, h: 220 };
function applyVB() {
  document.getElementById("mapsvg").setAttribute("viewBox", vb.x + " " + vb.y + " " + vb.w + " " + vb.h);
}
function zoomMap(f) {
  var cx = vb.x + vb.w / 2, cy = vb.y + vb.h / 2;
  var nw = Math.min(1200, Math.max(20, vb.w / f));
  vb.h = nw * 220 / 300; vb.w = nw;
  vb.x = cx - vb.w / 2; vb.y = cy - vb.h / 2;
  applyVB();
}
function resetVB() { vb = { x: 0, y: 0, w: 300, h: 220 }; applyVB(); }
document.getElementById("zin").onclick = function () { zoomMap(1.5); };
document.getElementById("zout").onclick = function () { zoomMap(1 / 1.5); };
document.getElementById("zreset").onclick = resetVB;
(function () {
  var s = document.getElementById("mapsvg"), drag = null;
  s.style.touchAction = "none";
  s.addEventListener("pointerdown", function (e) { drag = { x: e.clientX, y: e.clientY }; });
  s.addEventListener("pointermove", function (e) {
    if (!drag) return;
    var k = vb.w / s.clientWidth;
    vb.x -= (e.clientX - drag.x) * k; vb.y -= (e.clientY - drag.y) * k;
    drag = { x: e.clientX, y: e.clientY };
    applyVB();
  });
  var end = function () { drag = null; };
  s.addEventListener("pointerup", end);
  s.addEventListener("pointerleave", end);
  s.addEventListener("pointercancel", end);
})();

document.getElementById("mapclose").onclick = function () {
  document.getElementById("mappanel").style.display = "none";
  document.getElementById("mebadge").style.display = "none";
};

showState();
render();
</script>
</body>
</html>
```

Это полный код сайта. Если нужно, разделю его на три файла: `index.html`, `style.css` и `app.js`.
