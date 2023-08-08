## Installing Playwright Demo

1. Pull down target codebase
```
git clone https://github.com/mikecannondeveloper/todo-cra.git
```
2. navigate to codebase and install dependencies
```
npm install
```
3. Install playwright, answer commandline questionnaire as listed
```
npm init playwright@latest
```

```✔ Do you want to use TypeScript or JavaScript? · JavaScript
✔ Where to put your end-to-end tests? · tests
✔ Add a GitHub Actions workflow? (y/N) · false
✔ Install Playwright browsers (can be done manually via 'npx playwright install')? (Y/n) · true

```
4. In "package.json", add the "e2e-test" script 
```
"e2e-test": "npx playwright test --ui",
```
5. In the newly created \tests directory, rename "example.spec.js" to "todo.spec.js", then replace the contents with the following
```
const { test, expect } = require('@playwright/test');

// This function navigates to the test page
const pageSetup = async (page) => {
  await page.goto('http://localhost:3000');
}

test('has title', async ({ page }) => {
  await pageSetup(page);
  // Expect a title to contain "To-Do".
  await expect(page).toHaveTitle(/To-Do/);
});

test('add a list item', async({page}) => {
  const testText = 'Test Item';

  await pageSetup(page);
  // Type a list item
  await page.locator('#todo-input').type(testText);
  // Click the '+' Button
  await page.locator('#add-button').click();
  // Get Text of all items in todo list
  const todoList = await page.locator('.todo').allInnerTexts();
  // Check for list item
  expect(todoList).toContain(testText);

});

test('delete a list item', async({page}) => {
  const testText = 'Test Item';

  await pageSetup(page);
  // Type a list item
  await page.locator('#todo-input').type(testText);
  // Click the '+' Button
  await page.locator('#add-button').click();
  // Click the 🗑️ Button
  await page.locator('.delete-button').click();
  // Get Text of all items in todo list
  const todoList = await page.locator('.todo').allInnerTexts();
  // Check for empty list
  expect(todoList).toHaveLength(0);

});
```
6. Now you should be able to run the test by first starting the page
```
npm run start
```
And running the playwright test from another terminal in the same directory
```
npm run e2e-test
```