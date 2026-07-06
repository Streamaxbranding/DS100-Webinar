DS100 GTM + GA4 tracking test package

Purpose

This is a GitHub Pages static landing page test for the DS100 webinar funnel. It does not connect to Salesforce, does not save lead records, and does not send name, email, phone, company, or other PII to GA4. It only sends anonymous attribution and funnel events through GTM to GA4.

IDs

- GTM Container ID: GTM-KCLRHBH9
- GA4 Measurement ID: G-NL755SDF6F

Included files

- index.html: main landing page with GTM and dataLayer tracking.
- linkedin/: clean LinkedIn entry page.
- email/: clean email entry page.
- sales/: clean sales outreach entry page.
- qr/: clean QR/offline entry page.
- register/: test registration page. The form has no backend and does not send form field values to GA4.
- thank-you/: conversion thank-you page.
- debug/: local tracking state, current dataLayer, and session event-history helper page.
- README_TEST.md: this test, GTM, GA4, and GitHub Pages guide.
- assets/: upload this folder too if it exists in the project.

Clean entry links

Each entry page writes anonymous attribution to localStorage, pushes source_entry to dataLayer, then redirects to the main landing page.

- /linkedin/
  - source = linkedin
  - medium = organic_social
  - campaign = ds100_webinar_2026
  - content = linkedin_launch_post
- /email/
  - source = email
  - medium = edm
  - campaign = ds100_webinar_2026
  - content = email_invitation
- /sales/
  - source = sales_outreach
  - medium = direct_message
  - campaign = ds100_webinar_2026
  - content = sales_team
- /qr/
  - source = offline_event
  - medium = qr_code
  - campaign = ds100_webinar_2026
  - content = booth_poster

Tracked dataLayer events

- source_entry
- landing_page_view
- click_register
- click_demo
- register_page_view
- form_start
- form_submit_click
- thank_you_view

Recommended GA4 event parameters

Configure the GA4 Event Tags in GTM to pass these dataLayer variables when available:

- source
- medium
- campaign
- content
- cta
- session
- intent
- referrer
- page_location

GTM setup

1. In GTM, create or confirm the Google Tag / GA4 Configuration tag.
2. Set Measurement ID to G-NL755SDF6F.
3. Set the trigger to All Pages.
4. Create Data Layer Variables for these keys:
   - source
   - medium
   - campaign
   - content
   - cta
   - session
   - intent
   - referrer
   - page_location
5. Create one Custom Event trigger for each event:
   - source_entry
   - landing_page_view
   - click_register
   - click_demo
   - register_page_view
   - form_start
   - form_submit_click
   - thank_you_view
6. Create one GA4 Event tag for each Custom Event trigger. Use the same event name as the dataLayer event.
7. In each GA4 Event tag, add the recommended event parameters listed above.
8. Publish the GTM container after preview testing succeeds.

GA4 setup and checks

1. In GA4 Admin, confirm the web stream Measurement ID is G-NL755SDF6F.
2. Use DebugView while GTM Preview / Tag Assistant is connected.
3. Confirm these events appear:
   - source_entry
   - landing_page_view
   - click_register
   - click_demo
   - register_page_view
   - form_start
   - form_submit_click
   - thank_you_view
4. Mark thank_you_view as a key event / conversion after the event appears in GA4.
5. Check that no form field values such as name, email, phone, or company appear in event parameters.

Local testing

Option A: open the file directly.

1. Open DS100_GA4_GTM_test/index.html in a browser.
2. Some browser security behavior can differ for localStorage and referrer on file:// URLs, so use Option B if you want cleaner testing.

Option B: run a local static server from the project folder.

1. In Terminal:

   python3 -m http.server 8080

2. Open:

   http://localhost:8080/

3. Test the LinkedIn entry page:

   http://localhost:8080/linkedin/

4. It should save source=linkedin in localStorage and redirect to:

   http://localhost:8080/

5. In the browser Console, run:

   DS100TrackingDebug()

6. Confirm attribution.source is linkedin and dataLayer contains landing_page_view.
7. To inspect the full dataLayer manually, run:

   window.dataLayer

8. Click each CTA and confirm the expected navigation:
   - Thailand Register Now: register/?session=thailand&cta=thailand_card
   - Vietnam Register Now: register/?session=vietnam&cta=vietnam_card
   - Indonesia Register Now: register/?session=indonesia&cta=indonesia_card
   - Register for the Webinar: register/?session=general&cta=bottom_register
   - Get the Demo: register/?intent=demo&cta=demo_cta
9. On the register page, confirm register_page_view appears in dataLayer.
10. Start typing in the form and confirm form_start appears once.
11. Submit the form and confirm form_submit_click appears, then the page redirects to ../thank-you/.
12. On the thank-you page, confirm thank_you_view appears.
13. Open http://localhost:8080/debug/ and confirm it shows attribution, dataLayer events, and session event history.
14. Use Clear Tracking Storage before testing another source.

GTM Preview / Tag Assistant testing

1. Open GTM and click Preview.
2. Enter the local test URL or the GitHub Pages URL.
3. Connect Tag Assistant.
4. Visit /linkedin/ and follow the automatic redirect.
5. In Tag Assistant, check that source_entry fires on the entry page and landing_page_view fires on the landing page.
6. Click each CTA and confirm click_register or click_demo fires before navigation.
7. On /register/, confirm register_page_view, form_start, and form_submit_click fire.
8. On /thank-you/, confirm thank_you_view fires.
9. In GA4 DebugView, confirm the same events arrive with the expected source, medium, campaign, content, cta, session, intent, referrer, and page_location parameters.

GitHub Pages upload checklist

Do not upload only index.html. Upload the whole static package:

- index.html
- linkedin/
- email/
- sales/
- qr/
- register/
- thank-you/
- debug/
- README_TEST.md
- TRACKING_TEST_REPORT.md
- assets/ if it exists

After uploading, test these links:

- https://streamaxbranding.github.io/DS100-Webinar/
- https://streamaxbranding.github.io/DS100-Webinar/linkedin/
- https://streamaxbranding.github.io/DS100-Webinar/email/
- https://streamaxbranding.github.io/DS100-Webinar/sales/
- https://streamaxbranding.github.io/DS100-Webinar/qr/
- https://streamaxbranding.github.io/DS100-Webinar/register/?session=thailand&cta=thailand_card
- https://streamaxbranding.github.io/DS100-Webinar/thank-you/
- https://streamaxbranding.github.io/DS100-Webinar/debug/

Expected acceptance checks

- All pages load GTM-KCLRHBH9 in both the head GTM script and noscript iframe.
- /linkedin/ records source=linkedin and redirects to the landing page.
- Landing page load pushes landing_page_view.
- Thailand, Vietnam, and Indonesia Register Now buttons push click_register and navigate to /register/.
- Register for the Webinar pushes click_register with cta=bottom_register and session=general.
- Get the Demo pushes click_demo and navigates to /register/.
- Register page pushes register_page_view, form_start, and form_submit_click.
- Thank-you page pushes thank_you_view.
- Debug page shows attribution, current dataLayer events, and session event history.
- DS100TrackingDebug() returns the current attribution, dataLayer state, URL, and referrer on index.html, register/index.html, thank-you/index.html, and debug/index.html.
- No PII fields are sent to GA4.
