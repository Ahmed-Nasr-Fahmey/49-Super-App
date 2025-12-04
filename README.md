# 🚀 49 Super App

<div align="center">

**A multi-vertical Arabic super app packing 49+ services (ride hailing, delivery, social, auctions, health, food, marriage, media, chat, and more) into one seamless Flutter experience.**

[![Platform](https://img.shields.io/badge/Platform-iOS_&_Android-blue.svg)]()

[![Category](https://img.shields.io/badge/Category-Super_App_%2F_Lifestyle-purple.svg)]()

[![License](https://img.shields.io/badge/License-Private-red.svg)]()

[![Status](https://img.shields.io/badge/Release-In_Progress-orange.svg)]()  

[Features](#-features) • [Architecture](#-architecture) • [Tech Stack](#-tech-stack) • [Folder Structure](#-folder-structure) • [Download](#-download--installation) • [Screenshots](#-screenshots)

</div>

---

## 📱 Project Overview

**49** is a super app designed for the Egyptian and GCC market. It brings **49 primary categories** under one umbrella—rides, logistics, social networking, auctions, health, food delivery, marriage services, classifieds, chat, audio/video calls, reels, and everything in between.

My current focus areas:

- **Ride Hailing / Delivery (Client & Driver)**: supports full “Tawsila” (tracking/real-time) and “Tawsila Non-Tracking” ride types, plus “Tahmeela” logistics categories with live pricing, tracking, and driver earnings dashboards.
- **Social Hub**: real-time chats (personal & services), social feed, reels, Facebook, Instagram, stories, in-app audio/video calls.
- **Onboarding & Account**: client sign up, driver activation, document flows.

The app is still under active development (not yet public on the stores) with rapid iteration between product squads.

---

## 📥 Download & Installation

> Internal builds only (Firebase App Distribution / TestFlight). Public store release pending.

---

## 🌍 Environments & Flavors

Three flavors keep the giant codebase aligned with backend stacks:

- **Development (`development`)** – local builds for engineers, pointing to dev APIs, mock payment gateways, and debug Firebase project.
- **Staging (`staging`)** – QA/UAT environment mirroring production data structure to validate 49 categories before releases.
- **Production (`production`)** – hardened configuration for store releases. **Firebase Analytics** and **Firebase Crashlytics** are enabled **only** on `production` to keep telemetry clean.

Each flavor has its own bundle identifiers, Firebase options, Branch/Deep Link domains, and secrets for third-party SDKs (Agora, Map services, revenue stack, etc.).

---

## 🔄 CI/CD Pipeline

- **Android**
  - **GitHub Actions** watch feature branches (`dev`, `staging`, `release`).
  - Workflows run static analysis/tests, call **Fastlane** to build the right flavor, sign artifacts, and push to **Firebase App Distribution** for stakeholders.

- **iOS**
  - **Codemagic** pipelines manage Xcode builds per scheme/flavor, handle code signing, execute unit/widget tests, and deliver builds to TestFlight/internal testers.

Artifacts (APKs/AABs/IPAs) carry build metadata (commit hash, flavor, features toggled) to help QA track issues across the 49 feature modules.

---

## 🛠️ Tech Stack

### State Management & Navigation
- **flutter_bloc** / **Cubit** for feature domains (transport, chat, social, marketplace).
- **provider** for lightweight dependencies (e.g., locale, session).
- **go_router** for declarative nested navigation and deep links.

### Networking & Data
- **dio** with interceptors (auth, retries).
- **retrofit** or custom API clients for the microservice layer.
- **Hive** / **ObjectBox** / **SharedPreferences** for offline caches (chat history, trip drafts, media metadata).

### Real-time & Communications
- **Firebase Cloud Messaging** for push notifications.
- **Socket.io** for live ride updates and real-time chat.
- **Agora** / **WebRTC** for in-app voice & video calls.

### Social & Media
- Custom reels/stories player, UGC moderation stack, internal CDN for media uploads.

### Maps & Location
- **Google Maps SDK**, **geolocator**, **place picker** utilities for rider & driver journeys, plus route polylines and fare calculations.

### Analytics & Crash Reporting
- **Firebase Analytics**, **Crashlytics**, plus custom event streams per category.

### Tooling & Quality
- **build_runner**, **freezed**, **json_serializable**.
- **flutter_lints** + custom lint rules.
- Automated screenshot generation for design QA.

---

## 🏗️ Architecture

Feature-first Clean Architecture so each of the 49 categories can evolve independently:

```
┌─────────────────────────────────────┐
│ Presentation Layer                 │
│ (Screens, Widgets, BLoC/Cubits)    │
└─────────────────────────────────────┘
                 ↕
┌─────────────────────────────────────┐
│ Domain Layer                        │
│ (Entities, Use Cases, Repositories) │
└─────────────────────────────────────┘
                 ↕
┌─────────────────────────────────────┐
│ Data Layer                          │
│ (API + Local sources + Mappers)     │
└─────────────────────────────────────┘
```

Each major domain (Transport, Social, Media, Marketplace, Health, etc.) follows the same structure:

```
features/
  transport/
    data/
    domain/
    presentation/
  social_chat/
  stories_reels/
  marketplace/
  health/
  ...
```

Shared modules (`core/`) host common widgets (buttons, cards, timeline chips), services (network, analytics), theming, and localization (Arabic/English).

---

## ✨ Features

- **Ride Hailing & Delivery**
  - **Tawsila (Tracking + Real-time via Socket.io)**: Captain rides, Ladies rides, Taxi (اجرة), Governorates trips, Business rides, SUV, Scooter, Bicycle, Electric Scooter, TukTuk—each with live tracking, status events, fare updates, and driver/passenger coordination.
  - **Tawsila (Non-Tracking)**: Occasions limos, Microbus, Mini bus, School transport, Ambulance, Mortuary transport—scheduled services without continuous map tracking but with status confirmations and pricing.
  - **Tahmeela (Freight & Logistics)**: Tricycle, Errands/Requests, Quarter truck, Half truck, Recovery/Rescue, Cash-in-transit, Furniture moving, Heavy transport, Tow truck, Fluid tanker, Lorry, Refrigerated truck, Car carrier.
  - Multi-stop routing, live ETA, price adjustments, auto-matching with nearest driver.
  - Driver mode: availability toggle, request queue, trip acceptance, earnings dashboard, document status.

- **Trips Timeline**
  - Active, upcoming, and past trips with detailed route maps, fare receipts, and statuses.

- **Social & Communication**
  - Personal & service chats (end-to-end encrypted), typing indicators, read receipts.
  - Stories/Reels feed inspired by Instagram/TikTok with Spotlight/Snap tabs.
  - Facebook-/Instagram-style timeline posts; Twitter-like micro posts.
  - In-app audio/video calls using Agora/WebRTC.

- **Services Hub**
  - Access to dozens of other categories: marriage, auctions, classifieds, health bookings, food, charity, entertainment, etc. (Roadmap includes all 49 verticals).

- **Notifications & Alerts**
  - Smart notification center covering social interactions, ride updates, and category-specific alerts.

- **User Management**
  - Onboarding for riders and drivers, KYC flow, document uploads, multi-language support (Arabic default, English secondary).

---

## 📁 Folder Structure

```
lib/
├── core/
│   ├── config/            # env, flavor toggles
│   ├── constants/
│   ├── network/
│   ├── localization/
│   ├── router/
│   ├── services/          # analytics, notifications, sockets
│   ├── theme/
│   └── widgets/
│
├── features/
│   ├── auth/
│   ├── transport/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   ├── driver_mode/
│   ├── social_chat/
│   ├── stories_reels/
│   ├── marketplace/
│   ├── health/
│   ├── auctions/
│   ├── food/
│   ├── marriage/
│   └── ... (remaining categories)
│
├── generated/
└── main.dart
```

---

## 📸 Screenshots

<div align="center">

### Transport (Client & Driver)
<img src="https://github.com/user-attachments/assets/2eb7a1c0-e3e6-41d5-9d22-acc1ba01a635" alt="Client request with multi-stops and vehicle types" width="250"/>
<img src="https://github.com/user-attachments/assets/ae289d3f-110b-446b-a655-7d2df2dc6a13" alt="Driver mode availability and requests" width="250"/>
<img src="https://github.com/user-attachments/assets/3a2f45bc-f1ce-487c-a645-4200be2d068b" alt="Trip history with Google Maps trace" width="250"/>

### Pricing & Booking
<img src="https://github.com/user-attachments/assets/89b60347-847b-4d41-86af-52222c417342" alt="Vehicle options and fares" width="250"/>
<img src="https://github.com/user-attachments/assets/9e16e96e-8a5b-4b56-a4c8-50daa27d6b87" alt="Choose service type (ride, transport, errands)" width="250"/>

### Social Hub
<img src="https://github.com/user-attachments/assets/4e8f1858-3963-4379-ad92-beefcdc02a9a" alt="Social & service chats" width="250"/>
<img src="https://github.com/user-attachments/assets/3a4d8472-30df-4cba-8dcb-5b36537ddcea" alt="Reels / TikTok-style player" width="250"/>


### Live Trips
<img src="https://github.com/user-attachments/assets/b740b05a-c0e2-4d51-80fe-6290f38c086c" alt="Live trip waiting for driver offers" width="250"/>

</div>

---

## 👨‍💻 Developer & Contact

<div align="center">

### **Ahmed Nasr**

**Flutter Developer & Mobile App Specialist**

📧 **Email**: ahmed.nasr.fahmey@gmail.com  
🌐 **LinkedIn**: https://www.linkedin.com/in/ahmed-nasr-fahmey/

---

</div>

[⬆ Back to Top](#-49-super-app)


