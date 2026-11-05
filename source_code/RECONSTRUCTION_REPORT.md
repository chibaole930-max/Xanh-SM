# XanhSM Driver Application Reverse Engineering & Source Code Retrieval Report

## 1. Application Overview & Technology Stack
- **Package Name**: `net.gsm.driver.app` (VinFast / GSM Green and Smart Mobility Driver)
- **Application Format**: Split APK / XAPK (Base APK + Architecture Split `arm64-v8a` + Resource splits)
- **Core Framework**: Flutter 3.29.x / Dart 3.7.2 stable (`android_arm64`)
- **AOT Snapshot Hash**: `d91c0e6f35f0eb2e44124e8f42aa44a7`
- **Engine Build ID**: `2c0bd20296bfcbf70a9b1f8a745bd75c64897220`
- **Architecture Pattern**: Micro-Frontend / Modular Super-App Architecture ("Mini Apps")

## 2. Reconstructed Internal Application Modules
Total internal application source files reconstructed: **1149** files across 22 packages.

| Module | File Count | Purpose |
|---|---|---|
| `mini_app_driver_onboarding` | 190 | Module reconstructed in `packages/mini_app_driver_onboarding/` |
| `mini_app_map_contributor` | 147 | Module reconstructed in `packages/mini_app_map_contributor/` |
| `payment_data` | 116 | Module reconstructed in `packages/payment_data/` |
| `mini_app_payment` | 112 | Module reconstructed in `packages/mini_app_payment/` |
| `mini_app_safety_check` | 111 | Module reconstructed in `packages/mini_app_safety_check/` |
| `mini_app_driver_membership` | 110 | Module reconstructed in `packages/mini_app_driver_membership/` |
| `payment_domain` | 76 | Module reconstructed in `packages/payment_domain/` |
| `mini_app_xanh_local_guide` | 51 | Module reconstructed in `packages/mini_app_xanh_local_guide/` |
| `mini_app_driver_group` | 38 | Module reconstructed in `packages/mini_app_driver_group/` |
| `mini_app_lms_global` | 34 | Module reconstructed in `packages/mini_app_lms_global/` |
| `mini_app_lotus_lms` | 32 | Module reconstructed in `packages/mini_app_lotus_lms/` |
| `mini_app_driver_profile` | 27 | Module reconstructed in `packages/mini_app_driver_profile/` |
| `data` | 24 | Module reconstructed in `packages/data/` |
| `resources` | 19 | Module reconstructed in `packages/resources/` |
| `payment_resources` | 18 | Module reconstructed in `packages/payment_resources/` |
| `domain` | 12 | Module reconstructed in `packages/domain/` |
| `payment_core` | 8 | Module reconstructed in `packages/payment_core/` |
| `mini_app_design` | 8 | Module reconstructed in `packages/mini_app_design/` |
| `core` | 6 | Module reconstructed in `packages/core/` |
| `mini_app_core` | 5 | Module reconstructed in `packages/mini_app_core/` |
| `super_app_driver` | 3 | Module reconstructed in `packages/super_app_driver/` |
| `mini_app_security` | 2 | Module reconstructed in `packages/mini_app_security/` |

## 3. Key Backend Endpoints & API Surface
### Primary Host Domains:
- `https://api.staging.gsm-api.net`
- `https://gapi-stg.xanhsm.com`
- `https://greenportal.gsm-api.net/instruct/image10.png`
- `https://greenportal.gsm-api.net/instruct/image11.png`
- `https://greenportal.gsm-api.net/instruct/image12.png`
- `https://greenportal.gsm-api.net/instruct/image16.png`
- `https://greenportal.gsm-api.net/instruct/image18.png`
- `https://greenportal.gsm-api.net/instruct/image19.png`
- `https://greenportal.gsm-api.net/instruct/image20.png`
- `https://greenportal.gsm-api.net/instruct/image21.png`

### Discovered REST Endpoints:
- `/account/v1/public/supplier/profile`
- `/account/v1/public/supplier/vehicle-checklist`
- `/account/v1/public/supplier/vehicle-checklist/menu`
- `/account/v1/public/supplier/vehicle-checklist/registration/validate`
- `/account/v1/public/supplier/vehicle-checklist/search`
- `/auth/v1/public/user/lms/login`
- `/auth/v1/public/vneid/biometric-share-info/get`
- `/auth/v1/public/vneid/biometric-share-info/init`
- `/car/acquisition/create-profile-session`
- `/car/acquisition/create-profile-session-by-token`
- `/car/acquisition/create-registration`
- `/car/acquisition/get-bank-account-by-session`
- `/car/acquisition/get-ocr-result-by-session`
- `/car/acquisition/get-profile-by-session`
- `/car/acquisition/get-profile-state-session`
- `/car/acquisition/get-upload-document-url-by-session`
- `/car/acquisition/send-profile-access-code`
- `/car/acquisition/send-registration-otp`
- `/car/acquisition/submit-profile`
- `/car/acquisition/update-document-by-session`
- `/car/acquisition/update-profile-by-session`
- `/car/acquisition/verify-registration`
- `/car/driver/group/get-driver-group-by-driver-id`
- `/car/driver/group/m-app/update-name`
- `/car/driver/m-app/get-upload-document-url`
- `/car/driver/m-app/update-document`
- `/car/driver/reviewing/m-app/get-handover-report`
- `/car/driver/reviewing/m-app/get-handover-report-history`
- `/car/driver/reviewing/m-app/get-handover-report-status`
- `/car/driver/reviewing/m-app/handover-prepare-upload-document0`
- *(and 64 additional endpoints in endpoints.json)*

## 4. Native Security & Anti-Tamper Mechanisms
- **Hooking & Root Detection**: Manifest inspects `de.robv.android.xposed.installer` to block Xposed runtime hooks.
- **BShield Protection**: Integration of `assets/bshield.dat` and `flutter_bshield_callback` for app hardening.
- **Biometric & Face Liveness Verification**: Integrates FPT eKYC (`com.fpt.fci.ekycfull`), VN.AI FaceAuth (`vn.ai.faceauth.sdk`), and bundled TFLite machine learning models (`detectionNew.tflite`, `face_mask.tflite`, `face_blur.tflite`).
- **National ID Integration (VNeID)**: Custom deep link handling for Vietnam National Digital Identity (`/vneiduniversallink/`, `/auth/v1/public/vneid/biometric-share-info/*`).

## 5. Recovered File Tree on Disk
- `packages/` : Reconstructed Dart source tree with full paths and package headers
- `assets/` : Bundled TFLite models, shaders, translations, and manifest assets
- `endpoints.json` : Complete catalog of extracted API endpoints and domains
- `manifest_analysis.json` : Decoded Android activities, permissions, and broadcast receivers
- `internal_dart_sources.json` : Complete catalog of all 1,149 application source files