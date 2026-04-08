# 03 Bug Reports

QA Bug Report derived from my Fluent Boards board export.

My live testing project with BUG-Reports can be viewed at https://webdesignchat.com/projects#/boards/3 
## See my cover letter for login detail.

## Document Control

| Field | Value |
|---|---|
| Document | 03-bug-reports.md |
| Project | WPMU DEV QA Applicants Test Project (JP van Rooyen) |
| Generated from | Fluent Boards JSON board export |
| Generated on | 2026-04-08 |
| Author | JP van Rooyen |
| Purpose | Consolidated bug report for the tested plugin, preserving report detail from the source board. |

## Scope and Notes

- Source list: **Bug Reports**
- Total exported bug report entries in this document: **37**
- The built-in Fluent Boards JSON export did not include direct attachment URLs. Please refer to the separate folder named "Screenshots and Videos" for numbered attachments.


## BUG001 — UX improvement: Obscure Admin Menu Location

| Evidence Attachments Recorded | 0 |

### Details

**Environment:** WordPress 6.9.4, Local, Single Site, Chrome

**Steps to Reproduce:**

1. Activate plugin
2. Hover over "Settings" in wp admin menu
3. Find Plugin Menu "QA Test"

**Actual Result:**

- Menu location is not intuitive, especially for inexperienced users

**Expected Result:**

- Top Level menu position

**Severity:** Low

**Notes:** This may be intended due to the simple nature of the plugin, but production/commercial plugins make better sense to have their own top-level menu position. Alternatively, proper documentation with clear instructions will help.

## BUG002 — PHP warnings: 19x "Trying to access array offset on value of type bool"

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

**Environment:** WordPress 6.9.4, Local, Single Site, Chrome

**Steps to Reproduce:**

1. First time visit to plugin admin page
   1. Or, while plugin's database option does not yet exist, meaning, before any save operation was done
   2. Or, after deleting the plugin's option from database wp\_options -> qa\_test\_options
2. Check for php errors

**Actual Result:**

- PHP warning "Trying to access array offset on value of type bool" triggered by the following lines in the plugin main file:
  - Line 94 x 7 counts
  - Line 107 x 1 count
  - Line 110 x 11 counts

**Impact:**

- Could break page rendering in strict environments (eg. converting warnings into fatal errors with "set\_error\_handler(...)")
- Unnecessary error displays and bloat of error logs

**Expected Result:**

- No php warnings

**Severity:** High

**Notes:**

- This occurs only on initial page load before any options are saved, or after deleting the db option, before saving the form.
- The error persists until the save action was run, creating the plugin's db option.

**Recommendation:**

- Improve error handling
- Avoid $option returning as false in "$name\_options = esc\_attr( $options[ $name ] );" when database option does not exist
- Same goes for both Lines 94, 107, 110
- $options should either be an array at all times, or fall back to blank value if not an array

## BUG003 — PHP warnings: 2x Undefined Array Key on Ln94

| Evidence Attachments Recorded | 2 |

> Note: The source JSON confirms 2 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

**Environment:** WordPress 6.9.4, Local, Single Site, Chrome

**Steps to Reproduce:**

1. Visit plugin admin page
2. Click "Save Changes"

**Actual Result:**

- 2x php warnings: "Undefined array key" @ plugin file line 94
  - qa\_test\_nickname (the "Nickname" field)
  - qa\_test\_web (the "Website" field)

**Expected Result:**

- No php warnings

**Severity:** Critical

**Notes:**

- "Undefined Array Key" suggests there will be a problem saving to database.
  - Checking the database, I confirmed these two options are not saved in the db option (See BUG004)
- The error will likely disappear once these two fields have been properly coded to save, and they have been saved in the database.

## BUG004 — "Nickname" and "Website" don't persist

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

**Environment:** WordPress 6.9.4, Local, Single Site, Chrome

**Steps to Reproduce:**

1. Visit plugin admin page
2. Fill out form fields with valid content
3. Click "Save Changes" and allow page to refresh

**Actual Result:**

- Nickname field is not populated after saving with valid input
- Website field is not populated after saving with valid input

**Expected Result:**

- All fields should save and persist after saving with valid content

**Severity:** Critical

**Notes:**

- This bug correllates with BUG003 ("Undefined array key" @ plugin file line 94, for both these two options)
- These two fields are absent in database wp\_options -> qa\_test\_options

Recommendation:

- Ensure both fields have proper logic to save to database
- Ensure both fields have proper logic to read from their database option, to persist after saving/updating

## BUG005 — Inconsistent Required Fields

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

**Environment:** WordPress 6.9.4, Local, Single Site, Chrome

**Steps to Reproduce:**

1. Visit plugin admin page for the first time (all fields still blank)
2. Click "Save Changes" without filling any fields

**Actual Result:**

- Nickname field is marked as required in UI, but no admin notice appears, suggesting it is not required in the logic
  - Further (individual) testing will be done
- Address field seems to be required by logic, but is not marked as required in UI
  - Further (individual) testing will be done
