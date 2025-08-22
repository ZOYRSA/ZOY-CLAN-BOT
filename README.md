# ZOY-CLAN-BOT
Zoy Clan Bot Explore South African Clans,praises, and origins
<meta name="google-site-verification" content="XXXXXXXXXXXXXXXXXXXX" />


<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Zoy Clan Bot – SA Clan Names</title>
<meta name="description" content="Zoy Clan Bot – Explore South African clan names, praises, and origins. Add your own clan and chat with the bot!">
<meta name="keywords" content="South African clans, Zoy Clan Bot, isiXhosa clans, isiZulu clans, clan praises, Mpisi, Dlamini, Ndlovu">
<meta name="author" content="Yanga Mlindazwe">
<style>
:root {
  --bg: #0f1116; --card: #171923; --text: #e6e6e6; --muted: #a0a0a0;
  --accent: #8ae66e; --radius: 12px;
}
* { box-sizing: border-box; }
body { margin:0; padding:24px; font-family: system-ui, sans-serif; background: radial-gradient(1200px 600px at 20% -10%, #1b2135, var(--bg)); color: var(--text); }
h1{margin:0 0 4px;font-size:1.6rem;}
p{margin:0;color:var(--muted);}
.wrap{max-width:900px;margin:0 auto;}
.search, .chatbox, .add-clan{background:var(--card);padding:16px;border-radius:var(--radius);margin-bottom:16px;border:1px solid #22283a;}
input[type=text], textarea{width:100%;padding:12px 14px;border-radius:10px;border:1px solid #252b3e;background:#0e111a;color:var(--text);outline:none;font-size:1rem; margin-bottom:8px;}
button{padding:12px 16px;border-radius:10px;border:1px solid #2b3248;background:linear-gradient(180deg,#1e263c,#131a2e);color:var(--text);cursor:pointer;font-size:1rem;}
button:hover{filter:brightness(1.1);}
.grid{display:grid;gap:16px;margin-top:16px;}
.card{background:linear-gradient(180deg,#151a2b,#111524);border:1px solid #20263a;border-radius:var(--radius);padding:16px;}
.chat-window{height:300px;overflow-y:auto;border:1px solid #2a2f45;border-radius:10px;padding:10px;background:#0e111a;margin-bottom:10px;}
.message{margin:6px 0;padding:8px 12px;border-radius:10px;}
.user{background:#1f2a3a;text-align:right;}
.bot{background:#24303f;}
.footer{margin-top:28px;color:var(--muted);font-size:0.9rem;line-height:1.5;}
</style>
</head>
<body>
<div class="wrap">
<header>
  <h1>Zoy Clan Bot – SA Clan Names</h1>
  <p>Search, chat, or add clans to learn about South African clans, praises, and origins.</p>
</header>

<section>
<h2>About Zoy Clan Bot</h2>
<p>Zoy Clan Bot lets you explore South African clans, learn praises, origins, and add your own clan. Start by searching or chatting with the bot below.</p>
</section>

<!-- Search Section -->
<section class="search">
  <input id="q" type="text" placeholder="Try: Dlamini, Tshawe, Khumalo, Mokoena, Ndlovu..." />
  <button id="btn">Search</button>
</section>

<div id="results" class="grid"></div>

<!-- Chatbot Section -->
<section class="chatbox">
  <div class="chat-window" id="chatWindow"></div>
  <input id="chatInput" type="text" placeholder="Ask Zoy Clan Bot a question...">
  <button id="chatSend">Send</button>
</section>

<!-- Add Your Clan Section -->
<section class="add-clan">
  <h2>Add Your Clan</h2>
  <input id="newSurname" placeholder="Surname">
  <input id="newLang" placeholder="Language group (comma-separated)">
  <input id="newType" placeholder="Clan type (e.g., Iziduko)">
  <input id="newPrimary" placeholder="Primary clan name">
  <textarea id="newPraises" placeholder="Praises (semicolon-separated)"></textarea>
  <textarea id="newOrigin" placeholder="Origin summary"></textarea>
  <input id="newEtymology" placeholder="Etymology/Meaning">
  <input id="newRegions" placeholder="Regions (comma-separated)">
  <textarea id="newNotes" placeholder="Notes"></textarea>
  <button id="addClanBtn">Add Clan</button>
  <p id="addClanMessage" style="color:var(--accent);margin-top:8px;"></p>
</section>

<div class="footer">
  <strong>Notes & cultural care:</strong>
  <ul>
    <li>Clan praises vary by house, region, and lineage. Always confirm with elders.</li>
    <li>Submissions are saved offline via your browser. Permanent edits require modifying clans.json.</li>
    <li>Zoy Clan Bot explains clan info respectfully using your dataset.</li>
  </ul>
  <div class="small">Prototype v0.4 — offline & client-side only.</div>
</div>
</div>

<script>
// Load dataset
let clans = [];
async function loadData() {
  try {
    const res = await fetch('clans.json');
    if(!res.ok) throw new Error('Could not load clans.json.');
    clans = await res.json();
    // Load saved clans from localStorage
    const saved = localStorage.getItem('userClans');
    if(saved) clans = clans.concat(JSON.parse(saved));
  } catch(e) { alert(e.message); }
}

// Render search results
function render(items){
  const root=document.getElementById('results'); root.innerHTML='';
  if(!items.length){root.innerHTML='<div class="card">No results. Try another surname or spelling.</div>'; return;}
  items.forEach(item=>{
    const languages=(item.language_group||[]).join(', ');
    const regions=(item.regions||[]).join(', ');
    const praises=(item.praises||[]).join(', ');
    const el=document.createElement('div'); el.className='card';
    el.innerHTML=`
      <h2>${item.primary_clan_name} (${item.surname})</h2>
      <div><strong>Clan Type:</strong> ${item.clan_type}</div>
      <div><strong>Language Group:</strong> ${languages}</div>
      <div><strong>Praises:</strong> ${praises}</div>
      <div><strong>Origin:</strong> ${item.origin_summary||'—'}</div>
      <div><strong>Meaning:</strong> ${item.etymology_or_meaning||'—'}</div>
      <div><strong>Regions:</strong> ${regions||'—'}</div>
      <div><strong>Notes:</strong> ${item.notes||'—'}</div>
    `;
    root.appendChild(el);
  });
}

// Chatbot reply
function chatReply(query){
  const lower=query.toLowerCase();
  const item=clans.find(d=>d.surname.toLowerCase()===lower || (d.primary_clan_name||'').toLowerCase()===lower);
  if(item){
    return `✨ ${item.surname} belongs to the *${item.primary_clan_name}* clan. `+
           `Their praises include ${item.praises.join(', ')}. `+
           `Origin: ${item.origin_summary}. `+
           (item.etymology_or_meaning?`Meaning: ${item.etymology_or_meaning}. `:'')+
           `Regions: ${item.regions.join(', ')}. Notes: ${item.notes}.`;
  }else{
    return "❌ Sorry, Zoy Clan Bot doesn't have this clan yet. You can add it below!";
  }
}

// Add clan with LocalStorage
function addClan(newClan){
  clans.push(newClan);
  let saved = localStorage.getItem('userClans');
  let arr = saved ? JSON.parse(saved) : [];
  arr.push(newClan);
  localStorage.setItem('userClans', JSON.stringify(arr));
}

// Initialize
(async function init(){
  await loadData();

  // Search
  const input=document.getElementById('q'); const btn=document.getElementById('btn');
  function search(){const q=(input.value||'').trim().toLowerCase(); if(!q){render([]);return;}
    const items=clans.filter(d=>d.surname.toLowerCase().includes(q) || (d.primary_clan_name||'').toLowerCase().includes(q));
    render(items);
  }
  btn.addEventListener('click',search);
  input.addEventListener('keydown',e=>{if(e.key==='Enter')search();});

  // Chatbot
  const chatWindow=document.getElementById('chatWindow'); const chatInput=document.getElementById('chatInput'); const chatSend=document.getElementById('chatSend');
  function appendMessage(text,cls){const div=document.createElement('div');div.className='message '+cls;div.innerHTML=text;chatWindow.appendChild(div);chatWindow.scrollTop=chatWindow.scrollHeight;}
  chatSend.addEventListener('click',()=>{
    const msg=chatInput.value.trim(); if(!msg) return;
    appendMessage(msg,'user');
    const reply=chatReply(msg);
    appendMessage(reply,'bot');
    chatInput.value='';
  });
  chatInput.addEventListener('keydown',e=>{if(e.key==='Enter'){chatSend.click();}});

  // Add Your Clan
  const addBtn=document.getElementById('addClanBtn'); const addMsg=document.getElementById('addClanMessage');
  addBtn.addEventListener('click',()=>{
    const newClan={
      surname: document.getElementById('newSurname').value.trim(),
      language_group: document.getElementById('newLang').value.split(',').map(s=>s.trim()),
      clan_type: document.getElementById('newType').value.trim(),
      primary_clan_name: document.getElementById('newPrimary').value.trim(),
      praises: document.getElementById('newPraises').value.split(';').map(s=>s.trim()),
      origin_summary: document.getElementById('newOrigin').value.trim(),
      etymology_or_meaning: document.getElementById('newEtymology').value.trim(),
      regions: document.getElementById('newRegions').value.split(',').map(s=>s.trim()),
      notes: document.getElementById('newNotes').value.trim()
    };
    if(!newClan.surname || !newClan.primary_clan_name){addMsg.textContent='⚠️ Surname and Primary Clan Name are required.'; return;}
    addClan(newClan);
    addMsg.textContent='✅ Clan added and saved locally. Please verify with elders.';
    ['newSurname','newLang','newType','newPrimary','newPraises','newOrigin','newEtymology','newRegions','newNotes'].forEach(id=>document.getElementById(id).value='');
  });
})();
</script>
</body>
</html>


On Mon, 11 Dec 2023, 07:08 Yanga Zoy Mlindazwe, <yangagunxwana@gmail.com> wrote:

[
  {
    "surname": "Dlamini",
    "primary_clan_name": "Dlamini",
    "clan_type": "Zulu",
    "language_group": ["Zulu", "Swazi"],
    "praises": ["Zulu kaBhekuzulu", "Sihlangu seDlamini"],
    "origin_summary": "Originates from Swazi and Zulu royalty lines.",
    "etymology_or_meaning": "Dlamini means 'descendants of Dlamini'.",
    "regions": ["KwaZulu-Natal", "Swaziland"],
    "notes": "One of the largest clans in Southern Africa."
  },
  {
    "surname": "Mpisi",
    "primary_clan_name": "Mpisi",
    "clan_type": "Xhosa",
    "language_group": ["Xhosa"],
    "praises": ["oGadlume", "Bumbantaba", "NoMathe"],
    "origin_summary": "Situated in Bizana, KwaMpisi area.",
    "etymology_or_meaning": "Mpisi means 'hyena', symbolic of strength.",
    "regions": ["Eastern Cape"],
    "notes": "Rural clan with strong traditional leadership."
  },
  {
    "surname": "Khumalo",
    "primary_clan_name": "Khumalo",
    "clan_type": "Zulu",
    "language_group": ["Zulu"],
    "praises": ["Khulu kaMntwana", "Shaka kaSenzangakhona"],
    "origin_summary": "Royal lineage from Zulu Kingdom.",
    "etymology_or_meaning": "Khumalo means 'greatness'.",
    "regions": ["KwaZulu-Natal", "Gauteng"],
    "notes": "Known for historical leadership in Zululand."
  },
  {
    "surname": "Tshawe",
    "primary_clan_name": "Tshawe",
    "clan_type": "Xhosa",
    "language_group": ["Xhosa"],
    "praises": ["AmaTshawe", "Ndaba kaTshawe"],
    "origin_summary": "Royal Xhosa clan from the Eastern Cape.",
    "etymology_or_meaning": "Tshawe means 'brave'.",
    "regions": ["Eastern Cape"],
    "notes": "Known for chiefs and traditional governance."
  },
  {
    "surname": "Mokoena",
    "primary_clan_name": "Mokoena",
    "clan_type": "Sotho",
    "language_group": ["Sotho"],
    "praises": ["SeMokoena", "BaMokoena"],
    "origin_summary": "Sotho clan originally from Free State region.",
    "etymology_or_meaning": "Mokoena means 'rock' or 'strong'.",
    "regions": ["Free State", "Gauteng"],
    "notes": "Large Sotho-speaking family group."
  },
  {
    "surname": "Ndlovu",
    "primary_clan_name": "Ndlovu",
    "clan_type": "Zulu/Xhosa",
    "language_group": ["Zulu", "Xhosa"],
    "praises": ["Inkosi yeNdlovu", "Ndlovu kaMntwana"],
    "origin_summary": "Found across Southern Africa; associated with strength.",
    "etymology_or_meaning": "Ndlovu means 'elephant', symbol of power.",
    "regions": ["KwaZulu-Natal", "Eastern Cape", "Mpumalanga"],
    "notes": "Common surname among Zulu and Xhosa people."
  },
  {
    "surname": "Ngcobo",
    "primary_clan_name": "Ngcobo",
    "clan_type": "Zulu",
    "language_group": ["Zulu"],
    "praises": ["Inkosi kaNgcobo", "BaNgcobo"],
    "origin_summary": "Zulu clan primarily in KwaZulu-Natal.",
    "etymology_or_meaning": "Ngcobo means 'leader'.",
    "regions": ["KwaZulu-Natal"],
    "notes": "Historically important in regional politics."
  },
  {
    "surname": "Radebe",
    "primary_clan_name": "Radebe",
    "clan_type": "Zulu",
    "language_group": ["Zulu"],
    "praises": ["uRadebe kaMntwana", "Abazimnyama"],
    "origin_summary": "Royal lineage from Zulu history.",
    "etymology_or_meaning": "Radebe means 'descendants of Radebe'.",
    "regions": ["KwaZulu-Natal", "Gauteng"],
    "notes": "Influential family in politics and culture."
  },
  {
    "surname": "Mthembu",
    "primary_clan_name": "Mthembu",
    "clan_type": "Zulu",
    "language_group": ["Zulu"],
    "praises": ["Mthembu kaZwide", "Abantu beMthembu"],
    "origin_summary": "Zulu clan from KwaZulu-Natal.",
    "etymology_or_meaning": "Mthembu means 'guardian' or 'protector'.",
    "regions": ["KwaZulu-Natal"],
    "notes": "Prominent in traditional leadership and history."
  },
  {
    "surname": "Ndwandwe",
    "primary_clan_name": "Ndwandwe",
    "clan_type": "Zulu",
    "language_group": ["Zulu"],
    "praises": ["AmaNdwandwe", "Ndwandwe kaZwide"],
    "origin_summary": "Historic Zulu clan involved in early conflicts with Shaka.",
    "etymology_or_meaning": "Ndwandwe means 'warriors'.",
    "regions": ["KwaZulu-Natal", "Mpumalanga"],
    "notes": "Historically significant in Zulu wars."
  },
  {
    "surname": "Zulu",
    "primary_clan_name": "Zulu",
    "clan_type": "Zulu",
    "language_group": ["Zulu"],
    "praises": ["Shaka kaSenzangakhona", "Inkosi yeZulu"],
    "origin_summary": "Royal Zulu clan; founders of the Zulu Kingdom.",
    "etymology_or_meaning": "Zulu means 'heaven' or 'sky'.",
    "regions": ["KwaZulu-Natal"],
    "notes": "One of the most famous clans in Southern Africa."
  },
  {
    "surname": "Mabena",
    "primary_clan_name": "Mabena",
    "clan_type": "Sotho/Tswana",
    "language_group": ["Sotho", "Tswana"],
    "praises": ["BaMabena", "Inkosi kaMabena"],
    "origin_summary": "Sotho-speaking clan found in Free State and Gauteng.",
    "etymology_or_meaning": "Mabena means 'stone or rock'.",
    "regions": ["Free State", "Gauteng"],
    "notes": "Traditional family with historic significance."
  }
]
