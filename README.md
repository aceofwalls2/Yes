<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>UC-NET // ARK ARCHIVE // LEVEL 7 OVERRIDE</title>
<style>
:root{
  --primary-red:#cc0000; --bright-red:#ff3333; --dark-red:#800000; --blood:#5a0000;
  --bg-black:#040404; --terminal-gray:#101012; --text-light:#e0e0e0;
  --green-glow:#33ff66; --hud-blue:#2aa9ff; --amber:#ffaa00; --bone:#d8d2c4;
}
*{box-sizing:border-box;}
body,html{margin:0;padding:0;height:100%;background:var(--bg-black);color:var(--text-light);
  font-family:'Courier New',Courier,monospace;overflow:hidden;}

/* ---- CRT FX ---- */
.scanlines{position:fixed;inset:0;z-index:9999;pointer-events:none;
  background:linear-gradient(rgba(18,16,16,0) 50%,rgba(0,0,0,0.28) 50%),
             linear-gradient(90deg,rgba(255,0,0,0.06),rgba(0,255,0,0.02),rgba(0,0,255,0.06));
  background-size:100% 3px,3px 100%;animation:flick 0.15s infinite;}
@keyframes flick{0%{opacity:0.97}50%{opacity:1}100%{opacity:0.98}}
.screen-glare{position:fixed;inset:0;z-index:9998;pointer-events:none;
  background:radial-gradient(circle at center,rgba(255,255,255,0.035) 0%,rgba(0,0,0,0.55) 100%);}
.vignette{position:fixed;inset:0;z-index:9997;pointer-events:none;box-shadow:inset 0 0 220px rgba(0,0,0,0.95);}

/* ---- BOOT ---- */
#boot-sequence{position:absolute;inset:0;background:#000;color:var(--text-light);
  padding:26px;font-size:11pt;display:flex;flex-direction:column;z-index:1000;overflow:hidden;}
.boot-line{margin-bottom:3px;opacity:0;animation:showLine 0.1s forwards;white-space:pre-wrap;}
.boot-line .ok{color:var(--green-glow);} .boot-line .wn{color:var(--amber);} .boot-line .er{color:var(--bright-red);}
@keyframes showLine{to{opacity:1}}

