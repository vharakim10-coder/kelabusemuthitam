<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Private Journal</title>
<style>
  :root{
    --bg-dark:#07140d;
    --panel-dark: rgba(10,30,18,0.85);
    --accent: #84e39a;
    --text:#e8f6ef;
    --muted: #b7d8c1;
    --glass: rgba(255,255,255,0.03);
  }
  body{ margin:0; font-family: "Inter", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    background: linear-gradient(180deg,#06150d 0%, #071a0f 60%); color:var(--text); min-height:100vh; }
  /* animated leaves background (pure CSS) */
  .bg-anim {
    position:fixed; inset:0; z-index:0; overflow:hidden; pointer-events:none;
    background:
      radial-gradient(circle at 10% 20%, rgba(132,227,154,0.05), transparent 10%),
      radial-gradient(circle at 80% 80%, rgba(132,227,154,0.04), transparent 10%);
  }
  .leaf {
    position:absolute; width:220px; height:220px; background:linear-gradient(135deg, rgba(132,227,154,0.12), rgba(13,60,30,0.06));
    border-radius:50%;
    filter: blur(12px);
    transform: translateY(-10vh) rotate(10deg);
    animation: floaty 18s ease-in-out infinite;
    mix-blend-mode: screen;
  }
  .leaf.two{ left:60%; top:60%; width:300px; height:300px; opacity:0.8; animation-duration:24s; animation-delay:-6s; }
  .leaf.one{ left:5%; top:15%; opacity:0.9; animation-duration:20s; animation-delay:-2s; }
  .leaf.three{ left:78%; top:5%; width:180px; height:180px; animation-duration:22s; animation-delay:-4s; opacity:0.7;}
  @keyframes floaty { 0% { transform: translateY(-8vh) rotate(0deg); } 50% { transform: translateY(6vh) rotate(6deg); } 100% { transform: translateY(-8vh) rotate(0deg); } }

  /* layout */
  .wrap{ position:relative; z-index:2; max-width:960px; margin:32px auto; padding:18px; }
  .card{
    background:var(--panel-dark); border-radius:14px; padding:20px; box-shadow: 0 10px 30px rgba(0,0,0,0.5);
    backdrop-filter: blur(6px); border: 1px solid rgba(255,255,255,0.03);
  }

  .pw-area{ text-align:center; padding:30px 14px; }
  input[type="password"]{ padding:12px 14px; width:72%; max-width:360px; border-radius:8px; border:none; outline:none; font-size:16px; text-align:center; }
  button.btn{ padding:10px 16px; margin-left:8px; border-radius:10px; border:none; cursor:pointer; font-weight:600; background:linear-gradient(180deg,var(--accent), #49b16f); color:#06210d; }

  .meta-row{ display:flex; gap:8px; align-items:center; justify-content:space-between; margin-bottom:12px; flex-wrap:wrap; }
  .left { display:flex; gap:8px; align-items:center; }
  .small-btn{ background:transparent; border:1px solid rgba(255,255,255,0.06); color:var(--muted); padding:8px 10px; border-radius:8px; cursor:pointer; }

  .content { display:none; opacity:0; transition:opacity 0.9s ease, transform 0.9s ease; transform: translateY(6px); }
  .content.show{ display:block; opacity:1; transform: translateY(0); }

  h1{ margin:6px 0 10px 0; color:var(--accent); font-size:24px; text-align:center; }
  article{ line-height:1.7; color:var(--text); font-size:15.5px; margin-top:6px; white-space:pre-wrap; }

  .actions{ display:flex; gap:10px; flex-wrap:wrap; justify-content:center; margin-top:14px; }
  .download { text-decoration:none; color:var(--text); padding:10px 14px; border-radius:10px; border:1px solid rgba(255,255,255,0.04); display:inline-block; }
  .summary-box{ margin-top:18px; background: rgba(0,0,0,0.15); padding:12px; border-radius:10px; color:var(--muted); }

  /* light mode */
  body.light { background: linear-gradient(180deg,#f6fbf6 0%, #e6f6e9 60%); color:#07230f; }
  body.light .card{ background: rgba(255,255,255,0.92); color:#052418; border:1px solid rgba(6,34,20,0.06); }
  body.light .small-btn{ color:#0a3a22; border-color: rgba(10,58,34,0.06); }
  body.light .btn{ color:white; background: linear-gradient(180deg,#047a3b,#0b8f4a); }

  /* responsive */
  @media (max-width:520px){
    input[type="password"]{ width:84%; }
    .meta-row{ flex-direction:column; align-items:stretch; }
  }

</style>
</head>
<body>
<div class="bg-anim" aria-hidden="true">
  <div class="leaf one"></div>
  <div class="leaf two"></div>
  <div class="leaf three"></div>
</div>

<div class="wrap">
  <div class="card pw-area" id="gate">
    <h2>🔒 PRIVATE JOURNAL — Masukkan Kata Sandi</h2>
    <div style="margin-top:6px;color:var(--muted)">Ketik kata sandi untuk membuka: <strong>01-05-03</strong></div>
    <div style="margin-top:14px;">
      <input id="pw" type="password" inputmode="numeric" placeholder="•• •• ••" aria-label="Kata sandi">
      <button class="btn" onclick="unlock()">Masuk</button>
    </div>
    <div style="margin-top:10px">
      <button class="small-btn" onclick="toggleTheme()">🌗 Dark/Light</button>
      <button class="small-btn" onclick="previewAsApp()">➕ Add to Home Screen</button>
    </div>
  </div>

  <div class="card content" id="contentPanel" role="main" aria-live="polite">
    <div class="meta-row">
      <div class="left">
        <h1>Catatan Rahasia</h1>
        <div style="font-size:13px;color:var(--muted); margin-left:8px;">(Preview online)</div>
      </div>
      <div class="right">
        <div class="actions">
          <a id="downloadLink" class="download" href="ORANG BODOH TERTUTUP MENULIS SURAT YANG AKAN DILIHAT SEMESTA SETELAH MASUK KEDALAM TANAH 1.docx" download>
            📥 Unduh File Word Asli
          </a>
          <button class="small-btn" onclick="generateSummary()">✨ AI Autosummary</button>
          <button class="small-btn" onclick="copyToClipboard()">📋 Salin Ringkasan</button>
        </div>
      </div>
    </div>

    <article id="fullText">
Kisah yang banyak plot twist, drama, tragedi dan kisah seorang yang kuat dan punya banyak pemikiran dan Tangguh. Kisahku, seekor semut hitam. Lahir dari keluarga yang kurang sempurna yang pada saat lahirnya terjadi tragedy yang menjadikan keluarganya kurang sempurna. Saat berumur satu hari sudah ada tragedy besar. Seorang ayah yang meninggalkan bayinya yang masih berumur 1 hari, dan seorang ibu yang harus menanggung kesakitan kepedihan dan kepahitan karena ditinggal. Tumbuh besar yang dibantu dibesarkan nenek dan kakek, dan berpikir kalua dia adalah anak yatim, dan bahkan memiliki pertanyaan tentang dimana kuburan sang ayah berada. Masa kecil yang suram yang dipenuhi pembulian dan perundungan oleh temannya sendiri karena bertubuh kecil dan dianggap lemah. Dengan beberapa bekas luka yang menjadi saksi betapa pahit dan menyedihkan masa kecilnya. Yang menjadikannya merasa sendiri sampai sekarang, sosok pelindung? Tak ada, tak ada yang membuat dirinya merasa terlindungi. Hanya dia dan tuhan nya sendiri. Masa SMP tak luput juga dengan masa pembulian hanya saja masih beruntung tuhan memberikan akal dan kepintaran yang dimilikinya sejak kecil yang setidaknya oleh karena hal itu membuatnya bisa yakin kalua dunia ini sangat bisa dilewati bahkan untuk seekor semut sekalipun. Masa muda yang penuh dengan pengalaman dan beberapa masa muda yang membuatnya jatuh kedalam jurang kegelapan karena Kembali lagi, taak ada pelindung, taka da yang dipercayai selain dirinya sendiri. Masa muda masa remaja dimana dia mendapatkan Bahagia yang harusnya bukan Bahagia diantara beberapa ujian yang dialaminya diantara benturan tangisnya yang jatuh ketanah seakan berkamuflase menjadi hujan, diantara beberapa orang yang ada pada saat itu yang menjadi suatu bahagian dan juga racun yang tertutup. Seorang ibu yang harusnya melindungi seorang anak layaknya seekor semut yang kecil mencari tau dunia, seorang ibu yang seharusnya menjadi pelindung seekor semut itu ternyata tak tau arti pelindung dan meindungi. Seekor semut yang kecewa pun tak tau menyikapi bagaimana dan apa yang harus dilakukan untuk dunia yang penuh racun ini. Seekor semut yang kecil itu pun harus berusaha sendiri melindungi dirinya dari iblis jahat yang ada dirumah yang diundang oleh ibunya. Iblis itu jahat seperti sifatnya seorang iblis, rasanya kebaikan menolak hadir dalam hatinya. Iblis itu datang saat malam seekor semut hanya berlindung dengan perisai daunnya dikala malam tiba dan penuh harap menanyakan kepada semesta tentang kapankah semua ini usai? Semesta menjawab dengan mendatangkan masa muda yang gembira dan penuh petualangan. Seekor semut yang hanya tau jalan sarangnya kini juga tau bagaimana dunia diluar sana. Tapi masa muda itu juga membuat semut itu luoa akan dirinya lupa akan kepahitannya sehingga lupa untuk waspada dan menjebak semut kedalam lubang mangsa hitam yang buas. Sang semut mencoba bangkit dengan air kesedihan yang selalu turun dari pipinya dan selalu berharap kepada semesta, dan akhirnya bisa lepas. Semut kecil kini memegang tongkat masa kelam yang membuatnya kokoh terhadap badai masa kelam yang sama.  Kehidupan semut dihadapi beberapa ujian yang bahkan dirinya tak tau mengatasinya, seekor semut yang dituduh sebagai iblis , seekor semut yang dituduh dengan cerita layaknya Netflix, seekor semut yang ingin mengejar tuhannya tapi tak tau kalua diluar sana sedang terjadi amukan besar yang bahkan dirinya sendiri tak tahu kalua dia penyebabnya, seekor semut yang hanya semut dengan keterbatasan ukurannya yang kecil yang membuatnya tak tau bagaiamana luasnya alam semesta seekor semut yang hanya tau kalua tuhannya yang lebih luas dari lautan semesta. Seekor semut yang hanya ingin mencari keadilan tuhan. jika kepada yang maha adil dan maha mendengar aduannya tidak didengar maka kepada siapa lagi dia mengadu (suara hati itu selalu muncul Ketika semut sudah merasa lemah tapi yakin tuhannya akan menjawabnya).
    </article>

    <div class="summary-box" id="summaryBox" style="display:none;">
      <strong>Ringkasan AI:</strong>
      <div id="summaryText" style="margin-top:8px; color:var(--text)"></div>
    </div>
  </div>
</div>

<!-- Sentimental piano audio (user interaction required by browsers) -->
<audio id="bgAudio" loop preload="none">
  <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_3b9f3f9cda.mp3?filename=piano-ambient-11014.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

<script>
/* --- Password gate --- */
function unlock(){
  const pw = document.getElementById('pw').value.trim();
  if(pw === '01-05-03'){
    document.getElementById('gate').style.display='none';
    const c = document.getElementById('contentPanel');
    c.classList.add('show');
    window.scrollTo({top:0,behavior:'smooth'});
    // try to play audio after user interaction
    const a = document.getElementById('bgAudio');
    try { a.play().catch(()=>{}); } catch(e){}
  } else {
    alert('Kata sandi salah — coba lagi.');
  }
}

/* --- Theme toggle --- */
function toggleTheme(){
  document.body.classList.toggle('light');
}

/* --- Copy summary --- */
function copyToClipboard(){
  const s = document.getElementById('summaryText').innerText;
  if(!s){ alert('Belum ada ringkasan — klik "AI Autosummary" terlebih dahulu.'); return; }
  navigator.clipboard.writeText(s).then(()=> alert('Ringkasan disalin ke clipboard.') );
}

/* --- Simple client-side "AI" autosummary ---
   Approach: sentence tokenize, score using word frequency (TF-like), select top sentences.
   This runs entirely in the browser, no external API required.
*/
function generateSummary(){
  const text = document.getElementById('fullText').innerText;
  if(!text || text.length<40){ alert('Teks terlalu singkat untuk diringkas.'); return; }

  // Split into sentences (rough)
  let sentences = text.match(/[^\.!\?]+[\.!\?]+|[^\.!\?]+$/g) || [text];
  sentences = sentences.map(s=> s.trim()).filter(s=> s.length>20);

  // build word freq excluding stopwords
  const stop = new Set(['yang','dan','di','ke','dengan','atau','untuk','pada','sebuah','seorang','itu','ini','saat','ini','agar','karena','adalah','tidak','telah','dari','kepada','sebagai','seperti','tetapi','juga','lebih']);
  const freq = {};
  const words = text.toLowerCase().replace(/[^a-z0-9\u00C0-\u024F\s]+/gi,' ').split(/\s+/).filter(Boolean);
  for(const w of words){
    if(stop.has(w)) continue;
    if(w.length<=2) continue;
    freq[w] = (freq[w]||0)+1;
  }
  // normalize freq
  const maxf = Math.max(...Object.values(freq),1);
  Object.keys(freq).forEach(k=> freq[k] = freq[k]/maxf);

  // score sentences
  const scored = sentences.map(s=>{
    const ws = s.toLowerCase().replace(/[^a-z0-9\u00C0-\u024F\s]+/gi,' ').split(/\s+/).filter(Boolean);
    let score = 0;
    for(const w of ws){
      if(freq[w]) score += freq[w];
    }
    // small boost for shorter sentences that are meaningful
    score = score * (1 + Math.min(0.6, 30/ws.length));
    return {s,score};
  });

  // pick top 3 sentences by score and preserve original order
  scored.sort((a,b)=> b.score - a.score);
  const top = scored.slice(0,3).map(x=> x.s);
  // preserve order as in original text
  const ordered = sentences.filter(s=> top.includes(s)).slice(0,3);
  const summary = ordered.join(' ');
  document.getElementById('summaryText').innerText = summary || top.join(' ');
  document.getElementById('summaryBox').style.display = 'block';
}

/* --- "Preview as app" helper for Android users --- */
function previewAsApp(){
  alert('Untuk menambahkan ke home screen di Android: buka menu browser → Add to Home screen / Tambahkan ke Layar Utama.');
}
</script>
</body>
</html>
# kelabusemuthitam
private journal
