
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Zoy Clan Bot – Explore South African Clan Names & Praises</title>
<meta name="description" content="Zoy Clan Bot lets you explore South African clans, their praises, origins, meanings, and regions. Search or chat with the bot and add your own clan.">
<meta name="keywords" content="South African clans, Zoy Clan Bot, isiXhosa clans, isiZulu clans, clan praises, Mpisi, Dlamini, Ndlovu, South African history">
<meta name="author" content="Yanga Mlindazwe">
<meta name="robots" content="index, follow">

<!-- Google site verification -->
<meta name="google-site-verification" content="YOUR_GOOGLE_VERIFICATION_CODE" />

<!-- Structured data for SEO -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "Zoy Clan Bot",
  "url": "https://zoyclanbot.github.io/zoy-clan-bot/",
  "description": "Explore South African clans, their praises, origins, meanings, and regions. Search, chat, and add new clans.",
  "publisher": {
    "@type": "Person",
    "name": "Yanga Mlindazwe"
  }
}
</script>

<style>
:root { --bg:#0f1116;--card:#171923;--text:#e6e6e6;--muted:#a0a0a0;--accent:#8ae66e;--radius:12px; }
*{box-sizing:border-box;}
body{margin:0;padding:24px;font-family:system-ui,sans-serif;background:radial-gradient(1200px 600px at 20% -10%,#1b2135,var(--bg));color:var(--text);}
h1{margin:0 0 4px;font-size:1.6rem;}
h2{margin-top:24px;}
p,li{margin:0 0 8px;color:var(--muted);}
.wrap{max-width:900px;margin:0 auto;}
.search,.chatbox,.add-clan{background:var(--card);padding:16px;border-radius:var(--radius);margin-bottom:16px;border:1px solid #22283a;}
input[type=text],textarea{width:100%;padding:12px 14px;border-radius:10px;border:1px solid #252b3e;background:#0e111a;color:var(--text);outline:none;font-size:1rem;margin-bottom:8px;}
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

<section class="search">
  <input id="q" type="text" placeholder="Try: Dlamini, Tshawe, Khumalo, Mokoena, Ndlovu..." />
  <button id="btn">Search</button>
</section>

<div id="results" class="grid"></div>

<section class="chatbox">
  <div class="chat-window" id="chatWindow"></div>
  <input id="chatInput" type="text" placeholder="Ask Zoy Clan Bot a question...">
  <button id="chatSend">Send</button>
</section>

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
  <div class="small">Prototype v0.5 — offline & client-side only.</div>
</div>
</div>

<script>
// Load dataset
let clans=[];
async function loadData(){
  try{
    const res=await fetch('clans.json');
    if(!res.ok)throw new Error('Could not load clans.json.');
    clans=await res.json();
    const saved=localStorage.getItem('userClans');
    if(saved)clans=clans.concat(JSON.parse(saved));
  }catch(e){alert(e.message);}
}

// Render search results
function render(items){
  const root=document.getElementById('results');root.innerHTML='';
  if(!items.length){root.innerHTML='<div class="card">No results. Try another surname or spelling.</div>';return;}
  items.forEach(item=>{
    const languages=(item.language_group||[]).join(', ');
    const regions=(item.regions||[]).join(', ');
    const praises=(item.praises||[]).join(', ');
    const el=document.createElement('div');el.className='card';
    el.innerHTML=`
      <h2>${item.primary_clan_name} (${item.surname})</h2>
      <div><strong>Clan Type:</strong> ${item.clan_type}</div>
      <div><strong>Language Group:</strong> ${languages}</div>
      <div><strong>Praises:</strong> ${praises}</div>
      <div><strong>Origin:</strong> ${item.origin_summary||'—'}</div>
      <div><strong>Meaning:</strong> ${item.etymology_or_meaning||'—'}</div>
      <div><strong>Regions:</strong> ${regions||'—'}</div>
      <div><strong>Notes:</strong> ${item.notes||'—'}</div>`;
    root.appendChild(el);
  });
}

// Chatbot reply
function chatReply(query){
  const lower=query.toLowerCase();
  const item=clans.find(d=>d.surname.toLowerCase()===lower||(d.primary_clan_name||'').toLowerCase()===lower);
  if(item){
    return `✨ ${item.surname} belongs to the *${item.primary_clan_name}* clan. Praises: ${item.praises.join(', ')}. Origin: ${item.origin_summary}. Meaning: ${item.etymology_or_meaning||'—'}. Regions: ${item.regions.join(', ')||'—'}. Notes: ${item.notes||'—'}.`;
  }else{
    return "❌ Sorry, Zoy Clan Bot doesn't have this clan yet. You can add it below!";
  }
}

// Add clan
function addClan(newClan){
  clans.push(newClan);
  let saved=localStorage.getItem('userClans');
  let arr=saved?JSON.parse(saved):[];
  arr.push(newClan);
  localStorage.setItem('userClans',JSON.stringify(arr));
}

// Init
(async function init(){
  await loadData();

  // Search
  const input=document.getElementById('q'),btn=document.getElementById('btn');
  function search(){
    const q=(input.value||'').trim().toLowerCase();if(!q){render([]);return;}
    const items=clans.filter(d=>d.surname.toLowerCase().includes(q)||(d.primary_clan_name||'').toLowerCase().includes(q));
    render(items);
  }
  btn.addEventListener('click',search);
  input.addEventListener('keydown',e=>{if(e.key==='Enter')search();});

  // Chat
  const chatWindow=document.getElementById('chatWindow'),chatInput=document.getElementById('chatInput'),chatSend=document.getElementById('chatSend');
  function appendMessage(text,cls){const div=document.createElement('div');div.className='message '+cls;div.innerHTML=text;chatWindow.appendChild(div);chatWindow.scrollTop=chatWindow.scrollHeight;}
  chatSend.addEventListener('click',()=>{const msg=chatInput.value.trim();if(!msg)return;appendMessage(msg,'user');const reply=chatReply(msg);appendMessage(reply,'bot');chatInput.value='';});
  chatInput.addEventListener('keydown',e=>{if(e.key==='Enter'){chatSend.click();}});

  // Add Clan
  const addBtn=document.getElementById('addClanBtn'),addMsg=document.getElementById('addClanMessage');
  addBtn.addEventListener('click',()=>{
    const newClan={
      surname:document.getElementById('newSurname').value.trim(),
      language_group:document.getElementById('newLang').value.split(',').map(s=>s.trim()),
      clan_type:document.getElementById('newType').value.trim(),
      primary_clan_name:document.getElementById('newPrimary').value.trim(),
      praises:document.getElementById('newPraises').value.split(';').map(s=>s.trim()),
      origin_summary:document.getElementById('newOrigin').value.trim(),
      etymology_or_meaning:document.getElementById('newEtymology').value.trim(),
      regions:document.getElementById('newRegions').value.split(',').map(s=>s.trim()),
      notes:document.getElementById('newNotes').value.trim()
    };
    if(!newClan.surname||!newClan.primary_clan_name){addMsg.textContent='⚠️ Surname and Primary Clan Name are required.';return;}
    addClan(newClan);
    addMsg.textContent='✅ Clan added and saved locally. Please verify with elders.';
    ['newSurname','newLang','newType','newPrimary','newPraises','newOrigin','newEtymology','newRegions','newNotes'].forEach(id=>document.getElementById(id).value='');
  });
})();
</script>
</body>
</html>

[
  {
    "surname": "Dlamini",
    "language_group": ["Zulu", "Swati"],
    "clan_type": "Izithakazelo",
    "primary_clan_name": "Dlamini",
    "praises": ["Zulu kaBhekuzulu", "Mlangeni", "Sothole"],
    "origin_summary": "One of the largest clans in Southern Africa, prominent among the Swati and Zulu.",
    "etymology_or_meaning": "Derived from 'udlamini', meaning one who conquers or rules.",
    "regions": ["KwaZulu-Natal", "Eswatini", "Mpumalanga"],
    "notes": "Royal family of Eswatini."
  },
  {
    "surname": "Mpisi",
    "language_group": ["Xhosa"],
    "clan_type": "Iziduko",
    "primary_clan_name": "Mpisi",
    "praises": ["oGadlume", "Mafuya", "Mthimkhulu"],
    "origin_summary": "A Xhosa clan with roots in the Eastern Cape.",
    "etymology_or_meaning": "Means 'hyena' in Nguni languages.",
    "regions": ["Eastern Cape"],
    "notes": "Clan associated with resilience and adaptability."
  },
  {
    "surname": "Khumalo",
    "language_group": ["Zulu"],
    "clan_type": "Izithakazelo",
    "primary_clan_name": "Khumalo",
    "praises": ["Khulu kaMntwana", "Mntungwa", "Gatsheni"],
    "origin_summary": "One of the main Zulu clans, historically linked to King Mzilikazi.",
    "etymology_or_meaning": "Means 'descendants of Khumalo'.",
    "regions": ["KwaZulu-Natal", "Zimbabwe"],
    "notes": "Important in the formation of the Ndebele kingdom."
  },
  {
    "surname": "Ndlovu",
    "language_group": ["Zulu", "Xhosa"],
    "clan_type": "Izithakazelo/Iziduko",
    "primary_clan_name": "Ndlovu",
    "praises": ["Inkosi yeNdlovu", "Mntungwa", "Bhebhe kaKhokho"],
    "origin_summary": "Common clan name in both Zulu and Xhosa communities.",
    "etymology_or_meaning": "Means 'elephant'.",
    "regions": ["KwaZulu-Natal", "Eastern Cape"],
    "notes": "Symbolizes strength and leadership."
  }
]

