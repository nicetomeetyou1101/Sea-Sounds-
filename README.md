# Sea-Sounds-
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>深海迷航：沉默的受害者</title>
    <style>
        :root {
            --primary: #0077b6;
            --accent: #00b4d8;
            --dark: #03045e;
            --danger: #e63946;
            --light: #caf0f8;
            --gold: #ffd700;
        }

        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-user-select: none;
        }

        body {
            background-color: var(--dark);
            color: #fff;
            overflow: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            padding: 20px;
            text-align: center;
            overflow-y: auto;
        }

        #start-screen {
            background: linear-gradient(135deg, #03045e 0%, #0077b6 100%);
            z-index: 100;
            padding-top: 25px;
        }

        h1 {
            font-size: 2.3rem;
            margin-bottom: 5px;
            color: var(--light);
            text-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }

        .subtitle {
            font-size: 0.95rem;
            margin-bottom: 10px;
            color: var(--accent);
            max-width: 750px;
            line-height: 1.4;
        }

        .score-menu-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            margin-bottom: 15px;
        }

        .high-score-menu {
            font-size: 1.1rem;
            color: var(--gold);
            font-weight: bold;
            background: rgba(0,0,0,0.25);
            padding: 4px 16px;
            border-radius: 15px;
            border: 1px dashed var(--gold);
        }

        .certificate-btn {
            background: rgba(255, 215, 0, 0.15);
            border: 1px solid var(--gold);
            color: var(--gold);
            padding: 4px 14px;
            border-radius: 12px;
            font-size: 0.85rem;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.2s;
        }
        .certificate-btn:hover {
            background: var(--gold);
            color: #000;
            transform: scale(1.03);
        }

        .char-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            max-width: 800px;
            width: 100%;
            margin-bottom: 15px;
        }

        @media (max-width: 768px) {
            .char-grid { grid-template-columns: repeat(2, 1fr); }
            h1 { font-size: 1.8rem; }
        }
        @media (max-width: 480px) {
            .char-grid { grid-template-columns: 1fr; }
        }

        .char-card {
            background: rgba(255, 255, 255, 0.08);
            border: 3px solid transparent;
            border-radius: 12px;
            padding: 10px;
            position: relative;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .char-card.selected {
            border-color: var(--accent);
            background: rgba(0, 180, 216, 0.25);
            box-shadow: 0 0 15px rgba(0, 180, 216, 0.6);
            transform: scale(1.02);
        }

        .char-avatar {
            font-size: 2.3rem;
            margin-bottom: 3px;
        }

        .char-name {
            font-weight: bold;
            font-size: 1rem;
        }

        .char-desc {
            font-size: 0.75rem;
            color: #ddd;
            margin-top: 4px;
            line-height: 1.3;
        }

        .badge {
            position: absolute;
            top: 5px;
            right: 5px;
            font-size: 1rem;
            background: rgba(0,0,0,0.4);
            border-radius: 50%;
            width: 22px;
            height: 22px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 1px solid var(--gold);
            opacity: 0.2;
            filter: grayscale(100%);
        }

        .char-card.cleared .badge {
            opacity: 1;
            filter: grayscale(0%);
            box-shadow: 0 0 8px var(--gold);
        }

        .hint-text {
            font-size: 0.85rem;
            color: #caf0f8;
            margin-bottom: 15px;
            background: rgba(0,0,0,0.3);
            padding: 6px 18px;
            border-radius: 20px;
            line-height: 1.4;
            max-width: 750px;
        }

        /* 影片連結區塊樣式 */
        .video-box {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            padding: 8px 16px;
            border-radius: 10px;
            margin-bottom: 18px;
            font-size: 0.9rem;
        }
        .video-link {
            color: var(--accent);
            text-decoration: none;
            font-weight: bold;
            margin-left: 5px;
            transition: color 0.2s;
        }
        .video-link:hover {
            color: var(--gold);
            text-decoration: underline;
        }

        .btn {
            background-color: var(--danger);
            color: white;
            border: none;
            padding: 12px 50px;
            font-size: 1.25rem;
            font-weight: bold;
            border-radius: 30px;
            cursor: pointer;
            transition: transform 0.1s;
            box-shadow: 0 4px 15px rgba(230, 57, 70, 0.4);
            margin-bottom: 30px;
            flex-shrink: 0;
        }

        .btn:hover { transform: scale(1.05); }

        #game-container {
            position: relative;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(to bottom, #4ea8de 0%, #0077b6 40%, #023e8a 100%);
            overflow: hidden;
            display: none;
            transition: background 2.0s ease-in-out;
        }

        canvas {
            display: block;
            width: 100%;
            height: 100%;
            touch-action: none;
        }

        #knowledge-toast {
            position: absolute;
            top: -140px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            color: #111111;
            padding: 10px 22px;
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.25);
            max-width: 750px;
            width: 90%;
            z-index: 200;
            transition: top 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            text-align: left;
            border: 1px solid rgba(255, 255, 255, 0.5);
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 15px;
        }

        #knowledge-toast.show { top: 15px; }

        .toast-content {
            font-size: 0.9rem;
            line-height: 1.4;
            flex-grow: 1;
            font-weight: 500;
        }

        .toast-tag {
            color: #ffffff !important;
            font-weight: bold;
            margin-right: 8px;
            background: var(--primary);
            padding: 3px 10px;
            border-radius: 8px;
            font-size: 0.8rem;
            display: inline-block;
            vertical-align: middle;
        }

        .google-link {
            color: white;
            text-decoration: none;
            font-weight: bold;
            font-size: 0.82rem;
            background: #023e8a;
            padding: 6px 14px;
            border-radius: 15px;
            white-space: nowrap;
            transition: background 0.2s, transform 0.1s;
        }
        .google-link:hover {
            background: var(--accent);
            transform: scale(1.05);
        }

        #rt-high-score-box {
            position: absolute;
            top: 95px; 
            right: 20px;
            background: rgba(0, 0, 0, 0.4);
            padding: 5px 12px;
            border-radius: 8px;
            border-right: 4px solid var(--gold);
            text-align: right;
            z-index: 160;
            font-size: 0.85rem;
        }

        #level-up-alert {
            position: absolute;
            top: 35%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.5);
            background: rgba(230, 57, 70, 0.95);
            color: #fff;
            padding: 20px 40px;
            border-radius: 15px;
            font-size: 1.6rem;
            font-weight: bold;
            box-shadow: 0 0 30px rgba(230, 57, 70, 0.6);
            z-index: 250;
            opacity: 0;
            pointer-events: none;
            transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            text-align: center;
            width: 90%;
            max-width: 600px;
        }
        #level-up-alert.show {
            transform: translate(-50%, -50%) scale(1);
            opacity: 1;
        }

        #ui-panel {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            background: rgba(2, 62, 138, 0.95);
            border-top: 3px solid var(--accent);
            padding: 12px 25px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            z-index: 150;
        }

        .player-info {
            display: flex;
            align-items: center;
            gap:
