<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Lustfully Edible</title>
    <style>
        :root {
            --primary-bg: #121212;
            --secondary-bg: #1e1e1e;
            --tertiary-bg: #2a2a2a;
            --primary-text: #e0e0e0;
            --secondary-text: #b0b0b0;
            --accent-color: #ff007f;
            --accent-hover: #ff4da6;
            --success-color: #4caf50;
            --error-color: #f44336;
            --pass-color: #616161;
            --border-radius: 16px;
            --font-family: 'Helvetica', 'Arial', sans-serif;
            --neon-pink-glow: 0 0 5px var(--accent-color), 0 0 10px var(--accent-color), 0 0 20px var(--accent-color), 0 0 30px #ff007f;
            /* Avatar CSS Vars */
            --avatar-skin: #f0d0b0;
            --avatar-hair: #000000;
            --avatar-eyes: #4a90e2;
            --avatar-cyber: none;
            /* Genital CSS Vars */
            --penis-length: 5;
            --penis-girth: 1.5;
            --penis-bend: straight;
            --vagina-hood: tucked;
            --pubic-hair: trimmed;
            /* NEW: Clothing CSS Vars */
            --clothing-top: none; /* e.g., 'shirt' */
            --clothing-bottom: none; /* e.g., 'pants' */
            --clothing-accessory: none; /* e.g., 'hat' */
            --nudity-visible: false; /* Toggle for matches */
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        html {
            font-size: 16px;
        }

        body {
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            background-color: var(--primary-bg);
            font-family: var(--font-family);
            color: var(--primary-text);
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
            -webkit-touch-callout: none;
            overflow: hidden;
        }

        .app-shell {
            width: min(100vw, 420px);
            height: 100vh;
            max-height: 100vh;
            background-color: var(--primary-bg);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            position: relative;
            border: 1px solid #333;
        }
        @media (min-width: 480px) {
            .app-shell {
                border-radius: 24px;
                height: min(100vh, 880px);
            }
        }

        .view {
            display: none;
            flex-direction: column;
            height: 100%;
            width: 100%;
            padding: env(safe-area-inset-top, 20px) env(safe-area-inset-right, 15px) calc(80px + env(safe-area-inset-bottom, 10px)) env(safe-area-inset-left, 15px);
            overflow-y: auto;
            animation: fadeIn 0.3s ease-in-out;
        }

        .view.active {
            display: flex;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.98); }
            to { opacity: 1; transform: scale(1); }
        }

        /* Avatar View Styles */
        #avatar-view {
            gap: 1.5rem;
            align-items: center;
            justify-content: flex-start;
            padding-top: 2rem;
        }
        #avatar-view h2 {
            text-align: center;
            color: var(--accent-color);
            text-shadow: var(--neon-pink-glow);
            margin: 0;
        }
        .avatar-builder {
            display: flex;
            flex-direction: column;
            gap: 1rem;
            width: 100%;
            background-color: var(--secondary-bg);
            padding: 1.5rem;
            border-radius: var(--border-radius);
        }
        .avatar-preview {
            width: 100%;
            height: 250px; /* Taller for clothing */
            border-radius: var(--border-radius);
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #3a3a3a, #1a1a1a);
            position: relative;
            overflow: hidden;
            border: 2px solid var(--tertiary-bg);
            transition: all 0.3s ease;
            font-size: 4rem;
        }
        .avatar-preview::before {
            content: '👤';
            z-index: 2;
            filter: drop-shadow(0 0 5px var(--avatar-skin));
        }
        /* NEW: Clothing Overlays (Pseudo-elements for simple layers) */
        .avatar-preview::after {
            content: '';
            position: absolute;
            bottom: 20%;
            left: 50%;
            transform: translateX(-50%);
            font-size: 2rem;
            opacity: 0.8;
            z-index: 3;
        }
        /* Top (Shirt) */
        body:has([data-top="shirt"]) .avatar-preview::before {
            content: '👕' !important; /* Override body for top */
            font-size: 3rem;
            top: 20%;
        }
        /* Bottom (Pants) */
        body:has([data-bottom="pants"]) .avatar-preview::after {
            content: '👖';
            bottom: 10%;
        }
        /* Accessory (Hat) */
        body:has([data-accessory="hat"]) .avatar-preview::before {
            content: '🧢👤'; /* Stack hat + body */
        }
        /* Genital Previews (Hidden if clothed) */
        .avatar-preview.clothed::after { content: none; } /* Hide if dressed */
        /* Male genital preview */
        body:has(#male-genitals.active):not(.clothed) .avatar-preview::after {
            content: '🍆';
            filter: hue-rotate(var(--penis-bend) === 'right' ? 10deg : var(--penis-bend) === 'left' ? -10deg : 0deg);
        }
        /* Female genital preview */
        body:has(#female-genitals.active):not(.clothed) .avatar-preview::after {
            content: '🌸';
        }
        .avatar-preview.cyber::after {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: linear-gradient(45deg, transparent 30%, rgba(255,0,127,0.1) 50%, transparent 70%);
            animation: glitch 1s linear infinite;
            z-index: 1;
        }
        @keyframes glitch {
            0%, 100% { transform: translate(0); }
            20% { transform: translate(-2px, 2px); }
            40% { transform: translate(-2px, -2px); }
            60% { transform: translate(2px, 2px); }
            80% { transform: translate(2px, -2px); }
        }
        .avatar-trait {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 1rem;
        }
        .avatar-trait select, .avatar-trait input[type="file"], .avatar-trait input[type="range"] {
            flex: 1;
            padding: 0.5rem;
            background-color: var(--tertiary-bg);
            border: 1px solid #444;
            border-radius: 8px;
            color: var(--primary-text);
        }
        .avatar-trait input[type="range"] { 
            padding: 0; 
            height: 20px; 
        }
        .avatar-trait label {
            font-size: 0.9rem;
            color: var(--secondary-text);
            min-width: 120px;
        }
        #avatar-photo-upload {
            opacity: 0;
            position: absolute;
        }
        #avatar-photo-upload + label {
            cursor: pointer;
            padding: 0.5rem 1rem;
            background-color: var(--accent-color);
            color: white;
            border-radius: 8px;
            text-align: center;
            transition: background-color 0.2s;
            flex: 1;
        }
        #avatar-photo-upload + label:hover {
            background-color: var(--accent-hover);
        }
        .avatar-preview.has-photo {
            background-size: cover;
            background-position: center;
        }
        .avatar-preview.has-photo::before {
            content: none;
        }
        .genital-section, .clothing-section {
            display: none;
            flex-direction: column;
            gap: 0.75rem;
            padding: 1rem;
            background-color: var(--tertiary-bg);
            border-radius: 12px;
            border: 1px solid var(--accent-color);
            box-shadow: var(--neon-pink-glow);
        }
        .genital-section.active, .clothing-section.active {
            display: flex;
        }
        .genital-subtrait, .clothing-subtrait {
            display: flex;
            flex-direction: column;
            gap: 0.25rem;
        }
        .genital-subtrait label, .clothing-subtrait label {
            font-size: 0.8rem;
            color: var(--accent-color);
            text-transform: uppercase;
        }
        .genital-subtrait span, .clothing-subtrait span {
            font-size: 0.75rem;
            color: var(--secondary-text);
            text-align: right;
        }
        /* NEW: Nudity Toggle */
        .nudity-toggle {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            font-size: 0.9rem;
            color: var(--secondary-text);
        }
        .nudity-toggle .switch { width: 40px; height: 22px; }
        .nudity-toggle .switch input + .slider:before { height: 16px; width: 16px; }
        input:checked + .slider:before { transform: translateX(18px); }

        /* Profile Edit Wardrobe Btn */
        .edit-wardrobe-btn {
            width: 100%;
            padding: 0.8rem;
            background: linear-gradient(45deg, var(--accent-color), #c70065);
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        .edit-wardrobe-btn:hover { transform: translateY(-2px); box-shadow: var(--neon-pink-glow); }

        /* Rest of styles unchanged... (age-gate, auth, swipe, etc.) */
        #age-gate-view {
            justify-content: center;
            align-items: center;
            text-align: center;
            gap: 2rem;
            background: linear-gradient(135deg, #2c0b1e, #121212);
        }
        #age-gate-view h1 {
            font-size: clamp(2rem, 8vw, 3rem);
            color: var(--accent-color);
            text-shadow: var(--neon-pink-glow);
        }
        #age-gate-view p {
            font-size: clamp(1rem, 4vw, 1.2rem);
            color: var(--secondary-text);
            max-width: 80%;
        }

        #auth-view {
            justify-content: center;
            gap: 1.5rem;
        }
        .auth-container {
            width: 100%;
            padding: 2rem;
            background-color: var(--secondary-bg);
            border-radius: var(--border-radius);
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }
        .auth-toggle {
            display: flex;
            border: 1px solid var(--tertiary-bg);
            border-radius: var(--border-radius);
            overflow: hidden;
        }
        .auth-toggle button {
            flex: 1;
            padding: 0.8rem;
            background: none;
            border: none;
            color: var(--secondary-text);
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .auth-toggle button.active {
            background-color: var(--accent-color);
            color: white;
            font-weight: bold;
        }
        .form-section { display: none; flex-direction: column; gap: 1rem; }
        .form-section.active { display: flex; }

        .app-content {
            flex-grow: 1;
            overflow: hidden;
            position: relative;
        }

        #swipe-view {
            padding: 10px 10px calc(80px + env(safe-area-inset-bottom, 10px)) 10px;
            overflow: hidden;
        }
        .swipe-deck {
            position: relative;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .profile-card {
            position: absolute;
            width: 95%;
            height: 98%;
            background-color: var(--secondary-bg);
            border-radius: var(--border-radius);
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            cursor: grab;
            touch-action: none;
            transition: transform 0.3s ease, opacity 0.3s ease, box-shadow 0.3s ease;
            border: 2px solid transparent;
            will-change: transform;
        }
        .profile-card.flirting {
            border-color: var(--accent-color);
            box-shadow: var(--neon-pink-glow);
        }
        .profile-card.dragging {
            cursor: grabbing;
            transition: none;
        }
        .profile-card:nth-child(2) { transform: translateY(10px) scale(0.95); z-index: 2; }
        .profile-card:nth-child(3) { transform: translateY(20px) scale(0.9); z-index: 1; }
        .profile-card .profile-pic {
            height: 60%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 8rem;
            background: linear-gradient(135deg, #3a3a3a, #1a1a1a);
        }
        .profile-card .profile-info {
            height: 40%;
            padding: 1rem;
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(5px);
            overflow-y: auto;
        }
        .profile-info h3 { margin: 0 0 0.5rem 0; font-size: 1.5rem; }
        .profile-info p { margin: 0 0 1rem 0; color: var(--secondary-text); }
        .profile-info .info-section { margin-bottom: 1rem; }
        .profile-info .info-section h4 { margin: 0 0 0.25rem; color: var(--accent-color); font-size: 0.9rem; text-transform: uppercase; }
        .profile-info .info-section p { margin: 0; color: var(--primary-text); font-size: 1rem; line-height: 1.4; }

        .swipe-actions {
            position: absolute;
            bottom: calc(85px + env(safe-area-inset-bottom, 10px));
            left: 0;
            width: 100%;
            display: flex;
            justify-content: center;
            gap: 1.5rem;
            z-index: 10;
        }
        .swipe-actions button {
            width: 70px;
            height: 70px;
            border-radius: 50%;
            border: 2px solid var(--tertiary-bg);
            background-color: var(--secondary-bg);
            color: white;
            font-size: 2.5rem;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: all 0.2s ease;
            box-shadow: 0 4px 8px rgba(0,0,0,0.4);
        }
        .swipe-actions button:hover,
        .swipe-actions button:active { transform: scale(1.1); }
        .swipe-actions .swipe-like { border-color: var(--accent-color); color: var(--accent-color); }
        .swipe-actions .swipe-pass { border-color: var(--pass-color); color: var(--pass-color); }

        #matches-view { gap: 1rem; }
        .matches-list { display: flex; flex-direction: column; gap: 0.75rem; }
        .match-item {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 0.75rem;
            background-color: var(--secondary-bg);
            border-radius: var(--border-radius);
            cursor: pointer;
            transition: background-color 0.2s ease;
        }
        .match-item:hover { background-color: var(--tertiary-bg); }
        .match-avatar {
            font-size: 2.5rem;
            width: 50px;
            height: 50px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 50%;
            background-color: var(--tertiary-bg);
        }
        .match-info h4 { margin: 0; font-size: 1.1rem; }
        .match-info p { margin: 0.25rem 0 0; color: var(--secondary-text); font-size: 0.9rem; }

        #profile-view { align-items: center; gap: 1.5rem; }
        .profile-header { text-align: center; }
        .profile-avatar-wrapper {
            position: relative;
            padding: 5px;
            border-radius: 50%;
            transition: box-shadow 0.3s ease-in-out;
            width: 130px;
            height: 130px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .profile-avatar-wrapper.flirting { box-shadow: var(--neon-pink-glow); }
        .profile-avatar {
            font-size: 6rem;
            width: 120px;
            height: 120px;
            border-radius: 50%;
            background: linear-gradient(135deg, #3a3a3a, #1a1a1a);
            display: flex;
            justify-content: center;
            align-items: center;
            border: 3px solid var(--tertiary-bg);
            position: relative; /* For clothing overlays in profile */
        }
        /* Profile Clothing Stubs (Same as preview) */
        body:has([data-top="shirt"]) .profile-avatar::before { content: '👕'; }
        .profile-details, .profile-section {
            width: 100%;
            background-color: var(--secondary-bg);
            padding: 1.5rem;
            border-radius: var(--border-radius);
            text-align: center;
        }
        .profile-section { text-align: left; padding: 1rem 1.5rem; }
        .profile-section h4 { margin: 0 0 0.5rem; color: var(--accent-color); }
        .profile-section p { margin: 0; color: var(--secondary-text); line-height: 1.5; }
        .profile-details h2 { margin: 0 0 0.5rem; }
        .profile-details p { margin: 0; color: var(--secondary-text); }

        .profile-setting {
            width: 100%;
            background-color: var(--secondary-bg);
            padding: 1rem 1.5rem;
            border-radius: var(--border-radius);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .switch { position: relative; display: inline-block; width: 50px; height: 28px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: var(--tertiary-bg); transition: .4s; border-radius: 34px; }
        .slider:before { position: absolute; content: ""; height: 20px; width: 20px; left: 4px; bottom: 4px; background-color: white; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--accent-color); }
        input:checked + .slider:before { transform: translateX(22px); }

        .flirt-button {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            padding: 1rem 2rem;
            font-size: 1.1rem;
            font-weight: bold;
            background: linear-gradient(45deg, var(--accent-color), #c70065);
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(255, 0, 127, 0.4);
        }
        .flirt-button:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(255, 0, 127, 0.6); }
        .flirt-button.active { background: linear-gradient(45deg, #4caf50, #388e3c); box-shadow: 0 4px 15px rgba(76, 175, 80, 0.4); }

        #map-view { gap: 1rem; }
        #map-container {
            position: relative;
            width: 100%;
            flex-grow: 1;
            height: 400px;
            background: radial-gradient(ellipse at bottom, #1b2735 0%, #090a0f 100%);
            border-radius: var(--border-r