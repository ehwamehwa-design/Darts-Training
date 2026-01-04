# Darts-Training
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<title>Darts 10 Min Training</title>
<style>
body {
  font-family: Arial, sans-serif;
  background:#111;
  color:#fff;
  text-align:center;
  padding:20px;
}
.card {
  background:#222;
  padding:20px;
  border-radius:10px;
  max-width:400px;
  margin:auto;
}
.timer {
  font-size:48px;
  margin:20px 0;
}
input {
  width:90%;
  padding:10px;
  margin:5px 0;
  border-radius:5px;
  border:none;
}
button {
  padding:10px 20px;
  font-size:16px;
  margin-top:10px;
  cursor:pointer;
}
.hidden { display:none; }
</style>
</head>
<body>

<div class="card">
  <h2 id="title">Bereit?</h2>
  <div class="timer" id="timer">10:00</div>

  <div id="inputs"></div>

  <button id="startBtn" onclick="start()">Start Training</button>
  <button id="nextBtn" class="hidden" onclick="next()">Weiter</button>
</div>

<script>
const segments = [
  { name:"Warm-up", time:60, inputs:["Highest Score (3 Darts)"] },
  { name:"20er Segment", time:120, inputs:["20er in Folge","Triple Treffer"] },
  { name:"Bullseye", time:120, inputs:["50er Checkouts (6 Darts)"] },
  { name:"121", time:180, inputs:["Highest Score (acc.)"] },
  { name:"501", time:120, inputs:["Darts für 501"] }
];

let seg = 0, time = 0, interval;

function start(){
  document.getElementById("startBtn").classList.add("hidden");
  loadSegment();
}

function loadSegment(){
  if(seg >= segments.length){
    document.getElementById("title").innerText = "Training beendet 🎯";
    document.getElementById("timer").innerText = "00:00";
    return;
  }

  const s = segments[seg];
  time = s.time;
  document.getElementById("title").innerText = s.name;
  document.getElementById("inputs").innerHTML = "";
  document.getElementById("nextBtn").classList.add("hidden");

  s.inputs.forEach(i=>{
    document.getElementById("inputs").innerHTML +=
      `<input placeholder="${i}">`;
  });

  interval = setInterval(tick,1000);
}

function tick(){
  if(time <= 0){
    clearInterval(interval);
    pauseInput();
    return;
  }
  time--;
  updateTimer();
}

function updateTimer(){
  const m = String(Math.floor(time/60)).padStart(2,"0");
  const s = String(time%60).padStart(2,"0");
  document.getElementById("timer").innerText = `${m}:${s}`;
}

function pauseInput(){
  let pause = 30;
  document.getElementById("title").innerText += " – Eingabe (30s)";
  document.getElementById("nextBtn").classList.remove("hidden");

  interval = setInterval(()=>{
    pause--;
    document.getElementById("timer").innerText = `Pause ${pause}s`;
    if(pause<=0){
      clearInterval(interval);
      next();
    }
  },1000);
}

function next(){
  clearInterval(interval);
  seg++;
  loadSegment();
}
</script>

</body>
</html>
