# my-coding-file-
This repository contains the PNG file and code I created."
<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Krishna's Pro BG Remover</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #121212; color: white; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; }
        .card { background: #1e1e1e; padding: 40px; border-radius: 25px; box-shadow: 0 15px 35px rgba(0,0,0,0.5); width: 450px; text-align: center; border: 1px solid #333; }
        h2 { color: #00d2ff; margin-bottom: 10px; }
        p { color: #aaa; margin-bottom: 25px; }
        .upload-area { border: 2px dashed #444; padding: 20px; border-radius: 15px; margin-bottom: 20px; transition: 0.3s; }
        .upload-area:hover { border-color: #00d2ff; }
        input[type="file"] { margin: 10px 0; color: #ccc; }
        #resultImg { max-width: 100%; margin-top: 20px; display: none; border-radius: 15px; background: repeating-conic-gradient(#333 0% 25%, #444 0% 50%) 50% / 20px 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.3); }
        button { background: linear-gradient(135deg, #00d2ff, #3a7bd5); color: white; border: none; padding: 15px 30px; border-radius: 10px; cursor: pointer; font-weight: bold; width: 100%; font-size: 16px; transition: 0.3s; }
        button:hover { transform: translateY(-2px); box-shadow: 0 5px 15px rgba(0,210,255,0.3); }
        button:disabled { background: #444; cursor: not-allowed; transform: none; }
        .loader { display: none; color: #00d2ff; margin-top: 15px; font-weight: bold; }
        .download-btn { background: linear-gradient(135deg, #28a745, #1e7e34); margin-top: 15px; }
    </style>
</head>
<body>

<div class="card">
    <h2>AI स्टिकर मेकर 🚀</h2>
    <p>समोसा और भालू वाली फोटो अपलोड करें</p>
    
    <div class="upload-area">
        <input type="file" id="fileInput" accept="image/*">
    </div>
    
    <button id="processBtn">बैकग्राउंड हटाएँ (PNG बनाएँ)</button>
    
    <div class="loader" id="loader">AI काम कर रहा है... कृपया रुकें ⏳</div>
    
    <img id="resultImg" alt="Result">
    
    <button id="downloadBtn" class="download-btn" style="display:none;">फ़ाइल डाउनलोड करें</button>
</div>

<script>
    const fileInput = document.getElementById('fileInput');
    const processBtn = document.getElementById('processBtn');
    const resultImg = document.getElementById('resultImg');
    const downloadBtn = document.getElementById('downloadBtn');
    const loader = document.getElementById('loader');

    // आपकी API Key यहाँ सुरक्षित तरीके से सेट है
    const KRISHNA_API_KEY = "FkWuV8jH3PJQBTSHbofFTv2D";

    processBtn.addEventListener('click', async () => {
        const file = fileInput.files[0];

        if (!file) {
            alert("कृष्णा, पहले एक फोटो तो चुनो!");
            return;
        }

        loader.style.display = 'block';
        processBtn.disabled = true;
        resultImg.style.display = 'none';
        downloadBtn.style.display = 'none';

        const formData = new FormData();
        formData.append('image_file', file);
        formData.append('size', 'auto');

        try {
            const response = await fetch('https://api.remove.bg/v1.0/removebg', {
                method: 'POST',
                headers: { 'X-Api-Key': KRISHNA_API_KEY },
                body: formData
            });

            if (response.ok) {
                const blob = await response.blob();
                const url = URL.createObjectURL(blob);
                
                resultImg.src = url;
                resultImg.style.display = 'block';
                downloadBtn.style.display = 'block';
                
                downloadBtn.onclick = () => {
                    const a = document.createElement('a');
                    a.href = url;
                    a.download = "krishna_transparent.png";
                    a.click();
                };
            } else {
                const errorData = await response.json();
                alert("Error: " + (errorData.errors[0].title || "API में कोई दिक्कत है"));
            }
        } catch (error) {
            console.error(error);
            alert("इंटरनेट चेक करें, कुछ गड़बड़ हुई!");
        } finally {
            loader.style.display = 'none';
            processBtn.disabled = false;
        }
    });
</script>

</body>
</html>
