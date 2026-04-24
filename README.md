# 🍽️ Kitchen AI Inspector

> AI-powered food quality and kitchen hygiene inspection — built for Google Solution Challenge 2026

[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?logo=flutter)](https://flutter.dev)
[![Gemini AI](https://img.shields.io/badge/Gemini-2.0%20Flash-4285F4?logo=google)](https://ai.google.dev)
[![SDG 2](https://img.shields.io/badge/SDG-2%20Zero%20Hunger-DDA63A)](https://sdgs.un.org/goals/goal2)
[![SDG 3](https://img.shields.io/badge/SDG-3%20Good%20Health-4C9F38)](https://sdgs.un.org/goals/goal3)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📌 Problem Statement

Every year, **600 million people** fall ill from unsafe food — and over **420,000 die** as a result (WHO, 2022). In homes, small restaurants, and street kitchens across the developing world, there is no affordable, accessible way to verify food quality before it reaches a plate.

Professional food inspectors are expensive, scarce, and unavailable in real time. The result: contaminated, spoiled, or improperly prepared food silently harms millions of people who have no means to know any better.

**Kitchen AI Inspector solves this** by putting a professional-grade food quality inspector in everyone's pocket — completely free, offline-capable for image capture, and powered by Google's Gemini AI.

---

## 💡 Solution

Kitchen AI Inspector is a Flutter mobile app that uses **Google Gemini 2.0 Flash's multimodal vision capabilities** to analyse photos of food plates and kitchen items in real time.

A user simply points their camera at a dish or ingredient, taps **Scan Plate**, and within seconds receives:

- A **PASS / FAIL** safety verdict
- A **hygiene score out of 100**
- A breakdown of specific **issues found** (contamination, spoilage, poor plating hygiene)
- Actionable **recommendations** to correct problems

No training required. No expensive equipment. No waiting for an inspector.

---

## 🌍 UN Sustainable Development Goal Alignment

### SDG 2 — Zero Hunger
> *"End hunger, achieve food security and improved nutrition, and promote sustainable agriculture"*

Food spoilage and contamination are among the leading causes of food waste globally. By identifying failing food **before it is served or discarded unnecessarily**, Kitchen AI Inspector helps:

- Reduce food waste from incorrectly identified spoilage
- Empower small food businesses and home cooks to maintain quality standards
- Enable safer food handling in communities with limited access to formal food safety education

### SDG 3 — Good Health and Well-Being
> *"Ensure healthy lives and promote well-being for all at all ages"*

Foodborne illnesses disproportionately affect low-income communities that cannot afford professional food safety services. Kitchen AI Inspector helps:

- Prevent foodborne illness by catching contamination before consumption
- Democratise access to food safety knowledge previously available only to large food businesses
- Provide actionable health guidance instantly, in any language Gemini supports

---

## ✨ Key Features

| Feature | Description |
|---|---|
| AI-powered inspection | Gemini 2.0 Flash analyses food images with expert-level criteria |
| Pass / Fail verdict | Instant binary safety decision with a confidence score |
| Hygiene score (0–100) | Granular quality rating for tracking improvement over time |
| Issues & recommendations | Specific problems identified with corrective actions |
| Scan history | Full session history with thumbnails, scores, and timestamps |
| Live stats banner | Pass/fail counts and average score across all scans |
| Detail bottom sheet | Tap any history item to review its full AI report |
| Shimmer loading | Polished loading state while Gemini processes the image |
| Secure API key | Key injected at build time via `--dart-define`, never hardcoded |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Framework | Flutter 3.x (Dart) |
| AI / Vision | Google Gemini 2.0 Flash (`google_generative_ai`) |
| Image input | `image_picker` (camera + gallery) |
| UI polish | `shimmer` for loading states |
| State management | Flutter `setState` (built-in) |
| Target platforms | Android, iOS |

---

## 🏗️ Architecture

```
lib/
└── main.dart               # Single-file app (MyApp → MyHomePage → _MyHomePageState)

Key responsibilities inside _MyHomePageState:
├── _analyzeFoodQuality()   # Picks image → calls Gemini → parses JSON → updates state
├── _parseGeminiJson()      # Strips markdown fences, decodes structured JSON response
├── _buildCurrentResultCard()  # Renders the latest scan result card
├── _buildStatsBanner()     # Live pass/fail/avg-score stats
├── _buildHistoryTile()     # Each item in the scrollable history list
└── _buildFAB()             # Animated floating action button with pulse effect
```

### How the AI inspection works

```
User taps "Scan Plate"
        │
        ▼
image_picker opens gallery / camera
        │
        ▼
Image bytes read → sent to Gemini 2.0 Flash
with a structured prompt requesting JSON output:
  { pass, score, summary, issues[], recommendations[] }
        │
        ▼
Response parsed → _scanHistory updated → UI rebuilds
```

The Gemini prompt instructs the model to act as a professional food safety inspector with 20 years of experience, evaluating five criteria: **freshness, colour, contamination, plating hygiene, and portion presentation**.

---

## 🚀 Getting Started

### Prerequisites

- Flutter SDK `>=3.0.0` — [Install Flutter](https://flutter.dev/docs/get-started/install)
- Dart SDK `>=3.0.0` (bundled with Flutter)
- A valid **Google Gemini API key** — [Get one free at Google AI Studio](https://aistudio.google.com/app/apikey)
- Android emulator or physical device (Android 6.0+ / iOS 13+)

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/kitchen-ai-inspector.git
cd kitchen-ai-inspector
```

### 2. Install dependencies

```bash
flutter pub get
```

### 3. Verify `pubspec.yaml` dependencies

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_generative_ai: ^0.4.6
  image_picker: ^1.1.2
  shimmer: ^3.0.0
```

### 4. Run the app

Replace `YOUR_GEMINI_KEY` with your actual API key:

```bash
flutter run --dart-define=GEMINI_KEY=YOUR_GEMINI_KEY
```

> ⚠️ **Never commit your API key to source control.** The app reads the key via `String.fromEnvironment('GEMINI_KEY')` at compile time. The source code contains only a placeholder fallback for local development.

### 5. Build a release APK (for submission)

```bash
flutter build apk --release --dart-define=GEMINI_KEY=YOUR_GEMINI_KEY
```

The APK will be output to:
```
build/app/outputs/flutter-apk/app-release.apk
```

---

## 📱 Android Permissions

Add the following to `android/app/src/main/AndroidManifest.xml` inside the `<manifest>` tag:

```xml
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES"/>
<uses-permission android:name="android.permission.INTERNET"/>
```

Also add inside `<application>`:

```xml
<provider
    android:name="androidx.core.content.FileProvider"
    android:authorities="${applicationId}.fileprovider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_paths"/>
</provider>
```

---

## 🍏 iOS Permissions

Add the following to `ios/Runner/Info.plist`:

```xml
<key>NSCameraUsageDescription</key>
<string>Kitchen AI Inspector needs camera access to scan food plates.</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>Kitchen AI Inspector needs gallery access to select food photos.</string>
```

---

## 🔐 API Key Security

| Environment | How to provide the key |
|---|---|
| Local development | `flutter run --dart-define=GEMINI_KEY=xxx` |
| CI/CD | Set `GEMINI_KEY` as a secret environment variable |
| Release build | `flutter build apk --dart-define=GEMINI_KEY=xxx` |
| Firebase Studio | Add to run configuration's additional args |

The key is baked into the compiled binary at build time and is never present in the Dart source code or version control.

---



---

## 🗺️ Roadmap

- [x] Gemini 2.0 Flash multimodal food inspection
- [x] Structured JSON response parsing (pass/fail/score/issues/recommendations)
- [x] Scan history with thumbnails and timestamps
- [x] Live stats banner (pass count, fail count, average score)
- [x] Shimmer loading animation
- [x] Secure API key via `--dart-define`
- [ ] Camera source choice (camera vs gallery bottom sheet)
- [ ] Persistent history across app restarts (`shared_preferences`)
- [ ] Onboarding splash screen with SDG badges
- [ ] Multi-language support via Gemini's multilingual output
- [ ] Export scan report as PDF
- [ ] Offline mode with on-device ML fallback

---

## 👥 Team

| Name | Role |
|---|---|
| Debargha Sikdar | Developer & Designer |

**Institution:** *(Your University / College)*
**Country:** India
**Event:** Google Solution Challenge 2026

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- [Google Gemini AI](https://ai.google.dev) — multimodal vision model powering the inspection engine
- [Flutter](https://flutter.dev) — cross-platform mobile framework
- [Google Solution Challenge](https://developers.google.com/community/gdsc-solution-challenge) — for the opportunity to build technology that matters
- [World Health Organization](https://www.who.int/news-room/fact-sheets/detail/food-safety) — food safety statistics cited in this README
- [UN Sustainable Development Goals](https://sdgs.un.org) — SDG 2 and SDG 3 frameworks

---

<div align="center">

**Built with ❤️ for Google Solution Challenge 2026**

*Making food safety accessible to everyone, everywhere.*

</div>
