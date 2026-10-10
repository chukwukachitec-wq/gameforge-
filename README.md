# GameForge Studio — GitHub / Render starter

GameForge Studio is a prototype web interface plus an Express API that can queue Godot Android **debug APK** exports. This repository is prepared for GitHub and includes a Render Blueprint (`render.yaml`) and Dockerfile.

## Important before making it public

**This is a prototype, not a production-ready public APK-building service.** It currently has no user accounts or authentication, job state is stored in memory, and uploaded projects are processed by a build worker. Do not use it for sensitive projects or advertise it as a secure commercial service until authentication, durable storage/queueing, worker isolation, quotas, abuse controls, and retention are implemented. Render's free filesystem is ephemeral, so uploaded files and generated APKs can disappear after restart or redeploy. Free services may spin down when idle, and APK builds may exceed free resource limits.

The browser mini-game in the site is an HTML demo. It is **not** itself a Godot project and cannot be exported to APK by the builder. To build an APK, upload a ZIP containing a real Godot project with `project.godot` and `export_presets.cfg`, including an Android export preset named exactly `Android`.

## Files

- `index.html` — GameForge Studio interface
- `server/index.js` — Express API and Godot export worker
- `Dockerfile` — container image with Godot CI tools and Node.js
- `render.yaml` — Render Blueprint configuration
- `docker-compose.yml` — local Docker run configuration

## Put this project on GitHub using Android

1. Download `GameForge_Studio_GitHub.zip` from this conversation and extract it on your device if your file manager supports ZIP extraction.
2. Open [GitHub](https://github.com/) and sign in, then tap **+** → **New repository**.
3. Name it `gameforge-studio`. For a first deployment, set it to **Private** because this prototype has no authentication. Do not upload secrets, signing keystores, or private user projects.
4. Create the repository. If GitHub's mobile page doesn't offer folder upload, open the repository in your browser and use **Add file** → **Upload files**. Upload the *contents* of this folder, including `Dockerfile`, `render.yaml`, `.dockerignore`, and `.gitignore` (if hidden files aren't selectable, add those separately or use GitHub's desktop-mode view).
5. Commit the files to the default branch, usually `main`.
6. Open the Render dashboard, choose **New** → **Blueprint**, connect GitHub if prompted, select `gameforge-studio`, and apply the `render.yaml` configuration. Review the plan and any charges before confirming.
7. Watch the Render build and deploy logs. A successful deploy should have a Render URL and `https://YOUR-SERVICE.onrender.com/api/health` should return JSON with `"ok": true`.

If Render does not offer a free Docker web service for this configuration or the Godot image exceeds its resource limits, stop rather than choosing a paid plan without approval. We can instead deploy the static website separately and discuss a suitable host for the APK worker.

## Local run

Requires Docker. Run:

```sh
docker compose up --build
```

Then open `http://localhost:3000`. Health endpoint: `http://localhost:3000/api/health`.

## Godot project requirements

1. Open your game in Godot 4.4.x and verify it runs.
2. Install matching export templates and configure the Android SDK/JDK.
3. In **Project → Export**, add an Android preset named exactly `Android`.
4. Zip the Godot project folder containing `project.godot` and `export_presets.cfg`. Exclude `.godot` cache files and secrets.

The builder currently produces debug APKs only. Release signing and Play Store publishing are not configured. Never upload your private signing keystore to a shared build service.

## Current limitations

- No sign-in, admin dashboard, trial enforcement, payment processing, or $1/day billing is implemented.
- Job history is in memory and can reset on restart.
- The Render free filesystem is temporary; files and APKs are not durable.
- This project has not been deployed yet. Creating a GitHub repository and completing a successful Render deployment are separate steps.
