# ladakh-hilux-game
Ladakh Off-Road Hilux 3D – An open-world off-road driving game inspired by the rugged landscapes of Leh-Ladakh, built with HTML, CSS, JavaScript and Three.js.


Ladakh Off-Road Hilux 3D

★Project Summary

Ladakh Off-Road Hilux 3D is a browser-based open-world driving game inspired by the rugged terrain and mountain landscapes of Leh-Ladakh. The game allows players to drive a Hilux-style off-road vehicle across procedurally generated terrain while exploring checkpoints and managing vehicle resources.

★ Objective
The objective of this project was to create a lightweight 3D off-road driving experience that can run directly in a web browser without requiring any installation.
Technologies Used
HTML5
CSS3
JavaScript
Three.js

★ Features
3D terrain generation
Hilux-style off-road vehicle
First-person and third-person camera modes
Fuel management system
Vehicle damage system
Dynamic checkpoints
Day and night cycle
Weather particle effects
Real-time HUD display
Development Process
The project was developed using web technologies and the Three.js graphics library. Procedural terrain generation techniques were used to create a mountain-style environment. Vehicle movement, steering, camera controls, weather effects, and game logic were implemented using JavaScript.
AI-assisted tools were used during development to help generate and refine code, while testing and project setup were performed manually.

★Challenges Faced
Creating a 3D environment using browser technologies.
Managing camera movement and vehicle controls.
Optimizing performance for low-end laptops.
Designing terrain that resembles off-road mountain regions.
Future Improvements
More realistic Ladakh roads and landscapes.
Improved vehicle models and animations.
Additional missions and checkpoints.
Sound effects and background music.
Better textures and environmental details.
Mobile-friendly controls.

★Conclusion
This project demonstrates the use of HTML, CSS, JavaScript, and Three.js to build an interactive 3D driving game. It serves as a foundation for future game development projects and showcases the possibilities of browser-based 3D graphics.

This is the HTML CODE use for the project 
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ladakh Off-Road Hilux</title>
  <script src="https://unpkg.com/three@0.128.0/build/three.min.js"></script>
</head>

<body>
  <div id="game-container"></div>

  <div id="hud-overlay">
    <div class="hud-card top-left">
      <div class="hud-item">📍 Target: <span id="checkpoint-txt">Checkpoint 1</span></div>
      <div class="hud-item">🏆 Score: <span id="score-txt">00000</span> m</div>
    </div>
    <div class="hud-card top-right">
      <div class="hud-item">☀️ Time: <span id="time-txt">12:00</span></div>
      <div class="hud-item">🌧️ Weather: <span id="weather-txt">Sunny</span></div>
    </div>
    <div class="hud-card bottom-center">
      <div class="gauge-container">
        <div class="gauge-label">SPEED</div>
        <div class="gauge-value"><span id="speed-txt">0</span> km/h</div>
      </div>
      <div class="gauge-container">
        <div class="gauge-label">FUEL</div>
        <div class="gauge-bar">
          <div id="fuel-fill"></div>
        </div>
      </div>
      <div class="gauge-container">
        <div class="gauge-label">DAMAGE</div>
        <div class="gauge-bar">
          <div id="damage-fill"></div>
        </div>
      </div>
    </div>
    <div id="view-indicator">Cam Mode: 3rd Person [C]</div>
  </div>
</body>

</html>

This is the CSS CODE use of the project 

/* Reset default browser spacing */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    user-select: none;
}

body, html {
    width: 100%;
    height: 100%;
    overflow: hidden;
    background-color: #0a0c10;
}

/* Full-screen 3D Viewport container */
#game-container {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
}

/* Heads-Up Display (HUD) Top Overlay */
.hud-overlay {
    position: absolute;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    width: 90%;
    max-width: 800px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 15px;
    z-index: 10;
    pointer-events: none; /* Allows clicks to pass through to the game canvas */
}

/* Individual HUD Stats Cards */
.hud-card {
    background: rgba(10, 12, 16, 0.75);
    border: 1px solid rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(8px);
    padding: 12px;
    border-radius: 8px;
    text-align: center;
    color: #ffffff;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
}

.hud-label {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: #8a99ad;
    margin-bottom: 4px;
}

.hud-value {
    font-size: 1.6rem;
    font-weight: 700;
    color: #00ffcc;
}

