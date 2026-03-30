# 🧪 QA Maestro Tests — My Demo App (Android)

> 🚧 **Work in Progress** — This is a personal study project built to learn mobile test automation with Maestro. The CI/CD pipelines are not fully functional yet — still experimenting, breaking things, and figuring it out. That's the whole point! 😄

---

## 📖 About This Project

This project is a hands-on learning exercise in **mobile UI test automation** using [Maestro](https://maestro.mobile.dev/) — a lightweight, YAML-based testing framework for Android and iOS apps.

The app under test is **Sauce Labs' My Demo App** (Android), a sample e-commerce app perfect for practising end-to-end automation flows.

---

## 🎯 What I'm Learning

- Writing Maestro test flows using YAML
- Structuring tests into modular **subflows** for reusability
- Using **faker** to generate random test data (names, addresses, card numbers)
- Setting up **GitHub Actions** for automated mobile testing (Cloud + Local emulator)
- Writing manual test cases alongside automated ones
- Managing environment variables and secrets in CI

---

## 🗂️ Project Structure

```
blessybabu-qa-maestrotests/
│
├── flows/
│   └── mydemoapp_test.yaml       # Main test flow (orchestrates all subflows)
│
├── subflows/                     # Modular test steps
│   ├── launch.yaml               # App launch & initial state
│   ├── select_random_product.yaml
│   ├── add_to_cart.yaml
│   ├── verify_cart_content.yaml
│   ├── login.yaml
│   ├── checkout.yaml             # Generates random shipping address
│   ├── payment.yaml              # Generates random card details
│   ├── review_order.yaml
│   └── checkout_complete.yaml
│
├── Manual_Test_Cases.csv         # Manual test cases covering all subflows
│
└── .github/workflows/
    ├── maestro-daily-test.yml    # 🚧 Maestro Cloud pipeline (WIP)
    └── mydemoapp.yml             # 🚧 Local emulator pipeline (WIP)
```
## 📱 Automated E2E Demo
This video demonstrates the full purchase flow, including:
- **Subflow Architecture:** Modular steps for Launch, Login, and Checkout.
- **Data Injection:** Using Inline JS to generate unique payment data.
- **Smart Assertions:** Validating UI states with Regex and visibility checks.

https://github.com/blessybabu-qa/MaestroTests/raw/refs/heads/main/fleetster_test.mp4
---

## 🔄 End-to-End Test Flow

The main flow covers a complete purchase journey:

```
Launch App
    ↓
Select a Random Product
    ↓
Add to Cart
    ↓
Verify Cart Contents → Proceed to Checkout
    ↓
Login (with credentials from env vars)
    ↓
Fill Shipping Address (faker-generated random data)
    ↓
Enter Payment Details (faker-generated card data)
    ↓
Review Order (assert all details are correct)
    ↓
Place Order → Checkout Complete ✅
```

---

## 📋 Manual Test Cases

The `Manual_Test_Cases.csv` file documents **25+ test cases** across all subflows:

| Subflow | # Test Cases |
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

---

## ⚙️ GitHub Actions Pipelines

> 🚧 **Both pipelines are currently a work in progress.** They're set up for learning purposes and may not run successfully yet.

### 1. Maestro Cloud (`maestro-daily-test.yml`)
Uploads the APK and test flows to **Maestro Cloud** for execution on real devices.

**Required Secrets:**
- `MAESTRO_CLOUD_API_KEY`
- `MAESTRO_PROJECT_ID`
- `USER_NAME`
- `USER_PWD`

### 2. Local Emulator (`mydemoapp.yml`)
Spins up an **Android emulator on macOS** (via GitHub Actions) and runs tests locally with Maestro CLI.

**Required Secrets:**
- `USER_NAME`
- `USER_PWD`

Both workflows are triggered manually via `workflow_dispatch` for now.

---

## 🚀 Running Locally

If you want to try it yourself:

1. **Install Maestro CLI:**
   ```bash
   curl -Ls "https://get.maestro.mobile.dev" | bash
   ```

2. **Download the APK:**
   ```bash
   curl -L "https://github.com/saucelabs/my-demo-app-android/releases/download/2.0.1/mda-2.0.1-22.apk" -o myapp.apk
   ```

3. **Install on your device/emulator:**
   ```bash
   adb install myapp.apk
   ```

4. **Run the tests:**
   ```bash
   maestro test flows/mydemoapp_test.yaml \
     -e USER_NAME=bod@example.com \
     -e USER_PWD=10203040
   ```

---

## 🌱 Status & Roadmap

- [x] Manual test cases written
- [x] Maestro subflows created for all steps
- [x] Faker-based random data generation in checkout & payment
- [x] GitHub Actions workflow files set up
- [ ] 🚧 Maestro Cloud pipeline — debugging in progress
- [ ] 🚧 Local emulator pipeline — debugging in progress
- [ ] Add negative test scenarios
- [ ] Add screenshots/video artifacts on failure
- [ ] Explore tagging and selective test runs

---

## 📚 Resources

- [Maestro Docs](https://maestro.mobile.dev/docs)
- [Maestro Cloud](https://cloud.mobile.dev/)
- [My Demo App (Android)](https://github.com/saucelabs/my-demo-app-android)

---

> 💡 *This project is purely for learning and experimentation. Expect rough edges, failed pipelines, and lots of trial and error — that's how we grow!*
