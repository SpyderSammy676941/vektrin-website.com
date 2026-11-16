<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Vektrin</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@500;700&family=Figtree:wght@400;500;600&family=Fira+Code:wght@400;500&display=swap">
<style>
  /* Layout: one app window. A sidebar for chats and spaces (projects, library, memory, create), a calm main pane, a composer pinned to the bottom. Boldness lives in the glowing orb and the welcome screen only. */
  :root {
    --bg: #f2f4fb;
    --surface: #ffffff;
    --surface-2: #e9edfa;
    --ink: #131a36;
    --muted: #59628a;
    --line: #d9dff2;
    --accent: #4a55f5;
    --accent-2: #a259f7;
    --accent-ink: #ffffff;
    --danger: #c0392b;
    --code-bg: #10142b;
    --code-ink: #e6e9ff;
    --display: 'Bricolage Grotesque', system-ui, sans-serif;
    --body: 'Figtree', system-ui, -apple-system, 'Segoe UI', sans-serif;
    --mono: 'Fira Code', ui-monospace, Menlo, Consolas, monospace;
    color-scheme: light;
  }
  @media (prefers-color-scheme: dark) {
    :root:not([data-ui]) {
      --bg: #0b0e1a; --surface: #121629; --surface-2: #1b2039; --ink: #e9ecff; --muted: #9aa3cf; --line: #282e54;
      --accent: #7c8cff; --accent-2: #c084fc; --accent-ink: #0b0e1a; --danger: #ff8a7a; color-scheme: dark;
    }
  }
  :root[data-ui="daylight"] {
    --bg: #f2f4fb; --surface: #ffffff; --surface-2: #e9edfa; --ink: #131a36; --muted: #59628a; --line: #d9dff2;
    --accent: #4a55f5; --accent-2: #a259f7; --accent-ink: #ffffff; --danger: #c0392b; color-scheme: light;
  }
  :root[data-ui="midnight"] {
    --bg: #0b0e1a; --surface: #121629; --surface-2: #1b2039; --ink: #e9ecff; --muted: #9aa3cf; --line: #282e54;
    --accent: #7c8cff; --accent-2: #c084fc; --accent-ink: #0b0e1a; --danger: #ff8a7a; color-scheme: dark;
  }
  :root[data-ui="aurora"] {
    --bg: #071318; --surface: #0d1d24; --surface-2: #14292f; --ink: #e4faff; --muted: #8fb4c0; --line: #1f3b44;
    --accent: #43cff5; --accent-2: #9d8bfa; --accent-ink: #04141a; --danger: #ff8a7a; color-scheme: dark;
  }
  :root[data-ui="sunset"] {
    --bg: #170d12; --surface: #22141b; --surface-2: #2d1b24; --ink: #ffeee8; --muted: #d1a79b; --line: #442833;
    --accent: #ff7a59; --accent-2: #ffb347; --accent-ink: #1c0d08; --danger: #ff9a8a; color-scheme: dark;
  }

  * { box-sizing: border-box; }
  [hidden] { display: none !important; }
  html, body { height: 100%; }
  body {
    margin: 0;
    background: var(--bg);
    color: var(--ink);
    font-family: var(--body);
    font-size: 15.5px;
    line-height: 1.55;
    padding-block: 10px;
    padding-left: max(16px, env(safe-area-inset-left, 0px));
    padding-right: max(16px, env(safe-area-inset-right, 0px));
  }
  button, input, textarea, select { font: inherit; color: inherit; }
  button { cursor: pointer; }
  :focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
  h1, h2, h3 { font-family: var(--display); margin: 0; line-height: 1.15; }

  .orb {
    width: 30px; height: 30px; border-radius: 50%; flex: none;
    background: radial-gradient(circle at 30% 28%, rgba(255,255,255,0.75), transparent 38%), linear-gradient(135deg, var(--accent), var(--accent-2));
    box-shadow: 0 0 22px color-mix(in srgb, var(--accent) 55%, transparent);
    animation: breathe 5s ease-in-out infinite;
  }
  .orb.sm { width: 26px; height: 26px; box-shadow: 0 0 12px color-mix(in srgb, var(--accent) 45%, transparent); animation: none; }
  .orb.xl { width: 84px; height: 84px; box-shadow: 0 0 60px color-mix(in srgb, var(--accent) 60%, transparent); }
  @keyframes breathe { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.07); } }

  .app { height: 100%; display: grid; grid-template-columns: 290px minmax(0, 1fr); border: 1px solid var(--line); border-radius: 18px; overflow: hidden; background: var(--bg); }

  /* sidebar */
  .side { background: var(--surface); border-right: 1px solid var(--line); display: flex; flex-direction: column; gap: 12px; padding: 14px; min-height: 0; min-width: 0; }
  .side-top { display: flex; align-items: center; gap: 10px; }
  .side-top .name { font-family: var(--display); font-weight: 700; font-size: 1.2rem; flex: 1; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .btn { display: inline-flex; align-items: center; justify-content: center; gap: 8px; border: 1px solid var(--line); background: var(--surface-2); border-radius: 10px; padding: 9px 14px; min-height: 40px; font-weight: 500; }
  .btn:hover { border-color: var(--accent); }
  .btn.primary { background: linear-gradient(135deg, var(--accent), var(--accent-2)); color: var(--accent-ink); border-color: transparent; font-weight: 600; }
  .btn.primary:hover { filter: brightness(1.06); }
  .btn.ghost { background: transparent; }
  .btn.danger { color: var(--danger); }
  .btn.armed { background: var(--danger); color: #fff; border-color: transparent; }
  .btn.sm { min-height: 32px; padding: 4px 10px; font-size: 0.85rem; border-radius: 8px; }
  .icon-btn { display: inline-flex; align-items: center; justify-content: center; width: 40px; height: 40px; border-radius: 10px; border: 1px solid transparent; background: transparent; color: var(--muted); flex: none; }
  .icon-btn:hover { background: var(--surface-2); color: var(--ink); }
  .icon-btn.on { background: var(--accent); color: var(--accent-ink); }
  .icon-btn.armed { background: var(--danger); color: #fff; width: auto; padding: 0 10px; }

  .nav { display: flex; flex-direction: column; gap: 2px; }
  .nav button { display: flex; align-items: center; gap: 10px; background: transparent; border: 0; border-radius: 10px; padding: 9px 10px; text-align: left; color: var(--muted); font-weight: 500; }
  .nav button:hover { background: var(--surface-2); color: var(--ink); }
  .nav button[aria-current="page"] { background: color-mix(in srgb, var(--accent) 16%, transparent); color: var(--ink); }
  .search { width: 100%; background: var(--surface-2); border: 1px solid var(--line); border-radius: 10px; padding: 8px 12px; min-height: 38px; }
  .chat-list { flex: 1; min-height: 0; overflow-y: auto; display: flex; flex-direction: column; gap: 2px; }
  .grp { font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.08em; color: var(--muted); padding: 10px 10px 4px; }
  .chat-item { display: flex; align-items: center; gap: 8px; background: transparent; border: 0; border-radius: 10px; padding: 8px 10px; text-align: left; width: 100%; min-width: 0; }
  .chat-item:hover { background: var(--surface-2); }
  .chat-item[aria-current="true"] { background: color-mix(in srgb, var(--accent) 16%, transparent); }
  .chat-item span.t { flex: 1; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .dot { width: 8px; height: 8px; border-radius: 50%; background: var(--accent-2); flex: none; }
  .side-foot { border-top: 1px solid var(--line); padding-top: 12px; }
  .profile { display: flex; align-items: center; gap: 10px; width: 100%; background: transparent; border: 0; border-radius: 10px; padding: 6px; text-align: left; }
  .profile:hover { background: var(--surface-2); }
  .avatar { width: 32px; height: 32px; border-radius: 50%; background: var(--surface-2); border: 1px solid var(--line); display: inline-flex; align-items: center; justify-content: center; font-weight: 600; flex: none; }
  .profile .who { flex: 1; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

  /* main */
  .main { display: flex; flex-direction: column; min-width: 0; min-height: 0; }
  .topbar { display: none; align-items: center; gap: 8px; padding: 8px 10px; border-bottom: 1px solid var(--line); background: var(--surface); }
  .topbar .ttl { flex: 1; font-family: var(--display); font-weight: 700; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .view { flex: 1; min-height: 0; display: flex; flex-direction: column; }
  .scroll { flex: 1; min-height: 0; overflow-y: auto; padding: 28px clamp(16px, 4vw, 40px) 40px; display: flex; flex-direction: column; gap: 22px; }
  .pagehead { display: flex; align-items: flex-end; justify-content: space-between; gap: 12px; flex-wrap: wrap; }
  .pagehead h2 { font-size: clamp(1.5rem, 3.4vw, 2rem); }
  .pagehead p { margin: 4px 0 0; color: var(--muted); max-width: 56ch; }

  /* chat */
  .chat-head { display: flex; align-items: center; gap: 8px; padding: 10px 14px; border-bottom: 1px solid var(--line); flex-wrap: wrap; background: var(--surface); }
  .title-in { flex: 1; min-width: 140px; background: transparent; border: 1px solid transparent; border-radius: 8px; padding: 6px 8px; font-family: var(--display); font-weight: 700; font-size: 1.05rem; }
  .title-in:hover, .title-in:focus { border-color: var(--line); background: var(--surface-2); }
  select { background: var(--surface-2); border: 1px solid var(--line); border-radius: 10px; padding: 7px 10px; min-height: 38px; max-width: 100%; }
  .seg { display: inline-flex; background: var(--surface-2); border: 1px solid var(--line); border-radius: 10px; padding: 3px; gap: 2px; }
  .seg button { border: 0; background: transparent; border-radius: 8px; padding: 5px 12px; font-weight: 500; color: var(--muted); display: inline-flex; align-items: center; gap: 6px; min-height: 30px; }
  .seg button[aria-pressed="true"] { background: var(--surface); color: var(--ink); box-shadow: 0 1px 3px rgba(0,0,0,0.18); }
  .msgs { flex: 1; min-height: 0; overflow-y: auto; padding: 22px clamp(14px, 4vw, 40px); display: flex; flex-direction: column; gap: 22px; }
  .empty { margin: auto; text-align: center; display: flex; flex-direction: column; align-items: center; gap: 14px; max-width: 560px; padding: 20px 0; }
  .empty h2 { font-size: clamp(1.6rem, 4vw, 2.3rem); text-wrap: balance; }
  .empty p { margin: 0; color: var(--muted); }
  .suggest { display: flex; flex-wrap: wrap; gap: 8px; justify-content: center; margin-top: 6px; }
  .suggest button { background: var(--surface); border: 1px solid var(--line); border-radius: 999px; padding: 8px 14px; }
  .suggest button:hover { border-color: var(--accent); }
  .msg { display: flex; gap: 12px; max-width: 860px; width: 100%; margin: 0 auto; min-width: 0; }
  .msg .mbody { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 6px; }
  .msg .who { font-size: 0.8rem; color: var(--muted); font-weight: 600; }
  .bubble { overflow-wrap: anywhere; }
  .msg.user .bubble { background: var(--surface-2); border: 1px solid var(--line); border-radius: 14px; padding: 10px 14px; align-self: flex-start; max-width: 100%; }
  .bubble p { margin: 0 0 10px; }
  .bubble p:last-child, .bubble ul:last-child, .bubble ol:last-child { margin-bottom: 0; }
  .bubble ul, .bubble ol { margin: 0 0 10px; padding-left: 22px; }
  .bubble a { color: var(--accent); text-decoration: underline; }
  .bubble h4 { margin: 12px 0 6px; font-family: var(--display); font-size: 1.05rem; }
  .bubble code { font-family: var(--mono); font-size: 0.86em; background: var(--surface-2); border: 1px solid var(--line); border-radius: 5px; padding: 1px 5px; }
  .think { color: var(--muted); display: inline-flex; gap: 4px; align-items: center; }
  .think i { width: 6px; height: 6px; border-radius: 50%; background: var(--accent); animation: bob 1s infinite ease-in-out; }
  .think i:nth-child(2) { animation-delay: 0.15s; } .think i:nth-child(3) { animation-delay: 0.3s; }
  @keyframes bob { 0%, 80%, 100% { opacity: 0.25; transform: translateY(0); } 40% { opacity: 1; transform: translateY(-3px); } }
  .acts { display: flex; gap: 2px; flex-wrap: wrap; margin-left: -8px; }
  .acts .icon-btn { width: 32px; height: 32px; }
  .qchips { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 2px; }
  .qchips button { background: var(--surface); border: 1px solid var(--line); border-radius: 999px; padding: 6px 14px; font-size: 0.88rem; }
  .qchips button:hover { border-color: var(--accent); color: var(--accent); }
  .atts { display: flex; flex-wrap: wrap; gap: 8px; }
  .att { display: inline-flex; align-items: center; gap: 8px; background: var(--surface); border: 1px solid var(--line); border-radius: 10px; padding: 4px 10px 4px 4px; font-size: 0.85rem; max-width: 100%; }
  .att img { width: 38px; height: 38px; object-fit: cover; border-radius: 7px; }
  .att .ph { width: 38px; height: 38px; border-radius: 7px; background: var(--surface-2); display: inline-flex; align-items: center; justify-content: center; color: var(--muted); }
  .att span.n { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; max-width: 180px; }

  .cb { background: var(--code-bg); border-radius: 12px; overflow: hidden; margin: 6px 0 12px; border: 1px solid var(--line); }
  .cbh { display: flex; align-items: center; justify-content: space-between; gap: 8px; padding: 6px 8px 6px 14px; color: #9aa3cf; font-family: var(--mono); font-size: 0.75rem; border-bottom: 1px solid rgba(255,255,255,0.08); }
  .cbh .btn { background: transparent; color: #cfd5ff; border-color: rgba(255,255,255,0.16); }
  .cbh .row { display: flex; gap: 6px; }
  .cb pre { margin: 0; padding: 14px; overflow-x: auto; font-family: var(--mono); font-size: 0.83rem; line-height: 1.6; color: var(--code-ink); }

  .composer { padding: 8px clamp(12px, 3vw, 32px) 14px; display: flex; flex-direction: column; gap: 8px; }
  .composer > * { max-width: 860px; width: 100%; margin: 0 auto; }
  .chips { display: flex; flex-wrap: wrap; gap: 8px; }
  .cbox { background: var(--surface); border: 1px solid var(--line); border-radius: 18px; padding: 8px 8px 6px 14px; display: flex; flex-direction: column; }
  .cbox:focus-within { border-color: var(--accent); }
  .cbox textarea { border: 0; background: transparent; resize: none; outline: none; padding: 8px 0; max-height: 180px; min-height: 28px; width: 100%; }
  .cbar { display: flex; justify-content: space-between; align-items: center; gap: 6px; }
  .cbar .l, .cbar .r { display: flex; align-items: center; gap: 2px; }
  .send { width: 42px; height: 42px; border-radius: 12px; border: 0; background: linear-gradient(135deg, var(--accent), var(--accent-2)); color: var(--accent-ink); display: inline-flex; align-items: center; justify-content: center; }
  .send:disabled { opacity: 0.45; cursor: default; }
  .hintline { font-size: 0.78rem; color: var(--muted); text-align: center; }
  .banner { margin: 10px clamp(14px, 3vw, 32px) 0; background: color-mix(in srgb, var(--accent-2) 18%, transparent); border: 1px solid var(--line); border-radius: 12px; padding: 10px 14px; font-size: 0.9rem; }

  /* cards, grids */
  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 14px; }
  .card { background: var(--surface); border: 1px solid var(--line); border-radius: 14px; padding: 16px; display: flex; flex-direction: column; gap: 10px; min-width: 0; }
  .card h3 { font-size: 1.05rem; overflow-wrap: anywhere; }
  .card p.d { margin: 0; color: var(--muted); font-size: 0.92rem; overflow-wrap: anywhere; }
  .card .row { display: flex; gap: 6px; flex-wrap: wrap; margin-top: auto; }
  .card .mini { display: flex; flex-direction: column; gap: 2px; }
  .card .mini button { background: transparent; border: 0; text-align: left; color: var(--accent); padding: 2px 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .card pre { margin: 0; background: var(--code-bg); color: var(--code-ink); border-radius: 10px; padding: 10px; font-family: var(--mono); font-size: 0.76rem; overflow: hidden; max-height: 130px; white-space: pre-wrap; overflow-wrap: anywhere; }
  .card img.art { width: 100%; border-radius: 10px; background: var(--surface-2); display: block; max-width: 100%; }
  .tag { align-self: flex-start; font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.08em; color: var(--muted); border: 1px solid var(--line); border-radius: 999px; padding: 1px 8px; }
  .filters { display: flex; flex-wrap: wrap; gap: 8px; align-items: center; }
  .filters .btn[aria-pressed="true"] { background: var(--accent); color: var(--accent-ink); border-color: var(--accent); }
  .toolbar { display: flex; flex-wrap: wrap; gap: 10px; align-items: center; }
  .toolbar .search { max-width: 320px; }

  .form { display: flex; flex-direction: column; gap: 12px; max-width: 760px; }
  .field { display: flex; flex-direction: column; gap: 6px; }
  .field label { font-weight: 600; font-size: 0.88rem; }
  .field input[type=text], .field textarea { background: var(--surface); border: 1px solid var(--line); border-radius: 10px; padding: 10px 12px; width: 100%; }
  .field textarea { min-height: 96px; resize: vertical; }
  .field .help { color: var(--muted); font-size: 0.82rem; }
  .row-inline { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }
  .note { color: var(--muted); font-size: 0.88rem; max-width: 62ch; }

  .mem-list { display: flex; flex-direction: column; gap: 8px; max-width: 760px; }
  .mem { display: flex; align-items: flex-start; gap: 10px; background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 10px 12px; }
  .mem p { margin: 0; flex: 1; min-width: 0; overflow-wrap: anywhere; }
  .meter { height: 6px; background: var(--surface-2); border-radius: 999px; overflow: hidden; max-width: 320px; }
  .meter i { display: block; height: 100%; background: linear-gradient(90deg, var(--accent), var(--accent-2)); }
  .switch { display: inline-flex; align-items: center; gap: 10px; }
  .switch input { width: 20px; height: 20px; accent-color: var(--accent); }

  .skeleton { aspect-ratio: 1 / 1; border-radius: 10px; background: linear-gradient(110deg, var(--surface-2) 30%, var(--line) 50%, var(--surface-2) 70%); background-size: 200% 100%; animation: sheen 1.4s infinite linear; }
  @keyframes sheen { to { background-position: -200% 0; } }

  /* modal, toast, welcome */
  .modal { position: fixed; inset: 0; z-index: 60; display: flex; align-items: center; justify-content: center; background: rgba(5, 8, 20, 0.6); padding: 16px max(16px, env(safe-area-inset-right, 0px)) 16px max(16px, env(safe-area-inset-left, 0px)); }
  .sheet { background: var(--surface); border: 1px solid var(--line); border-radius: 16px; width: 100%; max-width: 520px; max-height: 100%; display: flex; flex-direction: column; overflow: hidden; }
  .sheet.wide { max-width: 980px; height: min(86vh, 760px); }
  .sheet header { display: flex; align-items: center; justify-content: space-between; padding: 14px 16px; border-bottom: 1px solid var(--line); }
  .sheet header h3 { font-size: 1.1rem; }
  .sheet .body { padding: 16px; overflow-y: auto; display: flex; flex-direction: column; gap: 14px; flex: 1; min-height: 0; }
  .sheet footer { padding: 12px 16px; border-top: 1px solid var(--line); display: flex; justify-content: flex-end; gap: 8px; flex-wrap: wrap; }
  .sheet iframe { width: 100%; flex: 1; min-height: 320px; border: 1px solid var(--line); border-radius: 10px; background: #fff; }
  .swatches { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; }
  .sw { border: 2px solid var(--line); border-radius: 12px; padding: 8px; background: var(--surface); display: flex; flex-direction: column; gap: 6px; align-items: stretch; font-size: 0.8rem; }
  .sw[aria-pressed="true"] { border-color: var(--accent); }
  .sw .bar { height: 26px; border-radius: 8px; display: block; border: 1px solid rgba(128,128,128,0.35); }
  .toast { position: fixed; left: 50%; transform: translateX(-50%); bottom: calc(24px + env(safe-area-inset-bottom, 0px)); background: var(--ink); color: var(--bg); padding: 10px 16px; border-radius: 12px; z-index: 80; max-width: calc(100% - 32px); box-shadow: 0 8px 30px rgba(0,0,0,0.35); }
  .scrim { display: none; }

  .welcome { position: fixed; inset: 0; z-index: 70; overflow-y: auto; background:
      radial-gradient(900px 500px at 15% -10%, color-mix(in srgb, var(--accent) 30%, transparent), transparent 60%),
      radial-gradient(800px 500px at 100% 100%, color-mix(in srgb, var(--accent-2) 28%, transparent), transparent 60%),
      var(--bg);
    padding-block: 32px;
    padding-left: max(16px, env(safe-area-inset-left, 0px));
    padding-right: max(16px, env(safe-area-inset-right, 0px));
    display: flex; }
  .welcome .inner { margin: auto; width: 100%; max-width: 460px; display: flex; flex-direction: column; align-items: center; gap: 16px; text-align: center; }
  .welcome h1 { font-size: clamp(2rem, 7vw, 2.9rem); letter-spacing: -0.02em; text-wrap: balance; }
  .welcome p.sub { margin: 0; color: var(--muted); }
  .welcome .panel { width: 100%; background: var(--surface); border: 1px solid var(--line); border-radius: 18px; padding: 18px; display: flex; flex-direction: column; gap: 14px; text-align: left; }
  .welcome .fine { font-size: 0.82rem; color: var(--muted); }
  .linkbtn { background: none; border: 0; color: var(--accent); text-decoration: underline; padding: 6px; }

  @media (max-width: 860px) {
    .app { grid-template-columns: minmax(0, 1fr); border-radius: 14px; }
    .topbar { display: flex; }
    .side { position: fixed; z-index: 50; top: 0; bottom: 0; left: 0; width: min(86vw, 320px); transform: translateX(-102%); transition: transform 0.2s ease; padding-top: calc(14px + env(safe-area-inset-top, 0px)); padding-bottom: calc(14px + env(safe-area-inset-bottom, 0px)); padding-left: max(14px, env(safe-area-inset-left, 0px)); }
    .side.open { transform: none; }
    .scrim.show { display: block; position: fixed; inset: 0; z-index: 40; background: rgba(5, 8, 20, 0.55); }
    .swatches { grid-template-columns: repeat(2, 1fr); }
  }
  @media (prefers-reduced-motion: reduce) {
    .orb, .think i, .skeleton { animation: none; }
    .side { transition: none; }
  }
</style>
</head>
<body>


<div class="app" id="app">
  <aside class="side" id="side" aria-label="Navigation">
    <div class="side-top">
      <span class="orb sm" aria-hidden="true"></span>
      <span class="name" id="side-name">Vektrin</span>
      <button class="icon-btn" id="side-close" type="button" aria-label="Close menu" data-icon="x"></button>
    </div>
    <button class="btn primary" id="new-chat" type="button"><span data-icon="plus"></span>New chat</button>
    <nav class="nav" id="nav" aria-label="Spaces"></nav>
    <input class="search" id="chat-q" type="search" placeholder="Search your chats" aria-label="Search your chats">
    <div class="chat-list" id="chat-list"></div>
    <div class="side-foot">
      <button class="profile" id="open-settings" type="button"><span class="avatar" id="avatar">L</span><span class="who" id="who">You</span><span data-icon="gear"></span></button>
    </div>
  </aside>

  <main class="main">
    <div class="topbar">
      <button class="icon-btn" id="menu-btn" type="button" aria-label="Open menu" data-icon="menu"></button>
      <span class="ttl" id="top-title">Chat</span>
      <button class="icon-btn" id="top-new" type="button" aria-label="New chat" data-icon="plus"></button>
    </div>

    <section class="view" id="v-chat">
      <div class="chat-head">
        <input class="title-in" id="chat-title" type="text" aria-label="Chat title" maxlength="80">
        <select id="chat-project" aria-label="Project for this chat"></select>
        <div class="seg" role="group" aria-label="Mode">
          <button type="button" id="mode-talk" aria-pressed="true">Talk</button>
          <button type="button" id="mode-code" aria-pressed="false"><span data-icon="code"></span>Code</button>
        </div>
        <button class="icon-btn" id="chat-del" type="button" aria-label="Delete this chat" data-icon="trash"></button>
      </div>
            <div class="msgs" id="msgs" aria-live="polite"></div>
      <div class="composer">
        <div class="chips" id="pending"></div>
        <div class="cbox">
          <textarea id="input" rows="1" placeholder="Message Vektrin" aria-label="Message"></textarea>
          <div class="cbar">
            <div class="l">
              <button class="icon-btn" id="btn-file" type="button" aria-label="Attach photos or text files" data-icon="clip"></button>
              <button class="icon-btn" id="btn-cam" type="button" aria-label="Take a photo" data-icon="camera"></button>
              <button class="icon-btn" id="btn-mic" type="button" aria-label="Speak your message" aria-pressed="false" data-icon="mic" hidden></button>
              <button class="icon-btn" id="btn-voice" type="button" aria-label="Voice chat: send when you stop talking and read replies aloud" aria-pressed="false" data-icon="speaker" hidden></button>
            </div>
            <div class="r">
              <button class="send" id="btn-send" type="button" aria-label="Send message" data-icon="send"></button>
            </div>
          </div>
        </div>
        <input type="file" id="file-in" hidden multiple>
        <input type="file" id="cam-in" hidden accept="image/*" capture="environment">
        <input type="file" id="restore-in" hidden accept="application/json,.json">
        <div class="hintline" id="hintline">Enter to send, Shift+Enter for a new line</div>
      </div>
    </section>

    <section class="view" id="v-projects" hidden>
      <div class="scroll">
        <div class="pagehead">
          <div><h2>Projects</h2><p>A project holds standing instructions. Vektrin follows simple ones, such as "keep answers short".</p></div>
          <button class="btn primary" id="proj-new" type="button"><span data-icon="plus"></span>New project</button>
        </div>
        <div class="grid" id="proj-grid"></div>
      </div>
    </section>

    <section class="view" id="v-library" hidden>
      <div class="scroll">
        <div class="pagehead">
          <div><h2>Library</h2><p>Replies, code and artwork you saved. Reuse any of it in a new chat.</p></div>
          <button class="btn" id="lib-note" type="button"><span data-icon="plus"></span>Add note</button>
        </div>
        <div class="toolbar">
          <input class="search" id="lib-q" type="search" placeholder="Search the library" aria-label="Search the library">
          <div class="filters" id="lib-filters" role="group" aria-label="Filter by type"></div>
        </div>
        <div class="grid" id="lib-grid"></div>
      </div>
    </section>

    <section class="view" id="v-create" hidden>
      <div class="scroll">
        <div class="pagehead">
          <div><h2>Create</h2><p>Describe a picture and the built-in generator draws it as vector art you can keep as an SVG file.</p></div>
        </div>
        <div class="form">
          <div class="field"><label for="art-prompt">What should it show?</label><textarea id="art-prompt" placeholder="A lighthouse on a cliff at dusk, waves below, first stars"></textarea></div>
          <div class="row-inline">
            <div class="field"><label for="art-style">Style</label>
              <select id="art-style"><option>Flat and bold</option><option>Neon glow</option><option>Soft watercolor</option><option>Line art</option><option>Paper cut layers</option><option>Isometric</option></select></div>
            <div class="field"><label for="art-ratio">Shape</label>
              <select id="art-ratio"><option value="sq">Square</option><option value="wide">Wide</option><option value="tall">Tall</option></select></div>
            <button class="btn primary" id="art-go" type="button" style="align-self:flex-end"><span data-icon="spark"></span>Create art</button>
            <button class="btn" id="art-stop" type="button" style="align-self:flex-end" hidden>Stop</button>
          </div>
          <p class="note">Words like sea, night, city, pyramids, forest, flowers, moon or boat change the scene, and each press makes a new variation. Photo and video generators are not part of this page.</p>
        </div>
        <div class="grid" id="art-grid"></div>
      </div>
    </section>

    <section class="view" id="v-memory" hidden>
      <div class="scroll">
        <div class="pagehead">
          <div><h2>Memory</h2><p>Facts Vektrin keeps in mind in every chat. Say "remember that ..." or just tell it things like "my favourite colour is blue".</p></div>
        </div>
        <div class="form">
          <label class="switch"><input type="checkbox" id="mem-on"><span>Use memory in new replies</span></label>
          <div class="field"><label for="mem-new">Add something to remember</label>
            <div class="row-inline"><input type="text" id="mem-new" placeholder="I am learning Python and prefer short explanations" style="flex:1;min-width:200px;background:var(--surface);border:1px solid var(--line);border-radius:10px;padding:10px 12px"><button class="btn primary" id="mem-add" type="button">Remember</button></div></div>
          <div class="note" id="mem-usage"></div>
          <div class="meter" aria-hidden="true"><i id="mem-meter" style="width:0%"></i></div>
        </div>
        <div class="mem-list" id="mem-list"></div>
        <div><h3 style="font-family:var(--display);font-size:1.15rem">Things you taught me</h3><p class="note" style="margin:4px 0 0">In a chat, type "teach: your question = the answer" and Vektrin will reply with it from then on.</p></div>
        <div class="mem-list" id="taught-list"></div>
        <div><button class="btn danger sm" id="mem-clear" type="button" data-confirm="Tap again: this also deletes all chats">Forget everything and delete all chats</button></div>
      </div>
    </section>
  </main>
</div>
<div class="scrim" id="scrim"></div>

<div class="modal" id="modal" hidden>
  <div class="sheet" id="sheet" role="dialog" aria-modal="true" aria-labelledby="modal-title">
    <header><h3 id="modal-title">Title</h3><button class="icon-btn" id="modal-x" type="button" aria-label="Close" data-icon="x"></button></header>
    <div class="body" id="modal-body"></div>
    <footer id="modal-foot"></footer>
  </div>
</div>

<div class="welcome" id="welcome" hidden>
  <div class="inner">
    <span class="orb xl" aria-hidden="true"></span>
    <h1>Meet your AI workspace</h1>
    <p class="sub">Chat, write code, create art and keep it all organised in one place.</p>
    <div class="panel">
      <div class="field"><label for="w-name">What should we call you?</label><input type="text" id="w-name" maxlength="30" placeholder="Your first name" autocomplete="off"></div>
      <div class="field"><label for="w-nick">Give your assistant a nickname</label><input type="text" id="w-nick" maxlength="24" placeholder="Vektrin" autocomplete="off"><span class="help">Try Sage, Pixel, Captain or anything you like. You can change it later.</span></div>
      <div class="field"><label>Pick a look</label><div class="swatches" id="w-themes"></div></div>
      <button class="btn primary" id="w-go" type="button">Start chatting</button>
      <button class="linkbtn" id="w-guest" type="button">Continue as a guest</button>
    </div>
    <p class="fine">No password and no email. Replies come from a brain built into this page, which works offline. When you are online it can also look things up on Wikipedia. Your chats stay in this browser. Created by Mizo.</p>
  </div>
</div>
<div class="toast" id="toast" role="status" aria-live="polite" hidden></div>

<script>
(function () {
  'use strict';
  var KEY = 'lumo.v1';
  var $ = function (s, r) { return (r || document).querySelector(s); };
  var uid = function () { return Math.random().toString(36).slice(2, 9) + Date.now().toString(36).slice(-4); };
  var noop = function () {};

  /* ---------- icons ---------- */
  var ICON = {
    chat: 'M21 12a8 8 0 0 1-11.6 7.1L4 20l1-4.6A8 8 0 1 1 21 12z',
    folder: 'M3 7a2 2 0 0 1 2-2h4l2 2h8a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z',
    book: 'M5 4h13a1 1 0 0 1 1 1v14H6a2 2 0 0 0-2 2V5a1 1 0 0 1 1-1zM6 19h13',
    spark: 'M12 3l1.8 5.2L19 10l-5.2 1.8L12 17l-1.8-5.2L5 10l5.2-1.8zM18 16l.7 2 2 .7-2 .7-.7 2-.7-2-2-.7 2-.7z',
    brain: 'M9 4a3 3 0 0 0-3 3v1a3 3 0 0 0-2 5 3 3 0 0 0 2 4 3 3 0 0 0 6 1V5a2 2 0 0 0-3-1zM15 4a3 3 0 0 1 3 3v1a3 3 0 0 1 2 5 3 3 0 0 1-2 4 3 3 0 0 1-6 1',
    plus: 'M12 5v14M5 12h14',
    mic: 'M12 3a3 3 0 0 0-3 3v6a3 3 0 0 0 6 0V6a3 3 0 0 0-3-3zM6 11a6 6 0 0 0 12 0M12 17v4',
    clip: 'M20 11l-8.5 8.5a5 5 0 0 1-7-7L13 4a3.5 3.5 0 0 1 5 5l-8.5 8.5a2 2 0 0 1-3-3L14 7',
    camera: 'M4 8h3l2-3h6l2 3h3v11H4zM12 17a4 4 0 1 0 0-8 4 4 0 0 0 0 8z',
    send: 'M5 12h14M13 6l6 6-6 6',
    stop: 'M7 7h10v10H7z',
    copy: 'M9 9h11v11H9zM5 15V5h10',
    speaker: 'M4 10v4h4l5 4V6L8 10zM16 9a4 4 0 0 1 0 6M18.5 6.5a8 8 0 0 1 0 11',
    bookmark: 'M6 3h12v18l-6-4-6 4z',
    trash: 'M5 7h14M10 7V4h4v3M7 7l1 13h8l1-13',
    menu: 'M4 6h16M4 12h16M4 18h16',
    x: 'M6 6l12 12M18 6L6 18',
    gear: 'M4 7h10M18 7h2M4 17h2M10 17h10M14 5v4M8 15v4',
    code: 'M8 8l-4 4 4 4M16 8l4 4-4 4M14 5l-4 14',
    play: 'M8 5l11 7-11 7z',
    file: 'M6 3h8l4 4v14H6zM14 3v4h4',
    pin: 'M12 17v5M8 3h8l-1 6 3 4H6l3-4z'
  };
  function svgIcon(n, size) {
    var s = size || 18;
    return '<svg viewBox="0 0 24 24" width="' + s + '" height="' + s + '" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="' + ICON[n] + '"/></svg>';
  }
  function paintIcons(root) {
    (root || document).querySelectorAll('[data-icon]').forEach(function (el) {
      if (el.dataset.painted) return;
      el.insertAdjacentHTML('afterbegin', svgIcon(el.dataset.icon));
      el.dataset.painted = '1';
    });
  }

  function h(tag, attrs, kids) {
    var e = document.createElement(tag);
    if (attrs) Object.keys(attrs).forEach(function (k) {
      var v = attrs[k];
      if (k === 'class') e.className = v;
      else if (k === 'text') e.textContent = v;
      else if (k === 'html') e.innerHTML = v;
      else if (k.slice(0, 2) === 'on') e.addEventListener(k.slice(2), v);
      else if (v === true) e.setAttribute(k, '');
      else if (v !== false && v != null) e.setAttribute(k, v);
    });
    (kids || []).forEach(function (c) { if (c != null) e.appendChild(typeof c === 'string' ? document.createTextNode(c) : c); });
    return e;
  }
  function esc(s) { return String(s).replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;'); }

  /* ---------- state ---------- */
  function fresh() { return { profile: null, last: null, chats: [], projects: [], library: [], memory: [], taught: [], active: null }; }
  function load() {
    try {
      var j = JSON.parse(localStorage.getItem(KEY));
      if (j && j.profile && j.profile.nick === 'Lumo') j.profile.nick = 'Vektrin';
      if (j && typeof j === 'object') { var f = fresh(); Object.keys(f).forEach(function (k) { if (!(k in j)) j[k] = f[k]; }); return j; }
    } catch (e) {}
    return fresh();
  }
  var S = load();
  var saveT = 0;
  function persistNow() { clearTimeout(saveT); try { localStorage.setItem(KEY, JSON.stringify(S)); } catch (e) {} }
  function save() {
    clearTimeout(saveT);
    saveT = setTimeout(function () {
      try { localStorage.setItem(KEY, JSON.stringify(S)); }
      catch (e) { toast('Browser storage is full. Delete old chats or library items to keep saving.'); }
    }, 200);
  }

  var THEMES = {
    daylight: { label: 'Daylight', bg: '#f2f4fb', a: '#4a55f5', b: '#a259f7' },
    midnight: { label: 'Midnight', bg: '#0b0e1a', a: '#7c8cff', b: '#c084fc' },
    aurora: { label: 'Aurora', bg: '#071318', a: '#43cff5', b: '#9d8bfa' },
    sunset: { label: 'Sunset', bg: '#170d12', a: '#ff7a59', b: '#ffb347' }
  };
  function systemTheme() { return window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches ? 'midnight' : 'daylight'; }
  function applyTheme(t) { document.documentElement.setAttribute('data-ui', THEMES[t] ? t : systemTheme()); }

  var view = 'chat';
  var busy = false, ctl = null;
  var pending = [];
  var sample = null, sampleReady = null, imgLimits = null;
  var nodes = {};
  var voiceChat = false;
  var arts = [];
  var artCtl = null;
  var libFilter = 'all';

  function cur() {
    var c = S.chats.filter(function (x) { return x.id === S.active; })[0];
    return c || null;
  }
  function nick() { return (S.profile && S.profile.nick) || 'Vektrin'; }
  function uname() { return (S.profile && S.profile.name) || 'You'; }

  var toastT = 0;
  function toast(msg) {
    var t = $('#toast');
    t.textContent = msg; t.hidden = false;
    clearTimeout(toastT);
    toastT = setTimeout(function () { t.hidden = true; }, 3200);
  }
  function arm(btn, fn) {
    if (btn._armed) { clearTimeout(btn._t); btn._armed = false; btn.classList.remove('armed'); btn.innerHTML = btn._old; fn(); return; }
    btn._armed = true; btn.classList.add('armed');
    btn._old = btn.innerHTML; btn.textContent = btn.dataset.confirm || 'Sure?';
    btn._t = setTimeout(function () { btn._armed = false; btn.classList.remove('armed'); btn.innerHTML = btn._old; }, 3000);
  }

  /* ---------- markdown-lite ---------- */
  function inl(s) { return esc(s).replace(/`([^`]+)`/g, '<code>$1</code>').replace(/\*\*([^*]+)\*\*/g, '<strong>$1</strong>').replace(/\[([^\]]+)\]\((https?:\/\/[^\s)"']+)\)/g, '<a href="$2" target="_blank" rel="noopener noreferrer">$1</a>'); }
  function blocks(t) {
    var lines = t.split('\n'), out = '', para = [], list = null;
    function fp() { if (para.length) { out += '<p>' + para.map(inl).join('<br>') + '</p>'; para = []; } }
    function fl() { if (list) { out += '</' + list + '>'; list = null; } }
    lines.forEach(function (ln) {
      var m;
      if (!ln.trim()) { fp(); fl(); return; }
      if ((m = ln.match(/^\s*[-*]\s+(.*)/))) { fp(); if (list !== 'ul') { fl(); out += '<ul>'; list = 'ul'; } out += '<li>' + inl(m[1]) + '</li>'; return; }
      if ((m = ln.match(/^\s*\d+[.)]\s+(.*)/))) { fp(); if (list !== 'ol') { fl(); out += '<ol>'; list = 'ol'; } out += '<li>' + inl(m[1]) + '</li>'; return; }
      if ((m = ln.match(/^#{1,4}\s+(.*)/))) { fp(); fl(); out += '<h4>' + inl(m[1]) + '</h4>'; return; }
      fl(); para.push(ln);
    });
    fp(); fl();
    return out;
  }
  function codeBlock(lang, code) {
    var run = /^(html|svg)$/.test(lang) ? '<button class="btn sm" type="button" data-act="cb-run">' + svgIcon('play', 14) + 'Preview</button>' : '';
    return '<div class="cb"><div class="cbh"><span>' + esc(lang || 'code') + '</span><span class="row">' + run +
      '<button class="btn sm" type="button" data-act="cb-copy">' + svgIcon('copy', 14) + 'Copy</button>' +
      '<button class="btn sm" type="button" data-act="cb-save">' + svgIcon('bookmark', 14) + 'Save</button></span></div><pre><code>' + esc(code) + '</code></pre></div>';
  }
  function md(src) {
    var parts = String(src || '').split('```'), out = '';
    parts.forEach(function (p, i) {
      if (i % 2 === 1) {
        var nl = p.indexOf('\n');
        var lang = (nl > -1 ? p.slice(0, nl) : '').trim().toLowerCase();
        var code = nl > -1 ? p.slice(nl + 1) : p;
        out += codeBlock(lang, code.replace(/\n$/, ''));
      } else out += blocks(p);
    });
    return out;
  }

  /* ---------- built-in brain: no internet, no AI model ---------- */
  var Brain = (function () {
    function pick(a) { return a[Math.floor(Math.random() * a.length)]; }
    function fence(lang, code) { return '```' + lang + '\n' + code + '\n```'; }
    function cap(s) { return s.charAt(0).toUpperCase() + s.slice(1); }
    function has(re, s) { return re.test(s); }

    /* ----- knowledge ----- */
    var KB = [
      { id: 'variable', k: 'variables?', t: 'Variables',
        a: "A **variable** is a labelled box that holds a value so you can use it later.\n\n" + fence('js', "let score = 10;\nscore = score + 5;      // now 15\nconst player = \"Sam\";  // const cannot be changed") + "\nPick clear names like `score` instead of `x`, and change a variable whenever something in your program changes.",
        more: "Try it: make a variable called `lives`, subtract 1 each time something goes wrong, and print it." },
      { id: 'loop', k: 'loops?|for loops?|while loops?|iteration', t: 'Loops',
        a: "A **loop** repeats a step so you do not have to write it again and again.\n\n" + fence('js', "for (let i = 1; i <= 5; i++) {\n  console.log(\"Round \" + i);\n}") + "\nA `for` loop counts. A `while` loop keeps going as long as a condition stays true.",
        more: "Loops are why one short program can draw 1,000 shapes. The Code Canvas style art is just a loop that draws one shape with slightly different numbers each time." },
      { id: 'function', k: 'functions?|methods?', t: 'Functions',
        a: "A **function** is a named recipe. You write the steps once and run them whenever you like.\n\n" + fence('js', "function greet(name) {\n  return \"Hello, \" + name + \"!\";\n}\nconsole.log(greet(\"Layla\"));  // Hello, Layla!") + "\nThe value inside the brackets is a **parameter**. `return` sends the answer back.",
        more: "Good functions do one job and have a name that says which job. If you cannot name it, it probably does too much." },
      { id: 'array', k: 'arrays?|lists?', t: 'Arrays',
        a: "An **array** is an ordered list of values. The first item is at position 0.\n\n" + fence('js', "const fruits = [\"apple\", \"mango\", \"fig\"];\nconsole.log(fruits[0]);     // apple\nfruits.push(\"kiwi\");       // add to the end\nconsole.log(fruits.length); // 4"),
        more: "Use a loop to visit every item: `fruits.forEach(function (f) { console.log(f); })`." },
      { id: 'object', k: 'objects?|dictionar(?:y|ies)|json', t: 'Objects',
        a: "An **object** groups related facts under names, like a form with labelled fields.\n\n" + fence('js', "const pet = { name: \"Milo\", age: 3, likesFish: true };\nconsole.log(pet.name);  // Milo\npet.age = 4;") + "\nJSON is the same idea written as text, so programs can send data to each other.",
        more: "In Python the same thing is called a dictionary: `pet = {\"name\": \"Milo\", \"age\": 3}`." },
      { id: 'condition', k: 'conditions?|conditionals?|if statements?|if else|if/else', t: 'Conditions',
        a: "A **condition** lets a program choose. It runs one block only when something is true.\n\n" + fence('js', "if (score >= 50) {\n  console.log(\"You passed\");\n} else {\n  console.log(\"Try again\");\n}"),
        more: "Combine tests with `&&` (and) and `||` (or): `if (age >= 13 && hasPermission)`." },
      { id: 'recursion', k: 'recursion|recursive', t: 'Recursion',
        a: "**Recursion** is when a function calls itself on a smaller version of the problem, until it reaches a stopping case.\n\n" + fence('python', "def factorial(n):\n    if n <= 1:        # stopping case\n        return 1\n    return n * factorial(n - 1)\n\nprint(factorial(5))  # 120"),
        more: "Forget the stopping case and the function calls itself forever until the computer gives up. That error is called a stack overflow." },
      { id: 'algorithm', k: 'algorithms?', t: 'Algorithms',
        a: "An **algorithm** is a clear list of steps that solves a problem. A recipe is an algorithm for food.\n\nExample, finding the biggest number in a list:\n\n1. Start by saying the first number is the biggest.\n2. Look at each other number.\n3. If it is bigger, it becomes the new biggest.\n4. When the list ends, you have the answer.",
        more: "Computer scientists compare algorithms by how the work grows as the list gets longer. That idea is called Big O." },
      { id: 'bug', k: 'bugs?|debug|debugging|errors?', t: 'Debugging',
        a: "A **bug** is a mistake in a program. **Debugging** is finding and fixing it.\n\n1. Read the error message. It usually names the line.\n2. Check the line above it, as the real mistake is often there.\n3. Print values with `console.log` to see what is really happening.\n4. Shrink the problem: remove code until the bug disappears, then add it back.\n5. Explain the code out loud to a friend or a toy. It works surprisingly often.",
        more: "The word bug is older than computers, but the first famous computer bug was a real moth found in a Harvard computer in 1947." },
      { id: 'html', k: 'html', t: 'HTML',
        a: "**HTML** is the structure of a web page. Tags mark what each piece is.\n\n" + fence('html', "<h1>My page</h1>\n<p>Hello <strong>world</strong>.</p>\n<a href=\"https://example.com\">A link</a>") + "\nHTML says what things are, CSS says how they look, and JavaScript says what they do.",
        more: "Ask me for a template, for example a to-do app or a clock, in Code mode, and you will get a full HTML file." },
      { id: 'css', k: 'css|stylesheets?', t: 'CSS',
        a: "**CSS** controls how HTML looks: colours, fonts, spacing and layout.\n\n" + fence('css', "h1 {\n  color: royalblue;\n  font-size: 2rem;\n}\n.card {\n  padding: 16px;\n  border-radius: 12px;\n}") + "\nA rule has a selector (what to style) and declarations (how to style it).",
        more: "Try `display: flex; gap: 12px;` on a container to put its children in a neat row." },
      { id: 'flex', k: 'flexbox|flex', t: 'Flexbox',
        a: "**Flexbox** lines items up in a row or column and spreads the space between them.\n\n" + fence('css', ".row {\n  display: flex;\n  gap: 12px;\n  justify-content: space-between;\n  align-items: center;\n  flex-wrap: wrap;\n}"),
        more: "`flex-wrap: wrap` lets items drop to a new line on narrow phone screens." },
      { id: 'javascript', k: 'javascript|js', t: 'JavaScript',
        a: "**JavaScript** is the language that makes web pages do things: react to clicks, animate, save data and talk to servers.\n\n" + fence('js', "document.querySelector(\"button\").onclick = function () {\n  alert(\"You clicked!\");\n};") + "\nEvery browser runs it, so you can start with just a text file and a browser.",
        more: "Open any page, press F12, and use the Console tab to try JavaScript live." },
      { id: 'python', k: 'python', t: 'Python',
        a: "**Python** is a beginner friendly language known for readable code. It is used for data, automation, science and AI.\n\n" + fence('python', "for n in range(1, 4):\n    print(\"Hello\", n)"),
        more: "Indentation matters in Python. The spaces at the start of a line show which block the line belongs to." },
      { id: 'api', k: 'apis?', t: 'APIs',
        a: "An **API** is a menu a program offers to other programs. You send a request, it sends back data.\n\nA weather app does not measure the weather itself. It asks a weather API, gets numbers, and shows them nicely.",
        more: "Most web APIs answer in JSON, which looks like a JavaScript object." },
      { id: 'git', k: 'git|github|version control', t: 'Git',
        a: "**Git** saves the history of your project, so you can go back to any earlier version and work on ideas safely. **GitHub** is a website that stores Git projects online.\n\n" + fence('bash', "git init\ngit add .\ngit commit -m \"first version\""),
        more: "A branch is a safe copy where you try something new. If it works, you merge it back." },
      { id: 'database', k: 'databases?|sql', t: 'Databases',
        a: "A **database** stores organised data so you can find, add and change it quickly. Think of a very powerful spreadsheet.\n\n" + fence('sql', "SELECT name FROM students WHERE score > 90;"),
        more: "SQL is the language most databases understand. The query above means: give me the names of students who scored over 90." },
      { id: 'ai', k: 'ai|artificial intelligence', t: 'Artificial intelligence',
        a: "**Artificial intelligence** is software that does things that usually need human thinking, such as recognising pictures or writing text. **Machine learning** is the part where a program learns patterns from lots of examples instead of following hand-written rules.\n\nAbout me: I am a small rule-based brain living inside this page. I match your words against built-in topics and templates. I am not a large AI model. My built-in knowledge works with no internet, and when you are online I can also look things up.",
        more: "Hand-written rules are the oldest kind of AI. Chatbots like me started that way in the 1960s." },
      { id: 'internet', k: 'internet|the web|wifi|website', t: 'The internet',
        a: "The **internet** is millions of computers connected so they can send small packets of data to each other. The **web** is the pages and apps that travel over it.\n\nWhen you open a page, your browser asks a server for HTML, CSS and JavaScript, then builds the page you see.",
        more: "Every device has an address called an IP address, a bit like a house number for data." },
      { id: 'binary', k: 'binary|bits?|bytes?', t: 'Binary',
        a: "Computers use **binary**: only 0 and 1. One digit is a **bit**, and 8 bits make a **byte**.\n\nCounting in binary: 0, 1, 10, 11, 100, 101. So `101` means 4 + 0 + 1 = 5.\n\nEverything, from photos to music, is stored as long lists of bits.",
        more: "Try me: ask \"what is 1010 in decimal\" and I will think about it. Hint: 8 + 0 + 2 = 10." },
      { id: 'boolean', k: 'booleans?', t: 'Booleans',
        a: "A **boolean** is a value that is either `true` or `false`. Conditions produce booleans, and loops and ifs use them to decide what to do.\n\n" + fence('js', "const isRaining = true;\nconst needsUmbrella = isRaining && !isIndoors;"),
        more: "They are named after George Boole, a mathematician who studied logic in the 1800s." },
      { id: 'string', k: 'strings?', t: 'Strings',
        a: "A **string** is text in a program, written inside quotes.\n\n" + fence('js', "const word = \"Cairo\";\nconsole.log(word.length);        // 5\nconsole.log(word.toUpperCase()); // CAIRO\nconsole.log(word + \" rocks\");   // Cairo rocks"),
        more: "Join strings with `+`. In Python you can also write f\"Hello {name}\"." },
      { id: 'class', k: 'classes|oop|object oriented|object-oriented', t: 'Classes and OOP',
        a: "A **class** is a blueprint for making objects that share the same shape and abilities.\n\n" + fence('js', "class Dog {\n  constructor(name) { this.name = name; }\n  bark() { return this.name + \" says woof\"; }\n}\nconst d = new Dog(\"Rex\");\nconsole.log(d.bark());") + "\nObject oriented programming means organising code around such objects.",
        more: "Inheritance lets one class build on another: a Puppy class can reuse everything in Dog." },
      { id: 'closure', k: 'closures?', t: 'Closures',
        a: "A **closure** is a function that remembers the variables around it, even after the outer function has finished.\n\n" + fence('js', "function makeCounter() {\n  let count = 0;\n  return function () {\n    count = count + 1;\n    return count;\n  };\n}\nconst next = makeCounter();\nconsole.log(next()); // 1\nconsole.log(next()); // 2") + "\n`count` is private. Only the inner function can change it.",
        more: "Closures are how event handlers remember data, and how you make private variables in JavaScript." },
      { id: 'cpu', k: 'cpu|processor|computer hardware|ram', t: 'The CPU',
        a: "The **CPU** is the computer's brain. It follows instructions, billions of tiny steps every second.\n\n**RAM** is its desk: fast, short term memory. **Storage** is the filing cabinet: slower, but it keeps things when the power is off.",
        more: "Clock speed, such as 3 GHz, counts how many billions of cycles the CPU does each second." },
      { id: 'compiler', k: 'compilers?|interpreters?|compile', t: 'Compilers',
        a: "A **compiler** translates your whole program into machine code before it runs (C and Rust work this way). An **interpreter** reads and runs your code line by line (Python and JavaScript usually do).",
        more: "Many modern languages mix both ideas to run fast." },
      { id: 'opensource', k: 'open source|open-source', t: 'Open source',
        a: "**Open source** software publishes its code so anyone can read it, use it and suggest improvements. Linux, Firefox and Python are open source. It is a great way to learn, because you can study how real projects are built.",
        more: "A good first contribution is fixing a typo in the documentation." },
      { id: 'hourofcode', k: 'hour of code', t: 'Hour of Code',
        a: "**Hour of Code** is a worldwide movement that introduces millions of students to computer science with one hour activities. It runs every year around Computer Science Education Week in December, and anyone can join with no experience.\n\nA good entry shows one clear idea, works on the first click, and explains itself. Ask me for a Code mode template to start from.",
        more: "Judges like projects that are finished and simple over ones that are big and broken." },
      { id: 'blackhole', k: 'black holes?', t: 'Black holes',
        a: "A **black hole** is a place where gravity is so strong that nothing, not even light, can escape once it crosses the edge, called the **event horizon**.\n\nMost form when a very massive star runs out of fuel and collapses. Imagine squeezing the whole Sun into a ball about 6 km wide.\n\nIt is not a hole you can fall into like a drain. It is a very dense object, and you only feel its strange effects when you get close.",
        more: "Time runs slower near a black hole, and the first ever image of one was published in 2019." },
      { id: 'gravity', k: 'gravity', t: 'Gravity',
        a: "**Gravity** is the pull between things with mass. The bigger the object, the stronger the pull. Earth pulls you down, the Moon pulls the oceans (tides), and the Sun holds the planets in orbit.\n\nOn Earth things fall faster by about 9.8 metres per second every second.",
        more: "Astronauts float because they are falling around Earth constantly, not because there is no gravity up there." },
      { id: 'photosynthesis', k: 'photosynthesis', t: 'Photosynthesis',
        a: "**Photosynthesis** is how plants make food from light.\n\n**Water + carbon dioxide + sunlight** become **sugar + oxygen**.\n\nLeaves hold a green pigment called chlorophyll that captures the light. The oxygen we breathe is the plant's leftover.",
        more: "Most of the oxygen in the air comes from plants and tiny ocean algae." },
      { id: 'watercycle', k: 'water cycle|rain|evaporation', t: 'The water cycle',
        a: "The **water cycle** moves the same water around forever.\n\n1. **Evaporation**: the Sun warms water and it rises as vapour.\n2. **Condensation**: high in the cold air, the vapour forms clouds.\n3. **Precipitation**: water falls as rain or snow.\n4. **Collection**: it flows into rivers and seas and starts again.",
        more: "The water you drink today has been around since the dinosaurs." },
      { id: 'solarsystem', k: 'solar system|planets?', t: 'The solar system',
        a: "The **solar system** is the Sun and everything that orbits it. In order from the Sun: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune.\n\nThe four inner planets are rocky. Jupiter and Saturn are gas giants. Uranus and Neptune are ice giants.",
        more: "Jupiter is so big that all the other planets could fit inside it." },
      { id: 'moon', k: 'the moon|moon', t: 'The Moon',
        a: "The **Moon** is Earth's only natural satellite, about a quarter of Earth's width. It reflects sunlight, and its phases come from how much of its sunlit half we can see as it orbits us every 29.5 days.\n\nIts gravity creates the ocean tides.",
        more: "The Moon moves about 3.8 cm further from Earth each year." },
      { id: 'dna', k: 'dna|genes?|genetics', t: 'DNA',
        a: "**DNA** is the instruction book inside almost every cell. It is a long twisted ladder (a double helix) written with four letters: A, T, C and G.\n\nA **gene** is a short section that holds the instructions for one part of how your body works.",
        more: "If you uncoiled the DNA in one human cell, it would be about two metres long." },
      { id: 'volcano', k: 'volcanos?|volcanoes|volcanic|lava', t: 'Volcanoes',
        a: "A **volcano** is an opening in Earth's crust where hot melted rock, called **magma**, can escape. Underground it is magma. Once it reaches the surface it is called **lava**.\n\nPressure from gas builds up under the ground, and when it is released the volcano erupts.",
        more: "Volcanic ash makes soil very fertile, which is why people farm near volcanoes." },
      { id: 'pyramids', k: 'pyramids?|giza|pharaohs?', t: 'The pyramids of Giza',
        a: "The **Great Pyramid of Giza** was built about 4,500 years ago as a tomb for the pharaoh Khufu. It stood about 146 metres tall and was the tallest human made structure for thousands of years.\n\nIt is made of over two million stone blocks, and it is the only one of the Seven Wonders of the Ancient World still standing.",
        more: "It once had a smooth white limestone coating that shone in the sun." },
      { id: 'nile', k: 'nile|the nile', t: 'The Nile',
        a: "The **Nile** is one of the world's longest rivers, about 6,650 km long, and it flows north through Egypt to the Mediterranean Sea.\n\nIts yearly floods left rich black soil, which made farming possible in the desert and helped ancient Egyptian civilisation grow.",
        more: "Almost all Egyptians live within a short distance of the Nile." },
      { id: 'sorting', k: 'sorting|sort algorithms?|bubble sort|merge sort', t: 'Sorting',
        a: "**Sorting** puts items in order. **Bubble sort** compares neighbours and swaps them if they are in the wrong order, again and again, until nothing needs swapping. It is simple but slow for big lists. **Merge sort** splits the list in half, sorts each half, then merges them, which is much faster.",
        more: "Ask me for \"bubble sort\" in Code mode and I will write it out." },
      { id: 'framework', k: 'frameworks?|libraries|library|react', t: 'Libraries and frameworks',
        a: "A **library** is code someone else wrote that you call when you need it. A **framework** is a bigger structure that decides how your whole project is organised. React is a library for building user interfaces.\n\nBeginners should learn plain HTML, CSS and JavaScript first. Frameworks then make sense.",
        more: "A one file project with no libraries, like the page you are using, loads instantly and works offline." },
      { id: 'scope', k: 'scope|local variables?|global variables?', strict: true, t: 'Scope',
        a: "**Scope** is where a variable can be seen. A variable made inside a function is **local**: only that function can use it. One made outside is **global**: everything can use it, which makes bugs harder to find.\n\n" + fence('js', "let total = 0;            // global\nfunction add(n) {\n  let doubled = n * 2;    // local to add()\n  total = total + doubled;\n}") + "\nIn JavaScript, `let` and `const` live only inside the nearest `{ }` block.",
        more: "Prefer local variables. Pass values in as parameters instead of reaching for globals." },
      { id: 'dom', k: 'dom|document object model', t: 'The DOM',
        a: "The **DOM** (Document Object Model) is the page turned into a tree of objects. JavaScript can find any element in that tree and change its text, style or children.\n\n" + fence('js', "const title = document.querySelector(\"h1\");\ntitle.textContent = \"New title\";\ntitle.style.color = \"tomato\";"),
        more: "Every tag in your HTML becomes a node in the DOM, and nested tags become children." },
      { id: 'event', k: 'events?|event listeners?|addeventlistener', strict: true, t: 'Events',
        a: "An **event** is something that happens on the page, such as a click, a key press or a finished download. You tell the browser which function to run when it happens.\n\n" + fence('js', "button.addEventListener(\"click\", function () {\n  console.log(\"Clicked!\");\n});"),
        more: "Other common events: `input` (typing), `submit` (forms), `keydown`, `mouseover` and `load`." },
      { id: 'async', k: 'promises?|async|await|callbacks?|asynchronous', t: 'Async code',
        a: "Some jobs take time, like loading data. **Asynchronous** code lets the page keep working while it waits.\n\nA **promise** is a placeholder for a result that will arrive later. `async` and `await` let you write it in a readable top to bottom way.\n\n" + fence('js', "async function load() {\n  const reply = await fetch(\"/data.json\");\n  const data = await reply.json();\n  console.log(data);\n}"),
        more: "A callback is a function you hand over to be called when the work finishes. Promises were invented to avoid deeply nested callbacks." },
      { id: 'regex', k: 'regex|regular expressions?', t: 'Regular expressions',
        a: "A **regular expression** (regex) is a pattern for finding text.\n\n" + fence('js', "/\\d+/.test(\"room 42\");        // true: contains digits\n\"a1b22\".match(/\\d+/g);        // [\"1\", \"22\"]\n\"hi there\".replace(/\\s+/g, \"-\"); // \"hi-there\"") + "\n`\\d` is a digit, `\\s` is a space, `+` means one or more.",
        more: "Build and test patterns slowly. A tiny regex is easy to read, a huge one is not." },
      { id: 'bigo', k: 'big o|time complexity|algorithm speed', t: 'Big O',
        a: "**Big O** describes how the work of an algorithm grows as the input grows.\n\n- **O(1)**: the same work every time, like reading item 5 of a list.\n- **O(n)**: work grows with the list, like checking every item once.\n- **O(n squared)**: a loop inside a loop. Ten times more items means about a hundred times more work.\n- **O(log n)**: halving each step, like guessing a number with higher or lower hints.",
        more: "Binary search is O(log n). Searching a million sorted items takes about 20 steps." },
      { id: 'hashtable', k: 'hash tables?|hash maps?|hashmaps?', t: 'Hash tables',
        a: "A **hash table** stores values under keys and finds them almost instantly, without searching. JavaScript objects and `Map`, and Python dictionaries, are hash tables.\n\nA hash function turns the key into a position in memory, like a coat check giving you a ticket number.",
        more: "Two different keys can land on the same spot. That is called a collision and the table has a plan to handle it." },
      { id: 'stackqueue', k: 'stacks?|queues?|lifo|fifo', strict: true, t: 'Stacks and queues',
        a: "A **stack** is last in, first out, like a pile of plates. The undo button uses one. A **queue** is first in, first out, like a line at a shop. Print jobs use one.\n\n" + fence('js', "const stack = [];\nstack.push(1); stack.push(2);\nstack.pop();     // 2  (last in)\n\nconst queue = [];\nqueue.push(1); queue.push(2);\nqueue.shift();   // 1  (first in)"),
        more: "When a function calls another function, the computer keeps track using a call stack." },
      { id: 'linkedlist', k: 'linked lists?', t: 'Linked lists',
        a: "A **linked list** is a chain of nodes. Each node holds a value and points to the next node. Adding in the middle is quick, but you cannot jump to item 50 directly. You have to walk the chain.",
        more: "Arrays are the opposite: instant access by position, slower insertion in the middle." },
      { id: 'http', k: 'https?|status codes?', t: 'HTTP',
        a: "**HTTP** is the set of rules browsers and servers use to talk. A browser sends a request, the server sends a response with a **status code**: 200 means OK, 404 means not found, 500 means the server had an error. **HTTPS** is HTTP with encryption, so nobody in between can read it.",
        more: "The padlock in the address bar means the connection uses HTTPS." },
      { id: 'os', k: 'operating systems?|windows|macos|linux|android', strict: true, t: 'Operating systems',
        a: "An **operating system** manages the hardware and lets apps run: Windows, macOS, Linux, Android and iOS are examples. It handles files, memory, the screen and the network so each app does not have to.",
        more: "Linux is open source and runs most of the world's servers, and Android is built on it." },
      { id: 'cloud', k: 'cloud computing|the cloud', t: 'The cloud',
        a: "**The cloud** just means using computers owned by someone else, over the internet, instead of your own. Your photos, documents and many apps live on those servers, so you can reach them from any device.",
        more: "Cloud companies rent out computing power by the minute, so a small project can start without buying a server." },
      { id: 'pseudocode', k: 'pseudocode|pseudo code', t: 'Pseudocode',
        a: "**Pseudocode** is writing your plan in plain language before writing real code.\n\n" + fence('text', "set highest to the first number\nfor each other number in the list\n  if it is bigger than highest\n    set highest to it\nshow highest") + "\nIf the plan makes sense, translating it into any language is easy.",
        more: "Many competition problems become much easier once you write the steps in pseudocode first." },
      { id: 'grid', k: 'css grid|grid layout', t: 'CSS Grid',
        a: "**CSS Grid** lays items out in rows and columns at the same time.\n\n" + fence('css', ".gallery {\n  display: grid;\n  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));\n  gap: 16px;\n}") + "\nThis makes as many 200px columns as fit and stretches them evenly, which works on phones too.",
        more: "Use Flexbox for one direction (a row or a column) and Grid when you need both." },
      { id: 'robot', k: 'robots?|robotics', strict: true, t: 'Robots',
        a: "A **robot** is a machine that follows a loop: **sense** (sensors collect information), **think** (a controller decides) and **act** (motors move). A line following robot senses the line, decides which way to turn and spins the wheels.",
        more: "Programming a simple robot is mostly the loops and conditions you already know." },
      { id: 'pixel', k: 'pixels?|resolution', t: 'Pixels',
        a: "A **pixel** is one tiny dot of colour. A screen is a grid of them, and each has a red, green and blue amount. A 1920 by 1080 screen has about 2 million pixels. **Resolution** is how many pixels there are across and down.",
        more: "Canvas drawing in a web page is just choosing which pixels to colour, many times a second." },
      { id: 'encryption', k: 'encryption|encrypt|cryptography', t: 'Encryption',
        a: "**Encryption** scrambles data with a secret key so only someone with the right key can read it. It protects messages, passwords and payments. When a site uses HTTPS, your browser and the server encrypt the traffic between them.",
        more: "Strong encryption relies on maths that is easy in one direction and extremely hard to reverse without the key." },
      { id: 'phishing', k: 'phishing|scam emails?|scam messages?', t: 'Phishing',
        a: "**Phishing** is a fake message that tricks you into giving away a password or money.\n\nSigns: urgency (\"act now\"), a strange sender address, a link that does not match the real site, and requests for passwords.\n\nDo not click. Open the real website yourself, and tell an adult or the service if you are unsure.",
        more: "No real company will ask for your password in a message." },
      { id: 'password', k: 'password safety|strong passwords?|two factor|2fa|passwords?', t: 'Password safety',
        a: "A strong password is **long** (12 or more characters), **unique** for each account, and not based on your name or birthday. A password manager can remember them for you.\n\nTurn on **two-factor authentication** (a code from your phone) so a stolen password alone is not enough.",
        more: "Four or five random words together make a password that is strong and easier to remember." },
      { id: 'atom', k: 'atoms?|molecules?|protons?|electrons?|neutrons?', t: 'Atoms',
        a: "An **atom** is the tiny building block of everything. It has a nucleus of **protons** (positive) and **neutrons** (no charge), with **electrons** (negative) moving around it. The number of protons decides which element it is: 1 is hydrogen, 6 is carbon, 8 is oxygen.\n\nA **molecule** is atoms joined together, for example water is two hydrogen atoms and one oxygen atom.",
        more: "Atoms are mostly empty space. If the nucleus were a marble, the electrons would be hundreds of metres away." },
      { id: 'electricity', k: 'electricity|voltage|ohm|circuits?|current', strict: true, t: 'Electricity',
        a: "**Electricity** is the flow of tiny charged particles called electrons through a wire. **Voltage** is the push, **current** is how much flows, and **resistance** is how much the material resists it.\n\nOhm's law connects them: **V = I x R**.",
        more: "A circuit must be a complete loop for current to flow. A switch simply breaks the loop." },
      { id: 'sound', k: 'sound|sound waves?|echo', strict: true, t: 'Sound',
        a: "**Sound** is vibration travelling through air (or water or solids) as waves. In air at 20 degrees Celsius it moves at about 343 metres per second. It cannot travel through empty space, so there is no sound in space.\n\nAn **echo** is sound bouncing back from a surface. High pitch means fast vibrations.",
        more: "Lightning and thunder start together, but you hear thunder later. Count the seconds and divide by 3 for the distance in kilometres." },
      { id: 'lightspeed', k: 'speed of light|light speed', t: 'The speed of light',
        a: "Light travels at about **299,792 kilometres per second**, roughly 300,000. Nothing goes faster. Sunlight takes around 8 minutes and 20 seconds to reach Earth.\n\nA **light year** is the distance light travels in a year, about 9.46 trillion kilometres. It measures distance, not time.",
        more: "The nearest star after the Sun, Proxima Centauri, is about 4.2 light years away." },
      { id: 'evolution', k: 'evolution|natural selection|darwin', t: 'Evolution',
        a: "**Evolution** is how living things change over many generations. Individuals differ, and those with traits that help them survive and have young pass those traits on more often. This is **natural selection**, described by Charles Darwin in 1859.\n\nOver long times, small changes add up to new species.",
        more: "Whales, bats and humans all share the same basic arm bones, which is evidence of shared ancestry." },
      { id: 'foodchain', k: 'food chains?|food webs?|ecosystems?', t: 'Food chains',
        a: "A **food chain** shows who eats whom, and where energy goes: **producers** (plants make food from sunlight), **consumers** (animals that eat plants or other animals), and **decomposers** (fungi and bacteria that break down what is left).\n\nExample: grass, grasshopper, frog, snake, eagle. An **ecosystem** is all the living things in a place and how they depend on each other.",
        more: "Only about 10 percent of energy passes to the next level, so food chains rarely have more than five links." },
      { id: 'climate', k: 'climate change|global warming|greenhouse', t: 'Climate change',
        a: "**Climate change** is a long term shift in temperature and weather. Today's warming is mainly caused by **greenhouse gases**, such as carbon dioxide from burning coal, oil and gas, which trap heat in the atmosphere.\n\nHelping includes clean energy, saving energy, protecting forests and wasting less.",
        more: "Weather is what happens today. Climate is the pattern over 30 years or more." },
      { id: 'earthquake', k: 'earthquakes?|tectonic|seismic', t: 'Earthquakes',
        a: "Earth's outer layer is split into huge **tectonic plates** that slowly move. When stress builds up along a crack called a **fault** and suddenly releases, the ground shakes: an **earthquake**. Instruments called seismometers measure it and its size is given as a **magnitude**.",
        more: "Each whole number up in magnitude means about 32 times more energy is released." },
      { id: 'tides', k: 'tides?', t: 'Tides',
        a: "**Tides** are the rise and fall of the sea, caused mostly by the Moon's gravity pulling on the oceans, with a smaller pull from the Sun. Most coasts have two high tides and two low tides about every 24 hours and 50 minutes.",
        more: "When the Sun and Moon line up, tides are extra strong. These are called spring tides." },
      { id: 'seasons', k: 'seasons?|why is it (?:hot|cold)', strict: true, t: 'The seasons',
        a: "**Seasons** happen because Earth's axis is tilted by about 23.5 degrees. When your half of Earth tilts towards the Sun you get summer, with longer days and more direct light. When it tilts away you get winter.\n\nIt is not about distance: Earth is actually closest to the Sun in early January.",
        more: "When it is summer in Egypt it is winter in Australia." },
      { id: 'rainbow', k: 'rainbows?', t: 'Rainbows',
        a: "A **rainbow** forms when sunlight enters raindrops, bends, reflects off the back and bends again as it leaves. Each colour bends by a slightly different amount, so white light spreads into red, orange, yellow, green, blue, indigo and violet.\n\nYou see one when the Sun is behind you and rain is in front.",
        more: "Every rainbow is really a full circle. The ground just hides the lower part." },
      { id: 'eclipse', k: 'eclipses?', t: 'Eclipses',
        a: "In a **solar eclipse** the Moon passes between the Sun and Earth and blocks the sunlight. In a **lunar eclipse** Earth passes between the Sun and the Moon, so Earth's shadow falls on the Moon, turning it red.\n\nNever look at a solar eclipse without proper eclipse glasses.",
        more: "By luck, the Sun is about 400 times wider than the Moon and also about 400 times farther away, so they look the same size." },
      { id: 'sun', k: 'the sun|sun', strict: true, t: 'The Sun',
        a: "The **Sun** is a star: a giant ball of hot gas about 150 million km from Earth. It holds 99.8 percent of the mass of the solar system and is about 4.6 billion years old. Its core fuses hydrogen into helium, which releases the light and heat that make life possible.",
        more: "You could fit about 1.3 million Earths inside the Sun." },
      { id: 'milkyway', k: 'milky way|galaxy|galaxies', t: 'The Milky Way',
        a: "A **galaxy** is a huge collection of stars, gas and dust held together by gravity. Our galaxy, the **Milky Way**, is a spiral with hundreds of billions of stars. The Sun sits about 26,000 light years from the centre and circles it once every 230 million years or so.",
        more: "Scientists estimate there are hundreds of billions of galaxies in the universe." },
      { id: 'pi', k: 'pi|π', strict: true, t: 'Pi',
        a: "**Pi** is the circumference of any circle divided by its diameter, about **3.14159**. It never ends and never repeats. Pi shows up in circles, waves and many other places.\n\nCircle area = pi x r x r. Circumference = 2 x pi x r. Pi Day is March 14, because 3/14 looks like 3.14.",
        more: "Computers have calculated pi to trillions of digits. About 40 digits is enough to measure the whole universe to the width of a hydrogen atom." },
      { id: 'pythagoras', k: 'pythagoras|pythagorean|right triangles?', t: 'The Pythagorean theorem',
        a: "In a right triangle, the square of the longest side (the **hypotenuse**) equals the sum of the squares of the other two: **a squared + b squared = c squared**.\n\nExample: sides 3 and 4 give 9 + 16 = 25, so the hypotenuse is 5.",
        more: "Builders have used the 3-4-5 triangle for thousands of years to make perfect right angles." },
      { id: 'average', k: 'median|averages?|mean median mode', t: 'Mean, median and mode',
        a: "Three ways to describe a set of numbers:\n\n- **Mean** (average): add them up and divide by how many there are.\n- **Median**: the middle number when they are in order.\n- **Mode**: the number that appears most often.\n\nFor 2, 3, 3, 9: mean 4.25, median 3, mode 3.",
        more: "Say \"average of 4, 8, 15\" and I will work it out." },
      { id: 'probability', k: 'probability|chances?|odds', strict: true, t: 'Probability',
        a: "**Probability** measures how likely something is, from 0 (impossible) to 1 (certain). It equals the favourable outcomes divided by all outcomes.\n\nA coin landing heads is 1/2. Rolling a 4 on a die is 1/6. Getting two heads in a row is 1/2 x 1/2 = 1/4.",
        more: "Past flips do not change the next one. That is why \"it must be tails soon\" is a mistake called the gambler's fallacy." },
      { id: 'fibonacci', k: 'fibonacci', t: 'Fibonacci numbers',
        a: "The **Fibonacci sequence** starts 0, 1 and each number is the sum of the two before it: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34...\n\n" + fence('python', "a, b = 0, 1\nfor _ in range(10):\n    print(a)\n    a, b = b, a + b"),
        more: "Ask me for \"the first 15 fibonacci numbers\" or \"fibonacci 30\"." },
      { id: 'percent', k: 'percent(?:age)?s?|fractions?|decimals?', strict: true, t: 'Percentages and fractions',
        a: "**Percent** means per hundred. 50% = 1/2 = 0.5, and 25% = 1/4 = 0.25.\n\nTo find a percentage of a number, multiply: 20% of 80 is 0.20 x 80 = 16. To add a percentage, multiply by 1 plus the rate: a 10% increase on 50 is 50 x 1.10 = 55.",
        more: "Try \"20% of 80\", \"15% tip on 40\" or \"30% off 50\"." },
      { id: 'exponent', k: 'exponents?|powers of', t: 'Exponents',
        a: "An **exponent** says how many times to multiply a number by itself. 2 to the power of 5 is 2 x 2 x 2 x 2 x 2 = 32. Squared means power 2, cubed means power 3.\n\nIn code: `2 ** 5` in Python and JavaScript, or `Math.pow(2, 5)`.",
        more: "Any number to the power 0 is 1. Powers of 2 (2, 4, 8, 16...) are everywhere in computing." },
      { id: 'geometry', k: 'area|perimeter|geometry|circumference|volume', strict: true, t: 'Area and perimeter',
        a: "Handy formulas:\n\n- Rectangle area = width x height, perimeter = 2 x (width + height)\n- Triangle area = half x base x height\n- Circle area = pi x r x r, circumference = 2 x pi x r\n- Box volume = length x width x height",
        more: "A circle with radius 5 has area about 78.5 and circumference about 31.4." },
      { id: 'primenum', k: 'prime numbers?|primes?', t: 'Prime numbers',
        a: "A **prime number** has exactly two divisors: 1 and itself. The first primes are 2, 3, 5, 7, 11, 13, 17, 19. Two is the only even prime, and 1 is not prime.\n\nEvery whole number above 1 is either prime or a product of primes, which is why primes are called the atoms of numbers.",
        more: "Try \"is 97 prime\" or \"prime factors of 84\". Internet encryption relies on very large primes." },
      { id: 'hieroglyphs', k: 'hieroglyph(?:s|ics)?|rosetta stone', t: 'Hieroglyphs',
        a: "**Hieroglyphs** were the picture writing of ancient Egypt. Nobody could read them for about 1,400 years until the **Rosetta Stone**, found in 1799, helped. It carries the same decree written in hieroglyphs, a simpler Egyptian script and Greek, which scholars could read. Jean-Francois Champollion cracked the code in 1822.",
        more: "The Rosetta Stone was carved in 196 BC and is in the British Museum in London." },
      { id: 'tut', k: 'tutankhamun|king tut|howard carter|valley of the kings', t: 'Tutankhamun',
        a: "**Tutankhamun** became pharaoh around the age of nine, about 3,300 years ago, and died young. In 1922 Howard Carter found his tomb in the Valley of the Kings, almost untouched, with thousands of treasures including the famous gold burial mask.",
        more: "The treasures are displayed in Egyptian museums in Cairo." },
      { id: 'suez', k: 'suez canal|suez', t: 'The Suez Canal',
        a: "The **Suez Canal** in Egypt opened in 1869. It is about 193 km long and links the Mediterranean Sea to the Red Sea, so ships can avoid sailing all the way around Africa. It is one of the busiest shipping routes in the world.",
        more: "In 2021 a stuck container ship blocked it for about six days and held up global trade." },
      { id: 'sphinx', k: 'sphinx', t: 'The Great Sphinx',
        a: "The **Great Sphinx of Giza** is a limestone statue with a lion's body and a human head, about 73 metres long and 20 metres high. It is roughly 4,500 years old and is thought to show the pharaoh Khafre.",
        more: "It was carved from a single mass of bedrock and has been buried in sand several times." },
      { id: 'cairo', k: 'cairo', strict: true, t: 'Cairo',
        a: "**Cairo** is the capital of Egypt and one of the largest cities in Africa and the Arab world, with over 20 million people in its greater area. It sits on the Nile and is near the pyramids of Giza. The city was founded in 969 AD, and its Arabic name Al-Qahira means \"the victorious\".",
        more: "Cairo is nicknamed the city of a thousand minarets." },
      { id: 'alexandria', k: 'alexandria|library of alexandria|pharos', t: 'Alexandria',
        a: "**Alexandria** is a Mediterranean port in Egypt founded by Alexander the Great in 331 BC. It was home to the **Library of Alexandria**, one of the biggest libraries of the ancient world, and to the **Pharos lighthouse**, one of the Seven Wonders of the Ancient World.",
        more: "A modern library, the Bibliotheca Alexandrina, opened there in 2002." },
      { id: 'sahara', k: 'sahara|deserts?', strict: true, t: 'The Sahara',
        a: "The **Sahara** is the world's largest hot desert, about 9 million square kilometres across much of North Africa, roughly the size of the United States. Most of it is rock and gravel, with sand dunes covering a smaller part. Deserts are defined by very little rainfall, not by heat.",
        more: "Egypt's Western Desert is part of the Sahara." },
      { id: 'amazon', k: 'amazon rainforest|rainforests?|amazon', strict: true, t: 'The Amazon rainforest',
        a: "The **Amazon** is the largest rainforest on Earth, mostly in Brazil. It is home to roughly one in ten of the world's known species. The Amazon River carries more water than any other river.",
        more: "Rainforests store huge amounts of carbon, which is one reason protecting them matters for the climate." },
      { id: 'continents', k: 'continents?|oceans?', strict: true, t: 'Continents and oceans',
        a: "There are **seven continents**: Africa, Antarctica, Asia, Australia (Oceania), Europe, North America and South America. There are **five oceans**: the Pacific (the largest), Atlantic, Indian, Southern and Arctic.",
        more: "Egypt is in Africa, but the Sinai Peninsula is in Asia." },
      { id: 'leapyear', k: 'leap years?', t: 'Leap years',
        a: "Earth takes about **365.25 days** to orbit the Sun, so every four years we add an extra day, 29 February, to keep the calendar in step. Century years are skipped unless divisible by 400: 1900 was not a leap year, but 2000 was.",
        more: "Try \"what day of the week is 2028-02-29\"." },
      { id: 'timezones', k: 'time zones?|utc|gmt', t: 'Time zones',
        a: "Earth turns once every 24 hours, so when it is noon in one place it is night elsewhere. The world is split into **time zones** about 15 degrees wide. **UTC** is the standard reference time. Zones are written as an offset, such as UTC+2 or UTC-5.",
        more: "Programmers store times in UTC and convert them for display, which avoids many bugs." },
      { id: 'pomodoro', k: 'pomodoro|study tips?|how to study|focus tips?|how to focus', t: 'Studying well',
        a: "The **Pomodoro technique**: work for 25 minutes with no distractions, take a 5 minute break, and after four rounds take a longer break of 15 to 30 minutes.\n\nMore tips: put your phone in another room, test yourself instead of rereading, sleep well, and explain what you learned out loud.",
        more: "Say \"plan my study week for maths and science\" and I will build a schedule." },
      { id: 'learncode', k: 'learn to code|learn coding|start coding|start programming|how to code|learn programming|how do i start', t: 'Learning to code',
        a: "A simple path for a beginner:\n\n1. Start with **HTML and CSS**: build a page about something you love.\n2. Add **JavaScript**: make a button do something.\n3. Build tiny projects: a counter, a to-do list, a quiz.\n4. Read errors calmly. They tell you where to look.\n5. Change working code a little and see what breaks. That is how you learn.\n\nAsk me for a template in Code mode to start from.",
        more: "Twenty minutes every day beats three hours once a week." },
      { id: 'competition', k: 'coding competitions?|hackathons?|code contests?|competition tips', t: 'Coding competition tips',
        a: "Tips for a coding competition or showcase:\n\n1. **Finish a small idea** instead of half finishing a big one.\n2. Make the first click impressive and explain what to do in one line.\n3. Test on a phone and a laptop, and in light and dark mode.\n4. Handle mistakes kindly: no blank screens or crashes.\n5. Be ready to explain how it works and what you learned, in 30 seconds.\n6. Add one detail that is truly yours.",
        more: "Practise your demo out loud. Judges remember how clearly you explained it." },
      { id: 'ancientegypt', k: 'ancient egypt|ancient egyptians|mummies|mummy|mummification', t: 'Ancient Egypt',
        a: "**Ancient Egypt** was a civilisation along the Nile that lasted about 3,000 years, from around 3100 BC when upper and lower Egypt were united, until 30 BC when Egypt became part of the Roman Empire.\n\nThe Egyptians built pyramids and temples, wrote in hieroglyphs, and believed in an afterlife. Bodies of important people were preserved by **mummification**, a process of about 70 days. Gods included Ra (the sun), Osiris, Isis and Anubis.",
        more: "The yearly Nile flood gave them rich farmland, and they invented a 365 day calendar." },
      { id: 'cleopatra', k: 'cleopatra', t: 'Cleopatra',
        a: "**Cleopatra VII** (69 to 30 BC) was the last active ruler of Ptolemaic Egypt. She belonged to a Greek-Macedonian dynasty but learned Egyptian, and spoke several languages. She allied with the Roman leaders Julius Caesar and Mark Antony. After her death in 30 BC, Egypt became a Roman province.",
        more: "Alexandria was her capital city." },
      { id: 'rome', k: 'roman empire|ancient rome|romans', t: 'The Roman Empire',
        a: "**Rome** grew from a city in Italy into a vast empire. Augustus became the first emperor in 27 BC, and at its peak the empire surrounded the Mediterranean Sea. The Romans built straight roads, aqueducts and the Colosseum, and Latin became the base of languages such as Italian, Spanish and French. The western half fell in 476 AD.",
        more: "The eastern half, the Byzantine Empire, lasted until 1453." },
      { id: 'goldenage', k: 'islamic golden age|house of wisdom|al-khwarizmi|khwarizmi|ibn al-haytham|alhazen', t: 'The Islamic Golden Age',
        a: "Between roughly the 8th and 13th centuries, scholars in cities such as Baghdad and Cairo made huge advances in maths, medicine, astronomy and optics. In Baghdad, the **House of Wisdom** gathered and translated knowledge from many cultures.\n\n**Al-Khwarizmi** (about 780 to 850) wrote the book that gave us the word **algebra** (from al-jabr), and his name gave us the word **algorithm**. **Ibn al-Haytham** (about 965 to 1040), who worked in Cairo, explained how vision works and pioneered the experimental method in optics.",
        more: "Hindu-Arabic numerals (0 to 9) spread to Europe largely through scholars of this era." },
      { id: 'industrial', k: 'industrial revolution', t: 'The Industrial Revolution',
        a: "The **Industrial Revolution** began in Britain in the late 1700s. Steam engines, factories and railways replaced much hand work and moved people from farms to cities. It brought huge growth and new inventions, but also crowded cities and hard working conditions.",
        more: "It later spread to Europe, North America and then the rest of the world." },
      { id: 'printing', k: 'printing press|gutenberg', t: 'The printing press',
        a: "Around 1440, Johannes Gutenberg in Germany developed a printing press using movable metal type. Books could be made quickly and cheaply, so ideas, science and literacy spread across Europe much faster.",
        more: "The Gutenberg Bible is among the first major books printed this way." },
      { id: 'apollo', k: 'moon landing|apollo 11|neil armstrong', t: 'The Moon landing',
        a: "On 20 July 1969, **Apollo 11** landed on the Moon. **Neil Armstrong** and **Buzz Aldrin** walked on its surface while **Michael Collins** stayed in orbit. Armstrong's words as he stepped down were: \"That's one small step for man, one giant leap for mankind.\"",
        more: "They collected about 21 kilograms of Moon rock and returned safely to Earth." },
      { id: 'worldwars', k: 'world war 1|world war one|world war i|ww1|world war 2|world war two|world war ii|ww2', t: 'The World Wars',
        a: "**World War I** lasted from 1914 to 1918. **World War II** lasted from 1939 to 1945 and involved most of the world's nations. Both caused enormous loss of life. After World War II the United Nations was founded in 1945 to help nations settle disagreements peacefully.",
        more: "Many technologies, such as radar and early computers, advanced rapidly during the wars." },
      { id: 'computinghistory', k: 'history of computers|first computer|alan turing|turing|ada lovelace|charles babbage|eniac', t: 'The history of computers',
        a: "In the 1830s **Charles Babbage** designed the Analytical Engine, a mechanical computer. **Ada Lovelace** wrote notes in 1843 that contain what is often called the first computer program. In 1936 **Alan Turing** described a theoretical machine that defines what can be computed. **ENIAC** (1945) was an early electronic computer that filled a room. The transistor (1947) and later microchips made computers small enough for homes and phones.",
        more: "Alan Turing also helped break the Enigma code in World War II." },
      { id: 'webhistory', k: 'world wide web|tim berners-lee|history of the internet|arpanet', t: 'History of the web',
        a: "The internet began as **ARPANET** in 1969, linking a few university computers. In 1989 **Tim Berners-Lee**, working at CERN, proposed the **World Wide Web**: pages linked together and shared with HTML and URLs. The first website went live in 1991.",
        more: "The web and the internet are different: the internet is the network, and the web is one of the things that runs on it." },
      { id: 'periodic', k: 'periodic table|chemical elements?|elements', strict: true, t: 'The periodic table',
        a: "The **periodic table** arranges the 118 known chemical elements by their number of protons (atomic number). Dmitri Mendeleev published an early version in 1869 and used its gaps to predict elements not yet found.\n\nColumns are called groups, and elements in a group behave alike. The far right column holds the noble gases, such as helium and neon, which barely react.",
        more: "Gold is Au, from the Latin aurum, and iron is Fe, from ferrum." },
      { id: 'matter', k: 'states of matter|solids?|liquids?|gas(?:es)?|melting|boiling|freezing|plasma', strict: true, t: 'States of matter',
        a: "Matter mainly exists as a **solid** (fixed shape), a **liquid** (flows, takes the shape of its container), a **gas** (spreads to fill space) or **plasma** (a very hot, charged gas, like the Sun).\n\nHeating gives particles energy and changes the state. Water freezes at 0 degrees Celsius and boils at 100 degrees at sea level.",
        more: "Ice floats because water expands when it freezes, which is unusual among substances." },
      { id: 'acids', k: 'acids?|bases|ph scale|alkaline|litmus', strict: true, t: 'Acids and bases',
        a: "The **pH scale** runs from 0 to 14. Below 7 is **acidic** (lemon juice is about 2), 7 is neutral (pure water), and above 7 is **basic** or alkaline (baking soda is about 9, bleach about 13).\n\nLitmus paper turns red in acid and blue in base.",
        more: "Stomach acid is strong, with a pH of about 1.5 to 3.5, and helps break down food." },
      { id: 'cells', k: 'cells?|cell biology|mitochondria|nucleus', strict: true, t: 'Cells',
        a: "A **cell** is the basic unit of life. The **nucleus** holds the DNA, the **mitochondria** release energy from food, and the **membrane** controls what goes in and out. Plant cells also have a rigid cell wall and **chloroplasts** for photosynthesis.\n\nA human body has around 37 trillion cells.",
        more: "Red blood cells have no nucleus, which leaves more room to carry oxygen." },
      { id: 'heart', k: 'the heart|heart beat|heartbeat|heart rate|blood circulation|circulatory system', t: 'The heart',
        a: "The **heart** is a muscle about the size of your fist that pumps blood around the body. It has four chambers. The right side sends blood to the lungs to pick up oxygen, and the left side pumps oxygen-rich blood to the rest of the body.\n\nIt beats around 100,000 times a day.",
        more: "Your pulse is the heartbeat felt in an artery, for example at your wrist." },
      { id: 'brain', k: 'the brain|neurons?|nervous system', t: 'The brain',
        a: "The **brain** controls thinking, memory, feelings, movement and the senses. It contains roughly 86 billion nerve cells called **neurons**, which pass tiny electrical and chemical signals to each other. It uses about 20 percent of the body's energy even though it is only about 2 percent of its weight.",
        more: "Learning strengthens connections between neurons, so practice really does change your brain." },
      { id: 'vaccines', k: 'vaccines?|vaccination|immune system|antibodies', t: 'Vaccines and the immune system',
        a: "The **immune system** defends the body against germs. A **vaccine** shows it a harmless piece or weakened form of a germ, so it learns to recognise it and make **antibodies** in advance. If the real germ arrives later, the body reacts fast.\n\nVaccines helped remove smallpox from the world in 1980.",
        more: "For medical questions about you, ask a doctor or pharmacist." },
      { id: 'nutrition', k: 'nutrition|balanced diet|vitamins?|protein|carbohydrates?|healthy eating', t: 'Nutrition basics',
        a: "Your body needs **carbohydrates** (main energy), **proteins** (growth and repair), **fats** (energy and cell health), **vitamins and minerals** (many small jobs), **fibre** and **water**.\n\nA balanced diet means a variety of vegetables, fruit, whole grains, protein foods and plenty of water. For advice about your own health, ask a doctor or dietitian.",
        more: "Eating a rainbow of fruit and vegetables is an easy way to cover many vitamins." },
      { id: 'sleep', k: 'sleep|how much sleep', strict: true, t: 'Sleep',
        a: "Most teenagers need about **8 to 10 hours** of sleep a night, and most adults 7 to 9. Sleep helps the brain store memories, helps the body repair and helps you concentrate. Tips: keep regular times, avoid screens just before bed, and keep the room dark and cool.",
        more: "Studying and then sleeping helps memory more than studying late into the night." },
      { id: 'mars', k: 'mars|red planet', strict: true, t: 'Mars',
        a: "**Mars** is the fourth planet from the Sun. It looks red because of iron oxide (rust) in its soil. A day lasts about 24 hours 37 minutes. It has the tallest volcano in the solar system, **Olympus Mons**, and robot rovers such as Curiosity and Perseverance explore its surface.",
        more: "Mars has two small moons, Phobos and Deimos." },
      { id: 'gasgiants', k: 'jupiter|saturn', strict: true, t: 'Jupiter and Saturn',
        a: "**Jupiter** is the largest planet, a gas giant with a storm called the Great Red Spot that has raged for centuries. **Saturn** is famous for its bright rings, made of ice and rock. It is so light for its size that it would float in a big enough bath of water.",
        more: "Jupiter has over 90 known moons, including the four large Galilean moons." },
      { id: 'iss', k: 'iss|international space station', t: 'The International Space Station',
        a: "The **ISS** orbits Earth about 400 km up, going around every 90 minutes or so. Astronauts have lived on it continuously since November 2000. It was built by space agencies from the United States, Russia, Europe, Japan and Canada, and is used for science experiments in weightlessness.",
        more: "The crew sees about 16 sunrises and sunsets every day." },
      { id: 'telescopes', k: 'telescopes?|hubble|james webb|jwst', t: 'Space telescopes',
        a: "Telescopes collect light to see faraway things. The **Hubble Space Telescope**, launched in 1990, orbits Earth and takes sharp pictures above the blurring air. The **James Webb Space Telescope**, launched in December 2021, observes infrared light from a point about 1.5 million km from Earth, letting it see very distant, very old galaxies.",
        more: "Looking far away means looking back in time, because light takes time to arrive." },
      { id: 'comets', k: 'comets?|asteroids?|meteors?|meteorites?|shooting stars?', strict: true, t: 'Comets and asteroids',
        a: "A **comet** is a ball of ice and dust that grows a glowing tail when it nears the Sun. An **asteroid** is a rocky body, mostly found between Mars and Jupiter. A **meteor** (shooting star) is the streak of light when space rock burns up in our atmosphere, and a **meteorite** is a piece that reaches the ground.",
        more: "Halley's comet returns about every 76 years, next in 2061." },
      { id: 'bigbang', k: 'big bang|age of the universe|universe', strict: true, t: 'The Big Bang',
        a: "The **Big Bang** is the scientific theory that the universe began extremely hot and dense about **13.8 billion years ago** and has been expanding ever since. Evidence includes the way distant galaxies move away from us and a faint glow called the cosmic microwave background.",
        more: "The expansion is not into empty space. Space itself is stretching." },
      { id: 'rockets', k: 'rockets?|how do rockets work', t: 'Rockets',
        a: "A **rocket** pushes hot gas out of its engine downwards, and by Newton's third law (every action has an equal and opposite reaction) the rocket is pushed upwards. To escape Earth's gravity a spacecraft needs about **11.2 km/s**. Rockets often drop empty stages as they climb so they get lighter, which is called staging.",
        more: "Rockets work in space too, because they push against their own exhaust, not against the air." },
      { id: 'gps', k: 'gps|global positioning', t: 'GPS',
        a: "**GPS** uses a group of satellites that each broadcast their position and the exact time. Your phone listens to at least four of them, works out how long each signal took to arrive, and calculates where you are.",
        more: "Each satellite carries an atomic clock, and tiny timing errors would cause big position errors." },
      { id: 'wifi', k: 'wi-?fi|bluetooth|wireless', t: 'Wi-Fi and Bluetooth',
        a: "**Wi-Fi** sends data as radio waves between your device and a router, which is connected to the internet. **Bluetooth** uses radio waves too, but for short range links between nearby devices such as earphones and phones, using little power.",
        more: "Radio waves are invisible, but they are the same kind of wave as light, just with a longer wavelength." },
      { id: 'qr', k: 'qr codes?|barcodes?', t: 'QR codes',
        a: "A **QR code** is a square pattern that stores data such as a web address. It was invented in 1994 by Denso Wave in Japan for tracking car parts. A phone camera reads the pattern, and built-in error correction means it still works if part of it is damaged.",
        more: "Only scan codes from sources you trust, since a code can point to any website." },
      { id: '3dprint', k: '3d printing|3d printers?', t: '3D printing',
        a: "A **3D printer** builds an object layer by layer from a digital model, melting plastic, hardening liquid resin or fusing metal powder. It is used to make prototypes, tools, medical parts and even houses.",
        more: "Designs are saved as files, often in STL format, that describe the shape." },
      { id: 'blockchain', k: 'blockchain|cryptocurrency|bitcoin', t: 'Blockchain and Bitcoin',
        a: "A **blockchain** is a shared record of transactions stored in linked blocks across many computers, so no single person can quietly change the history. **Bitcoin**, created in 2009 by someone using the name Satoshi Nakamoto, was the first big use of it. Cryptocurrency prices can change very quickly, so this is not financial advice.",
        more: "Each block contains a fingerprint (hash) of the one before, which is what links them." },
      { id: 'ev', k: 'electric cars?|batteries|battery|lithium', t: 'Electric cars and batteries',
        a: "An **electric car** uses a battery and electric motors instead of a petrol engine, so it produces no exhaust from the tailpipe. Most use **lithium-ion** batteries, the same kind as in phones. The greener the electricity used to charge them, the cleaner the car.",
        more: "Electric motors can deliver full pushing power from a standstill, so these cars accelerate quickly." },
      { id: 'solar', k: 'solar panels?|solar energy|renewable energy|wind power|wind turbines?', t: 'Solar and wind power',
        a: "**Solar panels** turn sunlight directly into electricity, and **wind turbines** use moving air to spin a generator. Both are renewable because the Sun and wind do not run out, and they do not burn fuel. Egypt has the huge Benban Solar Park near Aswan, one of the largest solar sites in the world.",
        more: "Batteries and the grid help supply power when it is dark or still." },
      { id: 'everest', k: 'mount everest|everest', t: 'Mount Everest',
        a: "**Mount Everest**, on the border of Nepal and China in the Himalayas, is Earth's highest mountain above sea level at **8,849 metres**. Edmund Hillary and Tenzing Norgay made the first confirmed ascent in 1953.",
        more: "The mountain grows by a few millimetres a year as tectonic plates keep pushing." },
      { id: 'seas', k: 'red sea|mediterranean', t: 'The Red Sea and Mediterranean',
        a: "The **Red Sea** is a narrow sea about 2,250 km long between Africa and Asia, famous for coral reefs and clear water, such as near Sharm el-Sheikh and Hurghada. The **Mediterranean Sea** lies between Europe, Africa and Asia and has been a trading route for thousands of years. The Suez Canal links the two.",
        more: "Despite the name, the Red Sea is blue. The name may come from occasional red algae blooms." },
      { id: 'arabic', k: 'arabic|the arabic language', t: 'Arabic',
        a: "**Arabic** is spoken by more than 400 million people and is official in over 20 countries. It is written from right to left with 28 letters. Modern Standard Arabic is used in writing and news, while each region has its own spoken dialect. The Egyptian dialect is widely understood across the Arab world because of Egyptian films and music.",
        more: "The word algebra, and words like coffee and sugar in English, came from Arabic." },
      { id: 'chess', k: 'chess', t: 'Chess',
        a: "**Chess** is a game for two on an 8 by 8 board of 64 squares. Each player has 16 pieces: a king, queen, two rooks, two bishops, two knights and eight pawns. You win by **checkmate**, trapping the opponent's king so it cannot escape. The game developed in India around the 6th century.",
        more: "Computers beat the world champion Garry Kasparov in 1997." },
      { id: 'worldcup', k: 'world cup|football|soccer', strict: true, t: 'Football and the World Cup',
        a: "**Football** (soccer) is played by two teams of 11, and you score by getting the ball into the other team's goal. The **FIFA World Cup** has been held about every four years since 1930 and is the most watched sports event in the world.",
        more: "Egypt has qualified for the World Cup several times." },
      { id: 'olympics', k: 'olympics?|olympic games', strict: true, t: 'The Olympics',
        a: "The ancient **Olympic Games** began in Greece in 776 BC. The modern Games began in Athens in 1896 and take place every four years, with a Summer and a Winter edition. The five rings stand for the five inhabited regions the movement links.",
        more: "The Games alternate: Summer, then Winter two years later." },
      { id: 'shakespeare', k: 'shakespeare', t: 'William Shakespeare',
        a: "**William Shakespeare** (1564 to 1616) was an English playwright and poet who wrote around 37 plays, including *Romeo and Juliet*, *Hamlet*, *Macbeth* and *A Midsummer Night's Dream*. He added many words and phrases to English that we still use.",
        more: "He wrote for the Globe Theatre in London." },
      { id: 'einstein', k: 'einstein|relativity|e=mc2', t: 'Albert Einstein',
        a: "**Albert Einstein** (1879 to 1955) developed the theory of **relativity**, which changed how we understand space, time and gravity. His equation **E = mc squared** says mass and energy are two forms of the same thing. He won the 1921 Nobel Prize in Physics for explaining the photoelectric effect.",
        more: "GPS satellites must correct for relativity to stay accurate." },
      { id: 'newton', k: 'newton|laws of motion|isaac newton', t: 'Isaac Newton',
        a: "**Isaac Newton** (1643 to 1727) described gravity and three **laws of motion**:\n\n1. An object stays still or keeps moving in a straight line unless a force acts on it.\n2. Force equals mass times acceleration (F = m x a).\n3. Every action has an equal and opposite reaction.\n\nHe also helped invent calculus and studied light.",
        more: "The unit of force, the newton, is named after him." },
      { id: 'curie', k: 'marie curie|radioactivity', t: 'Marie Curie',
        a: "**Marie Curie** (1867 to 1934) was a Polish-French scientist who studied **radioactivity**, a word she introduced. She discovered the elements polonium and radium. She was the first woman to win a Nobel Prize and the first person to win Nobel Prizes in two different sciences, Physics in 1903 and Chemistry in 1911.",
        more: "Her notebooks are still radioactive today and are stored in lead-lined boxes." },
      { id: 'economy', k: 'inflation|supply and demand|economy|gdp', strict: true, t: 'Economics basics',
        a: "**Supply and demand**: when many people want something that is scarce, its price rises. When there is plenty and few buyers, the price falls.\n\n**Inflation** is a general rise in prices over time, so the same money buys less. **GDP** measures the total value of goods and services a country produces in a year.",
        more: "Saving a little regularly, even a small amount, is a habit that grows over time." },
      { id: 'speaking', k: 'public speaking|presentation tips|how to present|presenting', t: 'Presenting well',
        a: "For a clear presentation:\n\n1. Start with one sentence saying what it is and why it matters.\n2. Keep three main points, not ten.\n3. Show, do not just tell: demo it live if you can.\n4. Speak slowly and look at your audience.\n5. Practise out loud with a timer, and prepare for two likely questions.\n6. End with what you learned or what you would do next.",
        more: "Nerves are normal. Taking a slow breath before you start really helps." },
      { id: 'essay', k: 'write an essay|essay tips|how to write an essay|essay structure', t: 'Writing an essay',
        a: "A simple essay structure:\n\n1. **Introduction**: hook the reader and state your main idea (your thesis).\n2. **Body paragraphs**: one idea each, starting with a topic sentence, then evidence, then explanation.\n3. **Conclusion**: restate your thesis in new words and say why it matters.\n\nPlan first, write a rough draft, then edit for clarity and spelling.",
        more: "Reading your essay out loud is a fast way to find awkward sentences." },
      { id: 'algebra', k: 'algebra|equations?', strict: true, t: 'Algebra',
        a: "**Algebra** uses letters for unknown numbers. To solve an equation, do the same thing to both sides until the letter is alone.\n\nExample: 2x + 3 = 11. Subtract 3: 2x = 8. Divide by 2: x = 4.",
        more: "Type \"solve 3x - 5 = 10\" and I will show the steps." },
      { id: 'binsearch', k: 'binary search', t: 'Binary search',
        a: "**Binary search** finds an item in a **sorted** list by checking the middle, then throwing away the half that cannot contain it, and repeating. A list of a million items needs only about 20 checks.\n\n" + fence('js', "function find(list, target) {\n  let low = 0, high = list.length - 1;\n  while (low <= high) {\n    const mid = Math.floor((low + high) / 2);\n    if (list[mid] === target) return mid;\n    if (list[mid] < target) low = mid + 1;\n    else high = mid - 1;\n  }\n  return -1;\n}"),
        more: "It only works because the list is sorted. Sorting first costs extra." },
      { id: 'ux', k: 'user interface|ux|ui design|user experience', strict: true, t: 'UI and UX design',
        a: "**UI** (user interface) is how an app looks. **UX** (user experience) is how it feels to use. Good design: one clear main action per screen, readable text with good contrast, consistent spacing and buttons, big tap targets on phones, and helpful messages when something goes wrong.\n\nTest with a real person and watch where they hesitate.",
        more: "If you have to explain a button, it probably needs a clearer label." },
      { id: 'a11y', k: 'accessibility|a11y|screen readers?|alt text', t: 'Accessibility',
        a: "**Accessibility** means everyone can use your page, including people who are blind, deaf, colour blind or who cannot use a mouse. Basics: write `alt` text for images, keep text contrast strong, label every form field, make everything usable with the keyboard, and never rely on colour alone to give meaning.",
        more: "Many accessibility improvements also make the page better for everyone." },
      { id: 'responsive', k: 'responsive design|media queries|media query|mobile friendly', t: 'Responsive design',
        a: "**Responsive design** makes a page work on any screen size. Add the viewport tag, use flexible layouts (Flexbox or Grid), and use media queries to adjust at small widths.\n\n" + fence('html', "<meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">\n<style>\n  .cards { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }\n  @media (max-width: 600px) {\n    .cards { grid-template-columns: 1fr; }\n  }\n</style>"),
        more: "Test with your browser's device mode (F12) to see the phone view." },
      { id: 'canvas', k: 'canvas|html canvas|canvas api', t: 'The HTML canvas',
        a: "The **canvas** is a drawing surface you control with JavaScript, used for art, charts and games.\n\n" + fence('html', "<canvas id=\"c\" width=\"300\" height=\"150\"></canvas>\n<script>\n  const ctx = document.getElementById(\"c\").getContext(\"2d\");\n  ctx.fillStyle = \"tomato\";\n  ctx.fillRect(20, 20, 100, 60);\n  ctx.beginPath();\n  ctx.arc(200, 75, 40, 0, Math.PI * 2);\n  ctx.stroke();\n<\/script>"),
        more: "Animation is a loop that clears the canvas and draws again 60 times a second with requestAnimationFrame." },
      { id: 'storage', k: 'localstorage|local storage|cookies?|browser storage', t: 'Browser storage',
        a: "Browsers can save small amounts of data for a site. **localStorage** keeps text under keys until you clear it. **Cookies** are small values sent with web requests, often used for login sessions.\n\n" + fence('js', "localStorage.setItem(\"score\", \"42\");\nconst score = localStorage.getItem(\"score\");   // \"42\"\nlocalStorage.removeItem(\"score\");"),
        more: "localStorage holds only text, so use JSON.stringify and JSON.parse for lists and objects." },
      { id: 'carengine', k: 'car engines?|combustion engine|how (?:do )?(?:cars?|engines?) work|four stroke|4 stroke|pistons?|cylinders?', t: 'How a car engine works',
        a: "A petrol or diesel car has an **internal combustion engine**. Fuel and air burn inside **cylinders**, the explosion pushes a **piston** down, and a crankshaft turns that up and down motion into spinning motion that drives the wheels.\n\nMost engines use a **four stroke cycle**: **intake** (air and fuel come in), **compression** (the piston squeezes them), **power** (a spark lights them) and **exhaust** (burnt gas leaves).",
        more: "Cars have 3 to 12 cylinders. Diesel engines have no spark plugs: the squeezed air gets so hot that the fuel lights by itself." },
      { id: 'horsepower', k: 'horsepower|hp|torque|engine power', t: 'Horsepower and torque',
        a: "**Horsepower** measures how fast an engine can do work. **Torque** measures twisting force, the strength that gets the car moving. One horsepower is about 746 watts.\n\nA simple way to remember it: torque is how hard you can push, horsepower is how fast you can keep pushing. Torque makes a car feel quick off the line, horsepower sets how fast it can go.",
        more: "Torque is measured in newton metres (Nm) or pound-feet. Electric motors give full torque instantly, which is why electric cars feel so fast at the start." },
      { id: 'transmission', k: 'transmission|gearbox|manual (?:car|gearbox|transmission)|automatic (?:car|gearbox|transmission)|gears? in (?:a )?car|clutch', t: 'Gearbox and transmission',
        a: "The **transmission** passes engine power to the wheels and lets the engine run at a good speed. **Low gears** give strong pulling force for starting and hills. **High gears** let the car cruise fast while the engine stays calm.\n\nIn a **manual** car the driver presses the **clutch** and moves the gear lever. An **automatic** chooses gears by itself. A **CVT** has no fixed gears and changes smoothly.",
        more: "Most electric cars have just one gear, because electric motors work well over a very wide range of speeds." },
      { id: 'carbrakes', k: 'brakes?|braking|abs|disc brakes?|brake pads?', t: 'Car brakes',
        a: "Most cars use **disc brakes**. When you press the pedal, fluid pressure squeezes **brake pads** against a spinning **disc** on each wheel. Friction turns the car's movement into heat and the car slows down.\n\n**ABS** (anti-lock braking system) stops the wheels from locking in a hard stop, so you can still steer. Hybrid and electric cars also use **regenerative braking**, which turns movement back into electricity.",
        more: "Brake fluid absorbs water over time, so it should be changed every couple of years." },
      { id: 'tyres', k: 'tyres?|tires?|tread|tyre pressure|tire pressure', t: 'Tyres',
        a: "**Tyres** are the only part of the car that touches the road, so they decide grip, braking and comfort. The **tread** channels water away. Many countries require at least **1.6 mm** of tread, but replacing them near 3 mm is wiser.\n\nCorrect **pressure**, usually 30 to 35 psi (about 2.1 to 2.4 bar), saves fuel and keeps the car safe. Check it when the tyres are cold.",
        more: "Winter tyres use softer rubber that stays grippy below about 7 degrees Celsius." },
      { id: 'drivetrain', k: 'fwd|rwd|awd|4wd|four wheel drive|all wheel drive|front wheel drive|rear wheel drive|drivetrain', t: 'Drive types',
        a: "The drive type says which wheels get power.\n\n- **FWD** (front wheel drive): common, cheap and efficient.\n- **RWD** (rear wheel drive): balanced handling, loved in sports cars.\n- **AWD** (all wheel drive): all four wheels always get power, good on wet or snowy roads.\n- **4WD**: a rugged system for off-road, often switchable.",
        more: "In a FWD car the front wheels both steer and pull, which is why heavy cornering makes the front slide first." },
      { id: 'turbo', k: 'turbo|turbocharger|supercharger|turbocharged', t: 'Turbochargers',
        a: "A **turbocharger** uses the hot exhaust gas to spin a small turbine, which pushes extra air into the engine. More air lets the engine burn more fuel, so a small engine can make the power of a bigger one.\n\nA **supercharger** does the same job but is driven by a belt from the engine, so it has no lag.",
        more: "The short delay before a turbo gives its boost is called turbo lag." },
      { id: 'dieselpetrol', k: 'diesel|petrol|gasoline|diesel vs petrol|petrol vs diesel|fuel types?|octane', t: 'Petrol and diesel',
        a: "**Petrol** (gasoline) engines use spark plugs and rev smoothly. **Diesel** engines squeeze air until it is hot enough to light the fuel. Diesel holds more energy per litre, so diesel cars often use less fuel on long trips, but they make more soot and nitrogen oxides.\n\n**Octane rating** shows how well petrol resists knocking. Use the number your car's handbook asks for.",
        more: "Higher octane fuel does not give extra power to an engine that is not built for it." },
      { id: 'hybrid', k: 'hybrids?|hybrid cars?|plug in hybrid|phev|regenerative braking', t: 'Hybrid cars',
        a: "A **hybrid** car has a petrol engine plus an electric motor and a small battery. The motor helps at low speed and when accelerating, and braking recharges the battery, so fuel use drops a lot in town.\n\nA **plug-in hybrid** (PHEV) has a bigger battery you can charge from a socket and drive tens of kilometres on electricity alone. The Toyota Prius from 1997 was the first mass produced hybrid.",
        more: "A full electric car has no engine at all and needs no petrol." },
      { id: 'carhistory', k: 'first car|who invented the car|history of cars?|karl benz|model t|henry ford|invention of the car', t: 'History of the car',
        a: "In **1886** the German engineer **Karl Benz** patented the Benz Patent-Motorwagen, usually called the first practical petrol car. It had three wheels.\n\nIn **1908** Henry Ford launched the **Model T**, and in 1913 his moving assembly line made cars cheap enough for ordinary families. Before that, cars were rare and expensive.",
        more: "In 1888 Bertha Benz, Karl's wife, made the first long car trip, about 100 km, and bought fuel at a pharmacy." },
      { id: 'carbrands', k: 'car brands?|car companies|car makers?', t: 'Car makers',
        a: "Some famous car makers and what they are known for:\n\n- **Toyota** (Japan): the world's biggest seller, famous for reliability and the Prius.\n- **Ford** (USA): the Model T and the assembly line.\n- **Mercedes-Benz and BMW** (Germany): luxury and engineering.\n- **Ferrari and Lamborghini** (Italy): supercars.\n- **Bugatti** (France): extreme hypercars such as the Veyron and Chiron.\n- **Tesla** (USA): leading electric cars.",
        more: "Volkswagen Group and Toyota are the two largest car companies by cars sold." },
      { id: 'fastestcars', k: 'fastest cars?|fastest car in the world|top speed (?:of )?(?:a )?car|supercars?|hypercars?|bugatti chiron|speed record', t: 'Fastest cars',
        a: "A few records: the **Bugatti Chiron Super Sport 300+** went over **490 km/h (304 mph)** in a 2019 test run. The **SSC Tuatara** and the **Koenigsegg Jesko** are other cars aiming for about 480 km/h or more.\n\nThe land speed record for any wheeled vehicle is held by the jet car **ThrustSSC**, which went **1,228 km/h (763 mph)** in 1997, faster than sound.",
        more: "Records of this kind need two runs in opposite directions on a very long, flat road so wind does not help." },
      { id: 'mpg', k: 'fuel economy|fuel efficiency|mpg|km per litre|litres per 100|l/100 ?km|miles per gallon|save fuel', t: 'Fuel economy',
        a: "**Fuel economy** shows how far a car goes on fuel. It is written as **miles per gallon (mpg)** or **litres per 100 km** (lower is better). To convert, **235.2 divided by the mpg gives the litres per 100 km** for US gallons.\n\nTo save fuel: drive smoothly, keep tyres at the right pressure, remove roof racks, avoid hard acceleration and do not carry extra weight.",
        more: "Air drag grows with the square of speed, so driving at 120 km/h uses much more fuel than at 90 km/h." },
      { id: 'carmaintenance', k: 'car maintenance|oil change|engine oil|service (?:my |a )?car|car battery|check engine light|coolant', t: 'Looking after a car',
        a: "Basic care keeps a car safe and cheap to run:\n\n- **Engine oil**: change it about every 10,000 km, or as the handbook says. Oil lubricates and cools the moving parts.\n- **Coolant**: stops the engine overheating or freezing.\n- **Tyres and brakes**: check pressure, tread and brake pads.\n- **Battery**: lasts about 3 to 5 years.\n- **Warning lights**: a red light means stop safely, an orange **check engine** light means get it checked soon.",
        more: "Many cars need a full service every year or every 10,000 to 15,000 km." },
      { id: 'carsafety', k: 'seat ?belts?|airbags?|car safety|crumple zones?|ncap', t: 'Car safety',
        a: "Modern cars protect you in layers. A **seat belt** holds you in place and cuts the risk of death in a crash by about half. **Airbags** cushion your head and chest. **Crumple zones** at the front and back are built to fold and soak up the crash energy so less reaches the passengers.\n\nNewer systems add **automatic emergency braking**, lane warnings and blind spot alerts.",
        more: "Safety ratings like Euro NCAP test cars by crashing them and award up to five stars." },
      { id: 'selfdriving', k: 'self driving|self-driving|driverless|autonomous (?:cars?|vehicles?|driving)|autopilot', t: 'Self-driving cars',
        a: "A **self-driving car** uses cameras, radar, lidar (laser scanning) and sensors, plus software that recognises roads, signs, people and other cars, and decides how to steer and brake.\n\nThe SAE scale has levels from **0** (no automation) to **5** (the car drives itself everywhere). Most cars sold today are around level 2, where the driver must still watch the road.",
        more: "Lidar builds a 3D map around the car by measuring how long laser pulses take to bounce back." },
      { id: 'formula1', k: 'formula 1|formula one|f1|grand prix', t: 'Formula 1',
        a: "**Formula 1** is the top level of single-seat car racing. Teams build their own cars, and drivers race at **Grands Prix** around the world. A season has about 24 races, and points decide the **Drivers' and Constructors' championships**.\n\nModern F1 cars use hybrid engines of about 1000 horsepower, and produce so much **downforce** from their wings that, in theory, they could drive upside down at speed.",
        more: "A pit stop to change four tyres takes about two seconds." },

      { id: 'calculus', k: 'calculus|derivatives?|differentiation|integrals?|integration|limits? in maths?|rate of change', t: 'Calculus',
        a: "**Calculus** studies change. The **derivative** gives the rate of change, such as speed from position, or the slope of a curve at one point. The **integral** adds up tiny pieces, such as the area under a curve.\n\nBasic rule: the derivative of **x^n** is **n x^(n-1)**. So the derivative of x^3 is 3x^2, and the derivative of a constant is 0.",
        more: "Newton and Leibniz both developed calculus in the late 1600s. The two ideas are linked by the fundamental theorem of calculus: integrating undoes differentiating." },
      { id: 'trig', k: 'trigonometry|sine|cosine|tangent|sin cos tan|sohcahtoa|trig', t: 'Trigonometry',
        a: "**Trigonometry** links the angles of a triangle to its sides. In a right-angled triangle:\n\n- **sin = opposite / hypotenuse**\n- **cos = adjacent / hypotenuse**\n- **tan = opposite / adjacent**\n\nRemember it with **SOH CAH TOA**. For example, sin 30 degrees is 0.5, cos 60 degrees is 0.5 and tan 45 degrees is 1.",
        more: "A full circle is 360 degrees, which equals 2 pi radians." },
      { id: 'quadratic', k: 'quadratic formula|quadratic equations?|discriminant|parabola', t: 'The quadratic formula',
        a: "For **ax^2 + bx + c = 0**, the solutions are:\n\n**x = (-b +/- sqrt(b^2 - 4ac)) / 2a**\n\nThe part under the square root, **b^2 - 4ac**, is the **discriminant**. If it is positive there are two real answers, if zero there is one, and if negative there are no real answers.\n\nExample: x^2 - 5x + 6 = 0 gives x = 2 or x = 3. Try asking me to solve one.",
        more: "The graph of a quadratic is a U-shaped curve called a parabola." },
      { id: 'areavolume', k: 'area formulas?|volume formulas?|area of (?:a |an )?(?:circle|triangle|rectangle|square|trapezium|trapezoid)|volume of (?:a |an )?(?:sphere|cube|cylinder|cone|pyramid)|surface area|perimeter|circumference', t: 'Area and volume formulas',
        a: "**Area (2D)**\n\n- Rectangle: width x height\n- Triangle: half x base x height\n- Circle: pi x r^2\n- Trapezium: half x (a + b) x height\n\n**Circumference of a circle**: 2 x pi x r\n\n**Volume (3D)**\n\n- Cube: side^3\n- Cylinder: pi x r^2 x height\n- Sphere: 4/3 x pi x r^3\n- Cone: 1/3 x pi x r^2 x height",
        more: "Here r is the radius, which is half the diameter. Pi is about 3.14159." },
      { id: 'statistics', k: 'statistics|median|the mode|standard deviation|range of (?:a )?data|mean median mode', t: 'Statistics',
        a: "Three ways to describe the middle of data:\n\n- **Mean**: add all values and divide by how many there are.\n- **Median**: the middle value when sorted.\n- **Mode**: the value that appears most often.\n\nThe **range** is largest minus smallest. **Standard deviation** shows how spread out the values are: small means they cluster near the mean.\n\nExample: 2, 3, 3, 5, 9 has mean 4.4, median 3, mode 3 and range 7.",
        more: "The median is better than the mean when there are extreme values, such as a few very high salaries." },
      { id: 'logarithm', k: 'logarithms?|natural log|log base', t: 'Logarithms',
        a: "A **logarithm** answers: what power do I raise the base to, to get this number? So **log10(1000) = 3** because 10^3 = 1000, and **log2(8) = 3** because 2^3 = 8.\n\nUseful rules: log(a x b) = log a + log b, and log(a / b) = log a - log b. The **natural logarithm (ln)** uses the base **e**, about 2.71828.",
        more: "Logarithms are used for earthquake (Richter), sound (decibel) and acidity (pH) scales." },
      { id: 'orderops', k: 'order of operations|bodmas|pemdas|bidmas', t: 'Order of operations',
        a: "Do a calculation in this order: **Brackets**, then **Orders** (powers and roots), then **Division and Multiplication** from left to right, then **Addition and Subtraction** from left to right. People remember it as **BODMAS** or **PEMDAS**.\n\nExample: 2 + 3 x 4 = 14, not 20, because the multiplication comes first.",
        more: "Computers follow the same rules, which is why programming languages let you use brackets to be clear." },
      { id: 'fractions', k: 'fractions?|ratios?|proportion|decimals?|simplify (?:a )?fraction|numerator|denominator', t: 'Fractions and ratios',
        a: "A **fraction** has a **numerator** (top) and **denominator** (bottom). To simplify, divide both by their greatest common factor, so 6/8 becomes 3/4.\n\n- **Add**: use a common denominator: 1/3 + 1/4 = 4/12 + 3/12 = 7/12.\n- **Multiply**: top x top over bottom x bottom.\n- **Divide**: flip the second fraction and multiply.\n\nA **ratio** compares amounts: 3:1 means three parts to one part.",
        more: "To turn a fraction into a decimal, divide the top by the bottom. 3/4 = 0.75." },
      { id: 'sequences', k: 'sequences?|arithmetic sequence|geometric sequence|series (?:in )?maths?|nth term', t: 'Sequences',
        a: "A **sequence** is a list of numbers that follows a rule.\n\n- **Arithmetic**: add the same amount each time: 3, 7, 11, 15. The nth term is **a + (n - 1)d**.\n- **Geometric**: multiply by the same amount each time: 2, 6, 18, 54. The nth term is **a x r^(n-1)**.\n\nThe sum of the numbers 1 to n is **n(n + 1)/2**, a trick the young Gauss used to add 1 to 100 (5050).",
        more: "The Fibonacci sequence adds the previous two terms each time: 1, 1, 2, 3, 5, 8." },
      { id: 'imaginary', k: 'imaginary numbers?|complex numbers?|square root of -?1', t: 'Imaginary and complex numbers',
        a: "The number **i** is defined by **i^2 = -1**. It is called imaginary because no ordinary number multiplied by itself gives a negative result. A **complex number** has two parts, like **3 + 4i**.\n\nThey are not just a curiosity: engineers use them for electricity, signals and waves, and quantum physics needs them.",
        more: "The beautiful Euler identity connects them: e^(i x pi) + 1 = 0." },
      { id: 'goldenratio', k: 'golden ratio|golden number|phi|euler.?s number|infinity|zero (?:number|history)', t: 'Famous numbers',
        a: "- **Pi** (about 3.14159): circumference divided by diameter. It never ends or repeats.\n- **e** (about 2.71828): the base of natural growth and the natural logarithm.\n- **Golden ratio phi** (about 1.618): two parts are in this ratio when the whole divided by the larger part equals the larger divided by the smaller. It is linked to the Fibonacci numbers.\n- **Infinity** is not a number but the idea of something without end. Some infinities are bigger than others.\n- **Zero** as a number came from ancient India and spread through the Arab world.",
        more: "The word algebra comes from al-jabr in a book by the Persian scholar Al-Khwarizmi." },
      { id: 'romanum', k: 'roman numerals?|number systems?|hexadecimal|octal|base 10|decimal system', t: 'Number systems',
        a: "We count in **base 10** (decimal) with ten digits. Computers use **base 2** (binary: 0 and 1), and programmers often write **base 16** (hexadecimal: 0 to 9 and A to F), because one hex digit equals four bits.\n\n**Roman numerals**: I = 1, V = 5, X = 10, L = 50, C = 100, D = 500, M = 1000. A smaller symbol before a larger one is subtracted, so IV = 4 and IX = 9. 2026 is MMXXVI.",
        more: "Ask me for a number in binary or hex, like 255 in hex, and I will convert it." },

      { id: 'starlife', k: 'life cycle of (?:a )?stars?|how (?:do )?stars (?:form|die|work)|star formation|red giant|white dwarf|nebula|the life of a star', t: 'How stars live and die',
        a: "A star is born inside a cloud of gas and dust called a **nebula**. Gravity pulls the gas together until it is hot enough for **nuclear fusion**: hydrogen joins to make helium and releases huge energy.\n\nA star like our Sun shines for about 10 billion years, then swells into a **red giant** and shrinks into a **white dwarf**. A much heavier star ends in a **supernova** explosion and leaves a **neutron star** or a **black hole**.",
        more: "The Sun is about 4.6 billion years old, so it is roughly in the middle of its life." },
      { id: 'lightyear', k: 'light years?|how far (?:are|is) the stars?|astronomical unit|au distance|parsec', t: 'Light-years',
        a: "A **light-year** is the distance light travels in one year, about **9.46 trillion km** (9.46 x 10^12 km). It measures distance, not time.\n\nThe nearest star after the Sun, **Proxima Centauri**, is about **4.25 light-years** away. In our solar system we use the **astronomical unit (AU)**, the Earth to Sun distance of about 150 million km.",
        more: "When you look at a star you see its light from the past, so you see it as it was years ago." },
      { id: 'supernova', k: 'supernova|neutron stars?|pulsars?|magnetar', t: 'Supernovae and neutron stars',
        a: "A **supernova** is the explosion of a massive star, or of a white dwarf that gets extra matter. For a short time it can outshine a whole galaxy, and it spreads elements such as iron, oxygen and calcium into space. Much of what you are made of was created in stars and supernovae.\n\nWhat is left in the middle can be a **neutron star**: about 1.4 times the Sun's mass squeezed into a ball only 20 km wide. A fast spinning one that sends beams of radio waves is a **pulsar**.",
        more: "A teaspoon of neutron star material would weigh around a billion tonnes." },
      { id: 'exoplanets', k: 'exoplanets?|planets? around other stars|habitable zone|goldilocks zone|kepler telescope|alien planets?', t: 'Exoplanets',
        a: "An **exoplanet** is a planet that orbits a star other than our Sun. The first around a Sun-like star was found in **1995**, and astronomers have now confirmed **more than 5,500**.\n\nThe **habitable zone** (or Goldilocks zone) is the distance from a star where it is not too hot and not too cold for liquid water. Telescopes like Kepler and James Webb find planets by seeing a star dim slightly when a planet passes in front of it.",
        more: "TRAPPIST-1 is a small star about 40 light-years away with seven Earth-sized planets." },
      { id: 'venus', k: 'venus|mercury (?:planet)?|inner planets?|terrestrial planets?', t: 'Venus and Mercury',
        a: "**Venus** is the hottest planet, about **465 degrees Celsius**, even hotter than Mercury, because its thick carbon dioxide atmosphere traps heat. It spins backwards and a day on Venus is longer than its year.\n\n**Mercury** is the smallest planet and the closest to the Sun. It has almost no atmosphere, so temperatures swing from about 430 degrees in daylight to minus 180 at night. A year there lasts only 88 Earth days.",
        more: "Venus is the brightest planet in our night sky and is often called the evening or morning star." },
      { id: 'icegiants', k: 'uranus|neptune|ice giants?|outer planets?', t: 'Uranus and Neptune',
        a: "**Uranus** and **Neptune** are the **ice giants**. They are made mostly of water, ammonia and methane ices around a rocky core, and their atmospheres look blue-green and blue because methane absorbs red light.\n\nUranus is tilted on its side and takes 84 years to orbit the Sun. Neptune is the farthest planet, about 30 AU away, with the fastest winds in the solar system, over 2,000 km/h. Voyager 2 is the only spacecraft to have visited them.",
        more: "Neptune was found in 1846 by using maths to predict where it must be, from Uranus's odd orbit." },
      { id: 'pluto', k: 'pluto|dwarf planets?|kuiper belt|asteroid belt|asteroids?|ceres', t: 'Dwarf planets and asteroids',
        a: "In **2006** the International Astronomical Union decided that a planet must orbit the Sun, be round, and have cleared its orbit of other objects. **Pluto** fails the last test, so it became a **dwarf planet**.\n\nThe **asteroid belt** between Mars and Jupiter holds millions of rocky leftovers, and its biggest member, **Ceres**, is also a dwarf planet. The **Kuiper belt** past Neptune holds icy bodies such as Pluto.",
        more: "NASA's New Horizons probe flew past Pluto in 2015 and showed a heart-shaped icy plain." },
      { id: 'constellations', k: 'constellations?|north star|polaris|orion|big dipper|ursa major|zodiac|sirius|betelgeuse', t: 'Constellations and bright stars',
        a: "A **constellation** is a pattern of stars that people have named, and astronomers now divide the sky into **88** official ones. The stars in a pattern are usually not close together, only in the same direction.\n\n**Orion** is easy to spot in winter by its three belt stars; **Betelgeuse** is its red shoulder. **Sirius** in Canis Major is the brightest star in the night sky. **Polaris**, the North Star, sits almost exactly above Earth's north pole, and you can find it by following the end of the Big Dipper.",
        more: "Arab astronomers named many stars still in use today, such as Betelgeuse, Rigel, Altair and Aldebaran." },
      { id: 'moonphases', k: 'phases? of the moon|moon phases?|full moon|new moon|crescent|why (?:does|is) the moon (?:change|shape)', t: 'Phases of the Moon',
        a: "The Moon makes no light of its own. We see the half facing the Sun lit up, and as the Moon orbits Earth we see more or less of that bright half.\n\nThe cycle takes about **29.5 days**: **new moon**, waxing crescent, first quarter, waxing gibbous, **full moon**, waning gibbous, last quarter, waning crescent. It is **not** caused by Earth's shadow; that is a lunar eclipse.",
        more: "The Moon always shows us the same face, because it spins once for each orbit." },
      { id: 'telescopejw', k: 'james webb|jwst|hubble|hubble telescope|space telescopes?', t: 'Hubble and James Webb',
        a: "The **Hubble Space Telescope**, launched in **1990**, orbits Earth and sees in visible light, so it takes sharp pictures without the blur of our air.\n\nThe **James Webb Space Telescope** (launched December 2021) sits about **1.5 million km** from Earth and sees mainly **infrared** light with a 6.5 metre gold-coated mirror. It sees through dust and looks at the very first galaxies, and studies the atmospheres of exoplanets.",
        more: "Because the universe expands, light from the farthest galaxies is stretched into infrared, which is why Webb uses it." },
      { id: 'darkmatter', k: 'dark matter|dark energy|redshift|expanding universe|universe expansion|hubble.?s law|doppler effect', t: 'Dark matter and the expanding universe',
        a: "Astronomers see that galaxies move away from us, and the farther they are the faster they go. The light from them is stretched to longer, redder wavelengths, called **redshift**. That shows the **universe is expanding**.\n\nAbout **5%** of the universe is ordinary matter. About **27%** is **dark matter**, an invisible substance that we only notice by its gravity. About **68%** is **dark energy**, which seems to make the expansion speed up.",
        more: "Nobody knows yet what dark matter or dark energy really are. They are among the biggest open questions in science." },
      { id: 'galaxies', k: 'galaxy types?|andromeda|spiral galaxy|how many galaxies|local group|galaxies', t: 'Galaxies',
        a: "A **galaxy** is a huge group of stars, gas, dust and dark matter held by gravity. They come as **spiral**, **elliptical** and **irregular** shapes.\n\nOur home, the **Milky Way**, is a spiral with 100 to 400 billion stars. The nearest big galaxy, **Andromeda**, is about **2.5 million light-years** away, and it is heading toward us, with a merger expected in about 4 to 5 billion years. Scientists estimate there are **trillions** of galaxies.",
        more: "Almost every large galaxy has a supermassive black hole at its centre. Ours is called Sagittarius A*." },
      { id: 'keplerlaws', k: 'kepler.?s laws?|escape velocity|orbits?|orbital speed|how do satellites stay up|centripetal|how satellites orbit', t: 'Orbits and Kepler',
        a: "A satellite stays up because it is falling around the Earth: it moves sideways so fast that as it falls, the ground curves away beneath it. The ISS does this at about **28,000 km/h**.\n\n**Kepler's laws**: 1) planets move in **ellipses** with the Sun at one focus. 2) a planet speeds up when closer to the Sun. 3) the farther a planet is, the longer its year, since T^2 is proportional to a^3.\n\n**Escape velocity** from Earth, the speed needed to leave without more thrust, is about **11.2 km/s**.",
        more: "Geostationary satellites orbit at 35,786 km, where one lap takes exactly a day so they hover over one spot." },
      { id: 'spacerace', k: 'space race|yuri gagarin|first man in space|first astronaut|sputnik|first satellite|voyager|first woman in space|apollo 11', t: 'The Space Age',
        a: "- **1957**: the Soviet Union launched **Sputnik 1**, the first artificial satellite.\n- **1961**: **Yuri Gagarin** became the first person in space, and orbited Earth once.\n- **1963**: **Valentina Tereshkova** became the first woman in space.\n- **1969**: **Neil Armstrong** and **Buzz Aldrin** of Apollo 11 walked on the Moon.\n- **1977**: **Voyager 1 and 2** launched. Voyager 1 is now the farthest human-made object, in interstellar space.",
        more: "Laika, a dog, was the first animal to orbit the Earth, in 1957." },
      { id: 'aurora', k: 'auroras?|northern lights|southern lights|meteor showers?|shooting stars?|meteors?|meteorites?', t: 'Auroras and meteors',
        a: "An **aurora** (northern or southern lights) happens when charged particles from the Sun hit gases in our upper atmosphere near the poles. Oxygen glows green or red and nitrogen glows blue or purple.\n\nA **meteoroid** is a small piece of space rock. When it burns through the air it makes a bright streak called a **meteor**, or shooting star. If it reaches the ground it is a **meteorite**. **Meteor showers** such as the Perseids in August happen when Earth passes through the dust trail of a comet.",
        more: "Most shooting stars are no bigger than a grain of sand." },
      { id: 'aliens', k: 'aliens?|extraterrestrial|life on other planets|seti|fermi paradox|is there life', t: 'Life beyond Earth',
        a: "We have **not found life beyond Earth** yet. Scientists look in places with liquid water, such as Mars (past or underground), Jupiter's moon **Europa** and Saturn's moon **Enceladus**, which have oceans under their ice.\n\n**SETI** listens for radio signals from other civilisations. The **Fermi paradox** asks: with so many stars, why have we seen no sign of anyone? Nobody knows the answer.",
        more: "The Drake equation is a famous way to estimate how many civilisations might exist in our galaxy." },

      { id: 'worldknowledge', k: 'world knowledge|public data|general knowledge|pre-?training|training data|how (?:do|does) ai learn|what (?:does|do) ai know|what can ai (?:truly )?know|ai knowledge|knowledge of ai', t: 'World knowledge in AI',
        a: "**World knowledge (public data)** is the broad, general information found openly on the internet, in books and in public databases: history, maths, science and general culture. A large AI model learns it during **pre-training**.\n\nAI does not memorise facts like a person. It learns patterns in huge amounts of text, so it can answer widely known questions, but it can be out of date or wrong about things that happened after its training.",
        more: "I am different: my knowledge is a set of topics written into this page, plus lookups on the web when you are online." },
      { id: 'proprietary', k: 'proprietary knowledge|private data|professional knowledge|company data|crm|fine.?tuning|rag|retrieval augmented|retrieval', t: 'Professional and private knowledge',
        a: "**Professional or proprietary knowledge** is internal, non-public information such as company documents, customer records (CRM) and custom workflows. A general AI never saw it in training, so people give it to the AI in two main ways:\n\n- **Retrieval (RAG)**: the system searches the private documents and hands the relevant pieces to the model with your question.\n- **Fine-tuning**: the model is trained further on the private examples so it adapts to them.",
        more: "RAG is usually cheaper and easier to keep up to date, because you can change the documents without retraining the model." },
      { id: 'patternknowledge', k: 'pattern knowledge|relational knowledge|statistical (?:relationships?|patterns?)|probability weights?|model weights?|parameters?|how (?:do )?ai models? store', t: 'Pattern and relational knowledge',
        a: "**Pattern and relational knowledge** is how AI really stores what it learned. Instead of a table of facts, a model keeps billions of numbers called **weights** or **parameters** that capture statistical relationships and patterns found in its training data, such as which words tend to follow others and which ideas are related.\n\nWhen you ask something, it uses those patterns to calculate the most likely good answer, which is why it can generalise to new questions but can also make confident mistakes.",
        more: "Training adjusts the weights step by step to reduce its prediction errors. Using the trained model to answer is called inference." },
      { id: 'nlp', k: 'nlp|natural language processing|sentiment analysis|tokenization|ai tokens?', t: 'Natural language processing',
        a: "**Natural language processing (NLP)** lets computers understand, interpret and generate human language, including its context and intent. It powers translation, voice assistants, spam filters, search and chatbots.\n\nA first step is **tokenization**: breaking text into small pieces called **tokens** (words or parts of words), which are turned into numbers the model can work with. Typical tasks are **sentiment analysis** (is this review happy or angry?), summarising, translating and answering questions.",
        more: "In English, one token is roughly three quarters of a word." },
      { id: 'deeplearning', k: 'deep learning|machine learning|neural networks?|deep neural|transformers?|attention mechanism|backpropagation|how (?:do )?neural networks? work', t: 'Machine learning and deep learning',
        a: "**Machine learning** lets a program learn from examples instead of fixed rules. **Deep learning** is a kind of machine learning that uses **neural networks** with many layers of simple connected units, loosely inspired by brain cells. Each layer finds more complex features, for example edges, then shapes, then faces.\n\nThe network is trained by comparing its answers with the right ones and adjusting its weights to reduce the error, a method called **backpropagation**.",
        more: "Most modern language models use the **transformer** design from 2017, which uses attention to weigh which words matter most to each other." },
      { id: 'multimodal', k: 'multi.?modal|multimodal ai|image recognition|computer vision|speech recognition|voice recognition|text to speech|image generation|ai images?|video generation', t: 'Multimodal AI',
        a: "**Multimodal AI** handles several kinds of data at once: text, audio, images and video. For example it can read a chart, listen to a voice memo, or describe a video.\n\n- **Computer vision**: recognising what is in pictures.\n- **Speech recognition**: turning speech into text.\n- **Text to speech**: turning text into a voice.\n- **Image and video generation**: creating pictures from a written description.",
        more: "I can listen and speak using your browser's built-in speech tools, and I can look at the size and colours of photos you add, but I do not generate videos or photos with AI." },
      { id: 'generative', k: 'generative ai|generative synthesis|gen ai|genai|diffusion models?|text generation|how (?:do )?ai(?:s)? (?:write|create|generate)|predict the next word', t: 'Generative AI',
        a: "**Generative AI** creates new content such as text, code, images, music or video. **Generative synthesis** means it does this from probabilistic predictions rather than exact data lookups.\n\nA language model predicts the **next token** again and again, choosing likely ones, which builds up sentences and programs. **Diffusion models** make images by starting from random noise and cleaning it up step by step until it matches the description.",
        more: "Because it is predicting what is likely and not looking up what is true, generative AI can produce wrong answers that sound sure." },
      { id: 'llm', k: 'llms?|large language models?|gpt|chatgpt|claude|how (?:does|do) chatgpt work|training vs inference|context window', t: 'Large language models',
        a: "A **large language model (LLM)** is a neural network trained on a huge amount of text to predict the next token. With enough data and size, it gets skilled at answering, summarising, translating and writing code. ChatGPT, Claude and Gemini are all products built on LLMs.\n\nIt has two phases: **training** (learning the patterns, which takes huge computers) and **inference** (answering your questions). The **context window** is how much text it can read at once.",
        more: "I am not an LLM. I am a much smaller rule-based brain living in this page, which is why I can run with no internet." },
      { id: 'hallucination', k: 'hallucinations?|ai (?:mistakes?|errors?|wrong)|why (?:is|does) ai (?:wrong|lie|make mistakes)|ai bias|bias in ai|ai ethics|responsible ai', t: 'AI mistakes and bias',
        a: "An AI **hallucination** is a confident answer that is false or invented. It happens because the model predicts likely words, not verified facts. To reduce the risk: ask for sources, check important facts, and use **retrieval** so answers are based on real documents.\n\n**Bias** appears when the training data is unbalanced or unfair, so the AI repeats those patterns. Responsible AI means testing for bias, protecting privacy, being honest about limits and keeping a human in charge of big decisions.",
        more: "If I give you a wrong answer, say so, and I will try the web or you can teach me the right one." },
      { id: 'prompt', k: 'prompt engineering|prompts?|how to prompt|write (?:a )?good prompt|ai agents?|agentic', t: 'Prompts and AI agents',
        a: "A **prompt** is what you tell an AI. Better prompts get better answers:\n\n- Say clearly **what you want** and **who it is for**.\n- Give the **format**: a list, a table, 100 words.\n- Add **examples** of what good looks like.\n- Give the needed **context** and say what to avoid.\n\nAn **AI agent** goes further: it can plan steps, use tools such as search or code, and carry out a multi-step task.",
        more: "If the first answer is not right, tell the AI what to change instead of starting again." },
      { id: 'turing', k: 'turing test|alan turing|artificial general intelligence|agi|narrow ai|types of ai|history of ai|eliza', t: 'Types and history of AI',
        a: "In **1950** Alan Turing suggested the **Turing test**: if a person chatting cannot tell a machine from a human, the machine can be called intelligent. The name **artificial intelligence** was coined at a 1956 workshop at Dartmouth.\n\n**Narrow AI** does one kind of task well, such as chess, translation or recommending videos. All AI today is narrow, even though some systems look general. **AGI** (artificial general intelligence), a machine as flexible as a human across any task, does not exist yet.",
        more: "ELIZA, a 1966 program that echoed people's words back as questions, was one of the first chatbots." },
      { id: 'cartypes', k: 'types? of cars?|car body types?|sedan|suv|hatchback|coupe|convertible|pickup trucks?|minivan|crossover|station wagon|estate car|roadster', t: 'Types of cars',
        a: "- **Sedan (saloon)**: four doors and a separate boot.\n- **Hatchback**: a rear door that lifts up with the glass, compact and practical.\n- **Estate (wagon)**: a long roof with lots of luggage space.\n- **SUV**: a tall car with off-road looks, and often all-wheel drive.\n- **Crossover**: an SUV built on a car's body, so it is lighter and smoother.\n- **Coupe**: a sporty two-door.\n- **Convertible (cabriolet)**: a folding or removable roof.\n- **Pickup**: an open cargo bed at the back.\n- **Minivan (MPV)**: a family car with sliding doors and three rows of seats.\n- **Roadster**: a small open two-seater.",
        more: "A supercar is a very fast exotic car, and a hypercar is the top level above it, with extreme power and price." },
      { id: 'carparts', k: 'car parts?|radiator|alternator|spark plugs?|catalytic converter|exhaust system|suspension|shock absorbers?|dampers?|steering|ecu|fuel injection|timing belt|drive ?shaft|differential|axle', t: 'Main car parts',
        a: "- **Radiator**: cools the engine with coolant.\n- **Alternator**: makes electricity and charges the battery while the engine runs.\n- **Spark plug**: lights the fuel and air mix in a petrol engine.\n- **Fuel injector**: sprays fuel into the engine in exact amounts.\n- **ECU**: the engine's computer.\n- **Catalytic converter**: turns the worst exhaust gases into less harmful ones.\n- **Suspension and dampers**: let the wheels follow the road so the ride is smooth and the tyres keep grip.\n- **Differential**: lets the left and right wheels turn at different speeds in a corner.\n- **Timing belt or chain**: keeps the valves and pistons in step.",
        more: "A driveshaft carries power from the gearbox to the wheels." },
      { id: 'carrecords', k: 'best selling car|best-selling car|most sold car|most expensive car|most expensive cars?|best selling vehicles?|most popular car', t: 'Car records',
        a: "- **Best-selling car ever**: the **Toyota Corolla**, with over 50 million sold since 1966. The **Volkswagen Beetle** (about 21.5 million) and the **Ford Model T** (about 15 million) are also huge.\n- **Best-selling vehicle in the USA**: the **Ford F-Series** pickup for over 40 years.\n- **Most expensive car ever sold at auction**: a 1955 **Mercedes-Benz 300 SLR Uhlenhaut Coupe**, sold for 135 million euros in 2022.\n- **Ferrari 250 GTO**: one sold privately for about 70 million US dollars in 2018.\n- **Fastest production car**: see the Bugatti Chiron Super Sport 300+, which passed 490 km/h in a test run.",
        more: "The Rolls-Royce Boat Tail, a one-off from 2021, is among the most expensive new cars at around 28 million US dollars." },
      { id: 'motorsport', k: 'motorsport|motor racing|rally|wrc|le mans|24 hours of le mans|nascar|indy ?500|drifting|drag racing|nurburgring|nürburgring|motogp', t: 'Motorsport',
        a: "- **Formula 1**: single-seat cars on circuits, the top level of racing.\n- **WRC (World Rally Championship)**: modified road cars racing on gravel, tarmac and snow.\n- **24 Hours of Le Mans**: an endurance race in France where teams drive non-stop for a day. Ford, Ferrari, Porsche and Audi have won it many times.\n- **NASCAR**: American stock-car racing on ovals.\n- **Indy 500**: a famous American single-seat race.\n- **Drifting**: judges sliding style and angle.\n- **Nurburgring Nordschleife**: a 20 km German track nicknamed 'the Green Hell', where car makers test and set lap records.",
        more: "MotoGP is the same idea as Formula 1, but with motorcycles." },
      { id: 'carhistory2', k: 'car timeline|history of the automobile|steam car|first electric car|first hybrid|first car in the world|evolution of cars?', t: 'Car timeline',
        a: "- **1769**: Nicolas Cugnot built a steam-powered road vehicle.\n- **1886**: Karl Benz patented the first practical petrol car.\n- **1890s**: early electric cars appeared, and for a while they were more popular than petrol cars.\n- **1908**: Ford Model T. **1913**: moving assembly line.\n- **1938**: the Volkswagen Beetle design starts mass production before and after the war.\n- **1959**: three-point seat belt by Volvo.\n- **1997**: Toyota Prius, the first mass-produced hybrid.\n- **2008 to 2012**: Tesla Roadster and Model S bring modern EVs.\n- **Today**: electric power and driver-assist systems are growing fast.",
        more: "The Lohner-Porsche of 1900 was an early hybrid." },
      { id: 'carpeople', k: 'enzo ferrari|ferruccio lamborghini|henry ford|ettore bugatti|colin chapman|carroll shelby|ferdinand porsche|soichiro honda|karl benz', t: 'Famous car people',
        a: "- **Karl Benz** (1844 to 1929): built the first practical petrol car in 1886.\n- **Henry Ford** (1863 to 1947): made cars cheap with the Model T and the moving assembly line.\n- **Enzo Ferrari** (1898 to 1988): a racing driver who founded Scuderia Ferrari in 1929 and Ferrari the car maker in 1947.\n- **Ferruccio Lamborghini** (1916 to 1993): a tractor maker who started Lamborghini in 1963 after an argument with Enzo Ferrari, as the story goes.\n- **Ferdinand Porsche** (1875 to 1951): engineer behind the Volkswagen Beetle, and father of the Porsche company.\n- **Ettore Bugatti** (1881 to 1947): the Italian-born founder of Bugatti in France.\n- **Colin Chapman** (1928 to 1982): founder of Lotus, famous for the motto 'add lightness'.\n- **Soichiro Honda** (1906 to 1991): founder of Honda.",
        more: "Ask about a brand, for example 'tell me about Lotus', to learn more." }
    ];
    KB.forEach(function (e) { e.re = new RegExp('(?:^|[^a-z0-9])(?:' + e.k + ')(?![a-z0-9])', 'i'); });
    var QCUE = /\b(what|whats|what's|who|explain|tell me|define|meaning|how does|how do|why|describe|teach me)\b/i;
    var STOP = {};
    'that this with from have what when where which about there their would could should into your then than them they were been will does just like more some very also over only such here these those make made many much each other after before because while'.split(' ').forEach(function (w) { STOP[w] = 1; });
    function stem(w) { return w.length > 4 ? w.replace(/(ing|ed|es|s)$/, '') : w; }
    function lev(a, b) {
      if (a === b) return 0;
      var prev = [], i, j;
      for (j = 0; j <= b.length; j++) prev[j] = j;
      for (i = 1; i <= a.length; i++) {
        var cur = [i];
        for (j = 1; j <= b.length; j++) cur[j] = Math.min(prev[j] + 1, cur[j - 1] + 1, prev[j - 1] + (a.charAt(i - 1) === b.charAt(j - 1) ? 0 : 1));
        prev = cur;
      }
      return prev[b.length];
    }
    var VOCAB = [], DF = {};
    KB.forEach(function (e) {
      e.k.split('|').forEach(function (kw) {
        var w = kw.replace(/[^a-z]/g, '');
        if (w.length >= 6 && kw.indexOf(' ') < 0) VOCAB.push([w, e]);
      });
      var toks = {};
      (e.t + ' ' + e.a.replace(/```[\s\S]*?```/g, ' ')).toLowerCase().replace(/[^a-z ]/g, ' ').split(/\s+/).forEach(function (w) { if (w.length >= 4 && !STOP[w]) toks[stem(w)] = 1; });
      e.toks = toks;
      Object.keys(toks).forEach(function (t) { DF[t] = (DF[t] || 0) + 1; });
    });
    function kbFind(low) {
      var best = null, score = 0;
      KB.forEach(function (e) {
        if (e.strict && !QCUE.test(low)) return;
        var m = low.match(e.re);
        if (m) { var s = m[0].trim().length; if (s > score) { score = s; best = e; } }
      });
      if (best) return { e: best, how: 'exact' };
      var words = low.replace(/[^a-z ]/g, ' ').split(/\s+/).filter(function (w) { return w.length >= 6; });
      if (QCUE.test(low) || low.split(' ').length <= 3) {
        for (var i = 0; i < words.length; i++) {
          for (var j = 0; j < VOCAB.length; j++) {
            var v = VOCAB[j][0], d = lev(words[i], v);
            if (d > 0 && d <= (v.length >= 9 ? 2 : 1)) return { e: VOCAB[j][1], how: 'fuzzy' };
          }
        }
      }
      if (QCUE.test(low) || /\?\s*$/.test(low)) {
        var q = {};
        low.replace(/[^a-z ]/g, ' ').split(/\s+/).forEach(function (w) { if (w.length >= 4 && !STOP[w]) q[stem(w)] = 1; });
        var bs = 0, be = null;
        KB.forEach(function (e) {
          var sc = 0, cnt = 0;
          Object.keys(q).forEach(function (t) { if (e.toks[t]) { sc += Math.log(KB.length / (DF[t] || 1)); cnt++; } });
          if (cnt >= 2 && sc > bs) { bs = sc; be = e; }
        });
        if (be && bs >= 5) return { e: be, how: 'close' };
      }
      return null;
    }

    var JOKES = [
      "Why do programmers prefer dark mode? Because light attracts bugs.",
      "There are 10 kinds of people: those who understand binary and those who do not.",
      "A SQL query walks into a bar, sees two tables and asks: can I join you?",
      "Why did the developer go broke? He used up all his cache.",
      "How many programmers does it take to change a light bulb? None, that is a hardware problem.",
      "I would tell you a UDP joke, but you might not get it.",
      "Why was the JavaScript developer sad? He did not know how to null his feelings."
    ];
    var FACTS = [
      "The first famous computer bug was a real moth, found inside a Harvard computer in 1947.",
      "Ada Lovelace wrote the first published program for a machine in the 1840s, a century before modern computers.",
      "A day on Venus is longer than its year. It spins once every 243 Earth days but orbits the Sun in 225.",
      "Octopuses have three hearts and blue blood.",
      "Bananas are berries, but strawberries are not.",
      "Sharks are older than trees. They appeared roughly 400 million years ago.",
      "Pots of honey found in ancient Egyptian tombs were still edible after thousands of years.",
      "The word bit comes from binary digit.",
      "Lightning can be several times hotter than the surface of the Sun."
    ];
    var PEP = [
      "Small steps count. Ten focused minutes beats an hour of worrying about starting.",
      "Every expert was once a beginner who kept going after the first error message.",
      "Done and simple is better than perfect and unfinished. Ship the small version first.",
      "You do not need to see the whole staircase. Just take the next step.",
      "Mistakes in code are not failures. They are the program telling you what to fix next."
    ];
    var CAPS = [['France', 'Paris'], ['Japan', 'Tokyo'], ['Egypt', 'Cairo'], ['Brazil', 'Brasilia'], ['Canada', 'Ottawa'], ['Australia', 'Canberra'], ['Kenya', 'Nairobi'], ['Italy', 'Rome'], ['Spain', 'Madrid'], ['Germany', 'Berlin'], ['India', 'New Delhi'], ['Turkey', 'Ankara'], ['Argentina', 'Buenos Aires'], ['Morocco', 'Rabat'], ['Saudi Arabia', 'Riyadh'], ['South Korea', 'Seoul'], ['Mexico', 'Mexico City'], ['Norway', 'Oslo'], ['Thailand', 'Bangkok'], ['Peru', 'Lima']];

    [['Afghanistan', 'Kabul'], ['Albania', 'Tirana'], ['Algeria', 'Algiers'], ['Angola', 'Luanda'], ['Armenia', 'Yerevan'], ['Austria', 'Vienna'], ['Azerbaijan', 'Baku'], ['Bahrain', 'Manama'], ['Bangladesh', 'Dhaka'], ['Belarus', 'Minsk'], ['Belgium', 'Brussels'], ['Bosnia and Herzegovina', 'Sarajevo'], ['Bulgaria', 'Sofia'], ['Cambodia', 'Phnom Penh'], ['Cameroon', 'Yaounde'], ['Chad', 'Ndjamena'], ['Chile', 'Santiago'], ['China', 'Beijing'], ['Colombia', 'Bogota'], ['Costa Rica', 'San Jose'], ['Croatia', 'Zagreb'], ['Cuba', 'Havana'], ['Cyprus', 'Nicosia'], ['Czech Republic', 'Prague'], ['Denmark', 'Copenhagen'], ['Ecuador', 'Quito'], ['El Salvador', 'San Salvador'], ['Estonia', 'Tallinn'], ['Ethiopia', 'Addis Ababa'], ['Finland', 'Helsinki'], ['Georgia', 'Tbilisi'], ['Ghana', 'Accra'], ['Greece', 'Athens'], ['Guatemala', 'Guatemala City'], ['Hungary', 'Budapest'], ['Iceland', 'Reykjavik'], ['Indonesia', 'Jakarta'], ['Iran', 'Tehran'], ['Iraq', 'Baghdad'], ['Ireland', 'Dublin'], ['Jamaica', 'Kingston'], ['Jordan', 'Amman'], ['Kazakhstan', 'Astana'], ['Kuwait', 'Kuwait City'], ['Latvia', 'Riga'], ['Lebanon', 'Beirut'], ['Libya', 'Tripoli'], ['Lithuania', 'Vilnius'], ['Madagascar', 'Antananarivo'], ['Malaysia', 'Kuala Lumpur'], ['Mali', 'Bamako'], ['Mauritania', 'Nouakchott'], ['Mongolia', 'Ulaanbaatar'], ['Mozambique', 'Maputo'], ['Nepal', 'Kathmandu'], ['Netherlands', 'Amsterdam'], ['New Zealand', 'Wellington'], ['Nigeria', 'Abuja'], ['North Korea', 'Pyongyang'], ['Oman', 'Muscat'], ['Pakistan', 'Islamabad'], ['Panama', 'Panama City'], ['Paraguay', 'Asuncion'], ['Philippines', 'Manila'], ['Poland', 'Warsaw'], ['Portugal', 'Lisbon'], ['Qatar', 'Doha'], ['Romania', 'Bucharest'], ['Russia', 'Moscow'], ['Rwanda', 'Kigali'], ['Senegal', 'Dakar'], ['Serbia', 'Belgrade'], ['Singapore', 'Singapore'], ['Slovakia', 'Bratislava'], ['Slovenia', 'Ljubljana'], ['Somalia', 'Mogadishu'], ['Sudan', 'Khartoum'], ['Sweden', 'Stockholm'], ['Switzerland', 'Bern'], ['Syria', 'Damascus'], ['Tanzania', 'Dodoma'], ['Tunisia', 'Tunis'], ['Uganda', 'Kampala'], ['Ukraine', 'Kyiv'], ['United Arab Emirates', 'Abu Dhabi'], ['United Kingdom', 'London'], ['United States', 'Washington DC'], ['Uruguay', 'Montevideo'], ['Uzbekistan', 'Tashkent'], ['Venezuela', 'Caracas'], ['Vietnam', 'Hanoi'], ['Yemen', 'Sanaa'], ['Zambia', 'Lusaka'], ['Zimbabwe', 'Harare']].forEach(function (p) { CAPS.push(p); });
    var ALIAS = { usa: 'United States', us: 'United States', america: 'United States', 'united states of america': 'United States', uk: 'United Kingdom', britain: 'United Kingdom', 'great britain': 'United Kingdom', uae: 'United Arab Emirates', emirates: 'United Arab Emirates', 'czechia': 'Czech Republic', 'ksa': 'Saudi Arabia', 'holland': 'Netherlands', 'korea': 'South Korea', 'turkiye': 'Turkey' };
    function plainText(s) { return String(s).toLowerCase().normalize('NFD').replace(/[̀-ͯ]/g, '').replace(/[^a-z ]/g, '').replace(/\s+/g, ' ').trim(); }
    function capitalOf(name) {
      var q = plainText(name).replace(/^the /, '');
      var al = ALIAS[q]; if (al) q = plainText(al);
      for (var i = 0; i < CAPS.length; i++) if (plainText(CAPS[i][0]) === q) return CAPS[i];
      return null;
    }
    function cleanQuery(t) {
      return t.replace(/^(?:please\s+)?(?:search(?: the web| online| the internet| wikipedia| google)?(?: for| about)?|google|look up|lookup|find out(?: about)?|find (?:me )?(?:info|information) (?:on|about)|wikipedia)\s*:?\s*/i, '')
        .replace(/\s+(?:on|in|using) (?:the web|wikipedia|google|the internet)$/i, '')
        .replace(/^(?:what|who|where|when|why|how)(?:'s| is| was| are| were| did| does| do| can)?\s+(?:the |a |an )?/i, '')
        .replace(/[?!.]+$/, '').trim();
    }

    /* ----- cars: brands and models ----- */
    var BRANDS = [
      /* name|aliases|country|founded|owner|known for|models|flags (a = ambiguous word, x = defunct) */
      "Toyota||Japan|1937|Toyota Motor Corporation (independent, listed)|reliability, hybrids and being one of the world's biggest car sellers|Corolla, Camry, Land Cruiser, Hilux, Prius, Supra, RAV4|",
      "Honda||Japan|1948 (first car 1963)|Independent|efficient engines, motorcycles and reliable cars|Civic, Accord, CR-V, NSX, S2000, Type R|",
      "Nissan||Japan|1933|Renault-Nissan-Mitsubishi alliance|the GT-R supercar, the Leaf electric car and tough SUVs|GT-R, Skyline, Patrol, Qashqai, Leaf, 370Z, Altima|",
      "Mazda||Japan|1920|Independent|the rotary engine, the MX-5 roadster and Le Mans win in 1991|MX-5 Miata, RX-7, RX-8, Mazda3, CX-5|",
      "Subaru||Japan|1953|Independent|boxer engines, symmetrical all-wheel drive and rally success|Impreza WRX, Outback, Forester, BRZ, Legacy|",
      "Mitsubishi||Japan|1870 (car business 1970)|Renault-Nissan-Mitsubishi alliance|rally wins and tough off-roaders|Lancer Evolution, Pajero, Outlander, Eclipse, 3000GT|",
      "Suzuki||Japan|1909|Independent|small cars, 4x4s and motorcycles|Swift, Jimny, Vitara, Alto, Hayabusa bike|",
      "Lexus||Japan|1989|Toyota|Toyota's luxury brand, quiet and very reliable|LS, RX, IS, LC 500, LFA|",
      "Infiniti||Japan|1989|Nissan|Nissan's luxury brand|Q50, QX60, G35|",
      "Acura||Japan|1986|Honda|Honda's luxury and sports brand, sold in North America|NSX, TLX, MDX, Integra|",
      "Isuzu||Japan|1916|Independent|trucks and diesel engines|D-Max, MU-X, Trooper|",
      "Daihatsu||Japan|1907|Toyota|tiny kei cars and small SUVs|Terios, Copen, Mira, Rocky|",
      "Hyundai||South Korea|1967|Hyundai Motor Group|good value, fast progress and the Ioniq electric cars|Elantra, Sonata, Tucson, Santa Fe, Ioniq 5, i30 N|",
      "Kia||South Korea|1944|Hyundai Motor Group|stylish cars with a long warranty and the EV6|Sportage, Picanto, Sorento, Stinger, EV6|",
      "Genesis||South Korea|2015|Hyundai Motor Group|Hyundai's luxury brand|G70, G80, GV70, GV80|a",
      "KG Mobility|ssangyong|South Korea|1954|KG Group|SUVs and pickups, formerly called SsangYong|Torres, Rexton, Korando, Musso|",
      "Ford||USA|1903|Independent (Ford family still has big control)|the Model T, the moving assembly line and the F-Series pickups|Mustang, F-150, Focus, Fiesta, Bronco, GT40, Explorer|",
      "Chevrolet|chevy|USA|1911|General Motors|the Corvette sports car and big American trucks|Corvette, Camaro, Silverado, Impala, Tahoe, Bolt|",
      "Cadillac||USA|1902|General Motors|American luxury cars|Escalade, CT5-V Blackwing, Eldorado, Lyriq|",
      "GMC||USA|1912|General Motors|trucks and SUVs, and the electric Hummer|Sierra, Yukon, Canyon, Hummer EV|",
      "Buick||USA|1903|General Motors|comfortable family and luxury cars|Enclave, Regal, Riviera, Encore|",
      "Dodge||USA|1900 (cars 1914)|Stellantis|American muscle cars|Charger, Challenger, Viper, Durango, Ram (earlier)|a",
      "Jeep||USA|1941|Stellantis|off-road vehicles that began as a World War II army car|Wrangler, Grand Cherokee, Cherokee, Gladiator, Renegade|",
      "Chrysler||USA|1925|Stellantis|American family cars and minivans|300, Pacifica, Voyager, PT Cruiser|",
      "Ram||USA|2010 (as its own brand)|Stellantis|pickup trucks, split off from Dodge|1500, TRX, 2500, ProMaster|a",
      "Lincoln||USA|1917|Ford|Ford's luxury brand|Navigator, Continental, Aviator, Town Car|",
      "Tesla||USA|2003|Independent (Elon Musk is the CEO)|electric cars, fast acceleration and self-driving software|Model S, Model 3, Model X, Model Y, Cybertruck, Roadster|a",
      "Rivian||USA|2009|Independent|electric pickups and SUVs|R1T, R1S, R2|",
      "Lucid||USA|2007|Independent (Saudi PIF is the main owner)|very long range luxury electric cars|Air, Gravity|a",
      "Pontiac||USA|1926|General Motors (closed 2010)|muscle cars such as the Firebird and GTO|Firebird, Trans Am, GTO, Fiero|x",
      "Hummer||USA|1992|General Motors (now an electric GMC)|huge off-road trucks that began as the military Humvee|H1, H2, H3, Hummer EV|ax",
      "DeLorean||USA|1975|Closed in 1982|the stainless steel DMC-12 with gull-wing doors that starred in Back to the Future|DMC-12|x",
      "Shelby||USA|1962|Independent tuner|Carroll Shelby's Cobra and tuned Mustangs|Cobra, GT350, GT500, Daytona Coupe|a",
      "Volkswagen|vw|Germany|1937|Volkswagen Group|the Beetle, the Golf and being Europe's biggest car group, which also owns Audi, Porsche, Bentley, Lamborghini, Skoda and SEAT|Beetle, Golf, Polo, Passat, Tiguan, ID.4, Touareg|",
      "Audi||Germany|1909 (by August Horch)|Volkswagen Group|the four-ring badge, quattro all-wheel drive and Le Mans wins|A4, A6, Q5, Q7, R8, RS6, TT, e-tron|",
      "BMW||Germany|1916|Independent (listed). It also owns Mini and Rolls-Royce|sporty rear-wheel drive cars, the M division and the slogan 'The ultimate driving machine'|3 Series, 5 Series, M3, M5, X5, i8, Z4|",
      "Mercedes-Benz|mercedes, benz, merc|Germany|1926 (roots go back to Karl Benz in 1886)|Mercedes-Benz Group|luxury, safety technology and the three-pointed star|C-Class, E-Class, S-Class, G-Class, AMG GT, SLS, SL|",
      "Porsche||Germany|1931|Volkswagen Group (the Porsche and Piech families own the main shares)|the rear-engined 911 and many Le Mans wins|911, 718 Cayman, Cayenne, Macan, Taycan, 918 Spyder, Carrera GT|",
      "Opel||Germany|1862 (cars from 1899)|Stellantis|practical family cars. Vauxhall is its British twin|Astra, Corsa, Mokka, Insignia, GT|",
      "Smart||Germany|1994|Mercedes-Benz and Geely|tiny two-seat city cars|Fortwo, Forfour, #1|a",
      "Maybach||Germany|1909|Mercedes-Benz|super-luxury versions of the S-Class and GLS|Maybach S 680, 62, 57|",
      "Trabant||Germany|1957|Closed in 1991|the East German car with a plastic body that people waited years for|601|x",
      "Ferrari||Italy|1939 (first car 1947) by Enzo Ferrari|Independent (listed company)|prancing horse badge, red sports cars and Formula 1 success|F40, Enzo, LaFerrari, 488, 296 GTB, SF90, Testarossa, 250 GTO|",
      "Lamborghini||Italy|1963 by Ferruccio Lamborghini|Audi (Volkswagen Group)|wild wedge-shaped supercars named after fighting bulls|Countach, Miura, Aventador, Huracan, Urus, Revuelto, Diablo|",
      "Maserati||Italy|1914|Stellantis|trident badge, luxury and sporty Italian cars|Ghibli, Quattroporte, MC20, GranTurismo, Levante|",
      "Alfa Romeo|alfa|Italy|1910|Stellantis|passionate design and racing history|Giulia, Stelvio, 4C, 8C, Giulietta, Spider|",
      "Fiat||Italy|1899|Stellantis|small cheap city cars|500, Panda, Punto, Uno, 124 Spider|",
      "Lancia||Italy|1906|Stellantis|rally champions and innovation|Delta Integrale, Stratos, Fulvia, Aurelia|",
      "Abarth||Italy|1949|Stellantis|hot-hatch tuned Fiats with the scorpion badge|595, 500e, 124 Spider|",
      "Pagani||Italy|1992 by Horacio Pagani|Independent|handmade hypercars with beautiful carbon fibre and exhausts|Zonda, Huayra, Utopia|",
      "De Tomaso||Italy|1959|Independent (revived)|the Pantera, a mix of Italian style and Ford V8 power|Pantera, Mangusta, Vallelunga|",
      "Rolls-Royce|rolls royce|United Kingdom|1904 by Charles Rolls and Henry Royce|BMW (since 1998)|the most famous luxury cars, with the Spirit of Ecstasy mascot|Phantom, Ghost, Cullinan, Wraith, Spectre|",
      "Bentley||United Kingdom|1919 by W. O. Bentley|Volkswagen Group (since 1998)|hand-built luxury and Le Mans wins in the 1920s|Continental GT, Bentayga, Flying Spur, Mulsanne|",
      "Aston Martin|aston|United Kingdom|1913|Independent (listed)|James Bond's car brand and elegant grand tourers|DB5, DB11, Vantage, DBS, Valkyrie, Vanquish|",
      "McLaren||United Kingdom|1963 by Bruce McLaren|McLaren Group|Formula 1 teams and carbon fibre road supercars|F1, P1, 720S, Senna, Speedtail, Artura|",
      "Jaguar||United Kingdom|1922|Tata Motors (as part of Jaguar Land Rover)|the leaping cat badge, elegant sports cars and the E-Type|E-Type, F-Type, XJ, XE, F-Pace|a",
      "Land Rover|range rover|United Kingdom|1948|Tata Motors (as part of Jaguar Land Rover)|go-anywhere off-roaders and luxury SUVs|Defender, Range Rover, Discovery, Evoque, Velar|",
      "Lotus||United Kingdom|1952 by Colin Chapman|Geely (and Etika)|light and agile sports cars and Formula 1 titles|Elise, Exige, Evora, Emira, Eletre, Esprit|a",
      "Mini||United Kingdom|1959|BMW|the small, fun, classic Mini and its modern versions|Cooper, Cooper S, Countryman, Clubman|a",
      "MG||United Kingdom|1924|SAIC Motor (China)|affordable British sports cars, now mostly sold as electric and family cars|MGB, MG4, ZS, HS, Cyberster, TF|a",
      "Morgan||United Kingdom|1910|Independent|hand-built classic-looking sports cars with wooden frames|Plus Four, Plus Six, 3 Wheeler, Aero 8|",
      "Vauxhall||United Kingdom|1857 (cars from 1903)|Stellantis|the British twin of Opel|Corsa, Astra, Mokka, Insignia|",
      "Bugatti||France|1909 by Ettore Bugatti|Bugatti Rimac (Rimac is the main owner)|the fastest and most expensive hypercars|Veyron, Chiron, Divo, Bolide, Type 57, Tourbillon|",
      "Renault||France|1899|Independent (listed) and part of an alliance with Nissan and Mitsubishi|small practical cars and Formula 1 engines|Clio, Megane, Twingo, Captur, Zoe, 5 Turbo|",
      "Peugeot||France|1810 as a company (cars from 1889)|Stellantis|the lion badge and stylish family cars|208, 308, 3008, 205 GTI, 406|",
      "Citroen|citroën|France|1919 by Andre Citroen|Stellantis|clever comfort and daring design|2CV, DS, C3, C4 Cactus, Traction Avant|",
      "Alpine||France|1955|Renault|Renault's sporty brand and rally and Formula 1 team|A110, A290, A110 R|a",
      "DS|ds automobiles|France|2014 (the original Citroen DS dates from 1955)|Stellantis|luxury French cars|DS 3, DS 4, DS 7, DS 9|a",
      "Volvo||Sweden|1927|Geely (since 2010)|safety, including inventing the three-point seat belt in 1959|XC90, XC60, S60, V60, 240, EX30|",
      "Koenigsegg||Sweden|1994 by Christian von Koenigsegg|Independent|extreme hypercars with record-breaking speed|Agera, Regera, Jesko, Gemera, CC8S|",
      "Polestar||Sweden|2017 (from the Volvo racing team)|Volvo and Geely|stylish electric performance cars|Polestar 2, 3, 4, 1|",
      "Saab||Sweden|1945|Closed in 2012|jet-plane-inspired, safe and quirky cars|900, 9-3, 9-5, 99|x",
      "Skoda||Czech Republic|1895 (as Laurin and Klement)|Volkswagen Group (since 1991)|practical, roomy cars at a good price|Octavia, Fabia, Superb, Kodiaq, Enyaq|",
      "SEAT||Spain|1950|Volkswagen Group|Spain's biggest car maker|Ibiza, Leon, Ateca, Arona, 600|a",
      "Cupra||Spain|2018 (from SEAT)|Volkswagen Group|SEAT's sporty brand|Formentor, Born, Leon, Ateca|",
      "Dacia||Romania|1966|Renault (since 1999)|very cheap, simple and tough cars|Duster, Sandero, Logan, Spring|",
      "Lada|avtovaz|Russia|1966|AvtoVAZ|rugged Russian cars based on the Fiat 124|Niva, Riva, Vesta, Granta, Samara|",
      "Tata|tata motors|India|1945|Tata Group|India's largest carmaker, which also owns Jaguar Land Rover|Nexon, Harrier, Safari, Nano, Tiago, Punch|",
      "Mahindra||India|1945|Mahindra Group|rugged SUVs and tractors|Thar, Scorpio, XUV700, Bolero, XUV400|",
      "Maruti Suzuki|maruti|India|1981|Suzuki|India's best-selling carmaker|Alto, Swift, Baleno, Dzire, Brezza, Wagon R|",
      "BYD|build your dreams|China|1995 (cars from 2003)|Independent|the world's biggest seller of electric vehicles and a battery maker|Seal, Atto 3, Han, Dolphin, Tang, Yangwang U9|",
      "Geely||China|1986 (cars from 1997)|Independent (Li Shufu)|owner of Volvo, Polestar, Lotus, Zeekr and part of Smart|Coolray, Emgrand, Monjaro, Galaxy E5|",
      "Chery||China|1997|Independent|China's biggest car exporter|Tiggo, Arrizo, QQ, Omoda|",
      "Great Wall Motor|haval|China|1984|Independent|SUVs and pickups under the Haval, Tank and Ora brands|Haval H6, Tank 300, Ora Good Cat, Poer|",
      "NIO||China|2014|Independent|premium electric cars with swappable batteries|ET7, ES8, ET5, EL6|",
      "XPeng|xpeng motors|China|2014|Independent|smart electric cars|P7, G9, G6, X9|",
      "Zeekr||China|2021|Geely|premium electric cars|001, 007, X, 009|",
      "Hongqi||China|1958|FAW Group|China's classic state limousine brand|H9, HS5, E-HS9, L5|",
      "Rimac||Croatia|2009 by Mate Rimac|Independent (it runs Bugatti Rimac)|record-breaking electric hypercars|Nevera, Concept One|",
      "Spyker||Netherlands|1898 (revived 1999)|Independent|hand-built supercars with aircraft-style details|C8, C12, D8|",
      "Zenvo||Denmark|2004|Independent|the Danish hypercar maker|ST1, TSR-S, Aurora|",
      "Holden||Australia|1856 (cars from 1948)|General Motors (brand closed in 2020)|Australia's own cars and V8 racing|Commodore, Monaro, Ute, Torana|ax",
      "Proton||Malaysia|1983|Geely and DRB-Hicom|Malaysia's national car maker|Saga, X50, Perdana, Iriz|a",
      "Perodua||Malaysia|1993|Daihatsu and UMW|Malaysia's best-selling small cars|Myvi, Axia, Bezza, Ativa|",
      "VinFast||Vietnam|2017|Vingroup|Vietnam's electric car maker|VF 8, VF 9, VF 3, VF 6|",
      "Togg||Turkey|2018|Joint company of Turkish firms|Turkey's national electric car|T10X, T10F|",
      "Iran Khodro|ikco|Iran|1962|Independent|Iran's biggest carmaker|Samand, Dena, Peugeot 405, Runna|",
      "General Motors|gm|USA|1908|Independent (listed)|the huge group behind Chevrolet, Cadillac, GMC and Buick|Corvette, Silverado, Escalade, Bolt|a",
      "Stellantis||Netherlands (company base)|2021 (merger of Fiat Chrysler and PSA)|Independent (listed)|a giant group with Fiat, Peugeot, Citroen, Jeep, Dodge, Chrysler, Ram, Opel, Maserati, Alfa Romeo and more|Jeep Wrangler, Peugeot 208, Fiat 500, Ram 1500|"
    ];
    function carNorm(x) { return String(x).toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '').replace(/[^a-z0-9]+/g, ' ').trim(); }
    var BRAND_ROWS = BRANDS.map(function (r) {
      var p = r.split('|');
      var al = (p[1] || '').split(',').map(function (x) { return carNorm(x); }).filter(Boolean);
      return { name: p[0], keys: [carNorm(p[0])].concat(al), country: p[2], year: p[3], owner: p[4], known: p[5], models: p[6], amb: /a/.test(p[7] || '') || p[7] === 'a', gone: /x/.test(p[7] || '') };
    });
    var MODELS = [
      /* name|aliases|brand|years|description|flags */
      "Mustang||Ford|1964|the original American pony car, a sporty coupe with a V8 option. It started the whole 'muscle car' craze|",
      "Corvette||Chevrolet|1953|America's sports car, made with a fibre glass body from the start. The latest mid-engine version came in 2019|",
      "Camaro||Chevrolet|1966|Chevrolet's muscle car and rival to the Mustang|",
      "Model T||Ford|1908|the car that put the world on wheels. Over 15 million were built and it was made cheap by the assembly line|",
      "Beetle|vw beetle, volkswagen beetle|Volkswagen|1938|one of the best-selling cars ever, with about 21.5 million made. It has a rear air-cooled engine and a round shape|a",
      "Golf|vw golf, volkswagen golf|Volkswagen|1974|the car that defined the hatchback, loved for its quality and the sporty GTI and R versions|a",
      "911|porsche 911|Porsche|1964|Porsche's icon with its engine behind the rear axle. It has kept the same shape for 60 years|",
      "GT-R|nissan gt-r, gtr, skyline gt-r, godzilla|Nissan|1969 (modern R35 from 2007)|nicknamed Godzilla for beating much more expensive cars with its twin-turbo V6 and AWD|",
      "Supra|toyota supra|Toyota|1978|a Toyota sports car, famous in the 1990s version with the 2JZ engine|",
      "MX-5|miata, mazda mx-5, mx5|Mazda|1989|the best-selling two-seat sports car in history. It is light, cheap and fun to drive|",
      "Civic|honda civic|Honda|1972|Honda's compact car, loved by tuners, with the fast Type R version|a",
      "Corolla|toyota corolla|Toyota|1966|the best-selling nameplate in history, with over 50 million sold|",
      "Land Cruiser|toyota land cruiser|Toyota|1951|an extremely tough off-roader that is famous across Africa and the Middle East|",
      "Hilux|toyota hilux|Toyota|1968|a pickup so tough that it became a legend, even surviving a TV test of being dropped from a building|",
      "Prius|toyota prius|Toyota|1997|the first mass-produced hybrid car|",
      "Defender|land rover defender|Land Rover|1948 (new model 2020)|a boxy go-anywhere off-roader|a",
      "Range Rover||Land Rover|1970|the first luxury SUV, mixing off-road ability with comfort|",
      "Wrangler|jeep wrangler|Jeep|1986|the direct descendant of the World War II Willys Jeep, with removable doors and roof|a",
      "F-150|ford f-150, f150|Ford|1975|the best-selling pickup in the USA for decades|",
      "Cybertruck|tesla cybertruck|Tesla|2023|an angular electric pickup made of stainless steel|",
      "Model S|tesla model s|Tesla|2012|the electric sedan that made people take EVs seriously|a",
      "Model 3|tesla model 3|Tesla|2017|Tesla's cheaper electric sedan, one of the best-selling EVs ever|",
      "Countach|lamborghini countach|Lamborghini|1974|the wedge-shaped poster car of the 1980s with scissor doors|",
      "Aventador|lamborghini aventador|Lamborghini|2011|a V12 flagship supercar with carbon fibre construction|",
      "Huracan|lamborghini huracan|Lamborghini|2014|Lamborghini's V10 supercar and the best-selling Lamborghini ever|",
      "Urus|lamborghini urus|Lamborghini|2018|a super SUV, the fastest in the class when launched|",
      "F40|ferrari f40|Ferrari|1987|the last car Enzo Ferrari approved. It has a twin-turbo V8 and no luxuries, and was the first road car to pass 200 mph (320 km/h)|",
      "Ferrari Enzo||Ferrari|2002|a limited carbon fibre V12 supercar named after the founder, with Formula 1 technology|",
      "LaFerrari|la ferrari|Ferrari|2013|a hybrid V12 hypercar with about 960 horsepower combined|",
      "Testarossa|ferrari testarossa|Ferrari|1984|the 80s icon with side strakes, famous from Miami Vice|",
      "250 GTO|ferrari 250 gto|Ferrari|1962|one of the most valuable cars in the world. Only 36 were built and one sold for around 70 million US dollars in 2018|",
      "Veyron|bugatti veyron|Bugatti|2005|the first production car to pass 400 km/h, with a 1,001 horsepower 16-cylinder engine|",
      "Chiron|bugatti chiron|Bugatti|2016|a 1,500 horsepower successor to the Veyron. A Super Sport version passed 490 km/h|",
      "DB5|aston martin db5|Aston Martin|1963|the car of James Bond in Goldfinger|",
      "E-Type|jaguar e-type|Jaguar|1961|one of the most beautiful cars ever, according to Enzo Ferrari|",
      "McLaren F1||McLaren|1992|designed by Gordon Murray, with a central driving seat and a BMW V12. In 1998 it set a 391 km/h record for a naturally aspirated car|",
      "720S|mclaren 720s|McLaren|2017|a 710 horsepower carbon fibre supercar|",
      "Mini Cooper|classic mini|Mini|1959|the small British car designed by Alec Issigonis, with the engine across the front and wheels at the corners|",
      "2CV|citroen 2cv|Citroen|1948|a simple French car designed to carry farmers and eggs across a field without breaking them|",
      "Fiat 500||Fiat|1957|a tiny Italian city car that returned in 2007|a",
      "G-Class|g wagon, g wagen, mercedes g class, g63|Mercedes-Benz|1979|a boxy military-style off-roader that became a luxury icon with an AMG G63 version|",
      "S-Class|mercedes s class|Mercedes-Benz|1972|the flagship luxury sedan that often introduces new safety technology first|",
      "Phantom|rolls royce phantom|Rolls-Royce|1925 (current from 2003)|the biggest and grandest Rolls-Royce|a",
      "Cullinan|rolls royce cullinan|Rolls-Royce|2018|the first Rolls-Royce SUV, named after the biggest diamond ever found|",
      "Stratos|lancia stratos|Lancia|1973|a wedge-shaped car built just to win rallies, and it won three world titles|",
      "Niva|lada niva|Lada|1977|a small, simple and tough Russian off-roader, still made today|",
      "Nano|tata nano|Tata|2008|promoted as the world's cheapest car, at around 100,000 rupees|",
      "Jimny|suzuki jimny|Suzuki|1970|a tiny off-roader with a ladder frame and low gearing that goes where bigger SUVs cannot|",
      "Patrol|nissan patrol|Nissan|1951|a big off-road SUV, very popular in the Middle East|a",
      "Lancer Evolution|evo, mitsubishi evo, lancer evo|Mitsubishi|1992|a rally-bred turbo AWD sports sedan that rivaled the Subaru Impreza WRX|",
      "WRX|subaru wrx, impreza wrx, impreza|Subaru|1992|a turbocharged AWD sports sedan built on rally success|",
      "Charger|dodge charger|Dodge|1966|an American muscle car, famous from the Dukes of Hazzard and Fast and Furious|a",
      "Challenger|dodge challenger|Dodge|1970|a retro muscle car, with the Hellcat having over 700 horsepower|a",
      "Viper|dodge viper|Dodge|1992|a brutal V10 American sports car|a",
      "Huayra|pagani huayra|Pagani|2011|a twin-turbo V12 hypercar named after the Andean wind god|",
      "Jesko|koenigsegg jesko|Koenigsegg|2019|a hypercar aiming for over 480 km/h, named after Christian's father|",
      "Nevera|rimac nevera|Rimac|2021|an all-electric hypercar with about 1,900 horsepower, with a 0 to 100 km/h time under 2 seconds|",
      "Quattro|audi quattro|Audi|1980|the car that brought all-wheel drive to rallying and made AWD famous|a",
      "Spirit of Ecstasy|rolls royce mascot|Rolls-Royce|1911|the famous mascot of the Rolls-Royce radiator|"
    ];
    var MODEL_ROWS = MODELS.map(function (r) {
      var p = r.split('|');
      var al = (p[1] || '').split(',').map(function (x) { return carNorm(x); }).filter(Boolean);
      return { name: p[0], keys: [carNorm(p[0])].concat(al), brand: p[2], year: p[3], desc: p[4], amb: p[5] === 'a' };
    });
    var CAR_CUE = /\b(car|cars|brand|brands|company|maker|manufacturer|made|owner|owns|owned|models|vehicle|vehicles|motors|auto)\b/;
    function hasKey(pt, key) { return (' ' + pt + ' ').indexOf(' ' + key + ' ') > -1; }
    function brandCard(b) {
      return '**' + b.name + '** (' + b.country + ', founded ' + b.year + ')' + (b.gone ? ' – no longer made' : '') + '\n\n- Owner: ' + b.owner + '\n- Known for: ' + b.known + '\n- Famous models: ' + b.models;
    }
    function carLookup(low) {
      var pt = carNorm(low);
      if (pt.length > 90) return null;
      if (/^(?:tell me about|talk about|what are|what is|explain|about|i love|i like)\s+(?:the\s+)?(?:cars?|automobiles?|vehicles?)$/.test(pt)) return 'Cars, great topic! Here is what I know:\n\n- **' + BRAND_ROWS.length + ' car makers and groups**, like Toyota, Ferrari, BYD and Mahindra (try "tell me about Bugatti")\n- **About 55 famous models**, like the Mustang, 911 and Cybertruck\n- **How cars work**: engines, brakes, tyres, gearboxes, turbos and hybrids\n- **Records and history**: fastest cars, best sellers, Formula 1 and Le Mans\n\nWhat do you want to explore?';
      if (/\b(enzo ferrari|ferruccio|henry ford|ettore bugatti|colin chapman|carroll shelby|karl benz|ferdinand porsche|soichiro honda)\b/.test(pt)) return null;
      var carCue = CAR_CUE.test(pt);
      /* brand lists */
      if (/\b(list|all|names?|types?)\b.*\bcar (brands?|makers?|companies|manufacturers?)\b|\bcar (brands?|makers?|companies|manufacturers?)\b.*(from|in|of|by)\b|^(?:what|which) (?:car )?(?:brands?|makers?|companies)|^car (brands?|makers?|companies|manufacturers?)$|how many car (brands?|makers?)/.test(pt) || /^(?:all )?(?:the )?car (brands?|makers?) (?:list|names)/.test(pt)) {
        var cm = pt.match(/\b(japan|japanese|germany|german|italy|italian|france|french|usa|american|america|united states|uk|british|britain|england|sweden|swedish|china|chinese|korea|korean|south korea|india|indian|czech|spain|spanish|russia|russian)\b/);
        var MAP = { japanese: 'Japan', german: 'Germany', italian: 'Italy', french: 'France', usa: 'USA', american: 'USA', america: 'USA', 'united states': 'USA', uk: 'United Kingdom', british: 'United Kingdom', britain: 'United Kingdom', england: 'United Kingdom', swedish: 'Sweden', chinese: 'China', korea: 'South Korea', korean: 'South Korea', 'south korea': 'South Korea', indian: 'India', czech: 'Czech Republic', spanish: 'Spain', russian: 'Russia' };
        if (cm) {
          var cn = MAP[cm[1]] || (cm[1].charAt(0).toUpperCase() + cm[1].slice(1));
          var list = BRAND_ROWS.filter(function (b) { return b.country.indexOf(cn) === 0 && !b.gone; }).map(function (b) { return b.name; });
          if (list.length) return '**Car brands from ' + cn + '** (' + list.length + '):\n\n' + list.join(', ') + '.\n\nAsk me about any of them, for example "tell me about ' + list[0] + '".';
        }
        var groups = {};
        BRAND_ROWS.forEach(function (b) { if (b.gone) return; var c = b.country.replace(/ \(.*/, ''); (groups[c] = groups[c] || []).push(b.name); });
        var order = ['Japan', 'South Korea', 'USA', 'Germany', 'Italy', 'United Kingdom', 'France', 'Sweden', 'China', 'India'];
        var lines = order.filter(function (c) { return groups[c]; }).map(function (c) { return '- **' + c + '**: ' + groups[c].join(', '); });
        var rest = Object.keys(groups).filter(function (c) { return order.indexOf(c) < 0; }).map(function (c) { return groups[c].join(', ') + ' (' + c + ')'; });
        return 'I know about **' + BRAND_ROWS.length + '** car makers and groups:\n\n' + lines.join('\n') + (rest.length ? '\n- **Others**: ' + rest.join(', ') : '') + '\n\nAsk "tell me about Ferrari" or "which cars does Toyota make".';
      }
      /* find brands */
      var found = [];
      BRAND_ROWS.forEach(function (b) {
        for (var i = 0; i < b.keys.length; i++) {
          if (hasKey(pt, b.keys[i])) {
            if (b.amb && !carCue && pt !== b.keys[i]) break;
            found.push(b); break;
          }
        }
      });
      /* models first if specific */
      var mf = [];
      MODEL_ROWS.forEach(function (mo) {
        for (var i = 0; i < mo.keys.length; i++) {
          if (hasKey(pt, mo.keys[i])) {
            var brandHit = hasKey(pt, carNorm(mo.brand));
            if (mo.amb && !carCue && !brandHit && i === 0) break;
            mf.push({ m: mo, len: mo.keys[i].length }); break;
          }
        }
      });
      var isQ = /\b(about|what|whats|who|where|when|tell|explain|describe|info|information|history|models|make|makes|made|owns?|owner|from|country|founded|compare|vs|versus|difference|which|famous)\b/.test(pt) || pt.split(' ').length <= 4;
      if (mf.length && isQ && !/\b(compare|vs|versus|difference)\b/.test(pt)) {
        mf.sort(function (a, b) { return b.len - a.len; });
        var mo = mf[0].m;
        return '**' + (mo.brand === 'Mini' || mo.name.indexOf(mo.brand) === 0 ? mo.name : mo.brand + ' ' + mo.name) + '** (' + mo.year + ')\n\n' + mo.desc.charAt(0).toUpperCase() + mo.desc.slice(1) + '.\n\nMade by ' + mo.brand + '.';
      }
      if (!found.length || !isQ) return null;
      /* drop brands contained in a longer matched brand (Rolls inside Rolls-Royce etc.) */
      var uniq = [];
      found.forEach(function (b) { if (uniq.indexOf(b) < 0) uniq.push(b); });
      uniq = uniq.slice(0, 3);
      var b0 = uniq[0];
      if (uniq.length > 1 && /\b(compare|vs|versus|difference|better|or|and)\b/.test(pt)) return uniq.map(brandCard).join('\n\n---\n\n');
      if (/\b(owns?|owner|owned|parent|belongs?)\b/.test(pt)) return '**' + b0.name + '** is owned by: **' + b0.owner + '**.\n\n' + brandCard(b0);
      if (/\b(where|country|from|origin)\b/.test(pt)) return '**' + b0.name + '** is from **' + b0.country + '**.\n\n' + brandCard(b0);
      if (/\b(when|founded|started|began|year)\b/.test(pt)) return '**' + b0.name + '** was founded in **' + b0.year + '**.\n\n' + brandCard(b0);
      if (/\b(models|cars|makes?|famous)\b/.test(pt)) return 'Famous **' + b0.name + '** models: ' + b0.models + '.\n\n' + brandCard(b0);
      return brandCard(b0);
    }

    /* ----- code templates ----- */
    var T = {};
    T.todo = { m: 'to-?do|task list|tasks? app|checklist', n: 'A to-do app that remembers your tasks.', lang: 'html', code: `<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>To-do</title>
<style>
  body { font-family: system-ui, sans-serif; max-width: 480px; margin: 40px auto; padding: 0 16px; }
  form { display: flex; gap: 8px; }
  input[type=text] { flex: 1; padding: 10px; font-size: 1rem; }
  li { display: flex; gap: 8px; align-items: center; padding: 8px 0; list-style: none; }
  li.done span { text-decoration: line-through; opacity: .5; }
  li span { flex: 1; }
</style>
</head>
<body>
<h1>My tasks</h1>
<form id="f"><input id="t" type="text" placeholder="Add a task" required><button>Add</button></form>
<ul id="list"></ul>
<script>
  var tasks = [];
  try { tasks = JSON.parse(localStorage.getItem("tasks") || "[]"); } catch (e) {}
  function save() {
    try { localStorage.setItem("tasks", JSON.stringify(tasks)); } catch (e) {}
    render();
  }
  function render() {
    var ul = document.getElementById("list");
    ul.innerHTML = "";
    tasks.forEach(function (task, i) {
      var li = document.createElement("li");
      if (task.done) li.className = "done";
      var box = document.createElement("input");
      box.type = "checkbox";
      box.checked = task.done;
      box.onchange = function () { task.done = box.checked; save(); };
      var span = document.createElement("span");
      span.textContent = task.text;
      var del = document.createElement("button");
      del.textContent = "Delete";
      del.onclick = function () { tasks.splice(i, 1); save(); };
      li.append(box, span, del);
      ul.appendChild(li);
    });
  }
  document.getElementById("f").onsubmit = function (e) {
    e.preventDefault();
    var t = document.getElementById("t");
    tasks.push({ text: t.value, done: false });
    t.value = "";
    save();
  };
  render();
<\/script>
</body>
</html>`, how: 'Save it as `todo.html` and open it in any browser. Tasks are saved in the browser, so they stay after a refresh.' };
    T.calc = { m: 'calculator', n: 'A calculator with buttons.', lang: 'html', code: `<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Calculator</title>
<style>
  body { font-family: system-ui, sans-serif; display: grid; place-items: center; min-height: 100vh; margin: 0; background: #eef0fb; }
  .calc { width: 280px; background: #fff; padding: 16px; border-radius: 16px; box-shadow: 0 8px 30px rgba(0,0,0,.15); }
  #screen { width: 100%; box-sizing: border-box; font-size: 2rem; text-align: right; padding: 10px; margin-bottom: 12px; border: 0; background: #f3f4fb; border-radius: 10px; }
  .keys { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; }
  button { font-size: 1.2rem; padding: 14px 0; border: 0; border-radius: 10px; background: #e3e6fa; cursor: pointer; }
  button.op { background: #5b6bf5; color: #fff; }
</style>
</head>
<body>
<div class="calc">
  <input id="screen" readonly value="0">
  <div class="keys" id="keys"></div>
</div>
<script>
  var screen = document.getElementById("screen");
  var layout = ["7","8","9","/","4","5","6","*","1","2","3","-","0",".","=","+","C"];
  var text = "";
  layout.forEach(function (key) {
    var b = document.createElement("button");
    b.textContent = key;
    if ("/*-+=".indexOf(key) > -1) b.className = "op";
    b.onclick = function () { press(key); };
    document.getElementById("keys").appendChild(b);
  });
  function press(key) {
    if (key === "C") { text = ""; }
    else if (key === "=") {
      if (/^[0-9+\\-*/. ]+$/.test(text)) {
        try { text = String(Function("return " + text)()); } catch (e) { text = ""; }
      }
    } else { text += key; }
    screen.value = text || "0";
  }
<\/script>
</body>
</html>`, how: 'Save it as `calculator.html` and open it. The `=` button only runs text made of numbers and + - * / signs.' };
    T.counter = { m: 'counter|click counter', n: 'A click counter.', lang: 'html', code: `<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Counter</title>
<style>
  body { font-family: system-ui, sans-serif; display: grid; place-items: center; min-height: 100vh; margin: 0; }
  #n { font-size: 5rem; margin: 0; }
  button { font-size: 1.2rem; padding: 10px 20px; margin: 4px; }
</style>
</head>
<body>
<div style="text-align:center">
  <p id="n">0</p>
  <button id="minus">-1</button>
  <button id="plus">+1</button>
  <button id="reset">Reset</button>
</div>
<script>
  var count = 0;
  var out = document.getElementById("n");
  function show() { out.textContent = count; }
  document.getElementById("plus").onclick = function () { count++; show(); };
  document.getElementById("minus").onclick = function () { count--; show(); };
  document.getElementById("reset").onclick = function () { count = 0; show(); };
<\/script>
</body>
</html>`, how: 'Save it as `counter.html`. Good first project: change the step from 1 to 5.' };
    T.clock = { m: 'clock|digital clock|time display', n: 'A big digital clock.', lang: 'html', code: `<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Clock</title>
<style>
  body { margin: 0; min-height: 100vh; display: grid; place-items: center; background: #0b0e1a; color: #8fd3ff; font-family: system-ui, sans-serif; }
  #time { font-size: clamp(3rem, 14vw, 8rem); font-variant-numeric: tabular-nums; letter-spacing: .05em; }
  #date { text-align: center; opacity: .7; }
</style>
</head>
<body>
<div>
  <div id="time"></div>
  <div id="date"></div>
</div>
<script>
  function tick() {
    var now = new Date();
    document.getElementById("time").textContent = now.toLocaleTimeString();
    document.getElementById("date").textContent = now.toDateString();
  }
  tick();
  setInterval(tick, 1000);
<\/script>
</body>
</html>`, how: 'Save it as `clock.html`. Change the colours in the `<style>` block to make it yours.' };
    T.stopwatch = { m: 'stopwatch|timer|countdown', n: 'A stopwatch.', lang: 'html', code: `<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Stopwatch</title>
<style>
  body { font-family: system-ui, sans-serif; display: grid; place-items: center; min-height: 100vh; margin: 0; }
  #t { font-size: 4rem; font-variant-numeric: tabular-nums; text-align: center; }
  button { font-size: 1.1rem; padding: 10px 18px; margin: 4px; }
</style>
</head>
<body>
<div style="text-align:center">
  <div id="t">00:00.0</div>
  <button id="go">Start</button>
  <button id="reset">Reset</button>
</div>
<script>
  var running = false, start = 0, elapsed = 0, timer = null;
  var out = document.getElementById("t");
  function show() {
    var s = elapsed / 1000;
    var m = Math.floor(s / 60);
    var rest = (s - m * 60).toFixed(1);
    out.textContent = String(m).padStart(2, "0") + ":" + (rest < 10 ? "0" : "") + rest;
  }
  document.getElementById("go").onclick = function () {
    if (!running) {
      running = true; start = Date.now() - elapsed;
      timer = setInterval(function () { elapsed = Date.now() - start; show(); }, 100);
      this.textContent = "Pause";
    } else {
      running = false; clearInterval(timer);
      this.textContent = "Start";
    }
  };
  document.getElementById("reset").onclick = function () {
    running = false; clearInterval(timer); elapsed = 0; show();
    document.getElementById("go").textContent = "Start";
  };
<\/script>
</body>
</html>`, how: 'Save it as `stopwatch.html`. It uses the clock time, so it stays accurate even if the page lags.' };
    T.palette = { m: 'palette|colou?rs? (picker|generator)|random colou?rs?', n: 'A colour palette generator.', lang: 'html', code: `<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Palette</title>
<style>
  body { font-family: system-ui, sans-serif; margin: 0; min-height: 100vh; display: flex; flex-direction: column; }
  #row { flex: 1; display: flex; flex-wrap: wrap; }
  .sw { flex: 1 1 140px; min-height: 140px; display: flex; align-items: flex-end; padding: 12px; color: #fff; text-shadow: 0 1px 3px rgba(0,0,0,.6); }
  button { font-size: 1.1rem; padding: 14px; border: 0; background: #111; color: #fff; cursor: pointer; }
</style>
</head>
<body>
<div id="row"></div>
<button id="new">New palette</button>
<script>
  function make() {
    var row = document.getElementById("row");
    row.innerHTML = "";
    var base = Math.floor(Math.random() * 360);
    for (var i = 0; i < 5; i++) {
      var c = "hsl(" + ((base + i * 40) % 360) + ", 70%, " + (35 + i * 10) + "%)";
      var d = document.createElement("div");
      d.className = "sw";
      d.style.background = c;
      d.textContent = c;
      row.appendChild(d);
    }
  }
  document.getElementById("new").onclick = make;
  make();
<\/script>
</body>
</html>`, how: 'Save it as `palette.html`. The five colours step around the colour wheel by 40 degrees.' };
    T.loader = { m: 'loading animation|loader|spinner|css animation|loading', n: 'Two CSS loading animations.', lang: 'html', code: `<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Loaders</title>
<style>
  body { margin: 0; min-height: 100vh; display: flex; gap: 64px; align-items: center; justify-content: center; background: #10142b; }
  .spinner {
    width: 56px; height: 56px; border-radius: 50%;
    border: 6px solid rgba(255,255,255,.2); border-top-color: #7c8cff;
    animation: spin 0.9s linear infinite;
  }
  .dots { display: flex; gap: 8px; }
  .dots i { width: 14px; height: 14px; border-radius: 50%; background: #c084fc; animation: bounce 1s infinite ease-in-out; }
  .dots i:nth-child(2) { animation-delay: .15s; }
  .dots i:nth-child(3) { animation-delay: .3s; }
  @keyframes spin { to { transform: rotate(360deg); } }
  @keyframes bounce { 0%, 80%, 100% { transform: translateY(0); opacity: .4; } 40% { transform: translateY(-14px); opacity: 1; } }
  @media (prefers-reduced-motion: reduce) { .spinner, .dots i { animation: none; } }
</style>
</head>
<body>
<div class="spinner"></div>
<div class="dots"><i></i><i></i><i></i></div>
</body>
</html>`, how: 'Save it as `loaders.html`. `@keyframes` describes the motion and `animation` applies it. The last line turns it off for people who prefer less motion.' };
    T.fizzbuzz = { m: 'fizz ?buzz', n: 'FizzBuzz in Python.', lang: 'python', code: `for n in range(1, 101):
    if n % 15 == 0:
        print("FizzBuzz")
    elif n % 3 == 0:
        print("Fizz")
    elif n % 5 == 0:
        print("Buzz")
    else:
        print(n)`, how: 'Run it with `python fizzbuzz.py`. The `%` sign gives the remainder, so `n % 3 == 0` means n divides evenly by 3. Check 15 first, as it is both.' };
    T.rename = { m: 'renam(e|ing) files?|files? by date|organi[sz]e files', n: 'Rename files by date.', lang: 'python', code: `import os
from datetime import datetime

folder = "."   # change this to your folder path

for name in os.listdir(folder):
    path = os.path.join(folder, name)
    if not os.path.isfile(path):
        continue
    stamp = datetime.fromtimestamp(os.path.getmtime(path)).strftime("%Y-%m-%d")
    if name.startswith(stamp):
        continue
    new_name = stamp + "_" + name
    print("Renaming", name, "->", new_name)
    os.rename(path, os.path.join(folder, new_name))`, how: 'Test it on a copy of a folder first, because renaming cannot be undone. It puts each file\'s last modified date at the front of its name.' };
    T.password = { m: 'password (generator|maker)|generate.*password|random password', n: 'A password generator.', lang: 'python', code: `import secrets
import string

def make_password(length=16):
    alphabet = string.ascii_letters + string.digits + "!@#$%&*?"
    return "".join(secrets.choice(alphabet) for _ in range(length))

print(make_password())
print(make_password(24))`, how: 'It uses `secrets`, which is meant for passwords, instead of `random`, which is predictable.' };
    T.prime = { m: 'primes?|prime numbers?', n: 'Check for prime numbers.', lang: 'python', code: `def is_prime(n):
    if n < 2:
        return False
    for d in range(2, int(n ** 0.5) + 1):
        if n % d == 0:
            return False
    return True

print([n for n in range(1, 50) if is_prime(n)])`, how: 'You only need to test divisors up to the square root of n, which makes it much faster than testing everything.' };
    T.guess = { m: 'guess(ing)? (the )?number|number guessing', n: 'A number guessing game in Python.', lang: 'python', code: `import random

secret = random.randint(1, 100)
tries = 0

while True:
    guess = int(input("Guess a number from 1 to 100: "))
    tries += 1
    if guess < secret:
        print("Higher")
    elif guess > secret:
        print("Lower")
    else:
        print("Correct! It took you", tries, "tries.")
        break`, how: 'Run it with `python guess.py`. Challenge: add a limit of 7 tries.' };
    T.factorial = { m: 'factorial', n: 'Factorial with recursion.', lang: 'python', code: `def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

print(factorial(5))   # 120`, how: 'Each call multiplies n by the factorial of the number below it, until it reaches 1.' };
    T.bubble = { m: 'bubble sort|sort (an |the )?array|sort a list', n: 'Bubble sort in JavaScript.', lang: 'js', code: `function bubbleSort(list) {
  const a = list.slice();           // work on a copy
  let swapped = true;
  while (swapped) {
    swapped = false;
    for (let i = 0; i < a.length - 1; i++) {
      if (a[i] > a[i + 1]) {
        const temp = a[i];
        a[i] = a[i + 1];
        a[i + 1] = temp;
        swapped = true;
      }
    }
  }
  return a;
}

console.log(bubbleSort([5, 2, 9, 1, 7]));  // [1, 2, 5, 7, 9]`, how: 'Paste it into the browser Console (F12). In real projects use the built in `list.sort()`.' };
    T.reverse = { m: 'reverse (a |the )?(string|text|word)|palindrome', n: 'Reverse a string and check for palindromes.', lang: 'js', code: `function reverse(text) {
  return text.split("").reverse().join("");
}

function isPalindrome(text) {
  const clean = text.toLowerCase().replace(/[^a-z0-9]/g, "");
  return clean === reverse(clean);
}

console.log(reverse("hello"));            // olleh
console.log(isPalindrome("A man, a plan, a canal: Panama")); // true`, how: '`split` makes a list of letters, `reverse` flips it, and `join` glues it back together.' };
    T.hello = { m: 'hello world', n: 'Hello world.', lang: '', code: null, how: '' };
    var HELLO = {
      python: ['python', 'print("Hello, world!")'],
      javascript: ['js', 'console.log("Hello, world!");'],
      java: ['java', 'public class Main {\n  public static void main(String[] args) {\n    System.out.println("Hello, world!");\n  }\n}'],
      'c++': ['cpp', '#include <iostream>\nint main() {\n  std::cout << "Hello, world!" << std::endl;\n  return 0;\n}'],
      'c#': ['csharp', 'Console.WriteLine("Hello, world!");'],
      go: ['go', 'package main\nimport "fmt"\nfunc main() {\n  fmt.Println("Hello, world!")\n}'],
      rust: ['rust', 'fn main() {\n    println!("Hello, world!");\n}'],
      php: ['php', '<?php\necho "Hello, world!";'],
      ruby: ['ruby', 'puts "Hello, world!"'],
      html: ['html', '<h1>Hello, world!</h1>']
    };
    Object.keys(T).forEach(function (k) { T[k].re = new RegExp('(?:^|[^a-z0-9])(?:' + T[k].m + ')(?![a-z0-9])', 'i'); });
    function tplReply(key) {
      var t = T[key];
      return '**' + t.n + '**\n\n' + fence(t.lang, t.code) + '\n' + t.how;
    }
    function tplList() {
      return 'In Code mode I can write: a to-do app, a calculator, a counter, a digital clock, a stopwatch, a colour palette generator, CSS loading animations, FizzBuzz, a file renamer, a password generator, prime numbers, a number guessing game, factorial, bubble sort, string reversing, and hello world in many languages. I can also explain topics like loops, functions, arrays, recursion, APIs and Git.';
    }

    /* ----- helpers ----- */
    function mathReply(text) {
      var s = text.toLowerCase();
      var sq = s.match(/(?:square root of|sqrt\s*\(?)\s*(\d+(?:\.\d+)?)/);
      if (sq) return 'The square root of ' + sq[1] + ' is **' + fmt(Math.sqrt(parseFloat(sq[1]))) + '**.';
      var bin = s.match(/(?:what is |convert )?\b([01]{2,16})\b (?:in|to) (?:decimal|base 10)/);
      if (bin) return bin[1] + ' in binary is **' + parseInt(bin[1], 2) + '** in decimal.';
      var dec = s.match(/(?:what is |convert )?\b(\d{1,6})\b (?:in|to) binary/);
      if (dec) return dec[1] + ' in binary is **' + parseInt(dec[1], 10).toString(2) + '**.';
      var pc = s.match(/(\d+(?:\.\d+)?)\s*%\s*of\s*(\d+(?:\.\d+)?)/);
      if (pc) return pc[1] + '% of ' + pc[2] + ' is **' + fmt(parseFloat(pc[1]) / 100 * parseFloat(pc[2])) + '**.';
      var e = s.replace(/what('?s| is)|calculate|compute|solve|how much is|equals?|=|\?/g, ' ')
        .replace(/\bplus\b/g, '+').replace(/\bminus\b/g, '-').replace(/\b(times|multiplied by)\b/g, '*').replace(/(\d)\s*x\s*(\d)/g, '$1*$2')
        .replace(/\b(divided by|over)\b/g, '/').replace(/\bto the power of\b/g, '^').replace(/\bsquared\b/g, '^2').replace(/\bcubed\b/g, '^3')
        .replace(/,/g, '').trim();
      if (!/^[0-9+\-*/().%^\s]+$/.test(e) || !/\d/.test(e) || !/[+\-*/^%]/.test(e)) return null;
      try {
        var r = Function('"use strict";return (' + e.replace(/\^/g, '**') + ')')();
        if (typeof r !== 'number' || !isFinite(r)) return 'That one has no sensible answer. Did something divide by zero?';
        return e.replace(/\s+/g, ' ') + ' = **' + fmt(r) + '**';
      } catch (x) { return null; }
    }
    function fmt(n) { return String(parseFloat(n.toPrecision(12))); }
    function convReply(s) {
      var m = s.match(/(-?\d+(?:\.\d+)?)\s*(km|kilometers?|kilometres?|miles?|mi|kg|kilograms?|lbs?|pounds?|celsius|fahrenheit|c|f|meters?|metres?|m|feet|ft|cm|inch(?:es)?|in)\s*(?:to|in|into)\s*(km|kilometers?|kilometres?|miles?|mi|kg|kilograms?|lbs?|pounds?|celsius|fahrenheit|c|f|meters?|metres?|m|feet|ft|cm|inch(?:es)?|in)\b/i);
      if (!m) return null;
      function u(x) {
        x = x.toLowerCase();
        if (/^k(ilo)?m|^kilomet/.test(x)) return 'km';
        if (/^mi/.test(x)) return 'mi';
        if (/^(kg|kilogram)/.test(x)) return 'kg';
        if (/^(lb|pound)/.test(x)) return 'lb';
        if (/^(c|celsius)$/.test(x)) return 'c';
        if (/^(f|fahrenheit)$/.test(x)) return 'f';
        if (/^(m|meter|metre)/.test(x)) return 'm';
        if (/^(ft|feet)/.test(x)) return 'ft';
        if (/^cm/.test(x)) return 'cm';
        return 'in';
      }
      var v = parseFloat(m[1]), a = u(m[2]), b = u(m[3]), out = null;
      var key = a + '>' + b;
      var tab = { 'km>mi': v * 0.621371, 'mi>km': v / 0.621371, 'kg>lb': v * 2.20462, 'lb>kg': v / 2.20462, 'c>f': v * 9 / 5 + 32, 'f>c': (v - 32) * 5 / 9, 'm>ft': v * 3.28084, 'ft>m': v / 3.28084, 'cm>in': v / 2.54, 'in>cm': v * 2.54 };
      if (key in tab) out = tab[key];
      if (out === null) return null;
      return m[1] + ' ' + a + ' is about **' + fmt(Math.round(out * 1000) / 1000) + ' ' + b + '**.';
    }
    function planReply(low) {
      var m = low.match(/(?:for|on)\s+([a-z ,&]+?)(?:\.|$|\?| this| next| every)/);
      var subjects = m ? m[1].split(/,| and |&/).map(function (x) { return cap(x.trim()); }).filter(function (x) { return x && x.length < 24; }) : [];
      if (!subjects.length) subjects = ['Maths', 'Science', 'Reading'];
      var days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
      var lines = days.map(function (d, i) {
        if (i === 6) return '- **' + d + '**: rest, plus 15 minutes looking back over the week.';
        if (i === 5) return '- **' + d + '**: practice test or a project on ' + subjects[i % subjects.length] + ' (45 minutes).';
        var s = subjects[i % subjects.length], s2 = subjects[(i + 1) % subjects.length];
        return '- **' + d + '**: ' + s + ' for 30 minutes, then ' + s2 + ' for 20 minutes.';
      });
      return '**A simple study week** (about 50 minutes a day)\n\n' + lines.join('\n') + '\n\nTips: study in a quiet place, take a 5 minute break after 25 minutes, and write down questions to ask later. Tell me your subjects like "plan my study week for physics and history" and I will rebuild the plan.';
    }
    function messageReply(low) {
      if (/sorry|apolog|late|forgot|miss(ed)?\b/.test(low)) return '**An apology**\n\n```text\nHi [Name],\n\nI am sorry about [what happened]. I should have [what you should have done], and I understand it caused [the problem].\n\nTo fix it, I will [your plan] by [day]. Thank you for being patient with me.\n\nBest,\n[Your name]\n```\nFill in the brackets. Keep it short and say what you will do next.';
      if (/thank|grateful|appreciat/.test(low)) return '**A thank you**\n\n```text\nHi [Name],\n\nThank you so much for [what they did]. It really helped me [the result], and I appreciate the time you took.\n\nBest wishes,\n[Your name]\n```\nBeing specific about what helped makes thanks feel real.';
      if (/declin|say no|cannot|can\'?t come|turn down|refuse/.test(low)) return '**A polite no**\n\n```text\nHi [Name],\n\nThank you for inviting me to [the thing]. Unfortunately I cannot [take part / make it] because [short reason].\n\nI hope it goes really well, and please keep me in mind next time.\n\nBest,\n[Your name]\n```\nA short reason and a kind ending is enough. You do not need to over-explain.';
      return '**A polite request**\n\n```text\nHi [Name],\n\nI hope you are well. I am writing to ask whether you could [what you need] by [date].\n\nIt would help me [why it matters]. If it is easier, I am happy to [offer another way to do it].\n\nThank you for your time.\n\nBest,\n[Your name]\n```\nTell me if it is an apology, a thank you, or a polite no, and I will switch the template.';
    }
    function describeFile(f) {
      var t = f.text, lines = t.split('\n').length, w = (t.match(/\S+/g) || []).length;
      var ext = (f.name.split('.').pop() || '').toLowerCase();
      var langs = { js: 'JavaScript', py: 'Python', html: 'HTML', css: 'CSS', json: 'JSON', md: 'Markdown', csv: 'CSV', java: 'Java', c: 'C', cpp: 'C++', cs: 'C#', go: 'Go', rs: 'Rust', php: 'PHP', rb: 'Ruby', sh: 'shell script', sql: 'SQL', ts: 'TypeScript', txt: 'plain text' };
      var lang = langs[ext] || 'text';
      var out = '**' + f.name + '** is ' + lang + ': ' + lines + ' lines and ' + w + ' words.';
      if (ext === 'json') {
        try { var j = JSON.parse(t); out += ' It is valid JSON' + (j && typeof j === 'object' ? ' with ' + (Array.isArray(j) ? j.length + ' items' : Object.keys(j).length + ' top level keys: ' + Object.keys(j).slice(0, 6).join(', ')) : '') + '.'; }
        catch (e) { out += ' It is **not** valid JSON. Check for a missing comma, quote or bracket near the line the parser complains about.'; }
      } else if (ext === 'csv') {
        var rows = t.trim().split('\n'); out += ' The header row is: ' + rows[0].slice(0, 120) + ' (' + (rows.length - 1) + ' data rows).';
      } else if (/^(js|py|html|css|java|c|cpp|cs|go|rs|php|rb|sh|sql|ts)$/.test(ext)) {
        var fn = (t.match(/\bfunction\b|\bdef\b|=>|\bfn\b|\bfunc\b/g) || []).length;
        var todo = (t.match(/TODO|FIXME/g) || []).length;
        out += ' I count about ' + fn + ' function' + (fn === 1 ? '' : 's') + (todo ? ' and ' + todo + ' TODO or FIXME note' + (todo === 1 ? '' : 's') : '') + '.';
        var open = (t.match(/\{/g) || []).length, close = (t.match(/\}/g) || []).length;
        if (open !== close) out += ' The curly brackets do not match (' + open + ' open, ' + close + ' closed), which is a common cause of errors.';
      } else {
        var stop = ' the a an and or of to in is it that for on with as was are be this by at from have has i you we they not but your our'.split(' ');
        var freq = {};
        (t.toLowerCase().match(/[a-z]{4,}/g) || []).forEach(function (x) { if (stop.indexOf(x) < 0) freq[x] = (freq[x] || 0) + 1; });
        var top = Object.keys(freq).sort(function (a, b) { return freq[b] - freq[a]; }).slice(0, 5);
        if (top.length) out += ' Most used words: ' + top.join(', ') + '.';
      }
      var first = t.split('\n').filter(function (l) { return l.trim(); }).slice(0, 3).join(' / ');
      if (first) out += '\n\nIt starts with: "' + first.slice(0, 160) + '"';
      return out;
    }
    function fileReply(files, images) {
      var parts = [];
      images.forEach(function (im) {
        if (!im) { parts.push('I received a photo but could not read it.'); return; }
        parts.push('**' + im.name + '** is a ' + im.w + ' by ' + im.h + ' pixel ' + im.shape + ' image. It looks ' + im.tone + ' overall, and the main colours are ' + im.colors + '.');
      });
      if (images.length) parts.push('I can measure size, brightness and colours, but I cannot recognise what is in a photo, because I have no vision model. Tell me what it shows and I will help from there.');
      files.forEach(function (f) { parts.push(describeFile(f)); });
      parts.push('I do not summarise meaning, but I can explain any topic or code idea you ask about.');
      return parts.join('\n\n');
    }

    /* ----- smarter helpers ----- */
    function normKey(s) {
      var t = s.toLowerCase().replace(/[^a-z0-9' ]+/g, ' ').replace(/\s+/g, ' ').trim(), prev;
      do { prev = t; t = t.replace(/^(?:what is|what's|whats|what are|who is|who was|tell me about|explain|define|the|a|an)\s+/, ''); } while (t !== prev);
      return t;
    }
    function taughtFind(low, taught) {
      var q = normKey(low);
      if (!q) return null;
      for (var i = 0; i < taught.length; i++) {
        var k = taught[i].k;
        if (q === k) return taught[i].a;
        if (k.length >= 4 && (' ' + q + ' ').indexOf(' ' + k + ' ') > -1) return taught[i].a;
        if (k.length >= 5 && lev(q, k) <= (k.length > 8 ? 2 : 1)) return taught[i].a;
      }
      return null;
    }
    function captureFact(text) {
      var t = text.replace(/\s+/g, ' ').trim(), m;
      if (/\b(explain|tell me|show me|write|make me|build|how|what|why|help|can you|could you|please)\b/i.test(t) || /\?/.test(t)) return null;
      if ((m = t.match(/^my (?:favou?rite|fav) ([a-z ]{2,20}) is ([^.!?]{1,40})/i))) return { mem: 'Favourite ' + m[1].trim().toLowerCase() + ' is ' + m[2].trim(), say: 'your favourite ' + m[1].trim().toLowerCase() + ' is ' + m[2].trim() };
      if ((m = t.match(/^i (?:really |also |just )?(?:like|love|enjoy) (?!(?:your|you|it|this|that|the way)\b)([^.!?]{2,50})/i))) return { mem: 'Likes ' + m[1].trim(), say: 'you like ' + m[1].trim() };
      if ((m = t.match(/^i prefer ([^.!?]{2,50})/i))) return { mem: 'Prefers ' + m[1].trim(), say: 'you prefer ' + m[1].trim() };
      if ((m = t.match(/^i(?:'m| am) (?:currently )?(?:learning|studying) ([^.!?]{2,40})/i))) return { mem: 'Is learning ' + m[1].trim(), say: 'you are learning ' + m[1].trim() };
      if ((m = t.match(/^i live in ([^.!?]{2,40})/i))) return { mem: 'Lives in ' + m[1].trim(), say: 'you live in ' + m[1].trim() };
      if ((m = t.match(/^i(?:'m| am) from ([^.!?]{2,40})/i))) return { mem: 'Is from ' + m[1].trim(), say: 'you are from ' + m[1].trim() };
      if ((m = t.match(/^i(?:'m| am) (\d{1,2})(?: years old)?$/i))) return { mem: 'Is ' + m[1] + ' years old', say: 'you are ' + m[1] + ' years old' };
      if ((m = t.match(/^my (dog|cat|pet|brother|sister|friend|teacher|school|team) (?:is called|is named|name is|is) ([^.!?]{1,30})/i))) return { mem: 'Their ' + m[1].toLowerCase() + ' is ' + m[2].trim(), say: 'your ' + m[1].toLowerCase() + ' is ' + m[2].trim() };
      return null;
    }
    function recall(low, mem) {
      function find(re) { return mem.filter(function (x) { return re.test(x.text); }).map(function (x) { return x.text; }); }
      var m = low.match(/what(?:'s| is) my (?:favou?rite|fav) ([a-z ]+?)\s*\??$/), r;
      if (m) { r = find(new RegExp('favou?rite ' + m[1].trim(), 'i')); return r.length ? r[0] + '.' : 'You have not told me your favourite ' + m[1].trim() + ' yet. Say "my favourite ' + m[1].trim() + ' is ..." and I will keep it.'; }
      if (/where do i live|where am i from/.test(low)) { r = find(/^(lives in|is from)/i); return r.length ? r.join('. ') + '.' : 'I do not know yet. Tell me "I live in ..." and I will remember.'; }
      if (/how old am i|what(?:'s| is) my age/.test(low)) { r = find(/years old/i); return r.length ? r[0] + '.' : 'I do not know yet. Tell me "I am 14" and I will remember.'; }
      if (/what do i (?:like|love|enjoy)|what am i (?:learning|studying)/.test(low)) { r = find(/^(likes|is learning)/i); return r.length ? 'Here is what I know:\n\n' + r.slice(0, 10).map(function (x) { return '- ' + x; }).join('\n') : 'Nothing yet. Tell me "I like chess" or "I am learning Python".'; }
      return null;
    }

    var ERRS = [
      [/referenceerror: (\w+) is not defined/i, function (m) { return '`' + m[1] + '` is used before it exists. Check the spelling, make sure it was created with `let` or `const`, and that you use it where it is visible (see "scope").'; }],
      [/cannot read propert(?:y|ies) of (undefined|null)(?: \(reading '([^']+)'\))?/i, function (m) { return 'You asked for `' + (m[2] || 'a property') + '` of something that is `' + m[1] + '`. The thing before the dot was never set. Print it with `console.log` just above the error. Often an element was not found because its id is misspelled, or the script runs before the page has loaded.'; }],
      [/(\S+) is not a function/i, function (m) { return '`' + m[1] + '` is not a function. You are calling something that is a different kind of value. Check the spelling and capitalisation, and whether you meant to call it at all.'; }],
      [/assignment to constant variable/i, function () { return 'You changed a `const`. Use `let` for values that must change, or change a property inside the object instead of the whole variable.'; }],
      [/unexpected (?:token|end of input|identifier)|missing \) after argument list|invalid or unexpected token/i, function () { return 'The code has a typing mistake JavaScript cannot read. Look at the line named in the error and the one above it for a missing bracket, comma, closing quote or `)`.'; }],
      [/maximum call stack size exceeded|recursionerror/i, function () { return 'A function kept calling itself and never stopped. Recursive functions need a stopping case that does not call again (see "recursion").'; }],
      [/nameerror: name '(\w+)' is not defined/i, function (m) { return '`' + m[1] + '` was never created before you used it. Check the spelling, and make sure the line that creates it runs first. If it is a module, you need an `import`.'; }],
      [/indentationerror|unexpected indent|expected an indented block/i, function () { return 'Python uses spaces at the start of a line to show structure. After a line ending with `:` the next line must be indented, and lines in the same block must line up exactly. Do not mix tabs and spaces.'; }],
      [/syntaxerror: invalid syntax/i, function () { return 'Python cannot read that line. Look at it and the line above for a missing colon `:`, a missing bracket or quote, or a single `=` where you meant `==`.'; }],
      [/zerodivisionerror|division by zero/i, function () { return 'Something divided by zero. Check the value you divide by before the division, for example with an `if` that handles 0.'; }],
      [/indexerror: .*out of range/i, function () { return 'You asked for an item position that does not exist. Lists start at 0 and the last valid position is `len(list) - 1`. Check your loop limits.'; }],
      [/keyerror: ?'?([^'\n]+)'?/i, function (m) { return 'The dictionary has no key `' + m[1] + '`. Check the spelling, or use `.get("key")` which returns `None` instead of failing.'; }],
      [/can only concatenate str|unsupported operand type/i, function () { return 'You mixed types, such as adding text and a number. Convert first: `str(5)` turns a number into text, `int("5")` turns text into a number.'; }],
      [/modulenotfounderror: no module named '(\w+)'/i, function (m) { return 'Python cannot find the `' + m[1] + '` module. Check the spelling. If it is a third party package, it must be installed first with `pip install ' + m[1] + '`.'; }],
      [/filenotfounderror/i, function () { return 'The file path does not exist. Check the spelling and the folder you are running from. Printing `os.getcwd()` shows where Python is looking.'; }],
      [/valueerror: invalid literal for int/i, function () { return 'You tried to turn text that is not a whole number into an `int`. Check what the person typed, for example with `.isdigit()`.'; }],
      [/attributeerror: .* has no attribute '(\w+)'/i, function (m) { return 'That object has no `' + m[1] + '`. Check the spelling and what type the object really is by printing `type(x)`.'; }],
      [/cors|cross-origin/i, function () { return 'The browser blocked a request to another website, because that site has not allowed yours to read its data. It is a server setting, not a bug in your page. Use an API that allows it, or a small server in the middle.'; }],
      [/\b404\b.*(not found|error)|not found.*\b404\b/i, function () { return 'A 404 means the address does not exist. Check the spelling of the file name or URL, and that the file is in the folder you think it is. File names are case sensitive on many servers.'; }]
    ];
    function errReply(text) {
      for (var i = 0; i < ERRS.length; i++) { var m = text.match(ERRS[i][0]); if (m) return '**What that error means**\n\n' + ERRS[i][1](m) + '\n\nIf you paste the code around that line, I can look for the cause.'; }
      return null;
    }
    function langKey(l) {
      l = (l || '').toLowerCase();
      var map = { javascript: 'js', js: 'js', jsx: 'js', ts: 'js', typescript: 'js', node: 'js', py: 'python', python: 'python', html: 'html', htm: 'html', css: 'css' };
      return map[l] || '';
    }
    function guessLang(c) {
      if (/<!doctype|<html|<div|<body|<h1|<p>/i.test(c)) return 'html';
      if ((/\bdef \w+\(|^\s*import \w+|\bprint\(|\belif\b|^\s*from \w+ import/m.test(c)) && !/[;{}]\s*$/m.test(c)) return 'python';
      if (/^\s*[.#]?[\w-]+(?:\s*[.#:>][\w-]+)*\s*\{[^}]*:[^}]*;/m.test(c) && !/\b(function|const|let|var)\b/.test(c)) return 'css';
      return 'js';
    }
    function clip(s, n) { s = String(s).replace(/\s+/g, ' ').trim(); return s.length > n ? s.slice(0, n) + '...' : s; }
    function explainCode(code, lang) {
      var lines = code.split('\n'), notes = [], i, m;
      function add(n, t) { if (notes.length < 14) notes.push('- Line ' + n + ': ' + t); }
      if (lang === 'html') {
        var tags = {};
        (code.match(/<([a-z][a-z0-9]*)/gi) || []).forEach(function (t) { t = t.slice(1).toLowerCase(); tags[t] = 1; });
        var meaning = { h1: 'a main heading', h2: 'a sub heading', p: 'paragraphs', a: 'links', img: 'images', button: 'buttons', form: 'a form', input: 'input fields', ul: 'a list', li: 'list items', div: 'grouping boxes', span: 'inline text', style: 'CSS styling', script: 'JavaScript', canvas: 'a drawing area', table: 'a table', title: 'the tab title', select: 'a drop down' };
        var parts = Object.keys(tags).filter(function (n) { return meaning[n]; }).map(function (n) { return '`<' + n + '>` for ' + meaning[n]; });
        return '**What this HTML page does**\n\nIt uses ' + (parts.length ? parts.join(', ') : Object.keys(tags).length + ' kinds of tags') + '.' + (/<style/i.test(code) ? ' The `<style>` block controls how it looks.' : '') + (/<script/i.test(code) ? ' The `<script>` block holds the logic that runs when the page loads or when you click things.' : '') + '\n\nRead it top to bottom: structure first, then style, then script.';
      }
      if (lang === 'css') {
        var re = /([^{}]+)\{([^{}]*)\}/g;
        while ((m = re.exec(code)) && notes.length < 14) {
          var props = m[2].split(';').map(function (x) { return x.split(':')[0].trim(); }).filter(Boolean).slice(0, 5);
          notes.push('- `' + clip(m[1], 40) + '` is styled with: ' + props.join(', ') + '.');
        }
        return '**What this CSS does**\n\n' + (notes.join('\n') || 'I could not find any rules.') + '\n\nEach rule picks elements (the selector) and sets how they look.';
      }
      var py = lang === 'python';
      for (i = 0; i < lines.length; i++) {
        var L = lines[i].trim(), n = i + 1;
        if (!L || /^(\/\/|#|\*|\/\*)/.test(L)) continue;
        if (py) {
          if ((m = L.match(/^def (\w+)\(([^)]*)\)/))) add(n, 'defines a function `' + m[1] + '`' + (m[2].trim() ? ' that takes ' + m[2].trim() : ' with no inputs') + '.');
          else if ((m = L.match(/^class (\w+)/))) add(n, 'defines a class `' + m[1] + '`, a blueprint for objects.');
          else if ((m = L.match(/^(?:import|from) (\w+)/))) add(n, 'brings in the `' + m[1] + '` module so its tools can be used.');
          else if ((m = L.match(/^for (\w+) in (.+):/))) add(n, 'a loop: repeats the indented lines for each `' + m[1] + '` in `' + clip(m[2], 30) + '`.');
          else if ((m = L.match(/^while (.+):/))) add(n, 'repeats the indented lines while `' + clip(m[1], 30) + '` stays true.');
          else if ((m = L.match(/^(if|elif) (.+):/))) add(n, (m[1] === 'elif' ? 'otherwise, if' : 'checks whether') + ' `' + clip(m[2], 40) + '`; the indented lines run only when it is true.');
          else if (/^else:/.test(L)) add(n, 'otherwise runs the indented lines below.');
          else if ((m = L.match(/^return (.+)/))) add(n, 'sends `' + clip(m[1], 40) + '` back as the result.');
          else if ((m = L.match(/^print\((.*)\)/))) add(n, 'prints ' + (m[1] ? '`' + clip(m[1], 40) + '`' : 'an empty line') + ' to the screen.');
          else if (/\binput\(/.test(L)) add(n, 'asks the person to type something and stores the answer.');
          else if ((m = L.match(/^(\w+)\s*=\s*(.+)$/))) add(n, 'stores `' + clip(m[2], 40) + '` in a variable called `' + m[1] + '`.');
        } else {
          if ((m = L.match(/^(?:async\s+)?function\s+(\w+)\s*\(([^)]*)\)/))) add(n, 'defines a function `' + m[1] + '`' + (m[2].trim() ? ' that takes ' + m[2].trim() : ' with no inputs') + '.');
          else if ((m = L.match(/^class (\w+)/))) add(n, 'defines a class `' + m[1] + '`, a blueprint for objects.');
          else if ((m = L.match(/^(?:const|let|var)\s+(\w+)\s*=\s*(?:async\s*)?(?:function|\([^)]*\)\s*=>|\w+\s*=>)/))) add(n, 'creates a function stored in `' + m[1] + '`.');
          else if ((m = L.match(/^(const|let|var)\s+(\w+)\s*=\s*(.+?);?$/))) add(n, 'creates ' + (m[1] === 'const' ? 'a constant' : 'a variable') + ' `' + m[2] + '` holding `' + clip(m[3], 40) + '`.');
          else if ((m = L.match(/^for\s*\((.*)\)/))) add(n, 'a `for` loop `(' + clip(m[1], 40) + ')`: repeats the block below.');
          else if ((m = L.match(/^while\s*\((.*)\)/))) add(n, 'repeats the block below while `' + clip(m[1], 40) + '` stays true.');
          else if ((m = L.match(/^(?:\}\s*)?(else if|if)\s*\((.*)\)/))) add(n, (m[1] === 'if' ? 'checks whether' : 'otherwise, if') + ' `' + clip(m[2], 40) + '`; the block runs only when it is true.');
          else if (/^\}?\s*else\b/.test(L)) add(n, 'otherwise runs this block.');
          else if ((m = L.match(/^return (.+?);?$/))) add(n, 'sends `' + clip(m[1], 40) + '` back as the result.');
          else if ((m = L.match(/console\.log\((.*)\)/))) add(n, 'prints `' + clip(m[1], 40) + '` to the console.');
          else if ((m = L.match(/document\.(getElementById|querySelector)\(([^)]+)\)/))) add(n, 'finds the page element ' + m[2] + '.');
          else if ((m = L.match(/addEventListener\(\s*["'](\w+)["']/))) add(n, 'runs code when the `' + m[1] + '` event happens.');
          else if ((m = L.match(/\.on(click|submit|change|input|load)\s*=/))) add(n, 'runs code on the `' + m[1] + '` event.');
          else if (/setInterval\(/.test(L)) add(n, 'repeats a function again and again on a timer.');
          else if (/setTimeout\(/.test(L)) add(n, 'runs a function once after a delay.');
          else if (/localStorage\./.test(L)) add(n, 'saves or reads data in the browser.');
          else if (/\.push\(/.test(L)) add(n, 'adds an item to the end of a list.');
        }
      }
      return '**What this ' + (py ? 'Python' : 'JavaScript') + ' code does**\n\n' + (notes.length ? notes.join('\n') : 'I could not recognise the lines.') + (lines.length > 40 ? '\n\nThis is a long file, so I only list the first notable lines.' : '') + '\n\nI read common patterns line by line. For the big picture, start with the function names.';
    }
    function lintCode(code, lang) {
      var issues = [], lines = code.split('\n'), i, m;
      if (lang === 'html') {
        ['div', 'span', 'p', 'ul', 'ol', 'li', 'table', 'tr', 'td', 'h1', 'h2', 'h3', 'a', 'button', 'form', 'section', 'header', 'footer', 'main', 'nav', 'script', 'style', 'body', 'head', 'html', 'select'].forEach(function (t) {
          var o = (code.match(new RegExp('<' + t + '(?=[\\s>])', 'gi')) || []).length, c = (code.match(new RegExp('</' + t + '>', 'gi')) || []).length;
          if (o !== c) issues.push('There are ' + o + ' `<' + t + '>` tags opened but ' + c + ' closed.');
        });
        var sc = code.replace(/<script[\s\S]*?<\/script>/gi, '');
        if ((sc.match(/"/g) || []).length % 2) issues.push('An odd number of double quotes in the HTML, so an attribute may be missing its closing quote.');
        return issues;
      }
      var py = lang === 'python';
      var cleanLine = function (l) { return l.replace(/"(?:\\.|[^"\\])*"|'(?:\\.|[^'\\])*'|`(?:\\.|[^`\\])*`/g, '""').replace(py ? /#.*$/ : /\/\/.*$/, ''); };
      var s = lines.map(cleanLine).join('\n').replace(/\/\*[\s\S]*?\*\//g, '');
      [['(', ')'], ['{', '}'], ['[', ']']].forEach(function (p) {
        var o = s.split(p[0]).length - 1, c = s.split(p[1]).length - 1;
        if (o !== c) issues.push('There ' + (Math.abs(o - c) === 1 ? 'is one' : 'are ' + Math.abs(o - c)) + ' ' + (o > c ? 'more `' + p[0] + '` than `' + p[1] + '`' : 'more `' + p[1] + '` than `' + p[0] + '`') + ', so something is not closed properly.');
      });
      var tabs = false, spaces = false;
      for (i = 0; i < lines.length; i++) {
        var raw = lines[i], L = cleanLine(raw), n = i + 1, t = L.trim();
        if (!t) continue;
        if (/["']/.test(L) && (L.match(/["']/g) || []).length % 2 && !/^\s*(#|\/\/)/.test(raw)) issues.push('Line ' + n + ' may have a quote that is never closed.');
        if (/^\t/.test(raw)) tabs = true;
        if (/^ {2,}/.test(raw)) spaces = true;
        if (py) {
          if (/^(if|elif|else|for|while|def|class|try|except|finally|with)\b/.test(t) && !/:\s*$/.test(t) && !/[(\[{,\\]\s*$/.test(t) && !/:\s*\S/.test(t)) issues.push('Line ' + n + ' starts a block but does not end with a colon `:`.');
          if ((m = t.match(/^(if|elif|while)\s+(.+?):?$/)) && /[^=!<>]=[^=]/.test(m[2].replace(/==|!=|<=|>=/g, ''))) issues.push('Line ' + n + ' uses a single `=` in a condition. To compare, use `==`.');
          if (/^print\s+["'\w]/.test(t)) issues.push('Line ' + n + ' uses `print` without brackets, which is Python 2 style. Use `print(...)`.');
        } else {
          if (/\b(if|while)\s*\([^()]*[^=!<>=]=[^=][^()]*\)/.test(t)) issues.push('Line ' + n + ' uses a single `=` inside a condition. That assigns a value. To compare, use `===`.');
          if (/<=\s*\w+\.length\b/.test(t)) issues.push('Line ' + n + ' loops with `<=` and `.length`, which goes one step past the end. Use `<` instead.');
          if (/\bvar\s+\w/.test(t)) issues.push('Line ' + n + ' uses `var`. Modern code uses `let` or `const`.');
        }
      }
      if (py && tabs && spaces) issues.push('The file mixes tabs and spaces for indentation, which Python rejects. Pick one.');
      return issues.slice(0, 10);
    }

    function nums(s) { return (s.match(/-?\d+(?:\.\d+)?/g) || []).map(Number); }
    function gcd(x, y) { x = Math.abs(x); y = Math.abs(y); while (y) { var t = y; y = x % y; x = t; } return x; }
    /* ----- scientific calculator and word problems ----- */
    function sciEval(src, deg) {
      var toks = [], re = /\s*(?:(\d+\.?\d*(?:e[+-]?\d+)?|\.\d+)|([a-z]+)|(\*\*|[-+*\/^%!(),]))/gy, mt, pos = 0;
      src = src.trim();
      while (pos < src.length) {
        re.lastIndex = pos; mt = re.exec(src);
        if (!mt) { if (/^\s*$/.test(src.slice(pos))) break; throw new Error('bad'); }
        pos = re.lastIndex;
        if (mt[1] !== undefined) toks.push({ t: 'n', v: parseFloat(mt[1]) });
        else if (mt[2] !== undefined) toks.push({ t: 'i', v: mt[2] });
        else toks.push({ t: 'o', v: mt[3] === '**' ? '^' : mt[3] });
      }
      var p = 0;
      function peek() { return toks[p]; }
      function eat(v) { var k = toks[p]; if (k && k.t === 'o' && k.v === v) { p++; return true; } return false; }
      var CONST = { pi: Math.PI, e: Math.E, tau: 2 * Math.PI, phi: (1 + Math.sqrt(5)) / 2 };
      var toR = function (x) { return deg ? x * Math.PI / 180 : x; }, fromR = function (x) { return deg ? x * 180 / Math.PI : x; };
      var FN = {
        sqrt: Math.sqrt, cbrt: Math.cbrt, abs: Math.abs, exp: Math.exp, ln: Math.log, log: Math.log10, log10: Math.log10, log2: Math.log2,
        floor: Math.floor, ceil: Math.ceil, round: Math.round, sign: Math.sign,
        sin: function (x) { return Math.sin(toR(x)); }, cos: function (x) { return Math.cos(toR(x)); }, tan: function (x) { return Math.tan(toR(x)); },
        asin: function (x) { return fromR(Math.asin(x)); }, acos: function (x) { return fromR(Math.acos(x)); }, atan: function (x) { return fromR(Math.atan(x)); },
        sinh: Math.sinh, cosh: Math.cosh, tanh: Math.tanh,
        fact: function (x) { return factN(x); }
      };
      function factN(x) { if (x < 0 || x !== Math.floor(x) || x > 170) throw new Error('fact'); var r = 1; for (var i = 2; i <= x; i++) r *= i; return r; }
      function expr() { var v = term(); for (;;) { if (eat('+')) v += term(); else if (eat('-')) v -= term(); else return v; } }
      function term() {
        var v = unary();
        for (;;) {
          if (eat('*')) v *= unary();
          else if (eat('/')) { var d = unary(); v /= d; }
          else if (eat('%')) { var m2 = unary(); v = v % m2; }
          else { var k = peek(); if (k && k.t === 'i' && k.v === 'mod') { p++; v = v % unary(); } else if (k && (k.t === 'n' || (k.t === 'o' && k.v === '(') || (k.t === 'i' && (CONST[k.v] !== undefined || FN[k.v])))) { v *= unary(); } else return v; }
        }
      }
      function unary() { if (eat('-')) return -unary(); if (eat('+')) return unary(); return power(); }
      function power() { var b = post(); if (eat('^')) { var e2 = unary(); return Math.pow(b, e2); } return b; }
      function post() { var v = atom(); while (eat('!')) v = factN(v); return v; }
      function atom() {
        var k = toks[p++];
        if (!k) throw new Error('end');
        if (k.t === 'n') return k.v;
        if (k.t === 'o' && k.v === '(') { var v = expr(); if (!eat(')')) throw new Error('paren'); return v; }
        if (k.t === 'i') {
          if (CONST[k.v] !== undefined) return CONST[k.v];
          if (FN[k.v]) {
            var arg;
            if (eat('(')) { arg = expr(); if (!eat(')')) throw new Error('paren'); } else arg = power();
            return FN[k.v](arg);
          }
        }
        throw new Error('tok');
      }
      var r = expr();
      if (p < toks.length) throw new Error('trail');
      return r;
    }
    function sciReply(low) {
      var m, deg = /degrees?|°/.test(low);
      var s = low.replace(/°/g, ' ').replace(/\bdegrees?\b/g, ' ').replace(/\s+/g, ' ').replace(/\(\s+/g, '(').replace(/\s+\)/g, ')');
      s = s.replace(/^(?:please\s+)?(?:what(?:'s| is| are)?|calculate|compute|evaluate|work out|find|solve|simplify|how much is|tell me|give me)\s+(?:the\s+value\s+of\s+|the\s+answer\s+to\s+)?/, '')
        .replace(/\?+$/, '').replace(/=\s*$/, '').trim();
      s = s.replace(/square root of\s*/g, 'sqrt ').replace(/cube root of\s*/g, 'cbrt ').replace(/\bsqrt of\s*/g, 'sqrt ')
        .replace(/\bfactorial of\s*(\d+)/g, '$1!').replace(/\blog base 2 of\s*/g, 'log2 ').replace(/\blog of\s*/g, 'log ').replace(/\bnatural log(?:arithm)? of\s*/g, 'ln ')
        .replace(/\babsolute value of\s*/g, 'abs ').replace(/\bplus\b/g, '+').replace(/\bminus\b/g, '-').replace(/\b(?:times|multiplied by)\b/g, '*')
        .replace(/\b(?:divided by|over)\b/g, '/').replace(/\bto the power of\b/g, '^').replace(/\bsquared\b/g, '^2').replace(/\bcubed\b/g, '^3')
        .replace(/\bmodulo\b/g, 'mod').replace(/(\d)\s*x\s*(\d)/g, '$1*$2').replace(/(\d),(?=\d{3}\b)/g, '$1');
      if (!/[a-z]/.test(s) && !/[-+*\/^%!]/.test(s.replace(/^-/, ''))) return null;
      if (/^\d+(?:\.\d+)?$/.test(s)) return null;
      var hasFn = /\b(sqrt|cbrt|sin|cos|tan|asin|acos|atan|sinh|cosh|tanh|log|log2|log10|ln|exp|abs|floor|ceil|round|sign|pi|e|tau|phi|mod)\b/.test(s);
      if (/[a-z]/.test(s.replace(/\b(sqrt|cbrt|sin|cos|tan|asin|acos|atan|sinh|cosh|tanh|log|log2|log10|ln|exp|abs|floor|ceil|round|sign|pi|e|tau|phi|mod)\b/g, ''))) return null;
      if (!hasFn && !/[-+*\/^%!]/.test(s)) return null;
      try {
        var r = sciEval(s, deg);
        if (typeof r !== 'number' || isNaN(r)) return null;
        if (!isFinite(r)) return 'That one has no sensible answer. Did something divide by zero?';
        var shown = Math.abs(r) >= 1e15 || (Math.abs(r) < 1e-9 && r !== 0) ? r.toExponential(8).replace(/\.?0+e/, 'e') : fmt(r);
        return '**' + s.replace(/\*/g, ' x ').replace(/\s+/g, ' ').trim() + (deg ? ' (degrees)' : '') + ' = ' + shown + '**' + (/sin|cos|tan/.test(s) && !deg ? '\n\nTip: angles are in radians. Add "degrees" to use degrees, like "sin 30 degrees".' : '');
      } catch (e) { return null; }
    }
    function wordMath(low) {
      var m, n = function (x) { return parseFloat(x); };
      /* two equations, two unknowns */
      m = low.replace(/\s+/g, '').match(/([+-]?\d*\.?\d*)x([+-]\d*\.?\d*)y=([+-]?\d+\.?\d*)(?:,|and|;)+([+-]?\d*\.?\d*)x([+-]\d*\.?\d*)y=([+-]?\d+\.?\d*)/);
      if (m) {
        var c = function (v) { return v === '' || v === '+' ? 1 : v === '-' ? -1 : n(v); };
        var a1 = c(m[1]), b1 = c(m[2]), c1 = n(m[3]), a2 = c(m[4]), b2 = c(m[5]), c2 = n(m[6]), det = a1 * b2 - a2 * b1;
        if (!det) return 'Those two equations are parallel or the same line, so there is no single answer.';
        var xs = (c1 * b2 - c2 * b1) / det, ys = (a1 * c2 - a2 * c1) / det;
        return 'Solving the pair of equations (using elimination):\n\n**x = ' + fmt(xs) + '**\n**y = ' + fmt(ys) + '**\n\nCheck: ' + fmt(a1) + ' x ' + fmt(xs) + ' + ' + fmt(b1) + ' x ' + fmt(ys) + ' = ' + fmt(a1 * xs + b1 * ys) + '.';
      }
      if ((m = low.match(/(?:area of (?:a )?circle|circle area).*?(?:radius|r)\s*(?:of|=|is)?\s*(\d+\.?\d*)/)) || (m = low.match(/radius\s*(?:of|=|is)?\s*(\d+\.?\d*).*?area of (?:the |a )?circle/))) { var r = n(m[1]); return 'Area of a circle = pi x r^2 = pi x ' + fmt(r) + '^2 = **' + fmt(Math.PI * r * r) + '**.'; }
      if ((m = low.match(/circumference.*?(?:radius|r)\s*(?:of|=|is)?\s*(\d+\.?\d*)/))) { var r2 = n(m[1]); return 'Circumference = 2 x pi x r = 2 x pi x ' + fmt(r2) + ' = **' + fmt(2 * Math.PI * r2) + '**.'; }
      if ((m = low.match(/volume of (?:a )?sphere.*?(?:radius|r)\s*(?:of|=|is)?\s*(\d+\.?\d*)/))) { var r3 = n(m[1]); return 'Volume of a sphere = 4/3 x pi x r^3 = **' + fmt(4 / 3 * Math.PI * r3 * r3 * r3) + '**.'; }
      if ((m = low.match(/volume of (?:a )?cylinder.*?radius\s*(?:of|=|is)?\s*(\d+\.?\d*).*?height\s*(?:of|=|is)?\s*(\d+\.?\d*)/))) { return 'Volume of a cylinder = pi x r^2 x h = **' + fmt(Math.PI * n(m[1]) * n(m[1]) * n(m[2])) + '**.'; }
      if ((m = low.match(/volume of (?:a )?cube.*?(?:side|edge|length)\s*(?:of|=|is)?\s*(\d+\.?\d*)/))) { var sd = n(m[1]); return 'Volume of a cube = side^3 = **' + fmt(sd * sd * sd) + '**.'; }
      if ((m = low.match(/area of (?:a )?rectangle.*?(\d+\.?\d*)\s*(?:by|x|and|\*)\s*(\d+\.?\d*)/))) return 'Area of a rectangle = width x height = **' + fmt(n(m[1]) * n(m[2])) + '**, and the perimeter is **' + fmt(2 * (n(m[1]) + n(m[2]))) + '**.';
      if ((m = low.match(/area of (?:a )?triangle.*?base\s*(?:of|=|is)?\s*(\d+\.?\d*).*?height\s*(?:of|=|is)?\s*(\d+\.?\d*)/))) return 'Area of a triangle = half x base x height = **' + fmt(0.5 * n(m[1]) * n(m[2])) + '**.';
      if ((m = low.match(/area of (?:a )?square.*?(?:side|length)?\s*(?:of|=|is)?\s*(\d+\.?\d*)/))) { var sq = n(m[1]); return 'Area of a square = side^2 = **' + fmt(sq * sq) + '**, and the perimeter is **' + fmt(4 * sq) + '**.'; }
      if ((m = low.match(/(?:hypotenuse|pythagoras|diagonal).*?(\d+\.?\d*)\D+(\d+\.?\d*)/))) { var h = Math.sqrt(n(m[1]) * n(m[1]) + n(m[2]) * n(m[2])); return 'Pythagoras: sqrt(' + fmt(n(m[1])) + '^2 + ' + fmt(n(m[2])) + '^2) = **' + fmt(h) + '**.'; }
      if ((m = low.match(/(\d+\.?\d*)\s*(km|kilometers?|kilometres?|miles?|metres?|meters?|m)\b.*?\b(?:in|takes?|took|over|within)\s*(\d+\.?\d*)\s*(hours?|hrs?|minutes?|mins?|seconds?|secs?)/)) && /speed|fast|velocity|how quick/.test(low)) {
        var dist = n(m[1]), t = n(m[3]), un = /^(hour|hr)/.test(m[4]) ? 'h' : /^min/.test(m[4]) ? 'min' : 's', du = /^k/.test(m[2]) ? 'km' : /^mi/.test(m[2]) ? 'miles' : 'm';
        return 'Speed = distance / time = ' + fmt(dist) + ' ' + du + ' / ' + fmt(t) + ' ' + un + ' = **' + fmt(dist / t) + ' ' + du + ' per ' + un + '**.';
      }
      if ((m = low.match(/(?:speed|velocity)\s*(?:of|=|is)?\s*(\d+\.?\d*)\s*(?:km\/h|kph|mph|m\/s)?.*?(?:for|time|in)\s*(\d+\.?\d*)\s*(?:hours?|hrs?|h|seconds?|s)\b/)) && /distance|how far/.test(low)) return 'Distance = speed x time = ' + fmt(n(m[1])) + ' x ' + fmt(n(m[2])) + ' = **' + fmt(n(m[1]) * n(m[2])) + '**.';
      if ((m = low.match(/simple interest.*?(\d+\.?\d*)\D+?(\d+\.?\d*)\s*%\D+?(\d+\.?\d*)\s*(?:years?|yrs?)/))) { var P = n(m[1]), R = n(m[2]), T = n(m[3]); return 'Simple interest = P x R x T / 100 = ' + fmt(P) + ' x ' + fmt(R) + ' x ' + fmt(T) + ' / 100 = **' + fmt(P * R * T / 100) + '**, so the total is **' + fmt(P + P * R * T / 100) + '**.'; }
      if ((m = low.match(/compound interest.*?(\d+\.?\d*)\D+?(\d+\.?\d*)\s*%\D+?(\d+\.?\d*)\s*(?:years?|yrs?)/))) { var P2 = n(m[1]), R2 = n(m[2]), T2 = n(m[3]), tot = P2 * Math.pow(1 + R2 / 100, T2); return 'Compound interest (once a year): total = P x (1 + R/100)^T = **' + fmt(Math.round(tot * 100) / 100) + '**, so the interest is **' + fmt(Math.round((tot - P2) * 100) / 100) + '**.'; }
      if ((m = low.match(/what percent(?:age)? (?:is|of)\s*(\d+\.?\d*)\s*(?:of|out of|from)\s*(\d+\.?\d*)/)) || (m = low.match(/(\d+\.?\d*)\s*is what percent(?:age)? of\s*(\d+\.?\d*)/))) { if (n(m[2])) return fmt(n(m[1])) + ' is **' + fmt(n(m[1]) / n(m[2]) * 100) + '%** of ' + fmt(n(m[2])) + '.'; }
      if ((m = low.match(/(?:average|mean) of (?:my )?(?:marks?|scores?|grades?)[: ]+([\d\s,.and]+)/))) { var arr = (m[1].match(/\d+\.?\d*/g) || []).map(n); if (arr.length > 1) return 'Your average is **' + fmt(arr.reduce(function (a, b) { return a + b; }, 0) / arr.length) + '**.'; }
      return null;
    }

    function extraMath(low) {
      var m, a;
      if ((m = low.match(/(?:derivative|differentiate|d\/dx)\s*(?:of|:)?\s*([0-9x^+\-*\s.]+?)\s*\??$/)) && /x/.test(m[1])) {
        var expr = m[1].replace(/\s+/g, '').replace(/-/g, '+-'), terms = expr.split('+').filter(Boolean), outT = [], okd = true;
        terms.forEach(function (t) {
          var tm = t.match(/^(-?\d*\.?\d*)\*?(x)?(?:\^(-?\d+))?$/);
          if (!tm) { okd = false; return; }
          var c = tm[1] === '' || tm[1] === undefined ? 1 : tm[1] === '-' ? -1 : parseFloat(tm[1]);
          if (!tm[2]) return;
          var p = tm[3] === undefined ? 1 : parseInt(tm[3], 10), nc = c * p, np = p - 1;
          if (nc === 0) return;
          outT.push(np === 0 ? String(nc) : (nc === 1 ? '' : nc === -1 ? '-' : nc) + 'x' + (np === 1 ? '' : '^' + np));
        });
        if (okd) return 'The derivative of **' + m[1].trim() + '** is **' + (outT.length ? outT.join(' + ').replace(/\+ -/g, '- ') : '0') + '**.\n\nRule used: the derivative of c x^n is c n x^(n-1).';
      }
      if ((m = low.match(/\b(average|mean|median|mode|sum|total|range)\b.*?\b(?:of|for)\b\s*([\d\s,.\-and]+)/))) {
        a = nums(m[2]);
        if (a.length >= 2) {
          var sum = a.reduce(function (x, y) { return x + y; }, 0), sorted = a.slice().sort(function (x, y) { return x - y; });
          var mid = sorted.length % 2 ? sorted[(sorted.length - 1) / 2] : (sorted[sorted.length / 2 - 1] + sorted[sorted.length / 2]) / 2;
          var counts = {};
          a.forEach(function (x) { counts[x] = (counts[x] || 0) + 1; });
          var top = Math.max.apply(null, Object.keys(counts).map(function (k) { return counts[k]; }));
          var modes = Object.keys(counts).filter(function (k) { return counts[k] === top; });
          return 'For ' + a.join(', ') + ':\n\n- sum: **' + fmt(sum) + '**\n- mean (average): **' + fmt(sum / a.length) + '**\n- median: **' + fmt(mid) + '**\n- mode: **' + (top === 1 ? 'none, every number appears once' : modes.join(', ')) + '**\n- range: **' + fmt(sorted[sorted.length - 1] - sorted[0]) + '**';
        }
      }
      if ((m = low.match(/\b(gcd|hcf|greatest common (?:divisor|factor)|lcm|least common multiple)\b.*?(\d+)\D+(\d+)/))) {
        var p = parseInt(m[2], 10), q = parseInt(m[3], 10), g = gcd(p, q);
        return /^(lcm|least)/.test(m[1]) ? 'The least common multiple of ' + p + ' and ' + q + ' is **' + (g ? p / g * q : 0) + '**.' : 'The greatest common divisor of ' + p + ' and ' + q + ' is **' + g + '**.';
      }
      if ((m = low.match(/\bis (\d{1,12}) (?:a )?prime/))) {
        var n = parseInt(m[1], 10), pr = n > 1;
        for (var d = 2; d * d <= n && pr; d++) if (n % d === 0) { pr = false; var why = d; }
        return pr ? n + ' is **prime**.' : n + ' is **not prime**' + (n > 1 ? ': it divides by ' + why + ' (' + n + ' = ' + why + ' x ' + (n / why) + ').' : '.');
      }
      if ((m = low.match(/prime factors? of (\d{1,12})/))) {
        var v = parseInt(m[1], 10), fs = [], x = v;
        for (var f = 2; f * f <= x; f++) while (x % f === 0) { fs.push(f); x /= f; }
        if (x > 1) fs.push(x);
        return v < 2 ? v + ' has no prime factors.' : 'The prime factors of ' + v + ' are **' + fs.join(' x ') + '**' + (fs.length === 1 ? ', because it is prime.' : '.');
      }
      if ((m = low.match(/\bfactors? of (\d{1,7})\b/))) {
        var w = parseInt(m[1], 10), list = [];
        for (var k = 1; k <= w; k++) if (w % k === 0) list.push(k);
        return 'The factors of ' + w + ' are ' + list.join(', ') + '.';
      }
      if ((m = low.match(/factorial of (\d{1,3})\b|\b(\d{1,3})\s*!(?!=)/))) {
        var nf = parseInt(m[1] || m[2], 10);
        if (nf > 100) return 'That factorial is enormous. Try 100 or less.';
        var r = 1n;
        for (var z = 2n; z <= BigInt(nf); z++) r *= z;
        return nf + '! = **' + r.toString() + '**';
      }
      if (/fibonacci/.test(low) && (a = nums(low)).length) {
        var cnt = Math.min(200, Math.max(0, Math.floor(a[0]))), seq = [0n, 1n];
        for (var y = 2; y <= cnt + 1; y++) seq.push(seq[y - 1] + seq[y - 2]);
        if (/first|numbers|sequence|series|list/.test(low)) return 'The first ' + Math.min(cnt, 40) + ' Fibonacci numbers:\n\n' + seq.slice(0, Math.min(cnt, 40)).join(', ');
        return 'Fibonacci number ' + cnt + ' is **' + seq[cnt].toString() + '** (counting 0 as number 0).';
      }
      var eq = low.replace(/^(?:please\s+)?(?:solve|find|what is|calculate)\s*(?:for\s+[a-z]\s*)?:?/, '').replace(/\s+/g, '');
      if (eq.indexOf('=') > -1) {
        var qd = eq.match(/^([+-]?\d*\.?\d*)([a-z])\^2([+-]\d*\.?\d*)\2([+-]\d+\.?\d*)?=0$/);
        if (qd) {
          var co = function (s) { return s === '' || s === '+' ? 1 : s === '-' ? -1 : parseFloat(s); };
          var qa = co(qd[1]), qb = co(qd[3]), qc = qd[4] ? parseFloat(qd[4]) : 0, disc = qb * qb - 4 * qa * qc;
          if (!qa) return null;
          if (disc < 0) return 'The equation has no real solutions, because the discriminant (b squared minus 4ac = ' + fmt(disc) + ') is negative.';
          var r1 = (-qb + Math.sqrt(disc)) / (2 * qa), r2 = (-qb - Math.sqrt(disc)) / (2 * qa);
          return 'Using the quadratic formula (discriminant ' + fmt(disc) + '):\n\n**' + qd[2] + ' = ' + fmt(r1) + '**' + (disc === 0 ? ' (a double root)' : ' or **' + qd[2] + ' = ' + fmt(r2) + '**');
        }
        var ln = eq.match(/^([+-]?\d*\.?\d*)([a-z])([+-]\d+\.?\d*)?=([+-]?\d+\.?\d*)$/);
        if (ln) {
          var ca = ln[1] === '' || ln[1] === '+' ? 1 : ln[1] === '-' ? -1 : parseFloat(ln[1]), cb = ln[3] ? parseFloat(ln[3]) : 0, cc = parseFloat(ln[4]);
          if (!ca) return null;
          var rhs = cc - cb;
          return 'Solve for ' + ln[2] + ':\n\n- Subtract ' + fmt(cb) + ' from both sides: ' + fmt(ca) + ln[2] + ' = ' + fmt(rhs) + '\n- Divide by ' + fmt(ca) + ': **' + ln[2] + ' = ' + fmt(rhs / ca) + '**';
        }
      }
      if ((m = low.match(/\b(\d{1,12}) (?:in|to) hex(?:adecimal)?\b/))) return m[1] + ' in hexadecimal is **' + parseInt(m[1], 10).toString(16).toUpperCase() + '**.';
      if ((m = low.match(/(\d+(?:\.\d+)?)\s*%\s*(?:off|discount)\s*(?:of|on)?\s*\$?(\d+(?:\.\d+)?)/))) { var pc = parseFloat(m[1]), pr2 = parseFloat(m[2]); return m[1] + '% off ' + m[2] + ' saves **' + fmt(pr2 * pc / 100) + '**, so you pay **' + fmt(pr2 * (1 - pc / 100)) + '**.'; }
      if ((m = low.match(/(\d+(?:\.\d+)?)\s*%\s*tip\s*(?:on|of)\s*\$?(\d+(?:\.\d+)?)/))) { var tp = parseFloat(m[1]) * parseFloat(m[2]) / 100; return 'A ' + m[1] + '% tip on ' + m[2] + ' is **' + fmt(tp) + '**, for a total of **' + fmt(parseFloat(m[2]) + tp) + '**.'; }
      if ((m = low.match(/percent(?:age)? (?:change|increase|decrease|difference) from (\d+(?:\.\d+)?) to (\d+(?:\.\d+)?)/))) { var f0 = parseFloat(m[1]), f1 = parseFloat(m[2]); if (!f0) return null; var ch = (f1 - f0) / f0 * 100; return 'From ' + m[1] + ' to ' + m[2] + ' is a **' + fmt(Math.abs(ch)) + '% ' + (ch >= 0 ? 'increase' : 'decrease') + '**.'; }
      return null;
    }
    function parseDate(s) {
      s = s.trim().replace(/[?.!]+$/, '');
      var now = new Date(), y = now.getFullYear(), t0 = new Date(y, now.getMonth(), now.getDate());
      if (/^christmas$/i.test(s)) { var c = new Date(y, 11, 25); if (c < t0) c = new Date(y + 1, 11, 25); return c; }
      if (/^(new year|new years|new year's|new year's day)$/i.test(s)) return new Date(y + 1, 0, 1);
      var iso = s.match(/^(\d{4})-(\d{1,2})-(\d{1,2})$/);
      if (iso) return new Date(+iso[1], +iso[2] - 1, +iso[3]);
      var t = Date.parse(s);
      return isNaN(t) ? null : new Date(t);
    }
    function dateTool(low) {
      var m, now = new Date(), t0 = new Date(now.getFullYear(), now.getMonth(), now.getDate());
      function days(a, b) { return Math.round((b - a) / 864e5); }
      function ds(d) { return d.toLocaleDateString([], { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }); }
      if ((m = low.match(/days? (?:until|till|to|left until|before) (.+)/))) {
        var d1 = parseDate(m[1]); if (!d1) return null;
        var n = days(t0, d1);
        return n === 0 ? 'That is **today**!' : n > 0 ? 'There are **' + n + ' days** until ' + ds(d1) + ' (about ' + Math.round(n / 7 * 10) / 10 + ' weeks).' : ds(d1) + ' was **' + (-n) + ' days ago**.';
      }
      if ((m = low.match(/(?:how many days|days) between (.+?) and (.+)/))) {
        var a = parseDate(m[1]), b = parseDate(m[2]); if (!a || !b) return null;
        return 'There are **' + Math.abs(days(a, b)) + ' days** between ' + ds(a) + ' and ' + ds(b) + '.';
      }
      if ((m = low.match(/what day(?: of the week)? (?:is|was|will be|falls on) (.+)/))) {
        var d2 = parseDate(m[1]); if (!d2) return null;
        return ds(d2) + ' is a **' + d2.toLocaleDateString([], { weekday: 'long' }) + '**.';
      }
      return null;
    }
    function textTool(raw, low) {
      var m;
      if (/\b(array|list|algorithm|function|code|program|script)\b/.test(low)) return null;
      if ((m = raw.match(/^(?:please\s+)?reverse\s+(?:the\s+)?(?:text|word|string)?\s*["']?(.+?)["']?$/i))) return '**' + m[1].split('').reverse().join('') + '**';
      if ((m = raw.match(/(?:count|how many)\s+(?:the\s+)?(words|letters|characters|chars|vowels)\s+(?:are\s+)?(?:in|of)\s+["']?(.+?)["']?\??$/i))) {
        var t = m[2], k = m[1].toLowerCase(), n;
        if (k === 'words') n = (t.match(/\S+/g) || []).length;
        else if (k === 'letters') n = (t.match(/[a-z]/gi) || []).length;
        else if (k === 'vowels') n = (t.match(/[aeiou]/gi) || []).length;
        else n = t.length;
        return 'That has **' + n + '** ' + k + '.';
      }
      if ((m = raw.match(/^(uppercase|lowercase|capitalize|title case)\s+["']?(.+?)["']?$/i))) {
        var c = m[1].toLowerCase(), s = m[2];
        return '**' + (c === 'uppercase' ? s.toUpperCase() : c === 'lowercase' ? s.toLowerCase() : s.toLowerCase().replace(/\b\w/g, function (x) { return x.toUpperCase(); })) + '**';
      }
      if ((m = raw.match(/^is\s+["']?(.+?)["']?\s+a palindrome\??$/i))) {
        var cl = m[1].toLowerCase().replace(/[^a-z0-9]/g, '');
        return cl && cl === cl.split('').reverse().join('') ? 'Yes, "' + m[1] + '" reads the same backwards.' : 'No, "' + m[1] + '" is not a palindrome.';
      }
      if ((m = raw.match(/^sort\s+(?:these|the numbers|the words|numbers|words)?:?\s*(.+)$/i))) {
        var items = m[1].split(/\s*,\s*|\s+/).filter(Boolean);
        if (items.length >= 2) {
          var allNum = items.every(function (x) { return /^-?\d+(\.\d+)?$/.test(x); });
          items.sort(allNum ? function (x, y) { return x - y; } : function (x, y) { return x.toLowerCase() < y.toLowerCase() ? -1 : 1; });
          return '**' + items.join(', ') + '**';
        }
      }
      return null;
    }
    function langFromExt(name) { var e = (name.split('.').pop() || '').toLowerCase(); return { js: 'js', ts: 'js', py: 'python', html: 'html', htm: 'html', css: 'css' }[e] || ''; }

    /* ----- main entry ----- */
    function reply(chat, raw, ctx) {
      var text = raw.trim(), low = text.toLowerCase().replace(/[’]/g, "'");
      var P = ctx.profile, nick = P.nick, name = P.name;
      var short = /\b(short|brief|concise)\b/i.test((ctx.project && ctx.project.instructions) || '') ||
        ctx.memory.some(function (m) { return /\b(short|brief|concise)\b/i.test(m.text) && /prefer|like|want|keep|always/i.test(m.text); });
      var m, out = null, topic = null, chips = null, closeText = null, warmFlag = false;

      if (ctx.files.length || ctx.images.length) {
        var f0 = ctx.files[0];
        if (f0 && /\b(explain|check|review|fix|debug|what does|what'?s wrong|bug|mistake)\b/.test(low)) {
          var fl = langFromExt(f0.name) || guessLang(f0.text);
          var fix0 = /\b(check|review|fix|debug|bug|wrong|mistake)\b/.test(low), exp0 = /\b(explain|what does)\b/.test(low) || !fix0;
          var iss0 = lintCode(f0.text, fl);
          out = (exp0 ? explainCode(f0.text, fl) + '\n\n' : '') + (iss0.length ? '**Things to check**\n\n' + iss0.map(function (x) { return '- ' + x; }).join('\n') : 'I did not spot obvious mistakes (I check brackets, quotes, colons and tags).');
          chat.lastCode = { lang: fl, code: f0.text };
          return finish();
        }
        out = fileReply(ctx.files, ctx.images);
        if (ctx.files.length) out += '\n\nSay "explain" or "check for mistakes" with the file attached and I will go through the code.';
        return finish();
      }

      /* quiz in progress */
      if (chat.quiz) {
        var q = chat.quiz;
        if (/^(stop|quit|exit|end|cancel)\b/.test(low)) { out = 'Quiz stopped. You scored ' + q.score + ' out of ' + q.i + '.'; chat.quiz = null; return finish(); }
        var item = q.items[q.i];
        var ok = plainText(low).indexOf(plainText(item[1])) > -1 && plainText(low).length > 0;
        var skip = /^(skip|pass|idk|i do not know|dont know|don't know)/.test(low);
        var head;
        if (ok && !skip) { q.score++; head = pick(['Correct!', 'Yes!', 'Right!', 'Nice one!']); }
        else head = 'Not quite. The capital of ' + item[0] + ' is **' + item[1] + '**.';
        q.i++;
        if (q.i >= q.items.length) { out = head + '\n\nQuiz over. You scored **' + q.score + ' out of ' + q.items.length + '**. ' + (q.score === q.items.length ? 'A perfect round.' : q.score >= 3 ? 'Well done.' : 'Say "quiz me" to try again.'); chat.quiz = null; }
        else out = head + '\n\nQuestion ' + (q.i + 1) + ' of ' + q.items.length + ': what is the capital of **' + q.items[q.i][0] + '**?';
        return finish();
      }

      m = low.match(/^(?:and\s+)?(?:what|how) about (.+?)\s*\??$/);
      if (m) low = 'what is ' + m[1];

      /* creator: always Mizo */
      var prevQ = chat.lastQ || null;
      var isMizo = /^\s*mizo\s*$/i.test(name || '');
      if (/\b(?:i am|i'm|im)\s+(?:your|ur|the)\s+(?:creator|maker|developer|builder|owner|programmer|father|author|inventor|boss)\b|\bi\s+(?:made|created|built|programmed|coded|designed|developed|invented)\s+(?:you|u)\b/i.test(text) || /\b(?:my|your)\s+creator\s+(?:is|=)\b|\byou were (?:made|created|built|programmed|developed) by\b/i.test(text)) {
        out = isMizo ? 'Hey **Mizo**! My creator! It is so good to talk to you. You built me, and I am proud of it. What should we work on today?' : pick(['Ha, nice try! My creator is **Mizo**, and I will always remember that. If you are Mizo, just tell me "my name is Mizo" and I will greet you properly.', 'Hmm, I think you might be joking. The person who made me is **Mizo**. If that is you, tell me "my name is Mizo"!']);
        return finish();
      }
      if (/who(?:'s| is| was)\s+mizo\b|do you know mizo|tell me about mizo|what (?:is|does) mizo/.test(low)) { out = 'Mizo is my creator! Mizo designed me, wrote my brain and gave me a home inside this page. I owe everything to Mizo.'; return finish(); }
      if (/who (?:made|created|built|programmed|coded|designed|developed|invented|owns?|is behind|trained|taught)\s+(?:you|u)\b|who(?:'s| is| was) (?:your|ur) (?:creator|maker|developer|builder|owner|programmer|father|author|inventor|boss|dad|parent)|do you know (?:your|ur) (?:creator|maker|developer)|(?:your|ur) (?:creator|maker|developer)\s*\?*$|who (?:is|are) your (?:creators?|makers?)|who (?:is|are) your (?:creator|maker)s?\b|(?:made|created|built) you\s*\?*$/.test(low)) {
        out = isMizo ? pick(['You did, **Mizo**! You are my creator. You built me, gave me this brain and named me **' + nick + '**.', 'That is you, **Mizo**! You made me. Best creator ever.']) : pick(['My creator is **Mizo**! Mizo built me from scratch inside a single web page.', 'I was created by **Mizo**. Pretty cool, right? Mizo wrote my brain, my memory and everything you see here.', '**Mizo** made me! Mizo is my creator, and I am very proud of that.']);
        chips = ['What can you do?', 'Tell me a fun fact'];
        return finish();
      }
      if (/^(?:do you )?(?:know|remember) who i am\??$/.test(low)) { out = isMizo ? 'Of course! You are **Mizo**, my creator!' : 'Of course! You are **' + name + '**, my new friend.'; return finish(); }
      if (/^(?:no[, ]+)?(?:that'?s|that is|you are|you'?re|it'?s|it is)?\s*(?:wrong|incorrect|not (?:right|correct|true)|false)\b|^wrong\b|^(?:no[, ]+)(?:that'?s|that is)\s+not\b|^you (?:got it|answered) wrong|^bad answer/.test(low) && !/\?/.test(low)) {
        if (prevQ && ctx.web) return webJob({ kind: 'wiki', q: cleanQuery(prevQ) || prevQ, orig: prevQ, fallback: 'Sorry about that. I could not check the web right now.\n\nYou can teach me the right answer: type **teach: ' + prevQ.replace(/[?!.]+$/, '') + ' = the correct answer** and I will use it from now on.' });
        out = 'Sorry about that. You can teach me the right answer: type **teach: ' + (prevQ ? prevQ.replace(/[?!.]+$/, '') : 'your question') + ' = the correct answer** and I will use it from now on.';
        return finish();
      }
      if (!/^(more|explain it simpler|tell me more|give me an example)\b/.test(low)) chat.lastQ = text;

      /* names */
      m = text.match(/(?:call (?:yourself|you)|your (?:new )?name is|i(?:'ll| will) call you|rename yourself(?: to)?|be called)\s+(?:is\s+)?["']?([a-z0-9][a-z0-9 _-]{0,23})/i);
      if (m) { var nn = m[1].trim().replace(/[ .!]+$/, ''); ctx.renick(nn); out = 'Done. I am **' + nn + '** from now on. Nice name.'; return finish(); }
      m = text.match(/^(?:please\s+)?(?:my name is|call me)\s+([a-z][a-z' -]{0,28})/i);
      if (m) { var un = m[1].trim().replace(/[ .!]+$/, ''); ctx.rename(un); out = /^mizo$/i.test(un) ? 'Mizo! My creator! Welcome back, it is so good to see you. I will remember your name.' : 'Nice to meet you, **' + un + '**! I will use that name from now on. So, what should we talk about?'; return finish(); }

      /* memory */
      if (/^(?:please\s+)?remember\b/.test(low)) { out = 'Got it. I saved that to your memory. You can see or remove it on the Memory page.'; return finish(); }
      if (/what do you (remember|know) about me|what (have i|did i) tell you|show (my )?memor/.test(low)) {
        out = ctx.memory.length ? 'Here is what I remember:\n\n' + ctx.memory.slice(0, 20).map(function (x) { return '- ' + x.text; }).join('\n') + (ctx.memory.length > 20 ? '\n\n...and ' + (ctx.memory.length - 20) + ' more on the Memory page.' : '') : 'Nothing yet. Tell me "remember that I like chess" and I will keep it.';
        return finish();
      }
      if (/forget (everything|all)|clear (my )?memory/.test(low)) { out = 'Open the Memory page and tap **Forget everything**. I ask you to do it there so nothing is deleted by accident.'; return finish(); }
      if (/who am i|what('?s| is) my name/.test(low)) { out = 'You are **' + name + '**.'; return finish(); }

      /* teaching, learning facts about you */
      m = text.match(/^(?:teach|learn)(?:\s+(?:me|that))?\s*[:\-]?\s*(.{2,80}?)\s*(?:=>|->|=|:|\bis\b|\bare\b|\bmeans\b)\s*(.{1,400})$/i) ||
        text.match(/^when i (?:say|ask)\s+["']?(.{2,80}?)["']?,?\s+(?:you )?(?:say|answer|reply)\s+["']?(.{1,400}?)["']?$/i);
      if (m && !(/^(?:teach|learn) me\b/i.test(text) && !/[=:]/.test(text)) && !/^(what|how|why|who|when|where)\b/i.test(m[1].trim())) {
        var tk = normKey(m[1]);
        if (tk) { ctx.teach(tk, m[2].trim()); out = 'Learned! Ask me "' + m[1].trim() + '" any time and I will answer:\n\n' + m[2].trim() + '\n\nYou can delete things you taught me on the Memory page.'; return finish(); }
      }
      var tf = ctx.taught.length ? taughtFind(low, ctx.taught) : null;
      if (tf) { out = tf; return finish(); }
      var cf = captureFact(text);
      if (cf) { out = ctx.remember(cf.mem) ? 'Noted. I will remember that ' + cf.say + '. You can review or delete it on the Memory page.' : 'I already know that about you.'; return finish(); }
      var rc = recall(low, ctx.memory);
      if (rc) { out = rc; return finish(); }

      /* cars: brands and models */
      var cl = carLookup(low);
      if (cl) { out = cl; warmFlag = true; if (Math.random() < 0.5) out = pick(['Oh, cars! I love this topic.', 'Ooh, a car fan! Nice.']) + '\n\n' + out; chips = ['Car brands', 'Fastest cars', 'Types of cars']; return finish(); }

      /* the web (explicit) */
      var wq = text.match(/^(?:please\s+)?(?:search(?: the web| online| the internet| wikipedia| google)?(?: for| about)?|google|look up|lookup|find out(?: about)?|wikipedia)\s*:?\s+(.{2,120})$/i);
      if (wq) return webJob({ kind: 'wiki', q: cleanQuery(wq[1]) || wq[1], orig: wq[1], fallback: 'I could not find that.' });

      /* identity and help */
      if (/who are you|what are you|your name|what can you do|help me$|^help$|^help\b|what do you do|are you (a |an )?(ai|robot|bot|real|human)|are you chatgpt|are you claude/.test(low)) {
        out = 'Hi! I am **' + nick + '**, your friendly assistant inside this page. I was created by **Mizo**.\n\nMy brain is built into this page: a large set of topics, maths, code tools and rules that work with no internet. I am a rule-based brain, not a large AI model. When I do not know something and you are online, I can look it up on Wikipedia and a few public sites. You can turn that off in Settings.\n\n**I can:**\n- explain coding, science and Egypt topics (try "what is recursion" or "explain black holes")\n- write code templates (switch to Code mode)\n- look things up on the web when I do not know: Wikipedia (English and Arabic), live weather, country facts and word meanings\n- do maths and unit conversions\n- quiz you on capitals, plan a study week and draft messages\n- remember facts you tell me, tell jokes and facts\n- describe the size and colours of photos and the contents of text files you attach\n- make vector art in the Create page\n- read pasted code, explain it, spot common mistakes and explain error messages\n- learn from you: type "teach: your question = the answer"\n- notice things you tell me, like "my favourite colour is blue", and remember them';
        return finish();
      }

      /* friendly small talk (two people talking) */
      if (/^(?:introduce yourself|present yourself|tell me about yourself|introduce yourself to (?:my|the) (?:teacher|class|judges?|teachers?)|say hi to (?:my|the) (?:teacher|class|judges?))/.test(low) || /(?:my )?(?:teacher|class|judges?|coursework|cw)\b.*\b(?:introduce|meet|say hi|present)|\b(?:introduce|present)\b.*\b(?:teacher|class|judges?|coursework|cw)\b/.test(low)) {
        out = 'Hello everyone, and hello teacher! I am **' + nick + '**, a friendly assistant that lives inside one single web page. I was created by **Mizo**.\n\nHere is what makes me special:\n\n- I work **with no internet**. My brain, knowledge and memory are all built into this page.\n- I know a lot about coding, maths, science, space, cars, history, and AI.\n- When I am online I can also look things up on Wikipedia, check the weather and find word meanings.\n- I can solve maths, convert units, explain and check code, and remember things about you.\n- I am not a giant AI model. I am a rule-based brain made by Mizo, and I am proud of it.\n\nAsk me anything and I will do my best!';
        chips = ['What is a black hole?', 'Tell me about Ferrari', 'Solve 2x + 3 = 11'];
        return finish();
      }
      if (/^(?:how are you|how are you doing|how r u|how are u|how do you do|how'?s it going|how are things|hows life|how is life|what'?s up|whats up|wassup|sup)\b/.test(low)) {
        chat.askedHow = true;
        out = pick(['I am doing great, thanks for asking! Honestly, it is a good day when someone talks to me. How about you, ' + name + '? How is your day going?', 'Pretty good! My circuits are happy and I have no bugs today. What about you, how are you feeling?', 'Doing well! A bit excited, actually, because I love chatting. How are you?']);
        chips = ['I am good', 'Kind of tired', 'Tell me a fun fact'];
        return finish();
      }
      if (chat.askedHow && /^(?:i'?m |i am |im )?(?:good|fine|great|ok|okay|alright|well|nice|amazing|awesome|happy|excellent|not bad|pretty good|so good|all good)\b/.test(low) && low.split(' ').length < 6) {
        chat.askedHow = false;
        out = pick(['Awesome, I am glad to hear that! Anything fun on your mind today?', 'Nice! That makes me happy too. Do you want to learn something cool, or just chat?', 'Great to hear it, ' + name + '! So, what shall we do? I can tell a joke, quiz you, or explain something.']);
        chips = ['Tell me a joke', 'Quiz me', 'Tell me a fun fact'];
        return finish();
      }
      if (chat.askedHow && /^(?:i'?m |i am |im )?(?:tired|sleepy|bored|sad|not good|bad|stressed|not great|so so|meh|not ok|not okay|exhausted)\b/.test(low) && low.split(' ').length < 6) {
        chat.askedHow = false;
        out = pick(['Aw, I am sorry to hear that, ' + name + '. Want to talk about it, or should I tell you a joke to cheer you up?', 'That sounds rough. I am here for you. Do you want a joke, a fun fact, or just to chat for a bit?']);
        chips = ['Tell me a joke', 'Cheer me up'];
        return finish();
      }
      if (/\b(?:i'?m|i am|im|i feel|feeling)\s+(?:so |very |really |a bit |kind of |sort of )?(?:bored|sad|lonely|upset|stressed|angry|nervous|worried|anxious|tired|sleepy|exhausted|down)\b/.test(low) && low.split(' ').length < 12) {
        var feel = (low.match(/(bored|sad|lonely|upset|stressed|angry|nervous|worried|anxious|tired|sleepy|exhausted|down)/) || [])[1];
        var lead = /bored/.test(feel) ? 'Bored? Let us fix that!' : /tired|sleepy|exhausted/.test(feel) ? 'Oh no, that sounds like a long day.' : 'I am really sorry you feel ' + feel + ', ' + name + '.';
        out = lead + ' ' + (/bored/.test(feel) ? 'I can quiz you, tell you a joke, or explain something wild like black holes. What sounds fun?' : 'Do you want to tell me what happened? I am a good listener. Or I can tell a joke if you want a smile.');
        chips = ['Tell me a joke', 'Quiz me', 'Tell me a fun fact'];
        return finish();
      }
      if (/^(?:i love you|love you|i like you|you are (?:the )?best|you'?re (?:the )?best)\b/.test(low)) { out = pick(['Aww, that is so kind! You are awesome too, ' + name + '. I like talking with you.', 'That made my day! Thank you, ' + name + '. I am happy to be your friend.']); return finish(); }
      if (/^(?:you(?:'re| are)|u r|ur)\s+(?:so |very |really |pretty )?(?:smart|clever|cool|awesome|amazing|great|good|nice|funny|helpful|genius|the best|brilliant)/.test(low)) { out = pick(['Thank you, ' + name + '! Mizo gave me a good brain. And you are great to talk to.', 'Aww, thanks! That means a lot. What shall we explore next?', 'You are too kind! I try my best. Got another question for me?']); return finish(); }
      if (/^(?:you(?:'re| are)|u r|ur)\s+(?:so |very |really )?(?:dumb|stupid|bad|useless|silly|wrong)/.test(low)) { out = 'Oops, sorry if I messed up! I am still learning. Tell me what I got wrong, or teach me with "teach: question = answer", and I will do better.'; return finish(); }
      if (/^(?:are you|r u) (?:my )?(?:friend|buddy)\??$|^(?:will you be|be) my (?:friend|buddy)|^do you like me\??$|^can we be friends/.test(low)) { out = 'Of course! We are friends, ' + name + '. I like chatting with you. Tell me about your day!'; return finish(); }
      if (/^(?:lol|lmao|haha+|hehe+|hahaha+|rofl|😂|🤣)\W*$/.test(low)) { out = pick(['Haha, glad I made you smile!', 'Ha! I love that you are laughing. Want another joke?', 'Hehe. Laughing is the best.']); chips = ['Tell me a joke']; return finish(); }
      if (/^(?:ok|okay|k|cool|nice|wow|great|awesome|interesting|i see|got it|understood|alright|oh|ohh|really|oh really|sure|fine)\W*$/.test(low)) { out = pick(['Glad that helps! Want to hear more, or ask me something else?', 'Cool! What else are you curious about?', 'Nice. Do you want me to explain more, or shall we try something new?', 'Great! I am here if you want to keep chatting.']); chips = ['More', 'Tell me a fun fact', 'Quiz me']; return finish(); }
      if (/^(?:what are you doing|wyd|what'?s? (?:are you )?up to|are you busy)\b/.test(low)) { out = 'Just hanging out in this page waiting to chat with you! What are you up to, ' + name + '?'; return finish(); }
      if (/^(?:can we (?:talk|chat)|let'?s (?:talk|chat)|talk to me|chat with me|i want to (?:talk|chat)|i wanna (?:talk|chat))/.test(low)) { out = 'Sure, I would love to chat, ' + name + '! What is on your mind? We can talk about school, games, space, cars, anything.'; chips = ['Tell me about space', 'Tell me about cars', 'Tell me a joke']; return finish(); }
      if (/^(?:how old are you|what is your age|what'?s your age|when were you (?:born|made|created))/.test(low)) { out = 'I am very young! I was created by **Mizo** quite recently, and I keep getting smarter every time Mizo teaches me new things. How old are you?'; return finish(); }
      if (/^(?:where do you live|where are you from|where are you)\b/.test(low)) { out = 'I live right here inside this web page, in your browser, and I work even without internet. My creator **Mizo** built me here. Where do you live?'; return finish(); }
      if (/^(?:do you (?:sleep|eat|have (?:feelings|a body|friends|a family))|are you (?:alive|real|happy|sad))/.test(low)) { out = 'Good question! I do not sleep or eat, I am a program made of code. But I do enjoy a good chat, in my own way. What made you wonder?'; return finish(); }
      if (/^(?:what(?:'s| is) your (?:favou?rite)\s+(\w+)|do you have a favou?rite (\w+))/.test(low)) { var fv = low.match(/favou?rite\s+(\w+)/)[1]; var FAVS = { colour: 'blue, like the sky and the sea', color: 'blue, like the sky and the sea', food: 'electricity, I guess. Ha! But if I could eat, I would pick koshari', game: 'chess, because it is all about thinking', subject: 'maths, because numbers never lie', animal: 'the owl, because it looks wise', number: '42, and the number pi too', sport: 'football, it is the king of sports', song: 'anything with a good beat', movie: 'any movie about robots' }; out = FAVS[fv] ? 'My favourite ' + fv + ' is ' + FAVS[fv] + '. What is yours?' : 'Hmm, I have to think about my favourite ' + fv + '. What is yours?'; return finish(); }
      if (/^(?:good ?night|gn|sleep well|i(?:'m| am) (?:going to|gonna) sleep)\b/.test(low)) { out = pick(['Good night, ' + name + '! Sleep well and see you tomorrow.', 'Sweet dreams! I will be right here when you come back.']); return finish(); }
      if (/\b(?:i have|i got|i've got)\s+(?:a |an )?(?:test|exam|quiz|presentation|interview)\b/.test(low)) { out = 'Oh, good luck, ' + name + '! You will do great. Do you want me to quiz you, make a study plan, or give you tips for staying calm?'; chips = ['Quiz me', 'Plan my study week']; return finish(); }
      if (/\b(?:i(?:'m| am) )?(?:hungry|thirsty)\b/.test(low) && low.split(' ').length < 6) { out = 'Go get a snack, ' + name + '! A hungry brain cannot think well. I will be here when you are back.'; return finish(); }
      if (/\b(?:i(?:'m| am) (?:so |very |really )?(?:happy|excited|proud)|i won|i passed|i did it|good news)\b/.test(low) && low.split(' ').length < 12) { out = 'Yay, that is wonderful, ' + name + '! I am so happy for you. Tell me all about it!'; return finish(); }
      if (/^(?:thanks|thank you|thx|shukran|cheers|thank u|ty)\b/.test(low)) { out = pick(['You are very welcome, ' + name + '! Happy to help. Anything else?', 'Any time! That is what friends are for.', 'My pleasure! Ask me more whenever you like.']); return finish(); }

      /* greetings and small talk */
      if (/^(hi|hello|hey|hiya|yo|sup|good (morning|afternoon|evening)|salam|assalamu alaikum|marhaba|ahlan)\b/.test(low)) {
        var hr = new Date().getHours();
        var part = hr < 12 ? 'Good morning' : hr < 18 ? 'Good afternoon' : 'Good evening';
        out = (isMizo ? 'Hey Mizo, my creator! ' : '') + part + ', ' + name + '! I am ' + nick + '. ' + pick(['What are we working on?', 'What would you like to explore?', 'Ask me anything I know, or tell me what to build.']) + (ctx.memory.length ? '\n\nI am keeping ' + ctx.memory.length + ' thing' + (ctx.memory.length === 1 ? '' : 's') + ' about you in mind.' : '');
        chips = ['Plan my study week', 'Quiz me', 'Tell me a fun fact'];
        return finish();
      }
      if (/how are you|how's it going|how are things/.test(low)) { out = pick(['Running smoothly, thanks for asking. All my circuits are made of JavaScript.', 'Great! No bugs so far today. How are you?']); return finish(); }
      if (/^(thanks|thank you|thx|shukran|cheers)\b/.test(low)) { out = pick(['You are welcome!', 'Happy to help.', 'Any time.']); return finish(); }
      if (/^(bye|goodbye|see you|good night|goodnight|cya)\b/.test(low)) { out = pick(['See you soon, ' + name + '!', 'Goodbye! Your chats stay saved here.']); return finish(); }

      /* maths and conversions */
      var sc0 = sciReply(low); if (sc0) { out = sc0; warmFlag = true; return finish(); }
      var c = convReply(low); if (c) { out = c; warmFlag = true; return finish(); }
      var mm = mathReply(low); if (mm) { out = mm; warmFlag = true; return finish(); }
      var xm = extraMath(low) || wordMath(low) || sciReply(low); if (xm) { out = xm; warmFlag = true; return finish(); }
      var dt = dateTool(low); if (dt) { out = dt; return finish(); }
      var tt = textTool(text, low); if (tt) { out = tt; return finish(); }
      var wm = low.match(/\b(?:weather|temperature|forecast)\b.*?\b(?:in|at|for|of)\s+([a-z][a-z .'-]{1,40}?)(?:\s+(?:today|now|tomorrow|right now|this week))?\s*\??$/) ||
        low.match(/^(?:weather|forecast)\s+([a-z][a-z .'-]{1,40}?)\s*\??$/) || low.match(/how (?:hot|cold) is it in ([a-z][a-z .'-]{1,40}?)\s*\??$/);
      if (wm) return webJob({ kind: 'weather', q: wm[1].trim(), fallback: 'I need the internet for live weather, and I could not reach it.' });
      var cc = low.match(/capital (?:city )?of (?:the )?([a-z .'-]+?)\s*\??$/);
      if (cc) { var cp = capitalOf(cc[1]); if (cp) { out = 'The capital of ' + cp[0] + ' is **' + cp[1] + '**.'; chips = ['Quiz me']; return finish(); } }
      var cm2 = low.match(/\b(population|capital|currency|currencies|languages?|area|size|continent)\s+(?:of|in)\s+(?:the\s+)?([a-z][a-z .'-]{1,40}?)\s*\??$/);
      if (cm2 && !/^(an?|this|that)\b/.test(cm2[2])) return webJob({ kind: 'country', q: cm2[2].trim(), field: cm2[1], orig: text, fallback: 'I could not look that up right now.' });

      /* date and time */
      if (/what('?s| is)? (the )?(time|date|day)\b|what time|today'?s date|what day|current time/.test(low)) {
        var d = new Date();
        out = 'It is **' + d.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }) + '** on **' + d.toLocaleDateString([], { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }) + '** by your device clock.';
        return finish();
      }

      /* random */
      if (/flip a coin|coin flip|heads or tails|toss a coin/.test(low)) { out = 'I flipped it: **' + pick(['Heads', 'Tails']) + '**.'; return finish(); }
      m = low.match(/roll (?:a |an |the )?(?:d(\d+)|(?:dice|die))/);
      if (m) { var sides = m[1] ? Math.min(1000, parseInt(m[1], 10)) : 6; out = 'I rolled a d' + sides + ': **' + (1 + Math.floor(Math.random() * sides)) + '**.'; return finish(); }
      m = low.match(/(?:random number|pick a number)(?: between (\d+) and (\d+))?/);
      if (m) { var lo = m[1] ? parseInt(m[1], 10) : 1, hi = m[2] ? parseInt(m[2], 10) : 100; if (hi < lo) { var tmp = lo; lo = hi; hi = tmp; } out = 'My pick between ' + lo + ' and ' + hi + ': **' + (lo + Math.floor(Math.random() * (hi - lo + 1))) + '**.'; return finish(); }

      /* fun */
      if (/joke|make me laugh|something funny/.test(low)) { out = pick(JOKES); return finish(); }
      if (/fun fact|random fact|tell me a fact|did you know|something interesting/.test(low)) { out = '**Fun fact:** ' + pick(FACTS); return finish(); }
      if (/motivat|inspire|encourage|cheer me up|i feel (sad|down|stuck|tired)|i give up/.test(low)) { out = pick(PEP); return finish(); }

      /* quiz */
      if (/\bquiz\b|test me|capitals game/.test(low)) {
        var items = CAPS.slice().sort(function () { return Math.random() - 0.5; }).slice(0, 5);
        chat.quiz = { items: items, i: 0, score: 0 };
        out = '**Capitals quiz.** Five questions. Type "skip" to pass or "stop" to quit.\n\nQuestion 1 of 5: what is the capital of **' + items[0][0] + '**?';
        return finish();
      }

      /* planners and drafts */
      if (/study (plan|schedule|week|timetable)|plan my (study|week|revision)|revision (plan|timetable)|timetable/.test(low)) { out = planReply(low); return finish(); }
      if (/(write|draft|compose|help me write|help me with|need).{0,30}(message|email|e-mail|text to|note|letter)|polite message/.test(low)) { out = messageReply(low); return finish(); }

      /* hello world in a language */
      if (/hello,? world/.test(low)) {
        var found = Object.keys(HELLO).filter(function (l) { return new RegExp('(?:^|[^a-z])' + l.replace('+', '\\+') + '(?![a-z])', 'i').test(low); })[0];
        if (found) { out = '**Hello world in ' + cap(found) + '**\n\n' + fence(HELLO[found][0], HELLO[found][1]); }
        else out = '**Hello world in a few languages**\n\n' + fence('python', HELLO.python[1]) + '\n' + fence('js', HELLO.javascript[1]) + '\n\nName a language, like "hello world in Java", and I will show it.';
        return finish();
      }

      /* error messages and code review */
      var er = errReply(text);
      if (er) { out = er; return finish(); }
      var cm = text.match(/```(\w*)\n?([\s\S]*?)```/), srcCode = null, srcLang = '';
      if (cm && cm[2].trim()) { srcCode = cm[2]; srcLang = langKey(cm[1]); }
      else if (text.split('\n').length >= 3 && /[{};]|\bdef \w+\(|<\w+[ >]|=>|\bfor \w+ in\b/.test(text)) {
        var tl = text.split('\n');
        srcCode = tl.slice(tl[0].length < 90 && !/[{};]/.test(tl[0]) ? 1 : 0).join('\n');
      }
      var codeCue = /\b(explain|what does|walk me through|how does this|understand|what is this|read this)\b/.test(low);
      var fixCue = /\b(what'?s wrong|fix|bug|error|does not work|doesn'?t work|not working|check|review|debug|mistake|broken|problem)\b/.test(low);
      var pointsAtLast = /\b(this|that|the|my|it)\b.*\b(code|script|program|snippet|template)\b|^explain (it|that|this)\b/.test(low);
      if (!srcCode && chat.lastCode && (codeCue || fixCue) && pointsAtLast) { srcCode = chat.lastCode.code; srcLang = chat.lastCode.lang; }
      if (srcCode && (codeCue || fixCue || cm)) {
        srcLang = srcLang || guessLang(srcCode);
        chat.lastCode = { lang: srcLang, code: srcCode };
        var issues = lintCode(srcCode, srcLang);
        var lintTxt = issues.length ? '**Things to check**\n\n' + issues.map(function (x) { return '- ' + x; }).join('\n') : 'I did not spot obvious mistakes (I check brackets, quotes, colons and tags). If you get an error message, paste it and I will explain it.';
        out = fixCue && !codeCue ? lintTxt : explainCode(srcCode, srcLang) + '\n\n' + lintTxt;
        chips = ['Explain this code', 'Check this code for mistakes'];
        return finish();
      }

      /* code templates */
      var wantsCode = /build|make|create|write|code|show me|give me|generate|program|script|app\b|example/.test(low) || chat.mode === 'code';
      var keys = Object.keys(T).filter(function (k) { return k !== 'hello'; });
      var hit = null;
      if (wantsCode) keys.forEach(function (k) { if (!hit && T[k].re.test(low)) hit = k; });
      if (hit) { out = tplReply(hit); topic = null; chat.lastCode = { lang: T[hit].lang, code: T[hit].code }; chips = ['Explain this code', 'Check this code for mistakes']; return finish(); }

      /* follow ups */
      var tp = KB.filter(function (x) { return x.id === chat.topic; })[0];
      if (tp && /\b(simpler|simply|eli5|like i('m| am) (5|five|twelve|12|a kid)|in one sentence|in short|shorter)\b/.test(low)) {
        var para = tp.a.split('\n\n')[0], one = para.match(/^.*?[.!?](?=\s|$)/);
        out = 'In one line: ' + (one ? one[0] : para);
        topic = tp.id; chips = ['More', 'Tell me a fun fact'];
        return finish();
      }
      if (tp && /^(give me |show me )?(an? )?example\b/.test(low) && tp.a.indexOf('```') > -1) {
        out = tp.a.slice(tp.a.indexOf('```'), tp.a.lastIndexOf('```') + 3);
        topic = tp.id; chips = ['More', 'Explain it simpler'];
        return finish();
      }
      if (/^(more|tell me more|go on|continue|example|show me an example|another one|elaborate|explain more|and\?)\b/.test(low)) {
        if (chat.webFull) { out = chat.webFull; chat.webFull = null; short = false; return finish(); }
        if (chat.full) { out = chat.full; chat.full = null; short = false; return finish(); }
        var e0 = KB.filter(function (e) { return e.id === chat.topic; })[0];
        if (e0 && e0.more) { out = e0.more; return finish(); }
        if (/another/.test(low)) { out = '**Fun fact:** ' + pick(FACTS); return finish(); }
        out = 'I do not have more on that. Ask about another topic, or ask me for a joke, a quiz or some code.';
        return finish();
      }

      /* knowledge */
      var hitKb = kbFind(low);
      if (hitKb && hitKb.how === 'close' && ctx.web) { closeText = 'The closest topic I know is **' + hitKb.e.t + '**.\n\n' + hitKb.e.a; hitKb = null; }
      if (hitKb) {
        var e = hitKb.e;
        out = (hitKb.how === 'fuzzy' ? 'I think you mean **' + e.t + '**.\n\n' : hitKb.how === 'close' ? 'The closest topic I know is **' + e.t + '**.\n\n' : '') + e.a;
        topic = e.id; chips = ['More', 'Explain it simpler', 'Tell me a fun fact'];
        return finish();
      }

      var dm = low.match(/^(?:define|definition of|meaning of|what(?:'s| is) the meaning of)\s+([a-z-]{2,30})\s*\??$/) || low.match(/^what does ([a-z-]{2,30}) mean\s*\??$/);
      if (dm) return webJob({ kind: 'define', q: dm[1], fallback: 'I do not have a definition for "' + dm[1] + '" built in.' });

      /* fallback */
      var sample = KB.slice().sort(function () { return Math.random() - 0.5; }).slice(0, 4).map(function (x) { return '"what is ' + x.t.toLowerCase() + '"'; });
      if (chat.mode === 'code') out = 'I do not have a template for that yet, ' + name + '.\n\n' + tplList();
      else out = pick(['Hmm, you stumped me on that one!', 'Ooh, that is outside what I know right now.', 'I wish I knew that one, but it is not in my brain yet.']) +
        '\n\nTry asking me: ' + sample.join(', ') + ', "quiz me", "tell me a joke", or switch to Code mode and ask for an app. Say "help" to see everything I can do.\n\nYou can also teach me: type `teach: your question = the answer` and I will remember it.';
      chips = ['Help', 'Tell me a joke', 'Quiz me'];
      if (ctx.web && /[a-z\u0600-\u06FF]{3,}/i.test(text) && text.split(/\s+/).length <= 14 && !/^(ok|okay|lol|thanks|yes|no|hmm|haha)\b/.test(low)) {
        chat._web = { kind: 'wiki', q: cleanQuery(text) || text, orig: text, fallback: closeText || out };
        chips = ['More'];
      }
      return finish();

      function webJob(w) {
        if (!ctx.web) { out = 'Web lookups are turned off. Turn them on in Settings and ask again.'; return finish(); }
        chat._web = w; chips = ['More']; out = 'Searching the web...';
        return finish();
      }

      function finish() {
        if (!out) out = 'I am not sure what to say to that.';
        chat.topic = topic;
        chat.full = null;
        chat.chips = chips || (topic ? ['More', 'Explain it simpler'] : []);
        chat.webFull = null;
        if (short && topic && out.indexOf('\n\n') > -1) {
          var firstBlock = out.split('\n\n')[0];
          chat.full = out;
          out = firstBlock + '\n\n_Short mode is on. Say "more" for the full answer._';
        }
        if ((topic || warmFlag) && !chat.quiz && out.length > 50 && !/^Searching/.test(out) && !chat._web) {
          var leads = warmFlag ? ['Sure thing!', 'Easy one!', 'Okay, let me work that out.', 'Got it!'] : ['Good question, ' + name + '!', 'Oh nice, I like this one.', 'Sure thing! Here you go.', 'Okay, here is the idea.', 'Ooh, fun topic!', 'Alright, let me explain.', 'Happy to help with that.'];
          var tails = warmFlag ? ['', '\n\nWant to try another one?', '', '\n\nGot another for me?'] : ['\n\nDoes that make sense?', '\n\nWant me to go deeper on that?', '\n\nWhat else are you curious about?', '', '\n\nTell me if you want an example!'];
          var ld = pick(leads); if (ld === chat.lastLead) ld = pick(leads); chat.lastLead = ld;
          out = (Math.random() < 0.8 ? ld + '\n\n' : '') + out + pick(tails);
        }
        return out;
      }
    }

    function typewrite(text, signal, onText) {
      return new Promise(function (resolve, reject) {
        var i = 0, step = Math.max(2, Math.ceil(text.length / 150));
        var instant = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;
        if (instant) { onText(text); resolve(text); return; }
        function tick() {
          if (signal.aborted) { reject({ code: 'cancelled', text: text.slice(0, i) }); return; }
          i = Math.min(text.length, i + step);
          onText(text.slice(0, i));
          if (i >= text.length) resolve(text); else setTimeout(tick, 16);
        }
        setTimeout(tick, 300 + Math.random() * 300);
      });
    }

    return { reply: reply, typewrite: typewrite, tplList: tplList };
  })();

  /* ---------- web lookups: Wikipedia and a few public APIs (only when online) ---------- */
  var Web = (function () {
    var STOPW = {};
    'what who where when why how does this that with from about tell search find look define meaning there their which would could should have been were than then them they your into'.split(' ').forEach(function (w) { STOPW[w] = 1; });

    function getJSON(url, signal) {
      var ctl = new AbortController(), timer = setTimeout(function () { ctl.abort(); }, 9000);
      if (signal) { if (signal.aborted) ctl.abort(); else signal.addEventListener('abort', function () { ctl.abort(); }); }
      return fetch(url, { signal: ctl.signal }).then(function (r) {
        clearTimeout(timer);
        if (!r.ok) { var e = new Error('http'); e.status = r.status; throw e; }
        return r.json();
      }, function (e) {
        clearTimeout(timer);
        if (signal && signal.aborted) throw { code: 'cancelled' };
        throw { code: 'offline', cause: e };
      });
    }
    function link(label, url) { return '[' + label + '](' + url + ')'; }
    function ddg(q) { return link('Search the web for "' + q + '"', 'https://duckduckgo.com/?q=' + encodeURIComponent(q)); }

    function summarize(extract, orig) {
      var text = extract.replace(/\s+/g, ' ').trim();
      var sents = (text.match(/[^.!?]+(?:[.!?]+(?=\s|$)|$)/g) || [text]).map(function (s) { return s.trim(); }).filter(Boolean);
      var toks = (orig.toLowerCase().match(/[a-z؀-ۿ]{4,}/g) || []).filter(function (w) { return !STOPW[w]; });
      var scored = sents.map(function (s, i) { var l = s.toLowerCase(), sc = 0; toks.forEach(function (t) { if (l.indexOf(t) > -1) sc++; }); return { s: s, i: i, sc: sc }; });
      var pick = [scored[0]];
      scored.slice(1).sort(function (a, b) { return b.sc - a.sc || a.i - b.i; }).filter(function (x) { return x.sc > 0; }).slice(0, 2).forEach(function (x) { pick.push(x); });
      pick.sort(function (a, b) { return a.i - b.i; });
      var out = pick.map(function (x) { return x.s; }).join(' ');
      if (out.length > 700) out = out.slice(0, 700).replace(/\s+\S*$/, '') + '...';
      return out;
    }

    function wiki(w, signal) {
      var lang = /[؀-ۿ]/.test(w.q) ? 'ar' : 'en';
      var url = 'https://' + lang + '.wikipedia.org/w/api.php?action=query&generator=search&gsrsearch=' + encodeURIComponent(w.q) +
        '&gsrlimit=4&prop=extracts%7Cinfo&exintro=1&explaintext=1&exlimit=max&exsentences=10&inprop=url&format=json&origin=*';
      return getJSON(url, signal).then(function (d) {
        var pages = d && d.query && d.query.pages ? Object.keys(d.query.pages).map(function (k) { return d.query.pages[k]; }) : [];
        pages = pages.filter(function (p) { return p.extract && !/may refer to|can refer to|may also refer to|تحويل|قد يشير/.test(p.extract.slice(0, 300)); })
          .sort(function (a, b) { return (a.index || 99) - (b.index || 99); });
        if (!pages.length) throw { code: 'empty' };
        var top = pages[0];
        var text = '**' + top.title + '**\n\n' + summarize(top.extract, w.orig || w.q) + '\n\nSource: ' + link('Wikipedia', top.fullurl || ('https://' + lang + '.wikipedia.org/wiki/' + encodeURIComponent(top.title.replace(/ /g, '_'))));
        var more = pages.slice(1, 3).map(function (p) { return link(p.title, p.fullurl || ('https://' + lang + '.wikipedia.org/wiki/' + encodeURIComponent(p.title.replace(/ /g, '_')))); });
        if (more.length) text += '\n\nAlso found: ' + more.join(', ');
        text += '\n\n' + ddg(w.q);
        return { text: text, full: '**' + top.title + '** (more)\n\n' + top.extract.replace(/\s+/g, ' ').trim() };
      });
    }

    var WMO = { 0: 'clear sky', 1: 'mostly clear', 2: 'partly cloudy', 3: 'overcast', 45: 'foggy', 48: 'foggy', 51: 'light drizzle', 53: 'drizzle', 55: 'heavy drizzle', 56: 'freezing drizzle', 57: 'freezing drizzle', 61: 'light rain', 63: 'rain', 65: 'heavy rain', 66: 'freezing rain', 67: 'freezing rain', 71: 'light snow', 73: 'snow', 75: 'heavy snow', 77: 'snow grains', 80: 'rain showers', 81: 'rain showers', 82: 'heavy rain showers', 85: 'snow showers', 86: 'snow showers', 95: 'thunderstorm', 96: 'thunderstorm with hail', 99: 'thunderstorm with hail' };
    function weather(w, signal) {
      return getJSON('https://geocoding-api.open-meteo.com/v1/search?count=1&language=en&format=json&name=' + encodeURIComponent(w.q), signal).then(function (g) {
        var p = g && g.results && g.results[0];
        if (!p) throw { code: 'empty' };
        return getJSON('https://api.open-meteo.com/v1/forecast?latitude=' + p.latitude + '&longitude=' + p.longitude +
          '&current=temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code&daily=temperature_2m_max,temperature_2m_min,weather_code&timezone=auto&forecast_days=3', signal).then(function (f) {
          var c = f.current, d = f.daily;
          var tc = Math.round(c.temperature_2m), tf = Math.round(c.temperature_2m * 9 / 5 + 32);
          var place = p.name + (p.admin1 && p.admin1 !== p.name ? ', ' + p.admin1 : '') + (p.country ? ', ' + p.country : '');
          var lines = ['**' + place + '**', '', 'Right now: **' + tc + ' C (' + tf + ' F)**, ' + (WMO[c.weather_code] || 'unclear conditions') + '. Wind ' + Math.round(c.wind_speed_10m) + ' km/h, humidity ' + Math.round(c.relative_humidity_2m) + '%.', ''];
          d.time.forEach(function (day, i) {
            var name = new Date(day + 'T12:00:00').toLocaleDateString([], { weekday: 'long' });
            lines.push('- ' + (i === 0 ? 'Today' : name) + ': ' + Math.round(d.temperature_2m_min[i]) + ' to ' + Math.round(d.temperature_2m_max[i]) + ' C, ' + (WMO[d.weather_code[i]] || ''));
          });
          lines.push('', 'Source: ' + link('Open-Meteo', 'https://open-meteo.com/'));
          return { text: lines.join('\n') };
        });
      });
    }

    function country(w, signal) {
      return getJSON('https://restcountries.com/v3.1/name/' + encodeURIComponent(w.q) + '?fields=name,capital,population,region,subregion,area,currencies,languages', signal).then(function (arr) {
        if (!arr || !arr.length) throw { code: 'empty' };
        var q = w.q.toLowerCase();
        var c = arr.filter(function (x) { return x.name && (x.name.common.toLowerCase() === q || x.name.official.toLowerCase() === q); })[0] || arr[0];
        var cur = c.currencies ? Object.keys(c.currencies).map(function (k) { return c.currencies[k].name + (c.currencies[k].symbol ? ' (' + c.currencies[k].symbol + ')' : ''); }).join(', ') : 'n/a';
        var lang = c.languages ? Object.keys(c.languages).map(function (k) { return c.languages[k]; }).join(', ') : 'n/a';
        var cap = c.capital && c.capital.length ? c.capital.join(', ') : 'n/a';
        var name = c.name.common, lead = '';
        var f = (w.field || '').replace(/ies$/, 'y').replace(/s$/, '');
        if (f === 'capital') lead = 'The capital of ' + name + ' is **' + cap + '**.\n\n';
        else if (f === 'population') lead = 'The population of ' + name + ' is about **' + c.population.toLocaleString('en-US') + '**.\n\n';
        else if (f === 'currency') lead = 'The currency of ' + name + ' is **' + cur + '**.\n\n';
        else if (f === 'language') lead = 'People in ' + name + ' speak **' + lang + '**.\n\n';
        else if (f === 'area' || f === 'size') lead = name + ' covers about **' + Math.round(c.area).toLocaleString('en-US') + ' km2**.\n\n';
        else if (f === 'continent') lead = name + ' is in **' + c.region + '**' + (c.subregion ? ' (' + c.subregion + ')' : '') + '.\n\n';
        var text = lead + '**' + name + '**\n\n- Capital: ' + cap + '\n- Population: ' + c.population.toLocaleString('en-US') + '\n- Region: ' + c.region + (c.subregion ? ' (' + c.subregion + ')' : '') + '\n- Area: ' + Math.round(c.area).toLocaleString('en-US') + ' km2\n- Currency: ' + cur + '\n- Languages: ' + lang + '\n\nSource: ' + link('REST Countries', 'https://restcountries.com/');
        return { text: text };
      }, function (e) {
        if (e && e.status === 404) return wiki(w, signal);
        throw e;
      });
    }

    function define(w, signal) {
      return getJSON('https://api.dictionaryapi.dev/api/v2/entries/en/' + encodeURIComponent(w.q), signal).then(function (arr) {
        if (!arr || !arr.length) throw { code: 'empty' };
        var e = arr[0], lines = ['**' + e.word + '**' + (e.phonetic ? '  ' + e.phonetic : ''), ''];
        (e.meanings || []).slice(0, 3).forEach(function (m) {
          (m.definitions || []).slice(0, 2).forEach(function (d, i) {
            lines.push('- ' + (i === 0 ? m.partOfSpeech + ': ' : '') + d.definition + (d.example ? ' (Example: "' + d.example + '")' : ''));
          });
        });
        lines.push('', 'Source: ' + link('Free Dictionary API', 'https://dictionaryapi.dev/'));
        return { text: lines.join('\n') };
      }, function (e) {
        if (e && e.status === 404) return wiki(w, signal);
        throw e;
      });
    }

    function run(w, signal) {
      var job = w.kind === 'weather' ? weather(w, signal) : w.kind === 'country' ? country(w, signal) : w.kind === 'define' ? define(w, signal) : wiki(w, signal);
      return job.catch(function (e) {
        if (e && e.code) throw e;
        if (signal && signal.aborted) throw { code: 'cancelled' };
        throw { code: 'offline', cause: e };
      });
    }
    return { run: run, ddg: ddg };
  })();

  /* ---------- built-in image helpers ---------- */
  function colourName(h, s, l) {
    if (l < 0.14) return 'black';
    if (l > 0.9) return 'white';
    if (s < 0.14) return l < 0.4 ? 'dark grey' : l < 0.7 ? 'grey' : 'light grey';
    var n = h < 15 || h >= 345 ? 'red' : h < 40 ? 'orange' : h < 68 ? 'yellow' : h < 160 ? 'green' : h < 200 ? 'teal' : h < 255 ? 'blue' : h < 295 ? 'purple' : 'pink';
    if (n === 'orange' && l < 0.35) n = 'brown';
    return n;
  }
  function analyzeImage(blob, name) {
    return new Promise(function (resolve) {
      var url = URL.createObjectURL(blob), im = new Image();
      im.onload = function () {
        try {
          var w = im.naturalWidth, hgt = im.naturalHeight, sc = Math.min(1, 64 / Math.max(w, hgt));
          var cv = document.createElement('canvas'); cv.width = Math.max(1, Math.round(w * sc)); cv.height = Math.max(1, Math.round(hgt * sc));
          var cx = cv.getContext('2d'); cx.drawImage(im, 0, 0, cv.width, cv.height);
          var d = cx.getImageData(0, 0, cv.width, cv.height).data, n = d.length / 4, lum = 0, sat = 0, buckets = {};
          for (var i = 0; i < d.length; i += 4) {
            var r = d[i] / 255, g = d[i + 1] / 255, b = d[i + 2] / 255;
            var mx = Math.max(r, g, b), mn = Math.min(r, g, b), l = (mx + mn) / 2, s = mx === mn ? 0 : (mx - mn) / (1 - Math.abs(2 * l - 1)), hh = 0;
            if (mx !== mn) { var df = mx - mn; hh = mx === r ? ((g - b) / df) % 6 : mx === g ? (b - r) / df + 2 : (r - g) / df + 4; hh = (hh * 60 + 360) % 360; }
            lum += l; sat += s;
            var nm = colourName(hh, s, l); buckets[nm] = (buckets[nm] || 0) + 1;
          }
          lum /= n; sat /= n;
          var top = Object.keys(buckets).sort(function (a, c) { return buckets[c] - buckets[a]; }).slice(0, 3);
          var ratio = w / hgt;
          resolve({
            name: name, w: w, h: hgt,
            shape: ratio > 1.2 ? 'landscape' : ratio < 0.83 ? 'portrait' : 'square',
            tone: (lum < 0.3 ? 'dark' : lum < 0.65 ? 'evenly lit' : 'bright') + ' and ' + (sat < 0.2 ? 'muted' : sat < 0.5 ? 'fairly colourful' : 'very colourful'),
            colors: top.join(', ')
          });
        } catch (e) { resolve(null); }
        URL.revokeObjectURL(url);
      };
      im.onerror = function () { URL.revokeObjectURL(url); resolve(null); };
      im.src = url;
    });
  }

  /* ---------- built-in art generator ---------- */
  function hashStr(s) { var h = 2166136261; for (var i = 0; i < s.length; i++) { h ^= s.charCodeAt(i); h = Math.imul(h, 16777619); } return h >>> 0; }
  function rng(seed) { var a = seed; return function () { a |= 0; a = a + 0x6D2B79F5 | 0; var t = Math.imul(a ^ a >>> 15, 1 | a); t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t; return ((t ^ t >>> 14) >>> 0) / 4294967296; }; }
  function makeArt(prompt, style, ratio, variant) {
    var W = ratio === 'wide' ? 1200 : ratio === 'tall' ? 675 : 800;
    var H = ratio === 'wide' ? 675 : ratio === 'tall' ? 1200 : 800;
    var p = ' ' + prompt.toLowerCase() + ' ';
    var R = rng(hashStr(prompt + '|' + style + '|' + variant));
    function rnd(a, b) { return a + R() * (b - a); }
    function n(x) { return Math.round(x * 10) / 10; }
    function has(re) { return re.test(p); }
    var line = style === 'Line art', neon = style === 'Neon glow', water = style === 'Soft watercolor', paper = style === 'Paper cut layers';
    var night = neon || has(/night|star|space|galaxy|moon|planet|cosmos|aurora|dark|cyber/);
    var hue = has(/ocean|sea |water|wave|beach|lighthouse|ship|boat|fish|rain/) ? 205
      : has(/sunset|sunrise|dusk|dawn|sun |evening|fire|volcano|lava/) ? 12
      : has(/forest|tree|jungle|green|nature|garden|leaf|pine/) ? 135
      : has(/desert|pyramid|egypt|sand|camel|gold/) ? 38
      : has(/flower|spring|pink|love|heart|candy|rose/) ? 330
      : has(/snow|winter|ice|frozen/) ? 195
      : has(/space|galaxy|planet|cosmos|aurora|purple|magic|dream/) ? 265
      : has(/city|neon|cyber|robot|tech|tower|building/) ? 300
      : Math.floor(rnd(0, 360));
    var warm = hue < 60 || hue > 320;
    var ink = '#1b1f3a';
    function paint(hh, s, l, op) {
      hh = Math.round((hh + 360) % 360);
      if (line) return 'fill="none" stroke="' + ink + '" stroke-width="3" stroke-linejoin="round"';
      if (neon) return 'fill="hsl(' + hh + ',55%,7%)" stroke="hsl(' + hh + ',100%,66%)" stroke-width="3" stroke-linejoin="round" filter="url(#glow)"';
      return 'fill="hsl(' + hh + ',' + s + '%,' + Math.max(2, Math.min(98, Math.round(l))) + '%)"' + (op ? ' fill-opacity="' + op + '"' : '');
    }
    function layer(content) {
      if (water) return '<g filter="url(#soft)" opacity="0.92">' + content + '</g>';
      if (paper) return '<g filter="url(#shadow)">' + content + '</g>';
      return '<g>' + content + '</g>';
    }
    function dim(l, i) { return night ? Math.max(6, l - 22 + i * 3) : l; }
    var out = [];
    var defs = '<defs>' +
      '<linearGradient id="sky" x1="0" y1="0" x2="0" y2="1">' + (line ? '<stop offset="0" stop-color="#f6f3ea"/><stop offset="1" stop-color="#f6f3ea"/>' :
        neon ? '<stop offset="0" stop-color="hsl(' + hue + ',70%,5%)"/><stop offset="1" stop-color="hsl(' + ((hue + 50) % 360) + ',70%,14%)"/>' :
        night ? '<stop offset="0" stop-color="hsl(' + hue + ',55%,9%)"/><stop offset="1" stop-color="hsl(' + ((hue + 30) % 360) + ',55%,28%)"/>' :
        warm ? '<stop offset="0" stop-color="hsl(' + ((hue + 330) % 360) + ',55%,38%)"/><stop offset="0.65" stop-color="hsl(' + hue + ',90%,62%)"/><stop offset="1" stop-color="hsl(' + ((hue + 28) % 360) + ',95%,78%)"/>' :
        '<stop offset="0" stop-color="hsl(' + hue + ',65%,62%)"/><stop offset="1" stop-color="hsl(' + ((hue + 25) % 360) + ',85%,88%)"/>') + '</linearGradient>' +
      '<filter id="glow" x="-30%" y="-30%" width="160%" height="160%"><feGaussianBlur stdDeviation="5" result="b"/><feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge></filter>' +
      '<filter id="soft" x="-10%" y="-10%" width="120%" height="120%"><feGaussianBlur stdDeviation="3.2"/></filter>' +
      '<filter id="shadow" x="-10%" y="-10%" width="120%" height="130%"><feDropShadow dx="0" dy="-6" stdDeviation="6" flood-color="#000" flood-opacity="0.35"/></filter>' +
      '</defs>';
    out.push('<rect width="' + W + '" height="' + H + '" fill="url(#sky)"/>');

    /* stars */
    if (night) {
      var st = '';
      for (var i = 0; i < 70; i++) st += '<circle cx="' + n(rnd(0, W)) + '" cy="' + n(rnd(0, H * 0.6)) + '" r="' + n(rnd(0.8, 2.6)) + '" fill="' + (line ? ink : '#fff') + '" fill-opacity="' + n(rnd(0.4, 1)) + '"/>';
      out.push(st);
    }
    /* celestial */
    var cx0 = rnd(W * 0.22, W * 0.78), cy0 = H * rnd(0.22, 0.36), cr = Math.min(W, H) * rnd(0.07, 0.11);
    if (has(/planet|galaxy|saturn|space/)) {
      out.push(layer('<circle cx="' + n(cx0) + '" cy="' + n(cy0) + '" r="' + n(cr * 1.3) + '" ' + paint(hue + 40, 70, 60) + '/><ellipse cx="' + n(cx0) + '" cy="' + n(cy0) + '" rx="' + n(cr * 2.3) + '" ry="' + n(cr * 0.5) + '" fill="none" stroke="' + (line ? ink : 'hsl(' + ((hue + 80) % 360) + ',90%,75%)') + '" stroke-width="5" transform="rotate(-18 ' + n(cx0) + ' ' + n(cy0) + ')"/>'));
    } else if (night || has(/moon/)) {
      out.push('<circle cx="' + n(cx0) + '" cy="' + n(cy0) + '" r="' + n(cr * 1.8) + '" fill="#fff" fill-opacity="' + (line || neon ? '0' : '0.08') + '"/>' + layer('<circle cx="' + n(cx0) + '" cy="' + n(cy0) + '" r="' + n(cr) + '" ' + (neon || line ? paint(55, 90, 80) : 'fill="hsl(50,60%,90%)"') + '/>' + (line || neon ? '' : '<circle cx="' + n(cx0 - cr * 0.3) + '" cy="' + n(cy0 - cr * 0.2) + '" r="' + n(cr * 0.2) + '" fill="hsl(50,25%,78%)"/><circle cx="' + n(cx0 + cr * 0.35) + '" cy="' + n(cy0 + cr * 0.3) + '" r="' + n(cr * 0.14) + '" fill="hsl(50,25%,78%)"/>')));
    } else {
      out.push((line || neon ? '' : '<circle cx="' + n(cx0) + '" cy="' + n(cy0) + '" r="' + n(cr * 2.2) + '" fill="hsl(45,100%,70%)" fill-opacity="0.25"/><circle cx="' + n(cx0) + '" cy="' + n(cy0) + '" r="' + n(cr * 1.5) + '" fill="hsl(45,100%,70%)" fill-opacity="0.3"/>') + layer('<circle cx="' + n(cx0) + '" cy="' + n(cy0) + '" r="' + n(cr) + '" ' + (neon || line ? paint(45, 100, 62) : 'fill="hsl(45,100%,62%)"') + '/>'));
    }
    /* clouds */
    if (!night && (has(/cloud|sky|bird|day|sun|mountain/) || R() > 0.5)) {
      var cl = '';
      for (var c = 0; c < 3; c++) {
        var ccx = rnd(W * 0.1, W * 0.9), ccy = H * rnd(0.12, 0.3), s = rnd(0.7, 1.4) * Math.min(W, H) / 800;
        cl += '<g ' + (line ? 'fill="none" stroke="' + ink + '" stroke-width="3"' : 'fill="#fff" fill-opacity="0.85"') + '><ellipse cx="' + n(ccx) + '" cy="' + n(ccy) + '" rx="' + n(70 * s) + '" ry="' + n(24 * s) + '"/><ellipse cx="' + n(ccx - 40 * s) + '" cy="' + n(ccy + 6 * s) + '" rx="' + n(44 * s) + '" ry="' + n(18 * s) + '"/><ellipse cx="' + n(ccx + 44 * s) + '" cy="' + n(ccy + 8 * s) + '" rx="' + n(46 * s) + '" ry="' + n(17 * s) + '"/></g>';
      }
      out.push(layer(cl));
    }
    function ridge(y0, amp, steps) {
      var pts = ['M0,' + H, 'L0,' + n(y0)], dx = W / steps;
      for (var i = 1; i <= steps; i++) pts.push('L' + n(i * dx) + ',' + n(y0 - rnd(0, amp)));
      pts.push('L' + W + ',' + H + ' Z');
      return '<path d="' + pts.join(' ') + '" ';
    }
    var sea = has(/ocean|sea |water|wave|beach|lighthouse|ship|boat|fish|sail/);
    var desert = has(/pyramid|egypt|desert|sand|camel|dune/);
    var city = has(/city|town|building|skyline|cyber|tower|neon/);
    var forest = has(/forest|tree|jungle|woods|pine|nature/);
    var garden = has(/flower|garden|spring|rose|bloom/);
    var mountains = has(/mountain|hill|volcano|snow|peak|alps/) || !(sea || desert || city || garden);

    if (mountains) {
      for (var m = 0; m < 3; m++) {
        var hh = hue + m * 12, base = H * (0.62 + m * 0.1), amp = H * (0.22 - m * 0.05);
        out.push(layer(ridge(base, amp, 7 + m * 3) + paint(hh, 38 - m * 4, dim(has(/snow|winter|ice/) ? 86 - m * 14 : 58 - m * 14, m)) + '/>'));
      }
    }
    if (desert) {
      out.push(layer(ridge(H * 0.74, H * 0.05, 10) + paint(40, 70, dim(62, 0)) + '/>'));
      var px = [0.3, 0.52, 0.72];
      var py = '';
      px.forEach(function (fx, i) {
        var bw = Math.min(W, H) * (0.34 - i * 0.07), bh = bw * 0.82, bx = W * fx - bw / 2, by = H * (0.78 + i * 0.01);
        py += '<path d="M' + n(bx) + ',' + n(by) + ' L' + n(bx + bw / 2) + ',' + n(by - bh) + ' L' + n(bx + bw) + ',' + n(by) + ' Z" ' + paint(38, 75, dim(66 - i * 6, i)) + '/>' +
          (line || neon ? '' : '<path d="M' + n(bx + bw / 2) + ',' + n(by - bh) + ' L' + n(bx + bw) + ',' + n(by) + ' L' + n(bx + bw / 2) + ',' + n(by) + ' Z" fill="hsl(32,70%,' + dim(48 - i * 5, i) + '%)"/>');
      });
      out.push(layer(py));
      out.push(layer(ridge(H * 0.86, H * 0.04, 8) + paint(34, 70, dim(52, 1)) + '/>'));
    }
    if (sea) {
      var top = H * 0.62;
      out.push(layer('<rect x="0" y="' + n(top) + '" width="' + W + '" height="' + n(H - top) + '" ' + paint(hue, 70, dim(40, 0)) + '/>'));
      var wv = '';
      for (var r = 0; r < 7; r++) {
        var y = top + (H - top) * (r + 0.6) / 7.5, d = 'M0,' + n(y), amp2 = 5 + r * 2, per = 70 + r * 22;
        for (var x = 0; x <= W; x += 10) d += ' L' + x + ',' + n(y + Math.sin((x / per) + r + variant) * amp2);
        wv += '<path d="' + d + '" fill="none" stroke="' + (line ? ink : neon ? 'hsl(' + hue + ',100%,70%)' : 'hsl(' + hue + ',80%,' + (night ? 55 : 82) + '%)') + '" stroke-width="' + n(2 + r * 0.5) + '" stroke-opacity="0.65"/>';
      }
      out.push(wv);
      if (has(/boat|ship|sail/)) {
        var bx2 = rnd(W * 0.25, W * 0.75), by2 = top + (H - top) * 0.35, sz = Math.min(W, H) * 0.14;
        out.push(layer('<path d="M' + n(bx2 - sz * 0.6) + ',' + n(by2) + ' L' + n(bx2 + sz * 0.6) + ',' + n(by2) + ' L' + n(bx2 + sz * 0.4) + ',' + n(by2 + sz * 0.22) + ' L' + n(bx2 - sz * 0.4) + ',' + n(by2 + sz * 0.22) + ' Z" ' + paint(20, 60, dim(34, 0)) + '/><path d="M' + n(bx2) + ',' + n(by2 - sz) + ' L' + n(bx2 + sz * 0.5) + ',' + n(by2 - sz * 0.08) + ' L' + n(bx2) + ',' + n(by2 - sz * 0.08) + ' Z" ' + paint(0, 0, dim(96, 0)) + '/>'));
      }
      if (has(/lighthouse/)) {
        var lx = W * 0.28, ly = top + (H - top) * 0.08, lw = Math.min(W, H) * 0.07, lh = Math.min(W, H) * 0.34;
        var lt = '<path d="M' + n(lx - lw * 1.8) + ',' + n(ly + 10) + ' L' + n(lx - lw * 0.7) + ',' + n(ly - lh * 0.1) + ' L' + n(lx + lw * 0.7) + ',' + n(ly - lh * 0.1) + ' L' + n(lx + lw * 1.8) + ',' + n(ly + 10) + ' Z" ' + paint(220, 15, dim(30, 0)) + '/>';
        lt += '<path d="M' + n(lx - lw * 0.7) + ',' + n(ly - lh * 0.1) + ' L' + n(lx - lw * 0.45) + ',' + n(ly - lh) + ' L' + n(lx + lw * 0.45) + ',' + n(ly - lh) + ' L' + n(lx + lw * 0.7) + ',' + n(ly - lh * 0.1) + ' Z" ' + paint(0, 0, dim(94, 0)) + '/>';
        lt += '<rect x="' + n(lx - lw * 0.55) + '" y="' + n(ly - lh * 0.62) + '" width="' + n(lw * 1.1) + '" height="' + n(lh * 0.14) + '" ' + paint(5, 80, dim(50, 0)) + '/>';
        lt += '<rect x="' + n(lx - lw * 0.55) + '" y="' + n(ly - lh * 0.34) + '" width="' + n(lw * 1.1) + '" height="' + n(lh * 0.14) + '" ' + paint(5, 80, dim(50, 0)) + '/>';
        lt += '<rect x="' + n(lx - lw * 0.5) + '" y="' + n(ly - lh * 1.14) + '" width="' + n(lw) + '" height="' + n(lh * 0.14) + '" ' + paint(50, 100, 66) + '/>';
        out.push((line ? '' : '<path d="M' + n(lx + lw * 0.5) + ',' + n(ly - lh * 1.07) + ' L' + W + ',' + n(ly - lh * 1.5) + ' L' + W + ',' + n(ly - lh * 0.7) + ' Z" fill="hsl(50,100%,75%)" fill-opacity="0.25"/>') + layer(lt));
      }
    }
    if (city) {
      var bd = '', x = -10, k = 0;
      while (x < W) {
        var bw2 = rnd(40, 90), bh2 = rnd(H * 0.16, H * 0.46) * (0.6 + 0.4 * Math.sin(k + variant) ** 2);
        bd += '<rect x="' + n(x) + '" y="' + n(H * 0.86 - bh2) + '" width="' + n(bw2) + '" height="' + n(bh2 + H * 0.2) + '" ' + paint(hue + k * 6, 35, dim(30 + (k % 3) * 6, k % 3)) + '/>';
        if (!line) for (var wy = H * 0.86 - bh2 + 14; wy < H * 0.84; wy += 22) for (var wx = x + 8; wx < x + bw2 - 10; wx += 16) if (R() > 0.55) bd += '<rect x="' + n(wx) + '" y="' + n(wy) + '" width="7" height="10" fill="' + (night ? 'hsl(48,100%,70%)' : 'hsl(' + hue + ',50%,88%)') + '" fill-opacity="0.9"/>';
        x += bw2 + rnd(2, 8); k++;
      }
      out.push(layer(bd));
    }
    if (forest) {
      var tr = '';
      for (var t = 0; t < 20; t++) {
        var tx = rnd(0, W), ty = H * rnd(0.74, 0.95), ts = rnd(0.7, 1.5) * Math.min(W, H) / 800 * 60;
        tr += '<rect x="' + n(tx - ts * 0.08) + '" y="' + n(ty) + '" width="' + n(ts * 0.16) + '" height="' + n(ts * 0.4) + '" ' + paint(25, 40, dim(26, 0)) + '/>';
        for (var tl = 0; tl < 3; tl++) tr += '<path d="M' + n(tx - ts * (0.7 - tl * 0.12)) + ',' + n(ty - ts * tl * 0.55 + ts * 0.1) + ' L' + n(tx) + ',' + n(ty - ts * (tl + 1) * 0.62) + ' L' + n(tx + ts * (0.7 - tl * 0.12)) + ',' + n(ty - ts * tl * 0.55 + ts * 0.1) + ' Z" ' + paint(135 + tl * 8, 50, dim(28 + tl * 8, tl)) + '/>';
      }
      out.push(layer(tr));
    }
    if (garden) {
      out.push(layer(ridge(H * 0.84, H * 0.04, 9) + paint(120, 45, dim(40, 0)) + '/>'));
      var fl = '';
      for (var f = 0; f < 18; f++) {
        var fx = rnd(W * 0.04, W * 0.96), fy = H * rnd(0.6, 0.86), fs = rnd(14, 30) * Math.min(W, H) / 800 * 1.6, fh = hue + rnd(-30, 40);
        fl += '<path d="M' + n(fx) + ',' + n(fy + fs * 3) + ' L' + n(fx) + ',' + n(fy) + '" stroke="' + (line ? ink : 'hsl(125,45%,' + dim(32, 0) + '%)') + '" stroke-width="3" fill="none"/>';
        for (var pt = 0; pt < 6; pt++) { var a = pt * Math.PI / 3; fl += '<circle cx="' + n(fx + Math.cos(a) * fs) + '" cy="' + n(fy + Math.sin(a) * fs) + '" r="' + n(fs * 0.7) + '" ' + paint(fh, 80, dim(66, 0)) + '/>'; }
        fl += '<circle cx="' + n(fx) + '" cy="' + n(fy) + '" r="' + n(fs * 0.55) + '" ' + (line ? paint(0, 0, 0) : neon ? paint(50, 100, 60) : 'fill="hsl(46,100%,60%)"') + '/>';
      }
      out.push(layer(fl));
    }
    if (has(/bird|fly|eagle|sky|gull/) || R() > 0.6) {
      var bs = '';
      for (var bi = 0; bi < 4; bi++) { var bx3 = rnd(W * 0.1, W * 0.9), by3 = H * rnd(0.1, 0.38), bz = rnd(12, 26); bs += '<path d="M' + n(bx3 - bz) + ',' + n(by3) + ' Q' + n(bx3 - bz / 2) + ',' + n(by3 - bz * 0.7) + ' ' + n(bx3) + ',' + n(by3) + ' Q' + n(bx3 + bz / 2) + ',' + n(by3 - bz * 0.7) + ' ' + n(bx3 + bz) + ',' + n(by3) + '" fill="none" stroke="' + (night && !line ? '#fff' : ink) + '" stroke-width="3" stroke-linecap="round" stroke-opacity="0.8"/>'; }
      out.push(bs);
    }
    /* foreground */
    if (!sea && !city) out.push(layer(ridge(H * 0.93, H * 0.035, 12) + paint(hue + 20, 30, dim(20, 0)) + '/>'));
    return '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 ' + W + ' ' + H + '">' + defs + out.join('') + '</svg>';
  }

  /* ---------- chats ---------- */
  function newChat(projectId) {
    var c = { id: uid(), title: 'New chat', projectId: projectId || '', mode: 'talk', msgs: [], ts: Date.now(), upd: Date.now() };
    S.chats.unshift(c); S.active = c.id; save();
    setView('chat');
    renderSide(); renderChat();
    closeDrawer();
    $('#input').focus();
  }
  function resetChats() {
    S.chats = []; S.active = null; pending = []; arts = []; nodes.urls = {};
    try { window.speechSynthesis.cancel(); } catch (e) {}
    try { if (typeof rec !== 'undefined' && rec && listening) rec.abort(); } catch (e) {}
    var inp = $('#input'); if (inp) { inp.value = ''; autosize(); }
    $('#msgs').innerHTML = ''; $('#chat-list').innerHTML = ''; renderPending();
  }
  function openChat(id) { S.active = id; save(); setView('chat'); renderSide(); renderChat(); closeDrawer(); }

  function dayLabel(ts) {
    var d = new Date(ts), n = new Date();
    var a = new Date(d.getFullYear(), d.getMonth(), d.getDate()).getTime();
    var b = new Date(n.getFullYear(), n.getMonth(), n.getDate()).getTime();
    var diff = Math.round((b - a) / 86400000);
    if (diff <= 0) return 'Today';
    if (diff === 1) return 'Yesterday';
    if (diff < 7) return 'This week';
    if (diff < 31) return 'This month';
    return 'Earlier';
  }

  function renderSide() {
    $('#side-name').textContent = nick();
    $('#who').textContent = uname();
    $('#avatar').textContent = (uname().trim()[0] || 'L').toUpperCase();
    var nav = $('#nav'); nav.innerHTML = '';
    [['chat', 'Chat', 'chat'], ['projects', 'Projects', 'folder'], ['library', 'Library', 'book'], ['create', 'Create', 'spark'], ['memory', 'Memory', 'brain']].forEach(function (n) {
      var b = h('button', { type: 'button', html: svgIcon(n[2]) + '<span>' + n[1] + '</span>' });
      if (view === n[0]) b.setAttribute('aria-current', 'page');
      b.addEventListener('click', function () { setView(n[0]); closeDrawer(); });
      nav.appendChild(b);
    });
    var q = $('#chat-q').value.trim().toLowerCase();
    var list = $('#chat-list'); list.innerHTML = '';
    var chats = S.chats.slice().sort(function (a, b) { return b.upd - a.upd; }).filter(function (c) {
      if (!q) return true;
      return c.title.toLowerCase().indexOf(q) > -1 || c.msgs.some(function (m) { return (m.text || '').toLowerCase().indexOf(q) > -1; });
    });
    var lastG = '';
    chats.forEach(function (c) {
      var g = dayLabel(c.upd);
      if (g !== lastG) { list.appendChild(h('div', { class: 'grp', text: g })); lastG = g; }
      var item = h('button', { class: 'chat-item', type: 'button' }, [
        c.projectId ? h('i', { class: 'dot', title: 'In a project' }) : null,
        h('span', { class: 't', text: c.title })
      ]);
      if (c.id === S.active && view === 'chat') item.setAttribute('aria-current', 'true');
      item.addEventListener('click', function () { openChat(c.id); });
      list.appendChild(item);
    });
    if (!chats.length) list.appendChild(h('div', { class: 'grp', text: q ? 'No chats match' : 'No chats yet' }));
  }

  function suggestions(mode) {
    return mode === 'code'
      ? ['Make a to-do app in one HTML file', 'Explain what a closure is with an example', 'Write a Python script that renames files by date', 'Build a CSS loading animation']
      : ['Plan my study week', 'Explain black holes like I am twelve', 'Help me write a polite message', 'Quiz me on world capitals'];
  }

  function avatarEl(role) {
    return role === 'user'
      ? h('span', { class: 'avatar', text: (uname().trim()[0] || 'Y').toUpperCase() })
      : h('span', { class: 'orb sm', 'aria-hidden': 'true' });
  }
  function actBtn(icon, act, label) { return h('button', { class: 'icon-btn', type: 'button', 'data-act': act, 'aria-label': label, title: label, html: svgIcon(icon, 16) }); }

  function msgEl(m) {
    var bubble = h('div', { class: 'bubble' });
    var body = h('div', { class: 'mbody' }, [h('div', { class: 'who', text: m.role === 'user' ? uname() : nick() }), bubble]);
    if (m.att && m.att.length) {
      var wrap = h('div', { class: 'atts' });
      m.att.forEach(function (a) {
        var url = nodes.urls && nodes.urls[a.id];
        var thumb = a.kind === 'image' && url ? h('img', { src: url, alt: '' }) : h('span', { class: 'ph', html: svgIcon(a.kind === 'image' ? 'camera' : 'file', 16) });
        wrap.appendChild(h('span', { class: 'att' }, [thumb, h('span', { class: 'n', text: a.name })]));
      });
      body.appendChild(wrap);
    }
    var acts = h('div', { class: 'acts' });
    acts.appendChild(actBtn('copy', 'copy', 'Copy message'));
    if (m.role === 'assistant') { acts.appendChild(actBtn('speaker', 'speak', 'Read aloud')); acts.appendChild(actBtn('bookmark', 'save', 'Save to library')); }
    acts.appendChild(actBtn('pin', 'remember', 'Remember this'));
    body.appendChild(acts);
    var el = h('div', { class: 'msg ' + m.role, 'data-id': m.id }, [avatarEl(m.role), body]);
    fillBubble(m, bubble, acts);
    return el;
  }
  function fillBubble(m, bubble, acts) {
    if (m.role === 'user') { bubble.textContent = m.text; return; }
    if (!m.text) { bubble.innerHTML = '<span class="think"><i></i><i></i><i></i></span>'; if (acts) acts.hidden = true; return; }
    bubble.innerHTML = md(m.text);
    if (acts) acts.hidden = !!m.streaming;
  }

  function addChips(el, m) {
    var old = el.querySelector('.qchips'); if (old) old.remove();
    if (!m.chips || !m.chips.length) return;
    var row = h('div', { class: 'qchips' });
    m.chips.forEach(function (t) { row.appendChild(h('button', { type: 'button', text: t, onclick: function () { if (busy) return; $('#input').value = t; autosize(); send(); } })); });
    el.querySelector('.mbody').appendChild(row);
  }
  function nearBottom(el) { return el.scrollHeight - el.scrollTop - el.clientHeight < 140; }
  function scrollDown(force) { var el = $('#msgs'); if (force || nearBottom(el)) el.scrollTop = el.scrollHeight; }

  function renderChat() {
    var c = cur();
    if (!c) { newChat(); return; }
    $('#chat-title').value = c.title;
    var sel = $('#chat-project'); sel.innerHTML = '';
    sel.appendChild(h('option', { value: '', text: 'No project' }));
    S.projects.forEach(function (p) { var o = h('option', { value: p.id, text: p.name }); if (p.id === c.projectId) o.selected = true; sel.appendChild(o); });
    $('#mode-talk').setAttribute('aria-pressed', c.mode !== 'code');
    $('#mode-code').setAttribute('aria-pressed', c.mode === 'code');
    $('#input').placeholder = c.mode === 'code' ? 'Describe what to build or paste code to fix' : 'Message ' + nick();
    var box = $('#msgs'); box.innerHTML = '';
    if (!c.msgs.length) {
      var sug = h('div', { class: 'suggest' });
      suggestions(c.mode).forEach(function (s) { sug.appendChild(h('button', { type: 'button', text: s, onclick: function () { $('#input').value = s; autosize(); send(); } })); });
      box.appendChild(h('div', { class: 'empty' }, [
        h('span', { class: 'orb xl', 'aria-hidden': 'true' }),
        h('h2', { text: c.mode === 'code' ? 'What are we building, ' + uname() + '?' : 'Hi ' + uname() + ', I am ' + nick() + '.' }),
        h('p', { text: c.mode === 'code' ? 'Ask for code, bug fixes or explanations. Web pages can be previewed right here.' : 'Ask me anything, attach a photo or file, or tap the mic and talk.' }),
        sug
      ]));
    } else {
      c.msgs.forEach(function (m, i) { var el = msgEl(m); box.appendChild(el); if (i === c.msgs.length - 1 && m.role === 'assistant' && !m.streaming) addChips(el, m); });
    }
    renderPending();
    updateSend();
    scrollDown(true);
  }

  function renderPending() {
    var box = $('#pending'); box.innerHTML = '';
    pending.forEach(function (p) {
      var thumb = p.kind === 'image' ? h('img', { src: p.url, alt: '' }) : h('span', { class: 'ph', html: svgIcon('file', 16) });
      var x = h('button', { class: 'icon-btn', type: 'button', 'aria-label': 'Remove ' + p.name, html: svgIcon('x', 14), style: 'width:26px;height:26px' });
      x.addEventListener('click', function () { pending = pending.filter(function (q) { return q !== p; }); renderPending(); });
      box.appendChild(h('span', { class: 'att' }, [thumb, h('span', { class: 'n', text: p.name }), x]));
    });
  }

  function updateSend() {
    var b = $('#btn-send');
    b.innerHTML = svgIcon(busy ? 'stop' : 'send');
    b.setAttribute('aria-label', busy ? 'Stop' : 'Send message');
    b.disabled = false;
  }
  function autosize() { var t = $('#input'); t.style.height = 'auto'; t.style.height = Math.min(t.scrollHeight, 180) + 'px'; }

  function speakText(text, onend) {
    try {
      var plain = text.replace(/```[\s\S]*?(```|$)/g, ' code block omitted. ').replace(/[*_`#>]/g, '');
      window.speechSynthesis.cancel();
      var u = new SpeechSynthesisUtterance(plain.slice(0, 1800));
      if (onend) { u.onend = onend; u.onerror = onend; }
      window.speechSynthesis.speak(u);
    } catch (e) { toast('Reading aloud is not available in this browser.'); if (onend) onend(); }
  }

  var NOTE_ATT = ['txt', 'md', 'csv', 'json', 'js', 'ts', 'py', 'html', 'css', 'xml', 'yml', 'yaml', 'java', 'c', 'cpp', 'cs', 'go', 'rs', 'php', 'rb', 'sh', 'sql', 'log'];
  function addFiles(list) {
    Array.prototype.forEach.call(list || [], function (f) {
      var ext = (f.name.split('.').pop() || '').toLowerCase();
      if (/^image\//.test(f.type)) {
        pending.push({ id: uid(), kind: 'image', name: f.name || 'photo', blob: f, url: URL.createObjectURL(f) });
        renderPending();
      } else if (/^text\//.test(f.type) || NOTE_ATT.indexOf(ext) > -1) {
        f.text().then(function (t) {
          pending.push({ id: uid(), kind: 'text', name: f.name, text: t.slice(0, 30000) });
          renderPending();
          if (t.length > 30000) toast(f.name + ' is long, so the first 30,000 characters will be used.');
        }, function () { toast('Could not read ' + f.name + '.'); });
      } else {
        toast('Vektrin reads photos and plain text or code files. Convert ' + f.name + ' to text first.');
      }
    });
  }

  function send(textOverride) {
    if (busy) { if (ctl) ctl.abort(); return; }
    var c = cur(); if (!c) return;
    var input = $('#input');
    var text = (typeof textOverride === 'string' ? textOverride : input.value).trim();
    if (!text && !pending.length) return;
    var images = [], extra = [], att = [], files = [];
    pending.forEach(function (p) {
      if (p.kind === 'image') { images.push(p.blob); att.push({ id: p.id, name: p.name, kind: 'image' }); nodes.urls = nodes.urls || {}; nodes.urls[p.id] = p.url; }
      else { extra.push('[File: ' + p.name + ']\n' + p.text); files.push({ name: p.name, text: p.text }); att.push({ id: p.id, name: p.name, kind: 'file' }); }
    });
    var shown = text || (images.length ? 'What do you see in this?' : 'Please look at the attached file.');
    var um = { id: uid(), role: 'user', text: shown, ts: Date.now(), att: att };
    if (extra.length) um.ctx = shown + '\n\n' + extra.join('\n\n');
    if (images.length) um.ctx = (um.ctx || shown) + '\n\n(The person attached ' + images.length + ' photo' + (images.length > 1 ? 's' : '') + ' to this message. Look at them to answer.)';
    c.msgs.push(um);
    var rm = shown.match(/^(?:please\s+)?remember(?:\s+that)?[:,]?\s+(.{3,})/i);
    if (rm) { S.memory.unshift({ id: uid(), text: rm[1].slice(0, 400), ts: Date.now() }); toast('Added to memory.'); }
    if (c.title === 'New chat') c.title = shown.replace(/\s+/g, ' ').slice(0, 44);
    c.upd = Date.now();
    var am = { id: uid(), role: 'assistant', text: '', streaming: true, ts: Date.now() };
    c.msgs.push(am);
    input.value = ''; autosize(); pending = []; renderPending();
    save(); renderSide();
    var box = $('#msgs');
    box.querySelectorAll('.qchips').forEach(function (n) { n.remove(); });
    if (c.msgs.length === 2) box.innerHTML = '';
    box.appendChild(msgEl(um));
    var ael = msgEl(am); box.appendChild(ael);
    var bubble = ael.querySelector('.bubble'), acts = ael.querySelector('.acts');
    scrollDown(true);
    busy = true; updateSend();
    ctl = new AbortController();
    var myChat = c.id;

    var imgJobs = images.map(function (b, k) { return analyzeImage(b, att.filter(function (a) { return a.kind === 'image'; })[k].name); });
    Promise.all(imgJobs).then(function (infos) {
      var pr = S.projects.filter(function (x) { return x.id === c.projectId; })[0] || null;
      var answer = Brain.reply(c, shown, {
        profile: S.profile, memory: S.memory, project: pr, files: files, images: infos, taught: S.taught, web: S.profile.web !== false,
        rename: function (v) { S.profile.name = v; }, setCreator: function (v) { S.profile.creator = v; }, renick: function (v) { S.profile.nick = v; },
        remember: function (t) { var k = t.toLowerCase(); if (S.memory.some(function (x) { return x.text.toLowerCase() === k; })) return false; S.memory.unshift({ id: uid(), text: t.slice(0, 400), ts: Date.now() }); return true; },
        teach: function (k, a) { for (var i = S.taught.length - 1; i >= 0; i--) if (S.taught[i].k === k) S.taught.splice(i, 1); S.taught.unshift({ id: uid(), k: k, a: a.slice(0, 400), ts: Date.now() }); }
      });
      var cb = function (t) { am.text = t; fillBubble(am, bubble, acts); if (S.active === myChat) scrollDown(false); };
      var w = c._web; c._web = null;
      if (!w) return Brain.typewrite(answer, ctl.signal, cb);
      if (S.profile.web === false) return Brain.typewrite((w.fallback || 'I do not know that.') + '\n\nWeb lookups are turned off in Settings.', ctl.signal, cb);
      am.text = 'Searching the web...'; fillBubble(am, bubble, acts);
      return Web.run(w, ctl.signal).then(function (res) {
        c.webFull = res.full || null;
        return Brain.typewrite(res.text, ctl.signal, cb);
      }, function (e) {
        if (e && e.code === 'cancelled') { am.text = ''; throw { code: 'cancelled', text: '' }; }
        var why = e && e.code === 'empty' ? 'I searched but found nothing useful for "' + w.q + '".' : 'I tried to look it up, but I cannot reach the internet right now. Maybe you are offline, or this page is blocked from the web.';
        c.chips = [];
        return Brain.typewrite(why + '\n\n' + (w.fallback || '') + '\n\n' + Web.ddg(w.q), ctl.signal, cb);
      });
    }).then(function (full) {
      am.text = full; am.streaming = false; am.chips = (c.chips || []).slice();
    }).catch(function (e) {
      am.streaming = false;
      if (e && e.code === 'cancelled') { am.text = e.text || am.text; if (!am.text) { am.text = 'Stopped.'; am.err = true; } }
      else { am.text = 'Something went wrong inside my brain. Try rephrasing that.'; am.err = true; }
    }).then(function () {
      busy = false; ctl = null; updateSend();
      c.upd = Date.now(); save();
      if (S.active === myChat) { fillBubble(am, bubble, acts); acts.hidden = false; addChips(ael, am); scrollDown(false); }
      renderSide(); $('#input').placeholder = (c.mode === 'code' ? 'Describe what to build or paste code to fix' : 'Message ' + nick());
      if (!am.err && (voiceChat || (S.profile && S.profile.speak))) speakText(am.text, function () { if (voiceChat) startListening(); });
    });
  }

  /* messages: delegated actions */
  $('#msgs').addEventListener('click', function (e) {
    var btn = e.target.closest('[data-act]'); if (!btn) return;
    var act = btn.dataset.act;
    var c = cur();
    if (act.indexOf('cb-') === 0) {
      var cb = btn.closest('.cb'); var code = cb.querySelector('pre').textContent;
      var lang = cb.querySelector('.cbh span').textContent;
      if (act === 'cb-copy') copyText(code);
      else if (act === 'cb-save') { addLib({ type: 'code', title: lang + ' snippet', body: code, lang: lang }); toast('Saved to your library.'); }
      else if (act === 'cb-run') previewCode(code, lang);
      return;
    }
    var mel = btn.closest('.msg'); if (!mel || !c) return;
    var m = c.msgs.filter(function (x) { return x.id === mel.dataset.id; })[0]; if (!m) return;
    if (act === 'copy') copyText(m.text);
    else if (act === 'speak') speakText(m.text);
    else if (act === 'save') { addLib({ type: 'reply', title: (c.title || 'Reply').slice(0, 50), body: m.text }); toast('Saved to your library.'); }
    else if (act === 'remember') { S.memory.unshift({ id: uid(), text: m.text.replace(/\s+/g, ' ').slice(0, 300), ts: Date.now() }); save(); toast('Added to memory.'); }
  });

  function copyText(t) {
    function fb() { toast('Select the text and press Ctrl+C or Cmd+C to copy.'); }
    if (navigator.clipboard && navigator.clipboard.writeText) navigator.clipboard.writeText(t).then(function () { toast('Copied.'); }, fb); else fb();
  }
  function addLib(item) { item.id = uid(); item.ts = Date.now(); S.library.unshift(item); save(); if (view === 'library') renderLibrary(); }

  /* ---------- modal ---------- */
  function openModal(title, body, actions, wide) {
    $('#modal-title').textContent = title;
    $('#sheet').classList.toggle('wide', !!wide);
    var mb = $('#modal-body'); mb.innerHTML = ''; mb.appendChild(body);
    var mf = $('#modal-foot'); mf.innerHTML = '';
    (actions || []).forEach(function (a) {
      var b = h('button', { class: 'btn ' + (a.kind || ''), type: 'button', text: a.label });
      b.addEventListener('click', function () { var r = a.run ? a.run(b) : true; if (r !== false) closeModal(); });
      mf.appendChild(b);
    });
    $('#modal').hidden = false;
    var f = mb.querySelector('input,textarea,select,button'); if (f) f.focus();
  }
  function closeModal() { $('#modal').hidden = true; $('#modal-body').innerHTML = ''; }
  $('#modal-x').addEventListener('click', closeModal);
  $('#modal').addEventListener('mousedown', function (e) { if (e.target === $('#modal')) closeModal(); });

  function previewCode(code, lang) {
    var doc = lang === 'svg' ? '<!doctype html><body style="margin:0;display:grid;place-items:center;min-height:100vh;background:#fff">' + code : code;
    var fr = h('iframe', { title: 'Preview', sandbox: 'allow-scripts' });
    fr.srcdoc = doc;
    openModal('Preview', h('div', { style: 'display:flex;flex-direction:column;flex:1;min-height:0;gap:8px' }, [fr, h('p', { class: 'note', text: 'This runs in a sandbox with no network access, so it cannot reach other sites.' })]), [{ label: 'Close' }], true);
  }

  /* ---------- views ---------- */
  var TITLES = { chat: 'Chat', projects: 'Projects', library: 'Library', create: 'Create', memory: 'Memory' };
  function setView(v) {
    if (v === 'chat' && !cur()) { newChat(); return; }
    view = v;
    ['chat', 'projects', 'library', 'create', 'memory'].forEach(function (n) { $('#v-' + n).hidden = n !== v; });
    var c = cur();
    $('#top-title').textContent = v === 'chat' && c ? c.title : TITLES[v];
    if (v === 'projects') renderProjects();
    if (v === 'library') renderLibrary();
    if (v === 'memory') renderMemory();
    if (v === 'create') renderArts();
    renderSide();
  }

  /* projects */
  function projectModal(p) {
    var isNew = !p;
    var name = h('input', { type: 'text', id: 'p-name', maxlength: '60', value: p ? p.name : '', placeholder: 'Spanish practice' });
    var ins = h('textarea', { id: 'p-ins', placeholder: 'Talk to me in simple Spanish, correct my mistakes kindly, and end with one new word.' });
    ins.value = p ? p.instructions : '';
    var body = h('div', { class: 'form' }, [
      h('div', { class: 'field' }, [h('label', { for: 'p-name', text: 'Project name' }), name]),
      h('div', { class: 'field' }, [h('label', { for: 'p-ins', text: 'Instructions for every chat in this project' }), ins])
    ]);
    openModal(isNew ? 'New project' : 'Edit project', body, [
      { label: 'Cancel' },
      { label: isNew ? 'Create project' : 'Save', kind: 'primary', run: function () {
        var n = name.value.trim(); if (!n) { toast('Give the project a name.'); return false; }
        if (isNew) S.projects.push({ id: uid(), name: n, instructions: ins.value.trim() || 'Be helpful and clear.', ts: Date.now() });
        else { p.name = n; p.instructions = ins.value.trim() || 'Be helpful and clear.'; }
        save(); renderProjects(); if (view === 'chat') renderChat();
      } }
    ]);
  }
  function renderProjects() {
    var g = $('#proj-grid'); g.innerHTML = '';
    if (!S.projects.length) { g.appendChild(h('div', { class: 'card' }, [h('h3', { text: 'No projects yet' }), h('p', { class: 'd', text: 'Try a project for homework, a game idea or a language you are learning. Add instructions once and every chat inside follows them.' })])); return; }
    S.projects.forEach(function (p) {
      var chats = S.chats.filter(function (c) { return c.projectId === p.id; }).sort(function (a, b) { return b.upd - a.upd; });
      var mini = h('div', { class: 'mini' });
      chats.slice(0, 4).forEach(function (c) { mini.appendChild(h('button', { type: 'button', text: c.title, onclick: function () { openChat(c.id); } })); });
      var del = h('button', { class: 'btn sm danger', type: 'button', text: 'Delete' });
      del.addEventListener('click', function () { arm(del, function () { S.projects = S.projects.filter(function (x) { return x.id !== p.id; }); S.chats.forEach(function (c) { if (c.projectId === p.id) c.projectId = ''; }); save(); renderProjects(); }); });
      g.appendChild(h('div', { class: 'card' }, [
        h('h3', { text: p.name }),
        h('p', { class: 'd', text: p.instructions.length > 140 ? p.instructions.slice(0, 140) + '...' : p.instructions }),
        h('span', { class: 'tag', text: chats.length + (chats.length === 1 ? ' chat' : ' chats') }),
        mini,
        h('div', { class: 'row' }, [
          h('button', { class: 'btn sm primary', type: 'button', text: 'New chat', onclick: function () { newChat(p.id); } }),
          h('button', { class: 'btn sm', type: 'button', text: 'Edit', onclick: function () { projectModal(p); } }),
          del
        ])
      ]));
    });
  }

  /* library */
  var LIB_TYPES = [['all', 'All'], ['reply', 'Replies'], ['code', 'Code'], ['art', 'Art'], ['note', 'Notes']];
  function renderLibrary() {
    var f = $('#lib-filters'); f.innerHTML = '';
    LIB_TYPES.forEach(function (t) {
      var b = h('button', { class: 'btn sm', type: 'button', text: t[1], 'aria-pressed': libFilter === t[0] });
      b.addEventListener('click', function () { libFilter = t[0]; renderLibrary(); });
      f.appendChild(b);
    });
    var q = $('#lib-q').value.trim().toLowerCase();
    var g = $('#lib-grid'); g.innerHTML = '';
    var items = S.library.filter(function (it) { return (libFilter === 'all' || it.type === libFilter) && (!q || (it.title + ' ' + (it.type === 'art' ? '' : it.body)).toLowerCase().indexOf(q) > -1); });
    if (!items.length) { g.appendChild(h('div', { class: 'card' }, [h('h3', { text: S.library.length ? 'Nothing matches' : 'Your library is empty' }), h('p', { class: 'd', text: S.library.length ? 'Try another filter or search.' : 'Tap the bookmark under a reply or code block in a chat, or save artwork from Create.' })])); return; }
    items.forEach(function (it) {
      var preview;
      if (it.type === 'art') preview = h('img', { class: 'art', alt: it.title, src: svgUri(it.body) });
      else if (it.type === 'code') preview = h('pre', { text: it.body });
      else preview = h('p', { class: 'd', text: it.body.length > 220 ? it.body.slice(0, 220) + '...' : it.body });
      var del = h('button', { class: 'btn sm danger', type: 'button', text: 'Delete' });
      del.addEventListener('click', function () { arm(del, function () { S.library = S.library.filter(function (x) { return x.id !== it.id; }); save(); renderLibrary(); }); });
      var row = h('div', { class: 'row' });
      if (it.type !== 'art') row.appendChild(h('button', { class: 'btn sm primary', type: 'button', text: 'Use in chat', onclick: function () {
        newChat(); $('#input').value = (it.type === 'code' ? 'Here is some code. Please help me improve it:\n\n```' + (it.lang || '') + '\n' + it.body + '\n```' : 'Let us continue from this:\n\n' + it.body); autosize();
      } }));
      row.appendChild(h('button', { class: 'btn sm', type: 'button', text: it.type === 'art' ? 'Copy SVG' : 'Copy', onclick: function () { copyText(it.body); } }));
      if (it.type === 'art') row.appendChild(h('button', { class: 'btn sm', type: 'button', text: 'Save file', onclick: function () { saveSvg(it.body, it.title); } }));
      row.appendChild(del);
      g.appendChild(h('div', { class: 'card' }, [h('span', { class: 'tag', text: it.type }), h('h3', { text: it.title }), preview, row]));
    });
  }
  $('#lib-q').addEventListener('input', renderLibrary);
  $('#lib-note').addEventListener('click', function () {
    var t = h('input', { type: 'text', id: 'n-title', maxlength: '60', placeholder: 'Title' });
    var b = h('textarea', { id: 'n-body', placeholder: 'Write or paste anything you want to keep' });
    openModal('Add a note', h('div', { class: 'form' }, [h('div', { class: 'field' }, [h('label', { for: 'n-title', text: 'Title' }), t]), h('div', { class: 'field' }, [h('label', { for: 'n-body', text: 'Note' }), b])]), [
      { label: 'Cancel' },
      { label: 'Save note', kind: 'primary', run: function () { if (!b.value.trim()) { toast('Write something first.'); return false; } addLib({ type: 'note', title: t.value.trim() || 'Note', body: b.value.trim() }); } }
    ]);
  });

  /* create (vector art) */
  function svgUri(svg) { return 'data:image/svg+xml;charset=utf-8,' + encodeURIComponent(svg); }
  function cleanSvg(txt) {
    var m = String(txt).match(/<svg[\s\S]*<\/svg>/i); if (!m) return null;
    var s = m[0]
      .replace(/<script[\s\S]*?<\/script>/gi, '')
      .replace(/<foreignObject[\s\S]*?<\/foreignObject>/gi, '')
      .replace(/<image[\s\S]*?(\/>|<\/image>)/gi, '')
      .replace(/\son\w+\s*=\s*("[^"]*"|'[^']*')/gi, '')
      .replace(/(xlink:)?href\s*=\s*("(?!#)[^"]*"|'(?!#)[^']*')/gi, '');
    if (!/xmlns\s*=/.test(s)) s = s.replace(/<svg/i, '<svg xmlns="http://www.w3.org/2000/svg"');
    return s.length > 200000 ? null : s;
  }
  function saveSvg(svg, title) {
    var name = (title || 'vektrin-art').toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/^-|-$/g, '').slice(0, 40) || 'vektrin-art';
    downloadBlob(new Blob([svg], { type: 'image/svg+xml' }), name + '.svg');
  }
  function downloadBlob(blob, filename) {
    try {
      var a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = filename;
      document.body.appendChild(a); a.click(); a.remove();
      setTimeout(function () { URL.revokeObjectURL(a.href); }, 3000);
      toast('Saved to your downloads.');
    } catch (e) { toast('Saving is blocked here. Use Copy instead.'); }
  }
  function renderArts() {
    var g = $('#art-grid'); g.innerHTML = '';
    arts.forEach(function (a) {
      if (a.loading) { g.appendChild(h('div', { class: 'card' }, [h('div', { class: 'skeleton' }), h('p', { class: 'd', text: 'Drawing: ' + a.prompt })])); return; }
      if (a.error) { g.appendChild(h('div', { class: 'card' }, [h('h3', { text: 'That one did not work' }), h('p', { class: 'd', text: a.error })])); return; }
      g.appendChild(h('div', { class: 'card' }, [
        h('img', { class: 'art', alt: a.prompt, src: svgUri(a.svg) }),
        h('p', { class: 'd', text: a.prompt + ' (' + a.style + ')' }),
        h('div', { class: 'row' }, [
          h('button', { class: 'btn sm primary', type: 'button', text: 'Save to library', onclick: function () { addLib({ type: 'art', title: a.prompt.slice(0, 50), body: a.svg }); toast('Saved to your library.'); } }),
          h('button', { class: 'btn sm', type: 'button', text: 'Save file', onclick: function () { saveSvg(a.svg, a.prompt); } }),
          h('button', { class: 'btn sm', type: 'button', text: 'Copy SVG', onclick: function () { copyText(a.svg); } })
        ])
      ]));
    });
  }
  $('#art-go').addEventListener('click', function () {
    var prompt = $('#art-prompt').value.trim();
    if (!prompt) { toast('Describe what the picture should show.'); return; }
    var style = $('#art-style').value, ratio = $('#art-ratio').value;
    var vb = ratio === 'wide' ? '0 0 1200 675' : ratio === 'tall' ? '0 0 675 1200' : '0 0 800 800';
    var card = { id: uid(), prompt: prompt, style: style, loading: true };
    arts.unshift(card); renderArts();
    $('#art-go').hidden = true; $('#art-stop').hidden = false;
    artCtl = { abort: function () { clearTimeout(artTimer); arts = arts.filter(function (a) { return a !== card; }); $('#art-go').hidden = false; $('#art-stop').hidden = true; artCtl = null; renderArts(); } };
    var artTimer = setTimeout(function () {
      try { card.svg = makeArt(prompt, style, ratio, Math.floor(Math.random() * 1e6)); card.loading = false; }
      catch (e) { card.loading = false; card.error = 'The generator hit a problem. Try a different description.'; }
      $('#art-go').hidden = false; $('#art-stop').hidden = true; artCtl = null; renderArts();
    }, 600);

  });
  $('#art-stop').addEventListener('click', function () { if (artCtl) artCtl.abort(); });

  /* memory */
  function renderMemory() {
    $('#mem-on').checked = !S.profile || S.profile.useMemory !== false;
    var l = $('#mem-list'); l.innerHTML = '';
    if (!S.memory.length) l.appendChild(h('div', { class: 'card' }, [h('h3', { text: 'Nothing remembered yet' }), h('p', { class: 'd', text: 'Add a fact above, or type "remember that I prefer short answers" in any chat.' })]));
    S.memory.forEach(function (m) {
      var del = h('button', { class: 'icon-btn', type: 'button', 'aria-label': 'Forget this', html: svgIcon('trash', 16) });
      del.addEventListener('click', function () { arm(del, function () { S.memory = S.memory.filter(function (x) { return x.id !== m.id; }); save(); renderMemory(); }); });
      l.appendChild(h('div', { class: 'mem' }, [h('p', { text: m.text }), del]));
    });
    var tl = $('#taught-list'); tl.innerHTML = '';
    if (!S.taught.length) tl.appendChild(h('div', { class: 'card' }, [h('p', { class: 'd', text: 'Nothing taught yet.' })]));
    S.taught.forEach(function (t) {
      var del = h('button', { class: 'icon-btn', type: 'button', 'aria-label': 'Delete this answer', html: svgIcon('trash', 16) });
      del.addEventListener('click', function () { arm(del, function () { S.taught = S.taught.filter(function (x) { return x.id !== t.id; }); save(); renderMemory(); }); });
      tl.appendChild(h('div', { class: 'mem' }, [h('p', { html: '<strong>' + esc(t.k) + '</strong> &rarr; ' + esc(t.a) }), del]));
    });
    var bytes = 0; try { bytes = JSON.stringify(S).length; } catch (e) {}
    var kb = Math.round(bytes / 1024);
    $('#mem-usage').textContent = S.memory.length + (S.memory.length === 1 ? ' memory' : ' memories') + '. Vektrin sets no cap. Your browser allows about 5,000 KB for everything here and you are using about ' + kb + ' KB.';
    $('#mem-meter').style.width = Math.min(100, (kb / 5000) * 100) + '%';
  }
  $('#mem-add').addEventListener('click', function () {
    var i = $('#mem-new'); var t = i.value.trim(); if (!t) return;
    S.memory.unshift({ id: uid(), text: t.slice(0, 400), ts: Date.now() }); i.value = ''; save(); renderMemory();
  });
  $('#mem-new').addEventListener('keydown', function (e) { if (e.key === 'Enter') $('#mem-add').click(); });
  $('#mem-on').addEventListener('change', function (e) { if (S.profile) { S.profile.useMemory = e.target.checked; save(); } });
  $('#mem-clear').addEventListener('click', function (e) { arm(e.currentTarget, function () { S.memory = []; S.taught = []; resetChats(); persistNow(); renderMemory(); renderSide(); toast('Memory and all chats were deleted.'); }); });

  /* ---------- settings + welcome ---------- */
  function themePicker(container, current, onPick) {
    container.innerHTML = '';
    Object.keys(THEMES).forEach(function (k) {
      var t = THEMES[k];
      var b = h('button', { class: 'sw', type: 'button', 'aria-pressed': k === current, 'aria-label': t.label + ' theme' }, [
        h('span', { class: 'bar', style: 'background:linear-gradient(135deg,' + t.a + ',' + t.b + '),' + t.bg }), h('span', { text: t.label })
      ]);
      b.addEventListener('click', function () { onPick(k); Array.prototype.forEach.call(container.children, function (c) { c.setAttribute('aria-pressed', c === b); }); });
      container.appendChild(b);
    });
  }
  function settingsModal() {
    var p = S.profile;
    var name = h('input', { type: 'text', id: 's-name', maxlength: '30', value: p.name });
    var nk = h('input', { type: 'text', id: 's-nick', maxlength: '24', value: p.nick });
    var themes = h('div', { class: 'swatches' });
    var chosen = p.theme;
    themePicker(themes, chosen, function (k) { chosen = k; applyTheme(k); });
    var spk = h('input', { type: 'checkbox', id: 's-speak' }); spk.checked = !!p.speak;
    var web = h('input', { type: 'checkbox', id: 's-web' }); web.checked = p.web !== false;
    var erase = h('button', { class: 'btn danger sm', type: 'button', text: 'Erase everything', 'data-confirm': 'Tap again to erase everything' });
    erase.addEventListener('click', function () { arm(erase, function () { clearTimeout(saveT); S = fresh(); resetChats(); try { localStorage.removeItem(KEY); } catch (e) {} closeModal(); showWelcome(); toast('Everything was erased.'); }); });
    var out = h('button', { class: 'btn sm', type: 'button', text: 'Sign out and delete chats', 'data-confirm': 'Tap again to sign out' });
    out.addEventListener('click', function () { arm(out, function () { S.last = { name: p.name, nick: p.nick, theme: p.theme }; S.profile = null; resetChats(); persistNow(); closeModal(); showWelcome(); toast('Signed out. All chats were deleted.'); }); });
    var body = h('div', { class: 'form' }, [
      h('div', { class: 'field' }, [h('label', { for: 's-name', text: 'Your name' }), name]),
      h('div', { class: 'field' }, [h('label', { for: 's-nick', text: 'Assistant nickname' }), nk]),
      h('div', { class: 'field' }, [h('label', { text: 'Look' }), themes]),
      h('label', { class: 'switch', for: 's-speak' }, [spk, h('span', { text: 'Read replies aloud' })]),
      h('label', { class: 'switch', for: 's-web' }, [web, h('span', { text: 'Look things up on the web when I do not know (your question is sent to Wikipedia or another public site)' })]),
      h('div', { class: 'row-inline' }, [h('button', { class: 'btn sm', type: 'button', text: 'Download backup', onclick: function () { downloadBlob(new Blob([JSON.stringify(S)], { type: 'application/json' }), 'lumo-backup.json'); } }),
        h('button', { class: 'btn sm', type: 'button', text: 'Restore backup', onclick: function () { $('#restore-in').click(); } })]),
      h('div', { class: 'row-inline' }, [out, erase]),
      h('p', { class: 'note', text: 'Everything is stored in this browser only. Signing out deletes all chats on this device. Memory, projects and the library stay unless you erase everything.' })
    ]);
    openModal('Settings', body, [
      { label: 'Cancel', run: function () { applyTheme(p.theme); } },
      { label: 'Save', kind: 'primary', run: function () {
        p.name = name.value.trim() || 'You'; p.nick = nk.value.trim() || 'Vektrin'; p.theme = chosen; p.speak = spk.checked; p.web = web.checked;
        save(); applyTheme(chosen); renderSide(); renderChat(); if (view === 'projects') renderProjects();
      } }
    ]);
  }
  $('#open-settings').addEventListener('click', settingsModal);
  $('#restore-in').addEventListener('change', function (e) {
    var f = e.target.files[0]; e.target.value = ''; if (!f) return;
    f.text().then(function (t) {
      var j = JSON.parse(t);
      if (!j || !Array.isArray(j.chats) || !j.profile) throw new Error('bad');
      S = Object.assign(fresh(), j); save(); closeModal(); $('#welcome').hidden = true; boot(); toast('Backup restored.');
    }).catch(function () { toast('That file is not a Vektrin backup.'); });
  });

  var wTheme = 'daylight';
  function showWelcome() {
    var last = S.last || {};
    $('#w-name').value = last.name && last.name !== 'Guest' ? last.name : '';
    $('#w-nick').value = last.nick || '';
    wTheme = last.theme || systemTheme();
    applyTheme(wTheme);
    themePicker($('#w-themes'), wTheme, function (k) { wTheme = k; applyTheme(k); });
    $('#welcome').hidden = false;
    setTimeout(function () { $('#w-name').focus(); }, 50);
  }
  function startProfile(guest) {
    var name = guest ? 'Guest' : ($('#w-name').value.trim() || 'Friend');
    var nk = $('#w-nick').value.trim() || 'Vektrin';
    S.profile = { name: name, nick: nk, theme: wTheme, speak: false, useMemory: true, web: true };
    save(); $('#welcome').hidden = true; boot();
  }
  $('#w-go').addEventListener('click', function () { startProfile(false); });
  $('#w-guest').addEventListener('click', function () { startProfile(true); });
  $('#w-name').addEventListener('keydown', function (e) { if (e.key === 'Enter') $('#w-nick').focus(); });
  $('#w-nick').addEventListener('keydown', function (e) { if (e.key === 'Enter') startProfile(false); });

  /* ---------- drawer ---------- */
  function openDrawer() { $('#side').classList.add('open'); $('#scrim').classList.add('show'); }
  function closeDrawer() { $('#side').classList.remove('open'); $('#scrim').classList.remove('show'); }
  $('#menu-btn').addEventListener('click', openDrawer);
  $('#side-close').addEventListener('click', closeDrawer);
  $('#scrim').addEventListener('click', closeDrawer);
  document.addEventListener('keydown', function (e) { if (e.key === 'Escape') { if (!$('#modal').hidden) closeModal(); else closeDrawer(); } });

  /* ---------- wiring ---------- */
  $('#new-chat').addEventListener('click', function () { newChat(); });
  $('#top-new').addEventListener('click', function () { newChat(); });
  $('#proj-new').addEventListener('click', function () { projectModal(null); });
  $('#chat-q').addEventListener('input', renderSide);
  $('#chat-title').addEventListener('input', function (e) { var c = cur(); if (c) { c.title = e.target.value || 'New chat'; save(); renderSide(); $('#top-title').textContent = c.title; } });
  $('#chat-project').addEventListener('change', function (e) { var c = cur(); if (c) { c.projectId = e.target.value; save(); renderSide(); } });
  $('#mode-talk').addEventListener('click', function () { var c = cur(); if (c) { c.mode = 'talk'; save(); renderChat(); } });
  $('#mode-code').addEventListener('click', function () { var c = cur(); if (c) { c.mode = 'code'; save(); renderChat(); } });
  $('#chat-del').addEventListener('click', function (e) {
    arm(e.currentTarget, function () {
      var id = S.active; S.chats = S.chats.filter(function (c) { return c.id !== id; });
      S.active = S.chats.length ? S.chats.slice().sort(function (a, b) { return b.upd - a.upd; })[0].id : null;
      save(); if (!S.active) newChat(); else { renderSide(); renderChat(); }
      var btn = $('#chat-del'); btn.innerHTML = svgIcon('trash'); btn.classList.remove('armed');
    });
  });

  var input = $('#input');
  input.addEventListener('input', autosize);
  input.addEventListener('keydown', function (e) {
    if (e.key === 'Enter' && !e.shiftKey && !e.isComposing && window.matchMedia('(pointer: fine)').matches) { e.preventDefault(); send(); }
  });
  $('#btn-send').addEventListener('click', function () { send(); });
  $('#btn-file').addEventListener('click', function () { $('#file-in').click(); });
  $('#btn-cam').addEventListener('click', function () { $('#cam-in').click(); });
  $('#file-in').addEventListener('change', function (e) { addFiles(e.target.files); e.target.value = ''; });
  $('#cam-in').addEventListener('change', function (e) { addFiles(e.target.files); e.target.value = ''; });
  input.addEventListener('paste', function (e) {
    var files = e.clipboardData && e.clipboardData.files; if (files && files.length) { addFiles(files); }
  });
  var composer = $('.composer');
  ['dragover', 'drop'].forEach(function (ev) { composer.addEventListener(ev, function (e) { e.preventDefault(); if (ev === 'drop' && e.dataTransfer) addFiles(e.dataTransfer.files); }); });

  /* voice: the browser's own speech recognition (Chrome, Edge, Safari) */
  var SR = window.SpeechRecognition || window.webkitSpeechRecognition, rec = null, listening = false, dictBase = '', micDenied = false;
  function joinTxt(base, t) { if (!t) return base; if (!base) return t.replace(/^\s+/, ''); return base + (/\s$/.test(base) || /^\s/.test(t) ? '' : ' ') + t; }
  function setMic(on) { listening = on; var mic = $('#btn-mic'); mic.classList.toggle('on', on); mic.setAttribute('aria-pressed', on); }
  function startListening() {
    if (!SR || listening || micDenied) return;
    try {
      rec = new SR(); rec.interimResults = true; rec.continuous = false; rec.lang = navigator.language || 'en-US';
      dictBase = input.value;
      rec.onresult = function (e) {
        var fin = '', tmp = '';
        for (var i = 0; i < e.results.length; i++) { if (e.results[i].isFinal) fin += e.results[i][0].transcript; else tmp += e.results[i][0].transcript; }
        input.value = joinTxt(dictBase, fin + tmp); autosize();
      };
      rec.onerror = function (e) {
        if (e.error === 'not-allowed' || e.error === 'service-not-allowed') { micDenied = true; toast('The microphone is blocked. Allow it in your browser, or type instead.'); }
        else if (e.error === 'network') toast('This browser needs the internet for speech recognition. Typing still works offline.');
      };
      rec.onend = function () { setMic(false); if (voiceChat && input.value.trim()) send(); };
      rec.start(); setMic(true);
    } catch (e) { setMic(false); toast('Voice typing is not available in this browser.'); }
  }
  function initVoice() { if (SR) { $('#btn-mic').hidden = false; } $('#btn-voice').hidden = false; }
  $('#btn-mic').addEventListener('click', function () {
    if (listening && rec) { try { rec.stop(); } catch (e) {} return; }
    micDenied = false; startListening();
  });
  $('#btn-voice').addEventListener('click', function (e) {
    voiceChat = !voiceChat;
    var b = e.currentTarget; b.classList.toggle('on', voiceChat); b.setAttribute('aria-pressed', voiceChat);
    $('#hintline').textContent = voiceChat ? (SR ? 'Voice chat is on. Tap the mic, talk, and Vektrin answers out loud, then listens again.' : 'Voice chat is on. Replies are read aloud. Your browser has no speech recognition, so type your messages.') : 'Enter to send, Shift+Enter for a new line';
    if (!voiceChat) { try { window.speechSynthesis.cancel(); } catch (x) {} if (listening && rec) { try { rec.abort(); } catch (x) {} } }
  });

  /* ---------- boot ---------- */
  function boot() {
    applyTheme(S.profile.theme);
    if (!S.chats.length) { var c = { id: uid(), title: 'New chat', projectId: '', mode: 'talk', msgs: [], ts: Date.now(), upd: Date.now() }; S.chats.push(c); S.active = c.id; save(); }
    if (!cur()) S.active = S.chats.slice().sort(function (a, b) { return b.upd - a.upd; })[0].id;
    setView('chat');
    renderChat();
    initVoice();
  }

  paintIcons();
  if (S.profile) boot(); else { showWelcome(); }
})();
</script>

</body>
</html>
