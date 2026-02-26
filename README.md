# Detooz - AI-Powered Scam Detection Platform

<div align="center">
<p>
  <img src="https://github.com/user-attachments/assets/39c2f858-2b58-4bf3-adce-030783d119fa"
     width="200"
     height="200"
     style="border-radius:25px;" />

<p>


**Protecting India from SMS, WhatsApp, and Telegram Scams**

[![Flutter](https://img.shields.io/badge/Flutter-3.6+-02569B?logo=flutter)](https://flutter.dev)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-009688?logo=fastapi)](https://fastapi.tiangolo.com)
[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python)](https://python.org)
[![License](https://img.shields.io/badge/License-Proprietary-red)]()

</div>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [ML Model Details](#ml-model-details)
- [Security & Privacy](#security--privacy)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

Detooz is an AI-powered mobile application designed to protect vulnerable users—particularly **senior citizens** and **students** in India—from financial scams across multiple messaging platforms.

### The Problem
- India has **1.2+ billion mobile users** receiving billions of SMS messages daily
- Scammers exploit urgency tactics, multilingual messages, and impersonation
- Victims often lack technical knowledge to identify sophisticated scams
- Existing solutions don't adequately support Indian regional languages
- Family members are unaware when loved ones receive scam messages

### The Solution
A comprehensive mobile app that:

- ✅ **Detects** scam messages in real-time across SMS, WhatsApp, and Telegram
- ✅ **Alerts** designated family guardians via push notifications when scams are detected
- ✅ **Educates** users about common scam patterns through curated content
- ✅ **Protects** offline with on-device machine learning (99%+ accuracy)
- ✅ **Supports** 9 Indian languages for UI and analyzes messages in 22+ languages

---

## ✨ Key Features

### 🛡️ Three-Tier Hybrid Detection System

| Tier | Method | Speed | Accuracy | Use Case |
|------|--------|-------|----------|----------|
| **Tier 1** | Pattern Matching | <10ms | ~90% | Instant detection of obvious scams |
| **Tier 2** | On-Device ML (MobileBERT) | 100-200ms | 99%+ | Offline privacy-focused detection |
| **Tier 3** | Cloud AI (Llama 3.3 70B) | 500-1500ms | 95%+ | Complex multilingual analysis |

### 👨‍👩‍👧‍👦 Guardian Alert System
- Link multiple family guardians via OTP verification
- Real-time push notifications when high-risk scams detected
- Customizable alert thresholds (HIGH/MEDIUM/ALL)
- Guardian dashboard to monitor protected users

### 📱 Multi-Platform Detection
- **SMS**: Real-time interception and analysis
- **WhatsApp**: Notification monitoring (WhatsApp + WhatsApp Business)
- **Telegram**: Notification monitoring
- **Images**: Screenshot analysis for fake payment screens and phishing

### 🌐 Multilingual Support
- **UI Languages**: English, Hindi, Bengali, Telugu, Marathi, Tamil, Gujarati, Kannada, Urdu
- **Message Analysis**: All 22 scheduled Indian languages + mixed languages (Hinglish, Tanglish, etc.)
- **Dynamic Translation**: Google ML Kit for real-time UI translation

### 📚 Education Hub
- Curated cybersecurity articles from trusted sources
- RSS feed aggregation (Quick Heal, GBHackers, Krebs on Security)
- AI-generated "Detooz Exclusive" educational content
- Scam type explanations and prevention tips

### 🔒 Privacy-First Design
- On-device ML for offline detection (no data sent to cloud)
- User-controlled data retention (default 365 days)
- GDPR-compliant consent management
- Anonymous scanning option
- Data anonymization for training (opt-in)

---

## 🏗️ Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                      FLUTTER MOBILE APP                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐                 │
│  │ SMS/WA/TG  │  │  On-Device │  │   UI Layer │                 │
│  │  Listener  │  │  ML Model  │  │ (9 Languages)│                │
│  └─────┬──────┘  └─────┬──────┘  └─────────────┘                 │
│        └───────────────┼────────────────────────────────────────┤
│                        ▼                                         │
│         ┌────────────────────────┐                              │
│         │  Hybrid Detection      │                              │
│         │  (3-Tier System)       │                              │
│         └──────────┬─────────────┘                              │
└────────────────────┼────────────────────────────────────────────┘
                     │ HTTPS
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                     FASTAPI BACKEND                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │   Pattern    │  │  Local ML    │  │   External APIs      │  │
│  │  Matching    │  │ (MobileBERT) │  │  (Groq, OpenRouter)  │  │
│  └──────────────┘  └──────────────┘  └──────────────────────┘  │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │  PostgreSQL  │  │    Redis     │  │   Guardian Alerts    │  │
│  │   Database   │  │    Cache     │  │   (FCM Push)         │  │
│  └──────────────┘  └──────────────┘  └──────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### Detection Flow

```
SMS Received → Pattern Check → Obvious Scam? → YES → HIGH RISK
                     ↓                              ↓
                 Uncertain                    Save to DB
                     ↓                              ↓
              On-Device ML → Confident? → YES → Alert Guardian
                     ↓                              
                 Uncertain                    
                     ↓                        
               Cloud AI Analysis
                     ↓
            Final Risk Assessment
```

---

## 🛠️ Technology Stack

### Mobile Application (Flutter)
| Component | Technology | Version | Purpose |
|-----------|------------|---------|---------|
| Framework | Flutter | 3.6+ | Cross-platform mobile development |
| Language | Dart | 3.6+ | Programming language |
| State Management | Riverpod | 2.6.1 | Reactive state management |
| Local Storage | Hive | 2.2.3 | Offline data storage |
| Secure Storage | flutter_secure_storage | 9.0.0 | Token/credential storage |
| HTTP Client | http | 1.2.0 | API communication |
| On-Device ML | tflite_flutter | 0.11.0 | ML model inference |
| Translation | google_mlkit_translation | 0.13.0 | UI localization |
| Push Notifications | firebase_messaging | 15.2.0 | FCM integration |
| Authentication | firebase_auth | 5.3.0 | Phone OTP |
| Google Sign-In | google_sign_in | 6.2.1 | OAuth authentication |

### Backend (Python)
| Component | Technology | Version | Purpose |
|-----------|------------|---------|---------|
| Framework | FastAPI | 1.3.0 | Async web framework |
| Language | Python | 3.11+ | Programming language |
| Database | PostgreSQL | 13+ | Primary data store |
| ORM | SQLAlchemy | 2.0+ | Database abstraction |
| Cache | Redis | 6+ | Session & rate limiting |
| Authentication | JWT (HS256) | - | Token-based auth |
| Password Hashing | bcrypt | 4.0.1 | Secure password storage |
| ML Framework | PyTorch | - | Model training |
| Transformers | Hugging Face | - | Pre-trained models |

### External Services
| Service | Provider | Purpose | Free Tier |
|---------|----------|---------|-----------|
| Cloud AI | Groq (Llama 3.3 70B) | Text analysis | 1000 req/day |
| Image AI | OpenRouter (Gemini/Llama Vision) | Screenshot analysis | Limited |
| Push Notifications | Firebase FCM | Guardian alerts | Unlimited |
| Phone OTP | Fast2SMS | Phone verification | 10 SMS/day |
| Email OTP | SMTP (Gmail) | Email verification | Unlimited |

### ML Model
| Property | Value |
|----------|-------|
| Base Model | google/mobilebert-uncased |
| Model Size | 49.2 MB (TFLite) |
| Task | 3-class sequence classification (HAM/OTP/SCAM) |
| Accuracy | 99.06% on 34,068 test samples |
| Inference Time | 100-200ms on device |
| Max Sequence Length | 128 tokens |
| Training Dataset | 93,152 samples (7 Indian languages) |

---

## 🚀 Getting Started

### Prerequisites
- **Flutter SDK**: 3.6 or higher
- **Python**: 3.11 or higher
- **PostgreSQL**: 13 or higher
- **Redis**: 6 or higher
- **Android Studio**: For Android development
- **Docker** (optional): For containerized deployment

### Backend Setup

1. **Clone the repository**
```bash
git clone https://github.com/your-org/detooz.git
cd detooz/backend
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Configure environment variables**
```bash
cp .env.example .env
# Edit .env with your API keys and database credentials
```

5. **Initialize database**
```bash
python -m app.db.migrations
```

6. **Run the server**
```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

The API will be available at `http://localhost:8000`
API documentation: `http://localhost:8000/docs`

### Mobile App Setup

1. **Navigate to app directory**
```bash
cd ../app
```

2. **Install Flutter dependencies**
```bash
flutter pub get
```

3. **Configure Firebase**
- Download `google-services.json` from Firebase Console
- Place in `android/app/` directory
- Update `android/app/build.gradle` with your package name

4. **Update API endpoint**
```dart
// lib/services/api_service.dart
static const String baseUrl = 'http://your-backend-url:8000/api';
```

5. **Run the app**
```bash
flutter run
```

### Docker Deployment (Optional)

```bash
cd backend
docker-compose up -d
```

This will start:
- FastAPI backend on port 8000
- PostgreSQL on port 5432
- Redis on port 6379

---

## 📁 Project Structure

```
detooz/
├── app/                           # Flutter Mobile App
│   ├── lib/
│   │   ├── main.dart             # App entry point
│   │   ├── contracts/            # Shared ViewModels
│   │   ├── services/             # Business logic services
│   │   │   ├── ai_service.dart   # ML model inference
│   │   │   ├── api_service.dart  # Backend API client
│   │   │   ├── notification_service.dart
│   │   │   └── translation/      # ML Kit translation
│   │   ├── ui/                   # UI Layer
│   │   │   ├── screens/          # App screens
│   │   │   ├── components/       # Reusable widgets
│   │   │   └── theme/            # App theming
│   │   └── utils/                # Utility functions
│   ├── android/                  # Android-specific code
│   │   └── app/src/main/kotlin/
│   │       └── SmsNotificationListener.kt
│   ├── assets/                   # App assets
│   │   ├── scam_detector.tflite # ML model
│   │   └── vocab.txt             # Tokenizer vocabulary
│   └── pubspec.yaml              # Flutter dependencies
│
├── backend/                      # Python Backend
│   ├── app/
│   │   ├── main.py              # FastAPI app entry
│   │   ├── config.py            # Configuration
│   │   ├── routers/             # API endpoints
│   │   │   ├── auth.py
│   │   │   ├── scan.py
│   │   │   ├── guardian_link.py
│   │   │   └── ...
│   │   ├── services/            # Business logic
│   │   │   ├── scam_detector.py # Hybrid detection
│   │   │   ├── sms_patterns.py  # Pattern matching
│   │   │   ├── guardian_alert_service.py
│   │   │   └── ...
│   │   ├── models/              # Database models
│   │   │   └── models.py
│   │   ├── schemas/             # Pydantic schemas
│   │   │   └── schemas.py
│   │   └── db/                  # Database utilities
│   │       ├── database.py
│   │       └── migrations.py
│   ├── ml_pipeline/             # ML training pipeline
│   │   ├── train.py             # Model training
│   │   ├── convert_to_tflite.py # TFLite conversion
│   │   ├── final_training_set.csv
│   │   └── saved_model/         # Trained model
│   ├── tests/                   # Backend tests
│   ├── requirements.txt         # Python dependencies
│   ├── Dockerfile               # Container definition
│   └── docker-compose.yml       # Multi-container setup
│
└── docs/                        # Documentation
    ├── API.md                   # API documentation
    ├── DEPLOYMENT.md            # Deployment guide
    └── CONTRIBUTING.md          # Contribution guidelines
```

---

## 📡 API Documentation

### Base URL
- **Development**: `http://localhost:8000/api`
- **Production**: `https://api.detooz.com/api`

### Authentication
All protected endpoints require Bearer token:
```
Authorization: Bearer <jwt_token>
```

### Key Endpoints

#### Authentication
```http
POST /api/auth/register          # Email registration
POST /api/auth/login             # Email login
POST /api/auth/google            # Google Sign-In
POST /api/auth/otp/send          # Send phone OTP
POST /api/auth/otp/verify        # Verify phone OTP
```

#### Scam Detection
```http
POST /api/manual/scan            # Analyze text message
POST /api/manual/scan/image      # Analyze screenshot
POST /api/sms/analyze            # Automatic SMS analysis
POST /api/sms/batch              # Bulk analysis
GET  /api/sms/history            # Scan history
```

#### Guardian System
```http
POST /api/guardian-link/request  # Request guardian link
POST /api/guardian-link/verify   # Verify OTP
GET  /api/guardian-link/list     # List guardians
DELETE /api/guardian-link/{id}   # Remove guardian
GET  /api/guardian-alerts        # Get alerts
POST /api/guardian-alerts/{id}/action  # Take action on alert
```

#### Education
```http
GET /api/education/feed          # Get RSS articles
GET /api/education/exclusive     # Get Detooz exclusive content
POST /api/education/bookmark     # Bookmark article
GET /api/education/bookmarks     # Get user bookmarks
```

### Example Request

```bash
curl -X POST "http://localhost:8000/api/manual/scan" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Congratulations! You won Rs. 50 lakhs. Click here to claim.",
    "sender": "+91-98765-43210",
    "platform": "SMS"
  }'
```

### Example Response

```json
{
  "id": 123,
  "risk_level": "HIGH",
  "reason": "Contains lottery/prize claim and requests personal information",
  "scam_type": "Lottery Scam",
  "confidence": 0.95,
  "created_at": "2026-02-11T10:30:00Z"
}
```

Full API documentation available at: `http://localhost:8000/docs` (Swagger UI)

---

## 🤖 ML Model Details

### Training Dataset

**Source**: HuggingFace + UCI Spam Collection  
**Total Samples**: 93,152 (training) + 34,068 (test)  
**Languages**: English, Hindi, Bengali, Kannada, Malayalam, Tamil, Telugu, Mixed

**Class Distribution**:
| Class | Label | Description | Samples |
|-------|-------|-------------|---------|
| HAM | 0 | Safe messages | ~40% |
| OTP | 1 | Legitimate OTPs | ~5% |
| SCAM | 2 | Malicious messages | ~55% |

### Model Performance

**Evaluation Results** (34,068 test samples):

| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| HAM | 0.99 | 0.98 | 0.98 | 10,229 |
| OTP | 0.98 | 1.00 | 0.99 | 399 |
| SCAM | 0.99 | 0.99 | 0.99 | 23,440 |
| **Overall Accuracy** | | | **99.06%** | 34,068 |

**Confusion Matrix**:
```
              Predicted
           HAM    OTP   SCAM
Actual HAM  10,046   7    176
       OTP      0  399      0
      SCAM    135    1  23,304
```

**Key Insights**:
- ✅ Perfect OTP detection (100% recall)
- ✅ Only 311 misclassifications out of 34,068 samples (0.91% error rate)
- ✅ 176 false positives (HAM→SCAM) - safer than false negatives
- ✅ 135 false negatives (SCAM→HAM) - requires investigation

### Training Configuration

```python
Base Model: google/mobilebert-uncased
Task: Sequence Classification (3 classes)
Max Sequence Length: 128 tokens
Training Epochs: 2
Batch Size: 16 (train), 64 (eval)
Learning Rate: 2e-5
Optimizer: AdamW
Loss Function: CrossEntropyLoss
```

### TFLite Conversion

```bash
# Convert trained model to TFLite
python ml_pipeline/convert_to_tflite.py

# Output: scam_detector.tflite (49.2 MB)
# Quantization: Dynamic range quantization
# Optimization: Default optimizations
```

---

## 🔒 Security & Privacy

### Data Protection
- ✅ **End-to-End Encryption**: All API communication over HTTPS
- ✅ **Secure Storage**: Tokens stored in platform Keychain/Keystore
- ✅ **Password Hashing**: bcrypt with salt
- ✅ **JWT Authentication**: HS256 algorithm, 30-day expiration
- ✅ **Input Validation**: All user inputs sanitized
- ✅ **SQL Injection Prevention**: Parameterized queries via SQLAlchemy

### Privacy Features
- ✅ **On-Device Processing**: Tier 1 & 2 detection never sends data to cloud
- ✅ **Anonymous Scanning**: Optional mode without user association
- ✅ **Data Retention Control**: User-defined retention period (default 365 days)
- ✅ **Consent Management**: GDPR-compliant opt-in/opt-out
- ✅ **Data Anonymization**: Remove PII before training (opt-in)
- ✅ **Audit Trail**: All consent changes logged with IP and timestamp
- ✅ **Data Export**: GDPR-compliant data export on request
- ✅ **Right to Deletion**: Users can delete all their data

### Permissions (Android)
```xml
<!-- SMS Detection -->
<uses-permission android:name="android.permission.RECEIVE_SMS"/>
<uses-permission android:name="android.permission.READ_SMS"/>

<!-- WhatsApp/Telegram Detection -->
<uses-permission android:name="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE"/>

<!-- Network -->
<uses-permission android:name="android.permission.INTERNET"/>

<!-- Background Service -->
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>

<!-- Notifications -->
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
```

**Note**: All permissions are requested with clear explanations during onboarding.

---
### Code Style
- **Flutter/Dart**: Follow [Effective Dart](https://dart.dev/guides/language/effective-dart)
- **Python**: Follow [PEP 8](https://pep8.org/)
- **Commits**: Use [Conventional Commits](https://www.conventionalcommits.org/)

---

## 📞 Contact & Support

- **Email**: detooz4734@gmail.com
---

## 🙏 Acknowledgments

- **HuggingFace** for SMS spam datasets
- **Google** for MobileBERT and ML Kit
- **Groq** for fast AI inference
- **Firebase** for authentication and push notifications
- **FastAPI** and **Flutter** communities

---

## 📊 Project Status

**Current Version**: 1.0.0  
**Status**: Active Development(Semi-last stage)  
**Last Updated**: February 11, 2026

### Roadmap
- [x] Core scam detection (SMS)
- [x] Guardian alert system
- [x] On-device ML model
- [x] Multi-language support
- [x] WhatsApp/Telegram detection 
- [ ] iOS version (planned)
- [ ] Call scam detection (planned)
- [ ] Browser extension (planned)

---

<div align="center">

**Made with ❤️ in India**

*Protecting families, one message at a time*

</div>
