# Streamax DS100 Webinar GTM + GA4 Tracking Test Report

Date: 2026-07-06

## 1. Project Goal

This project is a GitHub Pages static landing page test for the DS100 webinar funnel. The goal is to track anonymous source attribution, CTA clicks, register-page visits, form-start behavior, submit clicks, and thank-you conversion through GTM and GA4.

Current boundaries:

- No Salesforce connection.
- No lead database or backend form submission.
- No name, email, phone, company, or other PII sent to dataLayer, GTM, or GA4.
- GA4 is not hardcoded in page HTML. GA4 delivery must be configured in GTM.

## 2. Modified Files

- `index.html`
- `linkedin/index.html`
- `email/index.html`
- `sales/index.html`
- `qr/index.html`
- `register/index.html`
- `thank-you/index.html`
- `debug/index.html`
- `README_TEST.md`
- `TRACKING_TEST_REPORT.md`

## 3. File Changes

`index.html`

- Confirmed GTM head script and noscript iframe use `GTM-KCLRHBH9`.
- Confirmed no `GTM-XXXXXXX` placeholder remains.
- Confirmed `landing_page_view`, `click_register`, and `click_demo`.
- Confirmed fallback attribution:
  - LinkedIn referrer -> `source=linkedin`
  - empty referrer -> `source=direct`
  - other referrer -> `source=referral`
- Added anonymous session event history in `sessionStorage` for local cross-page testing.
- Added `referrer`, `cta`, `session`, and `intent` defaults to event payloads.
- Added bottom `Register for the Webinar` CTA:
  - `register/?session=general&cta=bottom_register`
- Confirmed `DS100TrackingDebug()` returns attribution, dataLayer, event history, current URL, and referrer.

`linkedin/index.html`

- Confirmed GTM installation.
- Writes attribution:
  - `source=linkedin`
  - `medium=organic_social`
  - `campaign=ds100_webinar_2026`
  - `content=linkedin_launch_post`
- Pushes `source_entry`.
- Redirects to `../`.
- Adds anonymous session event history.

`email/index.html`

- Confirmed GTM installation.
- Writes attribution:
  - `source=email`
  - `medium=edm`
  - `campaign=ds100_webinar_2026`
  - `content=email_invitation`
- Pushes `source_entry`.
- Redirects to `../`.
- Adds anonymous session event history.

`sales/index.html`

- Confirmed GTM installation.
- Writes attribution:
  - `source=sales_outreach`
  - `medium=direct_message`
  - `campaign=ds100_webinar_2026`
  - `content=sales_team`
- Pushes `source_entry`.
- Redirects to `../`.
- Adds anonymous session event history.

`qr/index.html`

- Confirmed GTM installation.
- Writes attribution:
  - `source=offline_event`
  - `medium=qr_code`
  - `campaign=ds100_webinar_2026`
  - `content=booth_poster`
- Pushes `source_entry`.
- Redirects to `../`.
- Adds anonymous session event history.

`register/index.html`

- Confirmed GTM installation.
- Pushes `register_page_view`.
- Pushes `form_start` once on first input.
- Pushes `form_submit_click` on submit click.
- Redirects to `../thank-you/`.
- Does not send form values to dataLayer.
- Adds `DS100TrackingDebug()`.
- Adds anonymous session event history.
- Demo route keeps `intent=demo` and does not force a session value when the URL has no `session`.

`thank-you/index.html`

- Confirmed GTM installation.
- Pushes `thank_you_view`.
- Adds `DS100TrackingDebug()`.
- Adds anonymous session event history.

`debug/index.html`

- New debug page.
- Shows current:
  - source
  - medium
  - campaign
  - content
  - referrer
  - page_location
  - first_landing_url
  - first_visit_time
  - current dataLayer events
  - session event history
- Provides `Clear Tracking Storage`.
- Provides `Refresh Debug Info`.
- Provides `DS100TrackingDebug()`.

`README_TEST.md`

- Updated upload checklist to include `debug/` and `TRACKING_TEST_REPORT.md`.
- Added debug-page local testing guidance.
- Added all GitHub Pages links that should be tested after upload.

## 4. Local Automated Test Results

Automation method:

- Used the in-app browser automation against `http://localhost:8080/`.
- Read anonymous test results from the `debug/` page DOM.
- Did not read or export cookies, passwords, tokens, or account data.
- Did not send form values to GTM or GA4.

Static checks:

