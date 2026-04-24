<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday, Mitu</title>
    
    <!-- Fonts & Libraries -->
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,700;1,700&family=Plus+Jakarta+Sans:wght@300;400;600&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/0.160.0/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.2/dist/confetti.browser.min.js"></script>

    <style>
        :root {
            --bg-dark: #0c0a09;
            --primary-pink: #ec4899;
            --primary-red: #ef4444;
        }

        body {
            margin: 0;
            padding: 0;
            background-color: var(--bg-dark);
            color: #f3f4f6;
            font-family: 'Plus Jakarta Sans', sans-serif;
            overflow-x: hidden;
            min-height: 100vh;
        }

        h1, h2 { font-family: 'Playfair Display', serif; }

        /* Background Layers */
        #three-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1; /* Lowest */
            pointer-events: none;
        }

        .aurora-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            overflow: hidden;
            background: radial-gradient(circle at 50% 50%, #1c1917 0%, #0c0a09 100%);
        }

        .aurora-layer {
            position: absolute;
            width: 150%;
            height: 150%;
            top: -25%;
            left: -25%;
            filter: blur(80px);
            opacity: 0.4;
            mix-blend-mode: screen;
            animation: drift 20s infinite alternate ease-in-out;
        }
        .aurora-1 { background: radial-gradient(circle at 20% 30%, hsla(333, 70%, 55%, 0.4) 0%, transparent 50%); }
        .aurora-2 { background: radial-gradient(circle at 80% 70%, hsla(282, 82%, 54%, 0.3) 0%, transparent 50%); animation-delay: -5s; }
        .aurora-3 { background: radial-gradient(circle at 40% 80%, hsla(210, 89%, 60%, 0.3) 0%, transparent 50%); animation-delay: -10s; }

        @keyframes drift {
            from { transform: rotate(0deg) scale(1); }
            to { transform: rotate(10deg) scale(1.1); }
        }

        /* UI Layer */
        main {
            position: relative;
            z-index: 10; /* Ensure content is above everything */
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .glass-card {
            background: rgba(15, 12, 11, 0.7);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7);
            border-radius: 24px;
            width: 100%;
            max-width: 500px;
        }

        .text-gradient {
            background: linear-gradient(to bottom right, #ffffff, #f9a8d4, #f472b6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--primary-pink), var(--primary-red));
            box-shadow: 0 10px 20px rgba(236, 72, 153, 0.3);
            transition: all 0.3s ease;
        }
        .btn-primary:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 15px 30px rgba(236, 72, 153, 0.5);
        }

        #progress-container {
            position: fixed;
            top: 25px;
            left: 50%;
            transform: translateX(-50%);
            width: 150px;
            height: 4px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            z-index: 100;
        }
        #progress-bar {
            width: 20%;
            height: 100%;
            background: linear-gradient(90deg, var(--primary-pink), var(--primary-red));
            border-radius: 10px;
            transition: width 0.5s ease;
        }

        .step-content {
            display: none;
            opacity: 0;
            transform: translateY(20px);
        }

        /* Show the active step */
        .step-content.active {
            display: flex;
            opacity: 1;
            transform: translateY(0);
            transition: opacity 0.8s ease, transform 0.8s ease;
        }

        .polaroid-frame {
            background: white;
            padding: 10px 10px 35px 10px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.4);
            transform: rotate(-2deg);
        }

        .heart-beat { animation: heartbeat 1.5s infinite; }
        @keyframes heartbeat {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.15); }
        }
    </style>
