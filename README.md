[index.html](https://github.com/user-attachments/files/32430593/index.html)[U<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Which jobs have too few people, and which have too many? A 3D job market map</title>
<meta name="description" content="An interactive 3D map of 12 job sectors in 6 countries, 2019 to 2025: where openings outnumber job seekers, where people outnumber openings, and which jobs are most at risk. Simulated data.">
<meta property="og:title" content="Which jobs have too few people, and which have too many?">
<meta property="og:description" content="Interactive 3D map of job supply and demand across sectors and countries. Simulated data.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,500;12..96,700;12..96,800&family=Instrument+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  --ink:#0c1024;
  --ink-2:#121837;
  --line:#2a3260;
  --text:#e7ebfa;
  --muted:#9aa3cc;
  --teal:#35d6b5;
  --amber:#f2c14e;
  --coral:#ff6b57;
  --display:"Bricolage Grotesque","Instrument Sans",system-ui,-apple-system,"Segoe UI",sans-serif;
  --body:"Instrument Sans",system-ui,-apple-system,"Segoe UI",sans-serif;
}
*{box-sizing:border-box}
[hidden]{display:none!important}
html{color-scheme:dark;scroll-behavior:smooth}
@media (prefers-reduced-motion:reduce){html{scroll-behavior:auto}}
body{margin:0;background:var(--ink);color:var(--text);font:400 1rem/1.55 var(--body);-webkit-font-smoothing:antialiased}
a{color:inherit}
:focus-visible{outline:2px solid var(--teal);outline-offset:2px}
.wrap{width:min(1280px,100% - 40px);margin-inline:auto}
.skip{position:absolute;left:-999px;top:8px;background:var(--text);color:var(--ink);padding:8px 12px;border-radius:8px;z-index:10}
.skip:focus{left:8px}

.bar{display:flex;justify-content:space-between;align-items:center;padding:18px 0}
.brand{font:700 1.05rem var(--display);letter-spacing:-.01em}
.ghlink{font-size:.92rem;color:var(--muted);text-underline-offset:4px}
.ghlink:hover{color:var(--text)}

.hero{display:grid;grid-template-columns:minmax(0,1.5fr) minmax(0,1fr);gap:16px 48px;align-items:end;padding:4px 0 20px}
h1{margin:0;font:800 clamp(1.9rem,3vw,2.7rem)/1.06 var(--display);letter-spacing:-.025em;max-width:30ch;text-wrap:balance}
.hero p{margin:0;color:var(--muted);max-width:52ch;font-size:.98rem}

