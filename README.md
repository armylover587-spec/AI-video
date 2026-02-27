<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Muse AI Video Generator</title>
    <style>
        :root {
            --primary: #6366f1;
            --primary-dark: #4f46e5;
            --secondary: #ec4899;
            --google-blue: #4285F4;
            --bg: #f8fafc;
            --card-bg: #ffffff;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --radius: 16px;
            --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            --safe-top: env(safe-area-inset-top, 40px);
            --safe-bottom: env(safe-area-inset-bottom, 20px);
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
            background: linear-gradient(135deg, #e0e7ff 0%, #fbcfe8 100%);
            color: var(--text-main);
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
            -webkit-touch-callout: none;
            overflow: hidden;
        }

        #app-container {
            width: min(92vw, 500px);
            height: min(90vh, 850px);
            background: var(--card-bg);
            border-radius: 30px;
            box-shadow: var(--shadow);
            display: flex;
            flex-direction: column;
            position: relative;
            overflow: hidden;
            padding: var(--safe-top) 20px var(--safe-bottom) 20px;
        }

        header {
            text-align: center;
            padding: 15px 0;
        }

        header h1 {
            margin: 0;
            font-size: clamp(1.4rem, 5vw, 1.7rem);
            font-weight: 800;
            background: linear-gradient(to right, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        header p {
            margin: 5px 0 0;
            font-size: 0.85rem;
            color: var(--text-muted);
        }

        .content {
            flex: 1;
            overflow-y: auto;
            padding: 10px 5px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .section-label {
            font-size: 0.8rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            color: var(--text-muted);
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .upload-container {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .source-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }

        .source-btn {
            padding: 12px;
            border-radius: 12px;
            border: 1.5px solid #e2e8f0;
            background: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
            cursor: pointer;
            transition: all 0.2s ease;
            font-size: 0.85rem;
            font-weight: 600;
        }

        .source-btn:active {
            transform: scale(0.95);
            background: #f1f5f9;
        }

        .source-btn.google {
            border-color: #4285F433;
        }

        .image-grid-container {
            min-height: 100px;
            border: 2px dashed #cbd5e1;
            border-radius: var(--radius);
            padding: 10px;
            background: #f8fafc;
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            align-content: flex-start;
        }

        .image-grid-container.has-items {
            border-style: solid;
            border-color: var(--primary);
        }

        .img-thumb-wrapper {
            position: relative;
            width: calc(25% - 6px);
            aspect-ratio: 1;
        }

        .img-thumb {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .remove-img {
            position: absolute;
            top: -5px;
            right: -5px;
            background: #ef4444;
            color: white;
            width: 18px;
            height: 18px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }

        .empty-grid-msg {
            width: 100%;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--text-muted);
            font-size: 0.85rem;
            text-align: center;
        }

        .script-container {
            display: flex;
            flex-direction: column;
        }

        textarea {
            width: 100%;
            min-height: 120px;
            border: 1.5px solid #e2e8f0;
            border-radius: var(--radius);
            padding: 15px;
            font-family: inherit;
            font-size: 1rem;
            resize: none;
            outline: none;
            transition: border-color 0.3s;
            background: #fff;
            color: var(--text-main);
        }

        textarea:focus {
            border-color: var(--primary);
        }

        .btn {
            width: 100%;
            padding: 16px;
            border: none;
            border-radius: var(--radius);
            font-size: 1rem;
            font-weight: 700;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-dark) 100%);
            color: white;
        }

        .btn-primary:active {
            transform: translateY(2px);
            box-shadow: none;
        }

        .btn-primary:disabled {
            background: #cbd5e1;
            cursor: not-allowed;
        }

        /* Modal for Google Link */
        #google-modal {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.6);
            backdrop-filter: blur(4px);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 300;
            padding: 20px;
        }

        .modal-content {
            background: white;
            width: 100%;
            max-width: 400px;
            border-radius: 20px;
            padding: 25px;
            box-shadow: var(--shadow);
        }

        .modal-content h3 {
            margin: 0 0 15px 0;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .modal-content input {
            width: 100%;
            padding: 12px;
            border: 1.5px solid #e2e8f0;
            border-radius: 10px;
            margin-bottom: 15px;
            outline: none;
        }

        .modal-actions {
            display: flex;
            gap: 10px;
        }

        #processing-overlay {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(255, 255, 255, 0.98);
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 400;
            padding: 40px;
            text-align: center;
        }

        .loader {
            width: 50px;
            height: 50px;
            border: 4px solid #f3f3f3;
            border-top: 4px solid var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 20px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .progress-bar {
            width: 100%;
            height: 6px;
            background: #e2e8f0;
            border-radius: 3px;
            margin: 20px 0;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: var(--primary);
            width: 0%;
            transition: width 0.3s;
        }

        #preview-modal {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: #000;
            display: none;
            flex-direction: column;
            z-index: 500;
        }

        .preview-header {
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: white;
            background: rgba(0,0,0,0.5);
            position: absolute;
            top: 0;
            width: 100%;
            z-index: 10;
        }

        .preview-screen {
            flex: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }

        #preview-img {
            max-width: 100%;
            max-height: 100%;
            object-fit: contain;
            transition: opacity 0.4s ease-in-out;
        }

        .preview-controls {
            padding: 20px;
            background: rgba(0,0,0,0.9);
            display: flex;
            gap: 10px;
            padding-bottom: calc(20px + var(--safe-bottom));
        }

        .close-btn {
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
        }

        .hidden-input {
            display: none;
        }

        ::-webkit-scrollbar {
            width: 4px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 10px;
        }
    </style>
