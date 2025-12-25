# Merry-Christmas-project
Merry Christmas!!!
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>圣诞快乐特效</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background: linear-gradient(180deg, #0c1a2d 0%, #1a3a5f 100%);
            color: white;
            font-family: 'Microsoft YaHei', sans-serif;
            min-height: 100vh;
            overflow-x: hidden;
            text-align: center;
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding-top: 30px;
        }
        
        h1 {
            color: #ff6b6b;
            margin-bottom: 20px;
            text-shadow: 0 0 10px rgba(255, 107, 107, 0.5);
        }
        
        .instructions {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 20px;
            margin: 20px 0;
            backdrop-filter: blur(5px);
        }
        
        .trigger-area {
            margin: 30px 0;
        }
        
        #christmasInput {
            width: 80%;
            max-width: 400px;
            padding: 15px;
            font-size: 18px;
            border: none;
            border-radius: 50px;
            text-align: center;
            background: rgba(255, 255, 255, 0.9);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        #triggerBtn {
            background: linear-gradient(45deg, #ff6b6b, #ff8e53);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 18px;
            border-radius: 50px;
            margin-top: 20px;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(255, 107, 107, 0.4);
            transition: transform 0.3s;
        }
        
        #triggerBtn:hover {
            transform: scale(1.05);
        }
        
        .falling-area {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 100;
        }
        
        .tree {
            position: absolute;
            font-size: 40px;
            animation: fall linear forwards;
            user-select: none;
        }
        
        .snowflake {
            position: absolute;
            color: #e6f7ff;
            font-size: 24px;
            animation: fall linear forwards, sway 2s ease-in-out infinite;
            user-select: none;
        }
        
        @keyframes fall {
            to {
                transform: translateY(100vh);
            }
        }
        
        @keyframes sway {
            0%, 100% { transform: translateX(0); }
            50% { transform: translateX(20px); }
        }
        
        .message {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 48px;
            color: #ffcc00;
            text-shadow: 0 0 20px rgba(255, 204, 0, 0.8);
            opacity: 0;
            z-index: 1000;
            pointer-events: none;
            animation: popUp 2s ease-out;
        }
        
        @keyframes popUp {
            0% { opacity: 0; transform: translate(-50%, -50%) scale(0.5); }
            20% { opacity: 1; transform: translate(-50%, -50%) scale(1.2); }
            40% { transform: translate(-50%, -50%) scale(1); }
            80% { opacity: 1; }
            100% { opacity: 0; }
        }
        
        .footer {
            margin-top: 40px;
            color: rgba(255, 255, 255, 0.6);
            font-size: 14px;
        }
        
        .share-hint {
            background: rgba(76, 175, 80, 0.2);
            border: 1px solid rgba(76, 175, 80, 0.5);
            border-radius: 10px;
            padding: 15px;
            margin: 20px 0;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎄 圣诞快乐特效 🎄</h1>
        
        <div class="instructions">
            <p>✨ <strong>使用说明：</strong></p>
            <p>1. 在下方输入"圣诞快乐"并点击按钮</p>
            <p>2. 或者直接在页面任意位置点击触发特效</p>
            <p>3. 特效会在当前页面显示掉落的圣诞树和雪花</p>
        </div>
        
        <div class="share-hint">
            💡 提示：点击右上角"..."分享到微信群，让朋友一起体验！
        </div>
        
        <div class="trigger-area">
            <input 
                type="text" 
                id="christmasInput" 
                placeholder="输入'圣诞快乐'触发特效"
                autocomplete="off"
            >
            <br>
            <button id="triggerBtn">点击触发圣诞特效</button>
        </div>
        
        <div class="falling-area" id="fallingArea"></div>
        
        <div class="footer">
            <p>🎅 祝您和群友们圣诞快乐！</p>
            <p>特效会在微信浏览器中自动适配</p>
        </div>
    </div>

    <script>
        // 触发特效的函数
        function triggerChristmasEffect() {
            // 显示祝福语
            showMessage();
            
            // 创建圣诞树和雪花
            createFallingElements();
            
            // 播放音效（微信中需要用户交互才能播放）
            playSound();
        }
        
        // 显示祝福语
        function showMessage() {
            const message = document.createElement('div');
            message.className = 'message';
            message.textContent = '🎅 圣诞快乐！';
            document.body.appendChild(message);
            
            // 2秒后移除
            setTimeout(() => {
                message.remove();
            }, 2000);
        }
        
        // 创建掉落元素
        function createFallingElements() {
            const fallingArea = document.getElementById('fallingArea');
            const treeEmojis = ['🎄', '🌲', '🎅', '🤶', '🦌'];
            const snowflakeEmojis = ['❄️', '🌨️', '✨', '⭐', '🌟'];
            
            // 创建10-15棵圣诞树
            const treeCount = 10 + Math.floor(Math.random() * 6);
            for (let i = 0; i < treeCount; i++) {
                setTimeout(() => {
                    const tree = document.createElement('div');
                    tree.className = 'tree';
                    tree.textContent = treeEmojis[Math.floor(Math.random() * treeEmojis.length)];
                    tree.style.left = Math.random() * 100 + 'vw';
                    tree.style.fontSize = (30 + Math.random() * 30) + 'px';
                    tree.style.animationDuration = (3 + Math.random() * 4) + 's';
                    tree.style.opacity = 0.8 + Math.random() * 0.2;
                    
                    fallingArea.appendChild(tree);
                    
                    // 动画结束后移除元素
                    setTimeout(() => {
                        tree.remove();
                    }, 7000);
                }, i * 300);
            }
            
            // 创建50-80个雪花
            const snowflakeCount = 50 + Math.floor(Math.random() * 31);
            for (let i = 0; i < snowflakeCount; i++) {
                setTimeout(() => {
                    const snowflake = document.createElement('div');
                    snowflake.className = 'snowflake';
                    snowflake.textContent = snowflakeEmojis[Math.floor(Math.random() * snowflakeEmojis.length)];
                    snowflake.style.left = Math.random() * 100 + 'vw';
                    snowflake.style.fontSize = (15 + Math.random() * 20) + 'px';
                    snowflake.style.animationDuration = (5 + Math.random() * 6) + 's';
                    snowflake.style.opacity = 0.5 + Math.random() * 0.5;
                    
                    fallingArea.appendChild(snowflake);
                    
                    // 动画结束后移除元素
                    setTimeout(() => {
                        snowflake.remove();
                    }, 11000);
                }, i * 100);
            }
        }
        
        // 播放音效
        function playSound() {
            // 微信中需要用户交互才能播放音频，这里只创建但不自动播放
            // 实际使用中可以添加圣诞音乐
            console.log('🎵 圣诞音乐（微信中需要用户交互才能自动播放）');
        }
        
        // 事件监听
        document.getElementById('triggerBtn').addEventListener('click', triggerChristmasEffect);
        
        document.getElementById('christmasInput').addEventListener('keyup', function(e) {
            if (e.target.value.includes('圣诞快乐') || e.target.value.includes('圣诞节')) {
                triggerChristmasEffect();
                e.target.value = '';
            }
        });
        
        // 点击页面任意位置也能触发
        document.body.addEventListener('click', function(e) {
            if (e.target.id !== 'christmasInput' && e.target.id !== 'triggerBtn') {
                // 随机触发
                if (Math.random() > 0.7) {
                    triggerChristmasEffect();
                }
            }
        });
        
        // 页面加载后自动提示
        window.addEventListener('load', function() {
            setTimeout(() => {
                alert('🎄 圣诞快乐！在输入框输入"圣诞快乐"或点击按钮触发特效！');
            }, 500);
        });
        
        // 适配微信浏览器
        function isWeChatBrowser() {
            return /MicroMessenger/i.test(navigator.userAgent);
        }
        
        if (isWeChatBrowser()) {
            document.querySelector('.share-hint').style.display = 'block';
            
            // 微信中禁用长按菜单
            document.addEventListener('contextmenu', function(e) {
                e.preventDefault();
                return false;
            });
        }
    </script>
</body>
</html>
