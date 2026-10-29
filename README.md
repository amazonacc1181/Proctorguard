# ProctorGuard v0.1

A static, GitHub Pages-ready prototype of a browser-based proctored practice exam.

## Features

- Webcam permission, live preview, and check-in photo capture.
- Face detection in the browser using face-api.js and its Tiny Face Detector model.
- Timed 10-, 20-, or 30-minute mock test with 15 original NEET-style sample questions.
- Physics, Chemistry, and Biology question groups.
- +4 correct / −1 incorrect scoring, answer clearing, review markers, question palette, keyboard shortcuts, and automatic time-up submission.
- Subject-wise performance, explanations, monitoring event log, and downloadable JSON report.
- Advisory alerts for repeated face absence, multiple faces, exam-tab hidden state, and camera interruption.
- Responsive layout and consent before exam start.

## Run locally

Camera access requires HTTPS or localhost. From this directory run:

```bash
python -m http.server 8000
```

Open http://localhost:8000.

## Publish with GitHub Pages

Repository: https://github.com/amazonacc1181/Proctorguard

In repository Settings → Pages, choose **Deploy from a branch**, select `main` and `/(root)`, and save. Then wait for GitHub Pages to publish the site. Expected URL: https://amazonacc1181.github.io/Proctorguard/.

## Important limitations

This is a learning prototype, not suitable for official or high-stakes exams. Face detection does not verify identity, perform liveness checks, or reliably determine whether a candidate is cheating. The current version does not implement gaze tracking, object detection, or reliable head-turn analysis. Browser focus and camera events can have legitimate causes. Alerts are advisory and never automatically disqualify the candidate.

Camera frames and the captured photo are processed/stored in the browser tab; they are not uploaded by this app. The JSON report excludes the photo and video. The app loads face-api.js from jsDelivr and model weights from a public face-api.js model host, so an internet connection is required. GitHub Pages is static hosting and does not provide user accounts, an admin dashboard, server-side audit logs, or secure result storage.

## Technology

HTML, CSS, vanilla JavaScript, face-api.js 0.22.2, GitHub Pages.

## License

MIT.
