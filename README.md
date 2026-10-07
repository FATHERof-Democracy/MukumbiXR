<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>IV Cannulation - Emergency Shape Test</title>
    <script src="https://aframe.io"></script>
    <script>
      let currentStep = 1;
      AFRAME.registerComponent('cannulation-logic', {
        init: function () {
          const instructions = document.querySelector('#step-text');
          const armVeins = document.querySelector('#target-veins');
          const ivCatheter = document.querySelector('#iv-catheter');
          const swabBox = document.querySelector('#swab-box');

          swabBox.addEventListener('click', () => {
            if (currentStep === 1) {
              instructions.setAttribute('value', 'STEP 2: Stare at the blue ring on the upper arm to apply the Tourniquet.');
              swabBox.setAttribute('color', '#888888'); 
              currentStep = 2;
            }
          });

          document.querySelector('#tourniquet-zone').addEventListener('click', () => {
            if (currentStep === 2) {
              instructions.setAttribute('value', 'STEP 3: Stare at the highlighted blue vein to insert the IV Cannula.');
              armVeins.setAttribute('visible', 'true'); 
              currentStep = 3;
            }
          });

          armVeins.addEventListener('click', () => {
            if (currentStep === 3) {
              instructions.setAttribute('value', 'SUCCESS! Flashback observed. Catheter secured safely.');
              ivCatheter.setAttribute('position', '0.1 0.95 -1.05'); 
              ivCatheter.setAttribute('color', '#ff0000'); 
              currentStep = 4;
            }
          });
        }
      });
    </script>
  </head>
  <body>
    <a-scene>
      <a-sky color="#f4f9fa"></a-sky>
      <a-plane position="0 0 0" rotation="-90 0 0" width="30" height="30" color="#e5ecef"></a-plane>

      <a-text id="step-text" value="STEP 1: Stare at the green Alcohol Swab on the tray to sanitize hands." 
              position="0 2.3 -1.8" color="#2c3e50" width="3.8" align="center"></a-text>

      <!-- Patient Bed Mockup Block -->
      <a-box position="0 0.4 -1.5" width="1.2" height="0.8" depth="2.2" color="#ffffff"></a-box>

      <!-- Patient Arm Mockup Cylinder -->
      <a-cylinder position="0.1 0.85 -1.1" radius="0.06" height="0.8" rotation="0 0 85" color="#e0a98c" cannulation-logic></a-cylinder>

      <!-- Workflow hot spots -->
      <a-torus id="tourniquet-zone" position="-0.15 0.9 -1.35" radius="0.06" radius-tubular="0.007" rotation="0 75 0" color="#0000ff" class="clickable"></a-torus>
      <a-cylinder id="target-veins" position="0.18 0.88 -0.98" radius="0.005" height="0.2" rotation="5 0 80" color="#4ba3e3" visible="false" class="clickable"></a-cylinder>

      <!-- Mayo Instrument Stand -->
      <a-box position="0.6 0.8 -0.9" width="0.4" height="0.02" depth="0.4" color="#cbd5e1"></a-box>
      <a-box id="swab-box" position="0.55 0.82 -0.9" width="0.06" height="0.02" depth="0.06" color="#00ff00" class="clickable"></a-box>
      <a-cone id="iv-catheter" position="0.65 0.82 -0.9" radius-bottom="0.008" height="0.08" rotation="90 15 0" color="#ffcc00"></a-cone>

      <!-- Camera reticle -->
      
        <a-camera>
          <a-cursor color="#ff0000" fuse="true" fuse-timeout="1500" raycaster="objects: .clickable"></a-cursor>
        </a-camera>
      
    </a-scene>
  </body>
</html>
