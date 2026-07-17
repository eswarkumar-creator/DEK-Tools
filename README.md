<!DOCTYPE html>
<html lang="te">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DEK Voice AI - తెలుగు వాయిస్ మిత్ర</title>

<style>
body { margin: 0; font-family: sans-serif; background: linear-gradient(135deg, #001f3f, #ff69b4); color: white; text-align: center; }
.container { padding: 40px 20px; }
.logo { font-size: 40px; font-weight: bold; }
h1 { font-size: 32px; }
p { font-size: 20px; }
.box { background: white; color: #222; border-radius: 20px; padding: 25px; margin: 30px auto; max-width: 400px; }

button { width: 90%; padding: 15px; margin: 10px; border: none; border-radius: 30px; font-size: 18px; cursor: pointer; background: #007bff; color: white; transition: 0.3s; }
button:hover { opacity: 0.8; }

/* కొత్తగా పెట్టిన డిజైన్లు */
.btn-green { 
    background: #28a745 !important; 
    animation: pulse 1.5s infinite; 
}
@keyframes pulse { 
    0% { transform: scale(1); } 
    50% { transform: scale(1.03); } 
    100% { transform: scale(1); } 
}

.copy-btn { background: #ff9800; width: 60%; font-size: 16px; padding: 10px; }

textarea { width: 90%; min-height: 120px; margin-top: 15px; padding: 15px; border: 1px solid #ccc; border-radius: 10px; background: #f9f9f9; font-size: 18px; color: #333; resize: none; box-sizing: border-box; }

.footer { margin-top: 40px; font-size: 14px; }
</style>

</head>
<body>

<div class="container">
    <div class="logo">D.E.K</div>
    <h1>🎤 DEK Voice AI</h1>
    <p>మాట్లాడు... పని చేయించు</p>

    <div class="box">
        <button id="startSpeech">🎙️ మాట్లాడితే రాయించు</button>
        
        <!-- టెక్స్ట్ కాపీ చేసుకోవడానికి, టైప్ చేసుకోవడానికి textarea వాడాము -->
        <textarea id="outputBox" placeholder="మీరు మాట్లాడినది ఇక్కడ వస్తుంది..."></textarea>
        
        <button id="copyBtn" class="copy-btn">📋 టెక్స్ట్ కాపీ చేయి</button>

        <button>🔊 రాతను వినిపించు (త్వరలో)</button>
        <button>🤖 తెలుగు AI సహాయం (త్వరలో)</button>
    </div>

    <p>తెలుగు ప్రజల కోసం సులభమైన వాయిస్ సహాయకుడు</p>
    <div class="footer">© 2026 DEK Voice AI</div>
</div>

<script>
    const startBtn = document.getElementById('startSpeech');
    const outputBox = document.getElementById('outputBox');
    const copyBtn = document.getElementById('copyBtn');
    let isListening = false;

    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if(SpeechRecognition) {
        const recognition = new SpeechRecognition();
        recognition.lang = 'te-IN';
        recognition.continuous = true; // మీరు ఆపేవరకు వింటూనే ఉంటుంది

        startBtn.addEventListener('click', () => {
            if(!isListening) {
                recognition.start();
                isListening = true;
                startBtn.innerHTML = "🔴 ఆపడానికి ఇక్కడ నొక్కండి";
                startBtn.classList.add("btn-green"); // పచ్చ కలర్ రావడానికి
            } else {
                recognition.stop();
                isListening = false;
                startBtn.innerHTML = "🎙️ మాట్లాడితే రాయించు";
                startBtn.classList.remove("btn-green");
            }
        });

        recognition.onresult = (event) => {
            let final_transcript = '';
            for (let i = event.resultIndex; i < event.results.length; ++i) {
                if (event.results[i].isFinal) {
                    final_transcript += event.results[i][0].transcript + ' ';
                }
            }
            outputBox.value += final_transcript; // ముందున్న టెక్స్ట్‌కి కొత్తది కలుస్తుంది
        };

        recognition.onend = () => {
            // మొబైల్ బ్రౌజర్ సైలెంట్‌గా ఉన్నప్పుడు ఆటోమేటిక్ గా ఆపేస్తే, మళ్ళీ స్టార్ట్ చేయడానికి
            if(isListening) {
                recognition.start();
            }
        };
        
        recognition.onerror = (event) => {
            if(event.error === 'not-allowed') {
                isListening = false;
                startBtn.innerHTML = "🎙️ మాట్లాడితే రాయించు";
                startBtn.classList.remove("btn-green");
                alert("దయచేసి మైక్ పర్మిషన్ ఇవ్వండి.");
            }
        };

    } else {
        outputBox.value = "మీ బ్రౌజర్ సపోర్ట్ చేయడం లేదు. దయచేసి క్రోమ్ వాడండి.";
    }

    // కాపీ బటన్ ఫీచర్
    copyBtn.addEventListener('click', () => {
        if(outputBox.value.trim() !== "") {
            outputBox.select();
            navigator.clipboard.writeText(outputBox.value).then(() => {
                copyBtn.innerHTML = "✅ కాపీ అయింది!";
                setTimeout(() => { copyBtn.innerHTML = "📋 టెక్స్ట్ కాపీ చేయి"; }, 2000);
            });
        } else {
            alert("కాపీ చేయడానికి టెక్స్ట్ ఏమీ లేదు!");
        }
    });
</script>

</body>
</html>