</head>
<body>

    <div id="app-container">
        <header>
            <h1>Muse AI Video 🎬</h1>
            <p>Create cinematic stories from photos</p>
        </header>

        <div class="content">
            <div class="section">
                <div class="section-label"><span>🖼️</span> Visual Assets</div>
                <div class="upload-container">
                    <div class="source-buttons">
                        <div class="source-btn" onclick="document.getElementById('fileInput').click()">
                            <span style="font-size: 1.5rem;">📱</span>
                            <span>Device</span>
                        </div>
                        <div class="source-btn google" onclick="openGoogleModal()">
                            <span style="font-size: 1.5rem;">🌐</span>
                            <span>Google/Web</span>
                        </div>
                    </div>
                    
                    <div class="image-grid-container" id="imageGrid">
                        <div class="empty-grid-msg" id="emptyMsg">No photos added yet</div>
                    </div>
                </div>
                <input type="file" id="fileInput" class="hidden-input" multiple accept="image/*">
            </div>

            <div class="section script-container">
                <div class="section-label"><span>📝</span> Story Script</div>
                <textarea id="etScript" placeholder="Describe your story... e.g., 'A beautiful sunset over the mountains, followed by a peaceful forest walk...'"></textarea>
            </div>

            <div style="margin-top: auto; padding-bottom: 5px;">
                <button class="btn btn-primary" id="btnGenerate">
                    <span>✨</span> Generate AI Video
                </button>
            </div>
        </div>

        <!-- Google Link Modal -->
        <div id="google-modal">
            <div class="modal-content">
                <h3><span>🌐</span> Add from Google/Web</h3>
                <p style="font-size: 0.85rem; color: var(--text-muted); margin-bottom: 15px;">Paste a direct image URL from Google Photos or the web.</p>
                <input type="text" id="urlInput" placeholder="https://example.com/image.jpg">
                <div class="modal-actions">
                    <button class="btn" style="background: #f1f5f9;" onclick="closeGoogleModal()">Cancel</button>
                    <button class="btn btn-primary" onclick="addFromUrl()">Add Image</button>
                </div>
            </div>
        </div>

        <!-- Processing Overlay -->
        <div id="processing-overlay">
            <div class="loader"></div>
            <h2 id="status-text" style="margin: 0; font-size: 1.2rem;">Synthesizing...</h2>
            <p id="status-subtext" style="color: var(--text-muted); font-size: 0.9rem; margin-top: 8px;">Preparing AI models</p>
            <div class="progress-bar">
                <div class="progress-fill" id="progressBar"></div>
            </div>
        </div>

        <!-- Preview Modal -->
        <div id="preview-modal">
            <div class="preview-header">
                <span style="font-weight: bold;">Video Preview</span>
                <button class="close-btn" onclick="closePreview()">✕</button>
            </div>
            <div class="preview-screen">
                <img id="preview-img" src="" alt="Preview">
            </div>
            <div class="preview-controls">
                <button class="btn btn-primary" style="flex: 1;" onclick="closePreview()">
                    <span>💾</span> Save Video
                </button>
                <button class="btn" style="background: #333; color: white;" onclick="closePreview()">
                    <span>🔄</span> Edit
                </button>
            </div>
        </div>
    </div>

    <script>
        const APP_STORAGE_KEY = 'MUSE_AI_VIDEO_APP_V2';
        window.APP_STORAGE_KEY = APP_STORAGE_KEY;

        let selectedImages = [];
        let currentPreviewIndex = 0;
        let previewInterval = null;
        const synth = window.speechSynthesis;

        // DOM Elements
        const fileInput = document.getElementById('fileInput');
        const imageGrid = document.getElementById('imageGrid');
        const emptyMsg = document.getElementById('emptyMsg');
        const etScript = document.getElementById('etScript');
        const btnGenerate = document.getElementById('btnGenerate');
        const processingOverlay = document.getElementById('processing-overlay');
        const progressBar = document.getElementById('progressBar');
        const statusText = document.getElementById('status-text');
        const statusSubtext = document.getElementById('status-subtext');
        const previewModal = document.getElementById('preview-modal');
        const previewImg = document.getElementById('preview-img');
        const googleModal = document.getElementById('google-modal');
        const urlInput = document.getElementById('urlInput');

        // Prevent double-click zoom
        document.body.addEventListener('dblclick', function(event) {
            event.preventDefault();
        });

        // Load saved data
        window.addEventListener('load', () => {
            const saved = localStorage.getItem(APP_STORAGE_KEY + '_data');
            if (saved) {
                const data = JSON.parse(saved);
                etScript.value = data.script || '';
            }
        });

        // Save script on change
        etScript.addEventListener('input', () => {
            const data = { script: etScript.value };
            localStorage.setItem(APP_STORAGE_KEY + '_data', JSON.stringify(data));
        });

        // Handle Device Selection
        fileInput.addEventListener('change', (e) => {
            const files = Array.from(e.target.files);
            if (files.length === 0) return;

            files.forEach(file => {
                const reader = new FileReader();
                reader.onload = (event) => {
                    addImageToCollection(event.target.result);
                };
                reader.readAsDataURL(file);
            });
            fileInput.value = ''; // Reset for next selection
        });

        function openGoogleModal() {
            googleModal.style.display = 'flex';
            urlInput.focus();
        }

        function closeGoogleModal() {
            googleModal.style.display = 'none';
            urlInput.value = '';
        }

        function addFromUrl() {
            const url = urlInput.value.trim();
            if (url) {
                // Simple validation for image-like URL
                if (url.match(/\.(jpeg|jpg|gif|png|webp)$/) || url.startsWith('data:image')) {
                    addImageToCollection(url);
                    closeGoogleModal();
                } else {
                    alert("Please enter a valid image URL (ending in .jpg, .png, etc.)");
                }
            }
        }

        function addImageToCollection(src) {
            selectedImages.push(src);
            renderGrid();
        }

        function removeImage(index) {
            selectedImages.splice(index, 1);
            renderGrid();
        }

        function renderGrid() {
            // Clear grid except for the empty message if needed
            imageGrid.innerHTML = '';
            
            if (selectedImages.length === 0) {
                imageGrid.classList.remove('has-items');
                imageGrid.appendChild(emptyMsg);
                return;
            }

            imageGrid.classList.add('has-items');
            selectedImages.forEach((src, index) => {
                const wrapper = document.createElement('div');
                wrapper.className = 'img-thumb-wrapper';
                
                const img = document.createElement('img');
                img.src = src;
                img.className = 'img-thumb';
                
                const removeBtn = document.createElement('div');
                removeBtn.className = 'remove-img';
                removeBtn.innerHTML = '✕';
                removeBtn.onclick = (e) => {
                    e.stopPropagation();
                    removeImage(index);
                };
                
                wrapper.appendChild(img);
                wrapper.appendChild(removeBtn);
                imageGrid.appendChild(wrapper);
            });
        }

        // Generate Video Logic
        btnGenerate.addEventListener('click', () => {
            const script = etScript.value.trim();
            
            if (selectedImages.length === 0) {
                alert('Please add at least one photo from your device or Google.');
                return;
            }
            if (!script) {
                alert('Please write a story script for the AI to narrate.');
                return;
            }

            startGeneration(script);
        });

        function startGeneration(script) {
            processingOverlay.style.display = 'flex';
            updateProgress(10, "Analyzing Script...", "Processing your story text");

            setTimeout(() => {
                updateProgress(40, "Synthesizing Voice...", "Generating AI narration");
                
                const utterance = new SpeechSynthesisUtterance(script);
                utterance.rate = 0.95;
                utterance.pitch = 1.0;

                utterance.onstart = () => {
                    updateProgress(70, "Rendering Video...", "Syncing photos with audio");
                };

                utterance.onend = () => {
                    updateProgress(100, "Finalizing...", "Encoding video file");
                    setTimeout(() => {
                        showPreview();
                    }, 800);
                };

                // Simulate processing delay
                setTimeout(() => {
                    synth.speak(utterance);
                }, 1200);

            }, 1000);
        }

        function updateProgress(percent, title, sub) {
            progressBar.style.width = percent + '%';
            statusText.innerText = title;
            statusSubtext.innerText = sub;
        }

        function showPreview() {
            processingOverlay.style.display = 'none';
            previewModal.style.display = 'flex';
            
            currentPreviewIndex = 0;
            previewImg.src = selectedImages[0];
            
            // Start slideshow
            const durationPerImage = 3500;
            previewInterval = setInterval(() => {
                currentPreviewIndex = (currentPreviewIndex + 1) % selectedImages.length;
                previewImg.style.opacity = 0;
                setTimeout(() => {
                    previewImg.src = selectedImages[currentPreviewIndex];
                    previewImg.style.opacity = 1;
                }, 400);
            }, durationPerImage);
        }

        function closePreview() {
            clearInterval(previewInterval);
            synth.cancel();
            previewModal.style.display = 'none';
            progressBar.style.width = '0%';
        }

        // Android WebView Focus Safety
        document.addEventListener("focusin", function(e) {
            if (e.target.tagName === "INPUT" && e.target.type !== "text" && e.target.type !== "file") {
                setTimeout(() => e.target.blur(), 100);
            }
        });

    </script>
