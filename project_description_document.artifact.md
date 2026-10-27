# PocketLawyer: Comprehensive Project Description Document

## 1. Executive Summary & Abstract

**PocketLawyer** (`com.pocketlawyer.in`) is an intuitive, modern, native Android mobile application designed to bridge the gap between complex legal frameworks in India and everyday citizens. Recognizing that legal jargon, bureaucratic procedures, and language barriers often prevent individuals from exercising their constitutional and statutory rights, PocketLawyer delivers simple, accessible, multi-lingual legal assistance directly to users' smartphones.

The application combines state-of-the-art Generative Artificial Intelligence (Google Gemini API), automated legal document generation (A4 PDF complaints), an emergency Response & SOS safety system, a categorized legal rights repository, and an interactive 100-question legal knowledge quiz. Full support is provided in **English**, **Hindi (हिन्दी)**, and **Tamil (தமிழ்)**.

---

## 2. Project Objectives

1. **Democratize Legal Knowledge**: Provide plain-language explanations of current Indian laws (BNS, BNSS, BSA, Consumer Protection Act 2019, POSH Act 2013, Motor Vehicles Act 2019, RTI Act 2005, DPDP Act 2023).
2. **Break Language Barriers**: Deliver 100% complete localization in English, Hindi, and Tamil for UI screens, AI chatbot responses, and generated complaint letters.
3. **Automate Legal Documentation**: Allow users to draft, view, share, and download formal, legally formatted A4 complaint PDF letters in under 2 minutes.
4. **Enhance Personal Safety**: Provide a single-tap Emergency SOS system that acquires GPS location and dispatches SMS alerts to a saved emergency contact while providing instant dial shortcuts to national helplines (112, 100, 1091, 1930, 15100).
5. **Interactive Public Legal Literacy**: Offer a dynamic 100-question legal quiz with randomized 5-question trials and option shuffling to educate users on practical legal rights.

---

## 3. Technology Stack & Architecture

| Component | Technology / Library |
| :--- | :--- |
| **Language & Runtime** | Kotlin 2.0.21 / Java 17 |
| **UI Toolkit** | Jetpack Compose (Compose BOM 2025.01.01) with Material 3 Design |
| **Architecture Pattern** | MVVM (Model-View-ViewModel) + Jetpack Navigation Compose |
| **Authentication** | Android Credential Manager API (`androidx.credentials`) & Google ID Token Library |
| **Asynchronous Programming** | Kotlin Coroutines & StateFlow / SharedFlow |
| **Local Database** | Room Database 2.6.1 (`chat_history`, `complaint_history`, `quiz_history`) |
| **Key-Value Persistence** | Jetpack DataStore Preferences 1.1.2 (User sessions & emergency contacts) |
| **Networking & AI** | Retrofit 2.11.0, Gson, OkHttp3 Logging Interceptor & Google Gemini API |
| **Document Generation** | Native `android.graphics.pdf.PdfDocument` & Android `MediaStore.Downloads` API |
| **Localization** | `AppCompatDelegate` In-App Locales with `values/`, `values-hi/`, and `values-ta/` resources |

---

## 4. Key Modules & Features

### Module 1: User Authentication & Onboarding
* **Google Sign-In**: Integrated with Android Credential Manager API (`GetSignInWithGoogleOption` with `GetGoogleIdOption` fallback for new account selection).
* **Guest Mode**: Allows instant, anonymous access for users who prefer not to sign in immediately.
* **Session Persistence**: Persists user name, email, profile photo URI, and emergency contact via Jetpack DataStore.

---

### Module 2: Emergency SOS Safety System
* **One-Tap Emergency Alert**: Pressing the red SOS button acquires GPS/Network location coordinates, formats a Google Maps link, and automatically dispatches an emergency SMS.
* **Strict 10-Digit Mobile Validation**: SOS setup enforces strict 10-digit mobile number validation with real-time error feedback and white text styling.
* **Helpline Dialing Shortcuts**: Direct one-tap phone call triggers for:
  * **Police Emergency**: 100 / 112
  * **Women Helpline**: 1091 / 181
  * **Cyber Crime Helpline**: 1930
  * **Free Legal Aid (NALSA)**: 15100
  * **Personal Contact**: Saved 10-digit emergency contact.

---

