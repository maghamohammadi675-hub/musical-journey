<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Retro</title>
  <style>
    @font-face {
      font-family: 'Vazir';
      src: url('https://cdn.fontcdn.ir/Font/Persian/Vazir/Vazir-Regular.woff2') format('woff2');
      font-weight: normal;
      font-style: normal;
    }

    @font-face {
      font-family: 'Vazir';
      src: url('https://cdn.fontcdn.ir/Font/Persian/Vazir/Vazir-Bold.woff2') format('woff2');
      font-weight: bold;
      font-style: normal;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Vazir', Tahoma, sans-serif;
      background: linear-gradient(135deg, #0f172a, #1e40af, #38bdf8);
      color: #e0e1dd;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      overflow-x: hidden;
      position: relative;
    }

    .container {
      background: rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(20px);
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.5);
      border: 1px solid rgba(255, 255, 255, 0.3);
      animation: slideIn 1s ease-in-out;
      max-width: 500px;
      width: 100%;
      text-align: center;
    }

    h1 {
      font-size: 2.5rem;
      color: #00f5d4;
      margin-bottom: 20px;
      text-shadow: 0 0 10px rgba(0, 245, 212, 0.7);
      animation: glow 2s ease-in-out infinite alternate;
    }

    .menu-screen {
      display: none;
    }

    .menu-screen.active {
      display: block;
    }

    .menu-title {
      font-size: 1.8rem;
      color: #facc15;
      margin-bottom: 25px;
      text-shadow: 0 0 5px rgba(250, 204, 21, 0.7);
    }

    .menu-buttons {
      display: flex;
      flex-direction: column;
      gap: 15px;
      margin: 20px 0;
    }

    .menu-button {
      padding: 15px 25px;
      border: none;
      border-radius: 15px;
      font-size: 1.2rem;
      font-family: 'Vazir', Tahoma, sans-serif;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
      color: #fff;
    }

    .role-button {
      background: linear-gradient(135deg, #00f5d4, #3b82f6);
    }

    .role-button:hover {
      background: linear-gradient(135deg, #00c4b4, #2563eb);
      transform: translateY(-3px);
      box-shadow: 0 8px 20px rgba(0, 245, 212, 0.4);
    }

    .difficulty-button.easy {
      background: linear-gradient(135deg, #22c55e, #16a34a);
    }

    .difficulty-button.medium {
      background: linear-gradient(135deg, #f59e0b, #d97706);
    }

    .difficulty-button.hard {
      background: linear-gradient(135deg, #ef4444, #dc2626);
    }

    .difficulty-button:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
    }

    .back-button {
      background: linear-gradient(135deg, #6b7280, #4b5563);
      padding: 10px 20px;
      font-size: 1rem;
      margin-top: 20px;
    }

    .back-button:hover {
      background: linear-gradient(135deg, #5b6470, #3b4553);
      transform: translateY(-2px);
    }

    .role-description {
      font-size: 0.9rem;
      color: #a1a1aa;
      margin-top: 10px;
      line-height: 1.4;
    }

    .game-screen {
      display: none;
    }

    .game-screen.active {
      display: block;
    }

    .game-info {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 15px;
      font-size: 0.9rem;
      color: #a1a1aa;
    }

    #high-score-display {
      margin-bottom: 15px;
      position: relative;
    }

    #high-score-text {
      font-size: 1.2rem;
      color: #facc15;
      text-shadow: 0 0 5px rgba(250, 204, 21, 0.5);
      display: inline-block;
    }

    .new-record {
      display: inline-block !important;
      color: #00f5d4;
      font-weight: bold;
      margin-left: 10px;
      animation: recordBlink 1s ease-in-out infinite;
    }

    .hidden {
      display: none;
    }

    @keyframes recordBlink {
      0% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.7; transform: scale(1.1); }
      100% { opacity: 1; transform: scale(1); }
    }

    #game {
      width: 400px;
      height: 400px;
      background: linear-gradient(135deg, #1e293b, #2d3748);
      position: relative;
      border: 3px solid #facc15;
      border-radius: 15px;
      overflow: hidden;
      box-shadow: 0 0 20px rgba(250, 204, 21, 0.5);
      margin: 0 auto;
      contain: strict;
    }

    .player, .enemy, .coin {
      width: 30px;
      height: 30px;
      position: absolute;
      font-size: 24px;
      line-height: 30px;
      text-align: center;
      transition: left 0.1s ease, top 0.1s ease;
      z-index: 1;
    }

    .player {
      background: lime;
      border-radius: 50%;
      box-shadow: 0 0 15px rgba(0, 255, 0, 0.7);
      animation: pulsePlayer 2s ease-in-out infinite;
    }

    .player.police {
      background: #3b82f6;
      box-shadow: 0 0 15px rgba(59, 130, 246, 0.7);
    }

    .enemy {
      background: crimson;
      border-radius: 50%;
      box-shadow: 0 0 15px rgba(220, 20, 60, 0.7);
      animation: pulseEnemy 1.5s ease-in-out infinite;
    }

    .enemy.thief {
      background: #8b5cf6;
      box-shadow: 0 0 15px rgba(139, 92, 246, 0.7);
    }

    .coin {
      background: gold;
      border-radius: 50%;
      box-shadow: 0 0 15px rgba(255, 215, 0, 0.7);
      animation: sparkle 1s ease-in-out infinite;
    }

    #score {
      margin-top: 20px;
      font-size: 1.5rem;
      color: #facc15;
      text-shadow: 0 0 5px rgba(250, 204, 21, 0.5);
    }

    .score-update {
      animation: scorePop 0.5s ease-in-out;
    }

    .high-score-achieved {
      animation: highScoreGlow 1s ease-in-out;
    }

    #controls {
      margin-top: 20px;
      display: grid;
      grid-template-areas: 
        ". up ."
        "left . right"
        ". down .";
      gap: 10px;
      justify-content: center;
      width: 150px;
      margin: 20px auto;
    }

    #controls button {
      width: 50px;
      height: 50px;
      border: none;
      border-radius: 50%;
      font-size: 24px;
      cursor: pointer;
      color: #fff;
      transition: all 0.3s ease;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
    }

    #controls button:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5);
    }

    #controls .up { grid-area: up; background: linear-gradient(135deg, #22c55e, #16a34a); }
    #controls .down { grid-area: down; background: linear-gradient(135deg, #3b82f6, #2563eb); }
    #controls .left { grid-area: left; background: linear-gradient(135deg, #ec4899, #db2777); }
    #controls .right { grid-area: right; background: linear-gradient(135deg, #ef4444, #dc2626); }

    #game-over, #airplane-game-over {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.8);
      backdrop-filter: blur(5px);
      align-items: center;
      justify-content: center;
      z-index: 10;
      animation: fadeIn 0.5s ease-in-out;
    }

    .game-over-content {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 15px;
      padding: 30px;
      text-align: center;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
      color: #e0e1dd;
    }

    #high-score-game-over {
      margin-top: 10px;
      font-size: 1.1rem;
      color: #facc15;
    }

    #new-high-score-message {
      margin: 15px 0;
      padding: 10px;
      background: linear-gradient(135deg, #00f5d4, #3b82f6);
      border-radius: 10px;
      font-weight: bold;
      animation: celebration 2s ease-in-out infinite;
    }

    .game-over-buttons {
      display: flex;
      gap: 10px;
      justify-content: center;
      margin-top: 15px;
    }

    .game-over-button {
      padding: 12px 20px;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      transition: all 0.3s ease;
      font-family: 'Vazir', Tahoma, sans-serif;
      color: #fff;
    }

    #restart-button {
      background: linear-gradient(135deg, #00f5d4, #3b82f6);
      box-shadow: 0 5px 15px rgba(0, 245, 212, 0.3);
    }

    #restart-button:hover {
      background: linear-gradient(135deg, #00c4b4, #2563eb);
      transform: translateY(-3px);
    }

    #menu-button {
      background: linear-gradient(135deg, #6b7280, #4b5563);
      box-shadow: 0 5px 15px rgba(107, 114, 128, 0.3);
    }

    #menu-button:hover {
      background: linear-gradient(135deg, #5b6470, #3b4553);
      transform: translateY(-3px);
    }

    .particles {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      overflow: hidden;
      z-index: 0;
    }

    .particle {
      position: absolute;
      width: 8px;
      height: 8px;
      background: rgba(0, 245, 212, 0.5);
      border-radius: 50%;
      animation: float 6s infinite ease-in-out;
    }

    .particle:nth-child(1) { top: 10%; left: 15%; animation-delay: 0s; }
    .particle:nth-child(2) { top: 40%; left: 75%; animation-delay: 1s; }
    .particle:nth-child(3) { top: 60%; left: 25%; animation-delay: 2s; }
    .particle:nth-child(4) { top: 20%; left: 60%; animation-delay: 3s; }
    .particle:nth-child(5) { top: 30%; left: 45%; animation-delay: 4s; }
    .particle:nth-child(6) { top: 50%; left: 10%; animation-delay: 5s; }

    @keyframes float {
      0% { transform: translateY(0) scale(1); opacity: 0.6; }
      50% { transform: translateY(-100px) scale(1.4); opacity: 0.9; }
      100% { transform: translateY(0) scale(1); opacity: 0.6; }
    }

    @keyframes slideIn {
      from { opacity: 0; transform: translateY(50px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes glow {
      from { text-shadow: 0 0 10px rgba(0, 245, 212, 0.5), 0 0 20px rgba(0, 245, 212, 0.3); }
      to { text-shadow: 0 0 20px rgba(0, 245, 212, 0.7), 0 0 30px rgba(0, 245, 212, 0.5); }
    }

    @keyframes pulsePlayer {
      0% { box-shadow: 0 0 15px rgba(0, 255, 0, 0.7); }
      50% { box-shadow: 0 0 25px rgba(0, 255, 0, 1); }
      100% { box-shadow: 0 0 15px rgba(0, 255, 0, 0.7); }
    }

    @keyframes pulseEnemy {
      0% { box-shadow: 0 0 15px rgba(220, 20, 60, 0.7); }
      50% { box-shadow: 0 0 25px rgba(220, 20, 60, 1); }
      100% { box-shadow: 0 0 15px rgba(220, 20, 60, 0.7); }
    }

    @keyframes sparkle {
      0% { transform: rotate(0deg) scale(1); }
      50% { transform: rotate(180deg) scale(1.2); }
      100% { transform: rotate(360deg) scale(1); }
    }

    @keyframes scorePop {
      0% { transform: scale(1); }
      50% { transform: scale(1.2); }
      100% { transform: scale(1); }
    }

    @keyframes highScoreGlow {
      0% { color: #facc15; text-shadow: 0 0 5px rgba(250, 204, 21, 0.5); }
      50% { color: #00f5d4; text-shadow: 0 0 20px rgba(0, 245, 212, 1); }
      100% { color: #facc15; text-shadow: 0 0 5px rgba(250, 204, 21, 0.5); }
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    @keyframes coinCollect {
      0% { transform: scale(1) rotate(0deg); opacity: 1; }
      50% { transform: scale(1.5) rotate(180deg); opacity: 0.8; }
      100% { transform: scale(0) rotate(360deg); opacity: 0; }
    }

    @keyframes celebration {
      0% { transform: scale(1); }
      50% { transform: scale(1.05); }
      100% { transform: scale(1); }
    }

    .coin-collected {
      animation: coinCollect 0.3s ease-in-out forwards;
    }

    #airplane-game {
      width: 400px;
      height: 400px;
      background: linear-gradient(to bottom, #0c4a6e, #38bdf8, #bae6fd);
      position: relative;
      border: 3px solid #1e40af;
      border-radius: 15px;
      overflow: hidden;
      margin: 20px auto;
      box-shadow: 0 0 25px rgba(30, 64, 175, 0.7);
      animation: skyPulse 5s ease-in-out infinite;
    }

    @keyframes skyPulse {
      0% { background: linear-gradient(to bottom, #0c4a6e, #38bdf8, #bae6fd); }
      50% { background: linear-gradient(to bottom, #075985, #60a5fa, #e0f2fe); }
      100% { background: linear-gradient(to bottom, #0c4a6e, #38bdf8, #bae6fd); }
    }

    .airplane {
      width: 50px;
      height: 35px;
      position: absolute;
      font-size: 30px;
      line-height: 35px;
      text-align: center;
      transition: all 0.1s ease;
      z-index: 10;
      filter: drop-shadow(0 0 10px rgba(255, 255, 255, 0.9));
      background: none;
      border-radius: 0;
      clip-path: none;
      animation: airplaneGlow 1.5s ease-in-out infinite;
    }

    @keyframes airplaneGlow {
      0% { box-shadow: 0 0 10px rgba(255, 255, 255, 0.7); }
      50% { box-shadow: 0 0 20px rgba(255, 255, 255, 1); }
      100% { box-shadow: 0 0 10px rgba(255, 255, 255, 0.7); }
    }

    .airplane::after {
      content: '';
      position: absolute;
      width: 30px;
      height: 8px;
      background: linear-gradient(90deg, transparent, rgba(249, 115, 22, 0.8));
      left: -30px;
      top: 14px;
      animation: trail 0.3s linear infinite;
    }

    @keyframes trail {
      0% { opacity: 0.8; transform: translateX(0); }
      100% { opacity: 0; transform: translateX(-30px); }
    }

    .obstacle {
      width: 30px;
      height: 50px;
      position: absolute;
      background: linear-gradient(135deg, #4b5563, #1f2937);
      border-radius: 8px;
      box-shadow: 0 0 15px rgba(31, 41, 55, 0.8);
      z-index: 5;
      animation: obstacleGlow 1.2s ease-in-out infinite alternate;
    }

    @keyframes obstacleGlow {
      0% { box-shadow: 0 0 15px rgba(31, 41, 55, 0.8); }
      100% { box-shadow: 0 0 25px rgba(31, 41, 55, 1); }
    }

    .fuel-obstacle {
      width: 30px;
      height: 30px;
      position: absolute;
      background: linear-gradient(135deg, #16a34a, #15803d);
      border-radius: 50%;
      box-shadow: 0 0 15px rgba(22, 163, 74, 0.8);
      z-index: 6;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      animation: fuelGlow 1s ease-in-out infinite alternate;
    }

    .fuel-obstacle::before {
      content: "⛽";
      color: #fff;
      text-shadow: 0 0 5px rgba(255, 255, 255, 0.8);
    }

    @keyframes fuelGlow {
      0% { box-shadow: 0 0 15px rgba(22, 163, 74, 0.8); }
      100% { box-shadow: 0 0 25px rgba(22, 163, 74, 1); }
    }

    .bullet {
      width: 10px;
      height: 20px;
      position: absolute;
      background: linear-gradient(135deg, #f97316, #c2410c);
      border-radius: 50%;
      box-shadow: 0 0 12px rgba(249, 115, 22, 0.9);
      z-index: 7;
    }

    .explosion {
      width: 50px;
      height: 50px;
      position: absolute;
      font-size: 35px;
      animation: explode 0.5s ease-out forwards;
      z-index: 8;
    }

    @keyframes explode {
      0% { transform: scale(0.5); opacity: 1; }
      100% { transform: scale(2); opacity: 0; }
    }

    #airplane-controls {
      margin-top: 20px;
      display: grid;
      grid-template-areas: 
        ". up ."
        "left shoot right"
        ". down .";
      gap: 10px;
      justify-content: center;
      width: 170px;
      margin: 20px auto;
    }

    #airplane-controls button {
      width: 50px;
      height: 50px;
      border: none;
      border-radius: 50%;
      font-size: 24px;
      cursor: pointer;
      color: #fff;
      transition: all 0.3s ease;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
    }

    #airplane-controls .up { grid-area: up; background: linear-gradient(135deg, #22c55e, #16a34a); }
    #airplane-controls .down { grid-area: down; background: linear-gradient(135deg, #3b82f6, #2563eb); }
    #airplane-controls .left { grid-area: left; background: linear-gradient(135deg, #ec4899, #db2777); }
    #airplane-controls .right { grid-area: right; background: linear-gradient(135deg, #ef4444, #dc2626); }
    #airplane-controls .shoot { grid-area: shoot; background: linear-gradient(135deg, #f59e0b, #d97706); }

    #airplane-controls button:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5);
    }

    .game-buttons {
      display: flex;
      gap: 10px;
      justify-content: center;
      margin-top: 20px;
      flex-wrap: wrap;
    }

    .cloud {
      position: absolute;
      color: rgba(255, 255, 255, 0.7);
      font-size: 28px;
      animation: cloudFloat 6s infinite ease-in-out;
      z-index: 1;
      filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.5));
    }

    @keyframes cloudFloat {
      0%, 100% { transform: translateX(0); }
      50% { transform: translateX(-40px); }
    }

    .fuel-bar-container {
      width: 200px;
      height: 20px;
      background: rgba(255, 255, 255, 0.2);
      border-radius: 10px;
      overflow: hidden;
      margin: 10px auto;
    }

    .fuel-bar {
      height: 100%;
      background: linear-gradient(135deg, #16a34a, #22c55e);
      width: 100%;
      transition: width 0.3s ease;
    }

    @media (max-width: 500px) {
      #game, #airplane-game {
        width: 300px;
        height: 300px;
      }

      .player, .enemy, .coin {
        width: 25px;
        height: 25px;
        font-size: 20px;
        line-height: 25px;
      }

      .airplane {
        width: 40px;
        height: 30px;
        font-size: 24px;
      }

      .obstacle {
        width: 25px;
        height: 40px;
      }

      .fuel-obstacle {
        width: 25px;
        height: 25px;
        font-size: 14px;
      }

      h1 {
        font-size: 2rem;
      }

      .menu-title {
        font-size: 1.5rem;
      }

      .menu-button {
        font-size: 1rem;
        padding: 12px 20px;
      }

      #high-score-text {
        font-size: 1rem;
      }

      #controls, #airplane-controls {
        width: 120px;
      }

      #controls button, #airplane-controls button {
        width: 40px;
        height: 40px;
        font-size: 20px;
      }

      #airplane-controls {
        width: 140px;
      }

      .game-over-buttons {
        flex-direction: column;
        gap: 8px;
      }

      .fuel-bar-container {
        width: 150px;
      }
    }

    @media (max-width: 350px) {
      #controls, #airplane-controls {
        width: 100px;
      }

      #controls button, #airplane-controls button {
        width: 35px;
        height: 35px;
        font-size: 18px;
      }

      #high-score-text {
        font-size: 0.9rem;
      }

      .menu-button {
        font-size: 0.9rem;
        padding: 10px 15px;
      }

      .fuel-bar-container {
        width: 120px;
      }
    }
  </style>
</head>
<body>
  <div class="particles">
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
  </div>

  <div id="game-selector" class="container">
    <h1>🎮 انتخاب بازی</h1>
    <div class="menu-buttons">
      <button class="menu-button role-button" onclick="showGoldThiefGame()">
        💰 دزد طلا
        <div class="role-description">بازی کلاسیک دزد و پلیس</div>
      </button>
      <button class="menu-button role-button" onclick="showAirplaneGame()">
        ✈️ آتاری هواپیما
        <div class="role-description">پرواز کن و موانع را نابود کن</div>
      </button>
    </div>
  </div>

  <div class="container" id="gold-thief-container" style="display: none;">
    <h1>🎮 دزد طلا</h1>
    
    <div id="role-menu" class="menu-screen active">
      <div class="menu-title">انتخاب نقش</div>
      <div class="menu-buttons">
        <button class="menu-button role-button" onclick="selectRole('thief-male')">
          🦹‍♂️ دزد (مرد)
          <div class="role-description">سکه‌ها را جمع کن و از پلیس فرار کن</div>
        </button>
        <button class="menu-button role-button" onclick="selectRole('thief-female')">
          🦹‍♀️ دزد (زن)
          <div class="role-description">سکه‌ها را جمع کن و از پلیس فرار کن</div>
        </button>
        <button class="menu-button role-button" onclick="selectRole('police-male')">
          👮‍♂️ پلیس (مرد)
          <div class="role-description">دزد را بگیر و از جمع‌آوری سکه جلوگیری کن</div>
        </button>
        <button class="menu-button role-button" onclick="selectRole('police-female')">
          👮‍♀️ پلیس (زن)
          <div class="role-description">دزد را بگیر و از جمع‌آوری سکه جلوگیری کن</div>
        </button>
      </div>
      <button class="menu-button back-button" onclick="showGameSelector()">بازگشت</button>
    </div>

    <div id="difficulty-menu" class="menu-screen">
      <div class="menu-title">انتخاب سختی</div>
      <div class="menu-buttons">
        <button class="menu-button difficulty-button easy" onclick="selectDifficulty('easy')">
          🟢 آسان
          <div class="role-description">سرعت عادی</div>
        </button>
        <button class="menu-button difficulty-button medium" onclick="selectDifficulty('medium')">
          🟡 متوسط
          <div class="role-description">سرعت بیشتر</div>
        </button>
        <button class="menu-button difficulty-button hard" onclick="selectDifficulty('hard')">
          🔴 سخت
          <div class="role-description">سرعت خیلی زیاد</div>
        </button>
      </div>
      <button class="menu-button back-button" onclick="backToRoleMenu()">بازگشت</button>
    </div>

    <div id="game-screen" class="game-screen">
      <div class="game-info">
        <span id="current-role">نقش: دزد (مرد)</span>
        <span id="current-difficulty">سختی: آسان</span>
      </div>
      
      <div id="high-score-display">
        <span id="high-score-text">بهترین امتیاز: <span id="high-score-value">0</span></span>
        <span id="new-record-indicator" class="new-record hidden">🏆 رکورد جدید!</span>
      </div>
      
      <div id="game">
        <div class="player" id="player">🦹‍♂️</div>
        <div class="enemy" id="enemy">👮‍♂️</div>
        <div class="coin" id="coin">💰</div>
      </div>
      
      <div id="score">امتیاز: 0</div>
      <div style="margin-top: 10px; font-size: 12px; color: #888; opacity: 0.7;">ساخته شده توسط meti</div>
      
      <div id="controls">
        <button class="up" onclick="move('up')">△</button>
        <button class="left" onclick="move('left')">◻</button>
        <button class="down" onclick="move('down')">×</button>
        <button class="right" onclick="move('right')">◯</button>
      </div>
      
      <button class="menu-button back-button" onclick="backToMainMenu()" style="margin-top: 20px;">منوی اصلی</button>
    </div>
  </div>

  <div id="airplane-game-container" class="container" style="display: none;">
    <h1>✈️ آتاری هواپیما</h1>
    
    <div class="game-info">
      <span id="airplane-score-display">امتیاز: 0</span>
      <span id="airplane-high-score-display">بهترین: 0</span>
    </div>
    
    <div class="fuel-bar-container">
      <div class="fuel-bar" id="fuel-bar"></div>
    </div>
    
    <div id="airplane-game">
      <div id="airplane" class="airplane">✈️</div>
    </div>
    
    <div id="airplane-controls">
      <button class="up" onclick="airplaneMove('up')">△</button>
      <button class="left" onclick="airplaneMove('left')">◻</button>
      <button class="down" onclick="airplaneMove('down')">×</button>
      <button class="right" onclick="airplaneMove('right')">◯</button>
      <button class="shoot" onclick="airplaneShoot()">🔥</button>
    </div>
    
    <div style="margin-top: 10px; font-size: 12px; color: #888; opacity: 0.7;">ساخته شده توسط meti</div>
    
    <div class="game-buttons">
      <button class="menu-button back-button" onclick="startAirplaneGame()">شروع بازی</button>
      <button class="menu-button back-button" onclick="pauseAirplaneGame()">توقف</button>
      <button class="menu-button back-button" onclick="showGameSelector()">بازگشت</button>
    </div>
  </div>

  <div id="game-over">
    <div class="game-over-content">
      <h2 id="game-over-title">بازی تمام شد!</h2>
      <p>امتیاز نهایی: <span id="final-score">0</span></p>
      <p id="high-score-game-over">بهترین امتیاز: <span id="final-high-score">0</span></p>
      <div id="new-high-score-message" class="hidden">
        🎉 تبریک! رکورد جدید! 🎉
      </div>
      <div class="game-over-buttons">
        <button id="restart-button" class="game-over-button" onclick="restartGame()">شروع دوباره</button>
        <button id="menu-button" class="game-over-button" onclick="backToMainMenu()">منوی اصلی</button>
      </div>
    </div>
  </div>

  <div id="airplane-game-over">
    <div class="game-over-content">
      <h2>هواپیما سقوط کرد!</h2>
      <p>امتیاز نهایی: <span id="airplane-final-score">0</span></p>
      <p>بهترین امتیاز: <span id="airplane-final-high-score">0</span></p>
      <div id="airplane-new-high-score" class="hidden">
        🎉 رکورد جدید! 🎉
      </div>
      <div class="game-over-buttons">
        <button class="game-over-button" onclick="restartAirplaneGame()">شروع دوباره</button>
        <button class="game-over-button" onclick="showGameSelector()">انتخاب بازی</button>
      </div>
    </div>
  </div>

  <audio id="coin-sound" preload="auto">
    <source src="data:audio/wav;base64,UklGRnoGAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YQoGAACBhYqFbF1fdJivrJBhNjVgodDbq2EcBj+a2/LDciUFLIHO8tiJNwgZaLvt559NEAxQp+PwtmMcBjiR1/LMeSwFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhBzaL0fDJdyoFKoDK8dCDPQhVqOXSrGIhBjmQ1vLNeSsFV6vn7aJKDQpMpODxt2M" type="audio/wav">
  </audio>
  <audio id="game-over-sound" preload="auto">
    <source src="data:audio/wav;base64,UklGRnoGAABXQVZFZm1g" type="audio/wav">
  </audio>

  <script>
    // Game elements - Gold Thief
    const player = document.getElementById("player");
    const enemy = document.getElementById("enemy");
    const coin = document.getElementById("coin");
    const scoreEl = document.getElementById("score");
    const gameOverEl = document.getElementById("game-over");
    const finalScoreEl = document.getElementById("final-score");
    const coinSound = document.getElementById("coin-sound");
    const gameOverSound = document.getElementById("game-over-sound");

    // Menu elements - Gold Thief
    const roleMenu = document.getElementById("role-menu");
    const difficultyMenu = document.getElementById("difficulty-menu");
    const gameScreen = document.getElementById("game-screen");
    const currentRoleEl = document.getElementById("current-role");
    const currentDifficultyEl = document.getElementById("current-difficulty");
    const gameOverTitle = document.getElementById("game-over-title");

    // High score elements - Gold Thief
    const highScoreValueEl = document.getElementById("high-score-value");
    const finalHighScoreEl = document.getElementById("final-high-score");
    const newRecordIndicator = document.getElementById("new-record-indicator");
    const newHighScoreMessage = document.getElementById("new-high-score-message");

    // Game variables - Gold Thief
    let score = 0;
    let highScore = 0;
    let gameInterval;
    let isNewRecord = false;
    const step = 20;

    // Game settings - Gold Thief
    let selectedRole = 'thief-male';
    let selectedDifficulty = 'easy';
    let gameSpeed = 500;

    // Game dimensions
    let gameWidth = 400;
    let gameHeight = 400;
    const characterSize = 30;
    let maxX = gameWidth - characterSize;
    let maxY = gameHeight - characterSize;

    // Player and enemy positions - Gold Thief
    let playerPos = { x: 0, y: 0 };
    let enemyPos = { x: maxX, y: maxY };
    let coinCollected = false;

    // Difficulty settings - Gold Thief
    const difficultySettings = {
      easy: { speed: 500, speedMultiplier: 1 },
      medium: { speed: 350, speedMultiplier: 1.5 },
      hard: { speed: 250, speedMultiplier: 2 },
      veryHard: { speed: 150, speedMultiplier: 2.5 }
    };

    // Airplane Game Variables
    let airplaneGameActive = false;
    let airplaneInterval;
    let obstacleInterval;
    let bulletInterval;
    let airplaneScore = 0;
    let airplaneHighScore = 0;
    let airplanePaused = false;
    let fuelLevel = 100;
    let fuelInterval;

    const airplaneGameArea = { width: 400, height: 400 };
    let airplanePos = { x: 50, y: 200 };
    let obstacles = [];
    let bullets = [];
    let obstacleSpeed = 6;
    let bulletSpeed = 8;

    // High Score Management
    class HighScoreManager {
      static getKey(role, difficulty) {
        return `goldThief_${role}_${difficulty}`;
      }
      
      static load(role, difficulty) {
        try {
          const stored = sessionStorage.getItem(this.getKey(role, difficulty));
          return stored ? parseInt(stored, 10) : 0;
        } catch (error) {
          return 0;
        }
      }
      
      static save(role, difficulty, score) {
        try {
          sessionStorage.setItem(this.getKey(role, difficulty), score.toString());
        } catch (error) {}
      }
      
      static clear() {
        try {
          const keys = Object.keys(sessionStorage);
          keys.forEach(key => {
            if (key.startsWith('goldThief_')) {
              sessionStorage.removeItem(key);
            }
          });
        } catch (error) {}
      }
    }

    // Game Selector Functions
    function showAirplaneGame() {
      document.getElementById('game-selector').style.display = 'none';
      document.getElementById('airplane-game-container').style.display = 'block';
      document.getElementById('gold-thief-container').style.display = 'none';
      initializeAirplaneGame();
    }

    function showGoldThiefGame() {
      document.getElementById('game-selector').style.display = 'none';
      document.getElementById('gold-thief-container').style.display = 'block';
      document.getElementById('airplane-game-container').style.display = 'none';
      roleMenu.classList.add('active');
      difficultyMenu.classList.remove('active');
      gameScreen.classList.remove('active');
    }

    function showGameSelector() {
      document.getElementById('game-selector').style.display = 'block';
      document.getElementById('airplane-game-container').style.display = 'none';
      document.getElementById('gold-thief-container').style.display = 'none';
      document.getElementById('airplane-game-over').style.display = 'none';
      document.getElementById('game-over').style.display = 'none';
      stopAirplaneGame();
      stopGoldThiefGame();
    }

    function stopGoldThiefGame() {
      if (gameInterval) {
        clearInterval(gameInterval);
      }
    }

    // Gold Thief Game Functions
    function selectRole(role) {
      selectedRole = role;
      showDifficultyMenu();
    }

    function selectDifficulty(difficulty) {
      selectedDifficulty = difficulty;
      if (selectedRole.includes('police')) {
        if (difficulty === 'easy') gameSpeed = difficultySettings.medium.speed;
        else if (difficulty === 'medium') gameSpeed = difficultySettings.hard.speed;
        else gameSpeed = difficultySettings.veryHard.speed;
      } else {
        gameSpeed = difficultySettings[difficulty].speed;
      }
      startGame();
    }

    function showDifficultyMenu() {
      roleMenu.classList.remove('active');
      difficultyMenu.classList.add('active');
    }

    function backToRoleMenu() {
      difficultyMenu.classList.remove('active');
      roleMenu.classList.add('active');
    }

    function showGameScreen() {
      difficultyMenu.classList.remove('active');
      gameScreen.classList.add('active');
      
      const roleText = selectedRole === 'thief-male' ? 'دزد (مرد)' : 
                      selectedRole === 'thief-female' ? 'دزد (زن)' : 
                      selectedRole === 'police-male' ? 'پلیس (مرد)' : 'پلیس (زن)';
      const difficultyText = selectedDifficulty === 'easy' ? 'آسان' : 
                           selectedDifficulty === 'medium' ? 'متوسط' : 'سخت';
      
      currentRoleEl.textContent = `نقش: ${roleText}`;
      currentDifficultyEl.textContent = `سختی: ${difficultyText}`;
    }

    function backToMainMenu() {
      if (gameInterval) {
        clearInterval(gameInterval);
      }
      
      gameScreen.classList.remove('active');
      gameOverEl.style.display = "none";
      roleMenu.classList.add('active');
      
      resetGameState();
    }

    function initializeHighScore() {
      highScore = HighScoreManager.load(selectedRole, selectedDifficulty);
      updateHighScoreDisplay();
    }

    function updateHighScoreDisplay() {
      highScoreValueEl.textContent = highScore;
      finalHighScoreEl.textContent = highScore;
    }

    function checkHighScore() {
      if (score > highScore) {
        highScore = score;
        isNewRecord = true;
        HighScoreManager.save(selectedRole, selectedDifficulty, highScore);
        updateHighScoreDisplay();
        
        newRecordIndicator.classList.remove('hidden');
        scoreEl.classList.add('high-score-achieved');
        
        setTimeout(() => {
          newRecordIndicator.classList.add('hidden');
          scoreEl.classList.remove('high-score-achieved');
        }, 3000);
      }
    }

    function updatePosition() {
      playerPos.x = Math.max(0, Math.min(playerPos.x, maxX));
      playerPos.y = Math.max(0, Math.min(playerPos.y, maxY));
      enemyPos.x = Math.max(0, Math.min(enemyPos.x, maxX));
      enemyPos.y = Math.max(0, Math.min(enemyPos.y, maxY));

      player.style.left = playerPos.x + "px";
      player.style.top = playerPos.y + "px";
      enemy.style.left = enemyPos.x + "px";
      enemy.style.top = enemyPos.y + "px";
    }

    function placeCoin() {
      let x, y, attempts = 0;
      const maxStepsX = Math.floor(maxX / step);
      const maxStepsY = Math.floor(maxY / step);
      
      do {
        x = Math.floor(Math.random() * (maxStepsX + 1)) * step;
        y = Math.floor(Math.random() * (maxStepsY + 1)) * step;
        attempts++;
      } while (attempts < 20 && (
        (Math.abs(x - playerPos.x) < step * 3 && Math.abs(y - playerPos.y) < step * 3) ||
        (Math.abs(x - enemyPos.x) < step * 2 && Math.abs(y - enemyPos.y) < step * 2)
      ));
      
      coin.style.left = x + "px";
      coin.style.top = y + "px";
    }

    function setupGameRoles() {
      if (selectedRole === 'thief-male') {
        player.innerHTML = '🦹‍♂️';
        player.className = 'player';
        enemy.innerHTML = '👮‍♂️';
        enemy.className = 'enemy';
      } else if (selectedRole === 'thief-female') {
        player.innerHTML = '🦹‍♀️';
        player.className = 'player';
        enemy.innerHTML = '👮‍♀️';
        enemy.className = 'enemy';
      } else if (selectedRole === 'police-male') {
        player.innerHTML = '👮‍♂️';
        player.className = 'player police';
        enemy.innerHTML = '🦹‍♂️';
        enemy.className = 'enemy thief';
      } else {
        player.innerHTML = '👮‍♀️';
        player.className = 'player police';
        enemy.innerHTML = '🦹‍♀️';
        enemy.className = 'enemy thief';
      }
    }

    function checkCollision() {
      const px = playerPos.x;
      const py = playerPos.y;
      const cx = parseInt(coin.style.left);
      const cy = parseInt(coin.style.top);
      const ex = enemyPos.x;
      const ey = enemyPos.y;
      
      if (selectedRole.includes('thief')) {
        if (!coinCollected && Math.abs(px - cx) < characterSize && Math.abs(py - cy) < characterSize) {
          coinCollected = true;
          score++;
          scoreEl.textContent = `امتیاز: ${score}`;
          scoreEl.classList.add("score-update");
          setTimeout(() => scoreEl.classList.remove("score-update"), 500);
          
          checkHighScore();
          
          coin.classList.add("coin-collected");
          try {
            coinSound.play();
          } catch (error) {}
          
          setTimeout(() => {
            coin.classList.remove("coin-collected");
            placeCoin();
            coinCollected = false;
          }, 300);
        }

        if (Math.abs(px - ex) < characterSize && Math.abs(py - ey) < characterSize) {
          endGame('دستگیر شدی!');
        }
      } else {
        if (Math.abs(px - ex) < characterSize && Math.abs(py - ey) < characterSize) {
          score++;
          scoreEl.textContent = `امتیاز: ${score}`;
          scoreEl.classList.add("score-update");
          setTimeout(() => scoreEl.classList.remove("score-update"), 500);
          
          checkHighScore();
          
          enemyPos = { x: Math.floor(Math.random() * (maxX / step)) * step, y: Math.floor(Math.random() * (maxY / step)) * step };
          updatePosition();
        }

        if (!coinCollected && Math.abs(ex - cx) < characterSize && Math.abs(ey - cy) < characterSize) {
          coinCollected = true;
          if (score > 0) score--;
          scoreEl.textContent = `امتیاز: ${score}`;
          
          coin.classList.add("coin-collected");
          
          setTimeout(() => {
            coin.classList.remove("coin-collected");
            placeCoin();
            coinCollected = false;
          }, 300);
        }
      }
    }

    function enemyMove() {
      if (selectedRole.includes('thief')) {
        if (enemyPos.x < playerPos.x && enemyPos.x + step <= maxX) {
          enemyPos.x += step;
        } else if (enemyPos.x > playerPos.x && enemyPos.x - step >= 0) {
          enemyPos.x -= step;
        }
        
        if (enemyPos.y < playerPos.y && enemyPos.y + step <= maxY) {
          enemyPos.y += step;
        } else if (enemyPos.y > playerPos.y && enemyPos.y - step >= 0) {
          enemyPos.y -= step;
        }
      } else {
        const cx = parseInt(coin.style.left);
        const cy = parseInt(coin.style.top);
        
        const distanceToPlayer = Math.abs(enemyPos.x - playerPos.x) + Math.abs(enemyPos.y - playerPos.y);
        
        if (distanceToPlayer > step * 3) {
          if (enemyPos.x < cx && enemyPos.x + step <= maxX) {
            enemyPos.x += step;
          } else if (enemyPos.x > cx && enemyPos.x - step >= 0) {
            enemyPos.x -= step;
          }
          
          if (enemyPos.y < cy && enemyPos.y + step <= maxY) {
            enemyPos.y += step;
          } else if (enemyPos.y > cy && enemyPos.y - step >= 0) {
            enemyPos.y -= step;
          }
        } else {
          if (enemyPos.x < playerPos.x && enemyPos.x - step >= 0) {
            enemyPos.x -= step;
          } else if (enemyPos.x > playerPos.x && enemyPos.x + step <= maxX) {
            enemyPos.x += step;
          }
          
          if (enemyPos.y < playerPos.y && enemyPos.y - step >= 0) {
            enemyPos.y -= step;
          } else if (enemyPos.y > playerPos.y && enemyPos.y + step <= maxY) {
            enemyPos.y += step;
          }
        }
      }
    }

    function move(direction) {
      switch (direction) {
        case 'up':
          if (playerPos.y - step >= 0) {
            playerPos.y -= step;
          }
          break;
        case 'down':
          if (playerPos.y + step <= maxY) {
            playerPos.y += step;
          }
          break;
        case 'left': 
          if (playerPos.x - step >= 0) {
            playerPos.x -= step;
          }
          break;
        case 'right':
          if (playerPos.x + step <= maxX) {
            playerPos.x += step;
          }
          break;
      }
      
      updatePosition();
      checkCollision();
    }

    function endGame(message = 'بازی تمام شد!') {
      clearInterval(gameInterval);
      
      gameOverTitle.textContent = message;
      finalScoreEl.textContent = score;
      finalHighScoreEl.textContent = highScore;
      
      if (isNewRecord) {
        newHighScoreMessage.classList.remove('hidden');
      } else {
        newHighScoreMessage.classList.add('hidden');
      }
      
      gameOverEl.style.display = "flex";
      
      try {
        gameOverSound.play();
      } catch (error) {}
    }

    function resetGameState() {
      score = 0;
      isNewRecord = false;
      coinCollected = false;
      scoreEl.textContent = "امتیاز: 0";
      
      playerPos = { x: 0, y: 0 };
      enemyPos = { x: maxX, y: maxY };
      
      newRecordIndicator.classList.add('hidden');
      newHighScoreMessage.classList.add('hidden');
      scoreEl.classList.remove('high-score-achieved');
    }

    function startGame() {
      showGameScreen();
      initializeHighScore();
      setupGameRoles();
      resetGameState();
      adjustGameForMobile();
      
      placeCoin();
      updatePosition();
      gameInterval = setInterval(gameLoop, gameSpeed);
    }

    function restartGame() {
      gameOverEl.style.display = "none";
      resetGameState();
      
      placeCoin();
      updatePosition();
      gameInterval = setInterval(gameLoop, gameSpeed);
    }

    function gameLoop() {
      enemyMove();
      updatePosition();
      checkCollision();
    }

    // Airplane Game Functions
    function initializeAirplaneGame() {
      airplaneHighScore = parseInt(localStorage.getItem('airplaneHighScore') || '0');
      document.getElementById('airplane-high-score-display').textContent = `بهترین: ${airplaneHighScore}`;
      resetAirplaneGame();
      createClouds();
    }

    function createClouds() {
      const gameArea = document.getElementById('airplane-game');
      const existingClouds = gameArea.querySelectorAll('.cloud');
      existingClouds.forEach(cloud => cloud.remove());
      
      for (let i = 0; i < 6; i++) {
        const cloud = document.createElement('div');
        cloud.className = 'cloud';
        cloud.innerHTML = '☁️';
        cloud.style.left = Math.random() * 350 + 'px';
        cloud.style.top = Math.random() * 300 + 'px';
        cloud.style.animationDelay = Math.random() * 3 + 's';
        gameArea.appendChild(cloud);
      }
    }

    function updateFuelDisplay() {
      const fuelBar = document.getElementById('fuel-bar');
      fuelBar.style.width = `${fuelLevel}%`;
      if (fuelLevel <= 30) {
        fuelBar.style.background = 'linear-gradient(135deg, #ef4444, #dc2626)';
      } else if (fuelLevel <= 60) {
        fuelBar.style.background = 'linear-gradient(135deg, #f59e0b, #d97706)';
      } else {
        fuelBar.style.background = 'linear-gradient(135deg, #16a34a, #22c55e)';
      }
    }

    function resetAirplaneGame() {
      airplaneScore = 0;
      airplanePos = { x: 50, y: 200 };
      obstacles = [];
      bullets = [];
      obstacleSpeed = 6;
      bulletSpeed = 8;
      airplanePaused = false;
      fuelLevel = 100;
      
      document.getElementById('airplane-score-display').textContent = `امتیاز: ${airplaneScore}`;
      updateFuelDisplay();
      
      const gameArea = document.getElementById('airplane-game');
      const existingElements = gameArea.querySelectorAll('.obstacle, .fuel-obstacle, .bullet, .explosion');
      existingElements.forEach(el => el.remove());
      
      const airplane = document.getElementById('airplane');
      airplane.style.left = airplanePos.x + 'px';
      airplane.style.top = airplanePos.y + 'px';
    }

    function startAirplaneGame() {
      if (airplaneGameActive) return;
      
      airplaneGameActive = true;
      airplanePaused = false;
      
      airplaneInterval = setInterval(updateAirplaneGame, 30);
      obstacleInterval = setInterval(createObstacle, 1200);
      bulletInterval = setInterval(updateBullets, 20);
      fuelInterval = setInterval(updateFuel, 1000);
    }

    function pauseAirplaneGame() {
      if (!airplaneGameActive) return;
      
      airplanePaused = !airplanePaused;
      
      if (airplanePaused) {
        clearInterval(airplaneInterval);
        clearInterval(obstacleInterval);
        clearInterval(bulletInterval);
        clearInterval(fuelInterval);
      } else {
        airplaneInterval = setInterval(updateAirplaneGame, 30);
        obstacleInterval = setInterval(createObstacle, 1200);
        bulletInterval = setInterval(updateBullets, 20);
        fuelInterval = setInterval(updateFuel, 1000);
      }
    }

    function stopAirplaneGame() {
      airplaneGameActive = false;
      clearInterval(airplaneInterval);
      clearInterval(obstacleInterval);
      clearInterval(bulletInterval);
      clearInterval(fuelInterval);
    }

    function updateFuel() {
      if (airplanePaused) return;
      
      fuelLevel = Math.max(0, fuelLevel - 1); // Increased fuel consumption rate slightly
      updateFuelDisplay();
      
      if (fuelLevel <= 0) {
        endAirplaneGame();
      }
    }

    function airplaneMove(direction) {
      if (!airplaneGameActive || airplanePaused) return;
      
      const step = 20;
      const gameWidth = window.innerWidth <= 500 ? 300 : 400;
      const gameHeight = window.innerWidth <= 500 ? 300 : 400;
      
      switch (direction) {
        case 'up':
          if (airplanePos.y - step >= 0) {
            airplanePos.y -= step;
          }
          break;
        case 'down':
          if (airplanePos.y + step <= gameHeight - 35) {
            airplanePos.y += step;
          }
          break;
        case 'left':
          if (airplanePos.x - step >= 0) {
            airplanePos.x -= step;
          }
          break;
        case 'right':
          if (airplanePos.x + step <= gameWidth - 50) {
            airplanePos.x += step;
          }
          break;
      }
      
      const airplane = document.getElementById('airplane');
      airplane.style.left = airplanePos.x + 'px';
      airplane.style.top = airplanePos.y + 'px';
    }

    function airplaneShoot() {
      if (!airplaneGameActive || airplanePaused) return;
      
      const bullet = {
        x: airplanePos.x + 20,
        y: airplanePos.y - 20,
        element: null
      };
      
      const bulletEl = document.createElement('div');
      bulletEl.className = 'bullet';
      bulletEl.style.left = bullet.x + 'px';
      bulletEl.style.top = bullet.y + 'px';
      
      bullet.element = bulletEl;
      bullets.push(bullet);
      
      document.getElementById('airplane-game').appendChild(bulletEl);
    }

    function createObstacle() {
      if (!airplaneGameActive || airplanePaused) return;
      
      const gameWidth = window.innerWidth <= 500 ? 300 : 400;
      const gameHeight = window.innerWidth <= 500 ? 300 : 400;
      
      const isFuelObstacle = Math.random() < 0.3;
      
      const obstacle = {
        x: Math.random() * (gameWidth - (isFuelObstacle ? 30 : 30)),
        y: -50,
        element: null,
        isFuel: isFuelObstacle
      };
      
      const obstacleEl = document.createElement('div');
      obstacleEl.className = isFuelObstacle ? 'fuel-obstacle' : 'obstacle';
      obstacleEl.style.left = obstacle.x + 'px';
      obstacleEl.style.top = obstacle.y + 'px';
      
      obstacle.element = obstacleEl;
      obstacles.push(obstacle);
      
      document.getElementById('airplane-game').appendChild(obstacleEl);
    }

    function updateAirplaneGame() {
      if (airplanePaused) return;
      
      const gameHeight = window.innerWidth <= 500 ? 300 : 400;
      const gameWidth = window.innerWidth <= 500 ? 300 : 400;
      
      obstacles.forEach((obstacle, index) => {
        obstacle.y += obstacleSpeed;
        obstacle.element.style.top = obstacle.y + 'px';
        
        if (obstacle.y > gameHeight) {
          obstacle.element.remove();
          obstacles.splice(index, 1);
          if (!obstacle.isFuel) {
            airplaneScore += 10;
            document.getElementById('airplane-score-display').textContent = `امتیاز: ${airplaneScore}`;
            
            if (airplaneScore % 80 === 0) {
              obstacleSpeed += 1.2;
            }
          }
        }
        
        const obstacleWidth = obstacle.isFuel ? 30 : 30;
        const obstacleHeight = obstacle.isFuel ? 30 : 50;
        
        if (obstacle.x < airplanePos.x + 50 && 
            obstacle.x + obstacleWidth > airplanePos.x &&
            obstacle.y < airplanePos.y + 35 && 
            obstacle.y + obstacleHeight > airplanePos.y) {
          if (obstacle.isFuel) {
            fuelLevel = Math.min(100, fuelLevel + 33.33);
            airplaneScore += 5;
            document.getElementById('airplane-score-display').textContent = `امتیاز: ${airplaneScore}`;
            obstacle.element.remove();
            obstacles.splice(index, 1);
            updateFuelDisplay();
          } else {
            endAirplaneGame('هواپیما به مانع برخورد کرد!');
          }
        }
      });
      
      bullets.forEach((bullet, bulletIndex) => {
        obstacles.forEach((obstacle, obstacleIndex) => {
          if (!obstacle.isFuel) {
            const obstacleWidth = 30;
            const obstacleHeight = 50;
            
            if (bullet.x < obstacle.x + obstacleWidth && 
                bullet.x + 10 > obstacle.x &&
                bullet.y < obstacle.y + obstacleHeight && 
                bullet.y + 20 > obstacle.y) {
              
              const explosion = document.createElement('div');
              explosion.className = 'explosion';
              explosion.innerHTML = '💥';
              explosion.style.left = obstacle.x + 'px';
              explosion.style.top = obstacle.y + 'px';
              document.getElementById('airplane-game').appendChild(explosion);
              
              setTimeout(() => explosion.remove(), 500);
              
              bullet.element.remove();
              obstacle.element.remove();
              bullets.splice(bulletIndex, 1);
              obstacles.splice(obstacleIndex, 1);
              
              airplaneScore += 60;
              document.getElementById('airplane-score-display').textContent = `امتیاز: ${airplaneScore}`;
            }
          }
        });
      });
    }

    function updateBullets() {
      if (airplanePaused) return;
      
      bullets.forEach((bullet, index) => {
        bullet.y -= bulletSpeed;
        bullet.element.style.top = bullet.y + 'px';
        
        const gameHeight = window.innerWidth <= 500 ? 300 : 400;
        if (bullet.y < -20) {
          bullet.element.remove();
          bullets.splice(index, 1);
        }
      });
    }

    function endAirplaneGame(message = 'هواپیما سقوط کرد!') {
      stopAirplaneGame();
      
      if (airplaneScore > airplaneHighScore) {
        airplaneHighScore = airplaneScore;
        localStorage.setItem('airplaneHighScore', airplaneHighScore.toString());
        document.getElementById('airplane-new-high-score').classList.remove('hidden');
      } else {
        document.getElementById('airplane-new-high-score').classList.add('hidden');
      }
      
      document.getElementById('airplane-final-score').textContent = airplaneScore;
      document.getElementById('airplane-final-high-score').textContent = airplaneHighScore;
      document.getElementById('airplane-game-over').style.display = 'flex';
    }

    function restartAirplaneGame() {
      document.getElementById('airplane-game-over').style.display = 'none';
      resetAirplaneGame();
      startAirplaneGame();
    }

    function adjustGameForMobile() {
      if (window.innerWidth <= 500) {
        gameWidth = 300;
        gameHeight = 300;
        maxX = 270;
        maxY = 270;
      } else {
        gameWidth = 400;
        gameHeight = 400;
        maxX = 370;
        maxY = 370;
      }
      
      playerPos.x = Math.min(playerPos.x, maxX);
      playerPos.y = Math.min(playerPos.y, maxY);
      enemyPos.x = Math.min(enemyPos.x, maxX);
      enemyPos.y = Math.min(enemyPos.y, maxY);
    }

    document.addEventListener("keydown", (e) => {
      const key = e.key.toLowerCase();
      e.preventDefault();
      
      if (document.getElementById('airplane-game-container').style.display !== 'none' && 
          document.getElementById('airplane-game-container').style.display !== '') {
        if (key === "arrowup" || key === "w") airplaneMove("up");
        if (key === "arrowdown" || key === "s") airplaneMove("down");
        if (key === "arrowleft" || key === "a") airplaneMove("left");
        if (key === "arrowright" || key === "d") airplaneMove("right");
        if (key === " " || key === "spacebar") airplaneShoot();
      } else if (gameScreen.classList.contains('active')) {
        if (key === "arrowup" || key === "w") move("up");
        if (key === "arrowdown" || key === "s") move("down");
        if (key === "arrowleft" || key === "a") move("left");
        if (key === "arrowright" || key === "d") move("right");
      }
    });

    window.addEventListener('resize', () => {
      if (gameScreen.classList.contains('active')) {
        adjustGameForMobile();
        updatePosition();
      }
    });

    function addTouchControls() {
      const goldThiefButtons = document.querySelectorAll('#controls button');
      goldThiefButtons.forEach(button => {
        button.addEventListener('touchstart', (e) => {
          e.preventDefault();
          button.click();
        });
      });

      const airplaneButtons = document.querySelectorAll('#airplane-controls button');
      airplaneButtons.forEach(button => {
        button.addEventListener('touchstart', (e) => {
          e.preventDefault();
          button.click();
        });
      });
    }

    function initializeSounds() {
      try {
        const audioContext = new (window.AudioContext || window.webkitAudioContext)();
        
        window.playBeep = function(frequency = 800, duration = 200) {
          if (audioContext.state === 'suspended') {
            audioContext.resume();
          }
          
          const oscillator = audioContext.createOscillator();
          const gainNode = audioContext.createGain();
          
          oscillator.connect(gainNode);
          gainNode.connect(audioContext.destination);
          
          oscillator.frequency.value = frequency;
          oscillator.type = 'square';
          
          gainNode.gain.setValueAtTime(0.1, audioContext.currentTime);
          gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + duration / 1000);
          
          oscillator.start(audioContext.currentTime);
          oscillator.stop(audioContext.currentTime + duration / 1000);
        };
      } catch (error) {
        window.playBeep = function() {};
      }
    }

    function optimizePerformance() {
      let lastTime = 0;
      
      function gameAnimationLoop(currentTime) {
        if (currentTime - lastTime >= 16) {
          if (gameScreen.classList.contains('active')) {
            updatePosition();
          }
          lastTime = currentTime;
        }
        requestAnimationFrame(gameAnimationLoop);
      }
      
      requestAnimationFrame(gameAnimationLoop);
    }

    function exposeGlobalFunctions() {
      window.showAirplaneGame = showAirplaneGame;
      window.showGoldThiefGame = showGoldThiefGame;
      window.showGameSelector = showGameSelector;
      window.selectRole = selectRole;
      window.selectDifficulty = selectDifficulty;
      window.backToRoleMenu = backToRoleMenu;
      window.backToMainMenu = backToMainMenu;
      window.move = move;
      window.restartGame = restartGame;
      window.startAirplaneGame = startAirplaneGame;
      window.pauseAirplaneGame = pauseAirplaneGame;
      window.airplaneMove = airplaneMove;
      window.airplaneShoot = airplaneShoot;
      window.restartAirplaneGame = restartAirplaneGame;
    }

    class GameStats {
      static increment(key) {
        try {
          const current = parseInt(sessionStorage.getItem(key) || '0');
          sessionStorage.setItem(key, (current + 1).toString());
        } catch (error) {}
      }
      
      static get(key) {
        try {
          return parseInt(sessionStorage.getItem(key) || '0');
        } catch (error) {
          return 0;
        }
      }
      
      static getGameStats() {
        return {
          goldThiefGames: this.get('goldThief_games_played'),
          airplaneGames: this.get('airplane_games_played'),
          totalCoinsCollected: this.get('goldThief_total_coins'),
          totalObstaclesDestroyed: this.get('airplane_obstacles_destroyed')
        };
      }
    }

    window.addEventListener('error', (e) => {
      console.error('Game Error:', e.error);
      if (gameInterval) {
        clearInterval(gameInterval);
      }
      if (airplaneGameActive) {
        stopAirplaneGame();
      }
    });

    document.addEventListener('DOMContentLoaded', function() {
      exposeGlobalFunctions();
      addTouchControls();
      initializeSounds();
      optimizePerformance();
      showGameSelector();
      GameStats.increment('page_loads');
      
      console.log('🎮 Persian Games Collection Loaded!');
      console.log('🎯 Games: Gold Thief & Airplane Arcade');
      console.log('👨‍💻 Created by: meti');
    });

    window.addEventListener('beforeunload', () => {
      if (gameInterval) {
        clearInterval(gameInterval);
      }
      if (airplaneGameActive) {
        stopAirplaneGame();
      }
    });

    function showGameInfo() {
      const stats = GameStats.getGameStats();
      console.log('📊 Game Statistics:');
      console.log(`   Gold Thief games played: ${stats.goldThiefGames}`);
      console.log(`   Airplane games played: ${stats.airplaneGames}`);
      console.log(`   Total coins collected: ${stats.totalCoinsCollected}`);
      console.log(`   Total obstacles destroyed: ${stats.totalObstaclesDestroyed}`);
    }

    window.showGameInfo = showGameInfo;

    function resetAllGameData() {
      try {
        HighScoreManager.clear();
        localStorage.removeItem('airplaneHighScore');
        const keys = Object.keys(sessionStorage);
        keys.forEach(key => {
          if (key.startsWith('goldThief_') || key.startsWith('airplane_') || key.includes('games_played')) {
            sessionStorage.removeItem(key);
          }
        });
        console.log('🗑️ All game data has been reset!');
        location.reload();
      } catch (error) {
        console.error('Error resetting game data:', error);
      }
    }

    window.resetAllGameData = resetAllGameData;

    // Modified Airplane Style
    const airplaneStyle = document.querySelector('#airplane');
    airplaneStyle.style.background = 'none';
    airplaneStyle.style.borderRadius = '0';
    airplaneStyle.style.clipPath = 'none';
  </script>
</body>
</html>