/* ---- LOGIN (RE9 style) ---- */
#login-screen{display:none;justify-content:center;align-items:center;height:100%;z-index:500;position:relative;
  background:radial-gradient(ellipse at 50% 40%,#240707 0%,#0a0202 55%,#000 100%);}
.login-bg-grid{position:absolute;inset:0;opacity:0.12;pointer-events:none;
  background-image:linear-gradient(rgba(204,0,0,0.4) 1px,transparent 1px),linear-gradient(90deg,rgba(204,0,0,0.4) 1px,transparent 1px);
  background-size:46px 46px;mask-image:radial-gradient(ellipse at center,#000 30%,transparent 75%);
  -webkit-mask-image:radial-gradient(ellipse at center,#000 30%,transparent 75%);animation:gridDrift 18s linear infinite;}
@keyframes gridDrift{from{background-position:0 0}to{background-position:46px 46px}}
.embers{position:absolute;inset:0;pointer-events:none;overflow:hidden;}
.ember{position:absolute;bottom:-10px;width:3px;height:3px;background:var(--bright-red);border-radius:50%;
  box-shadow:0 0 6px var(--bright-red);opacity:0;animation:rise linear infinite;}
@keyframes rise{0%{transform:translateY(0);opacity:0}10%{opacity:0.9}100%{transform:translateY(-105vh);opacity:0}}

.login-stage{position:relative;z-index:2;display:flex;flex-direction:column;align-items:center;width:540px;}
.big-logo-wrap{position:relative;margin-bottom:6px;}
.umbrella-logo{width:170px;height:170px;filter:drop-shadow(0 0 22px rgba(204,0,0,0.9));animation:logoPulse 4s ease-in-out infinite;}
@keyframes logoPulse{0%,100%{filter:drop-shadow(0 0 16px rgba(204,0,0,0.7))}50%{filter:drop-shadow(0 0 30px rgba(255,51,51,1))}}
.logo-ring{position:absolute;inset:-26px;border:1px solid rgba(204,0,0,0.5);border-radius:50%;animation:spin 14s linear infinite;}
.logo-ring::before{content:'';position:absolute;top:-4px;left:50%;width:7px;height:7px;background:var(--bright-red);border-radius:50%;box-shadow:0 0 10px var(--bright-red);}
.logo-ring.r2{inset:-44px;border-style:dashed;border-color:rgba(204,0,0,0.3);animation-duration:22s;animation-direction:reverse;}
@keyframes spin{to{transform:rotate(360deg)}}

.brand{color:#fff;letter-spacing:10px;font-size:30pt;font-weight:bold;margin:14px 0 2px;text-shadow:0 0 18px rgba(204,0,0,0.6);}
.brand .b{color:var(--primary-red);}
.tagline{color:#8a8a8a;letter-spacing:6px;font-size:8.5pt;margin-bottom:4px;}
.divider{width:100%;height:1px;background:linear-gradient(90deg,transparent,var(--primary-red),transparent);margin:18px 0;}

.login-box{background:rgba(8,8,8,0.82);border:1px solid rgba(204,0,0,0.6);padding:30px 36px;width:100%;
  box-shadow:inset 0 0 26px rgba(204,0,0,0.18),0 0 40px rgba(204,0,0,0.4);position:relative;}
.login-box::before{content:'';position:absolute;top:0;left:0;width:100%;height:2px;background:var(--primary-red);box-shadow:0 0 12px var(--bright-red);}
.login-box .corner{position:absolute;width:14px;height:14px;border:2px solid var(--bright-red);}
.corner.tl{top:-2px;left:-2px;border-right:none;border-bottom:none;}
.corner.tr{top:-2px;right:-2px;border-left:none;border-bottom:none;}
.corner.bl{bottom:-2px;left:-2px;border-right:none;border-top:none;}
.corner.br{bottom:-2px;right:-2px;border-left:none;border-top:none;}
.box-title{color:#fff;letter-spacing:3px;font-size:11pt;text-transform:uppercase;border-bottom:1px solid #333;padding-bottom:10px;margin-bottom:4px;display:flex;justify-content:space-between;}
.box-title .stat{color:var(--green-glow);font-size:8pt;letter-spacing:1px;}
.input-group{margin-top:18px;}
.input-group label{display:block;font-size:8.5pt;color:var(--primary-red);margin-bottom:7px;font-weight:bold;letter-spacing:2px;}
.input-group input{width:100%;background:#000;border:1px solid #444;color:#fff;padding:12px;font-family:monospace;font-size:12pt;transition:all 0.3s;letter-spacing:3px;}
.input-group input:focus{border-color:var(--bright-red);outline:none;box-shadow:0 0 10px rgba(255,51,51,0.55);}
.btn-submit{margin-top:24px;background:transparent;color:var(--primary-red);border:1px solid var(--primary-red);padding:14px;width:100%;
  cursor:pointer;font-family:monospace;font-weight:bold;font-size:11pt;letter-spacing:3px;transition:all 0.2s;}
.btn-submit:hover{background:var(--primary-red);color:#fff;box-shadow:0 0 18px var(--primary-red);}
.hint{font-size:8pt;color:#4a4a4a;margin-top:12px;text-align:center;letter-spacing:1px;}

/* ---- SECURITY WARNING OVERLAY CSS ---- */
.security-warning-box {
  margin-top: 20px;
  border: 1px solid var(--primary-red);
  background: rgba(204, 0, 0, 0.05);
  padding: 12px 16px;
  font-size: 7.5pt;
  color: #a0a0a0;
  line-height: 1.5;
  text-align: justify;
  letter-spacing: 0.5px;
  position: relative;
}
.security-warning-box strong {
  color: var(--bright-red);
  letter-spacing: 1px;
  display: block;
  margin-bottom: 4px;
}

/* ---- DASHBOARD ---- */
#main-system{display:none;height:100%;grid-template-columns:310px 1fr 330px;}
.sidebar{background:var(--terminal-gray);border-right:1px solid #333;display:flex;flex-direction:column;}
.sidebar-header{padding:18px;border-bottom:1px solid var(--primary-red);text-align:center;background:rgba(204,0,0,0.1);}
.sidebar-header .logo-mini{width:54px;height:54px;margin:0 auto 8px;display:block;filter:drop-shadow(0 0 6px rgba(204,0,0,0.7));}
.sidebar-header h3{margin:4px 0;font-size:12pt;color:#fff;letter-spacing:2px;}
.nav-menu{list-style:none;padding:0;margin:0;flex-grow:1;overflow-y:auto;}
.nav-menu::-webkit-scrollbar{width:6px;} .nav-menu::-webkit-scrollbar-thumb{background:var(--dark-red);}
.nav-group{padding:10px 20px 4px;font-size:7.5pt;color:#555;letter-spacing:2px;border-top:1px solid #1c1c1c;}
.nav-item{padding:12px 20px;cursor:pointer;border-bottom:1px solid #161616;border-left:3px solid transparent;font-size:9pt;color:#888;transition:all 0.2s;letter-spacing:0.5px;}
.nav-item:hover{background:#1a1a1a;color:#ddd;}
.nav-item.active{background:#2a0a0a;border-left-color:var(--bright-red);color:#fff;font-weight:bold;box-shadow:inset 22px 0 22px -22px var(--primary-red);}
.user-card{padding:14px 18px;border-top:1px solid #333;font-size:8.5pt;color:#888;background:#0b0b0b;}
.user-card b{color:var(--green-glow);} .user-card .lvl{color:var(--bright-red);font-weight:bold;}

.main-content{padding:40px 48px;overflow-y:auto;position:relative;background:linear-gradient(180deg,#0a0a0a 0%,#141414 100%);}
.main-content::-webkit-scrollbar{width:8px;} .main-content::-webkit-scrollbar-thumb{background:var(--dark-red);}
.node-container{display:none;animation:fadeIn 0.4s;}
.node-container.active{display:block;}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}

h1{color:#fff;font-size:21pt;border-bottom:2px solid var(--primary-red);padding-bottom:10px;margin-top:0;letter-spacing:1px;}
.eyebrow{color:var(--primary-red);font-size:8.5pt;letter-spacing:4px;margin-bottom:-6px;}
h3{color:var(--primary-red);margin-top:28px;font-size:12.5pt;letter-spacing:1px;}
h4.sec{color:var(--hud-blue);margin-top:20px;font-size:11pt;letter-spacing:1px;}
p{line-height:1.65;font-size:10.5pt;color:#cfcfcf;margin-bottom:14px;text-align:justify;}
.redacted{background:#222;color:#222;cursor:crosshair;transition:all 0.25s;padding:0 4px;border-radius:2px;}
.redacted:hover{background:transparent;color:var(--bright-red);text-shadow:0 0 6px var(--bright-red);}

.system-table{width:100%;border-collapse:collapse;margin:22px 0;border:1px solid #333;}
.system-table th,.system-table td{border:1px solid #2c2c2c;padding:10px 12px;font-size:9.5pt;line-height:1.45;vertical-align:top;}
.system-table th{background:#111;color:var(--primary-red);text-transform:uppercase;letter-spacing:1px;}
.system-table td:first-child{color:#fff;font-weight:bold;white-space:nowrap;}
.system-table tr:nth-child(even){background:rgba(255,0,0,0.025);}

.hex-block{background:#000;border:1px solid #333;padding:22px;font-family:monospace;font-size:9pt;color:#6a6a6a;margin:20px 0;position:relative;line-height:1.5;white-space:pre-wrap;}
.hex-block[data-lbl]::before{content:attr(data-lbl);position:absolute;top:-10px;left:15px;background:#000;padding:0 10px;color:var(--primary-red);font-size:8pt;font-weight:bold;letter-spacing:1px;}

.alert-box{border:1px solid var(--bright-red);background:rgba(204,0,0,0.1);padding:14px 18px;margin:20px 0;border-left:5px solid var(--bright-red);}
.alert-box h4{margin:0 0 8px 0;color:#fff;font-size:11pt;display:flex;align-items:center;letter-spacing:1px;}
.alert-box h4::before{content:'\26A0';margin-right:10px;color:var(--bright-red);font-size:14pt;}
.alert-box.amber{border-color:var(--amber);background:rgba(255,170,0,0.08);border-left-color:var(--amber);}
.alert-box.amber h4::before{color:var(--amber);}
.alert-box.blue{border-color:var(--hud-blue);background:rgba(42,169,255,0.08);border-left-color:var(--hud-blue);}
.alert-box.blue h4::before{content:'\2139';color:var(--hud-blue);}

.stamp{display:inline-block;border:3px solid var(--bright-red);color:var(--bright-red);padding:6px 14px;font-weight:bold;letter-spacing:3px;transform:rotate(-5deg);opacity:0.85;font-size:13pt;margin:10px 0;text-shadow:0 0 6px rgba(255,51,51,0.4);}
.dossier-photo{border:1px solid #333;background:#0c0c0c;padding:16px;margin:18px 0;text-align:center;color:#555;font-size:8.5pt;letter-spacing:1px;border-left:4px solid var(--dark-red);}
.dossier-photo .ph{border:1px dashed #444;padding:30px 10px;color:#666;font-size:9pt;}

.live-log{background:#070707;border-left:1px solid #222;display:flex;flex-direction:column;font-size:8.5pt;}
.log-header{padding:14px;border-bottom:1px solid #222;color:var(--green-glow);font-weight:bold;background:#101010;text-align:center;letter-spacing:1px;}
.log-content{padding:14px;flex-grow:1;overflow-y:hidden;}
.log-line{margin-bottom:6px;color:#5a5a5a;word-wrap:break-word;line-height:1.4;}
.log-line .timestamp{color:var(--hud-blue);margin-right:5px;}
.log-line .success{color:var(--green-glow);} .log-line .warn{color:var(--amber);} .log-line .err{color:var(--bright-red);}
.threat-meter{padding:12px 14px;border-top:1px solid #222;background:#0a0a0a;}
.threat-meter .bar{height:8px;background:#1a1a1a;border:1px solid #333;margin-top:6px;overflow:hidden;}
.threat-meter .fill{height:100%;width:78%;background:linear-gradient(90deg,var(--amber),var(--bright-red));animation:pulseBar 2.5s infinite;}
@keyframes pulseBar{0%,100%{width:70%}50%{width:86%}}
.disclaimer{font-size:7.5pt;color:#3a3a3a;text-align:center;padding:8px;letter-spacing:0.5px;}
</style>
</head>
<body>
<div class="scanlines"></div>
<div class="screen-glare"></div>
<div class="vignette"></div>

<!-- BOOT -->
<div id="boot-sequence"></div>

<!-- LOGIN -->
<div id="login-screen">
  <div class="login-bg-grid"></div>
  <div class="embers" id="embers"></div>
  <div class="login-stage">
    <div class="big-logo-wrap">
      <div class="logo-ring"></div>
      <div class="logo-ring r2"></div>
      <img class="umbrella-logo" src="https://raw.githubusercontent.com/aceofwalls2/Yes/refs/heads/main/IMG_20260530_102835.png" alt="Umbrella Logo">
    </div>
    <div class="brand"><span class="b">U</span>MBRELLA</div>
    <div class="tagline">OUR BUSINESS IS LIFE ITSELF</div>
    <div class="divider"></div>
    <div class="login-box">
      <span class="corner tl"></span><span class="corner tr"></span><span class="corner bl"></span><span class="corner br"></span>
      <div class="box-title"><span>ARK ARCHIVE // SECURE GATEWAY</span><span class="stat">&#9679; LIVE</span></div>
      <div class="input-group">
        <label>OPERATOR ID</label>
        <input type="text" id="username" value="BRAYLIN" autocomplete="off">
      </div>
      <div class="input-group">
        <label>DECRYPTION OVERRIDE KEY</label>
        <input type="password" id="password" placeholder="ENTER ACCESS KEY..." autocomplete="off"
          onkeydown="if(event.key==='Enter') validateLogin()">
      </div>
      <button class="btn-submit" onclick="validateLogin()">&gt;&gt; AUTHENTICATE</button>
      <div id="error-msg" style="color:var(--bright-red);font-size:9.5pt;margin-top:14px;text-align:center;min-height:15px;letter-spacing:1px;"></div>
      
      <!-- ADDED SECURITY WARNING BOX -->
      <div class="security-warning-box">
        <strong>SECURITY WARNING:</strong> Anything viewed beyond this screen is covered under the Umbrella Corporation Security Agreement. Unauthorized use, tampering or recording will be punished under Umbrella Corporation's Treason and Terrorism Directive (Article 12, Paragraph 19, Section C).
      </div>
      
      <div class="hint" style="margin-top:18px;">CLEARANCE LEVEL 7 (OMEGA) REQUIRED &nbsp;//&nbsp; FILE PV-001 &nbsp;//&nbsp; ALL ATTEMPTS LOGGED</div>
    </div>
  </div>
</div>

<!-- DASHBOARD -->
<div id="main-system">
  <div class="sidebar">
    <div class="sidebar-header">
      <svg class="logo-mini" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
        <polygon points="30,4 70,4 96,30 96,70 70,96 30,96 4,70 4,30" fill="#fff" stroke="#cc0000" stroke-width="3"/>
        <polygon points="50,50 30,4 70,4" fill="#cc0000"/><polygon points="50,50 96,30 96,70" fill="#cc0000"/>
        <polygon points="50,50 70,96 30,96" fill="#cc0000"/><polygon points="50,50 4,70 4,30" fill="#cc0000"/>
        <rect x="46" y="6" width="8" height="88" fill="#fff"/><rect x="6" y="46" width="88" height="8" fill="#fff"/>
      </svg>
      <h3>UC-NET // ARK NODE</h3>
      <div style="font-size:8pt;color:var(--green-glow);">&#9679; LIVE SECURE CONNECTION</div>
    </div>
    <ul class="nav-menu">
      <li class="nav-group">// CASE FILE: REQUIEM</li>
      <li class="nav-item active" onclick="switchNode('overview',this)">01 // CASE OVERVIEW</li>
      <li class="nav-item" onclick="switchNode('synopsis',this)">02 // REQUIEM — FULL SYNOPSIS</li>
      <li class="nav-item" onclick="switchNode('endings',this)">03 // BRANCHING ENDINGS</li>
      <li class="nav-group">// PERSONNEL</li>
      <li class="nav-item" onclick="switchNode('grace',this)">04 // GRACE ASHCROFT</li>
      <li class="nav-item" onclick="switchNode('leon',this)">05 // LEON S. KENNEDY</li>
      <li class="nav-item" onclick="switchNode('gideon',this)">06 // VICTOR GIDEON</li>
      <li class="nav-item" onclick="switchNode('zeno',this)">07 // ZENO</li>
      <li class="nav-group">// BIOLOGY</li>
      <li class="nav-item" onclick="switchNode('tvirus',this)">08 // THE t-VIRUS</li>
      <li class="nav-item" onclick="switchNode('rcs',this)">09 // RACCOON CITY SYNDROME</li>
      <li class="nav-item" onclick="switchNode('marie',this)">10 // SPECIMEN: MARIE</li>
      <li class="nav-item" onclick="switchNode('elpis',this)">11 // PROJECT ELPIS</li>
      <li class="nav-group">// DOCTRINE</li>
      <li class="nav-item" onclick="switchNode('sterilization',this)">12 // STERILIZATION PROTOCOL</li>
    </ul>
    <div class="user-card">
      OPERATOR: <b id="who">BRAYLIN</b><br>
      CLEARANCE: <span class="lvl">LEVEL 7 (OMEGA)</span><br>
      SESSION: SPO-7741-&#916;
    </div>
  </div>

  <div class="main-content">

    <!-- 01 OVERVIEW -->
    <div id="node-overview" class="node-container active">
      <div class="eyebrow">FILE PV-001 // TOP SECRET // NOFORN</div>
      <h1>CASE OVERVIEW — THE REQUIEM INCIDENT</h1>
      <div class="alert-box"><h4>TOP SECRET // EXTREME BIOLOGICAL HAZARD</h4>
        Viewing this file outside a Level 7 secure facility is a violation of Executive Order 12-BRAVO. The Umbrella Corporation accepts no liability for personnel exposure beyond this point.</div>
      <p>This archive documents the events surrounding the rediscovery of the Raccoon City ruins, the Rhodes Hill Chronic Care Center, and the underground <strong>ARK</strong> facility &mdash; collectively designated the <strong>Requiem Incident</strong>. It centers on FBI agent Grace Ashcroft, DSO veteran Leon S. Kennedy, ex-Umbrella researcher Dr. Victor Gideon, and the resurfacing of founder Oswell E. Spencer's final project, <strong>Elpis</strong>.</p>
      <table class="system-table">
        <tr><th>Parameter</th><th>Registration Data</th></tr>
        <tr><td>CASE NAME</td><td>REQUIEM (mainline entry IX)</td></tr>
        <tr><td>PRIMARY SITES</td><td>Raccoon City ruins &middot; Rhodes Hill Care Center &middot; ARK facility</td></tr>
        <tr><td>CENTRAL PATHOGEN</td><td>t-Virus &mdash; dormant strain ("Raccoon City Syndrome")</td></tr>
        <tr><td>KEY ASSET</td><td>Project ELPIS &mdash; universal antiviral</td></tr>
        <tr><td>OUTCOME</td><td><span class="redacted">ARK destroyed; Elpis recovered; subjects extracted by Hound Wolf Squad</span></td></tr>
      </table>
      <div class="hex-block" data-lbl="DECRYPTED FRAGMENT // SPENCER ARCHIVE">"I regret everything I have built. The viruses, the corporation, the gods I tried to make of men. Let the last thing I create be a cure, not a weapon. She is not a key. She is my hope." &mdash; recovered statement, O. E. Spencer.</div>
      <span class="stamp">CLASSIFIED</span>
    </div>

    <!-- 02 SYNOPSIS -->
    <div id="node-synopsis" class="node-container">
      <div class="eyebrow">SECTION 02 // SEQUENCE OF EVENTS</div>
      <h1>REQUIEM — FULL SYNOPSIS</h1>
      <div class="alert-box blue"><h4>SPOILER ADVISORY</h4>This section documents the complete story of Resident Evil Requiem, including its ending.</div>
      <h3>ACT I — THE ASSIGNMENT</h3>
      <p>FBI analyst Grace Ashcroft &mdash; daughter of Raccoon City survivor Alyssa Ashcroft (of the Outbreak files) &mdash; is assigned a case tied to former Raccoon City survivors who are dying while developing a sickness linked to a dormant T-Virus. Her investigation leads her to a shuttered hotel and, ultimately, to abduction by Dr. Victor Gideon, who imprisons her in the Rhodes Hill Chronic Care Center.</p>
      <h3>ACT II — RHODES HILL</h3>
      <p>Trapped in the labyrinthine care center, Grace navigates locked wards crawling with infected. She encounters Jeff Gray, a dying staffer who warns her to stay away from Emily &mdash; a young blind girl sealed behind glass. Emily's former cellmate, <strong>Marie</strong>, escaped through a hole dug in her cell and mutated into the towering, light-fearing creature now stalking the wing. Exploiting Marie's photosensitivity, Grace survives, frees Emily, and presses deeper. Gideon reveals Grace is the "key" to unlocking Elpis, chosen because of who "her master" made her to be.</p>
      <h3>ACT III — LEON'S HUNT</h3>
      <p>In parallel chapters, DSO agent Leon S. Kennedy &mdash; now in his late forties and afflicted with late-stage Raccoon City Syndrome (a dormant T-Virus infection marked by black bruising) &mdash; pursues Gideon. The two protagonists' paths converge as Leon enters the facility seeking both Gideon and a cure for himself.</p>
      <h3>ACT IV — EMILY'S FATE</h3>
      <p>At the climax of the Rhodes Hill section, the dormant T-Virus within Emily activates and she mutates out of control. Despite Grace's pleas, Leon is forced to put the mutated Emily down. Grief-stricken, Grace is drawn onward toward the ARK.</p>
      <h3>ACT V — THE ARK &amp; ELPIS</h3>
      <p>Beneath the Raccoon City ruins lies the <strong>ARK</strong>, a hidden Umbrella lab. There, Grace and Leon confront <strong>Zeno</strong>, an imitation of Albert Wesker, who believes Grace is the key to a new bioweapon. Recalling her mother's recorded interview with Spencer &mdash; in which Spencer expressed regret and called Grace his "hope" &mdash; Grace enters the password <strong>"HOPE"</strong> at the central console, unlocking vials of Spencer's final project, Elpis. It is revealed to be not a weapon but a universal antiviral.</p>
      <p>Crucial revelation: Grace is <strong>not</strong> Alyssa's biological daughter, nor a bioweapon key. She is an orphan Spencer took in; Alyssa later became her guardian. The Connections and elements of the U.S. government had used Spencer as a scapegoat.</p>
      <div class="dossier-photo"><div class="ph">[ PASTE SCREENSHOT: ARK CENTRAL CONSOLE / "HOPE" PROMPT ]</div>FIG. R-05 &mdash; ELPIS VAULT UNLOCK SEQUENCE</div>
    </div>

    <!-- 03 ENDINGS -->
    <div id="node-endings" class="node-container">
      <div class="eyebrow">SECTION 03 // OUTCOME MATRIX</div>
      <h1>BRANCHING ENDINGS</h1>
      <p>Requiem breaks from series tradition with two endings, decided by a single choice at the ARK console: <strong>Release Elpis</strong> or <strong>Destroy Elpis</strong>.</p>
      <h3>TRUE ENDING — "RELEASE ELPIS"</h3>
      <p>Grace injects Leon with Elpis, curing his Raccoon City Syndrome; the cure even reverses the damage, visibly de-aging him (echoing Jill Valentine's recovery). Gideon, having injected himself with the <strong>NE-&gamma; Type parasite</strong>, decapitates Zeno and mutates. Restored Leon duels Gideon, disarms him, and finishes him with his own rocket launcher; Gideon's final phase becomes a Nemesis-like monstrosity before Leon ends him for good. As the ARK collapses, Chris Redfield's Hound Wolf Squad extracts Grace and Leon. A vial is preserved to also cure Sherry Birkin, Emily can still be saved, and the ARK's discovery becomes international news.</p>
      <h3>ALTERNATE ENDING — "DESTROY ELPIS"</h3>
      <p>Elpis is destroyed and its antiviral nature never revealed. The ARK is still destroyed, but Zeno explodes Leon's head; Zeno himself dies as the collapsing platform gives way. This is the non-canon ending.</p>
      <table class="system-table">
        <tr><th>Variable</th><th>True Ending</th><th>Alternate</th></tr>
        <tr><td>ELPIS</td><td>Released &mdash; revealed as antiviral</td><td>Destroyed</td></tr>
        <tr><td>LEON</td><td>Cured, de-aged, survives</td><td>Killed by Zeno</td></tr>
        <tr><td>GIDEON</td><td>Mutates, defeated by Leon</td><td>Does not appear</td></tr>
        <tr><td>RESCUE</td><td>Hound Wolf Squad</td><td>None</td></tr>
        <tr><td>CANON</td><td>Yes</td><td>No</td></tr>
      </table>
    </div>

    <!-- 04 GRACE -->
    <div id="node-grace" class="node-container">
      <div class="eyebrow">PERSONNEL FILE 04</div>
      <h1>GRACE ASHCROFT</h1>
      <table class="system-table">
        <tr><td>ROLE</td><td>FBI agent / analyst; co-protagonist of Requiem</td></tr>
        <tr><td>AGE</td><td>~25 (as of 2026); joined the Bureau July 2025</td></tr>
        <tr><td>GUARDIAN</td><td>Alyssa Ashcroft (Raccoon City survivor, Outbreak)</td></tr>
        <tr><td>TRUE ORIGIN</td><td><span class="redacted">Orphan taken in by Oswell Spencer; not a bioweapon key</span></td></tr>
      </table>
      <p>Grace is an anxious, inexperienced agent whose arc moves from being perpetually on the brink of panic to becoming self-assured enough to fight back. Her predominantly stealth-based sections form the most fear-inducing parts of the game. She carries personal guilt throughout and ultimately finds redemption while uncovering the truth of her own origin.</p>
      <div class="dossier-photo"><div class="ph">[ PASTE SCREENSHOT: GRACE ASHCROFT ]</div>SUBJECT: G. ASHCROFT</div>
    </div>

    <!-- 05 LEON -->
    <div id="node-leon" class="node-container">
      <div class="eyebrow">PERSONNEL FILE 05</div>
      <h1>LEON S. KENNEDY</h1>
      <table class="system-table">
        <tr><td>ROLE</td><td>DSO agent; series veteran; co-protagonist</td></tr>
        <tr><td>AGE</td><td>Late forties</td></tr>
        <tr><td>CONDITION</td><td>Late-stage Raccoon City Syndrome (dormant T-Virus)</td></tr>
        <tr><td>SIGNATURE WEAPON</td><td>Magnum "Requiem"; rocket launcher (finale)</td></tr>
      </table>
      <p>Once the rookie of Raccoon City, Leon arrives at Rhodes Hill battling a dormant T-Virus infection evidenced by strange bruising. He pursues Victor Gideon and seeks a cure. In the true ending he is cured by Elpis and de-aged, leaving Raccoon City with memories of victory rather than regret &mdash; and the door open for future reunions (Ada Wong, a teased Chris team-up).</p>
    </div>

    <!-- 06 GIDEON -->
    <div id="node-gideon" class="node-container">
      <div class="eyebrow">PERSONNEL FILE 06 // ANTAGONIST</div>
      <h1>DR. VICTOR GIDEON</h1>
      <table class="system-table">
        <tr><td>ROLE</td><td>Primary antagonist; former Umbrella researcher</td></tr>
        <tr><td>APPEARANCE</td><td>Disfigured face, greasy goggles &mdash; "steampunk" menace</td></tr>
        <tr><td>AGENDA</td><td>Carry out a twisted version of Spencer's vision via Elpis</td></tr>
        <tr><td>MUTATION</td><td><span class="redacted">NE-&gamma; Type parasite &rarr; Nemesis-like final form</span></td></tr>
      </table>
      <p>Gideon kidnaps Grace believing her the "key" to Elpis. He too bears Umbrella's Curse (the dormant T-Virus). At the ARK he murders Zeno, declaring he will use Elpis to sow global anarchy &mdash; where Zeno merely wanted a weapon. After injecting the NE-&gamma; parasite he mutates through multiple phases, culminating in a Nemesis-form boss fight before Leon kills him.</p>
    </div>

    <!-- 07 ZENO -->
    <div id="node-zeno" class="node-container">
      <div class="eyebrow">PERSONNEL FILE 07 // ANTAGONIST</div>
      <h1>ZENO</h1>
      <table class="system-table">
        <tr><td>ROLE</td><td>Secondary antagonist; imitation of Albert Wesker</td></tr>
        <tr><td>AFFILIATION</td><td>The Connections</td></tr>
        <tr><td>GOAL</td><td>Weaponize Grace / Elpis as a bioweapon</td></tr>
        <tr><td>FATE</td><td>Decapitated by a mutated Gideon (true ending)</td></tr>
      </table>
      <p>Zeno awaits Grace and Leon at the ARK's central console, convinced Grace is the key to a new bioweapon. His infection is neutralized when Grace reveals the truth of Spencer's regret and her own ordinary origin. He is ultimately killed by Gideon, who saw his ambitions as too small.</p>
    </div>

    <!-- 08 T-VIRUS -->
    <div id="node-tvirus" class="node-container">
      <div class="eyebrow">SECTION 08 // CORE PATHOGEN</div>
      <h1>THE t-VIRUS</h1>
      <p>The t-Virus (Tyrant Virion) is an engineered RNA virus derived from the natural <strong>Progenitor Virus</strong>, discovered in 1966 in the roots of the West African Stairway of the Sun plant. Dr. James Marcus stabilized it by splicing in <span class="redacted">leech genetic material</span>, producing an agent that reanimates and repurposes hosts rather than simply killing them.</p>
      <h3>MECHANISM</h3>
      <p>The virion binds host cells via the gp-T glycoprotein spike, injects its genome, halts normal metabolism, and dissolves the neocortex while preserving the brainstem &mdash; producing the relentless feeding drive of the "zombie." In genetically compatible hosts it instead drives explosive mutation into Bio-Organic Weapons.</p>
      <table class="system-table">
        <tr><th>Phase</th><th>Host Activity</th></tr>
        <tr><td>0&ndash;2 HRS</td><td>Membrane seeding, localized necrosis, severe itching.</td></tr>
        <tr><td>2&ndash;24 HRS</td><td>Febrile collapse, hemorrhaging, neocortex dissolves.</td></tr>
        <tr><td>24&ndash;48 HRS</td><td>Clinical death &rarr; somatic reanimation (brainstem only).</td></tr>
        <tr><td>V-ACT</td><td>Under trauma: remodeling into Crimson Heads.</td></tr>
      </table>
      <div class="hex-block" data-lbl="HEX DUMP // t-VIRUS RNA SEQUENCE">0000  41 55 47 47 43 55 41 41 ... PROGENITOR FRAME
0010  67 70 2D 54 20 53 50 49 ... gp-T SPIKE ONLINE
0020  52 4E 41 20 50 4F 4C 59 ... REPLICATION ++
0030  48 4F 53 54 3A 20 4E 55 ... HOST IDENTITY: NULL</div>
    </div>

    <!-- 09 RCS -->
    <div id="node-rcs" class="node-container">
      <div class="eyebrow">SECTION 09 // DORMANT STRAIN</div>
      <h1>RACCOON CITY SYNDROME</h1>
      <p>A delayed, dormant form of T-Virus infection observed in long-term Raccoon City survivors. The virus lies latent for decades before reactivating, producing a progressive sickness marked by characteristic black bruising across the skin &mdash; colloquially "Umbrella's Curse."</p>
      <table class="system-table">
        <tr><td>NATURE</td><td>Dormant/latent T-Virus reactivation</td></tr>
        <tr><td>MARKER</td><td>Spreading black bruising</td></tr>
        <tr><td>NOTABLE CARRIERS</td><td>Leon S. Kennedy; Victor Gideon; Raccoon survivors</td></tr>
        <tr><td>TERMINAL EVENT</td><td>Uncontrolled mutation (e.g., Subject Emily)</td></tr>
        <tr><td>CURE</td><td>Project ELPIS (see Section 11)</td></tr>
      </table>
      <p class="disclaimer" style="text-align:left;color:#777;">Note: This is a sensitive topic in-fiction only. Raccoon City Syndrome is a fictional disease within the Resident Evil universe.</p>
    </div>

    <!-- 10 MARIE -->
    <div id="node-marie" class="node-container">
      <div class="eyebrow">SECTION 10 // RHODES HILL SPECIMEN</div>
      <h1>SPECIMEN: MARIE ("THE GIRL")</h1>
      <table class="system-table">
        <tr><td>FIELD NAME</td><td>"The Girl"</td></tr>
        <tr><td>TRUE IDENTITY</td><td>MARIE &mdash; failed clone, Emily's former cellmate</td></tr>
        <tr><td>CREATOR</td><td>Dr. Victor Gideon</td></tr>
        <tr><td>STRAIN</td><td>High-instability mutated t-Virus</td></tr>
        <tr><td>STATUS</td><td><span class="redacted">NEUTRALIZED &mdash; DISSOLVED BY DAYLIGHT</span></td></tr>
      </table>
      <p>Originally a small, pale, white-haired girl resembling Emily, Marie was injected with a mutated t-Virus strain and transformed into a feral creature exceeding ten feet, with decayed skin, an enlarged cranium, and elongated limbs adapted for climbing and burrowing. She escaped her glass cell through a dug hole into the basement and stalked Grace through the locked wing, killing and consuming the staff sent to recapture her.</p>
      <h3>VULNERABILITY: PHOTOSENSITIVITY</h3>
      <p>Marie's defining weakness is a lethal sensitivity to light; intense light or UV causes her flesh to burn and melt. She recoils at the threshold of lit rooms.</p>
      <div class="hex-block" data-lbl="BEHAVIORAL THREAT FLAG">SUBJECT IS ADAPTIVE. If prey hides in a static lit safe-room, MARIE climbs into the ceiling, bypasses line-of-sight through the rafters, and tears out the overhead wiring &mdash; plunging the sector into darkness. A lit room is NOT permanently safe.</div>
      <h3>NEUTRALIZATION</h3>
      <p>Leon's magnum only briefly incapacitated her. Grace ultimately neutralized Marie at the water treatment plant by manually opening a roof hatch and flooding the chamber with daylight, dissolving the specimen entirely.</p>
      <div class="dossier-photo"><div class="ph">[ PASTE SCREENSHOT: "THE GIRL" / MARIE ]</div>SPECIMEN: MARIE</div>
    </div>

    <!-- 11 ELPIS -->
    <div id="node-elpis" class="node-container">
      <div class="eyebrow">SECTION 11 // COUNTERMEASURE</div>
      <h1>PROJECT ELPIS</h1>
      <div class="alert-box amber"><h4>FICTION NOTICE</h4>Elpis is in-universe Resident Evil fiction. It is not a real medical treatment and cannot be used as one.</div>
      <p>Elpis (Greek: "hope") was Oswell Spencer's final project &mdash; secretly an <strong>antiviral agent designed to undo virus-based infections</strong> rather than the bioweapon both Zeno and Gideon assumed. It is unlocked at the ARK console via the password "HOPE," drawn from Spencer's recorded regret and his description of Grace as his hope.</p>
      <h3>DEMONSTRATED EFFECTS</h3>
      <table class="system-table">
        <tr><td>CURES</td><td>Raccoon City Syndrome / dormant T-Virus infection</td></tr>
        <tr><td>SECONDARY</td><td>Reverses cellular damage &mdash; visible de-aging (cf. Jill)</td></tr>
        <tr><td>CONFIRMED USE</td><td>Leon cured; vial preserved for Sherry Birkin</td></tr>
        <tr><td>SCOPE</td><td>Described as undoing all virus-based infections</td></tr>
      </table>
      <h4 class="sec">CONCEPTUAL MECHANISM (LORE)</h4>
      <p>Consistent with series lore, an effective antiviral must block viral entry (gp-T spike), halt RNA replication, restore the suppressed immune response, and protect neural tissue during clearance. Elpis represents the realization of this across viral families &mdash; the antithesis of the weapons program Umbrella was built upon.</p>
    </div>

    <!-- 12 STERILIZATION -->
    <div id="node-sterilization" class="node-container">
      <div class="eyebrow">SECTION 12 // CONTINGENCY DOCTRINE</div>
      <h1>STERILIZATION PROTOCOL OMEGA</h1>
      <p>Doctrine treats any confirmed t-Virus release as a Class OMEGA event requiring immediate perimeter establishment and, where containment fails, authorized sterilization of the affected zone. The Raccoon City precedent governs all response planning.</p>
      <table class="system-table">
        <tr><th>Step</th><th>Directive</th></tr>
        <tr><td>01: DETECT</td><td>Rapid gp-T antigen assay; confirm strain and dormancy status.</td></tr>
        <tr><td>02: CORDON</td><td>Seal blast doors and ventilation. Assume aerosol potential. No egress.</td></tr>
        <tr><td>03: SUPPRESS</td><td>High-lumen UV vs. photosensitive B.O.W.s; target craniums of standard infected.</td></tr>
        <tr><td>04: TRIAGE</td><td>Administer ELPIS antiviral to infected personnel where stocks allow.</td></tr>
        <tr><td>05: STERILIZE</td><td>If containment is lost, authorized zone sterilization per <span class="redacted">Executive Order 12-BRAVO</span>.</td></tr>
      </table>
      <span class="stamp">END OF FILE</span>
      <p class="disclaimer">Fan-made creative work set in the fictional Resident Evil universe (&copy; Capcom). Not affiliated with or endorsed by Capcom. No real pathogen, disease, or medical procedure is described in this document.</p>
    </div>

  </div>

  <!-- LIVE LOG -->
  <div class="live-log">
    <div class="log-header">TERMINAL DAEMON // ARK PORT 443</div>
    <div class="log-content" id="log-content"></div>
    <div class="threat-meter">
      <div style="color:var(--amber);font-size:8pt;letter-spacing:1px;">ARK CONTAINMENT INTEGRITY</div>
      <div class="bar"><div class="fill"></div></div>
    </div>
  </div>
</div>

<script>
const VALID_USER="BRAYLIN", VALID_KEY="72382006";

// embers
(function(){ const e=document.getElementById('embers'); if(!e) return;
  for(let i=0;i<26;i++){ const d=document.createElement('div'); d.className='ember';
    d.style.left=(Math.random()*100)+'%'; d.style.animationDuration=(6+Math.random()*8)+'s';
    d.style.animationDelay=(Math.random()*8)+'s'; d.style.opacity=0.3+Math.random()*0.6;
    e.appendChild(d);} })();

const bootText=[
  {t:"Umbrella Corp. BIOS v7.7.41 ... ",c:"ok",s:"OK"},
  {t:"Loading secure kernel ... ",c:"ok",s:"OK"},
  {t:"Mounting encrypted ARK archive ... ",c:"ok",s:"OK"},
  {t:"Decrypting case file: REQUIEM ... ",c:"ok",s:"OK"},
  {t:"Hazard Class OMEGA telemetry ... ",c:"wn",s:"NOMINAL"},
  {t:"Scanning sub-levels for bio-signatures ... ",c:"wn",s:"4 ANOMALIES"},
  {t:"Cross-referencing Raccoon City Syndrome registry ... ",c:"ok",s:"DONE"},
  {t:"AI threat-assessment matrix ... ",c:"ok",s:"ONLINE"},
  {t:"Uplink to UC-NET ARK Node ... ",c:"ok",s:"SECURED"},
  {t:"Rendering Level 7 gateway ...",c:"",s:""}
];
window.onload=()=>{
  const boot=document.getElementById('boot-sequence'); let delay=0;
  bootText.forEach(line=>{ setTimeout(()=>{ const p=document.createElement('div'); p.className='boot-line';
    p.innerHTML = line.s ? `${line.t}<span class="${line.c}">${line.s}</span>` : line.t; boot.appendChild(p); },delay);
    delay += (Math.random()*240)+130; });
  setTimeout(()=>{ boot.style.display='none'; document.getElementById('login-screen').style.display='flex';
    document.getElementById('password').focus(); }, delay+700);
};
function validateLogin(){
  const u=document.getElementById('username').value.trim().toUpperCase();
  const k=document.getElementById('password').value.trim();
  const err=document.getElementById('error-msg');
  if(u===VALID_USER && k===VALID_KEY){
    err.style.color="var(--green-glow)"; err.innerText="ACCESS GRANTED. DECRYPTING CASE FILE...";
    document.getElementById('who').innerText=VALID_USER;
    setTimeout(()=>{ document.getElementById('login-screen').style.display='none';
      document.getElementById('main-system').style.display='grid'; startLiveLog(); },1400);
  } else { err.style.color="var(--bright-red)"; err.innerText="ACCESS DENIED. LETHAL COUNTERMEASURES ARMED.";
    document.getElementById('password').value=""; }
}
function switchNode(id,el){
  document.querySelectorAll('.nav-item').forEach(i=>i.classList.remove('active'));
  document.querySelectorAll('.node-container').forEach(n=>n.classList.remove('active'));
  el.classList.add('active'); document.getElementById('node-'+id).classList.add('active');
  document.querySelector('.main-content').scrollTop=0; addLog("Module access: "+id.toUpperCase(),"success");
}
const logContent=document.getElementById('log-content');
const randomLogs=[
  {t:"[SYS] Re-routing ARK thermal exhaust.",k:"normal"},
  {t:"[SEC] Motion detected, Rhodes Hill wing.",k:"err"},
  {t:"[WARN] Bio-matter in ventilation shaft B.",k:"warn"},
  {t:"[SYS] AI matrix analyzing mutation vectors...",k:"normal"},
  {t:"[SEC] Bulkhead C3 lockdown engaged.",k:"err"},
  {t:"[BIO] gp-T antigen assay POSITIVE.",k:"err"},
  {t:"[SYS] ELPIS vault: vials accounted for.",k:"warn"},
  {t:"[SEC] UV emitter array armed.",k:"normal"},
  {t:"[NET] Hound Wolf Squad beacon detected.",k:"normal"},
  {t:"[WARN] Subject MARIE signature lost in dark zone.",k:"warn"}
];
function startLiveLog(){ addLog("Terminal access granted to "+VALID_USER+".","success");
  addLog("Case file REQUIEM decrypted.","success"); addLog("Clearance LEVEL 7 (OMEGA) verified.","success");
  setInterval(()=>{ if(Math.random()>0.55){ const l=randomLogs[Math.floor(Math.random()*randomLogs.length)]; addLog(l.t,l.k); } },2400); }
</script>
</body>
</html>