- All 8 HTML pages include GTM head script and noscript iframe with `GTM-KCLRHBH9`.
- No `GTM-XXXXXXX` remains.
- No GA4 Measurement ID is hardcoded in page HTML.
- All inline scripts passed JavaScript syntax checks.
- CTA static checks passed for Thailand, Vietnam, Indonesia, bottom Register, and Demo.
- No upload-blocking files found after cleanup: no `.DS_Store`, no `node_modules`, no Playwright reports, no traces, no screenshots, no `.env`, no token/password files.

Browser tests:

| Test | Result | Evidence |
| --- | --- | --- |
| Debug page | Passed | Clear and Refresh buttons worked; debug page rendered current URL and empty state after clear. |
| LinkedIn full chain | Passed | `/linkedin/` -> `/` -> Thailand CTA -> `/register/?session=thailand&cta=thailand_card` -> submit -> `/thank-you/?session=thailand&cta=thailand_card&intent=`. |
| Email source | Passed | `source=email`, `medium=edm`, events included `source_entry`, `landing_page_view`. |
| Sales source | Passed | `source=sales_outreach`, `medium=direct_message`, events included `source_entry`, `landing_page_view`. |
| QR source | Passed | `source=offline_event`, `medium=qr_code`, events included `source_entry`, `landing_page_view`. |
| Direct source | Passed | `source=direct`, `medium=direct`, events included `landing_page_view`. |
| Bottom Register CTA | Passed | URL became `/register/?session=general&cta=bottom_register`; events included `click_register`. |
| Demo CTA | Passed | URL became `/register/?intent=demo&cta=demo_cta`; events included `click_demo`. Cache-busted register retest confirmed `session=""`, `intent=demo`. |

## 5. Entry Source Test Results

LinkedIn:

- Expected: `source=linkedin`, `medium=organic_social`, `campaign=ds100_webinar_2026`, `content=linkedin_launch_post`
- Result: Passed
- Events observed in session history: `source_entry`, `landing_page_view`, `click_register`, `register_page_view`, `form_start`, `form_submit_click`, `thank_you_view`

Email:

- Expected: `source=email`, `medium=edm`, `campaign=ds100_webinar_2026`, `content=email_invitation`
- Result: Passed
- Events observed: `source_entry`, `landing_page_view`

Sales:

- Expected: `source=sales_outreach`, `medium=direct_message`, `campaign=ds100_webinar_2026`, `content=sales_team`
- Result: Passed
- Events observed: `source_entry`, `landing_page_view`

QR:

- Expected: `source=offline_event`, `medium=qr_code`, `campaign=ds100_webinar_2026`, `content=booth_poster`
- Result: Passed
- Events observed: `source_entry`, `landing_page_view`

Direct:

- Expected: `source=direct`, `medium=direct`
- Result: Passed
- Events observed: `landing_page_view`

## 6. dataLayer Event Test Results

Required events:

- `source_entry`: Passed
- `landing_page_view`: Passed
- `click_register`: Passed
- `click_demo`: Passed
- `register_page_view`: Passed
- `form_start`: Passed
- `form_submit_click`: Passed
- `thank_you_view`: Passed

Recommended event parameters are present where applicable:

- `source`
- `medium`
- `campaign`
- `content`
- `cta`
- `session`
- `intent`
- `referrer`
- `page_location`

## 7. GTM Backend Configuration Status

Status: Not completed in this environment.

Reason:

- The current session could not connect to the Codex Chrome Extension, so I could not safely operate the user's existing logged-in Chrome/GTM session.
- I did not use any workaround to read cookies, passwords, tokens, or account data.

Required manual GTM configuration:

1. Create or confirm Google Tag:
   - Name: `Google Tag - GA4 - DS100`
   - Tag ID: `G-NL755SDF6F`
   - Trigger: `All Pages`
2. Create Data Layer Variables:
   - `DLV - source` -> `source`
   - `DLV - medium` -> `medium`
   - `DLV - campaign` -> `campaign`
   - `DLV - content` -> `content`
   - `DLV - cta` -> `cta`
   - `DLV - session` -> `session`
   - `DLV - intent` -> `intent`
   - `DLV - referrer` -> `referrer`
   - `DLV - page_location` -> `page_location`
3. Create Custom Event Trigger:
   - Name: `CE - DS100 All Tracking Events`
   - Event name: `source_entry|landing_page_view|click_register|click_demo|register_page_view|form_start|form_submit_click|thank_you_view`
   - Regex matching: enabled
