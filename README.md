<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>一键拍照</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;font-family:system-ui}
body{background:#111;color:#fff;padding:20px;max-width:700px;margin:0 auto}
#video{width:100%;background:#000;border-radius:12px;}
.btns{margin:16px 0;display:flex;gap:12px;flex-wrap:wrap}
button{padding:12px 20px;border:none;border-radius:8px;font-size:16px;cursor:pointer}
#shoot{background:#2563eb;color:white}
#download{background:#16a34a;color:white;display:none}
#canvas{display:none}
#preview{width:100%;border-radius:12px;margin-top:16px;display:none}
</style>
</head>
<body>
    <video id="video" autoplay playsinline></video>
    <div class="btns">
        <button id="shoot">📸 拍照</button>
        <button id="download">💾 下载照片</button>
    </div>
    <canvas id="canvas"></canvas>
    <img id="preview">

<script>
const video = document.getElementById('video');
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const shootBtn = document.getElementById('shoot');
const downloadBtn = document.getElementById('download');
const preview = document.getElementById('preview');
let imgUrl = '';

// 请求摄像头
navigator.mediaDevices.getUserMedia({
    video: { facingMode: 'environment' } // environment后置摄像头，user前置
})
.then(stream=>{
    video.srcObject = stream;
})
.catch(err=>{
    alert("无法打开摄像头："+err.message+"\n提示：需要HTTPS/本地服务环境，并且授权相机权限");
    console.error(err);
})

// 拍照
shootBtn.onclick = ()=>{
    canvas.width = video.videoWidth;
    canvas.height = video.videoHeight;
    ctx.drawImage(video,0,0);
    imgUrl = canvas.toDataURL("image/png");
    preview.src = imgUrl;
    preview.style.display = 'block';
    downloadBtn.style.display = 'inline-block';
}

// 下载
downloadBtn.onclick = ()=>{
    const a = document.createElement('a');
    a.href = imgUrl;
    a.download = `photo_${Date.now()}.png`;
    a.click();
}
</script>
</body>
</html>