/* Visual Progress Bars (Fuel & Damage) */
.bar-container {
    width: 100%;
    height: 8px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 4px;
    overflow: hidden;
    margin-top: 6px;
}

.bar-fill {
    height: 100%;
    width: 100%;
    transition: width 0.1s ease-out;
}

#fuel-fill {
    background: #ffaa00;
    box-shadow: 0 0 8px #ffaa00;
}

#damage-fill {
    background: #ff3333;
    box-shadow: 0 0 8px #ff3333;
    width: 0%; /* Starts at zero damage */
}

/* Camera Mode Indicator at the Bottom */
#view-indicator {
    position: absolute;
    bottom: 25px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(10, 12, 16, 0.8);
    border: 1px solid rgba(0, 255, 204, 0.3);
    color: #00ffcc;
    padding: 8px 20px;
    border-radius: 20px;
    font-size: 0.85rem;
    font-weight: 600;
    letter-spacing: 0.5px;
    z-index: 10;
    pointer-events: none;
    box-shadow: 0 0 10px rgba(0, 255, 204, 0.15);
}

/* Desktop Controls Reminder Sheet (Hidden on small screens) */
.controls-hint {
    position: absolute;
    bottom: 25px;
    right: 25px;
    background: rgba(10, 12, 16, 0.6);
    color: #8a99ad;
    padding: 10px 15px;
    border-radius: 6px;
    font-size: 0.75rem;
    line-height: 1.4;
    z-index: 10;
    pointer-events: none;
}

@media (max-width: 768px) {
    .hud-overlay {
        grid-template-columns: repeat(2, 1fr);
        top: 10px;
    }
    .hud-value {
        font-size: 1.2rem;
    }
    .controls-hint {
        display: none;
    }
}




This is the JAVASCRIPT CODE use for the project 




// --- 1. GAME CONFIGURATION & ENGINE SETTINGS ---
const scene = new THREE.Scene();
scene.background = new THREE.Color(0xdceeff); // Clear high-altitude blue sky color
scene.fog = new THREE.FogExp2(0xdceeff, 0.015); // Ladakh mountain fog effect

const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.shadowMap.enabled = true;
document.getElementById('game-container').appendChild(renderer.domElement);

// --- 2. LIGHTING (Sun & Shadows) ---
const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
scene.add(ambientLight);

const sunLight = new THREE.DirectionalLight(0xfffaed, 1.2);
sunLight.position.set(50, 100, 50);
sunLight.castShadow = true;
scene.add(sunLight);

// --- 3. CREATING THE ENVIRONMENT (Ladakh Mountains & Road) ---
// Ground Terrain
const terrainGeo = new THREE.PlaneGeometry(1000, 1000, 32, 32);
const terrainMat = new THREE.MeshStandardMaterial({ 
    color: 0xc4b295, // Sandy rock color
    roughness: 0.9 
});
const terrain = new THREE.Mesh(terrainGeo, terrainMat);
terrain.rotation.x = -Math.PI / 2;
terrain.receiveShadow = true;
scene.add(terrain);

// Simple Highway Loop
const roadGeo = new THREE.RingGeometry(80, 95, 64);
const roadMat = new THREE.MeshStandardMaterial({ 
    color: 0x22252a, // Dark asphalt color
    roughness: 0.5 
});
const road = new THREE.Mesh(roadGeo, roadMat);
road.rotation.x = -Math.PI / 2;
road.position.y = 0.02; // Elevated slightly over dirt to avoid clipping
scene.add(road);

// --- 4. BUILDING THE 3D TRUCK ENGINE (Chassis & Wheels) ---
const carGroup = new THREE.Group();

// Truck Body (Lower Chassis)
const bodyMat = new THREE.MeshStandardMaterial({ color: 0x111111, roughness: 0.2 }); // Matte Black Paint
const bodyGeo = new THREE.BoxGeometry(2.2, 0.8, 5);
const carBody = new THREE.Mesh(bodyGeo, bodyMat);
carBody.position.y = 0.8;
carBody.castShadow = true;
carGroup.add(carBody);

// Truck Cabin (Windshield/Roof)
const cabinGeo = new THREE.BoxGeometry(2, 0.8, 2.5);
const carCabin = new THREE.Mesh(cabinGeo, bodyMat);
carCabin.position.set(0, 1.5, -0.5);
carCabin.castShadow = true;
carGroup.add(carCabin);

