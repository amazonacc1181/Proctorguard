# ProctorGuard v0.2

A GitHub Pages-ready browser-based proctored practice exam prototype. This version adds live face-template comparison to earlier face-count monitoring.

## Features

- Camera permission, live preview, and check-in photo capture.
- **Single-face check-in:** requires exactly one visible face before creating the session reference.
- **Live face matching:** creates a face descriptor from the check-in image and compares it with live webcam descriptors throughout the session.
- **Multiple-face alerts:** repeated frames with more than one visible face trigger an advisory warning.
- **Possible identity mismatch alerts:** repeated high-distance comparisons against the reference are flagged; borderline comparisons are reported as uncertain instead of a mismatch.
- SSD MobileNet V1 face detection, 68-point landmarks, and face-recognition descriptors using face-api.js.
- Reference descriptors, frames, and comparisons remain in browser memory; this app does not upload them to a server.
- Timed 10-, 20-, or 30-minute mock test with 15 original NEET-style sample questions.
- Physics, Chemistry, and Biology groups, +4/−1 scoring, answer clearing, review markers, question palette, and keyboard shortcuts.
- Subject-wise performance, explanations, advisory event log, and downloadable JSON report.
- Responsive layout and consent before exam start.

## Run locally

Camera access requires HTTPS or localhost. From this directory run:

```bash
python -m http.server 8000
```

Open http://localhost:8000.

## Publish with GitHub Pages

Repository: https://github.com/amazonacc1181/Proctorguard

In repository Settings → Pages, choose **Deploy from a branch**, select `main` and `/(root)`, and save. The repository also contains a GitHub Actions Pages workflow. Expected URL: https://amazonacc1181.github.io/Proctorguard/.

## Important security and accuracy limitations

This is an experimental educational prototype, **not a high-stakes proctoring or identity-verification system**. Face descriptors can differ for the same person across lighting, pose, camera quality, distance, expression, and time; different people can also produce misleading similarities. Thresholds here are heuristic and have not been validated on a representative dataset. The page reports an *identity match likely*, *identity uncertain*, or *identity mismatch suspected*—it never proves identity or misconduct.

This version does **not** include presentation-attack/liveness detection. A photograph or video of the registered candidate may fool a face matcher. It cannot detect people or devices outside the camera's field of view, and it cannot guarantee that another person will not take over between checks. The browser can also be manipulated by the exam taker. For an official exam, use a professionally evaluated identity-verification and liveness system, robust back-end session logging, a human review process, accessibility accommodations, and a privacy/security assessment.

The check-in photo and face descriptor are held only in this browser tab's memory and are excluded from the JSON report. Refreshing or leaving the page discards the in-memory reference. The app loads face-api.js from jsDelivr and model weights from a public model host, so internet access is needed. GitHub Pages is static hosting; there are no accounts, server-side audit logs, admin dashboard, or secure result storage.

## Technology

HTML, CSS, vanilla JavaScript, face-api.js 0.22.2, GitHub Pages.

## License

MIT.


## V3 backend preview

The V3 work is being developed on the `feature/proctorguard-v3` branch so the stable V2 app remains unchanged. See [V3-README.md](V3-README.md) for API setup, environment variables, question-bank schema, and endpoints. The V3 interface is [v3.html](v3.html); it requires a separately hosted Node.js API and a reviewed 180-question bank. GitHub Pages alone cannot host the backend. Do not use this preview for high-stakes examinations until the security, accessibility, privacy, backup, and deployment reviews are complete.
