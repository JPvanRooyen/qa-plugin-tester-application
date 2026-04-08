# 02 Test Cases

QA Test Case register derived from my Fluent Boards board export.

## Document Control

| Field | Value |
|---|---|
| Document | 02-test-cases.md |
| Project | WPMU DEV QA Applicants Test Project (JP van Rooyen) |
| Generated from | Fluent Boards JSON board export |
| Generated on | 2026-04-08 |
| Author | JP van Rooyen |
| Purpose | Consolidated record of planned and executed manual QA test cases. |


## TC001 — Installation and Activation

Steps:

1. Upload plugin
2. Click install
3. Activate plugin

Expected:

- Plugin installs successfully
- No errors displayed

## TC002 — First impressions (Admin Menu, UX)

Steps:

1. Install and Activate plugin

Expected:

- Plugin admin menu appears in dashboard
- Easy and intuitive to navigate to the plugin

## TC003 — Load plugin admin page(s)

Steps:

1. Visit plugin admin page

Expected:

- No visible errors or warnings
- no php errors
- no browser console warnings or errors

## TC004 — Navigate Plugin Page(s) and/or links (UI, UX)

Steps:

1. Visit plugin admin page
2. Navigate through UI, form fields, links, perform rapid clicks, etc

Expected:

- Easy and intuitive to navigate plugin admin area
- All form fields visible, properly styled and aligned
- Required fields are indicated (typically with "\*")
- No visible errors or warnings
- no php errors
- no browser console warnings or errors

## TC005 — Attempt to save without input in form fields

Steps:

1. Navigate to plugin admin page
2. Click "Save Changes" button without filling any fields

Expected:

- Saving should be blocked by required fields
- Required fields should display admin notice warning that the fields are required

## TC006 — Confirm all options save, update, read @ database
Most obvious test = blackbox submission test i.e. visual feedback / values persist after saves

Steps:

1. Visit plugin admin page
2. Fill out all fields with valid input
3. Click "Save Changes"
4. Allow page to reload

Expected:

- All fields should be populated with the values entered
- All fields should persist even after
  - reloading page
  - clearing cache

## TC007 — Validation tests at all fields

Steps:

1. Visit Plugin admin page
2. Fill out all fields with various values to test validation logic
3. Click "Save Changes" after entering values

Expected:

- Invalid input in any field should trigger admin notices, showing validation errors
- Invalid input should block saving

**The following were tested during multiple save operations (more individual tests will be done for specific test cases):**

- Full Name
  - Input numbers
  - Input special characters
  - Input extremely large number of characters
- Nickname
  - Input numbers
  - Input special characters
  - Input extremely large number of characters
  - Include a space in the input
- Address
  - Input numbers
  - Input special characters
  - Input extremely large number of characters
- DOB Day
  - Input "0" (zero)
  - Input more days than any given month actually has
  - Input extremely high number
  - Input characters/letters other than numeric
  - Input extremely large number of characters
- DOB Month
  - Attempt typing in dropdown
- DOB Year
  - Input "0" (zero)
  - Input text and characters other than numeric
  - Test date range
  - Test very young age (recent years)
- Email Address
  - Input invalid email address formats, including
    - without @
    - without tld
    - invalid tld
    - etc
