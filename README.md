<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>IV Cannulation Master - Innovation Fair Prototype</title>
    <script src="https://aframe.io"></script>
    <script>
      let currentStep = 1;

      AFRAME.registerComponent('cannulation-logic', {
        init: function () {
          const instructions = document.querySelector('#step-text');
          const armVeins = document.querySelector('#target-veins');
          const ivCatheter = document.querySelector('#iv-catheter');
          const swabBox = document.querySelector('#swab-box');

          // Step 1: Site Sanitization
          swabBox.addEventListener('click', () => {
            if (currentStep === 1) {
              instructions.setAttribute('value', 'STEP 2: Stare at the blue ring on the upper arm to apply the Tourniquet.');
              instructions.setAttribute('color', '#333333');
              swabBox.setAttribute('color', '#888888'); 
              currentStep = 2;
            }
          });

          // Step 2: Tourniquet Application
          document.querySelector('#tourniquet-zone').addEventListener('click', () => {
            if (currentStep === 2) {
              instructions.setAttribute('value', 'STEP 3: Stare at the highlighted blue vein to insert the IV Cannula.');
              instructions.setAttribute('color', '#333333');
              armVeins.setAttribute('visible', 'true'); 
              currentStep = 3;
            } else if (currentStep < 2) {
              instructions.setAttribute('value', 'CRITICAL ERROR: Sanitize the area with the Swab first!');
              instructions.setAttribute('color', '#ff0000');
            }
          });

          // Step 3: Needle Puncture & Flow Check
          armVeins.addEventListener('click', () => {
            if (currentStep === 3) {
              instructions.setAttribute('value', 'SUCCESS! Flashback observed. Catheter secured safely.');
              instructions.setAttribute('color', '#00aa00');
              ivCatheter.setAttribute('position', '0.1 0.95 -1.05'); 
              ivCatheter.setAttribute('color', '#ff0000'); 
              currentStep = 4;
            } else if (currentStep < 3) {
              instructions.setAttribute('value', 'CRITICAL ERROR: Cannot puncture! Apply tourniquet first.');
              instructions.setAttribute('color', '#ff0000');
            }
          });
        }
      });
    </script>
  </head>
  <body>
    <a-scene>
      
      <!-- CENTRAL ASSET PRELOADER (Matches your exact downloaded filenames) -->
      <!-- If you upload them as .zip files, change the .glb extension below to .zip -->
      <a-assets timeout="10000">
        <a-asset-item id="arm-model" src="./hand_arm.glb"></a-asset-item>
        <a-asset-item id="bed-model" src="./hospital.bed.glb"></a-asset-item>
        <a-asset-item id="stand-model" src="./iv_drip_stand.glb"></a-asset-item>
        <a-asset-item id="monitor-model" src="./operating_room_roof_monitors.glb"></a-asset-item>
      </a-assets>

      <a-sky color="#f4f9fa"></a-sky>
      <a-plane position="0 0 0" rotation="-90 0 0" width="30" height="30" color="#e5ecef"></a-plane>

      <!-- HUD Interaction Text Panel -->
      <a-text id="step-text" value="STEP 1: Stare at the green Alcohol Swab on the tray to sanitize hands." 
              position="0 2.3 -1.8" color="#2c3e50" width="3.8" align="center"></a-text>

      <!-- 1. The Hospital Bed -->
      <a-gltf-model src="#bed-model" position="0 0 -1.5" scale="1 1 1" rotation="0 0 0"></a-gltf-model>
      <a-box position="0 0.4 -1.5" width="1.2" height="0.8" depth="2.2" color="#ffffff" opacity="0.1"></a-box>

      <!-- 2. Interactive Hand & Arm Placement -->
      <a-gltf-model src="#arm-model" position="0.1 0.85 -1.1" scale="0.7 0.7 0.7" rotation="0 15 0" cannulation-logic></a-gltf-model>

      <!-- 3. IV Drip Stand -->
      <a-gltf-model src="#stand-model" position="-0.6 0 -1.8" scale="0.9 0.9 0.9"></a-gltf-model>

      <!-- 4. Patient Monitor System -->
      <a-gltf-model src="#monitor-model" position="-0.7 1.2 -1.2" scale="0.5 0.5 0.5" rotation="0 45 0"></a-gltf-model>

      <!-- WORKFLOW GAZE HOTSPOTS -->
      <a-torus id="tourniquet-zone" position="-0.15 0.9 -1.35" radius="0.06" radius-tubular="0.007" rotation="0 75 0" color="#0000ff" class="clickable"></a-torus>
      <a-cylinder id="target-veins" position="0.18 0.88 -0.98" radius="0.005" height="0.2" rotation="5 0 80" color="#4ba3e3" visible="false" class="clickable"></a-cylinder>

      <!-- Equipment Mayo Tray -->
      <a-box position="0.6 0.8 -0.9" width="0.4" height="0.02" depth="0.4" color="#cbd5e1"></a-box>
      <a-cylinder position="0.6 0.4 -0.9" radius="0.02" height="0.8" color="#94a3b8"></a-cylinder>

      <!-- Swab & Catheter elements -->
      <a-box id="swab-box" position="0.55 0.82 -0.9" width="0.06" height="0.02" depth="0.06" color="#00ff00" class="clickable"></a-box>
      <a-cone id="iv-catheter" position="0.65 0.82 -0.9" radius-bottom="0.008" height="0.08" rotation="90 15 0" color="#ffcc00"></a-cone>

      <!-- Reticle Camera Setup -->
      
        <a-camera>
          <a-cursor color="#ff0000" fuse="true" fuse-timeout="1500" raycaster="objects: .clickable"></a-cursor>
        </a-camera>
      

    </a-scene>
  </body>
</html>