4. Create GA4 Event Tag:
   - Name: `GA4 Event - DS100 All Events`
   - Event Name: `{{Event}}`
   - Parameters:
     - `source = {{DLV - source}}`
     - `medium = {{DLV - medium}}`
     - `campaign = {{DLV - campaign}}`
     - `content = {{DLV - content}}`
     - `cta = {{DLV - cta}}`
     - `session = {{DLV - session}}`
     - `intent = {{DLV - intent}}`
     - `referrer = {{DLV - referrer}}`
     - `page_location = {{DLV - page_location}}`
   - Trigger: `CE - DS100 All Tracking Events`

## 8. GTM Container Publish Status

Status: Not published by Codex.

Reason:

- GTM backend access was not available through the current Chrome automation connection.

Manual publish version:

- Version name: `DS100 GA4 tracking initial setup`
- Version description: `Added GA4 anonymous tracking for DS100 webinar source, CTA, register flow and thank-you conversion.`

## 9. GA4 DebugView Status

Status: Not verified by Codex.

Reason:

- GA4 backend access was not available through the current Chrome automation connection.
- GTM container was not configured or published by Codex in this environment.

Manual verification required:

- Open GA4 DebugView for the property whose Measurement ID is `G-NL755SDF6F`.
- Use GTM Preview / Tag Assistant with `http://localhost:8080/linkedin/`.
- Confirm these events arrive:
  - `source_entry`
  - `landing_page_view`
  - `click_register`
  - `click_demo`
  - `register_page_view`
  - `form_start`
  - `form_submit_click`
  - `thank_you_view`

## 10. GA4 Custom Dimensions Status

Status: Not created by Codex.

Manual setup required:

| Dimension name | Scope | Event parameter |
| --- | --- | --- |
| DS100 Source | Event | source |
| DS100 Medium | Event | medium |
| DS100 Campaign | Event | campaign |
| DS100 Content | Event | content |
| DS100 CTA | Event | cta |
| DS100 Session | Event | session |
| DS100 Intent | Event | intent |

## 11. Key Event Status

`thank_you_view` key event status: Not marked by Codex.

Manual next step:

- After `thank_you_view` appears in GA4 Events, mark it as a key event / conversion.
- If it does not appear immediately, wait for GA4 processing and check again.

## 12. PII Check

Result: Passed at code level.

Findings:

- Form fields exist in HTML: first name, last name, email, company, country, phone, job title, message.
- No form values are read into dataLayer payloads.
- No `FormData` usage exists.
- No payload key sends `name`, `first_name`, `last_name`, `email`, `phone`, or `company`.
- Anonymous session event history stores only tracking metadata and event names.

## 13. GitHub Pages Upload Checklist

Upload these items to the GitHub repository root:

- `index.html`
- `linkedin/`
- `email/`
- `sales/`
- `qr/`
- `register/`
- `thank-you/`
- `debug/`
- `README_TEST.md`
- `TRACKING_TEST_REPORT.md`
- `assets/` if it exists

Do not upload:

- `.DS_Store`
- `node_modules/`
- Playwright reports or traces
- browser caches
- screenshots or temporary test artifacts
- `.env`
- password, token, cookie, or private account files

Current local check found none of the blocked files after `.DS_Store` cleanup.

## 14. Links To Test After GitHub Upload

- https://streamaxbranding.github.io/DS100-Webinar/
- https://streamaxbranding.github.io/DS100-Webinar/linkedin/
- https://streamaxbranding.github.io/DS100-Webinar/email/
- https://streamaxbranding.github.io/DS100-Webinar/sales/
- https://streamaxbranding.github.io/DS100-Webinar/qr/
- https://streamaxbranding.github.io/DS100-Webinar/register/?session=thailand&cta=thailand_card
- https://streamaxbranding.github.io/DS100-Webinar/thank-you/
- https://streamaxbranding.github.io/DS100-Webinar/debug/

## 15. Unfinished Items / Manual Confirmation Required

Code status:

- Complete.

Local automated testing:

- Complete for page code and local browser flow.

Manual GTM work required:

- Create/confirm Google Tag.
- Create DLV variables.
- Create regex Custom Event Trigger.
- Create GA4 Event tag.
- Preview and verify tags fire.
- Publish GTM container.

Manual GA4 work required:

- Verify DebugView receives events.
- Create custom dimensions.
- Mark `thank_you_view` as key event after it appears.

Manual account confirmation required:

- Confirm GTM container `GTM-KCLRHBH9` is the intended container.
- Confirm GA4 web stream Measurement ID `G-NL755SDF6F` is the intended stream.
