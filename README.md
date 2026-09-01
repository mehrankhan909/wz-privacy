<div align="center">

<img src="/app_icon.png" width="120" />

# Wearzeno

**Your AI-Powered Personal Wardrobe Assistant**

*Add clothes you own → Choose an occasion → Get outfit recommendations from your existing wardrobe*

[![React Native](https://img.shields.io/badge/React%20Native-0.81.5-61DAFB?style=flat&logo=react)](https://reactnative.dev/)
[![Expo SDK](https://img.shields.io/badge/Expo%20SDK-54-000020?style=flat&logo=expo)](https://expo.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?style=flat&logo=typescript)](https://www.typescriptlang.org/)
[![NativeWind](https://img.shields.io/badge/NativeWind-4.1-06B6D4?style=flat)](https://www.nativewind.dev/)
[![Zustand](https://img.shields.io/badge/Zustand-5.0-443E38?style=flat)](https://zustand-demo.pmnd.rs/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

---

</div>

## Overview

Wearzeno is a **fully offline**, on-device wardrobe management and outfit recommendation app. No cloud AI, no backend, no internet required. Users photograph clothes they own, the app organizes them using on-device ML, and generates outfit combinations tailored to the occasion, style, and weather.

**Built for men** — masculine color palettes, tailored recommendations, and a premium dark-mode-first design.

---

## Features

### Core
- **Wardrobe Management** — Add, edit, delete clothing items with photos, colors, categories, patterns, and more
- **Outfit Generation** — AI-powered outfit recommendations based on color theory, formality, occasion, style, and season
- **Wear Tracking** — Log outfits you wear, track wear history, see most/least worn items
- **Outfit Rating** — Rate generated outfits 1-5 stars, track compliments received
- **Favorites** — Save your best outfit combinations for quick access

### Intelligence
- **On-Device ML** — TensorFlow Lite + MobileNetV2 for automatic clothing category detection from photos
- **Color Extraction** — Extract dominant colors from clothing photos using pixel analysis
- **Color Theory Engine** — Complementary, analogous, triadic, and split-complementary color harmonies
- **Weather Integration** — Real-time weather from Open-Meteo API with outfit suggestions based on temperature and conditions
- **Smart Matching** — Weighted scoring algorithm considering color harmony (25%), occasion fit (25%), clothing compatibility (15%), style match (15%), season match (10%), formality (10%)

### Design
- **Premium Dark Mode** — Gold accent (#f2ca50) on deep black (#131313)
- **Warm Light Mode** — Ivory background (#FBF8F1) with dark gold accents
- **Glass Morphism UI** — Translucent cards with subtle borders and shadows
- **Masculine Palettes** — Navy, charcoal, olive, burgundy, brown, teal color schemes
- **Custom Palettes** — Create and save your own color palettes

---

## Architecture

```
wearzeno/
├── app/                          # Expo Router file-based routes
│   ├── _layout.tsx               # Root layout (theme, onboarding redirect)
│   ├── (tabs)/                   # Bottom tab navigation
│   │   ├── _layout.tsx           # Tab bar: Home | Wardrobe | Combos | Settings
│   │   ├── index.tsx             # Home screen (weather, stats, quick actions)
│   │   ├── wardrobe.tsx          # Wardrobe grid with filters
│   │   ├── combos.tsx            # Combos landing page
│   │   └── profile.tsx           # Settings tab
│   ├── onboarding/               # 5-step onboarding flow
│   ├── wardrobe/                 # Add, analyze, confirm, edit items
│   ├── combos/                   # Generate, results, detail
│   ├── profile.tsx               # User Profile (header icon destination)
│   ├── favorites.tsx             # Saved outfit combos
│   ├── search.tsx                # Local search across wardrobe
│   ├── appearance.tsx            # Theme switching
│   └── wear-history.tsx          # Wear tracking timeline
├── components/ui/                # 14 reusable UI components
├── store/                        # 6 Zustand stores with AsyncStorage persistence
├── services/                     # Business logic (no UI)
│   ├── recommendation/           # Outfit generation engine
│   ├── local-ai/                 # TensorFlow Lite analyzer
│   ├── color-engine.ts           # Color theory & harmony
│   ├── color-extraction.ts       # Pixel-level color extraction
│   ├── weather-service.ts        # Open-Meteo weather API
│   └── image-service.ts          # Image picker & file operations
├── types/                        # TypeScript type definitions
├── constants/                    # Static data, categories, colors, seed data
├── hooks/                        # Custom React hooks
├── assets/                       # App icon, splash, TFLite model
└── doc/                          # Complete reference documentation
```

---

## Tech Stack

| Layer | Technology | Version |
|---|---|---|
| Platform | Expo SDK | 54 |
| Framework | React Native | 0.81.5 |
| React | React | 19.1.0 |
| Navigation | Expo Router | ~6.0.24 |
| Styling | NativeWind + Tailwind CSS | 4.1.23 / 3.4.17 |
| State | Zustand | 5.0.15 |
| Persistence | AsyncStorage | 2.2.0 |
| Language | TypeScript | ~5.9.2 (strict) |
| ML Runtime | react-native-fast-tflite | ^3.0.1 |
| ML Model | MobileNetV2 | 1.0_224 (14MB) |
| Weather | Open-Meteo API | Free tier |
| Icons | @expo/vector-icons | ^15.0.3 |
| Animations | React Native Reanimated | ~4.1.1 |

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) >= 18
- [Expo CLI](https://docs.expo.dev/get-started/installation/) (`npm install -g expo-cli`)
- [EAS CLI](https://docs.expo.dev/build/setup/) (`npm install -g eas-cli`)
- Android Studio (for emulator) or physical device with Expo Go

### Installation

```bash
# Clone the repository
git clone https://github.com/ASO-team/wearzeno.git
cd wearzeno

# Install dependencies
npm install

# Start development server
npx expo start
```

### Running

```bash
# Expo Go (limited features — no TFLite, no full storage access)
npx expo start

# Android development build (full features)
npx expo run:android

# Build APK via EAS
eas build --profile preview --platform android
```

> **Note:** Expo Go has limitations — TFLite ML and full media library access require a development build.

---

## How It Works

### 1. Add Clothes
Take a photo or pick from gallery → app auto-extracts colors → select/adjust category, pattern, style, formality, seasons, occasions → save to wardrobe.

### 2. Choose Occasion
Pick an occasion (Office, Wedding, Casual, etc.), style preference, formality level, color palette, and season.

### 3. Get Recommendations
The engine generates all valid top + bottom + shoe combinations, scores each with a weighted formula, filters low scores, and returns the top 10 ranked outfits.

### 4. Track & Rate
Log outfits you wear, rate them, track your most-worn items, and build your style profile over time.

---

## Recommendation Engine

**File:** `services/recommendation/engine.ts`

The engine uses a **weighted multi-criteria scoring** system:

| Criterion | Weight | Description |
|---|---|---|
| Color Harmony | 25% | Complementary, analogous, triadic matching |
| Occasion Fit | 25% | Does the outfit match the target occasion? |
| Clothing Compatibility | 15% | Category pairing rules (e.g., suit needs formal shoes) |
| Style Match | 15% | Classic, Modern, Minimal, Bold alignment |
| Season Match | 10% | Weather-appropriate fabric and color choices |
| Formality Match | 10% | Casual ↔ Formal spectrum alignment |
| Favorite Color | Bonus | User's preferred colors get priority |

**Minimum wardrobe:** 3 items (1 top + 1 bottom + 1 shoes)

---

## State Management

6 Zustand stores with AsyncStorage persistence:

| Store | Key | Purpose |
|---|---|---|
| `app-store` | `wearzeno-app` | Theme, onboarding status, app ready state |
| `wardrobe-store` | `wearzeno-wardrobe` | All clothing items |
| `preference-store` | `wearzeno-preferences` | Occasions, styles, favorite colors, formality |
| `combo-store` | `wearzeno-favorites` | Generated combos, favorites, last params |
| `wear-store` | `wearzeno-wear` | Wear logs, stats |
| `rating-store` | `wearzeno-ratings` | Outfit ratings, compliments |
| `palette-store` | `wearzeno-palettes` | Custom color palettes |

---

## Design System

### Dark Mode
- Background: `#131313`
- Surface: `#2a2a2a`
- Primary (gold): `#f2ca50`
- Primary text on gold: `#3c2f00`
- Body text: `#e5e2e1`
- Secondary text: `#d0c5af`
- Outlines: `#4d4635`

### Light Mode
- Background: `#FBF8F1` (warm ivory)
- Primary: `#6B5C00` (dark gold)
- Body text: `#1B1B1B`

### Typography
- **Playfair Display** (serif): Display headlines
- **Libre Franklin** (sans): Body text, labels, buttons

---

## Documentation

Complete reference docs in the `doc/` directory:

| File | Contents |
|---|---|
| `doc/overview.md` | App identity, tech stack, principles |
| `doc/architecture.md` | Project structure, data flow |
| `doc/types.md` | All TypeScript types |
| `doc/stores.md` | Zustand stores, shapes |
| `doc/screens.md` | Every screen, route, navigation |
| `doc/components.md` | All reusable UI components |
| `doc/services.md` | Recommendation engine, ML, weather |
| `doc/design-system.md` | Stitch tokens, colors, typography |
| `doc/navigation.md` | Expo Router routes |
| `doc/data-models.md` | Wardrobe, outfit, preferences |
| `doc/constants.md` | Categories, occasions, colors |
| `doc/dev-guide.md` | Setup, running, conventions |

---

## Building

### Preview APK
```bash
eas build --profile preview --platform android
```

### Production AAB (Play Store)
```bash
eas build --profile production --platform android
```

### Build Profiles (`eas.json`)
- **preview**: APK for testing (development client)
- **production**: AAB for Play Store submission

---

## Project Highlights

- **100% Offline** — No cloud APIs, no backend, no internet dependency
- **On-Device ML** — TensorFlow Lite with MobileNetV2 for clothing recognition
- **Real-Time Weather** — Open-Meteo API with 1-hour caching
- **Zero TypeScript Errors** — Strict mode enforced
- **Masculine Design** — Purpose-built for men's fashion
- **Seed Data** — 8 sample items loaded on first launch for demo

---

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Code Style
- Use `useTheme()` for colors — never hardcode
- Use `useFileSystem()` for file operations
- Components use NativeWind classes, not StyleSheet
- No `any` types (strict mode enforced)
- Services don't import React components
- Run `npx tsc --noEmit` before committing

---

## App Store & Play Store Compliance

### iOS App Store
- **Privacy manifest** (`NSPrivacyTracking`, `NSPrivacyAccessedAPITypes`) included in `app.json`
- **Usage descriptions** for Camera, Photo Library, and Location with clear explanations
- **Data storage** — User-generated photos stored in Documents directory (backed up by default per Apple guidelines)
- **No encryption export compliance** (`ITSAppUsesNonExemptEncryption: false`)
- **No external purchases** — app is 100% free with no IAP
- **No data collection** — all data stays on device, no analytics, no tracking

### Google Play Store
- **Permissions declared** with proper usage descriptions
- **Data safety** — no data collected, no data shared, all processing on-device
- **Location permission** used solely for weather — disclosed in data safety form
- **Camera permission** used solely for user-initiated photo capture
- **No deceptive behavior** — app does exactly what description says

### Required Before Submission
1. **Privacy Policy** — Host at a public URL (e.g., GitHub Pages) and add to App Store Connect / Play Console
2. **Support URL** — Create a support page or GitHub Issues link
3. **Screenshots** — Provide screenshots for both iPhone and iPad (iOS), phone and tablet (Android)
4. **App Description** — Write a clear, accurate description of functionality
5. **Content Rating** — Fill out IARC questionnaire (Play Store) / age rating (App Store)

---

## License

This project is licensed under the MIT License.

---

<div align="center">

**Built with care for men who care about what they wear.**

</div>
