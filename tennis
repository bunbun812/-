<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>テニス・レシーブ・リアクション</title>
    <style>
        :root {
            --court-color: #3e8e41;
            --line-color: #ffffff;
        }
        body {
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background-color: #f0f0f0;
            font-family: 'Helvetica Neue', Arial, sans-serif;
            overflow: hidden;
        }
        #game-container {
            position: relative;
            width: 400px;
            height: 600px;
            background-color: var(--court-color);
            border: 5px solid #2e6b30;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            display: flex;
            flex-direction: column;
            align-items: center;
            cursor: crosshair;
        }
        /* コートのライン */
        .line { position: absolute; background: var(--line-color); }
        .center-line { width: 2px; height: 100%; left: 50%; top: 0; opacity: 0.5; }
        .net { width: 100%; height: 4px; top: 50%; left: 0; background: #333; z-index: 5; }

        /* キャラクター */
        .player {
            position: absolute;
            width: 50px;
            height: 50px;
            font-size: 40px;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: transform 0.1s;
        }
        #server { top: 20px; left: 175px; }
        #receiver { bottom: 20px; left: 175px; }

        /* ボール */
        #ball {
            position: absolute;
            width: 15px;
            height: 15px;
            background-color: #ccff00;
            border-radius: 50%;
            display: none;
            box-shadow: 0 0 10px rgba(0,0,0,0.2);
            z-index: 10;
        }

        /* UI */
        #info {
            margin-top: 20px;
            text-align: center;
        }
        #message {
            position: absolute;
            top: 45%;
            width: 100%;
            text-align: center;
            font-size: 24px;
            font-weight: bold;
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
            pointer-events: none;
            z-index: 20;
        }
        .btn {
            padding: 10px 20px;
            font-size: 18px;
            background: #ff4757;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }
        .btn:hover { background: #ff6b81; }
        #stats { font-size: 1.2rem; margin-bottom: 5px; font-weight: bold; }
    </style>
</head>
<body>

    <div id="stats">記録: -- 秒</div>
    
    <div id="game-container" onclick="handleTap()">
        <div class="line center-line"></div>
        <div class="net"></div>

        <div id="server" class="player">👤</div>
        <div id="ball"></div>
        <div id="receiver" class="player">🧍</div>

        <div id="message">画面をタップしてスタート</div>
    </div>

    <div id="info">
        <p>サーバーが動いたら、レシーブ（画面タップ）して！</p>
    </div>

    <script>
        const ball = document.getElementById('ball');
        const server = document.getElementById('server');
        const message = document.getElementById('message');
        const stats = document.getElementById('stats');
        
        let gameState = 'waiting'; // waiting, ready, swinging, finished
        let startTime;
        let timeoutId;
        let randomDelay;

        function handleTap() {
            if (gameState === 'waiting' || gameState === 'finished') {
                prepareServe();
            } else if (gameState === 'ready') {
                // お手付き（早すぎた）
                clearTimeout(timeoutId);
                message.innerText = "フライング！";
                gameState = 'finished';
            } else if (gameState === 'swinging') {
                receiveBall();
            }
        }

        function prepareServe() {
            gameState = 'ready';
            message.innerText = "構えて...";
            ball.style.display = 'none';
            server.style.transform = 'translateX(0) scale(1)';
            
            // 1秒〜4秒のランダムな待ち時間
            const waitTime = Math.random() * 3000 + 1000;
            
            timeoutId = setTimeout(() => {
                fireServe();
            }, waitTime);
        }

        function fireServe() {
            gameState = 'swinging';
            message.innerText = "";
            
            // サーバーの予備動作
            server.style.transform = 'translateY(10px) scale(1.2)';
            
            // ボールの出現
            ball.style.display = 'block';
            ball.style.top = '70px';
            ball.style.left = '192px';
            
            // 指定された速さ（0.01s - 3s）をランダムに決定
            // テニスの弾丸サーブからスローボールまで
            randomDelay = Math.random() * (3.0 - 0.01) + 0.01;
            
            // アニメーション設定
            ball.style.transition = `top ${randomDelay}s linear, transform ${randomDelay}s linear`;
            
            // 記録開始
            startTime = performance.now();

            // ボールがレシーバーの位置まで移動
            setTimeout(() => {
                ball.style.top = '530px';
            }, 10);

            // 制限時間内にタップできなかった場合
            timeoutId = setTimeout(() => {
                if (gameState === 'swinging') {
                    message.innerText = "空振り！";
                    gameState = 'finished';
                }
            }, randomDelay * 1000);
        }

        function receiveBall() {
            const hitTime = performance.now();
            const reactionTime = (hitTime - startTime) / 1000;
            
            gameState = 'finished';
            clearTimeout(timeoutId);

            // ボールを打ち返す演出
            ball.style.transition = `top 0.3s ease-out`;
            ball.style.top = '-100px';

            // 結果表示
            const result = reactionTime.toFixed(3);
            stats.innerText = `記録: ${result} 秒`;
            
            if (reactionTime < 0.15) {
                message.innerText = `神レシーブ！ (${result}s)`;
            } else if (reactionTime < 0.3) {
                message.innerText = `ナイスショット！ (${result}s)`;
            } else {
                message.innerText = `リターン成功 (${result}s)`;
            }
        }
    </script>
</body>
</html>