- Website
  - Test protocol enforcement as field description suggests
  - Test protocol-only submission (http:// or https:// without anything else)
  - Test with invalid protocol (eg. "httpa://), resembling a protocol, but not actually a protocol
  - test space
  - test without tld
  - test invalid tld
  - test any invalid url

## TC008 — Test all Required Fields individually

Steps:

1. Visit Plugin admin page
2. Leave one field blank
3. Fill out all remaining fields with valid input
4. Click "Save Changes" after entering values

Expected:

- If a blank field is required, it should trigger admin notice
- The required field should block saving

**All Fields tested: 8 Separate tests were done, testing each field one by one.**

## TC009 — Test all Dropdown Fields

Steps:

1. Visit Plugin admin page
2. Test dropdown field for proper functionality, including
   1. Attempt typing in dropdown field
   2. Check for spelling/grammar
   3. Check for missing options dependent on what the dropdown is for
   4. Check for inactive/broken options (not selectable)
   5. Check that each option saves successfully
3. Click "Save Changes" for each test and allow page to refresh
4. Repeat saving for each test

Expected:

- Manual typing not possible (unless used for filtering/selection assistance)
- No errors upon saving or selecting options
- All options save successfully
- No missing options (dependent on dropdown's function, such as date, month, etc)

## TC010 — Date Range tests (DOB Year field)

Steps:

1. Visit plugin admin page
2. Test Various date ranges in the "Date of Birth Year" field
3. Click "Save Changes"
4. Allow page to reload

Expected:

- Validation error notice should appear between a certain Year range, including
  - Very old age (early year)
  - Very young age (recent year)
- Invalid years should block saving

## TC011 — Blank Fields test - clear data tests

Steps:

1. Visit plugin admin page
2. Delete / clear all fields
   1. either all at once, or one by one and repeat test
3. Click "Save Changes"
4. Allow page to reload

Expected:

- Cleared fields should be blank (persistent after saving), meaning the field's value in the db option should be cleared or "zeroed"
- Required fields should block saving, therefore should not be blank

## TC012 — Sanitization tests

Steps:

1. Visit plugin admin page
2. Fill out all fields with potentially dangerous code, such as html, script, etc.
3. Click "Save Changes"
4. Allow page to reload

Expected:

- Any field with such input should be sanitized before saving
- Any field with unsanitized input should trigger admin warning notices
- Any field with unsanitized input should block saving

## TC013 — Performance impact

Most obvious test = blackbox submission test i.e. visual feedback / values persist after saves

Steps:

1. Visit plugin admin page, check performance impact
2. Save and reload page, check performance impact

Expected: Plugin should have little to no impact on site performance / page loading, especially such a simple plugin.

## TC014 — Spelling and Grammar checks

Steps:

1. Visit plugin admin page(s)
2. Check spelling and grammar throughout plugin (mainly based on the plugin's main/default language), including
   1. Admin menu item(s)
   2. Page title and browser tab title
   3. Headings
   4. Instructions sections
   5. Field labels, descriptions, and place holder text
   6. Buttons and links
   7. Any text rendered by the plugin
3. Potentially repeat with tools for checking any available language translations in the plugin's language/localization

Expected:

- No spelling or grammatical errors, especially the plugin's main/default language

## TC015 — Deactivate Plugin

Steps:

1. Deactivate plugin from wp-admin Plugins page
2. Check whether database option(s) remain in tact

Expected:

- Database entries should persist

## TC016 — Delete Plugin

Steps:

1. Deactivate plugin from wp-admin Plugins page
2. Check whether database option(s) remain in tact

Expected:

- Database entries should persist in this case, since there is no setting to opt for db deletion

## TC017 — Re-install plugin after deletion

Steps:

1. Install plugin again
2. Visit plugin page(s)

Expected:

- Database entries (previously saved fields) should persist

## TC018 — Delete db option while plugin is installed.

Steps:

1. Open site database with phpAdmin or similar
2. Find plugin's db option(s) - this case, wp\_options -> qa\_test\_options
3. Delete the plugin's updatable option(s) - this case, the form fields
4. Visit the plugin's admin page

Expected:

- Plugin should function normally, but all form fields should be blank

## TC019 — Save Plugin form after deleting db option

Steps:

1. Visit the plugin's admin page
2. Fill in the form fields
3. Click "Save Changes" and allow page to refresh

Expected:

- Database options should be recreated and updated with the saved values
- The saved values should persist in the plugin's form

## TC020 — Check for Broken Links

Steps:

1. Visit the plugin's admin and/or Frontend page(s)
2. Check all links and buttons for broken links
3. Check all admin menu items belonging to the plugin

Expected:

- All links work as expected

**Broken Links Checker plugin could be used for any Front-end pages (not applicable in this plugin's case)**

## TC021 — Basic Accessibility checks

Steps:

1. Visit plugin admin page(s)
2. Check for basic accessibility requirements

Expected:

- Basic accessibility requirements should be adhered to, especially if required by law

## TC022 — Accessibility: Keyboard-only

Steps:

1. Visit the plugin's admin page
2. Navigate with keyboard only
   1. Tab through all fields, in order
   2. Use up/down arrows in dropdown/numeric fields
   3. Space to submit

Expected:

- Form works as expected
- The next field is highlighted/outlined for visual feedback after tab (in proper order)
- No fields are skipped
- User can type into newly selected field (or change selection options of a dropdown or numeric field)
- Form submits and updates database when pressing space while "Save Changes" button is higlighted

## TC023 — Accessibility / UX: Tooltips

Steps:

1. Visit the plugin's admin page
2. Hover over field labels

Expected:

- Tooltips should appear

## TC024 — Accessibility / UX: Enter to save

Steps:

1. Visit the plugin's admin page
2. Fill out a form field
3. Press Enter

Expected:

- Form saves and updates database
- Page refreshes
- Values persist

## TC025 — Accessibility: Audio (Screen reader)

Steps:

1. Visit the plugin's admin page
2. Fill out a form field
3. Press Enter

Expected:

- Form saves and updates database
- Page refreshes
- Values persist

## TC026 — Accessibility / UX: Color Contrast

Steps:

1. Visit the plugin's admin page
2. Assess ease to read and view everything
   1. Optionally view whether contrast ratio passes in "Inspect Element -> Styles -> click color box -> Contrast Ratio report"

Expected:

- All content should be easy to view and read
- Contrast ratio in Inspector should pass

## TC027 — Accessibility / UX: Click Label To Focus

Steps:

1. Visit the plugin's admin page
2. Fill out a form field
3. Press Enter

Expected:

- Form saves and updates database
- Page refreshes
- Values persist

## TC028 — Permissions test for various User Roles

Steps:

1. Log in as different users (admin, editor, author, contributor, subscriber, etc)
2. Visit the plugin's admin page
   1. by clicking on menu if menu item still exists
   2. or by visiting "/wp-admin/options-general.php?page=qa-test-settings" directly
3. Attempt to navigate plugin and update form fields and save

Expected:

- Only admin or specified roles are allowed to access plugin settings
- In this case, no additional roles are specified in the code

## TC029 — Theme Compatibility tests

Steps:

1. Change to different wp themes
2. Visit the plugin's admin page
3. Test plugin's basic functionality

Expected:

- No errors should occur during page loads
- No errors should occur during saving

## TC030 — Plugins Compatibility tests

Steps:

1. Activate various plugins
2. Visit the plugin's admin page
3. Test plugin's basic functionality

Expected:

- No errors should occur during page loads
- No errors should occur during saving

## TC031 — Cypress automation - repeating all basic tests that make sense to be automated

Steps:

1. Run various automation tasks with Cypress including
   1. user permissions
   2. form submissions with valid input
   3. form submissions with invalid input
   4. etc

Expected:

- Tests results are consistent with manual tests already completed

## TC032 — Different Browsers compatibility tests

Steps:

1. Visit and submit the form with various browsers

Expected:

- Results consistent with original tests

## TC033 — Repeat basic tests in various environments

Steps:

1. Repeat previously completed basic tests in various environments, including
   1. Different WordPress version
   2. Different php
   3. Different servers:
      1. Local
      2. Staging
      3. Production
      4. Password-protected site
      5. etc.
2. Do more in-depth testing if any of the basic tests fail or the results differ from the initial tests.

Expected:

- Test results are consistent throughout various environments

## TC034 — Rapid repeated form submission

Steps:

1. Visit plugin page
2. fill out form fields with valid input
3. Click the submit button multiple times, rapidly

Expected:

- One single save operation
- Additional saves or clicks are ignored

## TC035 — Basic, high level code inspection

Steps:

1. Open plugin file "qa-applicants-task.php" in Sublime / editor
2. Inspect code sections for things like:
   1. Best practice security implementations
   2. Sensible validation and error handling
   3. Sensible and clearly functional database/save/update logic
   4. Sensible logic flow
   5. Audit the code for any flaws or improvements based on what is understood of the code

Expected:

- Security should be adequate
- Conflict prevention should be in place
- Save functionality should be in order
- Database interaction should be trustworthy
- Proper error handling should be in place
- No obvious/noticeable code flaws should be present

## TC036 — User Roles permissions for Edge cases

(eg. Editor without plugin access / with and without manage\_options)

Steps:

1. Switch to a users with different user roles (or edit the user roles)
2. Visit the plugin page and/or
3. Submit the form

Expected:

- Without the proper role and permissions a user cannot access the plugin's page

## TC037 — Form security like Recaptcha / Honeypot

I considered the need for form security such as honeypot or recaptcha, but deemed it a low priority in this plugin's use case:

- The plugin is scoped in the wp-admin area, which is already protected by login of a user with sufficient privelages
- While technically possible, abuse, especially by bots in this context is highly unlikely

## TCxxx — Regression Tests - Not viable/possible for this testing purpose, since no patches can be provided for retesting

Under normal circumstances I would do regression testing after receiving patched code addressing my bug reports.
