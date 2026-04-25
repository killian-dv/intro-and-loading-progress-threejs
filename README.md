# Intro and Loading Progress — Three.js Journey

Quick recap of the **Intro and Loading Progress** lesson from [Three.js Journey](https://threejsjourney.com/) by Bruno Simon.

## What this project covers

This project demonstrates how to build a polished loading experience for a Three.js scene while assets are being fetched and prepared. It combines Three.js loaders, a `LoadingManager`, DOM-based progress feedback, and a shader-based overlay transition to make startup feel smooth and intentional.

- **Centralized loading flow** using `THREE.LoadingManager` shared by `GLTFLoader` and `CubeTextureLoader`.
- **Real-time loading progress bar** updated from `onProgress` through a simple `scaleX(...)` transform.
- **Smooth loading end transition** with GSAP: fade out a full-screen shader overlay and finalize loading bar styles.
- **Environment and model loading** with a cube map and the `FlightHelmet` glTF model.
- **Scene quality setup** with shadows, tone mapping, orbit controls, and responsive resize handling.

## What I built

- Created a Three.js scene with `PerspectiveCamera`, `OrbitControls`, and a shadow-enabled `WebGLRenderer`.
- Added a shared `LoadingManager` and passed it to all loaders so progress is tracked globally.
- Connected `loadingManager.onProgress` to a DOM loading bar (`.loading-bar`) using a normalized `loaded / total` ratio.
- Implemented `loadingManager.onLoad` to:
  - wait briefly before transition
  - animate `uAlpha` from `1` to `0` on a full-screen overlay shader
  - mark the loading bar as ended
- Built a full-screen overlay mesh (`PlaneGeometry`) with a custom `ShaderMaterial` to hide scene pop-in.
- Loaded:
  - a cube environment map used for `scene.background` and `scene.environment`
  - the `FlightHelmet` glTF model with custom transform values
- Added a material update pass over the loaded scene graph to apply:
  - consistent environment intensity
  - cast/receive shadow behavior for mesh materials
- Configured renderer output with `ReinhardToneMapping`, exposure boost, and soft shadow mapping.
- Added viewport resize handling for camera projection and renderer resolution/pixel ratio.

## What I learned

### 1) How to coordinate multiple asset loaders

- `THREE.LoadingManager` acts as a shared lifecycle hook for all loaders using it.
- You can track total loading progress without manually counting each file type.
- This keeps startup logic centralized and easier to reason about.

### 2) How to drive UI feedback from loading state

- `onProgress` provides `loaded` and `total`, which are enough for a clean progress ratio.
- A simple CSS transform (`scaleX`) gives a performant visual loading indicator.
- Hooking loader events to DOM state improves perceived responsiveness immediately.

### 3) Why an overlay transition improves perceived quality

- Assets may finish loading at different moments and can pop into view harshly.
- A full-screen shader overlay hides those intermediate states.
- Fading the overlay with GSAP produces a controlled, cinematic reveal of the final scene.

### 4) How environment maps and PBR assets fit early in setup

- Applying a cube map to both background and environment gives instant visual context.
- PBR models like `FlightHelmet` benefit from environment lighting and tone mapping.
- A consistent material update pass helps keep imported meshes visually coherent.

### 5) How to keep rendering robust during interaction

- Camera aspect/projection must be updated on resize to avoid distortion.
- Renderer size and pixel ratio should track viewport changes for stable quality.
- Orbit controls with damping make initial scene exploration smoother.

### 6) Startup polish is both technical and visual

- Technical loading correctness is only half the experience.
- Transition timing, progress communication, and final reveal strongly affect perceived performance.
- Even small additions (progress bar + fade overlay) make a scene feel significantly more professional.

## Run the project

```bash
npm install
npm run dev
```

## Credits

Part of the **Three.js Journey** course by Bruno Simon.
