# Detooz - AI-Powered Scam Detection Platform
## Requirements Document

**Version:** 1.0  
**Last Updated:** February 11, 2026  
**Project Type:** Mobile Application (Flutter) + Backend API (FastAPI)

---

## 1. Executive Summary

### 1.1 Project Overview
Detooz is an AI-powered mobile application designed to protect vulnerable users—particularly senior citizens and students in India—from SMS, WhatsApp, and Telegram scams. The platform uses a hybrid three-tier detection system combining pattern matching, on-device machine learning, and cloud AI to provide real-time scam detection with guardian alert capabilities.

### 1.2 Problem Statement
- India has 1.2+ billion mobile users receiving billions of SMS messages daily
- Scammers exploit urgency tactics, multilingual messages, and impersonation
- Victims often lack technical knowledge to identify sophisticated scams
- Existing solutions don't adequately support Indian regional languages
- Family members are unaware when loved ones receive scam messages

### 1.3 Solution
A comprehensive mobile application that:
- Detects scam messages in real-time across multiple platforms (SMS, WhatsApp, Telegram)
- Alerts designated family guardians via push notifications when scams are detected
- Educates users about common scam patterns through curated content
- Provides offline detection capabilities for privacy-conscious users
- Supports 9 Indian languages for UI and analyzes messages in 22+ languages

---

## 2. Target Users

### 2.1 Primary Users
| User Type | Demographics | Primary Needs |
|-----------|--------------|---------------|
| **Senior Citizens** | Age 60+, limited tech literacy | Protection from KYC, lottery, and bank scams; simple UI |
| **Students** | Age 18-25, moderate tech literacy | Protection from job and quick money scams; educational content |
| **General Users** | Age 25-60, varied tech literacy | Comprehensive scam protection; privacy controls |

### 2.2 Secondary Users
| User Type | Role | Primary Needs |
|-----------|------|---------------|
| **Family Guardians** | Adult family members | Real-time alerts when loved ones receive scams; remote monitoring |
| **Administrators** | Platform managers | Content curation; system monitoring; user management |

---

## 3. Functional Requirements

### 3.1 Authentication & User Management

#### 3.1.1 User Registration
- **FR-AUTH-001**: System shall support email-based registration with password
- **FR-AUTH-002**: System shall support phone number registration with OTP verification
- **FR-AUTH-003**: System shall support Google Sign-In OAuth authentication
- **FR-AUTH-004**: System shall support Firebase Phone Authentication
- **FR-AUTH-005**: System shall provide 30-day grace period for email/phone verification
- **FR-AUTH-006**: System shall store user profile information (first name, middle name, last name, phone, country code)

#### 3.1.2 Session Management
- **FR-AUTH-007**: System shall issue JWT tokens with 30-day expiration for mobile apps
- **FR-AUTH-008**: System shall store tokens securely using platform-specific secure storage (Keychain/Keystore)
- **FR-AUTH-009**: System shall support token refresh mechanism
- **FR-AUTH-010**: System shall support FCM token registration for push notifications

### 3.2 Scam Detection System

#### 3.2.1 Three-Tier Detection Architecture
- **FR-DETECT-001**: System shall implement Tier 1 pattern matching with <10ms response time
- **FR-DETECT-002**: System shall implement Tier 2 on-device ML using MobileBERT TFLite model (49MB, ~100-200ms)
- **FR-DETECT-003**: System shall implement Tier 3 cloud AI using Groq Llama 3.3 70B (~500-1500ms)
- **FR-DETECT-004**: System shall cascade through tiers based on confidence thresholds
- **FR-DETECT-005**: System shall cache AI analysis results to reduce API calls

#### 3.2.2 Pattern Matching (Tier 1)
- **FR-DETECT-006**: System shall detect 8 high-risk scam categories:
  - KYC update/expiry scams
  - Prize/lottery scams
  - OTP theft attempts
  - Job offer scams
  - Loan approval scams
  - Investment scams
  - Government impersonation
  - Delivery/customs scams
- **FR-DETECT-007**: System shall detect 4 medium-risk indicators:
  - Suspicious shortened URLs
  - Urgency tactics
  - Money-related keywords
  - Verification requests
- **FR-DETECT-008**: System shall use regex patterns for instant offline detection

