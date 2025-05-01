<div class="navigation">
  <a href="/Tools">Home</a> | 
  <a href="/index.html-2">Tool Version 1</a> | 
  <a href="/Tools-index.html-2">Tool Version 2</a>
</div><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Image Compression Tool</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f5f7fa;
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
    }

    .container {
      max-width: 500px;
      width: 100%;
      background: #fff;
      padding: 25px;
      border-radius: 10px;
      box-shadow: 0 0 15px rgba(0,0,0,0.1);
      text-align: center;
    }

    .upload-box {
      border: 2px dashed #007bff;
      padding: 30px;
      margin-bottom: 20px;
      cursor: pointer;
    }

    .upload-label {
      color: #007bff;
      font-weight: bold;
    }

    .slider-container {
      margin: 20px 0;
    }

    .preview {
      display: flex;
      justify-content: space-between;
      gap: 10px;
      margin-bottom: 20px;
    }

    .preview img {
      max-width: 100px;
      height: auto;
      border-radius: 5px;
    }

    button {
      background-color: #007bff;
      color: white;
      padding: 12px 20px;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="container">ca-pub-8773480799818158/5072843171
    <h1>Image Compression Tool</h1>ca-pub-8773480799818158/5072843171
    <div class="upload-box" id="upload-area">
      <input type="file" id="upload" accept="image/*" hidden />
      <label for="upload" class="upload-label">Click or drag an image here to upload</label>
    </div>

    <div class="slider-container">
      <label for="quality-slider">Quality:</label>
      <input type="range" id="quality-slider" min="10" max="100" value="80" />
      <span id="quality-value">80%</span>
    </div>

    <div class="preview">
      <div>
        <h3>Original</h3>
        <img id="original-image" />
        <p>Size: <span id="original-size">-</span></p>
      </div>
      <div>
        <h3>Compressed</h3>
        <img id="compressed-image" />
        <p>Size: <span id="compressed-size">-</span></p>
        <p>Saved: <span id="saved-size">-</span></p>
      </div>
    </div>

    <button id="download-btn" disabled>Download Compressed Image</button>
  </div>

  <script>
    const uploadInput = document.getElementById("upload");
    const qualitySlider = document.getElementById("quality-slider");
    const qualityValue = document.getElementById("quality-value");
    const originalImage = document.getElementById("original-image");
    const compressedImage = document.getElementById("compressed-image");
    const originalSizeText = document.getElementById("original-size");
    const compressedSizeText = document.getElementById("compressed-size");
    const savedSizeText = document.getElementById("saved-size");
    const downloadBtn = document.getElementById("download-btn");

    let compressedBlob = null;

    qualitySlider.addEventListener("input", () => {
      qualityValue.textContent = `${qualitySlider.value}%`;
    });

    uploadInput.addEventListener("change", async (event) => {
      const file = event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = async function () {
        const img = new Image();
        img.src = reader.result;
        img.onload = async function () {
          originalImage.src = img.src;
          originalSizeText.textContent = formatBytes(file.size);

          const canvas = document.createElement("canvas");
          const ctx = canvas.getContext("2d");
          canvas.width = img.width;
          canvas.height = img.height;
          ctx.drawImage(img, 0, 0);

          const quality = qualitySlider.value / 100;
          canvas.toBlob((blob) => {
            compressedBlob = blob;
            compressedImage.src = URL.createObjectURL(blob);
            compressedSizeText.textContent = formatBytes(blob.size);
            savedSizeText.textContent = `${Math.round(100 - (blob.size / file.size) * 100)}%`;
            downloadBtn.disabled = false;
          }, "image/jpeg", quality);
        };
      };
      reader.readAsDataURL(file);
    });

    downloadBtn.addEventListener("click", () => {
      if (!compressedBlob) return;
      const link = document.createElement("a");
      link.href = URL.createObjectURL(compressedBlob);
      link.download = "compressed.jpg";
      link.click();
    });

    function formatBytes(bytes) {
      const sizes = ["Bytes", "KB", "MB"];
      if (bytes === 0) return "0 Byte";
      const i = parseInt(Math.floor(Math.log(bytes) / Math.log(1024)));
      return `${(bytes / Math.pow(1024, i)).toFixed(2)} ${sizes[i]}`;
    }
  </script>
</body>
</html>
