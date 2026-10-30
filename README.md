# window

A WebGPU ocean that turns your TV into a window. An iPhone sits under the TV,
mirrors the picture to it with AirPlay, and uses its front camera to follow
you. As you walk across the room the view shifts like looking through glass:
step right and you see more of the sea to the left, step closer and the view
opens up.

Deployed on Vercel as a static site (no build step). Any HTTPS host works.

## How it works

- **Tracking.** MediaPipe Pose (default) finds your eyes and shoulders in the
  front-camera image. Shoulder width and torso length give your distance;
  the eye position gives left/right and height. "Face" mode is more precise
  but only works within about 1.5 m. Everything runs on the phone; no video
  leaves it.
- **Window projection.** The picture on the TV is treated as a physical
  opening of known size. Each frame builds an off-axis (asymmetric) frustum
  from your eyes through the edges of the picture, and the virtual camera
  moves with your head. Smoothing uses a One Euro filter.
- **Phone performance.** The render size is capped at 1280 px on the long
  side (`?maxw=1600` to change, `?maxw=0` for full resolution).

## Setup

1. **Deploy:** import the repo in Vercel with the "Other" framework preset and
   no build command; `index.html` is served as is. The camera needs HTTPS,
   which Vercel provides.
2. **On the iPhone**, open the page in Safari, then Share → Add to Home
   Screen, and launch it from the Home Screen icon. That gives a full-screen
   picture with no Safari bars on the TV.
3. **Place the phone** in landscape, screen facing the room, just below the
   centre of the TV. Keep it on a charger; it will run warm.
4. **Mirror to the TV:** Control Centre → Screen Mirroring → your TV. Turn off
   rotation lock so the phone stays landscape.
5. Tap **Start head tracking** and allow the camera.
6. Tap ⚙︎ (bottom right) to open settings:
   - **TV size** is preset to 42".
   - **Camera left/right of centre**: the front camera is at one end of the
     phone, so if the phone is centred, set about +7 or −7 cm (positive = to
     your right as you face the TV).
   - **Camera below picture**: distance from the bottom of the picture to the
     lens.
   - Stand where you normally watch, measure the distance from your eyes to
     the screen, type it in, and tap **Calibrate**. This fixes the distance
     scale and puts the horizon at your eye height.
   - If the view moves the wrong way when you step sideways, tick
     **Flip left/right**.
   - **Show camera preview** helps check that it can see you.
   Close the panel with ⚙︎ again so it isn't on the TV.

Drag on the phone screen to aim the window. Settings are saved on the phone.

## Tuning

- **Parallax depth ×** exaggerates how far the camera moves with you.
- **Field of view ×** sets how wide the view is. 1 is a physically exact
  window, which is very narrow on a 42" TV, so the default is 3. The sideways
  shift as you move always follows real window geometry.
- **Zoom as you walk in/out** sets how much the view widens as you walk up to
  the TV (1 = exact, 0 = not at all; default 0.35).
- **Window frame** draws a frame with a reveal and glazing bars at the screen.
  It barely moves, while the bars on the glass shift slightly against the sea.
  **Frame depth** sets how deep the reveal is.
- **Smoothing** trades jitter for lag. AirPlay already adds some lag.
- **Height above water** lower values put the waves closer, so they shift
  more as you move.

URL options: `?window=0` gives the original free-flying camera
(desktop: click for mouse look, WASD to move, H for the panel);
`?tracker=face` starts in face mode.
