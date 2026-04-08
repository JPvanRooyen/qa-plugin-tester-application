# Test Plan – WordPress Plugin Beta Testing

## Objective

To validate the functionality, usability, validation logic, and compatibility of the provided plugin, including identifying defects and potential code-level risks.

Specific instructions on testing plugin page:
* Basic Black Box testing: "You can enter various options here. Do any tests you need to ensure all fields are working properly."
* Basic Reporting with pass/fail notice: "In test results submit a description of all the tests you have done and results as pass or fail."
* Use my own intuition: "You can provide multiple tests for single field."

## Scope

* Installation & activation
* Admin UI rendering
* Input validation
* Data persistence
* Basic Accessibility audit
* Compatibility (themes, browsers)

## Test Types

* Black-box testing (primary)
* Exploratory testing
* Functional testing
* Compatibility testing
* Regression testing - not viable as no patches of my reports are provided

## Environment

* WordPress 6.x (Local)
* PHP 8.x
* Browser: Chrome, Firefox
* OS: macOS

## Tools

* Chrome DevTools
* Cypress (automation)
* GitHub (documentation)
* Various WordPress plugins and Browser Extensions
* Fluent Boards (Kanban based plugin on my own website)
* My own custom made "QA Plugin Stack" plugin to handle bulk install/activate/deactivate of plugins for testing assistance
