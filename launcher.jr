(function () {
  function padLead(n, l) {
    return n.toString().padStart(l, "0");
  }

  function formatDateInput(date) {
    return (
      date.getFullYear() +
      "-" +
      padLead(date.getMonth() + 1, 2) +
      "-" +
      padLead(date.getDate(), 2)
    );
  }

  function formatTimeInputs(date) {
    return {
      h: padLead(date.getHours(), 2),
      m: padLead(date.getMinutes(), 2),
      s: padLead(date.getSeconds(), 2),
      ms: padLead(date.getMilliseconds(), 3),
    };
  }

  function savePreset(type, data) {
    localStorage.setItem("sa-preset-" + type, JSON.stringify(data));
  }

  function loadPreset(type) {
    let p = localStorage.getItem("sa-preset-" + type);
    return p ? JSON.parse(p) : null;
  }

  function fillFormFromPreset(preset) {
    if (!preset) return;
    $('input[name="sa-d"]').val(preset.d);
    $('input[name="sa-t-h"]').val(preset.h);
    $('input[name="sa-t-m"]').val(preset.m);
    $('input[name="sa-t-s"]').val(preset.s);
    $('input[name="sa-t-ms"]').val(preset.ms);
  }

  function getPresetLabel(type) {
    let p = loadPreset(type);
    if (!p) return "";
    let t = `${p.d} ${padLead(p.h,2)}:${padLead(p.m,2)}:${padLead(p.s,2)}:${padLead(p.ms,3)}`;
    return `<div style='color:#888;font-size:11px;margin-top:3px;text-align:center' class="sa-preset-label" data-type="${type}">(${t})</div>`;
  }

  function makePresetRow(type, name) {
    return `<div class="sa-preset-box" style="margin-bottom:16px;padding:8px 8px 5px 8px;border:1.5px solid #c2c2c2;border-radius:9px;box-shadow:0 1px 2px #0001;background:#fafbfd;max-width:220px;display:inline-block;vertical-align:top;margin-right:10px">
      <div style="font-weight:bold;margin-bottom:5px;text-align:center">${name}</div>
      <div style="text-align:center">
        <button type="button" class="sa-load-preset sa-btn-red" data-type="${type}">Folosește</button> 
        <button type="button" class="sa-save-preset sa-btn-blue" data-type="${type}">Salvează</button>
      </div>
      ${getPresetLabel(type)}
    </div>`;
  }

  function showToast(msg) {
    if (document.getElementById("vivok-toast")) {
      try {
        document.getElementById("vivok-toast").remove();
      } catch (e) {}
    }
    var t = document.createElement("div");
    t.id = "vivok-toast";
    t.innerHTML = msg;
    t.setAttribute("style", "position:fixed;z-index:99999;bottom:20px;left:50%;transform:translateX(-50%);background:rgba(30,30,30,0.94);color:#fff;padding:16px 34px;border-radius:15px;font-size:18px;box-shadow:0 4px 18px #0002;opacity:1;user-select:none;transition:opacity .4s;pointer-events:none;font-family:sans-serif;font-weight:600;text-align:center;");
    document.body.appendChild(t);
    setTimeout(() => t.style.opacity = "0", 4500);
    setTimeout(() => t.remove(), 5000);
  }

  if (document.getElementById("command-data-form") === null) {
    alert("Trebuie să fii pe pagina de confirmare a comenzii!");
    return;
  }

  if (!document.getElementById("sa-preset-style")) {
    document.head.insertAdjacentHTML("beforeend", `<style id="sa-preset-style">
      .sa-btn-red{background:#e53935;color:#fff;border:none;border-radius:6px;padding:3px 14px;margin-right:5px;font-size:13px;cursor:pointer;transition:background .15s}
      .sa-btn-red:hover{background:#b71c1c}
      .sa-btn-blue{background:#007bff;color:#fff;border:none;border-radius:6px;padding:3px 14px;font-size:13px;cursor:pointer;transition:background .15s}
      .sa-btn-blue:hover{background:#0056b3}
      .sa-preset-box{transition:box-shadow .18s,border .15s}
      .sa-preset-box:hover{box-shadow:0 2px 8px #007bff22;border-color:#007bff}
      .sa-warning-msg{margin-top:10px;margin-bottom:4px;color:#d9534f;font-size:12px;text-align:center;font-style:italic;user-select:none;}
    </style>`);
  }

  const autor = '<span style="float:right; padding:3px">Autor: DNS</span>';
  const warning = '<div class="sa-warning-msg">(⚠️ Co-Playerul ViVoK nu e \"legal\" – folosește-l doar pe barba ta! 😜)</div>';

  const container = $(
`<tr><td>Programeaza:</td><td style="position: relative">
  <table><tbody>
    <tr><td>Mod:</td><td><input name="sa-mod" type="radio" value="arrival" checked>Sosește la <input name="sa-mod" type="radio" value="launch">Lansează la</td></tr>
    <tr><td>Data:</td><td><input name="sa-d" type="date" required></td></tr>
    <tr><td>Ora:</td><td>
      <input name="sa-t-h" type="number" min="0" max="23" value="0" style="width: 40px" required>:
      <input name="sa-t-m" type="number" min="0" max="59" value="0" style="width: 40px" required>:
      <input name="sa-t-s" type="number" min="0" max="59" value="0" style="width: 40px" required>:
      <input name="sa-t-ms" type="number" min="0" max="999" value="0" style="width: 40px" required>
    </td></tr>
    <tr><td>Lansare:</td><td id="sa-launch"></td></tr>
    <tr><td>Sosire:</td><td id="sa-arrival"></td></tr>
    <tr><td>Întoarcere:</td><td id="sa-return"></td></tr>
    <tr><td style="vertical-align:top">Preseturi:</td><td>
      <div id="sa-presets" style="margin-bottom:4px">
        ${makePresetRow("full", "Full")}
        ${makePresetRow("nobil", "Nobil")}
        ${makePresetRow("fake", "Fake-Sprijin")}
      </div>
      ${warning}${autor}
    </td></tr>
    <tr><td></td><td><button type="button" id="sa-save" class="btn float_left">Programează</button></td></tr>
  </tbody></table>
</td></tr>`);

  $("#date_arrival").parents("table:first").attr("width", 500);
  $("#date_arrival").parent().after(container);

  const b = new Date();
  const t = formatTimeInputs(b);
  $('input[name="sa-d"]').val(formatDateInput(b));
  $('input[name="sa-t-h"]').val(t.h);
  $('input[name="sa-t-m"]').val(t.m);
  $('input[name="sa-t-s"]').val(t.s);
  $('input[name="sa-t-ms"]').val(t.ms);

  function getServerTime() {
    return Math.round(Timing.getCurrentServerTime());
  }

  function getDuration() {
    return 1000 * $(".relative_time").data("duration");
  }

  function getChosenDate() {
    var a = new Date($('input[name="sa-d"]').val());
    a.setHours($('input[name="sa-t-h"]').val());
    a.setMinutes($('input[name="sa-t-m"]').val());
    a.setSeconds($('input[name="sa-t-s"]').val());
    a.setMilliseconds($('input[name="sa-t-ms"]').val());
    return a;
  }

  function getMode() {
    return $('input[name="sa-mod"]:checked').val();
  }

  function updateDisplay(a, b, c) {
    function q(d) {
      return d instanceof Date
        ? window.Format.date(d / 1000, true) + ":" + window.Format.padLead(d.getUTCMilliseconds(), 3)
        : 0;
    }
    $("#sa-launch").text(q(a));
    $("#sa-arrival").text(q(b));
    $("#sa-return").text(q(c));
  }

  let timeoutRef = null;
  function scheduleAttack() {
    if ($("#command-data-form")[0].reportValidity()) {
      const arrival = getChosenDate();
      const duration = getDuration();
      const launch = getMode() === "arrival" ? new Date(arrival.getTime() - duration) : arrival;
      const realArrival = getMode() === "arrival" ? arrival : new Date(arrival.getTime() + duration);
      const ret = new Date(1000 * Math.floor(realArrival.getTime() / 100) + duration);
      if (timeoutRef) clearTimeout(timeoutRef);
      timeoutRef = setTimeout(() => $("#troop_confirm_submit").click(), launch.getTime() - getServerTime());
      updateDisplay(launch, realArrival, ret);
      showToast("📡 Atacul e pe traseu! Co-Playerul ViVoK susține că-i doar spectator.");
    }
  }

  $("#sa-save").off("click").on("click", scheduleAttack);

  $("#sa-presets").on("click", ".sa-save-preset", function () {
    const type = $(this).data("type");
    const data = {
      d: $('input[name="sa-d"]').val(),
      h: $('input[name="sa-t-h"]').val(),
      m: $('input[name="sa-t-m"]').val(),
      s: $('input[name="sa-t-s"]').val(),
      ms: $('input[name="sa-t-ms"]').val()
    };
    savePreset(type, data);
    updatePresetLabels();
    showToast("💾 Preset salvat! Să nu uiți unde l-ai pus!");
  });

  $("#sa-presets").on("click", ".sa-load-preset", function () {
    const type = $(this).data("type");
    const preset = loadPreset(type);
    if (!preset) {
      alert("Nu există preset salvat pentru '" + type + "'");
      return;
    }
    fillFormFromPreset(preset);
    showToast("🚀 Preset încărcat! Cineva o să plângă... sper că nu tu.");
  });

  function updatePresetLabels() {
    ["full", "nobil", "fake"].forEach(function (type) {
      $('.sa-preset-label[data-type="' + type + '"]').replaceWith(getPresetLabel(type));
    });
  }

  updatePresetLabels();

  $("body").append(`<div style="position:fixed;top:100px;right:20px;width:260px;background:#fdf8e4;color:#333;padding:12px 14px;border-radius:12px;box-shadow:0 0 8px rgba(0,0,0,0.1);font-size:12px;line-height:1.5;font-family:sans-serif;z-index:9999;">
    <b>📌 Ghid rapid: Lansator Vivok</b><br><br>
    1. Alege data și ora dorită<br>
    2. Apasă <b>Salvează</b> la Full / Nobil / Fake-Sprijin<br>
    3. Apasă <b>Programează</b> pentru lansarea curentă<br>
    4. Pentru alte atacuri la aceeași oră:<br>
    &nbsp;&nbsp;– Deschide alt tab cu următorul atac<br>
    &nbsp;&nbsp;– Dă click pe <b>Co-Playerul ViVoK</b> din bară<br>
    5. Timpul e deja completat<br><br>
    👉 Doar dă <b>Folosește</b> + <b>Programează</b><br><br>
    ✅ Gata! Fără completări repetate 💪
  </div>`);
})();
