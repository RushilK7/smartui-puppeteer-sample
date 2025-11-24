# SmartUI SDK Sample for Puppeteer

Welcome to the SmartUI SDK sample for Puppeteer. This repository demonstrates how to integrate SmartUI visual regression testing with Puppeteer.

## Repository Structure

```
smartui-puppeteer-sample/
├── sdk/
│   ├── puppeteerCloud.js    # Cloud test
│   ├── puppeteerLocal.js    # Local test
│   └── smartui-web.json    # SmartUI config (create with npx smartui config:create)
└── hooks/                    # Hooks integration examples
    └── samplePuppeteer.js   # Hooks example
```

## 1. Prerequisites and Environment Setup

### Prerequisites

- Node.js installed
- LambdaTest account credentials (for Cloud tests)
- Chrome browser (for Local tests)

### Environment Setup

**For Cloud:**
```bash
export LT_USERNAME='your_username'
export LT_ACCESS_KEY='your_access_key'
export PROJECT_TOKEN='your_project_token'
```

**For Local:**
```bash
export PROJECT_TOKEN='your_project_token'
```

## 2. Initial Setup and Dependencies

### Clone the Repository

```bash
git clone https://github.com/LambdaTest/smartui-puppeteer-sample
cd smartui-puppeteer-sample/sdk
```

### Install Dependencies

The repository already includes the required dependencies in `package.json`. Install them:

```bash
npm install
```

**Dependencies included:**
- `@lambdatest/smartui-cli` - SmartUI CLI
- `@lambdatest/puppeteer-driver` - SmartUI Puppeteer driver
- `puppeteer` - Puppeteer framework

### Create SmartUI Configuration

```bash
npx smartui config:create smartui-web.json
```

## 3. Steps to Integrate Screenshot Commands into Codebase

The SmartUI screenshot function is already implemented in the repository.

**Cloud Test** (`sdk/puppeteerCloud.js`):
```javascript
const { smartuiSnapshot } = require('@lambdatest/puppeteer-driver');

await page.goto("https://www.lambdatest.com");
await smartuiSnapshot(page, "screenshot");
```

**Local Test** (`sdk/puppeteerLocal.js`):
```javascript
const { smartuiSnapshot } = require('@lambdatest/puppeteer-driver');

await page.goto("https://www.lambdatest.com");
await smartuiSnapshot(page, "screenshot");
```

**Note**: The code is already configured and ready to use. You can modify the URL and screenshot name if needed.

## 4. Execution and Commands

### Local Execution

```bash
npx smartui exec node puppeteerLocal.js
```

### Cloud Execution

```bash
npx smartui exec node puppeteerCloud.js
```

**Note**: You can also use the npm scripts defined in `package.json`:
- Local: `npm run smartui-local`
- Cloud: `npm run smartui-cloud`

## Testing with LambdaTest Hooks

This repository also includes examples for using SmartUI with LambdaTest Hooks integration.

### Hooks Integration

**Location:** See the `hooks` folder for hooks integration examples.

**Purpose:** Use LambdaTest's webhook for seamless visual regression testing.

### Hooks Setup Steps

1. Navigate to the hooks directory:
```bash
cd hooks
npm install
```

2. Use SmartUI hooks in your test:
```javascript
await page.evaluate(() => {
  const screenshotName = "Your_Screenshot_Name";
  const lambdatestAction = JSON.stringify({
    action: 'smartui.takeScreenshot',
    arguments: { fullPage: true, screenshotName }
  });
  // Execute the SmartUI action
});
```

3. Run the tests:
```bash
npm run single
```

## Test Files

### Cloud Test (`sdk/puppeteerCloud.js`)

- Connects to LambdaTest Cloud using Puppeteer
- Reads credentials from environment variables (`LT_USERNAME`, `LT_ACCESS_KEY`)
- Takes screenshot with name: `screenshot`

### Local Test (`sdk/puppeteerLocal.js`)

- Runs Puppeteer locally using Chrome
- Requires Chrome browser installed
- Takes screenshot with name: `screenshot`

## Configuration

### SmartUI Config (`smartui-web.json`)

Create the SmartUI configuration file using:
```bash
npx smartui config:create smartui-web.json
```

## View Results

After running the tests, visit your SmartUI project dashboard to view the captured screenshots and compare them with baseline builds.

## More Information

For detailed onboarding instructions, see the [SmartUI Puppeteer Onboarding Guide](https://www.lambdatest.com/support/docs/smartui-onboarding-puppeteer/).
