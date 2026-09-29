# Driver Record

A simple shift log for drivers, designed in the style of an iOS app. It records shifts, vehicles, trailers and stops, and works out normal hours and overtime for each week.

## Using it

Open `index.html` in any browser. You don't need to install or build anything, and you don't need a server.

To use it like an app on iPhone, host the file (GitHub Pages works: Settings → Pages → deploy from branch), open it in Safari, then tap **Share → Add to Home Screen**.

## Features

- **Week tab**: switch between weeks and see normal, overtime and total hours plus the number of shifts, with the list of shifts for that week.
- **History tab**: search by vehicle, trailer, location, notes or date, filter by a date range, and export the results to CSV.
- **Add/edit sheet**: tap **+** to add a shift, or tap any shift to edit or delete it. The sheet shows the shift's length and any overtime as you type. Overnight shifts are handled.
- Follows the device's light or dark mode.

## Overtime rules

- Any time over **9.25 hours** in a shift is overtime.
- Every shift after the **fifth** in a Monday–Sunday week counts entirely as overtime.

Both values are set at the top of the script (`OT_THRESHOLD`, `NORMAL_SHIFTS`).

## Data

Shifts are saved in the browser's `localStorage` under the key `proLogs`. The data stays on the device and is not synced anywhere. Export to CSV regularly if you want a backup.
