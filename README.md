<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEOMA CORE - النسخة التجريبية</title>
    <style>
        :root {
            --neon-green: #00ff88;
            --dark-bg: #0a0a0a;
            --panel-bg: #141414;
            --text-gray: #b0b0b0;
        }

        body {
            margin: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: var(--dark-bg);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            overflow: hidden;
        }

        .container {
            width: 350px;
            background: var(--panel-bg);
            padding: 25px;
            border-radius: 24px;
            border: 1px solid #222;
            text-align: center;
            box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        }

        .header-stats {
            display: flex;
            justify-content: space-around;
            margin-bottom: 20px;
            background: rgba(255,255,255,0.03);
            padding: 10px;
            border-radius: 12px;
        }

        .stat-box { font-size: 14px; }
        .stat-box span { display: block; font-weight: bold; color: var(--neon-green); font-size: 18px; }

        .neoma-display {
            width: 160px;
            height: 160px;
            margin: 20px auto;
            border-radius: 50%;
            background: radial-gradient(circle at top, var(--neon-green), #004422);
            box-shadow: 0 0 40px rgba(0, 255, 136, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 60px;
            transition: 0.5s;
        }

        .neoma-display.working {
            animation: pulse 2s infinite;
            filter: grayscale(0.5);
        }

        @keyframes pulse {
            0% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.05); opacity: 0.7; }
            100% { transform: scale(1); opacity: 1; }
        }

        button {
            width: 100%;
            padding: 16px;
            font-size: 18px;
            border: none;
            border-radius: 14px;
            cursor: pointer;
            background: var(--neon-green);
            color: #000;
            font-
