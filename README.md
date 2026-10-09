# Personal Dashboard

A single-file HTML dashboard (`index.html`). It doesn't need a build step or a server-side backend.

## What's on it

| Card | Source |
| --- | --- |
| **Clock, date & greeting** | Your browser |
| **Weather**: now, next 12 hours, 6-day forecast, UV, air quality, sunrise/sunset | [Open-Meteo](https://open-meteo.com) (free, no key) + your GPS, or a city you choose |
| **Calendar**: next 7 days from all calendars you have visible, with "Join" links for Meet | Google Calendar API (read-only) |
| **Shopping list** | Google Tasks list (synced), **or** a local list you paste from Google Keep (see below) |
| **Today at a glance**: next-event countdown, hours booked, umbrella/sunscreen/jacket/AQI tips, sunset, reminders | Computed from the above |
| **Top 3 priorities**: reset every day | Saved in your browser |
| **Daily habits with 🔥 streaks + water tracker** | Saved in your browser |
| **Time progress**: day / week / month / year | Your browser |
| **Focus (Pomodoro) timer** with chime and notification, plus a daily session count | Your browser |
| **Quick notes** | Saved in your browser |

Click **⚙ Settings** to set your name, location, °F/°C, 12/24-hour clock, theme, habits, water goal and Google connection. The **🌓 Auto** button in the header cycles the theme between Auto (match your device), Light and Dark.

## Running it

Google sign-in won't work from a `file://` URL, so serve the folder over HTTP:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

You can also host it on GitHub Pages (Settings → Pages → deploy from this branch). Then add that URL as an authorized origin in step 4 below.

## Connecting Google Calendar & Tasks (one-time, ~5 minutes)

1. Go to <https://console.cloud.google.com/> and create a project, for example "Dashboard".
2. **APIs & Services → Library**: enable **Google Calendar API** and **Google Tasks API**.
3. **APIs & Services → OAuth consent screen**: choose *External*, fill in the app name and your email, then add yourself under **Test users**. You can leave it in "Testing" mode.
4. **APIs & Services → Credentials → Create credentials → OAuth client ID**:
   - Application type: **Web application**
   - Authorized JavaScript origins: `http://localhost:8000` (plus your GitHub Pages URL if you use one)
5. Copy the **Client ID**, paste it into the dashboard's Settings, save, then click **Connect Google**.

The access token stays in your browser's session storage and is never sent anywhere except Google's APIs. Google issues tokens for one hour. After that, click **Reconnect Google** (it won't ask for consent again).

## About Google Keep

Google doesn't offer a Keep API for personal Google accounts. The official Keep API is only available to Workspace enterprise admins. The dashboard gives you two workarounds:

- **Google Tasks (recommended, fully synced).** In Settings, set *Shopping list source* to *Google Tasks list*. The dashboard uses (or creates) a Tasks list named `Shopping`. Tasks appears in the side panel of Keep, Gmail and Calendar, and in the Google Tasks phone app. Checking items off works both ways.
- **Paste from Keep.** With the *This browser* source, click **Import**, then paste the text of your Keep list. **Copy** puts your unchecked items on the clipboard so you can paste them back into Keep.

## Privacy

Everything except the Google and weather API calls stays in your browser's `localStorage`. To find your location by GPS, the page sends your rounded coordinates to Open-Meteo and to BigDataCloud, which turns them into a city name. If you'd rather not, type a city into Settings.
