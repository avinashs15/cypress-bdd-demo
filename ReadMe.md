# Cypress Test Automation Project

This project demonstrates how to set up and execute Cypress tests using Page Object Model (POM) for test organization, feature files for BDD scenarios, step definitions for test actions, and Cypress CLI for test execution.

## Prerequisites

Before running the tests, ensure you have the following installed:

- Node.js (recommended LTS version)
- NPM (Node Package Manager)
- Cypress (installed locally in your project)

## Project Structure


cypress/
│
├── fixtures/         # Test data files (if any)
├── integration/      # Cypress test files (feature files, step definitions)
│   ├── features/     # Gherkin feature files
│   ├── step_definitions/  # Step definitions for Gherkin steps
├── plugins/          # Cypress plugins (if any)
├── support/          # Additional support files (page objects, utilities)
│   ├── page_objects/ # Page Object Model (POM) classes
│   ├── commands.js   # Custom Cypress commands (if any)
├── videos/           # Cypress test videos (if recorded)
├── screenshots/      # Screenshots (if captured during tests)
├── ...
├── cypress.json      # Cypress configuration file
└── tsconfig.json     # TypeScript configuration (if using TypeScript)

	- **fixtures/**: Contains test data files if needed for tests.
- **integration/**: Main folder for Cypress tests.
  - **features/**: Stores Gherkin feature files describing test scenarios.
  - **step_definitions/**: Holds JavaScript files defining step implementations for feature scenarios.
- **support/**: Additional support files for tests.
  - **page_objects/**: Houses Page Object Model (POM) classes for interacting with web elements.
  - **commands.js**: Custom Cypress commands if extended.
- **videos/** and **screenshots/**: Directories for storing recorded test videos and screenshots.
- **cypress.json**: Configuration file for Cypress.
- **tsconfig.json**: Configuration file for TypeScript (if applicable).

## Getting Started

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   cd <repository-folder>

2.	**Install dependencies:**
    npm install
    
Installs all dependencies listed in package.json, including Cypress.

3.  **nRun Cypress Test Runner:**
    npx cypress open

Opens the Cypress Test Runner GUI where you can interactively run and debug tests.

4.  **4. Run Cypress Tests in CLI:**
    npx cypress run

Executes Cypress tests in headless mode via the command line interface.

	•	Additional options:
	•	--browser: Specifies a browser to run tests (e.g., chrome, firefox).
	•	--spec: Runs specific test files or features.

## Writing Tests

### Page Object Model (POM)

Encapsulates interactions with web pages into reusable classes for better test maintenance.

	•	Create page objects under cypress/support/page_objects/.

Example: LoginPage.js

<pre>
```javascript
// cypress/support/page_objects/LoginPage.js

class LoginPage {
  visit() {
    cy.visit('/login');
  }

  fillUsername(username) {
    cy.get('#username').type(username);
  }

  fillPassword(password) {
    cy.get('#password').type(password);
  }

  submit() {
    cy.get('#login-button').click();
  }
}

export default new LoginPage();
</pre>

### Feature Files and Step Definitions

Uses Gherkin syntax for feature files (*.feature) and corresponding step definitions (*.js).

	•	Feature file example (checkout.feature):
<pre>
<details>
<summary>Click to expand! - Feature: User Login</summary>

```gherkin
Feature: Checkout process

  Scenario: User can add products to the cart and proceed to checkout
    Given I visit the Sauce Demo login page
    When I enter valid credentials
    And I add "Sauce Labs Backpack" to the cart
    And I add "Sauce Labs Bolt T-Shirt" to the cart
    And I proceed to checkout
    Then I should be on the checkout page
```

</details>
</pre>

### 	•	Step definition example (checkout.js):

<pre>
'''javascript
// cypress/integration/step_definitions/checkout.js

import { Given, When, Then } from '@cucumber/cucumber';
import LoginPage from '../../support/page_objects/LoginPage';
import ProductsPage from '../../support/page_objects/ProductsPage';

Given('I visit the Sauce Demo login page', () => {
  LoginPage.visit();
});

When('I enter valid credentials', () => {
  LoginPage.fillUsername('username');
  LoginPage.fillPassword('password');
  LoginPage.submit();
});

When('I add {string} to the cart', (productName) => {
  ProductsPage.addToCart(productName);
});

When('I proceed to checkout', () => {
  ProductsPage.navigateToShoppingCart();
});

Then('I should be on the checkout page', () => {
  cy.url().should('include', '/checkout');
});
</pre>