- Email Address is marked as required in UI, but no admin notice appears, suggesting it is not required in the logic
  - Further (individual) testing will be done
- Website field triggers Admin notice "Website is not valid", but should not be triggered when left blank, especially since it is not a required field
  - Further (individual) testing will be done to confirm whether this actually blocks saving, or whether it is only a notice issue
- The "Required Field" admin notices render twice

**Expected Result:**

- Based on fields marked as required ("\*"), saving should be blocked by
  - Full Name
  - Nickname
  - Email Address
- Website field should not trigger "Website not valid" notice when blank

**Severity:** High

**Notes:**

- Confirm which fields should be required
- Ensure the logic for each required field blocks saving and provides admin notice
- Ensure each required field is marked in UI as required ("\*")
- Fix Website field notice to be exempt when blank

## BUG006 — Empty Required fields do not block updating the options

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

**Environment:** WordPress 6.9.4, Local, Single Site, Chrome

**Steps to Reproduce:**

1. Visit plugin admin page
2. Clear all form fields
3. Click "Save Changes"

**Actual Result:**

- Although an admin notice appears to indicate required fields, it does not block the update action
- All fields, including all required fields are updated in the database, to "0"

**Expected Result:**

- Any required field that is blank should block updating completely

**Severity:** Critical

**Notes:**

- Ensure the update/save action is blocked by any required field that is left blank, and provide admin notice for visual feedback

## BUG007 — DOB Year should only allow numbers, not zero, and not outside the scope of the warning notice (number currently not restricted)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

- Outcome: - warns only 1800 to 2017 is allowed, but any value, including text is saved to db
  - inspection of code shows this warning is effective for 1970-2017, inconsistent with the warning message
  - However, any value entered is allowed, including script/code

## BUG008 — DOB Day should not allow zero, and not more than a month has days (number currently not restricted and no warning)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

Outcome:

- No warning
- No validation
- Allows saving of unlimited number value
- Does not enforce number of days per month

## BUG009 — UX Improvement: DOB day and year could be a date selector. -> Optimal to have single date selector for Day/month/year

| Evidence Attachments Recorded | 0 |

### Details

"Date of Birth Day","Date of Birth Month", "Date of Birth Year" provides poor UX.

A single field for "Date of Birth" is recommended, specifically a date selector widget.

## BUG010 — October missing in DOB Month dropdown

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG011 — UX Improvement: Inadequate Full Name validation (should allow "-", but should probably not allow dots)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG012 — UX improvement: Website field description is not clear for inexperienced users

| Evidence Attachments Recorded | 0 |

### Details

Website field description: "Website address including protocol" is not very obvious and inexperienced users might not understand. Especially in this field (preferably in all fields), specific examples as placeholder text would provide better UX.

## BUG013 — All text fields are without proper sanitization (html and other code is allowed -> serious potential security risks)

| Evidence Attachments Recorded | 0 |

### Details

While some escaping is present in the code related to this plugin, developers who know where to find the plugin's saved data could inject code via this plugin and use the saved code elsewhere on the site. This makes XSS vulnerability critical.

Someone could save malicious code in this form and echo it elsewhere (eg. echo $options['qa\_test\_web'];) which would execute the XSS

## BUG014 — UI/UX improvement: Responsive screen size layout

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

UI/UX improvement: Responsive screen size layout: Consistent margins & padding around the form would improve the UI. Currently, the lack of margin and padding is obvious on smaller responsive screen sizes

## BUG014 — UX: Deleting the plugin does not delete the db option(s). Users might require this, recommend adding a setting

| Evidence Attachments Recorded | 0 |

### Details

_No written description was present in the source export._

## BUG015 — UX improvement: Address field type

| Evidence Attachments Recorded | 0 |

### Details

Depending on the use case, the address field could at least be a Text Area in stead of just Text Field, but could ultimately be improved by including separate fields for Street, Suburb, City, Code, Country etc.

## BUG016 — Email Address field: Warns not valid, but does not block writing to the database

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG017 — Incomplete Website field validation

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

- Invalid protocol is not caught (example "x://") and does not trigger warning
- URL without tld is not caught and does not trigger warning

## BUG018 — Accessibility & UX: Implement Tooltips

| Evidence Attachments Recorded | 0 |

### Details

_No written description was present in the source export._

## BUG019 — UX improvement: Full Name validation notice could be improved from "Full Name consist of only letters and dots."

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG020 — Accessibility & UX: Error messages could be tied to their fields in-stead of appearing above the form

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG021 — Accessibility & UX: Form labels render like plain text and are not properly associated with input fields, preventing click-to-focus behavior and reducing accessibility for assistive technologies.

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

The field labels are not presented as labels, but as text.

The field labels can therefore not be clicked to focus/activate the text field

Rudimentary solution per field is typically as:

```
<input id="qa_test_fullname" name="qa_test_options[qa_test_fullname]" /> ```

