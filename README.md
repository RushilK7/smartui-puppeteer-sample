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

This repository also includes examples for using SmartUI with LambdaTest Hooks integration. Hooks-based integration allows you to use SmartUI directly within your existing LambdaTest Cloud automation tests without requiring the SmartUI CLI.

### SDK vs Hooks: Which Approach to Use?

**SDK Approach (Recommended for Local Testing):**
- ✅ Works with both local and cloud execution
- ✅ Uses SmartUI CLI for configuration and execution
- ✅ Supports multiple browsers and viewports via `smartui-web.json`
- ✅ Better for CI/CD integration
- ✅ Requires `PROJECT_TOKEN` environment variable

**Hooks Approach (Recommended for Cloud-Only Testing):**
- ✅ Works only with LambdaTest Cloud Grid
- ✅ No CLI required - direct integration with LambdaTest
- ✅ Uses LambdaTest capabilities for configuration
- ✅ Better for existing LambdaTest automation suites
- ✅ Requires `LT_USERNAME` and `LT_ACCESS_KEY` environment variables

### Hooks Integration Setup

**Location:** See the `hooks` folder for hooks integration examples.

**Purpose:** Use LambdaTest's webhook for seamless visual regression testing in Puppeteer tests running on LambdaTest Cloud Grid.

**Documentation:** [LambdaTest Puppeteer Visual Regression Documentation](https://www.lambdatest.com/support/docs/puppeteer-visual-regression/).

### Hooks Setup Steps

#### 1. Install Dependencies

Navigate to the hooks directory and install dependencies:

```bash
cd hooks
npm install puppeteer
```

#### 2. Configure Environment Variables

Set your LambdaTest credentials:

```bash
export LT_USERNAME='your_username'
export LT_ACCESS_KEY='your_access_key'
```

#### 3. Configure Capabilities

In your test file (e.g., `hooks/samplePuppeteer.js`), configure the capabilities with SmartUI options:

```javascript
const capabilities = {
  "browserName": "Chrome",
  "browserVersion": "latest",
  "LT:Options": {
    "platform": "Windows 10",
    "build": "Puppeteer SmartUI Build",
    "name": "Puppeteer SmartUI Test",
    "user": process.env.LT_USERNAME,
    "accessKey": process.env.LT_ACCESS_KEY,
    "network": true,
    "video": true,
    "console": true,
    "smartUIProjectName": "SmartUI_Puppeteer_sample#123"  // Your SmartUI project name
  }
};
```

#### 4. Connect to LambdaTest CDP

Connect Puppeteer to LambdaTest Cloud using CDP (Chrome DevTools Protocol):

```javascript
const browser = await puppeteer.connect({
  browserWSEndpoint: `wss://cdp.lambdatest.com/puppeteer?capabilities=${encodeURIComponent(JSON.stringify(capabilities))}`
});

const page = await browser.newPage();
```

#### 5. Add Screenshot Hooks

Use `page.evaluate()` with `lambdatest_action` to capture screenshots:

```javascript
// Navigate to your page
await page.goto('https://www.lambdatest.com');

// Take a full-page screenshot
await page.evaluate((_) => {},
  `lambdatest_action: ${JSON.stringify({
    action: 'smartui.takeScreenshot',
    arguments: {
      fullPage: true,
      screenshotName: 'homepage-screenshot'
    }
  })}`);

// Take a viewport screenshot (visible area only)
await page.evaluate((_) => {},
  `lambdatest_action: ${JSON.stringify({
    action: 'smartui.takeScreenshot',
    arguments: {
      fullPage: false,
      screenshotName: 'homepage-viewport'
    }
  })}`);
```

#### 6. Run the Test

Execute your test script:

```bash
cd hooks
node samplePuppeteer.js
```

Or use npm scripts if available:

```bash
npm run single
```

### Advanced Hooks Examples

#### Multiple Screenshots in One Test

```javascript
await page.goto('https://www.lambdatest.com');

// Screenshot 1: Homepage
await page.evaluate((_) => {},
  `lambdatest_action: ${JSON.stringify({
    action: 'smartui.takeScreenshot',
    arguments: { fullPage: true, screenshotName: 'homepage' }
  })}`);

// Navigate and take another screenshot
await page.goto('https://www.lambdatest.com/pricing');
await page.evaluate((_) => {},
  `lambdatest_action: ${JSON.stringify({
    action: 'smartui.takeScreenshot',
    arguments: { fullPage: true, screenshotName: 'pricing-page' }
  })}`);
```

#### Screenshot After User Interaction

```javascript
await page.goto('https://www.bing.com');

// Interact with the page
const searchBox = await page.$('[id="sb_form_q"]');
await searchBox.click();
await searchBox.type('LambdaTest');
await page.waitForTimeout(3000);
await page.keyboard.press('Enter');

