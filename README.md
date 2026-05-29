<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>UC-NET // LEVEL 7 TERMINAL OVERRIDE</title>
<style>
:root{
  --primary-red:#cc0000; --bright-red:#ff3333; --dark-red:#800000; --blood:#5a0000;
  --bg-black:#040404; --terminal-gray:#101012; --text-light:#e0e0e0;
  --green-glow:#33ff66; --hud-blue:#2aa9ff; --amber:#ffaa00;
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
.vignette{position:fixed;inset:0;z-index:9997;pointer-events:none;
  box-shadow:inset 0 0 220px rgba(0,0,0,0.95);}

/* ---- BOOT ---- */
#boot-sequence{position:absolute;inset:0;background:#000;color:var(--text-light);
  padding:26px;font-size:11pt;display:flex;flex-direction:column;z-index:1000;overflow:hidden;}
.boot-line{margin-bottom:3px;opacity:0;animation:showLine 0.1s forwards;white-space:pre-wrap;}
.boot-line .ok{color:var(--green-glow);} .boot-line .wn{color:var(--amber);} .boot-line .er{color:var(--bright-red);}
@keyframes showLine{to{opacity:1}}

/* ---- LOGIN ---- */
#login-screen{display:none;flex-direction:column;justify-content:center;align-items:center;height:100%;
  background:radial-gradient(circle,#1a0505 0%,#000 75%);z-index:500;}
.umbrella-logo{width:140px;height:140px;margin-bottom:26px;
  filter:drop-shadow(0 0 14px rgba(204,0,0,0.85));animation:glitch 3.4s infinite linear alternate-reverse;}
@keyframes glitch{0%{transform:translate(0)}20%{transform:translate(-2px,2px)}40%{transform:translate(-2px,-2px)}
  60%{transform:translate(2px,2px)}80%{transform:translate(2px,-2px)}100%{transform:translate(0)}}
.login-box{background:rgba(10,10,10,0.88);border:1px solid var(--primary-red);padding:40px;width:470px;
  box-shadow:inset 0 0 22px rgba(204,0,0,0.2),0 0 34px rgba(204,0,0,0.45);position:relative;}
.login-box::before{content:'';position:absolute;top:0;left:0;width:100%;height:2px;
  background:var(--primary-red);box-shadow:0 0 10px var(--bright-red);}
h2{color:#fff;margin:0 0 5px 0;font-size:15pt;letter-spacing:3px;text-transform:uppercase;
  border-bottom:1px solid #333;padding-bottom:10px;}
.sub-h{font-size:8pt;color:#777;letter-spacing:2px;margin-bottom:6px;}
.input-group{margin-top:22px;}
.input-group label{display:block;font-size:9pt;color:var(--primary-red);margin-bottom:8px;font-weight:bold;letter-spacing:1px;}
.input-group input{width:100%;background:#000;border:1px solid #444;color:#fff;padding:12px;
  font-family:monospace;font-size:12pt;transition:all 0.3s;letter-spacing:2px;}
.input-group input:focus{border-color:var(--bright-red);outline:none;box-shadow:0 0 8px rgba(255,51,51,0.5);}
.btn-submit{margin-top:28px;background:transparent;color:var(--primary-red);border:1px solid var(--primary-red);
  padding:15px;width:100%;cursor:pointer;font-family:monospace;font-weight:bold;font-size:11pt;letter-spacing:2px;transition:all 0.2s;}
.btn-submit:hover{background:var(--primary-red);color:#fff;box-shadow:0 0 16px var(--primary-red);}
.hint{font-size:8pt;color:#444;margin-top:14px;text-align:center;letter-spacing:1px;}

/* ---- DASHBOARD ---- */
#main-system{display:none;height:100%;grid-template-columns:300px 1fr 320px;}
.sidebar{background:var(--terminal-gray);border-right:1px solid #333;display:flex;flex-direction:column;}
.sidebar-header{padding:18px;border-bottom:1px solid var(--primary-red);text-align:center;background:rgba(204,0,0,0.1);}
.sidebar-header .logo-mini{width:54px;height:54px;margin:0 auto 8px;display:block;filter:drop-shadow(0 0 6px rgba(204,0,0,0.7));}
.sidebar-header h3{margin:4px 0;font-size:12pt;color:#fff;letter-spacing:2px;}
.nav-menu{list-style:none;padding:0;margin:0;flex-grow:1;overflow-y:auto;}
.nav-item{padding:14px 20px;cursor:pointer;border-bottom:1px solid #1c1c1c;border-left:3px solid transparent;
  font-size:9.5pt;color:#888;transition:all 0.2s;letter-spacing:1px;}
.nav-item:hover{background:#1a1a1a;color:#ddd;}
.nav-item.active{background:#2a0a0a;border-left-color:var(--bright-red);color:#fff;font-weight:bold;
  box-shadow:inset 22px 0 22px -22px var(--primary-red);}
.user-card{padding:14px 18px;border-top:1px solid #333;font-size:8.5pt;color:#888;background:#0b0b0b;}
.user-card b{color:var(--green-glow);}
.user-card .lvl{color:var(--bright-red);font-weight:bold;}

.main-content{padding:42px 48px;overflow-y:auto;position:relative;
  background:linear-gradient(180deg,#0a0a0a 0%,#141414 100%);}
.main-content::-webkit-scrollbar{width:8px;} .main-content::-webkit-scrollbar-thumb{background:var(--dark-red);}
.node-container{display:none;animation:fadeIn 0.4s;}
.node-container.active{display:block;}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}

h1{color:#fff;font-size:22pt;border-bottom:2px solid var(--primary-red);padding-bottom:10px;margin-top:0;letter-spacing:1px;}
.eyebrow{color:var(--primary-red);font-size:8.5pt;letter-spacing:4px;margin-bottom:-6px;}
h3{color:var(--primary-red);margin-top:30px;font-size:13pt;letter-spacing:1px;}
h4.sec{color:var(--hud-blue);margin-top:22px;font-size:11pt;letter-spacing:1px;}
p{line-height:1.65;font-size:10.5pt;color:#cfcfcf;margin-bottom:15px;text-align:justify;}
.redacted{background:#222;color:#222;cursor:crosshair;transition:all 0.25s;padding:0 4px;border-radius:2px;}
.redacted:hover{background:transparent;color:var(--bright-red);text-shadow:0 0 6px var(--bright-red);}

.system-table{width:100%;border-collapse:collapse;margin:24px 0;border:1px solid #333;}
.system-table th,.system-table td{border:1px solid #2c2c2c;padding:11px 12px;font-size:9.5pt;line-height:1.45;}
.system-table th{background:#111;color:var(--primary-red);text-transform:uppercase;letter-spacing:1px;}
.system-table td:first-child{color:#fff;font-weight:bold;white-space:nowrap;}
.system-table tr:nth-child(even){background:rgba(255,0,0,0.025);}

.hex-block{background:#000;border:1px solid #333;padding:22px;font-family:monospace;font-size:9pt;color:#6a6a6a;
  margin:22px 0;position:relative;line-height:1.5;white-space:pre-wrap;}
.hex-block.lbl-rna::before{content:'HEX DUMP // t-VIRUS RNA SEQUENCE';}
.hex-block.lbl-note::before{content:'DECRYPTED FRAGMENT // INTERNAL MEMO';}
.hex-block.lbl-warn::before{content:'BEHAVIORAL THREAT FLAG';color:var(--amber)!important;}
.hex-block::before{position:absolute;top:-10px;left:15px;background:#000;padding:0 10px;
  color:var(--primary-red);font-size:8pt;font-weight:bold;letter-spacing:1px;}

.alert-box{border:1px solid var(--bright-red);background:rgba(204,0,0,0.1);padding:15px 18px;margin:22px 0;border-left:5px solid var(--bright-red);}
.alert-box h4{margin:0 0 8px 0;color:#fff;font-size:11pt;display:flex;align-items:center;letter-spacing:1px;}
.alert-box h4::before{content:'\26A0';margin-right:10px;color:var(--bright-red);font-size:14pt;}
.alert-box.amber{border-color:var(--amber);background:rgba(255,170,0,0.08);border-left-color:var(--amber);}
.alert-box.amber h4::before{color:var(--amber);}

.stamp{display:inline-block;border:3px solid var(--bright-red);color:var(--bright-red);padding:6px 14px;
  font-weight:bold;letter-spacing:3px;transform:rotate(-5deg);opacity:0.85;font-size:13pt;margin:10px 0;
  text-shadow:0 0 6px rgba(255,51,51,0.4);}

/* ---- LIVE LOG ---- */
.live-log{background:#070707;border-left:1px solid #222;display:flex;flex-direction:column;font-size:8.5pt;}
.log-header{padding:14px;border-bottom:1px solid #222;color:var(--green-glow);font-weight:bold;background:#101010;text-align:center;letter-spacing:1px;}
.log-content{padding:14px;flex-grow:1;overflow-y:hidden;}
.log-line{margin-bottom:6px;color:#5a5a5a;word-wrap:break-word;line-height:1.4;}
.log-line .timestamp{color:var(--hud-blue);margin-right:5px;}
.log-line .success{color:var(--green-glow);} .log-line .warn{color:var(--amber);} .log-line .err{color:var(--bright-red);}
.threat-meter{padding:12px 14px;border-top:1px solid #222;background:#0a0a0a;}
.threat-meter .bar{height:8px;background:#1a1a1a;border:1px solid #333;margin-top:6px;overflow:hidden;}
.threat-meter .fill{height:100%;width:78%;background:linear-gradient(90deg,var(--amber),var(--bright-red));
  animation:pulseBar 2.5s infinite;}
@keyframes pulseBar{0%,100%{width:72%}50%{width:84%}}
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
  <svg class="umbrella-logo" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
    <polygon points="30,4 70,4 96,30 96,70 70,96 30,96 4,70 4,30" fill="#fff" stroke="#cc0000" stroke-width="2.5"/>
    <polygon points="50,50 30,4 70,4" fill="#cc0000"/>
    <polygon points="50,50 96,30 96,70" fill="#cc0000"/>
    <polygon points="50,50 70,96 30,96" fill="#cc0000"/>
    <polygon points="50,50 4,70 4,30" fill="#cc0000"/>
    <rect x="46" y="6" width="8" height="88" fill="#fff"/>
    <rect x="6" y="46" width="88" height="8" fill="#fff"/>
  </svg>
  <div class="login-box">
    <div class="sub-h">UMBRELLA CORPORATION &nbsp;//&nbsp; BIOHAZARD COUNTERMEASURE DIVISION</div>
    <h2>U.C. Mainframe — Secure Gateway</h2>
    <div class="input-group">
      <label>OPERATOR ID</label>
      <input type="text" id="username" value="BRAYLIN" autocomplete="off">
    </div>
    <div class="input-group">
      <label>DECRYPTION OVERRIDE KEY</label>
      <input type="password" id="password" placeholder="ENTER ACCESS KEY..." autocomplete="off"
        onkeydown="if(event.key==='Enter') validateLogin()">
    </div>
    <button class="btn-submit" onclick="validateLogin()">AUTHENTICATE</button>
    <div id="error-msg" style="color:var(--bright-red);font-size:10pt;margin-top:15px;text-align:center;min-height:15px;letter-spacing:1px;"></div>
    <div class="hint">CLEARANCE LEVEL 7 (OMEGA) REQUIRED &nbsp;//&nbsp; ALL ATTEMPTS LOGGED</div>
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
      <h3>UC-NET // NODE 097</h3>
      <div style="font-size:8pt;color:var(--green-glow);">&#9679; LIVE SECURE CONNECTION</div>
    </div>
    <ul class="nav-menu">
      <li class="nav-item active" onclick="switchNode('overview',this)">01 // PROJECT OVERVIEW</li>
      <li class="nav-item" onclick="switchNode('origin',this)">02 // ORIGIN &amp; HISTORY</li>
      <li class="nav-item" onclick="switchNode('pathogenesis',this)">03 // VIRAL PATHOGENESIS</li>
      <li class="nav-item" onclick="switchNode('bow',this)">04 // B.O.W. CATALOG</li>
      <li class="nav-item" onclick="switchNode('marie',this)">05 // SPECIMEN: MARIE</li>
      <li class="nav-item" onclick="switchNode('daylight',this)">06 // DAYLIGHT VACCINE</li>
      <li class="nav-item" onclick="switchNode('sterilization',this)">07 // STERILIZATION PROTOCOL</li>
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
      <h1>PROJECT DOSSIER: THE t-VIRUS</h1>
      <div class="alert-box">
        <h4>TOP SECRET // EXTREME BIOLOGICAL HAZARD</h4>
        Viewing this file outside a Level 7 secure facility is a violation of Executive Order 12-BRAVO. The Umbrella Corporation accepts no liability for personnel exposure beyond this point.
      </div>
      <table class="system-table">
        <tr><th>Parameter</th><th>Registration Data</th></tr>
        <tr><td>ASSET DESIGNATION</td><td>t-Virus (Tyrant Virion) &mdash; Progenitor Lineage</td></tr>
        <tr><td>BIOSAFETY CLASS</td><td>BSL-4 // Umbrella Hazard Class OMEGA</td></tr>
        <tr><td>ORIGINAL ARCHITECT</td><td>Dr. James Marcus &mdash; <span class="redacted">terminated by internal order</span></td></tr>
        <tr><td>CUSTODY</td><td>Umbrella Corp. &mdash; reviewed by Operator BRAYLIN</td></tr>
        <tr><td>FACILITY STATUS</td><td><span class="redacted">ARKLAY LABS COMPROMISED &mdash; PURGED</span></td></tr>
        <tr><td>FATALITY RATE</td><td>~100% without pre-symptomatic DAYLIGHT administration</td></tr>
      </table>
      <p>The t-Virus represents the apex of Umbrella Corporation's Bio-Organic Weapon (B.O.W.) development cycle. Synthesized by merging the foundational Progenitor Virus with <span class="redacted">leech genetic material harvested by Dr. James Marcus</span>, the agent bypasses standard necrotic pathways and forces catastrophic cellular resurrection &mdash; reanimating hosts that would otherwise die.</p>
      <div class="hex-block lbl-note">"Marcus was betrayed. The virus is his revenge. We sealed the labs, but the leeches remember. We are not the keepers of this thing. We are its first meal." &mdash; recovered audio log, Arklay Sub-Level 2.</div>
      <span class="stamp">CLASSIFIED</span>
    </div>

    <!-- 02 ORIGIN -->
    <div id="node-origin" class="node-container">
      <div class="eyebrow">SECTION 02 // LINEAGE</div>
      <h1>ORIGIN &amp; HISTORY</h1>
      <h3>THE PROGENITOR VIRUS</h3>
      <p>In 1966, an Umbrella founding expedition into West Africa discovered the Progenitor Virus residing in the roots of the Stairway of the Sun flowering plant. The Progenitor possessed a singular property: rather than simply killing a host, it could rewrite the host genome and dramatically enhance physical capability. In nearly all subjects, however, it was lethal.</p>
      <p>Founders Oswell E. Spencer, James Marcus, and Edward Ashford recognized the Progenitor as the seed of both an "evolved" humanity and an unprecedented class of weapon. The pharmaceutical company was, from its first day, a shell over a weapons program.</p>
      <h3>ENGINEERING THE t-VIRUS</h3>
      <p>Dr. James Marcus sought to make the Progenitor's power survivable and controllable. The breakthrough came when he spliced the Progenitor Virus with genetic material from <span class="redacted">leeches</span>. The resulting chimeric agent retained the genome-rewriting capability while gaining stability, transmissibility, and a far higher host-survival rate. This engineered agent was designated the t-Virus &mdash; where the Progenitor killed, the t-Virus reanimated and repurposed.</p>
      <table class="system-table">
        <tr><th>Year</th><th>Event</th></tr>
        <tr><td>1966</td><td>Progenitor Virus discovered in West Africa.</td></tr>
        <tr><td>1978</td><td>Marcus achieves first stable t-Virus via leech splice.</td></tr>
        <tr><td>1988</td><td><span class="redacted">Marcus assassinated on internal order; research seized.</span></td></tr>
        <tr><td>1998</td><td>Arklay containment failure &rarr; Raccoon City outbreak.</td></tr>
        <tr><td>&mdash;</td><td>Raccoon City sterilized; derivative strains proliferate.</td></tr>
      </table>
      <h3>CONTAINMENT FAILURE</h3>
      <p>Development centered on the Arklay Research Facility above Raccoon City. A combination of corporate sabotage, deliberate release of carriers, and laboratory negligence contaminated the entire facility. Infected staff and specimens escaped, the surrounding forest filled with infected wildlife, and the agent ultimately reached Raccoon City &mdash; culminating in the outbreak and the city's sterilization.</p>
      <div class="alert-box amber">
        <h4>OVERSIGHT ANNOTATION</h4>
        Post-incident review attributes the breach to inadequate compartmentalization between commercial and weapons divisions. Permanent monitoring of all surviving Progenitor-line cultures is mandated.
      </div>
    </div>

    <!-- 03 PATHOGENESIS -->
    <div id="node-pathogenesis" class="node-container">
      <div class="eyebrow">SECTION 03 // MECHANISM</div>
      <h1>VIRAL PATHOGENESIS &amp; MUTATION</h1>
      <p>On entry via fluid transmission, ingestion, or <span class="redacted">engineered aerosol deployment</span>, the virion binds host cells through the gp-T glycoprotein spike and injects its Progenitor-derived RNA genome. Cellular machinery is hijacked within 60 minutes; normal metabolism halts while gross tissue function is preserved and driven toward aggression and feeding.</p>
      <h3>CLINICAL DEGRADATION TIMELINE</h3>
      <table class="system-table">
        <tr><th>Phase</th><th>Host Symptoms &amp; Neural Activity</th></tr>
        <tr><td>0&ndash;2 HRS</td><td>Membrane seeding. Localized necrosis. Severe itching reported (<span class="redacted">"Itchy. Tasty."</span>).</td></tr>
        <tr><td>2&ndash;24 HRS</td><td>Febrile collapse. Temperature spikes; hemorrhaging; pain response lost; neocortex dissolves.</td></tr>
        <tr><td>24&ndash;48 HRS</td><td>Clinical death followed by somatic reanimation. Subject operates on brainstem alone (feeding drive).</td></tr>
        <tr><td>ACTIVE CARRIER</td><td>Relentless predatory behavior; bite transmits the agent to new hosts.</td></tr>
        <tr><td>V-ACT</td><td>Under severe trauma, viral RNA triggers remodeling into faster, durable Crimson Heads.</td></tr>
      </table>
      <h4 class="sec">THE MUTATION BRANCH (B.O.W. PATHWAY)</h4>
      <p>In hosts with compatible genetics, or after exposure to refined high-titer strains, the virus does not merely reanimate &mdash; it remodels. Bone, muscle, and connective tissue undergo explosive growth; limbs elongate; sensory organs distort. Outcome is governed by host genome, viral strain, dose, and environmental stress. These are the directed Bio-Organic Weapons.</p>
      <div class="hex-block lbl-rna">0000  41 55 47 47 43 55 41 41 47 47 43 43 55 41 47 ... PROGENITOR FRAME
0010  67 70 2D 54 20 53 50 49 4B 45 20 4F 4E 4C 49 ... gp-T SPIKE ONLINE
0020  52 4E 41 20 50 4F 4C 59 4D 45 52 41 53 45 20 ... REPLICATION ++
0030  48 4F 53 54 20 49 44 45 4E 54 49 54 59 3A 20 ... HOST IDENTITY: NULL</div>
    </div>

    <!-- 04 BOW -->
    <div id="node-bow" class="node-container">
      <div class="eyebrow">SECTION 04 // WEAPONIZED OUTCOMES</div>
      <h1>BIO-ORGANIC WEAPON CATALOG</h1>
      <p>Canonical t-Virus mutation outcomes documented across multiple containment incidents:</p>
      <table class="system-table">
        <tr><th>Designation</th><th>Profile</th></tr>
        <tr><td>ZOMBIE</td><td>Baseline infected host. Slow, relentless, infectious by bite. Mass byproduct, not a directed weapon.</td></tr>
        <tr><td>CERBERUS</td><td>Infected canines. Pack hunters retaining speed and coordination.</td></tr>
        <tr><td>LICKER</td><td>Advanced mutation. Exposed musculature and brain, elongated tongue; blind but acutely auditory.</td></tr>
        <tr><td>HUNTER</td><td>Engineered reptilian-human B.O.W. Bred for lethality; capable of decapitating strikes.</td></tr>
        <tr><td>TYRANT (T-series)</td><td>Apex directed B.O.W. Humanoid, immensely strong and durable; the program's namesake weapon.</td></tr>
        <tr><td>THE GIRL (MARIE)</td><td>Requiem-era specimen. See Section 05.</td></tr>
      </table>
      <div class="alert-box">
        <h4>HANDLING NOTE</h4>
        Directed B.O.W.s retain partial tactical intelligence. Do not assume reanimated hosts are mindless &mdash; advanced specimens stalk, ambush, and adapt.
      </div>
    </div>

    <!-- 05 MARIE -->
    <div id="node-marie" class="node-container">
      <div class="eyebrow">SECTION 05 // RHODES HILL INCIDENT</div>
      <h1>INCIDENT REPORT: SUBJECT MARIE</h1>
      <table class="system-table">
        <tr><td>FIELD NAME</td><td>"The Girl"</td></tr>
        <tr><td>TRUE IDENTITY</td><td>MARIE &mdash; failed clone of FBI agent Grace Ashcroft</td></tr>
        <tr><td>CREATOR</td><td>Dr. Victor Gideon</td></tr>
        <tr><td>LOCATION</td><td>Rhodes Hill Chronic Care Center</td></tr>
        <tr><td>STRAIN</td><td>High-instability mutated t-Virus variant</td></tr>
        <tr><td>STATUS</td><td><span class="redacted">NEUTRALIZED &mdash; DISSOLVED BY DAYLIGHT</span></td></tr>
      </table>
      <p>Originally indistinguishable from the child Emily &mdash; small, pale, white-haired, in a plain white dress &mdash; Marie was injected with a mutated strain of the t-Virus. The strain transformed her into a towering, long-limbed, feral creature exceeding ten feet in height, with decayed and torn skin, an enlarged cranium, and elongated hands and feet adapted for climbing and burrowing through facility bulkheads. She killed and consumed the staff sent to recapture her, then stalked Grace Ashcroft through the locked wing.</p>
      <h3>TACTICAL VULNERABILITY: PHOTOSENSITIVITY</h3>
      <p>As her mutation worsened, Marie developed an extreme, lethal sensitivity to light. Exposure to sufficiently intense light or UV radiation causes her flesh to burn and melt. Subjects evading her are advised to remain inside fully lit rooms; she recoils and retreats at the threshold of illuminated space. This is the single most reliable countermeasure against her.</p>
      <div class="hex-block lbl-warn">SUBJECT IS ADAPTIVE.
If prey relies on a static lit safe-room, MARIE will climb into the ceiling, bypass line-of-sight through the rafters, and tear out the overhead wiring &mdash; plunging the sector into darkness and forcing prey back into the open. A lit room is not permanently safe.</div>
      <h3>NEUTRALIZATION</h3>
      <p>An early intervention by DSO agent Leon S. Kennedy, using his magnum sidearm, only temporarily incapacitated her. Marie was ultimately neutralized at the facility's water treatment plant when Grace Ashcroft manually opened a roof hatch, flooding the chamber with intense daylight. The exposure caused the specimen to melt entirely, terminating the threat.</p>
    </div>

    <!-- 06 DAYLIGHT -->
    <div id="node-daylight" class="node-container">
      <div class="eyebrow">SECTION 06 // COUNTERMEASURE</div>
      <h1>THE DAYLIGHT VACCINE PROTOCOL</h1>
      <div class="alert-box amber">
        <h4>FICTION NOTICE</h4>
        DAYLIGHT is lore-flavored fiction consistent with the Resident Evil universe. It is not a real medical procedure and cannot be used as one.
      </div>
      <p>Because the t-Virus works by hijacking host RNA replication and suppressing immunity, an effective cure must simultaneously halt viral replication, flag infected cells for clearance, and restore the suppressed immune response before irreversible neural necrosis occurs.</p>
      <h3>COMPOSITION (CONCEPTUAL)</h3>
      <table class="system-table">
        <tr><th>Component</th><th>Function</th></tr>
        <tr><td>Progenitor antibody scaffold</td><td>Competitively binds the gp-T spike, blocking viral entry into healthy cells.</td></tr>
        <tr><td>RNA-replication inhibitor</td><td>Terminates transcription of the viral genome inside infected tissue.</td></tr>
        <tr><td>Immune-restoration factor</td><td>Reactivates the suppressed interferon response so the body clears flagged cells.</td></tr>
        <tr><td>Neuro-protective adjuvant</td><td>Slows neocortical necrosis to widen the treatment window.</td></tr>
      </table>
      <h3>ADMINISTRATION WINDOW</h3>
      <p>DAYLIGHT is viable only before clinical death and reanimation. Administered during the febrile-collapse window, it can arrest infection and permit partial neural recovery. Once a host fully reanimates as an active carrier, no recovery of identity has been documented; for B.O.W.-class mutations such as Subject MARIE, no vaccine is effective &mdash; only physical neutralization applies.</p>
    </div>

    <!-- 07 STERILIZATION -->
    <div id="node-sterilization" class="node-container">
      <div class="eyebrow">SECTION 07 // CONTINGENCY DOCTRINE</div>
      <h1>STERILIZATION PROTOCOL OMEGA</h1>
      <p>Joint doctrine treats any confirmed t-Virus release as a Class OMEGA event requiring immediate perimeter establishment and, where containment fails, authorized sterilization of the affected zone. The Raccoon City precedent governs all response planning.</p>
      <table class="system-table">
        <tr><th>Step</th><th>Directive</th></tr>
        <tr><td>01: DETECT</td><td>Rapid gp-T antigen assay on suspected carriers; confirm strain.</td></tr>
        <tr><td>02: CORDON</td><td>Lock all blast doors, seal ventilation. Assume aerosol potential. No egress.</td></tr>
        <tr><td>03: SUPPRESS</td><td>Deploy high-lumen UV against photosensitive B.O.W.s; target craniums of standard infected.</td></tr>
        <tr><td>04: TRIAGE</td><td>Administer experimental DAYLIGHT antigen to pre-symptomatic personnel only.</td></tr>
        <tr><td>05: STERILIZE</td><td>If containment is lost, initiate authorized zone sterilization per <span class="redacted">Executive Order 12-BRAVO</span>.</td></tr>
      </table>
      <span class="stamp">END OF FILE</span>
      <p class="disclaimer">Fan-made creative work set in the fictional Resident Evil universe (&copy; Capcom). Not affiliated with Capcom. No real pathogen or procedure is described.</p>
    </div>
  </div>

  <!-- LIVE LOG -->
  <div class="live-log">
    <div class="log-header">TERMINAL DAEMON // PORT 443</div>
    <div class="log-content" id="log-content"></div>
    <div class="threat-meter">
      <div style="color:var(--amber);font-size:8pt;letter-spacing:1px;">CONTAINMENT INTEGRITY</div>
      <div class="bar"><div class="fill"></div></div>
    </div>
  </div>
</div>

<script>
// Login: BRAYLIN / 72382006
const VALID_USER="BRAYLIN", VALID_KEY="72382006";

const bootText=[
  {t:"Umbrella Corp. BIOS v7.7.41 ... ",c:"ok",s:"OK"},
  {t:"Loading secure kernel ... ",c:"ok",s:"OK"},
  {t:"Mounting encrypted Arklay archive ... ",c:"ok",s:"OK"},
  {t:"Spinning up Hazard Class OMEGA telemetry ... ",c:"ok",s:"OK"},
  {t:"Cryogenic stabilization check (Tyrant cells) ... ",c:"wn",s:"NOMINAL"},
  {t:"Scanning sub-levels for bio-signatures ... ",c:"wn",s:"3 ANOMALIES"},
  {t:"AI threat-assessment matrix ... ",c:"ok",s:"ONLINE"},
  {t:"Establishing uplink to UC-NET Node 097 ... ",c:"ok",s:"OK"},
  {t:"Encryption handshake (OMEGA) ... ",c:"ok",s:"SECURED"},
  {t:"Rendering Level 7 gateway ...",c:"",s:""}
];

window.onload=()=>{
  const boot=document.getElementById('boot-sequence'); let delay=0;
  bootText.forEach(line=>{
    setTimeout(()=>{
      const p=document.createElement('div'); p.className='boot-line';
      p.innerHTML = line.s ? `${line.t}<span class="${line.c}">${line.s}</span>` : line.t;
      boot.appendChild(p);
    },delay);
    delay += (Math.random()*260)+140;
  });
  setTimeout(()=>{ boot.style.display='none'; document.getElementById('login-screen').style.display='flex';
    document.getElementById('password').focus(); }, delay+700);
};

function validateLogin(){
  const u=document.getElementById('username').value.trim().toUpperCase();
  const k=document.getElementById('password').value.trim();
  const err=document.getElementById('error-msg');
  if(u===VALID_USER && k===VALID_KEY){
    err.style.color="var(--green-glow)"; err.innerText="ACCESS GRANTED. DECRYPTING DOSSIER...";
    document.getElementById('who').innerText=VALID_USER;
    setTimeout(()=>{ document.getElementById('login-screen').style.display='none';
      document.getElementById('main-system').style.display='grid'; startLiveLog(); },1400);
  } else {
    err.style.color="var(--bright-red)"; err.innerText="ACCESS DENIED. LETHAL COUNTERMEASURES ARMED.";
    document.getElementById('password').value="";
  }
}

function switchNode(id,el){
  document.querySelectorAll('.nav-item').forEach(i=>i.classList.remove('active'));
  document.querySelectorAll('.node-container').forEach(n=>n.classList.remove('active'));
  el.classList.add('active');
  document.getElementById('node-'+id).classList.add('active');
  document.querySelector('.main-content').scrollTop=0;
  addLog("Module access: "+id.toUpperCase(),"success");
}

const logContent=document.getElementById('log-content');
const randomLogs=[
  {t:"[SYS] Re-routing thermal exhaust.",k:"normal"},
  {t:"[SEC] Motion detected in Sector 4.",k:"err"},
  {t:"[NET] Node 2 latency spike.",k:"normal"},
  {t:"[WARN] Bio-matter in ventilation shaft B.",k:"warn"},
  {t:"[SYS] AI matrix analyzing mutation vectors...",k:"normal"},
  {t:"[SEC] Bulkhead C3 lockdown engaged.",k:"err"},
  {t:"[SYS] Cryo-pump 4 diagnostic running...",k:"warn"},
  {t:"[BIO] gp-T antigen assay returned positive.",k:"err"},
  {t:"[SYS] DAYLIGHT stockpile: 2 doses remaining.",k:"warn"},
  {t:"[SEC] UV emitter array armed.",k:"normal"}
];
function addLog(text,type="normal"){
  const p=document.createElement('div'); p.className='log-line';
  const time=new Date().toLocaleTimeString('en-US',{hour12:false});
  p.innerHTML=`<span class="timestamp">[${time}]</span> <span class="${type}">${text}</span>`;
  logContent.appendChild(p);
  while(logContent.children.length>26) logContent.removeChild(logContent.firstChild);
}
function startLiveLog(){
  addLog("Terminal access granted to "+VALID_USER+".","success");
  addLog("Dossier PV-001 decrypted.","success");
  addLog("Clearance LEVEL 7 (OMEGA) verified.","success");
  setInterval(()=>{ if(Math.random()>0.55){ const l=randomLogs[Math.floor(Math.random()*randomLogs.length)]; addLog(l.t,l.k); } },2400);
}
</script>
</body>
</html>