#### 3.2.3 On-Device ML Model (Tier 2)
- **FR-DETECT-009**: System shall load MobileBERT TFLite model (49.2MB) on app initialization
- **FR-DETECT-010**: System shall perform 3-class classification (HAM, OTP, SCAM)
- **FR-DETECT-011**: System shall achieve 99%+ accuracy on test dataset
- **FR-DETECT-012**: System shall support offline inference without network connectivity
- **FR-DETECT-013**: System shall handle messages up to 128 tokens

#### 3.2.4 Cloud AI Analysis (Tier 3)
- **FR-DETECT-014**: System shall use Groq API with Llama 3.3 70B model
- **FR-DETECT-015**: System shall support analysis of messages in 22 Indian scheduled languages
- **FR-DETECT-016**: System shall return structured JSON with risk_level, reason, scam_type, confidence
- **FR-DETECT-017**: System shall fallback to OpenRouter if Groq is unavailable
- **FR-DETECT-018**: System shall handle mixed-language messages (Hinglish, Tanglish, etc.)

#### 3.2.5 Risk Classification
- **FR-DETECT-019**: System shall classify messages into three risk levels:
  - HIGH: Definite scam (phishing, fraud, money requests, fake prizes)
  - MEDIUM: Suspicious (urgency tactics, unknown links, unusual requests)
  - LOW: Likely legitimate
- **FR-DETECT-020**: System shall provide confidence score (0.0-1.0) for each classification
- **FR-DETECT-021**: System shall identify specific scam types (e.g., "Lottery Scam", "KYC Scam")

### 3.3 Platform-Specific Detection

#### 3.3.1 SMS Detection
- **FR-SMS-001**: System shall intercept incoming SMS messages using BroadcastReceiver (Android)
- **FR-SMS-002**: System shall analyze SMS content in real-time
- **FR-SMS-003**: System shall extract sender information from SMS
- **FR-SMS-004**: System shall skip messages from saved contacts
- **FR-SMS-005**: System shall prevent duplicate processing using message cache (200 entries)

#### 3.3.2 WhatsApp Detection
- **FR-WA-001**: System shall monitor WhatsApp notifications using NotificationListenerService
- **FR-WA-002**: System shall support both WhatsApp and WhatsApp Business
- **FR-WA-003**: System shall extract sender and message preview from notifications
- **FR-WA-004**: System shall analyze notification content for scam indicators

#### 3.3.3 Telegram Detection
- **FR-TG-001**: System shall monitor Telegram notifications using NotificationListenerService
- **FR-TG-002**: System shall extract sender and message preview from notifications
- **FR-TG-003**: System shall analyze notification content for scam indicators

### 3.4 Guardian Alert System

#### 3.4.1 Guardian Linking
- **FR-GUARD-001**: System shall allow users to link guardians via email/phone
- **FR-GUARD-002**: System shall generate 6-digit OTP for guardian verification
- **FR-GUARD-003**: System shall expire OTP after 10 minutes
- **FR-GUARD-004**: System shall support multiple guardians per user
- **FR-GUARD-005**: System shall allow guardians to monitor multiple protected users
- **FR-GUARD-006**: System shall support guardian link status (pending, active, revoked)

#### 3.4.2 Alert Generation
- **FR-GUARD-007**: System shall create alerts based on user-defined threshold (HIGH, MEDIUM, ALL)
- **FR-GUARD-008**: System shall send FCM push notifications to guardian devices
- **FR-GUARD-009**: System shall include in alert:
  - Protected user name
  - Scam type
  - Sender information
  - Message preview
  - Risk level
  - Timestamp
- **FR-GUARD-010**: System shall track alert status (pending, seen, actioned, dismissed)
- **FR-GUARD-011**: System shall allow guardians to take action on alerts

#### 3.4.3 Alert Management
- **FR-GUARD-012**: System shall display alert history for guardians
- **FR-GUARD-013**: System shall allow guardians to mark alerts as seen/actioned
- **FR-GUARD-014**: System shall allow guardians to add action notes
- **FR-GUARD-015**: System shall prevent duplicate alerts for same scan

### 3.5 Scan History & Management

#### 3.5.1 Scan Storage
- **FR-HIST-001**: System shall store all scan results in database
- **FR-HIST-002**: System shall store scan metadata:
  - Sender
  - Message preview (200 chars max)
  - Full message (optional, based on privacy settings)
  - Platform (SMS/WhatsApp/Telegram)
  - Risk level
  - Risk reason
  - Scam type
  - Confidence score
  - Timestamp
- **FR-HIST-003**: System shall support user-controlled data retention (default 365 days)
- **FR-HIST-004**: System shall support anonymous scanning (no user association)

