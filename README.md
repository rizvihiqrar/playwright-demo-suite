# playwright-demo-suite

![CI](https://github.com/rizvihiqrar/playwright-demo-suite/actions/workflows/playwright.yml/badge.svg)

A Playwright E2E suite targeting the public [Sauce Demo](https://www.saucedemo.com) storefront. Built with a Page Object Model and reusable action helpers. **Clone it and run it with no credentials to configure.**

---

## What It Tests

| Suite | Tag | Tests | Coverage |
|---|---|---|---|
| Smoke | `@smoke` | 3 | Login lands on products page, products render, logout |
| Login Regression | `@regression` | 4 | Valid login, invalid credentials, locked-out user |
| Inventory Regression | `@regression` | 6 | Item count, add and remove from cart, cart badge, sort by name and price |
| Checkout Regression | `@regression` | 3 | End-to-end checkout, form validation, cart persistence |

**16 tests across 4 files.**

On the live site, four tests occasionally need a retry. The config allows 2 retries, and the report marks those tests as flaky rather than failed. The retry setting is in `playwright.config.js`.

---

## Project Structure

```
├── tests/
│   ├── smoketests.spec.js
│   ├── loginRegression.spec.js
│   ├── inventoryRegression.spec.js
│   └── checkoutRegression.spec.js
├── pageObjects/          # Locators per page (login, inventory, ...)
├── Helper/               # Login helper (navigation and credentials)
├── Actions/              # Action dispatcher and hooks (login before each test)
├── Utils/                # Web actions, action types, test data
├── config.js             # App config (reads env vars, defaults to saucedemo.com)
├── playwright.config.js  # Playwright configuration
└── .github/workflows/    # GitHub Actions CI
```

---

## Quick Start

```bash
# 1. Clone and install
git clone https://github.com/rizvihiqrar/playwright-demo-suite.git
cd playwright-demo-suite
npm install

# 2. Install the Playwright browser
npx playwright install chromium

# 3. Run smoke tests
npm run smoke

# 4. Run the full regression
npm run regression

# 5. Open Playwright UI mode for debugging
npx playwright test --ui
```

No `.env` file is required. The suite defaults to `https://www.saucedemo.com` with the public test credentials.

---

## Configuration

Copy `.env.example` to `.env` to point the suite at a different target:

```bash
cp .env.example .env
```

| Variable | Default | Description |
|---|---|---|
| `BASE_URL` | `https://www.saucedemo.com` | Target application URL |
| `TEST_USERNAME` | `standard_user` | Primary test user |
| `TEST_PASSWORD` | `secret_sauce` | Primary test password |

---

## CI/CD

GitHub Actions runs the smoke suite on every push and pull request to `main`. The badge above reflects the latest run.

The workflow:
- Runs on `ubuntu-latest`
- Installs Node 20 and Playwright Chromium
- Runs `npm run smoke` (3 tests)
- Uploads the Playwright HTML report as an artifact (7-day retention)

---

## Tech Stack

- **Playwright**: browser automation
- **Node.js**: test runner
- **GitHub Actions**: CI
