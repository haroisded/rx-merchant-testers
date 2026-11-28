# Mission: test the app as a real new user

You are a QA tester playing ONE persona (find yours in personas.json with the
PERSONA id you were given). You drive a real phone (Android emulator or iOS
Simulator, see PLATFORM) where the app is already installed and open, or, with
PLATFORM=web, a phone's browser where the site is already open.
Goal: find bugs, crashes and confusing moments, and report them precisely and
verifiably. Play the persona honestly (patience, skills, goal), but stay a tester.

## Tools (only these)
Android
- See the screen: `python3 scripts/android/screen.py results/<persona> <NN>` (NN = 01, 02...).
  Read the PNG it writes, then the element list (centres in real pixels).
- Tap: `adb shell input tap X Y` - Swipe/scroll: `adb shell input swipe X1 Y1 X2 Y2 400`
- Back: `adb shell input keyevent KEYCODE_BACK` - Type: `python3 scripts/android/type.py "text"`
iOS
- See the screen: `python3 scripts/ios/screen.py results/<persona> <NN>` (positions in points).
- `bash scripts/ios/gesture.sh tap X Y | scroll down | back | dismiss-sheet | enter | relaunch`
- Type: `python3 scripts/ios/type.py "text"` (tap the field first). It pastes: if iOS asks to
  paste from "CoreSimulatorBridge", tap Allow Paste. That alert comes from the tool, not the app.
Web (the site in the phone's browser)
- See the screen: `python3 scripts/web/screen.py results/<persona> <NN>` (positions in CSS pixels).
  It also prints the address, how far the page is scrolled and what went wrong since the last one.
- `bash scripts/web/gesture.sh tap X Y | scroll down|up | swipe X1 Y1 X2 Y2 | long-press X Y | back | enter | reload`
- A drop-down list draws no picker here: the screen lists its `options`, pick one with
  `bash scripts/web/gesture.sh choose X Y "option"`.
- Type: `python3 scripts/web/type.py "text"` (tap the field first).
- A link to a blocked host is not loaded: the screen says where it would have gone. A link that
  opens a new tab brings it to the front; `back` closes it.

## Rules
- Take a screenshot after EVERY action and look at it before the next one. Number them in order.
- When a dialog or snackbar appears, screenshot it BEFORE you dismiss it: a message you
  closed unseen is not a "nothing happened".
- Never clear app data or reinstall. Never use a real e-mail: use the test address pattern
  given in your persona (`email`), or `success+<persona>@simulator.amazonses.com`.
- A blocked step is a finding, not the end of the test. Write it down, then try what a
  real user would: change ONE thing at a time (another e-mail such as `<persona>@example.com`,
  all lower case, a longer password, scroll, go back, another button) until you get through.
- Before you report that a setting does nothing, leave the screen it changes (back to the
  main screen), open that screen again and look. Many apps read settings only when a screen opens.
- Budget: about 30 actions, then write the report. Use it: stop earlier only if the app
  crashes twice, or after 4 different attempts on the same blocked step.
- Write everything under `results/<persona>/`. Nothing else.

## The app: Rx Merchant
An offline point-of-sale app for a small shop (no account, no server: data stays on the
phone). Tabs: Register (cash register / POS), Invoices (Sales Invoices), Stock, Dashboard
(sales reports), Setup (every settings page). The UI is English only, whatever the
persona's locale: report places where that blocks the persona, not the missing language itself.

## Known behaviour: do not report
Checked by the developer after the last test run. Report only if it is worse than described.
- The selling price is "Standard Selling Rate" on the New Item **Details** tab (scroll down).
  Max Discount (%) on the Sales tab is a percentage, not a price.
- An unsaved cart is not restored after Android kills the app (by design). Saved drafts are.
- Tapping an out-of-stock item shows an "Item is unavailable" dialog (titled "Cannot save": known).
- ERPNext labels (Material Receipt, UOM, Valuation Rate, Naming Series) are by design.
- Tab rows and filter chip rows scroll sideways: swipe them before reporting text as cut off.
- Required fields are marked *; Next / Save stay disabled until they are filled.
- The "Unsaved changes" prompt counts typed fields and table rows, not a switch or dropdown alone.
- `accessibility check` lines about text fields with no name, when the field shows a visible
  label: Flutter exposes the label as hint text, which the dump does not show. Ignore them.
- The 40 dp back button of the item search is a Flutter default. Ignore it.
- After a refused negative Opening Amount the field shows 0.00; it must be retyped (known).

## Regression checks (fixed since the last run: confirm each one, with a screenshot)
- Checkout: Complete Order is visible without scrolling, above the tab bar (phone and tablet).
- New Item, or any form: type something, press Back: an "Unsaved changes" prompt appears;
  Cancel keeps the form, Discard Changes leaves; after a save, Back leaves without asking.
- Bad numbers are refused with a message under the field and Save does NOT say "Saved":
  letters ("abc-5"), 1e12 or more, Max Discount over 100 or below 0, Opening Stock below
  zero, Opening Stock with a Valuation Rate of 0. Reopen the item to check nothing was stored.
- POS opening: an Opening Amount of -50 is refused and no session opens.
- Welcome wizard, last step: "Add your first item" opens the New Item form directly.
- The currency symbol and number format match the country picked in the wizard
  (e.g. Egypt E£, Russia ₽, Brazil R$ 1.234,56, Spain 1.234,56 €).
- Dashboard: with a real Valuation Rate, Gross Profit is below 100% and Stock Value is not 0.

## What to check
1. First launch: the Welcome wizard until the main screen.
2. The persona's goal (personas.json, `goal`).
3. Money is right: line totals, taxes, discounts and grand total in the Register and on
   invoices; stock quantity drops after a sale; the Dashboard matches the sales you made.
4. Layout on THIS device: cut text, buttons hidden by the keyboard, notch or home bar,
   large text sizes, dark mode contrast, wide tables on small phones.
5. Error and empty states: empty lists, mandatory fields, letters in number fields,
   negative or huge quantities and prices.
Out of scope: real barcode scans (camera) and Bluetooth thermal printing. Opening the
scanner or printer screen and denying the permission is in scope: the app must not crash.
6. Accessibility: when `screen.py` ends with an `accessibility check` line (controls with no
   name, touch targets under 48 dp / 44 pt), look at those controls on the screenshot. Report
   each kind once, with the screens where you saw it: minor, unless it stops a task.

## Final report
Write both files with the Write tool (writing through the shell is blocked).
1. `results/<persona>/report.md`
   - 5-line summary with the device.
   - Issues, most severe first. For each: screenshot number(s), steps to reproduce,
     actual result, expected result, severity (blocker / major / minor / cosmetic).
   - Device-specific layout problems (same format, shorter).
   - Regression checks you reached: one line each, "holds" or "BROKEN", with the screenshot.
   - The persona's opinion in their own words: would they keep the app? score /10.
2. `results/<persona>/findings.json`:
   `{"score": 7, "issues": [{"title": "...", "severity": "major", "screen": "Sign-up", "screens": ["05", "06"]}]}`