#### 3.5.2 History Viewing
- **FR-HIST-005**: System shall display scan history in chronological order
- **FR-HIST-006**: System shall support filtering by:
  - Risk level (HIGH/MEDIUM/LOW)
  - Platform (SMS/WhatsApp/Telegram)
  - Date range
  - Scam type
- **FR-HIST-007**: System shall support search by sender or message content
- **FR-HIST-008**: System shall display scan details on selection
- **FR-HIST-009**: System shall allow users to delete individual scans

#### 3.5.3 Batch Analysis
- **FR-HIST-010**: System shall support batch SMS analysis
- **FR-HIST-011**: System shall use quick pattern matching for batch processing
- **FR-HIST-012**: System shall return aggregated statistics for batch scans

### 3.6 Trusted Senders

#### 3.6.1 Trusted Sender Management
- **FR-TRUST-001**: System shall allow users to mark senders as trusted
- **FR-TRUST-002**: System shall bypass scam detection for trusted senders
- **FR-TRUST-003**: System shall support friendly names for trusted senders
- **FR-TRUST-004**: System shall allow users to add reason for trusting sender
- **FR-TRUST-005**: System shall allow users to remove trusted senders
- **FR-TRUST-006**: System shall display list of all trusted senders

### 3.7 Feedback & Reputation System

#### 3.7.1 User Feedback
- **FR-FEED-001**: System shall allow users to provide feedback on scan results
- **FR-FEED-002**: System shall support feedback verdicts: safe, scam, unsure
- **FR-FEED-003**: System shall allow users to add comments to feedback
- **FR-FEED-004**: System shall store original AI verdict for comparison
- **FR-FEED-005**: System shall use feedback for model improvement

#### 3.7.2 Blacklist/Reputation Database
- **FR-REP-001**: System shall maintain blacklist of known scam:
  - URLs
  - Phone numbers
  - Domains
- **FR-REP-002**: System shall store blacklist entries with:
  - Type (url/phone/domain)
  - Value and SHA256 hash
  - Source (community/system/verified)
  - Report count
  - First and last reported timestamps
  - Verification status
- **FR-REP-003**: System shall allow users to check reputation of URLs/phones
- **FR-REP-004**: System shall allow users to report new scam sources
- **FR-REP-005**: System shall store full scam messages for training data (with consent)

### 3.8 Image Analysis

#### 3.8.1 Screenshot Analysis
- **FR-IMG-001**: System shall support image upload for scam detection
- **FR-IMG-002**: System shall use OpenRouter vision models (Gemini/Llama Vision)
- **FR-IMG-003**: System shall analyze screenshots for:
  - Fake payment screens
  - Suspicious WhatsApp chats
  - Fake lottery/prize messages
  - Phishing websites
- **FR-IMG-004**: System shall return risk assessment for images
- **FR-IMG-005**: System shall support multiple vision model fallback
- **FR-IMG-006**: System shall limit image size to 15MB

### 3.9 Education Hub

#### 3.9.1 Content Sources
- **FR-EDU-001**: System shall aggregate content from RSS feeds:
  - Quick Heal Security Blog
  - GBHackers on Security
  - Krebs on Security
  - Other cybersecurity sources
- **FR-EDU-002**: System shall support admin-curated articles
- **FR-EDU-003**: System shall auto-sync RSS feeds every 30 minutes
- **FR-EDU-004**: System shall categorize articles (alert/tip/news)

#### 3.9.2 Content Display
- **FR-EDU-005**: System shall display articles with:
  - Title
  - Summary
  - Image
  - Source
  - Category
  - Read time estimate
  - Publication date
- **FR-EDU-006**: System shall support article bookmarking
- **FR-EDU-007**: System shall support featured articles
- **FR-EDU-008**: System shall open external articles in WebView

#### 3.9.3 Detooz Exclusive Content
- **FR-EDU-009**: System shall support AI-curated educational content
- **FR-EDU-010**: System shall watermark exclusive content images
- **FR-EDU-011**: System shall track source articles for exclusive content

### 3.10 User Settings & Preferences

#### 3.10.1 Detection Settings
- **FR-SET-001**: System shall allow users to toggle detection for:
  - SMS
  - WhatsApp
  - Telegram
- **FR-SET-002**: System shall allow users to set guardian alert threshold (HIGH/MEDIUM/ALL)
- **FR-SET-003**: System shall allow users to enable/disable auto-block for high-risk messages
- **FR-SET-004**: System shall allow users to enable/disable educational tips