.stage{position:relative;height:clamp(560px,calc(100vh - 236px),800px);border-radius:22px;border:1px solid var(--line);overflow:hidden;
  background:radial-gradient(1100px 620px at 50% 36%,#1d2655 0%,#131a3c 52%,#0d1129 100%)}
#cv{position:absolute;inset:0;display:block;width:100%;height:100%;touch-action:none;cursor:grab}
#cv:active{cursor:grabbing}

.top{position:absolute;top:14px;left:14px;right:14px;display:flex;justify-content:space-between;align-items:flex-start;gap:10px;flex-wrap:wrap;pointer-events:none}
.top>*{pointer-events:auto}
.tabs{display:inline-flex;padding:3px;background:rgba(10,14,36,.66);border:1px solid var(--line);border-radius:999px;backdrop-filter:blur(8px)}
.tabs button{border:0;background:transparent;color:var(--muted);font:600 .86rem var(--body);padding:8px 16px;border-radius:999px;cursor:pointer}
.tabs button:hover{color:var(--text)}
.tabs button[aria-selected="true"]{background:rgba(53,214,181,.18);color:#c6f7ec}
.tools{display:flex;gap:8px;align-items:center;flex-wrap:wrap;justify-content:flex-end}
.sim{border:1px solid rgba(242,193,78,.55);background:rgba(242,193,78,.09);color:var(--amber);font-size:.78rem;font-weight:500;padding:6px 11px;border-radius:999px}
.ib{width:36px;height:36px;border-radius:50%;border:1px solid var(--line);background:rgba(10,14,36,.66);color:var(--text);display:grid;place-items:center;cursor:pointer;backdrop-filter:blur(8px)}
.ib:hover{border-color:#4a56a0}
.ib[aria-pressed="true"]{background:rgba(53,214,181,.18);border-color:rgba(53,214,181,.6)}
.ib svg{width:17px;height:17px;fill:none;stroke:currentColor;stroke-width:1.8;stroke-linecap:round;stroke-linejoin:round}

.legend{position:absolute;left:18px;bottom:88px;font-size:.78rem;color:var(--muted);pointer-events:none;max-width:260px}
.legend .grad{display:block;height:8px;width:160px;border-radius:4px;margin-bottom:5px}
.legend .ends{display:flex;justify-content:space-between;width:160px}
.legend p{margin:6px 0 0}

.dock{position:absolute;left:14px;right:14px;bottom:14px;display:flex;align-items:center;gap:16px;padding:10px 16px 10px 12px;background:rgba(10,14,36,.72);border:1px solid var(--line);border-radius:16px;backdrop-filter:blur(8px)}
.play{flex:none;width:44px;height:44px;border-radius:50%;border:0;background:var(--teal);color:#062a23;display:grid;place-items:center;cursor:pointer}
.play svg{width:18px;height:18px;fill:currentColor}
.play .pause{display:none}
.play.on .pause{display:block}.play.on .tri{display:none}
.year{flex:none;font:700 2rem/1 var(--display);letter-spacing:-.02em;min-width:4.2ch;font-variant-numeric:tabular-nums}
.scrub{flex:1;min-width:0}
.scrub input{width:100%;margin:0;height:22px;background:transparent;-webkit-appearance:none;appearance:none;cursor:pointer;
  --p:0%}
.scrub input::-webkit-slider-runnable-track{height:4px;border-radius:2px;background:linear-gradient(90deg,var(--teal) var(--p),#2c3568 var(--p))}
.scrub input::-moz-range-track{height:4px;border-radius:2px;background:linear-gradient(90deg,var(--teal) var(--p),#2c3568 var(--p))}
.scrub input::-webkit-slider-thumb{-webkit-appearance:none;width:18px;height:18px;border-radius:50%;background:var(--text);border:3px solid var(--teal);margin-top:-7px}
.scrub input::-moz-range-thumb{width:12px;height:12px;border-radius:50%;background:var(--text);border:3px solid var(--teal)}
.ticks{display:flex;justify-content:space-between;font-size:.72rem;color:var(--muted);padding:0 2px;font-variant-numeric:tabular-nums}

.tip{position:absolute;pointer-events:none;background:rgba(8,11,28,.94);border:1px solid #39428a;border-radius:10px;padding:8px 11px;font-size:.82rem;line-height:1.4;max-width:250px;z-index:3}
.tip b{display:block;font:700 .92rem var(--display);margin-bottom:2px}
.tip span{display:block;color:#c3cbee}

.detail{position:absolute;top:64px;right:14px;width:272px;background:rgba(14,19,46,.88);border:1px solid #39428a;border-radius:14px;padding:16px 16px 14px;backdrop-filter:blur(10px);z-index:2}
.detail h2{margin:0 26px 8px 0;font:700 1.15rem/1.15 var(--display);letter-spacing:-.01em}
.detail .x{position:absolute;top:8px;right:8px;width:28px;height:28px;border-radius:50%;border:0;background:transparent;color:var(--muted);cursor:pointer;font-size:1.1rem;line-height:1}
.detail .x:hover{color:var(--text)}
.mk{display:inline-block;font-size:.78rem;font-weight:600;padding:3px 10px;border-radius:999px;border:1px solid currentColor;margin-bottom:10px}
.detail dl{margin:0;display:grid;gap:7px}
.detail dl div{display:flex;justify-content:space-between;gap:12px;font-size:.86rem}
.detail dt{color:var(--muted)}
.detail dd{margin:0;font-weight:600;font-variant-numeric:tabular-nums;text-align:right}
.detail svg{display:block;width:100%;height:auto;margin-top:12px}
.detail .cap{margin:4px 0 0;font-size:.74rem;color:var(--muted)}

.note{margin:12px 2px 0;color:var(--muted);font-size:.88rem;max-width:90ch}
.note strong{color:var(--amber);font-weight:600}

section.block{padding:56px 0 0}
h2.h{margin:0 0 6px;font:700 clamp(1.4rem,2.4vw,1.9rem)/1.15 var(--display);letter-spacing:-.02em}
.lead{margin:0 0 22px;color:var(--muted);max-width:62ch}

.finds{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:22px}
.find{border-left:3px solid var(--c,var(--teal));padding:2px 0 2px 14px}
.find small{display:block;color:var(--muted);font-size:.84rem;margin-bottom:3px}
.find b{display:block;font:700 1.18rem/1.2 var(--display);letter-spacing:-.01em}
.find span{display:block;color:#c3cbee;font-size:.9rem;margin-top:3px}

.rank{list-style:none;margin:0;padding:0;border-top:1px solid var(--line)}
.rank li{border-bottom:1px solid var(--line)}
.rank button{display:grid;grid-template-columns:minmax(150px,240px) minmax(0,1fr) 48px;gap:8px 20px;align-items:center;width:100%;text-align:left;padding:13px 8px;background:transparent;border:0;color:inherit;font:inherit;cursor:pointer;border-radius:8px}
.rank button:hover,.rank button[aria-pressed="true"]{background:rgba(120,140,255,.08)}
.rank .nm{font-weight:600}
.rank .nm small{display:block;font-weight:400;color:var(--muted);font-size:.82rem}
.rank .track{height:10px;border-radius:5px;background:#1b2250;overflow:hidden}
.rank .fill{display:block;height:100%;border-radius:5px}
.rank .sc{font:700 1.15rem var(--display);text-align:right;font-variant-numeric:tabular-nums}

.method{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:0;border-top:1px solid var(--line)}
.method>div{padding:22px 28px 0 0}
.method>div+div{padding-left:28px;border-left:1px solid var(--line)}
.method h3{margin:0 0 6px;font:700 1.08rem var(--display)}
.method p{margin:0 0 10px;color:#c3cbee;font-size:.95rem;max-width:44ch}
.method code{font-size:.86em;background:#1a2150;border-radius:5px;padding:1px 6px}
.callout{margin:0 0 28px;padding:14px 18px;border:1px solid rgba(242,193,78,.5);background:rgba(242,193,78,.07);border-radius:12px;max-width:80ch}
.callout b{color:var(--amber)}

footer{padding:56px 0 48px;color:var(--muted);font-size:.9rem}
footer p{margin:0 0 4px}

@media (max-width:980px){
  .hero{grid-template-columns:1fr}
  .finds{grid-template-columns:repeat(2,minmax(0,1fr))}
  .method{grid-template-columns:1fr}
  .method>div,.method>div+div{padding:20px 0 0;border-left:0}
}
@media (max-width:640px){
  .stage{height:min(74vh,580px);min-height:500px}
  .ticks span:not(:first-child):not(:last-child){display:none}
  .detail{top:auto;bottom:88px;right:10px;left:10px;width:auto}
  .legend{display:none}
  .year{font-size:1.5rem}
  .dock{gap:10px;padding:8px 12px 8px 8px}
  .play{width:38px;height:38px}
  .finds{grid-template-columns:1fr}
  .rank button{grid-template-columns:minmax(0,1fr) 44px}
  .rank .track{grid-column:1 / -1;order:3}
  .tabs button{padding:7px 12px}
}
</style>
</head>
<body>
<a class="skip" href="#stage">Skip to the chart</a>

<header class="bar wrap">
  <span class="brand">Job market map</span>
  <a class="ghlink" id="gh" href="#" target="_blank" rel="noopener" hidden>View the code on GitHub</a>
</header>

<main>
  <section class="hero wrap">
    <h1>Which jobs have too few people, and which have too many?</h1>
    <p>A 3D map of 12 job sectors in 6 countries, from 2019 to 2025. Drag to turn it, press play to watch every sector move, and click a bubble to see its story.</p>
  </section>

  <section class="wrap">
    <div class="stage" id="stage">
      <canvas id="cv" tabindex="0" role="img" aria-label="Interactive 3D chart of job sectors. Use the arrow keys to rotate and the plus and minus keys to zoom. The ranking below the chart lists the same data as text."></canvas>
      <noscript><p style="padding:24px">This chart needs JavaScript. A static version is in <code>charts/5_big_picture.png</code>.</p></noscript>

      <div class="top">
        <div class="tabs" role="tablist" aria-label="Chart view">
          <button role="tab" id="tab-map" aria-selected="true">Sector map</button>
          <button role="tab" id="tab-towers" aria-selected="false">Country towers</button>
        </div>
        <div class="tools">
          <span class="sim">Simulated data</span>
          <button class="ib" id="bAuto" aria-pressed="true" aria-label="Auto-rotate" title="Auto-rotate">
            <svg viewBox="0 0 24 24"><path d="M20 12a8 8 0 1 1-2.6-5.9"/><path d="M20 4v5h-5"/></svg>
          </button>
          <button class="ib" id="bIn" aria-label="Zoom in" title="Zoom in"><svg viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></svg></button>
          <button class="ib" id="bOut" aria-label="Zoom out" title="Zoom out"><svg viewBox="0 0 24 24"><path d="M5 12h14"/></svg></button>
          <button class="ib" id="bReset" aria-label="Reset view" title="Reset view"><svg viewBox="0 0 24 24"><path d="M4 12a8 8 0 1 0 2.6-5.9"/><path d="M4 4v5h5"/></svg></button>
        </div>
      </div>

      <aside class="detail" id="detail" hidden aria-live="polite">
        <button class="x" id="dClose" aria-label="Close details">&times;</button>
        <h2 id="dName"></h2>
        <span class="mk" id="dMarket"></span>
        <dl>
          <div><dt>Openings per job seeker</dt><dd id="dRatio"></dd></div>
          <div><dt id="dChangeL">Openings vs 2019</dt><dd id="dChange"></dd></div>
          <div><dt>Automation risk</dt><dd id="dAuto"></dd></div>
          <div><dt id="dRiskL">Danger score</dt><dd id="dRisk"></dd></div>
        </dl>
        <svg id="dSpark" viewBox="0 0 240 58" role="img" aria-label="Openings per job seeker over time"></svg>
        <p class="cap">Openings per job seeker over time. Dashed line: 1.0.</p>
      </aside>

      <div class="tip" id="tip" hidden></div>
      <div class="legend" id="legend"></div>

      <div class="dock">
        <button class="play" id="play" aria-label="Play">
          <svg class="tri" viewBox="0 0 24 24"><path d="M7 4.5v15l13-7.5z"/></svg>
          <svg class="pause" viewBox="0 0 24 24"><path d="M6 4h4.5v16H6zM13.5 4H18v16h-4.5z"/></svg>
        </button>
        <div class="year" id="year" aria-live="off"></div>
        <div class="scrub">
          <input type="range" id="slider" min="0" max="1" step="0.01" value="0" aria-label="Year">
          <div class="ticks" id="ticks" aria-hidden="true"></div>
        </div>
      </div>
    </div>
    <p class="note"><strong>Simulated data.</strong> Every number on this page was generated by <code>make_sample_data.py</code> to test the method. None of it is a real statistic. The code is built so real data can replace it without changes to the analysis.</p>
  </section>

  <section class="block wrap" aria-labelledby="h-find">
    <h2 class="h" id="h-find">What the map shows</h2>
    <p class="lead">Four things stand out in the simulated 2025 numbers.</p>
    <div class="finds" id="finds"></div>
  </section>

  <section class="block wrap" aria-labelledby="h-rank">
    <h2 class="h" id="h-rank">Which jobs are most in danger</h2>
    <p class="lead">Sectors ranked by danger score, from 0 (safe) to 100 (most at risk). Select a row to find it on the map.</p>
    <ol class="rank" id="rank"></ol>
  </section>

  <section class="block wrap" aria-labelledby="h-how">
    <h2 class="h" id="h-how">How it is calculated</h2>
    <p class="lead">Two ideas do all the work.</p>
    <p class="callout"><b>About the data.</b> This version runs on simulated numbers. Sectors were given realistic shapes (for example, more cybersecurity openings than qualified people), so the story you see was designed in. Swap in a real dataset to learn something new.</p>
    <div class="method">
      <div>
        <h3>Openings per job seeker</h3>
        <p>Vacancies divided by the number of people looking for work in that sector. Above 1, employers struggle to find people. Below 1, people struggle to find jobs.</p>
        <p>Tall towers and high bubbles are sectors where a person has plenty of choice.</p>
      </div>
      <div>
        <h3>Danger score</h3>
        <p>A 0 to 100 blend of three signals: too many people for the openings (<span id="w1"></span>), openings shrinking each year (<span id="w2"></span>), and how easily automation could do the work (<span id="w3"></span>).</p>
        <p>The weights are judgment calls, set at the top of <code>analyze_jobs.py</code>. Change them and the ranking moves.</p>
      </div>
      <div>
        <h3>Using real data</h3>
        <p>Replace <code>jobs_data.csv</code> with a file that has the same six columns, then run <code>analyze_jobs.py</code> and <code>make_3d_dashboard.py</code>. Every country needs a row for every sector and every year.</p>
        <p>Automation risk is the hardest column to source. Published studies of job exposure to AI are a good starting point.</p>
      </div>
    </div>
  </section>
</main>

<footer class="wrap">
  <p>Built with Python (pandas, matplotlib) and plain JavaScript. The 3D view is drawn on a canvas, with no chart libraries.</p>
  <p id="by"></p>
</footer>

<script type="application/json" id="data">{"meta":{"author":"Yovan","github":"","shortageAbove":1.2,"oversupplyBelow":0.8,"weights":{"oversupply":0.4,"decline":0.3,"automation":0.3}},"years":[2019,2020,2021,2022,2023,2024,2025],"sectors":[{"name":"AI & Data Science","automation":0.15,"risk":7.38,"riskLevel":"Low","ratioNow":1.8058,"growthCagr":12.227,"openings":[106617,121561,136736,152669,168023,188323,213014],"seekers":[81896,84185,91144,95977,101027,109575,117960],"ratio":[1.3019,1.444,1.5002,1.5907,1.6631,1.7187,1.8058],"change":[0.0,14.0,28.2,43.2,57.6,76.6,99.8]},{"name":"Bank Tellers","automation":0.8,"risk":91.19,"riskLevel":"High","ratioNow":0.3839,"growthCagr":-7.213,"openings":[238861,221449,207098,192530,179903,166903,152431],"seekers":[415490,421200,417029,407027,400618,404889,397045],"ratio":[0.5749,0.5258,0.4966,0.473,0.4491,0.4122,0.3839],"change":[0.0,-7.3,-13.3,-19.4,-24.7,-30.1,-36.2]},{"name":"Call Center Support","automation":0.8,"risk":87.86,"riskLevel":"High","ratioNow":0.3821,"growthCagr":-5.017,"openings":[474196,457339,434104,398073,376349,362243,348201],"seekers":[839138,843917,869915,894269,874540,875527,911380],"ratio":[0.5651,0.5419,0.499,0.4451,0.4303,0.4137,0.3821],"change":[0.0,-3.6,-8.5,-16.1,-20.6,-23.6,-26.6]},{"name":"Cybersecurity","automation":0.15,"risk":10.39,"riskLevel":"Low","ratioNow":1.9245,"growthCagr":8.39,"openings":[94379,107165,113254,123745,131788,145031,153046],"seekers":[62676,65210,69564,70232,73299,77845,79524],"ratio":[1.5058,1.6434,1.6281,1.7619,1.798,1.8631,1.9245],"change":[0.0,13.5,20.0,31.1,39.6,53.7,62.2]},{"name":"Data Entry & Clerical","automation":0.9,"risk":97.0,"riskLevel":"High","ratioNow":0.2741,"growthCagr":-7.311,"openings":[530145,490952,459834,427880,393952,357628,336166],"seekers":[1149996,1146432,1165273,1173421,1187024,1207544,1226347],"ratio":[0.461,0.4282,0.3946,0.3646,0.3319,0.2962,0.2741],"change":[0.0,-7.4,-13.3,-19.3,-25.7,-32.5,-36.6]},{"name":"Healthcare & Nursing","automation":0.1,"risk":21.74,"riskLevel":"Low","ratioNow":1.6711,"growthCagr":4.02,"openings":[743435,754005,789810,821691,876661,894117,941766],"seekers":[504670,519151,521103,523503,557726,568443,563562],"ratio":[1.4731,1.4524,1.5157,1.5696,1.5718,1.5729,1.6711],"change":[0.0,1.4,6.2,10.5,17.9,20.3,26.7]},{"name":"Manufacturing & Assembly","automation":0.7,"risk":71.49,"riskLevel":"High","ratioNow":0.7508,"growthCagr":-2.13,"openings":[852517,823790,790631,776666,778646,756367,749221],"seekers":[990661,974376,972190,1009323,980408,969876,997845],"ratio":[0.8606,0.8455,0.8132,0.7695,0.7942,0.7799,0.7508],"change":[0.0,-3.4,-7.3,-8.9,-8.7,-11.3,-12.1]},{"name":"Media & Content Writing","automation":0.6,"risk":76.13,"riskLevel":"High","ratioNow":0.4646,"growthCagr":-2.587,"openings":[177156,174909,172062,164013,157832,154040,151377],"seekers":[273909,280788,287558,296558,299530,312289,325798],"ratio":[0.6468,0.6229,0.5984,0.5531,0.5269,0.4933,0.4646],"change":[0.0,-1.3,-2.9,-7.4,-10.9,-13.0,-14.6]},{"name":"Renewable Energy","automation":0.2,"risk":20.45,"riskLevel":"Low","ratioNow":1.5161,"growthCagr":9.265,"openings":[142277,156355,171066,185202,211046,216556,242119],"seekers":[117046,122012,127680,136550,142895,148363,159700],"ratio":[1.2156,1.2815,1.3398,1.3563,1.4769,1.4596,1.5161],"change":[0.0,9.9,20.2,30.2,48.3,52.2,70.2]},{"name":"Retail Sales","automation":0.55,"risk":67.39,"riskLevel":"Medium","ratioNow":0.6594,"growthCagr":-0.945,"openings":[951931,949908,928917,939887,928900,944494,899239],"seekers":[1271504,1257521,1304251,1295490,1305074,1352232,1363669],"ratio":[0.7487,0.7554,0.7122,0.7255,0.7118,0.6985,0.6594],"change":[0.0,-0.2,-2.4,-1.3,-2.4,-0.8,-5.5]},{"name":"Skilled Trades","automation":0.15,"risk":24.66,"riskLevel":"Low","ratioNow":1.6635,"growthCagr":3.218,"openings":[419324,437102,456296,461530,466414,492404,507094],"seekers":[315075,316376,313536,321313,313644,322831,304835],"ratio":[1.3309,1.3816,1.4553,1.4364,1.4871,1.5253,1.6635],"change":[0.0,4.2,8.8,10.1,11.2,17.4,20.9]},{"name":"Software Development","automation":0.45,"risk":61.38,"riskLevel":"Medium","ratioNow":0.7451,"growthCagr":-0.343,"openings":[487727,478679,476953,469073,475748,492771,477771],"seekers":[498334,520507,548222,561004,579503,607469,641184],"ratio":[0.9787,0.9196,0.87,0.8361,0.821,0.8112,0.7451],"change":[0.0,-1.9,-2.2,-3.8,-2.5,1.0,-2.0]}],"countries":["Brazil","Germany","India","Japan","UK","USA"],"grid":{"openings":[[[13603,15942,16134,18847,21938,24433,26827],[29281,27236,27239,24154,23065,20343,19790],[58501,55128,52692,51204,44877,43079,40695],[11384,13286,13998,15405,16127,17359,18381],[68682,64469,60911,57575,54528,48889,47781],[86350,96629,105186,102024,107817,110305,114054],[109975,103109,99666,100176,102616,106913,104603],[22955,22064,21461,22014,19561,18951,19351],[17981,19126,21558,23296,25220,26228,28973],[118416,119274,111946,118867,118060,117825,110488],[50396,55160,57701,58459,56533,61771,60913],[59928,60683,62995,62094,61752,63648,66299]],[[10642,12194,13564,15398,17348,17543,19680],[24834,22644,20405,18364,17578,16890,14528],[46400,46002,42616,38274,34921,33550,32175],[9274,10900,11099,12300,13225,14591,16076],[51925,46883,46602,42184,39669,36904,33687],[75621,75116,74140,85321,90582,90827,103168],[77203,81522,84675,77769,82535,74909,73611],[17704,17073,16789,16866,16071,14072,14308],[14201,16123,17223,19045,21151,22228,24155],[100405,92218,86387,88647,82726,87947,77823],[40838,45350,47760,45162,50861,50420,50633],[48157,48755,46982,50906,50554,46876,48454]],[[26179,29268,35003,37440,40958,44798,50776],[60623,56340,51055,49344,46262,43305,38479],[117180,117458,110305,99633,100803,92026,92749],[23900,27228,28053,31357,33797,35578,36901],[138329,123838,112215,106297,99277,91025,83418],[190002,181954,188996,192769,215081,224370,230936],[221882,208245,191645,200466,195775,193567,197213],[43772,42155,45861,41368,40532,40327,39914],[35502,39382,42618,44775,51018,52236,58599],[246974,235495,225337,235386,229819,226464,221817],[104415,103783,114850,116548,113749,124405,127638],[120313,121319,121176,128435,120986,128446,124037]],[[11913,12960,15224,16626,19260,20527,24542],[26308,24205,21738,21215,18720,15993,15311],[50087,50033,49898,46865,43162,42278,41387],[10277,11702,12230,13531,15274,15645,17641],[56227,52419,48248,44011,41098,34572,34443],[82401,76093,83806,90758,91249,99483,100953],[93443,92357,91581,92081,89733,82281,81779],[19164,20239,18788,18151,17097,16340,17101],[14886,16814,18040,18876,21116,23082,25080],[97629,101065,101496,103501,102476,102720,97232],[47314,48160,50986,53392,50247,53301,55291],[51294,48593,51233,47683,48174,49401,46068]],[[9235,10668,10854,11597,13463,15482,17532],[19969,18146,17793,15751,15477,14347,13392],[38755,36822,36743,32379,31328,29670,28599],[8367,8786,9307,10227,10837,12219,13441],[42802,40904,37309,32953,29198,26167,24959],[60885,65892,64107,70439,73082,72037,76472],[71289,67356,66372,62464,67143,63646,66825],[14570,14619,13475,13193,13558,13364,12075],[11615,13202,14728,15080,17502,19881,21561],[82292,80346,76446,75070,73898,75918,75514],[36843,37843,40218,41422,40391,42058,48079],[41954,37984,40520,37602,38548,40589,39558]],[[35045,40529,45957,52761,55056,65540,73657],[77846,72878,68868,63702,58801,56025,50931],[163273,151896,141850,129718,121258,121640,112596],[31177,35263,38567,40925,42528,49639,50606],[172180,162439,154549,144860,130182,120071,111878],[248176,258321,273575,280380,298850,297095,316183],[278725,271201,256692,243710,240844,235051,225190],[58991,58759,55688,52421,51013,50986,48628],[48092,51708,56899,64130,75039,72901,83751],[306215,321510,327305,318416,321921,333620,316365],[139518,146806,144781,146547,154633,160449,164540],[166081,161345,154047,142353,155734,163811,153355]]],"seekers":[[[11416,12112,11981,13217,13695,14503,15066],[55120,56868,55480,55764,59421,53879,55242],[116626,118355,111977,124868,118277,122078,129436],[8276,9015,9333,9544,10149,10622,11250],[153576,162017,160780,165565,164697,163887,160762],[66979,68375,72850,71337,73398,82032,80384],[132062,133032,130469,131076,135109,130148,133305],[35719,38263,39555,39619,40825,42032,46018],[16768,17628,17258,18970,18442,20171,21735],[179988,179618,177377,178165,176718,193802,180664],[43616,43849,44203,43034,43680,43476,41726],[70410,71824,73475,76539,79971,82743,84963]],[[6472,6862,7826,7733,8176,8509,9203],[32897,34258,33649,33904,32148,31650,30515],[71574,71299,67017,71089,72274,70665,73145],[5181,4910,5611,5800,5981,6422,6518],[96632,97436,93373,95714,98689,96917,97281],[41200,42651,42748,41376,44611,45901,46198],[79204,80990,82014,76699,79474,79275,78989],[21835,22031,23010,21984,25520,25077,24996],[9175,9632,10231,11008,11269,11663,12760],[101516,99891,106619,101023,105072,106368,107476],[26165,25816,24908,24139,26000,25714,27371],[41688,42716,44753,47746,46998,49452,53319]],[[26281,26428,29055,29683,32978,35234,36608],[132245,134825,139719,126308,125888,132845,130773],[267391,262630,286227,279722,287552,272531,289164],[20643,20912,22940,22457,23360,24816,24296],[385241,359820,362726,379388,388712,399798,418499],[161934,169317,169600,165065,179238,178240,174346],[316649,314296,314749,338277,317849,303955,322105],[90437,90963,89334,95116,93134,97402,100768],[38570,37177,40740,44071,46710,47270,51565],[396470,404238,416810,417335,403549,421282,427835],[97889,103718,101031,102348,102347,101141,93292],[160546,158434,177096,179394,186664,199041,208137]],[[6723,7128,7280,7795,8054,9048,9465],[35180,33753,33950,34185,34071,34103,32283],[66287,72226,69753,71351,74061,69423,76645],[5204,5439,5394,5962,6027,6373,6560],[96859,95518,96847,92724,95957,97111,101892],[39792,42992,41954,44469,43299,46825,49443],[82430,79590,81368,79216,81369,79714,81918],[22269,24143,23669,24800,24889,26144,26224],[9400,10153,10695,11594,12059,12420,13037],[103228,101883,111811,105315,112766,109500,112710],[26153,25930,25472,25968,26099,26655,25651],[38574,44250,45568,46727,46974,51944,52679]],[[6463,6649,7093,7535,8152,9087,8961],[33094,32428,33160,32439,30416,31069,30664],[66285,64540,71902,69846,68359,68251,69003],[4983,5307,5444,5640,5853,6348,6735],[90129,88433,87232,89358,93787,93499,93883],[38415,42243,39477,42302,41226,44170,44438],[79545,76434,72406,80967,77196,78504,80390],[20395,21570,22292,24600,24112,24899,26755],[9191,9841,9833,11095,10968,11821,12395],[97042,99393,101682,101318,110193,102894,105794],[24813,25484,24276,25793,24112,24797,26126],[38343,41530,43221,44874,47734,47293,49004]],[[24541,25006,27909,30014,29972,33194,38657],[126954,129068,121071,124427,118674,121343,117568],[250975,254867,263039,277393,254017,272579,273987],[18389,19627,20842,20829,21929,23264,24165],[327559,343208,364315,350672,345182,356332,354030],[156350,153573,154474,158954,175954,171275,168753],[300771,290034,291184,303088,289411,298280,301138],[83254,83818,89698,90439,91050,96735,101037],[33942,37581,38923,39812,43447,45018,48208],[393260,372498,389952,392334,396776,418386,429190],[96439,91579,93646,100031,91406,101048,90669],[148773,161753,164109,165724,171162,176996,193082]]]}}</script>
<script>
(() => {
'use strict';
const DATA = JSON.parse(document.getElementById('data').textContent);
const META = DATA.meta, YEARS = DATA.years, NY = YEARS.length, LAST = NY - 1;
const REDUCE = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
const $ = id => document.getElementById(id);

/* ---------- helpers ---------- */
const clamp = (v, a, b) => Math.min(b, Math.max(a, v));
const lerp = (a, b, t) => a + (b - a) * t;
const mix = (a, b, t) => [lerp(a[0], b[0], t), lerp(a[1], b[1], t), lerp(a[2], b[2], t)];
const rgb = (c, a = 1) => `rgba(${c[0] | 0},${c[1] | 0},${c[2] | 0},${a})`;
const shade = (c, f) => [clamp(c[0] * f, 0, 255), clamp(c[1] * f, 0, 255), clamp(c[2] * f, 0, 255)];
const C = { teal: [53, 214, 181], amber: [242, 193, 78], coral: [255, 107, 87], red: [232, 80, 86], green: [40, 214, 170] };
const riskColor = s => s < 50 ? mix(C.teal, C.amber, s / 50) : mix(C.amber, C.coral, (s - 50) / 50);
const divColor = r => r < 1 ? mix(C.red, C.amber, clamp((r - 0.2) / 0.8, 0, 1)) : mix(C.amber, C.green, clamp((r - 1) / 1.3, 0, 1));
const sample = (arr, t) => { t = clamp(t, 0, LAST); const i = Math.min(NY - 2, Math.floor(t)); return lerp(arr[i], arr[i + 1], t - i); };
const fmtPct = v => (v > 0.5 ? '+' : v < -0.5 ? '\u2212' : '') + Math.abs(v).toFixed(0) + '%';
const fmtInt = v => Math.round(v).toLocaleString('en-US');
const marketOf = r => r >= META.shortageAbove ? 'Too few people for the jobs' : r <= META.oversupplyBelow ? 'Too many people for the jobs' : 'Roughly balanced';
const marketCol = r => r >= META.shortageAbove ? C.teal : r <= META.oversupplyBelow ? C.coral : C.amber;
const FONT = '"Instrument Sans", system-ui, -apple-system, "Segoe UI", sans-serif';

/* ---------- data ---------- */
const sectors = DATA.sectors.map((s, i) => Object.assign({}, s, { idx: i, color: riskColor(s.risk) }));
const countries = DATA.countries, NC = countries.length, NS = sectors.length;
const maxOpen = Math.max(...sectors.map(s => Math.max(...s.openings)));
const order = sectors.map(s => s.idx).sort((a, b) => sectors[b].ratioNow - sectors[a].ratioNow);
const gridO = DATA.grid.openings, gridK = DATA.grid.seekers;

/* ---------- scene 1 domains (sector map) ---------- */
const MAPE = { hx: 1.25, hy: 0.95, hz: 1.0 };
const allChange = sectors.flatMap(s => s.change), allRatio = sectors.flatMap(s => s.ratio);
let xmin = Math.min(0, ...allChange), xmax = Math.max(...allChange);
const padX = (xmax - xmin) * 0.08;
const stepX = (xmax - xmin) > 200 ? 50 : 25;
xmin = Math.floor((xmin - padX) / stepX) * stepX; xmax = Math.ceil((xmax + padX) / stepX) * stepX;
const ymax = Math.max(1.5, Math.ceil(Math.max(...allRatio) * 1.1 / 0.5) * 0.5);
const mapX = v => -MAPE.hx + 2 * MAPE.hx * (v - xmin) / (xmax - xmin);
const mapY = v => -MAPE.hy + 2 * MAPE.hy * v / ymax;
const mapZ = v => -MAPE.hz + 2 * MAPE.hz * v;
const range = (a, b, s) => { const o = []; for (let v = a; v <= b + 1e-9; v += s) o.push(+v.toFixed(4)); return o; };
const MAPCFG = {
  xTicks: range(xmin, xmax, stepX).map(v => ({ w: mapX(v), text: (v > 0 ? '+' : v < 0 ? '\u2212' : '') + Math.abs(v) + '%' })),
  zTicks: [0, 0.5, 1].map(v => ({ w: mapZ(v), text: Math.round(v * 100) + '%' })),
  yTicks: range(0, ymax, 0.5).map(v => ({ w: mapY(v), text: v.toFixed(1) })),
  wallVerticals: true,
  titles: { x: 'Change in openings since ' + YEARS[0], y: 'Openings per job seeker', z: 'Automation risk' },
  shortTitles: { x: 'Openings vs ' + YEARS[0], y: 'Openings per seeker', z: 'Automation' }
};

/* ---------- scene 2 domains (country towers) ---------- */
const TOWE = { hx: 1.75, hy: 0.8, hz: 0.95 };
let gmax = 0;
for (let j = 0; j < NC; j++) for (let s = 0; s < NS; s++) for (let y = 0; y < NY; y++) gmax = Math.max(gmax, gridO[j][s][y] / gridK[j][s][y]);
const RMAX = Math.max(2, Math.ceil(gmax * 1.05));
const towY = v => -TOWE.hy + 2 * TOWE.hy * clamp(v / RMAX, 0, 1);
const TOWCFG = {
  xTicks: [], zTicks: [],
  yTicks: range(0, RMAX, 1).map(v => ({ w: towY(v), text: v.toFixed(1) })),
  wallVerticals: false,
  titles: { y: 'Openings per job seeker' },
  shortTitles: { y: 'Openings per seeker' }
};

/* ---------- state ---------- */
const st = { view: 'map', t: 0, playing: false, auto: !REDUCE, dragging: false, sel: null, hover: null, px: 0, py: 0, inside: false };
const camDefault = { map: { yaw: 0.45, pitch: 0.3, zoom: 1 }, towers: { yaw: -0.46, pitch: 0.55, zoom: 1 } };
const cams = JSON.parse(JSON.stringify(camDefault));

/* ---------- canvas ---------- */
const stage = $('stage'), cv = $('cv'), ctx = cv.getContext('2d');
let W = 0, H = 0, DPR = 1;
function resize() {
  const r = stage.getBoundingClientRect();
  W = r.width; H = r.height; DPR = Math.min(2, window.devicePixelRatio || 1);
  cv.width = Math.round(W * DPR); cv.height = Math.round(H * DPR);
}
new ResizeObserver(resize).observe(stage); resize();

function projector(cam, fit, shift = 0) {
  const cy = Math.cos(cam.yaw), sy = Math.sin(cam.yaw), cp = Math.cos(cam.pitch), sp = Math.sin(cam.pitch);
  const D = 4.8, ox = W * 0.5, oy = H * (W < 640 ? 0.41 : 0.405) + shift;
  const S = Math.min(W * (W < 640 ? 0.235 : 0.26), H * 0.245) * fit * cam.zoom;
  const view = (x, y, z) => { const x1 = x * cy - z * sy, z1 = x * sy + z * cy; return [x1, y * cp + z1 * sp, -y * sp + z1 * cp]; };
  const scr = v => { const k = D / (D + v[2]); return { x: ox + v[0] * k * S, y: oy - v[1] * k * S, d: v[2], k }; };
  const P = (x, y, z) => scr(view(x, y, z));
  P.view = view; P.scr = scr; P.S = S; P.D = D;
  return P;
}

/* ---------- drawing primitives ---------- */
function path(pts) { ctx.beginPath(); pts.forEach((p, i) => i ? ctx.lineTo(p.x, p.y) : ctx.moveTo(p.x, p.y)); }
function seg(a, b) { ctx.beginPath(); ctx.moveTo(a.x, a.y); ctx.lineTo(b.x, b.y); ctx.stroke(); }
function quad(P, pts, fill) { path(pts.map(p => P(p[0], p[1], p[2]))); ctx.closePath(); ctx.fillStyle = fill; ctx.fill(); }
function outward(P, p3, off) {
  const q = P(p3[0], p3[1], p3[2]), c = P(0, 0, 0);
  let dx = q.x - c.x, dy = q.y - c.y; const l = Math.hypot(dx, dy) || 1;
  return { x: q.x + dx / l * off, y: q.y + dy / l * off, dx: dx / l, dy: dy / l };
}

function drawFrame(P, E, cfg) {
  const { hx, hy, hz } = E;
  const wx = P(hx, 0, 0).d > P(-hx, 0, 0).d ? hx : -hx;
  const wz = P(0, 0, hz).d > P(0, 0, -hz).d ? hz : -hz;
  quad(P, [[-hx, -hy, -hz], [hx, -hy, -hz], [hx, -hy, hz], [-hx, -hy, hz]], 'rgba(70,88,190,0.17)');
  quad(P, [[wx, -hy, -hz], [wx, -hy, hz], [wx, hy, hz], [wx, hy, -hz]], 'rgba(90,110,210,0.06)');
  quad(P, [[-hx, -hy, wz], [hx, -hy, wz], [hx, hy, wz], [-hx, hy, wz]], 'rgba(90,110,210,0.06)');

  ctx.lineWidth = 1; ctx.strokeStyle = 'rgba(150,165,235,0.15)';
  cfg.yTicks.forEach(t => { seg(P(wx, t.w, -hz), P(wx, t.w, hz)); seg(P(-hx, t.w, wz), P(hx, t.w, wz)); });
  cfg.zTicks.forEach(t => { seg(P(-hx, -hy, t.w), P(hx, -hy, t.w)); if (cfg.wallVerticals) seg(P(wx, -hy, t.w), P(wx, hy, t.w)); });
  cfg.xTicks.forEach(t => { seg(P(t.w, -hy, -hz), P(t.w, -hy, hz)); if (cfg.wallVerticals) seg(P(t.w, -hy, wz), P(t.w, hy, wz)); });

  ctx.strokeStyle = 'rgba(170,185,250,0.34)';
  const corners = [[-hx, -hz], [hx, -hz], [hx, hz], [-hx, hz]];
  corners.forEach((c, i) => { const n = corners[(i + 1) % 4]; seg(P(c[0], -hy, c[1]), P(n[0], -hy, n[1])); });
  ctx.strokeStyle = 'rgba(170,185,250,0.16)';
  corners.forEach((c, i) => { const n = corners[(i + 1) % 4]; seg(P(c[0], hy, c[1]), P(n[0], hy, n[1])); seg(P(c[0], -hy, c[1]), P(c[0], hy, c[1])); });

  // axis labels on the edges nearest the viewer
  const zEdge = P(0, -hy, hz).d < P(0, -hy, -hz).d ? hz : -hz;
  const xEdge = P(hx, -hy, 0).d < P(-hx, -hy, 0).d ? hx : -hx;
  const yCorner = corners.reduce((a, b) => P(b[0], -hy, b[1]).x < P(a[0], -hy, a[1]).x ? b : a);
  ctx.font = `500 11.5px ${FONT}`; ctx.fillStyle = 'rgba(178,189,236,0.9)'; ctx.textBaseline = 'middle'; ctx.textAlign = 'center';
  cfg.xTicks.forEach(t => { if (!t.text) return; const o = outward(P, [t.w, -hy, zEdge], 13); if (o.y > H - 92) return; ctx.fillText(t.text, o.x, o.y); });
  cfg.zTicks.forEach(t => { if (!t.text || Math.abs(t.w - zEdge) < 1e-6) return; const o = outward(P, [xEdge, -hy, t.w], 15); if (o.y > H - 92) return; ctx.fillText(t.text, o.x, o.y); });
  cfg.yTicks.forEach(t => { if (t.w <= -hy + 1e-6) return; const o = outward(P, [yCorner[0], t.w, yCorner[1]], 15); ctx.fillText(t.text, o.x, o.y); });
  ctx.font = `600 ${W < 640 ? 11.5 : 12.5}px ${FONT}`; ctx.fillStyle = 'rgba(214,221,250,0.95)';
  const T = W < 640 ? cfg.shortTitles : cfg.titles;
  const title = (text, o) => {
    const w = ctx.measureText(text).width;
    ctx.fillText(text, clamp(o.x, w / 2 + 8, W - w / 2 - 8), clamp(o.y, 70, H - 96));
  };
  if (T.x) title(T.x, outward(P, [0, -hy, zEdge], 46));
  if (T.z) title(T.z, outward(P, [xEdge, -hy, 0], 52));
  if (T.y) { const q = P(yCorner[0], hy, yCorner[1]); title(T.y, { x: q.x, y: q.y - 32 }); }
}

function drawPlane(P, E, y) {
  const { hx, hz } = E;
  const cs = [[-hx, -hz], [hx, -hz], [hx, hz], [-hx, hz]].map(c => ({ c, p: P(c[0], y, c[1]) }));
  path(cs.map(o => o.p)); ctx.closePath();
  ctx.fillStyle = 'rgba(255,255,255,0.055)'; ctx.fill();
  ctx.setLineDash([6, 5]); ctx.strokeStyle = 'rgba(255,255,255,0.5)'; ctx.lineWidth = 1.2; ctx.stroke(); ctx.setLineDash([]);
  const r = cs.reduce((a, b) => b.p.x > a.p.x ? b : a).p;
  const text = W < 640 ? 'Balanced (1.0)' : 'Balanced: 1 opening per job seeker';
  ctx.font = `500 11.5px ${FONT}`; ctx.fillStyle = 'rgba(255,255,255,0.8)'; ctx.textBaseline = 'middle';
  const wtxt = ctx.measureText(text).width;
  if (r.x + 10 + wtxt > W - 8) { ctx.textAlign = 'right'; ctx.fillText(text, r.x - 10, r.y); }
  else { ctx.textAlign = 'left'; ctx.fillText(text, r.x + 10, r.y); }
}

/* ---------- scene 1: sector map ---------- */
const hits = [];
function renderMap() {
  const cam = cams.map, P = projector(cam, 1), E = MAPE;
  drawFrame(P, E, MAPCFG);
  const items = sectors.map(s => {
    const ch = sample(s.change, st.t), ra = sample(s.ratio, st.t), op = sample(s.openings, st.t);
    const wy = mapY(ra), wx = mapX(ch), wz = mapZ(s.automation);
    const p = P(wx, wy, wz), floor = P(wx, -E.hy, wz);
    return { s, ch, ra, op, wy, p, floor, r: P.S * p.k * (0.032 + 0.078 * Math.sqrt(op / maxOpen)) };
  });
  const upto = Math.floor(st.t);
  ctx.lineJoin = 'round'; ctx.lineCap = 'round';
  items.forEach(it => {
    const s = it.s, isSel = st.sel === s.idx, dim = st.sel !== null && !isSel;
    ctx.beginPath();
    for (let i = 0; i <= upto; i++) { const q = P(mapX(s.change[i]), mapY(s.ratio[i]), mapZ(s.automation)); i ? ctx.lineTo(q.x, q.y) : ctx.moveTo(q.x, q.y); }
    ctx.lineTo(it.p.x, it.p.y);
    ctx.strokeStyle = rgb(s.color, dim ? 0.1 : isSel ? 0.9 : 0.4); ctx.lineWidth = isSel ? 2.4 : 1.5; ctx.stroke();
  });
  items.sort((a, b) => b.p.d - a.p.d);
  const yPlane = mapY(1), camAbove = P.D * Math.sin(cam.pitch) > yPlane;
  const behind = it => camAbove ? it.wy < yPlane : it.wy >= yPlane;
  const back = items.filter(behind), front = items.filter(it => !behind(it));
  back.forEach(drawBubble); drawPlane(P, E, yPlane); front.forEach(drawBubble);
  layoutLabels(items);
  items.forEach(drawLabel);
  items.forEach(it => hits.push({ kind: 'sector', i: it.s.idx, x: it.p.x, y: it.p.y, r: it.r, d: it.p.d, ra: it.ra, ch: it.ch, op: it.op }));
}
function drawBubble(it) {
  const s = it.s, isSel = st.sel === s.idx, isHov = st.hover && st.hover.kind === 'sector' && st.hover.i === s.idx;
  const dim = st.sel !== null && !isSel, a = dim ? 0.3 : 1, c = s.color;
  ctx.strokeStyle = rgb(c, 0.4 * a); ctx.lineWidth = 1; ctx.setLineDash([2, 3]);
  seg(it.p, it.floor); ctx.setLineDash([]);
  ctx.beginPath(); ctx.ellipse(it.floor.x, it.floor.y, it.r * 0.7, it.r * 0.7 * 0.38, 0, 0, Math.PI * 2);
  ctx.fillStyle = rgb(c, 0.22 * a); ctx.fill();
  const g = ctx.createRadialGradient(it.p.x - it.r * 0.35, it.p.y - it.r * 0.4, it.r * 0.08, it.p.x, it.p.y, it.r);
  g.addColorStop(0, rgb(shade(c, 1.5), a)); g.addColorStop(0.55, rgb(c, a)); g.addColorStop(1, rgb(shade(c, 0.5), a));
  ctx.save();
  if (isSel || isHov) { ctx.shadowColor = rgb(c, 0.95); ctx.shadowBlur = 26; }
  ctx.beginPath(); ctx.arc(it.p.x, it.p.y, it.r, 0, Math.PI * 2); ctx.fillStyle = g; ctx.fill();
  ctx.restore();
  if (isSel) { ctx.beginPath(); ctx.arc(it.p.x, it.p.y, it.r + 4, 0, Math.PI * 2); ctx.strokeStyle = 'rgba(255,255,255,0.85)'; ctx.lineWidth = 1.6; ctx.stroke(); }
}
function labelFont(it) {
  const s = it.s, big = st.sel === s.idx || (st.hover && st.hover.kind === 'sector' && st.hover.i === s.idx);
  const base = W < 640 ? 10.5 : 12;
  return `${big ? 700 : 500} ${big ? base + 1.5 : base}px ${FONT}`;
}
function layoutLabels(items) {
  // Give every label the first free spot around its bubble so names do not pile up.
  const placed = [], H_ = 15;
  const prio = items.slice().sort((a, b) => {
    const w = it => (st.sel === it.s.idx ? -2 : 0) + (st.hover && st.hover.kind === 'sector' && st.hover.i === it.s.idx ? -1 : 0);
    return (w(a) - w(b)) || (a.p.d - b.p.d);
  });
  prio.forEach(it => {
    ctx.font = labelFont(it);
    const w = ctx.measureText(it.s.name).width, x = it.p.x, y = it.p.y, r = it.r;
    const cands = [
      [x + r + 7, y, 'left'], [x - r - 7, y, 'right'], [x, y - r - 10, 'center'], [x, y + r + 10, 'center'],
      [x + r + 4, y - r - 6, 'left'], [x + r + 4, y + r + 6, 'left'], [x - r - 4, y - r - 6, 'right'], [x - r - 4, y + r + 6, 'right']
    ];
    let chosen = null;
    for (const c of cands) {
      const x0 = c[2] === 'left' ? c[0] : c[2] === 'right' ? c[0] - w : c[0] - w / 2;
      const rect = { x0, x1: x0 + w, y0: c[1] - H_ / 2, y1: c[1] + H_ / 2 };
      if (rect.x0 < 8 || rect.x1 > W - 8 || rect.y0 < 56 || rect.y1 > H - 92) continue;
      const hitsLabel = placed.some(q => rect.x0 < q.x1 && rect.x1 > q.x0 && rect.y0 < q.y1 && rect.y1 > q.y0);
      const hitsBubble = items.some(o => {
        if (o === it) return false;
        const cx = clamp(o.p.x, rect.x0, rect.x1), cy = clamp(o.p.y, rect.y0, rect.y1);
        return Math.hypot(o.p.x - cx, o.p.y - cy) < o.r;
      });
      if (!hitsLabel && !hitsBubble) { chosen = { c, rect }; break; }
      if (!chosen && !hitsLabel) chosen = { c, rect, soft: true };
    }
    if (!chosen) chosen = { c: cands[0], rect: { x0: cands[0][0], x1: cands[0][0] + w, y0: y - H_ / 2, y1: y + H_ / 2 }, soft: true };
    it.lab = { x: chosen.c[0], y: chosen.c[1], align: chosen.c[2] };
    placed.push(chosen.rect);
  });
}
function drawLabel(it) {
  const s = it.s, isSel = st.sel === s.idx, isHov = st.hover && st.hover.kind === 'sector' && st.hover.i === s.idx;
  const dim = st.sel !== null && !isSel && !isHov;
  ctx.font = labelFont(it); ctx.textBaseline = 'middle'; ctx.textAlign = it.lab.align;
  ctx.lineWidth = 3.5; ctx.strokeStyle = 'rgba(12,16,36,0.92)'; ctx.strokeText(s.name, it.lab.x, it.lab.y);
  ctx.fillStyle = dim ? 'rgba(231,235,250,0.4)' : '#eef1ff'; ctx.fillText(s.name, it.lab.x, it.lab.y);
}

/* ---------- scene 2: country towers ---------- */
function renderTowers() {
  const cam = cams.towers, P = projector(cam, W < 640 ? 0.8 : 0.8, -H * 0.045), E = TOWE;
  const cellX = 2 * E.hx / NS, cellZ = 2 * E.hz / NC;
  const cxOf = i => -E.hx + (i + 0.5) * cellX, czOf = j => -E.hz + (j + 0.5) * cellZ;
  const cfg = Object.assign({}, TOWCFG, {
    xTicks: range(0, NS, 1).map(i => ({ w: -E.hx + i * cellX, text: '' })),
    zTicks: range(0, NC, 1).map(j => ({ w: -E.hz + j * cellZ, text: '' }))
  });
  drawFrame(P, E, cfg);

  // sector names along the nearest long edge, country names along the nearest short edge
  const zEdge = P(0, -E.hy, E.hz).d < P(0, -E.hy, -E.hz).d ? E.hz : -E.hz;
  const xEdge = P(E.hx, -E.hy, 0).d < P(-E.hx, -E.hy, 0).d ? E.hx : -E.hx;
  order.forEach((si, i) => {
    const o = outward(P, [cxOf(i), -E.hy, zEdge], 12);
    const dim = st.sel !== null && st.sel !== si;
    ctx.save(); ctx.translate(o.x, o.y); ctx.rotate(-0.55);
    ctx.font = `${st.sel === si ? 700 : 500} 11.5px ${FONT}`; ctx.textAlign = 'right'; ctx.textBaseline = 'middle';
    ctx.fillStyle = dim ? 'rgba(200,208,245,0.4)' : 'rgba(226,231,252,0.95)'; ctx.fillText(sectors[si].name, 0, 0); ctx.restore();
  });
  countries.forEach((name, j) => {
    const o = outward(P, [xEdge, -E.hy, czOf(j)], 14);
    ctx.font = `600 12px ${FONT}`; ctx.textBaseline = 'middle'; ctx.textAlign = o.dx > 0 ? 'left' : 'right';
    ctx.fillStyle = 'rgba(226,231,252,0.95)'; ctx.fillText(name, o.x, o.y);
  });

  const bars = [];
  for (let j = 0; j < NC; j++) for (let i = 0; i < NS; i++) {
    const si = order[i], o = sample(gridO[j][si], st.t), k = sample(gridK[j][si], st.t);
    bars.push({ j, i, si, o, k, r: o / k, cx: cxOf(i), cz: czOf(j), d: P(cxOf(i), -E.hy, czOf(j)).d });
  }
  bars.sort((a, b) => b.d - a.d);
  bars.forEach(b => drawBar(P, E, b, cellX * 0.37, cellZ * 0.37));
  drawPlane(P, E, towY(1));
}
function drawBar(P, E, b, bx, bz) {
  const x0 = b.cx - bx, x1 = b.cx + bx, z0 = b.cz - bz, z1 = b.cz + bz, y0 = -E.hy, y1 = Math.max(y0 + 0.012, towY(b.r));
  const isSel = st.sel === b.si, dim = st.sel !== null && !isSel;
  const isHov = st.hover && st.hover.kind === 'bar' && st.hover.j === b.j && st.hover.si === b.si;
  const base = divColor(b.r), alpha = dim ? 0.28 : 1, boost = isHov ? 1.22 : 1;
  const faces = [
    { n: [0, 1, 0], v: [[x0, y1, z0], [x1, y1, z0], [x1, y1, z1], [x0, y1, z1]], k: 1.0 },
    { n: [0, 0, -1], v: [[x0, y0, z0], [x1, y0, z0], [x1, y1, z0], [x0, y1, z0]], k: 0.72 },
    { n: [0, 0, 1], v: [[x0, y0, z1], [x1, y0, z1], [x1, y1, z1], [x0, y1, z1]], k: 0.72 },
    { n: [-1, 0, 0], v: [[x0, y0, z0], [x0, y0, z1], [x0, y1, z1], [x0, y1, z0]], k: 0.72 },
    { n: [1, 0, 0], v: [[x1, y0, z0], [x1, y0, z1], [x1, y1, z1], [x1, y1, z0]], k: 0.72 }
  ];
  const L = [-0.35, 0.8, -0.5], ll = Math.hypot(...L);
  faces.forEach(f => {
    const nv = P.view(f.n[0], f.n[1], f.n[2]);
    const cen = f.v.reduce((a, p) => [a[0] + p[0] / 4, a[1] + p[1] / 4, a[2] + p[2] / 4], [0, 0, 0]);
    const cv3 = P.view(cen[0], cen[1], cen[2]);
    if (nv[0] * (0 - cv3[0]) + nv[1] * (0 - cv3[1]) + nv[2] * (-P.D - cv3[2]) <= 0) return;
    const lit = 0.55 + 0.6 * Math.max(0, (nv[0] * L[0] + nv[1] * L[1] + nv[2] * L[2]) / ll);
    const pts = f.v.map(p => P(p[0], p[1], p[2]));
    path(pts); ctx.closePath();
    ctx.fillStyle = rgb(shade(base, lit * boost), alpha); ctx.fill();
    ctx.strokeStyle = isSel || isHov ? 'rgba(255,255,255,0.9)' : 'rgba(8,11,30,0.55)'; ctx.lineWidth = isSel || isHov ? 1.4 : 0.7; ctx.stroke();
    hits.push({ kind: 'bar', j: b.j, si: b.si, poly: pts, d: b.d, r: b.r, o: b.o, k: b.k });
  });
}

/* ---------- picking, tooltip ---------- */
function inPoly(x, y, poly) {
  let inside = false;
  for (let i = 0, j = poly.length - 1; i < poly.length; j = i++) {
    const a = poly[i], b = poly[j];
    if ((a.y > y) !== (b.y > y) && x < (b.x - a.x) * (y - a.y) / (b.y - a.y) + a.x) inside = !inside;
  }
  return inside;
}
function pick(x, y) {
  let best = null;
  for (const h of hits) {
    const ok = h.kind === 'sector' ? Math.hypot(x - h.x, y - h.y) <= h.r + 5 : inPoly(x, y, h.poly);
    if (ok && (!best || h.d < best.d)) best = h;
  }
  return best;
}
const tip = $('tip'); let tipKey = '';
function updateTip() {
  const h = st.hover;
  if (!h || !st.inside || st.dragging) { tip.hidden = true; tipKey = ''; return; }
  const yr = YEARS[Math.round(st.t)];
  let html;
  if (h.kind === 'sector') {
    const s = sectors[h.i];
    html = `<b>${s.name}</b><span>${h.ra.toFixed(2)} openings per job seeker in ${yr}</span><span>Openings ${fmtPct(h.ch)} vs ${YEARS[0]}</span><span>Danger score ${Math.round(s.risk)} of 100</span>`;
  } else {
    html = `<b>${sectors[h.si].name}, ${countries[h.j]}</b><span>${h.r.toFixed(2)} openings per job seeker in ${yr}</span><span>${fmtInt(h.o)} openings for ${fmtInt(h.k)} job seekers</span>`;
  }
  if (html !== tipKey) { tip.innerHTML = html; tipKey = html; }
  tip.hidden = false;
  const tw = tip.offsetWidth, th = tip.offsetHeight;
  tip.style.left = clamp(st.px + 16, 8, W - tw - 8) + 'px';
  tip.style.top = clamp(st.py + 16, 8, H - th - 8) + 'px';
}

/* ---------- detail panel, ranking, findings ---------- */
const detail = $('detail');
function renderDetail() {
  if (st.sel === null) { detail.hidden = true; return; }
  const s = sectors[st.sel], ti = Math.round(st.t), ra = s.ratio[ti];
  detail.hidden = false;
  $('dName').textContent = s.name;
  const mk = $('dMarket'); mk.textContent = marketOf(ra); mk.style.color = rgb(marketCol(ra));
  $('dRatio').textContent = ra.toFixed(2);
  $('dChangeL').textContent = 'Openings vs ' + YEARS[0];
  $('dChange').textContent = fmtPct(s.change[ti]);
  $('dAuto').textContent = Math.round(s.automation * 100) + '%';
  $('dRiskL').textContent = 'Danger score in ' + YEARS[LAST];
  $('dRisk').textContent = Math.round(s.risk) + ' of 100';
  const w = 240, h = 58, p = 5, top = Math.max(1.2, ...s.ratio) * 1.1;
  const X = i => p + i * (w - 2 * p) / (NY - 1), Yy = v => h - p - v / top * (h - 2 * p);
  const line = s.ratio.map((v, i) => (i ? 'L' : 'M') + X(i).toFixed(1) + ' ' + Yy(v).toFixed(1)).join('');
  $('dSpark').innerHTML = `<line x1="${p}" x2="${w - p}" y1="${Yy(1)}" y2="${Yy(1)}" stroke="rgba(255,255,255,.45)" stroke-dasharray="4 4"/>` +
    `<path d="${line}" fill="none" stroke="${rgb(s.color)}" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"/>` +
    `<circle cx="${X(ti)}" cy="${Yy(s.ratio[ti])}" r="4.5" fill="${rgb(s.color)}" stroke="#fff" stroke-width="1.5"/>`;
}
const rankEl = $('rank');
const rankSorted = [...sectors].sort((a, b) => b.risk - a.risk);
rankSorted.forEach(s => {
  const li = document.createElement('li');
  li.innerHTML = `<button type="button" data-i="${s.idx}" aria-pressed="false">
    <span class="nm">${s.name}<small>${s.ratioNow.toFixed(2)} openings per job seeker, openings ${fmtPct((Math.pow(1 + s.growthCagr / 100, LAST) - 1) * 100)} since ${YEARS[0]}</small></span>
    <span class="track"><span class="fill" style="width:${s.risk.toFixed(1)}%;background:${rgb(s.color)}"></span></span>
    <span class="sc">${Math.round(s.risk)}</span></button>`;
  rankEl.appendChild(li);
});
rankEl.addEventListener('click', e => {
  const b = e.target.closest('button'); if (!b) return;
  select(+b.dataset.i);
  stage.scrollIntoView({ behavior: REDUCE ? 'auto' : 'smooth', block: 'center' });
});
function select(i) {
  st.sel = (i === null || st.sel === i) ? null : i;
  rankEl.querySelectorAll('button').forEach(b => b.setAttribute('aria-pressed', String(+b.dataset.i === st.sel)));
  renderDetail();
}
(function findings() {
  const by = (f, dir) => sectors.reduce((a, b) => (dir * (f(b) - f(a)) > 0 ? b : a));
  const hi = by(s => s.ratioNow, 1), lo = by(s => s.ratioNow, -1), gr = by(s => s.growthCagr, 1), rk = by(s => s.risk, 1);
  const items = [
    ['Most short of people', hi.name, `${hi.ratioNow.toFixed(2)} openings for every job seeker`, C.teal],
    ['Most crowded', lo.name, `only ${lo.ratioNow.toFixed(2)} openings per job seeker`, C.coral],
    ['Fastest growing openings', gr.name, `${fmtPct(gr.growthCagr)} a year since ${YEARS[0]}`, C.green],
    ['Most in danger', rk.name, `danger score ${Math.round(rk.risk)} of 100`, C.coral]
  ];
  $('finds').innerHTML = items.map(([k, n, d, c]) => `<div class="find" style="--c:${rgb(c)}"><small>${k}</small><b>${n}</b><span>${d}</span></div>`).join('');
})();

/* ---------- UI wiring ---------- */
const slider = $('slider'), yearEl = $('year'), playBtn = $('play');
slider.max = LAST;
$('ticks').innerHTML = YEARS.map(y => `<span>${y}</span>`).join('');
function updateYear() {
  yearEl.textContent = YEARS[Math.round(st.t)];
  slider.value = st.t; slider.style.setProperty('--p', (st.t / LAST * 100) + '%');
}
let lastYearIdx = -1;
function setPlaying(v) { st.playing = v; playBtn.classList.toggle('on', v); playBtn.setAttribute('aria-label', v ? 'Pause' : 'Play'); }
playBtn.addEventListener('click', () => { if (!st.playing && st.t >= LAST - 0.001) st.t = 0; setPlaying(!st.playing); });
slider.addEventListener('input', () => { st.t = parseFloat(slider.value); setPlaying(false); updateYear(); renderDetail(); });

function setAuto(v) { st.auto = v; $('bAuto').setAttribute('aria-pressed', String(v)); }
$('bAuto').addEventListener('click', () => setAuto(!st.auto));
$('bIn').addEventListener('click', () => { const c = cams[st.view]; c.zoom = clamp(c.zoom * 1.18, 0.55, 2.4); });
$('bOut').addEventListener('click', () => { const c = cams[st.view]; c.zoom = clamp(c.zoom / 1.18, 0.55, 2.4); });
$('bReset').addEventListener('click', () => { cams[st.view] = Object.assign({}, camDefault[st.view]); });

function setView(v) {
  st.view = v; st.hover = null;
  $('tab-map').setAttribute('aria-selected', String(v === 'map'));
  $('tab-towers').setAttribute('aria-selected', String(v === 'towers'));
  renderLegend();
}
$('tab-map').addEventListener('click', () => setView('map'));
$('tab-towers').addEventListener('click', () => setView('towers'));
function renderLegend() {
  const g = st.view === 'map'
    ? `linear-gradient(90deg,${rgb(C.teal)},${rgb(C.amber)},${rgb(C.coral)})`
    : `linear-gradient(90deg,${rgb(C.red)},${rgb(C.amber)},${rgb(C.green)})`;
  $('legend').innerHTML = st.view === 'map'
    ? `<span class="grad" style="background:${g}"></span><span class="ends"><span>Safer</span><span>More at risk</span></span><p>Color: danger score. Size: openings. Depth: automation risk.</p>`
    : `<span class="grad" style="background:${g}"></span><span class="ends"><span>Too many people</span><span>Too few</span></span><p>Height: openings per job seeker. Sectors run from most to least short of people.</p>`;
}

/* pointer, wheel, keyboard */
const ptrs = new Map(); let travelled = 0, downAt = null, pinch0 = 0, zoom0 = 1;
cv.addEventListener('pointerdown', e => {
  cv.setPointerCapture(e.pointerId); ptrs.set(e.pointerId, { x: e.offsetX, y: e.offsetY });
  st.dragging = true; travelled = 0; downAt = { x: e.offsetX, y: e.offsetY }; setAuto(false);
  if (ptrs.size === 2) { const [a, b] = [...ptrs.values()]; pinch0 = Math.hypot(a.x - b.x, a.y - b.y); zoom0 = cams[st.view].zoom; }
});
cv.addEventListener('pointermove', e => {
  st.px = e.offsetX; st.py = e.offsetY; st.inside = true;
  const p = ptrs.get(e.pointerId); if (!p) return;
  const dx = e.offsetX - p.x, dy = e.offsetY - p.y; p.x = e.offsetX; p.y = e.offsetY; travelled += Math.abs(dx) + Math.abs(dy);
  const cam = cams[st.view];
  if (ptrs.size === 1) { cam.yaw += dx * 0.0065; cam.pitch = clamp(cam.pitch + dy * 0.005, 0.04, 1.4); }
  else if (ptrs.size === 2 && pinch0) { const [a, b] = [...ptrs.values()]; cam.zoom = clamp(zoom0 * Math.hypot(a.x - b.x, a.y - b.y) / pinch0, 0.55, 2.4); }
});
function endPointer(e) {
  ptrs.delete(e.pointerId);
  if (ptrs.size === 0) {
    st.dragging = false;
    if (travelled < 6 && downAt) { const h = pick(downAt.x, downAt.y); select(h ? (h.kind === 'sector' ? h.i : h.si) : null); }
    downAt = null; pinch0 = 0;
  }
}
cv.addEventListener('pointerup', endPointer);
cv.addEventListener('pointercancel', e => { ptrs.delete(e.pointerId); st.dragging = ptrs.size > 0; });
cv.addEventListener('pointerleave', () => { st.inside = false; });
cv.addEventListener('wheel', e => { e.preventDefault(); const c = cams[st.view]; c.zoom = clamp(c.zoom * Math.exp(-e.deltaY * 0.0012), 0.55, 2.4); }, { passive: false });
cv.addEventListener('keydown', e => {
  const c = cams[st.view]; let used = true;
  if (e.key === 'ArrowLeft') c.yaw -= 0.1; else if (e.key === 'ArrowRight') c.yaw += 0.1;
  else if (e.key === 'ArrowUp') c.pitch = clamp(c.pitch + 0.06, 0.04, 1.4); else if (e.key === 'ArrowDown') c.pitch = clamp(c.pitch - 0.06, 0.04, 1.4);
  else if (e.key === '+' || e.key === '=') c.zoom = clamp(c.zoom * 1.12, 0.55, 2.4); else if (e.key === '-') c.zoom = clamp(c.zoom / 1.12, 0.55, 2.4);
  else if (e.key === 'Escape') select(null); else used = false;
  if (used) { e.preventDefault(); if (e.key.startsWith('Arrow')) setAuto(false); }
});
$('dClose').addEventListener('click', () => select(null));

/* ---------- main loop ---------- */
let prev = performance.now(), visible = true;
new IntersectionObserver(([en]) => { visible = en.isIntersecting; }).observe(stage);
function tick(now) {
  const dt = clamp((now - prev) / 1000, 0, 0.05); prev = now;
  if (st.playing) { st.t += dt / 1.15; if (st.t >= LAST) { st.t = LAST; setPlaying(false); } updateYear(); }
  if (st.auto && !st.dragging && !REDUCE) cams[st.view].yaw += dt * 0.15;
  const yi = Math.round(st.t); if (yi !== lastYearIdx) { lastYearIdx = yi; renderDetail(); }
  if (visible) {
    ctx.setTransform(DPR, 0, 0, DPR, 0, 0); ctx.clearRect(0, 0, W, H);
    hits.length = 0;
    if (st.view === 'map') renderMap(); else renderTowers();
    st.hover = st.inside && !st.dragging ? pick(st.px, st.py) : null;
    cv.style.cursor = st.dragging ? 'grabbing' : st.hover ? 'pointer' : 'grab';
    updateTip();
  }
  requestAnimationFrame(tick);
}

/* ---------- start ---------- */
$('w1').textContent = Math.round(META.weights.oversupply * 100) + '%';
$('w2').textContent = Math.round(META.weights.decline * 100) + '%';
$('w3').textContent = Math.round(META.weights.automation * 100) + '%';
if (META.github) { const a = $('gh'); a.href = META.github; a.hidden = false; }
$('by').textContent = META.author ? 'Project by ' + META.author + '.' : '';
renderLegend();
if (REDUCE) { st.t = LAST; } else { st.t = 0; setPlaying(true); }
updateYear();
requestAnimationFrame(tick);
})();
</script>
</body>
</html>
ploading index.html…]()
<img width="760" height="395" alt="demo" src="https://github.com/user-attachments/assets/fe250330-f691-4353-8562-6cb1c2c712f4" />
[index.html](https://github.com/user-attachments/files/32421878/index.html)

<img width="1280" height="665" alt="preview" src="https://github.com/user-attachments/assets/19bfea97-297d-4c14-8f77-ffe21971b474" />
https://github.com/user-attachments/assets/b5c6eda8-f091-4650-a54f-a998ca4a2d83
[job-market-project.zip](https://github.com/user-attachments/files/32421882/job-market-project.zip)
[README.md](https://github.com/user-attachments/files/32421900/README.md)

# Which jobs have too few people, and which have too many?

An interactive 3D map of job supply and demand: 12 sectors, 6 countries, 2019 to 2025.
It shows where openings outnumber job seekers (cybersecurity, AI, healthcare), where people
outnumber openings (data entry, call centers, bank tellers), and which jobs are most at risk.

![Animated preview of the 3D job market map](media/demo.gif)

https://yov-devs10.github.io/job-market-3d-map/

> **Simulated data.** Every number in this repo is generated by `make_sample_data.py` to test
> the method. Sectors were given realistic shapes (for example, more cybersecurity openings than
> qualified people), so the story was designed in. None of it is a real statistic. Replace
> `jobs_data.csv` with a real dataset to learn something new.

## What it does

1. **Measures the gap.** For every sector: openings per job seeker (vacancies divided by people
   looking for work). Above 1, employers struggle to find people. Below 1, people struggle to find jobs.
2. **Scores the danger.** A 0 to 100 score that blends three signals: too many people for the
   openings (40%), openings shrinking each year (30%), and how easily automation could do the work (30%).
   The weights are judgment calls, set at the top of `analyze_jobs.py`.
3. **Shows it in 3D.** `index.html` has two views you can rotate, zoom and scrub through time:
   - **Sector map:** each sector is a bubble. Height is openings per job seeker, left to right is
     change in openings since 2019, depth is automation risk, size is number of openings.
   - **Country towers:** every country and sector as a tower whose height is openings per job seeker.

The 3D view is drawn on an HTML canvas with plain JavaScript. There are no chart libraries.

## Run it

```bash
pip install -r requirements.txt

python make_sample_data.py     # writes jobs_data.csv (simulated)
python analyze_jobs.py         # prints a report, saves charts/ and sector_summary.csv
python make_3d_dashboard.py    # writes index.html, the interactive 3D page
```

Every country needs a row for every sector and every year. Job seekers per sector is the hardest
column to source: it usually has to be estimated, for example from unemployment by previous
occupation. Automation risk can come from published studies of job exposure to AI.
Places to look: ILOSTAT, OECD, the WEF Future of Jobs Report, Indeed Hiring Lab.


Built with Python (pandas, matplotlib) and JavaScript.