</head>
<body>

    <div id="progress-container">
        <div id="progress-bar"></div>
    </div>

    <div class="aurora-container">
        <div class="aurora-layer aurora-1"></div>
        <div class="aurora-layer aurora-2"></div>
        <div class="aurora-layer aurora-3"></div>
    </div>
    
    <canvas id="three-canvas"></canvas>

    <main>
        <!-- Step 1: The Grand Welcome -->
        <div id="step-1" class="step-content active flex-col items-center text-center glass-card p-10">
            <div class="text-6xl mb-6 heart-beat">❤️</div>
            <h1 class="text-4xl font-bold mb-4 text-gradient">Hello Mrs. Talha,</h1>
            <p class="text-gray-300 mb-8 leading-relaxed">I built a little world for you, just to bring a smile to your face on your special day.</p>
            <button onclick="nextStep(2)" class="btn-primary px-8 py-3 rounded-full font-semibold text-white uppercase tracking-widest text-xs">
                Let's Begin
            </button>
        </div>

        <!-- Step 2: Greeting -->
        <div id="step-2" class="step-content flex-col items-center text-center glass-card p-10">
            <div class="text-6xl mb-6">🎉</div>
            <h1 class="text-4xl font-bold mb-4 text-gradient">Happy Birthday!</h1>
            <p class="text-gray-300 mb-8">Another year of you making the world brighter. Your existence is a gift, and I'm so lucky to witness it.</p>
            <button onclick="nextStep(3)" class="btn-primary px-8 py-3 rounded-full font-semibold text-white">
                There's more...
            </button>
        </div>

        <!-- Step 3: Bento Grid -->
        <div id="step-3" class="step-content flex-col items-center max-w-4xl w-full">
            <h2 class="text-3xl font-bold mb-8 text-gradient text-center">A Few Things I Adore About You</h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-8 w-full">
                <div class="md:col-span-2 glass-card p-6">
                    <h3 class="text-pink-400 font-bold mb-2">✨ Your Unmatched Love</h3>
                    <p class="text-sm text-gray-300">It feels like home in every moment, holds my heart with quiet strength, and fills my life with a warmth I never knew I needed.</p>
                </div>
                <div class="glass-card p-6">
                    <h3 class="text-pink-400 font-bold mb-2">😊 That Smile</h3>
                    <p class="text-sm text-gray-300">Soft, warm, and impossible to ignore.</p>
                </div>
                <div class="md:col-span-3 glass-card p-6">
                    <h3 class="text-pink-400 font-bold mb-2">🌟 Your Radiant Spirit</h3>
                    <p class="text-sm text-gray-300">Your passion for life is infectious. Being around you makes everything feel more exciting.</p>
                </div>
            </div>
            <button onclick="nextStep(4)" class="btn-primary px-8 py-3 rounded-full font-semibold text-white">
                A story to tell
            </button>
        </div>

        <!-- Step 4: Polaroid -->
        <div id="step-4" class="step-content flex-col items-center text-center glass-card p-8">
            <h2 class="text-2xl font-bold mb-6 text-gradient">Remember Our Journey?</h2>
            <div class="polaroid-frame mb-6">
                <img src="https://i.postimg.cc/fWqQWgrs/WE.jpg" alt="Memory" class="w-56 h-56 object-cover bg-gray-200">
                <p class="text-gray-800 font-serif italic mt-2">The day we got married?</p>
            </div>
            <p class="text-gray-300 mb-6 italic text-sm">"Every moment with you feels like a scene from my dreams I used to see before."</p>
            <button onclick="nextStep(5)" class="btn-primary px-8 py-3 rounded-full font-semibold text-white">
                One last thing...
            </button>
        </div>

        <!-- Step 5: Final -->
        <div id="step-5" class="step-content flex-col items-center text-center glass-card p-10">
            <div class="text-6xl mb-6">🎂</div>
            <h2 class="text-3xl font-bold mb-4 text-gradient">My Wish For You</h2>
            <p class="text-gray-300 mb-8">May Allah bring you all the love, success, and pure happiness you so rightfully deserve.May your dreams soar higher than ever.</p>
            <div id="final-message" class="hidden mb-8 scale-0">
                <h3 class="text-xl font-bold text-pink-400">Happy Birthday. Many many happy returns of the day, Samsun Nahar Mitu, my world! ❤️</h3>
            </div>
            <button id="celebrate-btn" onclick="startFinale()" class="btn-primary px-10 py-4 rounded-full font-semibold text-white">
                LET'S CELEBRATE!
            </button>
        </div>
    </main>

    <script>
        // --- Navigation ---
        let currentStep = 1;
        function nextStep(step) {
            const currentEl = document.getElementById(`step-${currentStep}`);
            const nextEl = document.getElementById(`step-${step}`);
            
            document.getElementById('progress-bar').style.width = `${(step / 5) * 100}%`;

            currentEl.style.opacity = '0';
            currentEl.style.transform = 'translateY(-20px)';
            
            setTimeout(() => {
                currentEl.classList.remove('active');
                nextEl.classList.add('active');
                currentStep = step;
            }, 400);
        }

        // --- Three.js Background ---
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ canvas: document.getElementById('three-canvas'), alpha: true, antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        camera.position.z = 15;

        const heartShape = new THREE.Shape();
        heartShape.moveTo(0, 0);
        heartShape.bezierCurveTo(0, -0.3, -0.6, -0.3, -0.6, 0);
        heartShape.bezierCurveTo(-0.6, 0.3, 0, 0.6, 0, 1);
        heartShape.bezierCurveTo(0, 0.6, 0.6, 0.3, 0.6, 0);
        heartShape.bezierCurveTo(0.6, -0.3, 0, -0.3, 0, 0);

        const geometry = new THREE.ExtrudeGeometry(heartShape, { depth: 0.2, bevelEnabled: true, bevelThickness: 0.1, bevelSize: 0.1 });
        const hearts = [];
        const colors = [0xec4899, 0xf472b6, 0xef4444];

        for (let i = 0; i < 25; i++) {
            const material = new THREE.MeshPhongMaterial({ color: colors[i % 3], shininess: 100 });
            const heart = new THREE.Mesh(geometry, material);
            heart.position.set((Math.random() - 0.5) * 30, (Math.random() - 0.5) * 20, (Math.random() - 0.5) * 10);
            heart.rotation.x = Math.PI;
            heart.scale.set(0.6, 0.6, 0.6);
            heart.userData = { speed: 0.01 + Math.random() * 0.02, offset: Math.random() * 10 };
            scene.add(heart);
            hearts.push(heart);
        }

        const light = new THREE.PointLight(0xffffff, 1.5);
        light.position.set(0, 5, 10);
        scene.add(light);
        scene.add(new THREE.AmbientLight(0xffffff, 0.8));

        function animate() {
            requestAnimationFrame(animate);
            hearts.forEach(h => {
                h.position.y += Math.sin(Date.now() * 0.001 + h.userData.offset) * 0.01;
                h.rotation.y += 0.01;
            });
            renderer.render(scene, camera);
        }
        animate();

        // --- Finale ---
        function startFinale() {
            const btn = document.getElementById('celebrate-btn');
            const msg = document.getElementById('final-message');
            
            btn.style.display = 'none';
            msg.classList.remove('hidden');
            gsap.to(msg, { scale: 1, duration: 1, ease: "back.out" });

            confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 }, colors: ['#ec4899', '#ef4444', '#fbbf24'] });
            
            // Heart Swirl Out
            hearts.forEach((h, i) => {
                gsap.to(h.position, { y: 30, x: (Math.random() - 0.5) * 50, duration: 3, delay: i * 0.05 });
            });
        }

        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