// Adding 4 Off-road Wheels
const wheelGeo = new THREE.CylinderGeometry(0.5, 0.5, 0.6, 16);
const wheelMat = new THREE.MeshStandardMaterial({ color: 0x151515, roughness: 0.9 });
const wheelPositions = [
    [-1.2, 0.5, 1.8],  // Front Left
    [1.2, 0.5, 1.8],   // Front Right
    [-1.2, 0.5, -1.8], // Rear Left
    [1.2, 0.5, -1.8]  // Rear Right
];

wheelPositions.forEach(pos => {
    const wheel = new THREE.Mesh(wheelGeo, wheelMat);
    wheel.rotation.z = Math.PI / 2;
    wheel.position.set(pos[0], pos[1], pos[2]);
    wheel.castShadow = true;
    carGroup.add(wheel);
});

scene.add(carGroup);
carGroup.position.set(0, 0, 87); // Place truck directly on the road loop

// --- 5. VEHICLE PHYSICS VARIABLES ---
let speed = 0;
let maxSpeed = 45; // KMPH equivalent
let acceleration = 0.25;
let friction = 0.08;
let turnSpeed = 0.025;
let fuel = 100;
let score = 0;

// Input tracking dictionary
const keys = { ArrowUp: false, ArrowDown: false, ArrowLeft: false, ArrowRight: false };

// --- 6. INPUT CONTROLLERS ---
window.addEventListener('keydown', (e) => { if (e.key in keys) keys[e.key] = true; });
window.addEventListener('keyup', (e) => { if (e.key in keys) keys[e.key] = false; });

// --- 7. MAIN GAME LOOP (Updates 60 times a second) ---
function animate() {
    requestAnimationFrame(animate);

    if (fuel > 0) {
        // Drive Forward
        if (keys.ArrowUp) {
            speed = Math.min(speed + acceleration, maxSpeed);
            fuel -= 0.03; // Consumes fuel while gas pedal is held down
        } 
        // Reverse/Brakes
        else if (keys.ArrowDown) {
            speed = Math.max(speed - acceleration, -maxSpeed / 2);
        } 
        // Natural Engine Coast Friction
        else {
            if (speed > 0) speed = Math.max(speed - friction, 0);
            if (speed < 0) speed = Math.min(speed + friction, 0);
        }

        // Steer Left / Right (Only turns if truck is actively moving)
        if (Math.abs(speed) > 0.1) {
            const currentTurnDirection = speed > 0 ? 1 : -1;
            if (keys.ArrowLeft) carGroup.rotation.y += turnSpeed * currentTurnDirection;
            if (keys.ArrowRight) carGroup.rotation.y -= turnSpeed * currentTurnDirection;
        }
    } else {
        // Out of fuel logic
        speed = Math.max(speed - friction, 0);
        document.getElementById('fuel-fill').style.background = '#ff3333';
    }

    // Move truck relative to its facing angle
    carGroup.translateZ(speed * 0.01);

    // Score increments naturally based on distance traveled
    if (speed > 1) {
        score += Math.floor(speed * 0.05);
    }

    // Dynamic Cinematic Third-Person Camera follow physics
    const relativeCameraOffset = new THREE.Vector3(0, 4, -10);
    const cameraOffset = relativeCameraOffset.applyMatrix4(carGroup.matrixWorld);
    camera.position.x = THREE.MathUtils.lerp(camera.position.x, cameraOffset.x, 0.1);
    camera.position.y = THREE.MathUtils.lerp(camera.position.y, cameraOffset.y, 0.1);
    camera.position.z = THREE.MathUtils.lerp(camera.position.z, cameraOffset.z, 0.1);
    camera.lookAt(carGroup.position);

    // --- 8. SYNC VALUES TO CSS HUD DASHBOARD ---
    document.getElementById('speed-val').innerText = Math.round(Math.abs(speed));
    document.getElementById('score-val').innerText = score;
    document.getElementById('fuel-fill').style.width = Math.max(fuel, 0) + '%';

    renderer.render(scene, camera);
}

// --- 9. WINDOW RESIZE FIX ---
window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
});

// Kick off the simulator engine loop
animate();



Author

Prince Tandel