# LambdaTest SmartUI: Puppeteer Samples with SDK & LambdaTest Hooks

Welcome to the LambdaTest SmartUI Puppeteer samples repository. This guide provides detailed instructions on integrating Puppeteer with LambdaTest for automated, cloud-based testing, including visual regression testing using SmartUI. Discover how to leverage the power of Puppeteer alongside LambdaTest's extensive testing capabilities to ensure your web applications look and perform their best across a wide range of devices and browsers.

## Getting Started

### Prerequisites

Before you begin, ensure you have the following:
- An active LambdaTest account. [Sign up here](https://www.lambdatest.com/) if you don't have one.
- Your LambdaTest Username and Access Key, available in your LambdaTest profile.

### Initial Setup

Clone this repository to get started with SmartUI tests using Puppeteer:

```bash
git clone https://github.com/LambdaTest/smartui-puppeteer-sample.git
cd smartui-puppeteer-sample
```

Configure your environment with your LambdaTest credentials:

```bash
export LT_USERNAME="Your LambdaTest Username"
export LT_ACCESS_KEY="Your LambdaTest Access Key"
```
### Settings up SmartUI project

You, need to create a `SmartUI` project at [Lambdatest - SmartUI Web App](https://smartui.lambdatest.com/projects). Now, you need to follow the steps below: 
1. Click on the `New Project` button on the top right of the webpage.
2. Select your `Platform type` as `SDK` for running `SDK` sample test below, else you can select `Platform type` as `Web` for running `hooks` sample.
3. Provide name of your choice for the project.
4. Now, add `Approvers` who are required to review the changes and approve/reject the results of the tests.
5. (Optional) You can add `tags` of your choice such as `uat`, `dev` etc..
6. Click on the `Get Started` button for completing the project creation.
7. Now, select `NodeJS` setup guide and in the `Step 2` you can find the project token.

### Setting up Project token: 
Once, you have successfully setup the project for the `SmartUI` and copiec the `Project Token` from the `SmartUI Web App`: 

```bash
export PROJECT_TOKEN="<Your Copied Project Token to be pasted here>" 
```

## Testing with LambdaTest SDK

### Overview

Our sample tests demonstrate navigating to the LambdaTest homepage to verify the page title and conducting a full-page screenshot for visual regression testing.

#### Setup

Navigate to the SDK sample directory and install dependencies:

```bash
cd sdk
npm install
```

#### Using SmartUI with Puppeteer

LambdaTest's SmartUI SDK enhances your testing with automated visual regression capabilities. Here's how to capture a full-page screenshot:

```javascript
const { smartuiSnapshot } = require('@lambdatest/puppeteer-driver');

await smartuiSnapshot(page, "Your_Screenshot_Name");
```

Replace `"Your_Screenshot_Name"` with a meaningful identifier. Screenshots are stored in LambdaTest for seamless UI comparison over time.

### Execution

Execute tests locally or on the LambdaTest Automation Cloud grid:

**Local Execution:**
```bash
npx smartui exec node puppeteerLocal.js
```

**Cloud Execution:**
```bash
npx smartui exec node puppeteerCloud.js
```

**Note**: You can also use the npm scripts defined in `package.json`:
- Local: `npm run smartui-local`
- Cloud: `npm run smartui-cloud`

Visit our [documentation](https://www.lambdatest.com/support/docs/smartui-puppeteer-sdk/) for comprehensive SDK guides and tutorials.

## Testing with LambdaTest Hooks

### Overview

Like the SDK samples, these tests navigate to the LambdaTest homepage for title verification and visual regression via screenshot.

#### Setup

Access the Hooks sample directory and prepare your environment:

```bash
cd hooks
npm install
```

#### Leveraging SmartUI Webhooks

Use LambdaTest's webhook for seamless visual regression testing. Capture a screenshot with:

```javascript
await page.evaluate(() => {
  // Replace "Your_Screenshot_Name" with the screenshot identifier
  const screenshotName = "Your_Screenshot_Name";
  const lambdatestAction = JSON.stringify({
    action: 'smartui.takeScreenshot',
    arguments: { fullPage: true, screenshotName }
  });
  // Execute the SmartUI action
});
```

### Running the Tests

Deploy your tests on the LambdaTest Automation grid with:

```bash
npm run single
```

## Support and Assistance

Our dedicated support team is available 24/7 to assist with any questions or challenges you may encounter. Contact us anytime at [support@lambdatest.com](mailto:support@lambdatest.com) for prompt and friendly support.
