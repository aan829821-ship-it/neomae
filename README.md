<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8" />
<title>NEOMA – Core MVP</title>
<style>
    body {
        margin: 0;
        font-family: system-ui, sans-serif;
        background: #0b0b0b;
        color: #eaeaea;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100vh;
    }

    .card {
        width: 320px;
        padding: 20px;
        background: #111;
        border-radius: 16px;
        border: 1px solid #1f1f1f;
        text-align: center;
    }

    .stats {
        display: flex;
        justify-content: space-between;
        margin-bottom: 12px;
        font-size: 14px;
        opacity: 0.85;
    }

    .neoma {
        width: 140px;
        height: 140px;
        margin: 20px auto;
        border-radius: 50%;
        background: radial-gradient(circle at top, #00ff88, #006644);
        box-shadow: 0 0 30px #00ff8860;
    }

    button {
        width: 100%;
        padding: 14px;
        font-size: 16px;
        border: none;
        border-radius: 12px;
        cursor: pointer;
        background: #00ff88;
        color: #000;
        font-weight: bold;
    }

    button:disabled {
        background: #333;
        color: #777;
        cursor: not-allowed;
    }

    .timer {
        margin-top: 10px;
        font-size: 14px;
        opacity: 0.8;
    }
</style>
</head>
<body>

<div class="card">
    <div class="stats">
        <div>🔩 خردة: <span id="scrap">0</span></div>
        <div>💰 NMA: <span id="nma">0</span></div>
    </div>

    <div class="neoma"></div>

    <button id="missionBtn" onclick="startMission()">إرسال في مهمة</button>
    <div class="timer" id="timer"></div>
</div>

<script>
/* ====== DATA ====== */
let neoma = {
    scrap_count: 0,
    nma_balance: 0,
    is_on_mission: false,
    last_mission_start: null,
    mission_duration: 4 * 60 * 60 * 1000 // 4 ساعات
};

let countdownInterval = null;

/* ====== LOGIC ====== */
function startMission() {
    if (neoma.is_on_mission) return;

    neoma.is_on_mission = true;
    neoma.last_mission_start = Date.now();
    document.getElementById("missionBtn").disabled = true;

    startCountdown();

    setTimeout(completeMission, neoma.mission_duration);
}

function completeMission() {
    neoma.is_on_mission = false;
    document.getElementById("missionBtn").disabled = false;
    document.getElementById("timer").textContent = "";
    clearInterval(countdownInterval);

    if (Math.random() > 0.5) {
        neoma.scrap_count += 1;
        alert("🔩 نيوما عاد بقطعة خردة");
    } else {
        neoma.nma_balance += 50;
        alert("💰 نيوما عاد بـ 50 NMA");
    }

    updateUI();
}

function startCountdown() {
    countdownInterval = setInterval(() => {
        const elapsed = Date.now() - neoma.last_mission_start;
        const remaining = neoma.mission_duration - elapsed;

        if (remaining <= 0) return;

        const h = Math.floor(remaining / 3600000);
        const m = Math.floor((remaining % 3600000) / 60000);
        const s = Math.floor((remaining % 60000) / 1000);

        document.getElementById("timer").textContent =
            `⏳ ${h}h ${m}m ${s}s`;
    }, 1000);
}

function updateUI() {
    document.getElementById("scrap").textContent = neoma.scrap_count;
    document.getElementById("nma").textContent = neoma.nma_balance;
}

/* ====== INIT ====== */
updateUI();
</script>

</body>
</html>