### Module 3: AI Legal Assistant (Legal Chatbot)
* **Google Gemini AI Integration**: Powered by `gemini-3.5-flash` model (`generativelanguage.googleapis.com`).
* **Strict Multilingual Mandate**: System instructions mandate 100% response output in the active app language (Tamil script for Tamil, Hindi script for Hindi, English for English).
* **Structured Legal Answers**: Formats complex legal advice into four practical sections:
  1. Applicable Laws
  2. Your Rights
  3. Suggested Next Steps
  4. Required Documents
* **Topic-Tailored Fallback Engine**: If offline or facing network constraints, an intelligent keyword-matching fallback provides category-specific legal guidance tailored to the user's prompt topic.

---

### Module 4: Complaint Generator
* **4-Step Guided Wizard**:
  * **Step 1: Category Selection** (Police Station, Cyber Crime Cell, Consumer Disputes, Workplace/POSH, Landlord/Tenancy, Other).
  * **Step 2: Date & Location Details** (Integrated DatePicker dialog).
  * **Step 3: Complainant Name & Incident Description**.
  * **Step 4: Review Summary & PDF Generation**.
* **Formal A4 PDF Compilation**: Automatically formats the complaint with formal recipient addresses, subject lines, salutations, body paragraphs, closing requests, signature blocks, and disclaimer footers.
* **1-Click Public PDF Download**: Uses Android's `MediaStore.Downloads` API (Android 10–15) to save generated complaint PDFs directly into the user's public **Downloads** directory (`PocketLawyer_Complaint_<timestamp>.pdf`) and opens it in the default PDF Viewer.
* **Sharing**: Integrated `FileProvider` sharing via email or messaging apps.

---

### Module 5: Know Your Rights (Knowledge Base)
* **Categorized Legal Cards**:
  * Consumer Rights (COPRA 2019)
  * Women's Rights (POSH Act 2013, Domestic Violence Act 2005)
  * Employee Rights (Payment of Wages, Industrial Disputes)
  * Cyber Laws (IT Act 2000)
  * Traffic Rules (Motor Vehicles Act 2019)
  * Tenant Rights (Model Tenancy Act)
  * Digital Privacy (DPDP Act 2023)
* **Real-time Search Bar**: Filter rights topics instantly by keyword.

---

### Module 6: Dynamic 100-Question Legal Quiz
* **100-Question Pool**: Covers 10 distinct legal domains in Indian Law.
* **Dynamic 5-Question Shuffling**: Every trial randomly selects 5 questions (`QuizRepository.getRandomQuiz(5)`).
* **Option Shuffling**: Shuffles choices (`A`, `B`, `C`, `D`) for every question on every attempt so correct answers are balanced and randomized across all positions.
* **Instant Feedback & Scoring**: Visual green/red card borders with detailed legal explanations for every question.
* **Score Persistence**: Automatically saves quiz scores and history into Room Database.

---

### Module 7: Nearby Legal Help
* **Geographic Map Search**: One-tap shortcuts opening Google Maps for nearby:
  * Legal Aid Centers (NALSA / State Authorities)
  * Police Stations & Cyber Cells
  * Consumer Dispute Courts
  * Women Support Centers

---

### Module 8: History & Activity Log
* **Unified History**: View past AI Chat conversations, generated complaint letter summaries, and quiz scores stored locally in Room Database with timestamp sorting and detail inspection.

---

## 5. Security & Privacy Features

1. **Local Data Sovereignty**: All user complaint history, chat logs, and quiz results remain stored on the local device inside Room Database.
2. **Google Play App Signing & SHA-1/SHA-256 Fingerprinting**: Fully configured for Google OAuth 2.0 authentication and Play Integrity API.
3. **ProGuard / R8 Protection**: Built-in rules in `proguard-rules.pro` preserve Credential Manager, Google ID libraries, and network models while stripping debug logs in release builds.

---

## 6. Build & Version Release Specifications

* **Package Name**: `com.pocketlawyer.in`
* **Version Code**: `8`
* **Version Name**: `1.7`
* **Release Artifacts**:
  1. **Release AAB Bundle**: `PocketLawyer/app/build/outputs/bundle/release/app-release.aab`
  2. **Repository ZIP Archive**: `PocketLawyer_Repository_v1.7.zip`