However, this test plugin uses an array.
```

## BUG022 — Basic code flaw: "Website" field logic includes and unused array // It is also worth considering changing it from a text field to a url field for better UX and browser validation. (Line 40)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG023 — Basic code flaw: "Website" field logic will only save to db if the db cell is blank, meaning subsequent updates won't take effect. (Line 203)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

If Website saving logic was repaired to save, it might not update the db after initial save.

## BUG024 — Basic code flaw: No save input logic for Nickname field (Line 199-210)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG025 — Basic code flaw: DOB Year validation logic and admin notice do not match (Line 196-197)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG026 — Basic code flaw: Validation rules are inadequate and do not block saving (see "function plugin_options_validate( $input )") (Line 175-210)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG027 — Basic code flaw: I did not notice any sanitization logic

| Evidence Attachments Recorded | 0 |

### Details

_No written description was present in the source export._

## BUG028 — Basic code flaw in function qa_test_dob_m_field(): October is missing from the array (Line 149-165)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG029 — Basic code review: Noticed CodeSniffer was used by dev, who instructed it to ignore that no documentation was made. OK in this case. Documentation is NB in production plugins

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

_No written description was present in the source export._

## BUG030 — Plugin lacks the most basic Accessibility features

| Evidence Attachments Recorded | 0 |

### Details

Notes: While it makes sense that this testing plugin lacks the most basic accessibility features, commercial/production plugins should contain at least basic accessibility features.

Most important according to WCAG 4.1.2: Ensure every form element has a label

Target application: QA Test Page ‹ testss — WordPress - <https://testss.local/wp-admin/options-general.php?page=qa-test-settings>

List of basics missing:

- Element does not have an implicit (wrapped) label
- Element does not have an explicit label
- aria-label attribute does not exist or is empty
- aria-labelledby attribute does not exist, references elements that do not exist or references elements that are empty
- Element has no title attribute
- Element has no placeholder attribute
- Element's default semantics were not overridden with role="none" or role="presentation"

## BUG031 — Basic code check: Capability improvement potential for Best Practice

| Evidence Attachments Recorded | 0 |

### Details

**Description:**

The Plugin successfully enforces menu-level (and page load) capability restriction to admins (or users with manage\_options capability), but does not validate user permissions during execution.

**Risk:**

- Edge cases, custom endpoints, or misuse could provide unintended access

**Severity:** Medium

**Recommendation:**

- Extend capability checks beyond add\_options\_page(), into save logic etc.
- We could use a simple method like "if ( ! current\_user\_can('manage\_options') ) {
  return;
  }"

## BUG032 — Basic code flaw: Potential Plugin conflict

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

**Description:**

While the plugin operates normal with various plugins and themes active, there is potential for conflict due to missing namespace. Functions are global

**Impact:**

- Another plugin could use the same option name (qa\_test\_options), causing unintended results
- Data could be overwritten/destroyed
  - fatal warnings / broken UI could occur

## BUG033 — Basic code Best Practice: Inline CSS

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

**Description:**

The css for the required field asterisk is globally injected to the admin page at lines 38-41.

**Impact:**

- Inline CSS could conflict with Admin Themes or other UI elements

**Severity:** Low

## BUG034 — Basic code improvement: generic plugin slug (Lines 54 and 76)

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

**Description:**

"add\_settings\_section" and "do\_settings\_section" use a generic 'plugin' slug (not used by the plugin for rendering or page slug, but used by the Settings API).

**Impact:**

It could easily be used by other plugins and could conflict or cause bleed into other plugin pages.

**Severity:** Medium

**Recommendation:**

Use the constant "QTSLUG" which the developer has already used elsewhere in the plugin.

## BUG035 — Rapid repeat submission

| Evidence Attachments Recorded | 1 |

> Note: The source JSON confirms 1 attachment(s) for this item, but the built-in Fluent Boards JSON export did not include downloadable attachment URLs.

### Details

Steps:

Expected:

- One single save operation
- Additional saves or clicks are ignored

**Environment:** WordPress 6.9.4, Local, Single Site, Chrome

**Steps to Reproduce:**

1. Navigate to plugin settings
2. Fill out form fields with valid input
3. Click the submit button multiple times, rapidly

**Actual Result:**

- Missing or broken feature

**Expected Result:**

- One single save operation
- Additional saves or clicks should be ignored

**Severity:** Medium

**Notes:** Multiple rapid submits could impact performance and security, and could cause unintended data overwrites in the db.

## BUG036 — Basic code review: Security - Basic Escaping

| Evidence Attachments Recorded | 0 |

### Details

**Environment:** WordPress 6.9.4, Local, Single Site, Chrome

**Steps to Reproduce:**

1. Inspect code for basic escaping

**Actual Result:**

- While some very basic escaping exists, I believe escaping is not sufficient to cover all bases

**Expected Result:**

- Escaping should be robust enough to help ensure malicious code is not executed during output/rendering

**Severity:** Medium

**Notes:** Even if sanitisation is properly fixed, escaping should harden the security further to mitigate security risks on execution
