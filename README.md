# 🧪 QA Maestro Tests — My Demo App (Android)

A mobile UI test automation project built with [Maestro](https://maestro.mobile.dev/), targeting Sauce Labs' My Demo App (Android). The project covers a complete end-to-end purchase journey, structured with modular subflows for reusability and maintainability.

> ✅ All test flows are verified and running successfully on local device and emulator.
> A full demo video is included below as proof of execution.

---

## 📱 Automated E2E Demo

This video demonstrates the complete purchase flow running end-to-end locally, including:

- **Subflow architecture** — modular steps for launch, login, checkout, and order completion
- **Dynamic data injection** — inline JS with Faker generating unique addresses and payment details per run
- **Smart assertions** — UI state validation using regex patterns and visibility checks



---

## 📖 About This Project

This project demonstrates mobile UI test automation using [Maestro](https://maestro.mobile.dev/) — a lightweight, YAML-based testing framework for Android and iOS apps.

The app under test is **Sauce Labs' My Demo App** (Android), a sample e-commerce application used to practise realistic end-to-end automation flows covering product selection, cart management, login, checkout, and order confirmation.

---

## 🗂️ Project Structure

```
blessybabu-qa-maestrotests/
│
├── flows/
│   └── mydemoapp_test.yaml         # Main orchestrator — runs all subflows in sequence
│
├── subflows/                       # Modular, reusable test steps
│   ├── launch.yaml                 # App launch and initial state assertion
│   ├── select_random_product.yaml  # Random product selection from catalogue
│   ├── add_to_cart.yaml            # Add product and verify cart count
│   ├── verify_cart_content.yaml    # Open cart and proceed to checkout
│   ├── login.yaml                  # Login with env-var credentials
│   ├── checkout.yaml               # Faker-generated shipping address entry
│   ├── payment.yaml                # Faker-generated card details entry
│   ├── review_order.yaml           # Assert all order details before placing
│   └── checkout_complete.yaml      # Confirm order success screen
│
├── Manual_Test_Cases.csv           # 25+ manual test cases across all subflows
│
└── .github/workflows/
    ├── maestro-daily-test.yml      # Maestro Cloud pipeline (in progress)
    └── mydemoapp.yml               # Local Android emulator pipeline (in progress)
```

---

## 🔄 End-to-End Test Flow

```
Launch App
    ↓
Select a Random Product
    ↓
Add to Cart → Verify Cart Count
    ↓
Open Cart → Proceed to Checkout
    ↓
Login (credentials from environment variables)
    ↓
Fill Shipping Address (Faker-generated: name, address, city, zip, country)
    ↓
Enter Payment Details (Faker-generated: card number, expiry, CVV)
    ↓
Review Order (assert shipping + payment details with regex)
    ↓
Place Order → Checkout Complete ✅
```

---

## 🧠 Technical Highlights

**Modular subflow architecture** — each step of the purchase journey lives in its own YAML file, making the suite easy to maintain, reorder, and extend independently.

**Dynamic test data** — Faker is used inline via `evalScript` to generate unique names, addresses, and card details on every run. No hardcoded values in the test layer.

**Cross-subflow data sharing** — variables generated in one subflow (e.g. the shipping name from `checkout.yaml`) are referenced in later subflows (e.g. payment and review), keeping assertions consistent end-to-end.

**Regex-based assertions** — the review order step uses pattern matching to validate card number formatting (even when the app inserts spaces), demonstrating more than simple text matching.

**Environment variable management** — credentials are injected at runtime via `-e` flags locally and via GitHub Actions secrets in CI, keeping sensitive data out of source code.

---

## 📋 Manual Test Cases

The `Manual_Test_Cases.csv` file documents **25+ test cases** covering positive flows, edge cases, and validation scenarios across all subflows:

| Subflow | Test Cases |
|---|---|
| Launch | 2 |
| Select Random Product | 2 |
| Add to Cart | 2 |
| Verify Cart Content | 2 |
| Login | 3 |
| Checkout | 3 |
| Payment | 4 |
| Review Order | 4 |
| Checkout Complete | 3 |

Manual test cases are maintained alongside automation to ensure full coverage of scenarios not yet automated.

---

## ⚙️ CI/CD

GitHub Actions workflows are configured for two execution strategies:

### 1. Maestro Cloud (`maestro-daily-test.yml`)
Uploads the APK and test flows to Maestro Cloud for execution on real devices. Triggered manually via `workflow_dispatch`.

**Required secrets:** `MAESTRO_CLOUD_API_KEY`, `MAESTRO_PROJECT_ID`, `USER_NAME`, `USER_PWD`

### 2. Local Android Emulator (`mydemoapp.yml`)
Spins up an Android emulator on macOS via GitHub Actions and runs the full suite with Maestro CLI.

**Required secrets:** `USER_NAME`, `USER_PWD`

> **Pipeline status:** Both workflows are configured and committed. I'm currently working through environment-specific issues in CI — specifically emulator boot timing and Maestro Cloud API integration. This is an active area of development; resolving it is the next milestone for this project.

---

## 🚀 Running Locally

**Prerequisites:** Android device or emulator connected via ADB, Maestro CLI installed.

```bash
# 1. Install Maestro CLI
curl -Ls "https://get.maestro.mobile.dev" | bash

# 2. Download the APK
curl -L "https://github.com/saucelabs/my-demo-app-android/releases/download/2.0.1/mda-2.0.1-22.apk" -o myapp.apk

# 3. Install on your device or emulator
adb install myapp.apk

# 4. Run the full E2E flow
maestro test flows/mydemoapp_test.yaml \
  -e USER_NAME=bod@example.com \
  -e USER_PWD=10203040
```

---

## 🌱 Status & Roadmap

- [x] 25+ manual test cases documented
- [x] Full E2E Maestro flow verified locally (video proof above)
- [x] Modular subflow architecture implemented
- [x] Faker-based dynamic data generation for checkout and payment
- [x] Environment variable management for credentials
- [x] GitHub Actions workflow files configured
- [ ] Resolve CI pipeline issues (emulator boot timing, Maestro Cloud integration)
- [ ] Add negative test scenarios (invalid login, empty fields, payment errors)
- [ ] Capture screenshots and video artifacts on failure
- [ ] Add tagging for selective test execution

---

## 📚 Resources

- [Maestro Docs](https://maestro.mobile.dev/docs)
- [Maestro Cloud](https://cloud.mobile.dev/)
- [My Demo App (Android)](https://github.com/saucelabs/my-demo-app-android)
