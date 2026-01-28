# Test: Generate End-to-End Tests (Playwright/Cypress)

**Goal**: Create user-centric E2E tests to verify full workflows in the browser.

## Context
E2E tests ensure the application works from the user's perspective. Focus on user interactions (clicks, typing) and visible outcomes.

## Instructions

1.  **Define the Scenario**: logical flow (e.g., "User logs in and updates profile").
2.  **Select Locators**:
    *   Prioritize user-facing attributes: `getByRole`, `getByText`, `getByLabel`.
    *   Avoid brittle selectors like XPaths or CSS classes (`.div > .btn`).
    *   Use `data-testid` only as a last resort.
3.  **Write the Test**:
    *   Navigate to the page.
    *   Perform actions (fill forms, click buttons).
    *   Assert visibility of success messages or page transitions.
4.  **Handle Async**: Ensure `await` is used correctly for all interactions.

## Output Format (Playwright Example)

```typescript
import { test, expect } from '@playwright/test';

test.describe('Feature Name', () => {
  test('User can complete the primary workflow', async ({ page }) => {
    // 1. Navigate
    await page.goto('/login');

    // 2. Interact
    await page.getByLabel('Email').fill('user@example.com');
    await page.getByLabel('Password').fill('password123');
    await page.getByRole('button', { name: 'Sign In' }).click();

    // 3. Assert
    await expect(page.getByText('Welcome back')).toBeVisible();
    await expect(page).toHaveURL('/dashboard');
  });
});
```