#### 3.10.2 Privacy Settings
- **FR-SET-005**: System shall allow users to control data retention period
- **FR-SET-006**: System shall allow users to enable/disable data anonymization
- **FR-SET-007**: System shall allow users to opt-in/out of training data sharing
- **FR-SET-008**: System shall allow users to opt-in/out of analytics
- **FR-SET-009**: System shall allow users to opt-in/out of research use
- **FR-SET-010**: System shall track consent version and timestamp
- **FR-SET-011**: System shall log consent changes for audit trail
- **FR-SET-012**: System shall support GDPR data export requests

#### 3.10.3 UI Settings
- **FR-SET-013**: System shall support 9 UI languages:
  - English, Hindi, Bengali, Telugu, Marathi, Tamil, Gujarati, Kannada, Urdu
- **FR-SET-014**: System shall support dark mode
- **FR-SET-015**: System shall support large text mode for accessibility
- **FR-SET-016**: System shall use Google ML Kit for dynamic translation

### 3.11 Offline Capabilities

#### 3.11.1 Offline Detection
- **FR-OFF-001**: System shall perform pattern matching without network
- **FR-OFF-002**: System shall perform on-device ML inference without network
- **FR-OFF-003**: System shall cache recent scan results for offline viewing
- **FR-OFF-004**: System shall queue scans for cloud analysis when offline
- **FR-OFF-005**: System shall sync queued scans when network is restored

#### 3.11.2 Offline Storage
- **FR-OFF-006**: System shall use Hive for local data storage
- **FR-OFF-007**: System shall cache user settings locally
- **FR-OFF-008**: System shall cache trusted senders locally
- **FR-OFF-009**: System shall cache scan history locally

### 3.12 Notifications

#### 3.12.1 Local Notifications
- **FR-NOTIF-001**: System shall display local notification for high-risk scams
- **FR-NOTIF-002**: System shall display notification with:
  - Risk level indicator
  - Sender
  - Message preview
  - Action buttons (View/Dismiss)
- **FR-NOTIF-003**: System shall support notification channels by risk level

#### 3.12.2 Push Notifications
- **FR-NOTIF-004**: System shall use Firebase Cloud Messaging for push notifications
- **FR-NOTIF-005**: System shall send push notifications to guardians for alerts
- **FR-NOTIF-006**: System shall support notification deep linking to specific screens

#### 3.12.3 Foreground Service
- **FR-NOTIF-007**: System shall run foreground service for persistent notification monitoring
- **FR-NOTIF-008**: System shall display persistent notification indicating protection status
- **FR-NOTIF-009**: System shall survive app termination and device reboot

---

## 4. Non-Functional Requirements

### 4.1 Performance

#### 4.1.1 Response Times
- **NFR-PERF-001**: Pattern matching shall complete in <10ms
- **NFR-PERF-002**: On-device ML inference shall complete in 100-200ms
- **NFR-PERF-003**: Cloud AI analysis shall complete in 500-1500ms
- **NFR-PERF-004**: API endpoints shall respond within 2 seconds (95th percentile)
- **NFR-PERF-005**: Image analysis shall complete within 5 seconds

#### 4.1.2 Throughput
- **NFR-PERF-006**: System shall handle 1000+ concurrent users
- **NFR-PERF-007**: System shall process 100+ scans per second
- **NFR-PERF-008**: System shall support batch analysis of 100+ messages

#### 4.1.3 Resource Usage
- **NFR-PERF-009**: Mobile app shall use <200MB RAM during normal operation
- **NFR-PERF-010**: ML model shall use <100MB storage
- **NFR-PERF-011**: App shall use <50MB network data per day (typical usage)
- **NFR-PERF-012**: Battery drain shall be <5% per day with background monitoring

### 4.2 Scalability

#### 4.2.1 User Scalability
- **NFR-SCALE-001**: System shall support 100,000+ registered users
- **NFR-SCALE-002**: System shall support 1,000,000+ scans per day
- **NFR-SCALE-003**: Database shall support 10,000,000+ scan records

#### 4.2.2 Infrastructure Scalability
- **NFR-SCALE-004**: Backend shall support horizontal scaling
- **NFR-SCALE-005**: Database shall support read replicas
- **NFR-SCALE-006**: Redis cache shall support clustering

### 4.3 Reliability

#### 4.3.1 Availability
- **NFR-REL-001**: System shall maintain 99.5% uptime
- **NFR-REL-002**: Planned maintenance shall not exceed 4 hours per month
- **NFR-REL-003**: System shall gracefully degrade when cloud AI is unavailable