// Take screenshot after interaction
await page.evaluate((_) => {},
  `lambdatest_action: ${JSON.stringify({
    action: 'smartui.takeScreenshot',
    arguments: { fullPage: true, screenshotName: 'search-results' }
  })}`);
```

#### Set Test Status

You can also set test status using hooks:

```javascript
try {
  const title = await page.title();
  expect(title).toEqual('Expected Title');
  
  // Mark test as passed
  await page.evaluate(_ => {},
    `lambdatest_action: ${JSON.stringify({
      action: 'setTestStatus',
      arguments: { status: 'passed', remark: 'Title matched' }
    })}`);
} catch {
  // Mark test as failed
  await page.evaluate(_ => {},
    `lambdatest_action: ${JSON.stringify({
      action: 'setTestStatus',
      arguments: { status: 'failed', remark: 'Title not matched' }
    })}`);
}
```

### Hooks Configuration Options

The `smartUIProjectName` in capabilities is required. You can also add:

- **smartUIBaseline**: Set to `false` to update baseline (default: `true`)
- **smartUIBuild**: Optional build name for organizing screenshots

### View Hooks Results

After running your hooks-based tests, visit the [LambdaTest Automation Dashboard](https://automation.lambdatest.com/) to view:
- Test execution status
- Screenshots captured
- Visual comparison results
- Build and session details

Navigate to your SmartUI project in the [SmartUI Dashboard](https://smartui.lambdatest.com/) to see detailed visual regression results.

## Best Practices

### Screenshot Naming

- Use descriptive, unique names for each screenshot
- Include page/component name and state
- Avoid special characters
- Use consistent naming conventions

### When to Take Screenshots

- After critical user interactions
- Before and after form submissions
- At different viewport sizes
- After page state changes

### Puppeteer-Specific Tips

- Use `page.waitForSelector()` before screenshots
- Wait for network idle: `await page.goto(url, { waitUntil: 'networkidle0' })`
- Use `page.waitForTimeout()` for animations
- Take screenshots after interactions complete

### Example: Screenshot After Interaction

```javascript
await page.goto('https://www.lambdatest.com');
await page.waitForSelector('#search');

await page.type('#search', 'Puppeteer');
await page.keyboard.press('Enter');
await page.waitForSelector('.results');

await smartuiSnapshot(page, "search-results");
```

## Common Use Cases

### Responsive Testing

```javascript
const viewports = [
  { width: 1920, height: 1080, name: 'desktop' },
  { width: 768, height: 1024, name: 'tablet' },
  { width: 375, height: 667, name: 'mobile' }
];

for (const viewport of viewports) {
  await page.setViewport({ width: viewport.width, height: viewport.height });
  await page.goto('https://www.lambdatest.com');
  await smartuiSnapshot(page, `homepage-${viewport.name}`);
}
```

## CI/CD Integration

### GitHub Actions Example

```yaml
name: Puppeteer SmartUI Tests

on: [push, pull_request]

jobs:
  visual-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: |
          cd sdk
          npm ci
      
      - name: Run SmartUI tests
        env:
          PROJECT_TOKEN: ${{ secrets.SMARTUI_PROJECT_TOKEN }}
          LT_USERNAME: ${{ secrets.LT_USERNAME }}
          LT_ACCESS_KEY: ${{ secrets.LT_ACCESS_KEY }}
        run: |
          cd sdk
          npx smartui exec node puppeteerCloud.js
```

## Troubleshooting

### Issue: `Cannot find module '@lambdatest/puppeteer-driver'`

**Solution**: Install dependencies:
```bash
cd sdk
npm install @lambdatest/smartui-cli @lambdatest/puppeteer-driver puppeteer
```

### Issue: Screenshots not captured

**Solution**:
1. Verify `PROJECT_TOKEN` is set
2. Add waits before screenshots
3. Ensure page is fully loaded
4. Check for JavaScript errors in console

### Issue: Timeout errors

**Solution**: Increase wait times:
```javascript
await page.goto('https://example.com', { 
  waitUntil: 'networkidle0',
  timeout: 60000 
});
await page.waitForTimeout(2000);
await smartuiSnapshot(page, "screenshot");
```

## Configuration Tips

### Optimizing `smartui-web.json`

```json
{
  "web": {
    "browsers": ["chrome", "firefox"],
    "viewports": [
      [1920, 1080],
      [1366, 768],
      [375, 667]
    ],
    "waitForPageRender": 30000,
    "waitForTimeout": 2000
  }
}
```

## Additional Resources

- [SmartUI Puppeteer Onboarding Guide](https://www.lambdatest.com/support/docs/smartui-onboarding-puppeteer/)
- [Puppeteer Documentation](https://pptr.dev/)
- [LambdaTest Puppeteer Documentation](https://www.lambdatest.com/support/docs/puppeteer-testing/)
- [SmartUI Dashboard](https://smartui.lambdatest.com/)
- [LambdaTest Community](https://community.lambdatest.com/)

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
