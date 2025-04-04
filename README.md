# 123<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CMY Image Processing Tool</title>
    <style>
        /* Styles remain unchanged */
        :root {
            --primary: #4285f4;
            --success: #34a853;
            --danger: #ea4335;
            --light: #f5f5f5;
            --dark: #333;
            --border: #ddd;
            --warning: #fbbc05;
            --info: #17a2b8;
            --purple: #9c27b0;
        }
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        body {
            font-family: 'Segoe UI', Tahoma, sans-serif;
            line-height: 1.6;
            color: var(--dark);
            background: #f9f9f9;
            padding: 20px;
        }
        .container {
            max-width: 1800px;
            margin: 0 auto;
            padding: 20px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        header {
            text-align: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid var(--border);
        }
        h1 {
            color: var(--primary);
            margin-bottom: 10px;
        }
        .window-container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-bottom: 20px;
        }
        .window {
            flex: 1;
            min-width: 300px;
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 20px;
            background: white;
            position: relative;
        }
        .window-title {
            position: absolute;
            top: -10px;
            left: 20px;
            background: white;
            padding: 0 10px;
            font-weight: bold;
            color: var(--dark);
        }
        h2 {
            color: var(--success);
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid var(--border);
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }
        input[type="file"] {
            width: 100%;
            padding: 8px;
            border: 1px solid var(--border);
            border-radius: 4px;
            background: var(--light);
        }
        input[type="number"] {
            width: 80px;
            padding: 8px;
            border: 1px solid var(--border);
            border-radius: 4px;
        }
        .image-preview {
            max-width: 100%;
            max-height: 200px;
            display: block;
            margin: 15px auto;
            border: 1px solid var(--border);
        }
        textarea {
            width: 100%;
            height: 150px;
            padding: 10px;
            border: 1px solid var(--border);
            border-radius: 4px;
            font-family: Consolas, monospace;
            resize: vertical;
        }
        .button-group {
            display: flex;
            gap: 10px;
            margin: 15px 0;
            flex-wrap: wrap;
        }
        button {
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            background: var(--primary);
            color: white;
            font-weight: 500;
            cursor: pointer;
            flex: 1;
            min-width: 120px;
        }
        button:hover {
            opacity: 0.9;
        }
        button:disabled {
            background: #ccc;
            cursor: not-allowed;
        }
        button.success {
            background: var(--success);
        }
        button.danger {
            background: var(--danger);
        }
        button.warning {
            background: var(--warning);
        }
        button.info {
            background: var(--info);
        }
        button.purple {
            background: var(--purple);
        }
        .status {
            font-size: 0.9em;
            color: #666;
            margin: 10px 0;
            padding: 8px;
            background: #f8f8f8;
            border-radius: 4px;
        }
        .progress-container {
            width: 100%;
            height: 10px;
            background: #e0e0e0;
            border-radius: 5px;
            margin: 10px 0;
            overflow: hidden;
        }
        .progress-bar {
            height: 100%;
            background: var(--primary);
            width: 0%;
            transition: width 0.3s;
        }
        footer {
            text-align: center;
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid var(--border);
            color: #777;
        }
        @media (max-width: 768px) {
            .window-container {
                flex-direction: column;
            }
            .window {
                min-width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>CMY Image Processing Tool</h1>
            <p>Complete image processing workflow · Runs entirely locally</p>
        </header>
        
        <!-- Window 1: Base64 to PNG -->
        <div class="window-container">
            <div class="window" style="border-top: 3px solid var(--purple);">
                <div class="window-title">Window 1: Base64 to PNG</div>
                <h2>Convert Base64 to PNG Image</h2>
                <div class="form-group">
                    <label for="base64Input">Enter Base64 code:</label>
                    <textarea id="base64Input" placeholder="Paste Base64 code here..."></textarea>
                </div>
                
                <div class="status" id="base64Status">Waiting for Base64 input...</div>
                
                <div class="button-group">
                    <button id="convertBase64Btn">Convert to PNG</button>
                    <button id="resetBase64Btn" class="danger">Reset</button>
                </div>
                
                <img id="base64PreviewImg" class="image-preview" style="display:none;">
                
                <div class="button-group">
                    <button id="downloadBase64ImgBtn" class="success" disabled>Download PNG</button>
                    <button id="sendToWindow2Btn" class="info" disabled>Send to Window 2</button>
                </div>
            </div>
        </div>

        <!-- Window 2: Image to CMY Data -->
        <div class="window-container">
            <div class="window" style="border-top: 3px solid var(--primary);">
                <div class="window-title">Window 2: Image to Encoding</div>
                <h2>Extract CMY Data from Image</h2>
                <div class="form-group">
                    <label for="imageInput">Select Image:</label>
                    <input type="file" id="imageInput" accept="image/*">
                </div>
                
                <div class="form-group">
                    <label>Output Dimensions:</label>
                    <div style="display: flex; gap: 15px;">
                        <div>
                            <label for="outputWidth">Width:</label>
                            <input type="number" id="outputWidth" min="1" value="100">
                        </div>
                        <div>
                            <label for="outputHeight">Height:</label>
                            <input type="number" id="outputHeight" min="1" value="100">
                        </div>
                    </div>
                </div>
                
                <img id="previewImg" class="image-preview" style="display:none;">
                
                <div class="status" id="extractStatus">Waiting for image upload...</div>
                <div class="progress-container" id="extractProgress" style="display:none;">
                    <div class="progress-bar" id="extractProgressBar"></div>
                </div>
                
                <div class="button-group">
                    <button id="extractBtn" disabled>Extract CMY Data</button>
                    <button id="resetExtractBtn" class="danger">Reset</button>
                </div>
                
                <div class="form-group">
                    <label for="cmyOutput">Encoded Data:</label>
                    <textarea id="cmyOutput" placeholder="Encoded data will appear here..." readonly></textarea>
                </div>
                
                <div class="button-group">
                    <button id="downloadCmyBtn" class="success" disabled>Download Data</button>
                    <button id="copyCmyBtn" class="success" disabled>Copy Data</button>
                    <button id="sendToEncodeBtn" class="info" disabled>Send to Window 3</button>
                </div>
            </div>
            
            <!-- Window 3: CMY Data Encoding -->
            <div class="window" style="border-top: 3px solid var(--warning);">
                <div class="window-title">Window 3: CMY Encoding</div>
                <h2>Encode CMY Data</h2>
                <div class="form-group">
                    <label for="encodedInput">Enter CMY Data:</label>
                    <textarea id="encodedInput" placeholder="Paste CMY data here..."></textarea>
                </div>
                
                <div class="form-group">
                    <label>Image Dimensions:</label>
                    <div style="display: flex; gap: 15px;">
                        <div>
                            <label for="encodeWidth">Width:</label>
                            <input type="number" id="encodeWidth" min="1" value="100">
                        </div>
                        <div>
                            <label for="encodeHeight">Height:</label>
                            <input type="number" id="encodeHeight" min="1" value="100">
                        </div>
                    </div>
                </div>
                
                <div class="status" id="encodeStatus">Waiting for CMY data input...</div>
                <div class="progress-container" id="encodeProgress" style="display:none;">
                    <div class="progress-bar" id="encodeProgressBar"></div>
                </div>
                
                <div class="button-group">
                    <button id="startEncodeBtn">Start Encoding</button>
                    <button id="resetEncodeBtn" class="danger">Reset</button>
                </div>
                
                <div class="form-group">
                    <label for="encodedOutput">Encoded Result:</label>
                    <textarea id="encodedOutput" placeholder="Encoded result will appear here..." readonly></textarea>
                </div>
                
                <div class="button-group">
                    <button id="downloadEncodedBtn" class="success" disabled>Download Encoding</button>
                    <button id="copyEncodedBtn" class="success" disabled>Copy Encoding</button>
                    <button id="sendToDecodeBtn" class="info" disabled>Send to Window 4</button>
                </div>
            </div>
        </div>
        
        <!-- Window 4: Encoded Data Decoding -->
        <div class="window-container">
            <div class="window" style="border-top: 3px solid var(--info);">
                <div class="window-title">Window 4: Decoding</div>
                <h2>Decode Encoded Data</h2>
                <div class="form-group">
                    <label for="decodeInput">Enter Encoded Data:</label>
                    <textarea id="decodeInput" placeholder="Paste encoded data here..."></textarea>
                </div>
                
                <div class="form-group">
                    <label>Image Dimensions:</label>
                    <div style="display: flex; gap: 15px;">
                        <div>
                            <label for="decodeWidth">Width:</label>
                            <input type="number" id="decodeWidth" min="1" value="100">
                        </div>
                        <div>
                            <label for="decodeHeight">Height:</label>
                            <input type="number" id="decodeHeight" min="1" value="100">
                        </div>
                    </div>
                </div>
                
                <div class="status" id="decodeStatus">Waiting for encoded data input...</div>
                <div class="progress-container" id="decodeProgress" style="display:none;">
                    <div class="progress-bar" id="decodeProgressBar"></div>
                </div>
                
                <div class="button-group">
                    <button id="startDecodeBtn">Start Decoding</button>
                    <button id="resetDecodeBtn" class="danger">Reset</button>
                </div>
                
                <div class="form-group">
                    <label for="decodedOutput">Decoded CMY Data:</label>
                    <textarea id="decodedOutput" placeholder="Decoded result will appear here..." readonly></textarea>
                </div>
                
                <div class="button-group">
                    <button id="downloadDecodedBtn" class="success" disabled>Download Decoding</button>
                    <button id="copyDecodedBtn" class="success" disabled>Copy Decoding</button>
                    <button id="sendToReconstructBtn" class="info" disabled>Send to Window 5</button>
                </div>
            </div>
            
            <!-- Window 5: Image Reconstruction -->
            <div class="window" style="border-top: 3px solid var(--success);">
                <div class="window-title">Window 5: Image Reconstruction</div>
                <h2>Reconstruct Image from CMY Data</h2>
                <div class="form-group">
                    <label>Image Dimensions:</label>
                    <div style="display: flex; gap: 15px;">
                        <div>
                            <label for="width">Width:</label>
                            <input type="number" id="width" min="1" value="100">
                        </div>
                        <div>
                            <label for="height">Height:</label>
                            <input type="number" id="height" min="1" value="100">
                        </div>
                    </div>
                </div>
                
                <div class="form-group">
                    <label for="cmyInput">CMY Data (0-1 range):</label>
                    <textarea id="cmyInput" placeholder="Paste CMY data here..."></textarea>
                </div>
                
                <div class="status" id="reconstructStatus">Waiting for CMY data input...</div>
                <div class="progress-container" id="reconstructProgress" style="display:none;">
                    <div class="progress-bar" id="reconstructProgressBar"></div>
                </div>
                
                <div class="button-group">
                    <button id="reconstructBtn">Reconstruct Image</button>
                    <button id="resetReconstructBtn" class="danger">Reset</button>
                </div>
                
                <img id="reconstructedImg" class="image-preview" style="display:none;">
                
                <div class="button-group">
                    <button id="downloadImgBtn" class="success" disabled>Download Image</button>
                </div>
            </div>
        </div>
        
        <footer>
            <p>© 2023 CMY Image Processing Tool | Local Processing | Complete Workflow</p>
        </footer>
    </div>

    <script>
        // Base64 to PNG functionality
        const base64Input = document.getElementById('base64Input');
        const convertBase64Btn = document.getElementById('convertBase64Btn');
        const base64PreviewImg = document.getElementById('base64PreviewImg');
        const downloadBase64ImgBtn = document.getElementById('downloadBase64ImgBtn');
        const sendToWindow2Btn = document.getElementById('sendToWindow2Btn');
        const base64Status = document.getElementById('base64Status');
        const resetBase64Btn = document.getElementById('resetBase64Btn');
        
        convertBase64Btn.addEventListener('click', function() {
            const base64Data = base64Input.value.trim();
            
            if (!base64Data) {
                base64Status.textContent = "Please enter Base64 data";
                return;
            }
            
            try {
                if (!base64Data.startsWith('data:image/')) {
                    base64Status.textContent = "Please enter valid Base64 image data (starting with 'data:image/')";
                    return;
                }
                
                base64PreviewImg.src = base64Data;
                base64PreviewImg.style.display = 'block';
                downloadBase64ImgBtn.disabled = false;
                sendToWindow2Btn.disabled = false;
                base64Status.textContent = "Base64 conversion successful!";
            } catch (e) {
                base64Status.textContent = "Conversion failed: " + e.message;
            }
        });
        
        downloadBase64ImgBtn.addEventListener('click', function() {
            if (!base64PreviewImg.src) return;
            
            const a = document.createElement('a');
            a.href = base64PreviewImg.src;
            a.download = 'converted_image.png';
            a.click();
        });
        
        sendToWindow2Btn.addEventListener('click', function() {
            if (!base64PreviewImg.src) return;
            
            document.getElementById('imageInput').files = null;
            document.getElementById('previewImg').src = base64PreviewImg.src;
            document.getElementById('previewImg').style.display = 'block';
            document.getElementById('extractBtn').disabled = false;
            document.getElementById('extractStatus').textContent = "Image imported from Window 1";
            
            const img = new Image();
            img.onload = function() {
                document.getElementById('outputWidth').value = img.naturalWidth;
                document.getElementById('outputHeight').value = img.naturalHeight;
                document.getElementById('encodeWidth').value = img.naturalWidth;
                document.getElementById('encodeHeight').value = img.naturalHeight;
                document.getElementById('decodeWidth').value = img.naturalWidth;
                document.getElementById('decodeHeight').value = img.naturalHeight;
                document.getElementById('width').value = img.naturalWidth;
                document.getElementById('height').value = img.naturalHeight;
            };
            img.src = base64PreviewImg.src;
        });
        
        resetBase64Btn.addEventListener('click', function() {
            base64Input.value = '';
            base64PreviewImg.src = '';
            base64PreviewImg.style.display = 'none';
            base64Status.textContent = 'Waiting for Base64 input...';
            downloadBase64ImgBtn.disabled = true;
            sendToWindow2Btn.disabled = true;
        });

        // Color conversion functions
        function rgbToCmy(r, g, b) {
            let c = (1 - r / 255).toFixed(4);
            let m = (1 - g / 255).toFixed(4);
            let y = (1 - b / 255).toFixed(4);
            
            c = parseFloat(c);
            m = parseFloat(m);
            y = parseFloat(y);
            
            if (c > 0.6 && y < 0.4) {
                return { c: "1", m: "0", y: "0" };
            } else if (m > 0.6) {
                return { c: "0", m: "1", y: "0" };
            } else if (y > 0.7 && c < 0.4) {
                return { c: "0", m: "0", y: "1" };
            } else if (c > 0.5 && y > 0.5) {
                return { c: "0.6", m: "0", y: "0.6" };
            } else {
                return { c: "0", m: "0", y: "0" };
            }
        }

        function normalizedCmyToRgb(c, m, y) {
            return {
                r: Math.round((1 - parseFloat(c)) * 255),
                g: Math.round((1 - parseFloat(m)) * 255),
                b: Math.round((1 - parseFloat(y)) * 255)
            };
        }

        // Encoding function
        function encodeCmyData(c, m, y) {
            c = parseFloat(c);
            m = parseFloat(m);
            y = parseFloat(y);

            if (c > 0.6 && y < 0.4) return "10";
            if (m > 0.6) return "11";
            if (y > 0.7 && c < 0.4) return "00";
            if (c > 0.5 && y > 0.5) return "01";
            return "OFF";
        }

        // Encoding function with coordinates
        function encodeCmyDataWithCoords(cmyText, width, height) {
            const lines = cmyText.split('\n');
            let encodedText = `# Encoded data dimensions: ${width}x${height}\n`;
            encodedText += `# Format: x,y: encoded value\n`;
            
            for (const line of lines) {
                const trimmedLine = line.trim();
                if (!trimmedLine || trimmedLine.startsWith('#')) continue;
                
                const parts = trimmedLine.split(':');
                const coords = parts[0].split(',');
                const values = parts[1] ? parts[1].split(',') : [];
                
                if (values.length >= 3) {
                    const x = coords[0].trim();
                    const y = coords[1].trim();
                    const c = parseFloat(values[0].trim());
                    const m = parseFloat(values[1].trim());
                    const yVal = parseFloat(values[2].trim());
                    
                    const code = encodeCmyData(c, m, yVal);
                    encodedText += `${x},${y}: ${code}\n`;
                }
            }
            
            return encodedText;
        }

        // Decoding function
        function decodeToCmy(encodedText, width, height) {
            const lines = encodedText.split('\n');
            let cmyText = `# Decoded CMY dimensions: ${width}x${height}\n`;
            cmyText += `# Format: x,y: c,m,y\n`;
            
            for (const line of lines) {
                const trimmedLine = line.trim();
                if (!trimmedLine || trimmedLine.startsWith('#')) continue;
                
                const parts = trimmedLine.split(':');
                const coords = parts[0].split(',');
                const code = parts[1] ? parts[1].trim() : '';
                
                if (coords.length >= 2 && code) {
                    const x = coords[0].trim();
                    const y = coords[1].trim();
                    let c, m, yVal;
                    
                    switch(code) {
                        case '00':
                            c = '0'; m = '0'; yVal = '1';
                            break;
                        case '01':
                            c = '0.6'; m = '0'; yVal = '0.6';
                            break;
                        case '10':
                            c = '1'; m = '0'; yVal = '0';
                            break;
                        case '11':
                            c = '0'; m = '1'; yVal = '0';
                            break;
                        case 'OFF':
                            c = '0'; m = '0'; yVal = '0';
                            break;
                        default:
                            c = '0'; m = '0'; yVal = '0';
                    }
                    
                    cmyText += `${x},${y}: ${c},${m},${yVal}\n`;
                }
            }
            
            return cmyText;
        }

        // Window 2: Image to CMY Data
        const imageInput = document.getElementById('imageInput');
        const outputWidth = document.getElementById('outputWidth');
        const outputHeight = document.getElementById('outputHeight');
        const previewImg = document.getElementById('previewImg');
        const extractBtn = document.getElementById('extractBtn');
        const cmyOutput = document.getElementById('cmyOutput');
        const downloadCmyBtn = document.getElementById('downloadCmyBtn');
        const copyCmyBtn = document.getElementById('copyCmyBtn');
        const sendToEncodeBtn = document.getElementById('sendToEncodeBtn');
        const extractStatus = document.getElementById('extractStatus');
        const extractProgress = document.getElementById('extractProgress');
        const extractProgressBar = document.getElementById('extractProgressBar');
        const resetExtractBtn = document.getElementById('resetExtractBtn');
        
        let currentImageData = null;
        
        imageInput.addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = function(e) {
                previewImg.src = e.target.result;
                previewImg.style.display = 'block';
                extractBtn.disabled = false;
                extractStatus.textContent = `Loaded: ${file.name}`;
                currentImageData = e.target.result;
                
                const img = new Image();
                img.onload = function() {
                    outputWidth.value = img.naturalWidth;
                    outputHeight.value = img.naturalHeight;
                    document.getElementById('encodeWidth').value = img.naturalWidth;
                    document.getElementById('encodeHeight').value = img.naturalHeight;
                    document.getElementById('decodeWidth').value = img.naturalWidth;
                    document.getElementById('decodeHeight').value = img.naturalHeight;
                    document.getElementById('width').value = img.naturalWidth;
                    document.getElementById('height').value = img.naturalHeight;
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        });
        
        extractBtn.addEventListener('click', function() {
            if (!currentImageData) return;
            
            const targetWidth = parseInt(outputWidth.value);
            const targetHeight = parseInt(outputHeight.value);
            
            if (isNaN(targetWidth) || isNaN(targetHeight) || targetWidth <= 0 || targetHeight <= 0) {
                extractStatus.textContent = "Please enter valid dimensions";
                return;
            }
            
            extractStatus.textContent = "Processing image...";
            extractBtn.disabled = true;
            extractProgress.style.display = 'block';
            
            const img = new Image();
            img.onload = function() {
                const tempCanvas = document.createElement('canvas');
                tempCanvas.width = targetWidth;
                tempCanvas.height = targetHeight;
                const tempCtx = tempCanvas.getContext('2d');
                
                tempCtx.imageSmoothingQuality = 'high';
                tempCtx.drawImage(img, 0, 0, targetWidth, targetHeight);
                
                const pixelData = tempCtx.getImageData(0, 0, targetWidth, targetHeight).data;
                let cmyText = `# Image dimensions: ${targetWidth}x${targetHeight}\n`;
                cmyText += `# Format: x,y: c,m,y (processed values)\n`;
                
                const totalPixels = targetWidth * targetHeight;
                let processedPixels = 0;
                
                function processBatch(batchSize) {
                    const end = Math.min(processedPixels + batchSize, totalPixels);
                    
                    for (let i = processedPixels; i < end; i++) {
                        const idx = i * 4;
                        const r = pixelData[idx];
                        const g = pixelData[idx + 1];
                        const b = pixelData[idx + 2];
                        
                        const {c, m, y} = rgbToCmy(r, g, b);
                        const x = i % targetWidth;
                        const yCoord = Math.floor(i / targetWidth);
                        
                        cmyText += `${x},${yCoord}: ${c},${m},${y}\n`;
                        processedPixels++;
                    }
                    
                    const progress = (processedPixels / totalPixels) * 100;
                    extractProgressBar.style.width = `${progress}%`;
                    extractStatus.textContent = `Processed ${processedPixels}/${totalPixels} pixels`;
                    
                    if (processedPixels % 5000 === 0 || processedPixels === totalPixels) {
                        cmyOutput.value = cmyText;
                        cmyOutput.scrollTop = cmyOutput.scrollHeight;
                    }
                    
                    if (processedPixels < totalPixels) {
                        setTimeout(() => processBatch(5000), 0);
                    } else {
                        cmyOutput.value = cmyText;
                        downloadCmyBtn.disabled = false;
                        copyCmyBtn.disabled = false;
                        sendToEncodeBtn.disabled = false;
                        extractStatus.textContent = `Conversion complete! Total pixels: ${totalPixels}`;
                    }
                }
                
                processBatch(5000);
            };
            img.src = currentImageData;
        });
        
        downloadCmyBtn.addEventListener('click', function() {
            if (!cmyOutput.value) return;
            
            const blob = new Blob([cmyOutput.value], {type: 'text/plain'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'image_cmy_data.txt';
            a.click();
            URL.revokeObjectURL(url);
        });
        
        copyCmyBtn.addEventListener('click', function() {
            if (!cmyOutput.value) return;
            
            cmyOutput.select();
            document.execCommand('copy');
            extractStatus.textContent = "CMY data copied!";
            setTimeout(() => {
                extractStatus.textContent = "CMY data ready";
            }, 2000);
        });

        sendToEncodeBtn.addEventListener('click', function() {
            if (!cmyOutput.value) return;
            
            document.getElementById('encodedInput').value = cmyOutput.value;
            document.getElementById('encodeWidth').value = document.getElementById('outputWidth').value;
            document.getElementById('encodeHeight').value = document.getElementById('outputHeight').value;
            document.getElementById('encodeStatus').textContent = "CMY data imported from Window 2";
        });
        
        resetExtractBtn.addEventListener('click', function() {
            imageInput.value = '';
            previewImg.src = '';
            previewImg.style.display = 'none';
            cmyOutput.value = '';
            extractStatus.textContent = 'Waiting for image upload...';
            extractProgress.style.display = 'none';
            extractBtn.disabled = true;
            downloadCmyBtn.disabled = true;
            copyCmyBtn.disabled = true;
            sendToEncodeBtn.disabled = true;
            currentImageData = null;
        });
        
        // Window 3: CMY Data Encoding
        const encodedInput = document.getElementById('encodedInput');
        const encodeWidth = document.getElementById('encodeWidth');
        const encodeHeight = document.getElementById('encodeHeight');
        const startEncodeBtn = document.getElementById('startEncodeBtn');
        const encodedOutput = document.getElementById('encodedOutput');
        const downloadEncodedBtn = document.getElementById('downloadEncodedBtn');
        const copyEncodedBtn = document.getElementById('copyEncodedBtn');
        const sendToDecodeBtn = document.getElementById('sendToDecodeBtn');
        const encodeStatus = document.getElementById('encodeStatus');
        const encodeProgress = document.getElementById('encodeProgress');
        const encodeProgressBar = document.getElementById('encodeProgressBar');
        const resetEncodeBtn = document.getElementById('resetEncodeBtn');

        startEncodeBtn.addEventListener('click', function() {
            const cmyText = encodedInput.value.trim();
            const width = parseInt(encodeWidth.value);
            const height = parseInt(encodeHeight.value);
            
            if (!cmyText) {
                encodeStatus.textContent = "Please enter CMY data";
                return;
            }
            
            if (isNaN(width) || isNaN(height) || width <= 0 || height <= 0) {
                encodeStatus.textContent = "Please enter valid dimensions";
                return;
            }
            
            encodeStatus.textContent = "Encoding data...";
            startEncodeBtn.disabled = true;
            encodeProgress.style.display = 'block';
            
            setTimeout(() => {
                try {
                    const encodedResult = encodeCmyDataWithCoords(cmyText, width, height);
                    encodedOutput.value = encodedResult;
                    downloadEncodedBtn.disabled = false;
                    copyEncodedBtn.disabled = false;
                    sendToDecodeBtn.disabled = false;
                    encodeStatus.textContent = "Encoding complete!";
                } catch (e) {
                    encodeStatus.textContent = "Encoding error: " + e.message;
                } finally {
                    startEncodeBtn.disabled = false;
                    encodeProgress.style.display = 'none';
                }
            }, 100);
        });

        downloadEncodedBtn.addEventListener('click', function() {
            if (!encodedOutput.value) return;
            
            const blob = new Blob([encodedOutput.value], {type: 'text/plain'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'encoded_cmy_data.txt';
            a.click();
            URL.revokeObjectURL(url);
        });

        copyEncodedBtn.addEventListener('click', function() {
            if (!encodedOutput.value) return;
            
            encodedOutput.select();
            document.execCommand('copy');
            encodeStatus.textContent = "Encoded result copied!";
            setTimeout(() => {
                encodeStatus.textContent = "Encoded result ready";
            }, 2000);
        });

        sendToDecodeBtn.addEventListener('click', function() {
            if (!encodedOutput.value) return;
            
            document.getElementById('decodeInput').value = encodedOutput.value;
            document.getElementById('decodeWidth').value = encodeWidth.value;
            document.getElementById('decodeHeight').value = encodeHeight.value;
            document.getElementById('decodeStatus').textContent = "Encoded data imported from Window 3";
        });

        resetEncodeBtn.addEventListener('click', function() {
            encodedInput.value = '';
            encodedOutput.value = '';
            encodeStatus.textContent = 'Waiting for CMY data input...';
            encodeProgress.style.display = 'none';
            downloadEncodedBtn.disabled = true;
            copyEncodedBtn.disabled = true;
            sendToDecodeBtn.disabled = true;
        });

        // Window 4: Encoded Data Decoding
        const decodeInput = document.getElementById('decodeInput');
        const decodeWidth = document.getElementById('decodeWidth');
        const decodeHeight = document.getElementById('decodeHeight');
        const startDecodeBtn = document.getElementById('startDecodeBtn');
        const decodedOutput = document.getElementById('decodedOutput');
        const downloadDecodedBtn = document.getElementById('downloadDecodedBtn');
        const copyDecodedBtn = document.getElementById('copyDecodedBtn');
        const sendToReconstructBtn = document.getElementById('sendToReconstructBtn');
        const decodeStatus = document.getElementById('decodeStatus');
        const decodeProgress = document.getElementById('decodeProgress');
        const decodeProgressBar = document.getElementById('decodeProgressBar');
        const resetDecodeBtn = document.getElementById('resetDecodeBtn');

        startDecodeBtn.addEventListener('click', function() {
            const encodedText = decodeInput.value.trim();
            const width = parseInt(decodeWidth.value);
            const height = parseInt(decodeHeight.value);
            
            if (!encodedText) {
                decodeStatus.textContent = "Please enter encoded data";
                return;
            }
            
            if (isNaN(width) || isNaN(height) || width <= 0 || height <= 0) {
                decodeStatus.textContent = "Please enter valid dimensions";
                return;
            }
            
            decodeStatus.textContent = "Decoding data...";
            startDecodeBtn.disabled = true;
            decodeProgress.style.display = 'block';
            
            setTimeout(() => {
                try {
                    const decodedResult = decodeToCmy(encodedText, width, height);
                    decodedOutput.value = decodedResult;
                    downloadDecodedBtn.disabled = false;
                    copyDecodedBtn.disabled = false;
                    sendToReconstructBtn.disabled = false;
                    decodeStatus.textContent = "Decoding complete!";
                } catch (e) {
                    decodeStatus.textContent = "Decoding error: " + e.message;
                } finally {
                    startDecodeBtn.disabled = false;
                    decodeProgress.style.display = 'none';
                }
            }, 100);
        });

        downloadDecodedBtn.addEventListener('click', function() {
            if (!decodedOutput.value) return;
            
            const blob = new Blob([decodedOutput.value], {type: 'text/plain'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'decoded_cmy_data.txt';
            a.click();
            URL.revokeObjectURL(url);
        });

        copyDecodedBtn.addEventListener('click', function() {
            if (!decodedOutput.value) return;
            
            decodedOutput.select();
            document.execCommand('copy');
            decodeStatus.textContent = "Decoded result copied!";
            setTimeout(() => {
                decodeStatus.textContent = "Decoded result ready";
            }, 2000);
        });

        sendToReconstructBtn.addEventListener('click', function() {
            if (!decodedOutput.value) return;
            
            document.getElementById('cmyInput').value = decodedOutput.value;
            document.getElementById('width').value = decodeWidth.value;
            document.getElementById('height').value = decodeHeight.value;
            document.getElementById('reconstructStatus').textContent = "CMY data imported from Window 4";
        });

        resetDecodeBtn.addEventListener('click', function() {
            decodeInput.value = '';
            decodedOutput.value = '';
            decodeStatus.textContent = 'Waiting for encoded data input...';
            decodeProgress.style.display = 'none';
            downloadDecodedBtn.disabled = true;
            copyDecodedBtn.disabled = true;
            sendToReconstructBtn.disabled = true;
        });

        // Window 5: Image Reconstruction
        const cmyInput = document.getElementById('cmyInput');
        const widthInput = document.getElementById('width');
        const heightInput = document.getElementById('height');
        const reconstructBtn = document.getElementById('reconstructBtn');
        const reconstructedImg = document.getElementById('reconstructedImg');
        const downloadImgBtn = document.getElementById('downloadImgBtn');
        const reconstructStatus = document.getElementById('reconstructStatus');
        const reconstructProgress = document.getElementById('reconstructProgress');
        const reconstructProgressBar = document.getElementById('reconstructProgressBar');
        const resetReconstructBtn = document.getElementById('resetReconstructBtn');
        
        reconstructBtn.addEventListener('click', function() {
            const width = parseInt(widthInput.value);
            const height = parseInt(heightInput.value);
            const cmyText = cmyInput.value.trim();
            
            if (!cmyText) {
                reconstructStatus.textContent = "Please enter CMY data";
                return;
            }
            
            if (isNaN(width) || isNaN(height) || width <= 0 || height <= 0) {
                reconstructStatus.textContent = "Please enter valid dimensions";
                return;
            }
            
            reconstructStatus.textContent = "Reconstructing image...";
            reconstructBtn.disabled = true;
            reconstructProgress.style.display = 'block';
            
            const canvas = document.createElement('canvas');
            canvas.width = width;
            canvas.height = height;
            const ctx = canvas.getContext('2d');
            const imageData = ctx.createImageData(width, height);
            
            // Initialize white background
            for (let i = 0; i < imageData.data.length; i += 4) {
                imageData.data[i] = 255;
                imageData.data[i + 1] = 255;
                imageData.data[i + 2] = 255;
                imageData.data[i + 3] = 255;
            }
            
            // Parse original dimensions
            let originalWidth = 0, originalHeight = 0;
            const sizeLine = cmyText.split('\n')[0];
            const sizeMatch = sizeLine.match(/# Image dimensions: (\d+)x(\d+)/);
            if (sizeMatch) {
                originalWidth = parseInt(sizeMatch[1]);
                originalHeight = parseInt(sizeMatch[2]);
            }
            
            // Calculate aspect ratios
            const widthRatio = originalWidth > 0 ? width / originalWidth : 1;
            const heightRatio = originalHeight > 0 ? height / originalHeight : 1;
            
            const lines = cmyText.split('\n');
            let validPixels = 0;
            let currentLine = 0;
            const totalLines = lines.length;
            
            function processBatch(batchSize) {
                const end = Math.min(currentLine + batchSize, totalLines);
                
                for (let i = currentLine; i < end; i++) {
                    const line = lines[i].trim();
                    if (!line || line.startsWith('#')) continue;
                    
                    const parts = line.split(':');
                    const coords = parts[0].split(',');
                    const values = parts[1] ? parts[1].split(',') : coords;
                    
                    try {
                        let x, y, c, m, yVal;
                        
                        if (parts.length > 1 && coords.length >= 2) {
                            // Original coordinates
                            const origX = parseInt(coords[0].trim());
                            const origY = parseInt(coords[1].trim());
                            
                            // Calculate new coordinates
                            x = Math.round(origX * widthRatio);
                            y = Math.round(origY * heightRatio);
                            
                            c = parseFloat(values[0].trim());
                            m = parseFloat(values[1].trim());
                            yVal = parseFloat(values[2].trim());
                            
                            // Apply modified CMY values
                            if (c === 1 && m === 0 && yVal === 0) {
                                c = 1; m = 0.25; yVal = 0.04;
                            } else if (c === 0 && m === 1 && yVal === 0) {
                                c = 0; m = 0.8; yVal = 0.04;
                            } else if (c === 0 && m === 0 && yVal === 1) {
                                c = 0; m = 0; yVal = 0.8;
                            } else if (c === 0.6 && m === 0 && yVal === 0.6) {
                                c = 0.8; m = 0; yVal = 0.8;
                            } else if (c === 0 && m === 0 && yVal === 0) {
                                c = 0; m = 0; yVal = 0;
                            }
                        } else if (values.length >= 3) {
                            // If no coordinates, fill sequentially
                            x = validPixels % width;
                            y = Math.floor(validPixels / width);
                            c = parseFloat(values[0].trim());
                            m = parseFloat(values[1].trim());
                            yVal = parseFloat(values[2].trim());
                            
                            // Apply modified CMY values
                            if (c === 1 && m === 0 && yVal === 0) {
                                c = 1; m = 0.25; yVal = 0.04;
                            } else if (c === 0 && m === 1 && yVal === 0) {
                                c = 0; m = 0.8; yVal = 0.04;
                            } else if (c === 0 && m === 0 && yVal === 1) {
                                c = 0; m = 0; yVal = 0.8;
                            } else if (c === 0.6 && m === 0 && yVal === 0.6) {
                                c = 0.8; m = 0; yVal = 0.8;
                            } else if (c === 0 && m === 0 && yVal === 0) {
                                c = 0; m = 0; yVal = 0;
                            }
                        } else {
                            continue;
                        }
                        
                        // Ensure coordinates are within valid range
                        x = Math.max(0, Math.min(width - 1, x));
                        y = Math.max(0, Math.min(height - 1, y));
                        
                        const idx = (y * width + x) * 4;
                        const {r, g, b} = normalizedCmyToRgb(c, m, yVal);
                        
                        imageData.data[idx] = r;
                        imageData.data[idx + 1] = g;
                        imageData.data[idx + 2] = b;
                        validPixels++;
                    } catch (e) {
                        console.warn("Failed to parse line:", line, "Error:", e);
                    }
                    
                    currentLine++;
                }
                
                const progress = (currentLine / totalLines) * 100;
                reconstructProgressBar.style.width = `${progress}%`;
                reconstructStatus.textContent = `Processed ${currentLine}/${totalLines} lines`;
                
                if (currentLine < totalLines) {
                    setTimeout(() => processBatch(1000), 0);
                } else {
                    // Apply smoothing
                    ctx.putImageData(imageData, 0, 0);
                    
                    // Maintain original aspect ratio if needed
                    if (originalWidth > 0 && originalHeight > 0) {
                        const tempCanvas = document.createElement('canvas');
                        tempCanvas.width = originalWidth;
                        tempCanvas.height = originalHeight;
                        const tempCtx = tempCanvas.getContext('2d');
                        
                        // Draw to temporary canvas
                        tempCtx.drawImage(canvas, 0, 0, originalWidth, originalHeight);
                        
                        // Scale back to target dimensions
                        ctx.clearRect(0, 0, width, height);
                        ctx.imageSmoothingEnabled = true;
                        ctx.imageSmoothingQuality = 'high';
                        ctx.drawImage(tempCanvas, 0, 0, width, height);
                    }
                    
                    reconstructedImg.src = canvas.toDataURL('image/png');
                    reconstructedImg.style.display = 'block';
                    downloadImgBtn.disabled = false;
                    reconstructStatus.textContent = `Reconstruction complete! Valid pixels: ${validPixels}`;
                    reconstructBtn.disabled = false;
                }
            }
            
            processBatch(1000);
        });
        
        function clampValue(value, min, max) {
            return Math.max(min, Math.min(max, value));
        }
        
        downloadImgBtn.addEventListener('click', function() {
            if (!reconstructedImg.src) return;
            
            const a = document.createElement('a');
            a.href = reconstructedImg.src;
            a.download = 'reconstructed_image.png';
            a.click();
        });
        
        resetReconstructBtn.addEventListener('click', function() {
            cmyInput.value = '';
            widthInput.value = '100';
            heightInput.value = '100';
            reconstructedImg.src = '';
            reconstructedImg.style.display = 'none';
            reconstructStatus.textContent = 'Waiting for CMY data input...';
            reconstructProgress.style.display = 'none';
            downloadImgBtn.disabled = true;
        });
    </script>
</body>
</html>