#### 4.3.2 Data Integrity
- **NFR-REL-004**: System shall prevent data loss through database backups
- **NFR-REL-005**: System shall maintain data consistency across replicas
- **NFR-REL-006**: System shall validate all user inputs

#### 4.3.3 Error Handling
- **NFR-REL-007**: System shall log all errors with context
- **NFR-REL-008**: System shall display user-friendly error messages
- **NFR-REL-009**: System shall retry failed API calls with exponential backoff
- **NFR-REL-010**: System shall fallback to local detection when cloud fails

### 4.4 Security

#### 4.4.1 Authentication & Authorization
- **NFR-SEC-001**: System shall use bcrypt for password hashing
- **NFR-SEC-002**: System shall use JWT with HS256 algorithm
- **NFR-SEC-003**: System shall enforce token expiration
- **NFR-SEC-004**: System shall use HTTPS for all API communication
- **NFR-SEC-005**: System shall validate JWT on all protected endpoints

#### 4.4.2 Data Protection
- **NFR-SEC-006**: System shall encrypt sensitive data at rest
- **NFR-SEC-007**: System shall use platform secure storage for tokens
- **NFR-SEC-008**: System shall sanitize all user inputs
- **NFR-SEC-009**: System shall prevent SQL injection attacks
- **NFR-SEC-010**: System shall prevent XSS attacks

#### 4.4.3 Privacy
- **NFR-SEC-011**: System shall not store full messages without user consent
- **NFR-SEC-012**: System shall anonymize data for training when requested
- **NFR-SEC-013**: System shall support data deletion requests
- **NFR-SEC-014**: System shall comply with GDPR requirements
- **NFR-SEC-015**: System shall log consent changes with IP and timestamp

### 4.5 Usability

#### 4.5.1 User Interface
- **NFR-USE-001**: UI shall follow Material Design guidelines
- **NFR-USE-002**: UI shall be accessible to users with disabilities
- **NFR-USE-003**: UI shall support large text for senior citizens
- **NFR-USE-004**: UI shall use clear risk indicators (color-coded)
- **NFR-USE-005**: UI shall provide contextual help

#### 4.5.2 Onboarding
- **NFR-USE-006**: First-time setup shall complete in <5 minutes
- **NFR-USE-007**: Permission requests shall include clear explanations
- **NFR-USE-008**: Guardian setup shall be optional during onboarding

#### 4.5.3 Localization
- **NFR-USE-009**: UI shall support 9 Indian languages
- **NFR-USE-010**: Error messages shall be localized
- **NFR-USE-011**: Date/time formats shall follow user locale

### 4.6 Maintainability

#### 4.6.1 Code Quality
- **NFR-MAINT-001**: Code shall follow language-specific style guides
- **NFR-MAINT-002**: Code shall maintain >80% test coverage
- **NFR-MAINT-003**: Code shall be documented with inline comments
- **NFR-MAINT-004**: API shall be documented with OpenAPI/Swagger

#### 4.6.2 Monitoring
- **NFR-MAINT-005**: System shall log all API requests
- **NFR-MAINT-006**: System shall track error rates
- **NFR-MAINT-007**: System shall monitor API latency
- **NFR-MAINT-008**: System shall alert on critical errors

#### 4.6.3 Deployment
- **NFR-MAINT-009**: System shall support containerized deployment
- **NFR-MAINT-010**: System shall support CI/CD pipelines
- **NFR-MAINT-011**: System shall support blue-green deployments
- **NFR-MAINT-012**: System shall support database migrations

### 4.7 Compatibility

#### 4.7.1 Mobile Platform
- **NFR-COMPAT-001**: Android app shall support Android 8.0+ (API 26+)
- **NFR-COMPAT-002**: App shall support screen sizes from 4" to 7"
- **NFR-COMPAT-003**: App shall support both portrait and landscape orientations

#### 4.7.2 Backend Platform
- **NFR-COMPAT-004**: Backend shall run on Python 3.11+
- **NFR-COMPAT-005**: Backend shall support PostgreSQL 13+
- **NFR-COMPAT-006**: Backend shall support Redis 6+

---

## 5. System Constraints

### 5.1 Technical Constraints
- **CONST-001**: Mobile app must be built with Flutter 3.6+
- **CONST-002**: Backend must be built with FastAPI
- **CONST-003**: ML model must be TFLite compatible
- **CONST-004**: ML model size must be <100MB
- **CONST-005**: iOS support is out of scope for initial release

### 5.2 External Dependencies
- **CONST-006**: System depends on Groq API for cloud AI (1000 requests/day free tier)
- **CONST-007**: System depends on OpenRouter API for image analysis
- **CONST-008**: System depends on Firebase for authentication and push notifications
- **CONST-009**: System depends on Google ML Kit for UI translation
- **CONST-010**: System depends on Fast2SMS for phone OTP (10 SMS/day free tier)

### 5.3 Regulatory Constraints
- **CONST-011**: System must comply with Google Play Store policies
- **CONST-012**: System must comply with GDPR for EU users
- **CONST-013**: System must comply with Indian IT Act 2000
- **CONST-014**: System must obtain user consent for SMS/notification access

### 5.4 Business Constraints
- **CONST-015**: Initial release must use free-tier services only
- **CONST-016**: System must be deployable on AWS/GCP free tier
- **CONST-017**: Development must complete within 3 months

---

## 6. Acceptance Criteria

### 6.1 Core Functionality
- **AC-001**: User can register and login successfully
- **AC-002**: System detects high-risk SMS scams with >95% accuracy
- **AC-003**: Guardian receives push notification within 5 seconds of high-risk detection
- **AC-004**: User can view scan history with filtering
- **AC-005**: User can link and verify guardians via OTP
- **AC-006**: System works offline for pattern matching and on-device ML

### 6.2 Performance
- **AC-007**: Pattern matching completes in <10ms
- **AC-008**: On-device ML inference completes in <200ms
- **AC-009**: API responds within 2 seconds for 95% of requests
- **AC-010**: App uses <200MB RAM during normal operation

### 6.3 Usability
- **AC-011**: First-time setup completes in <5 minutes
- **AC-012**: UI is accessible in 9 Indian languages
- **AC-013**: Senior citizens can navigate app without assistance
- **AC-014**: Risk levels are clearly indicated with color coding

### 6.4 Security
- **AC-015**: All API communication uses HTTPS
- **AC-016**: Passwords are hashed with bcrypt
- **AC-017**: JWT tokens expire after 30 days
- **AC-018**: User data is encrypted at rest

### 6.5 Compliance
- **AC-019**: App passes Google Play Store review
- **AC-020**: System complies with GDPR requirements
- **AC-021**: User consent is tracked with audit trail
- **AC-022**: Data retention follows user preferences

---

## 7. Future Enhancements (Out of Scope for v1.0)

### 7.1 Phase 2 Features
- Call scam detection with transcription
- Multi-language voice alerts
- Community scam reporting dashboard
- Advanced analytics and insights
- iOS version

### 7.2 Phase 3 Features
- Browser extension for web scam detection
- Integration with banking apps
- AI-powered scam prediction
- Family plan subscriptions
- Enterprise/institution licenses

---

## 8. Glossary

| Term | Definition |
|------|------------|
| **Guardian** | A trusted family member who receives alerts when protected user gets scam messages |
| **Scan** | Analysis result of a message for scam indicators |
| **Risk Level** | Classification of message as HIGH, MEDIUM, or LOW risk |
| **Pattern Matching** | Regex-based detection of known scam patterns |
| **On-Device ML** | Machine learning inference performed locally on device |
| **Cloud AI** | AI analysis performed on remote servers (Groq/OpenRouter) |
| **TFLite** | TensorFlow Lite - optimized ML framework for mobile devices |
| **FCM** | Firebase Cloud Messaging - push notification service |
| **OTP** | One-Time Password - temporary code for verification |
| **JWT** | JSON Web Token - authentication token format |
| **GDPR** | General Data Protection Regulation - EU privacy law |

---

## 9. References

### 9.1 Technical Documentation
- Flutter Documentation: https://docs.flutter.dev/
- FastAPI Documentation: https://fastapi.tiangolo.com/
- TensorFlow Lite: https://www.tensorflow.org/lite
- Firebase Documentation: https://firebase.google.com/docs

### 9.2 External APIs
- Groq API: https://groq.com/
- OpenRouter API: https://openrouter.ai/
- Google ML Kit: https://developers.google.com/ml-kit
- Fast2SMS: https://www.fast2sms.com/

### 9.3 Datasets
- HuggingFace SMS Dataset: gandharvbakshi/SMS-dataset-OTP-OTP_INTENT_Phishing
- UCI Spam Collection: https://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection

---

**Document Status:** Final  
**Approval Required:** Product Owner, Technical Lead  
**Next Review Date:** March 11, 2026
