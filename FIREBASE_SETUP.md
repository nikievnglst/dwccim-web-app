# Making the Setlist Builder sync across every device

By default, when someone builds or edits a Sunday's lineup in the app, it's
only saved on their own phone. This guide connects the app to a free
Firebase database so that when anyone saves a lineup, **everyone's app
updates automatically** — no refresh needed.

It takes about 10 minutes, and it's free for a group this size (Firebase's
free "Spark" tier covers far more traffic than a church worship team will
ever generate). You only need a Google account.

## 1. Create a Firebase project

1. Go to <https://console.firebase.google.com> and sign in.
2. Click **Add project**, give it a name (e.g. "dwccim-worship"), and finish
   the wizard (you can turn off Google Analytics — not needed here).

## 2. Register a Web App

1. On your new project's home page, click the **</>** (Web) icon to add a
   web app.
2. Give it any nickname. You don't need Firebase Hosting.
3. Firebase will show you a `firebaseConfig` object that looks like this:

   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "dwccim-worship.firebaseapp.com",
     databaseURL: "https://dwccim-worship-default-rtdb.firebaseio.com",
     projectId: "dwccim-worship",
     ...
   };
   ```

   Keep this page open — you'll copy 4 of these values in step 4.

## 3. Turn on the Realtime Database

1. In the left sidebar, go to **Build → Realtime Database**.
2. Click **Create Database**. Choose any location close to you.
3. Choose **Start in test mode** for now. This makes the database open for
   reading and writing to anyone who has the URL — that's fine for a small
   internal lineup list, but don't store anything sensitive in it. (If you
   want to lock it down further later, you can add rules and simple
   authentication — happy to help with that separately.)

## 4. Paste your config into the app

1. Open `index.html` in any text editor.
2. Near the very top of the `<script>` section, find this block:

   ```js
   const FIREBASE_CONFIG = {
     apiKey: "PASTE_YOUR_API_KEY_HERE",
     authDomain: "PASTE_YOUR_AUTH_DOMAIN_HERE",
     databaseURL: "PASTE_YOUR_DATABASE_URL_HERE",
     projectId: "PASTE_YOUR_PROJECT_ID_HERE",
   };
   ```

3. Replace each `"PASTE_..._HERE"` with the matching value from your
   `firebaseConfig` in step 2 (`apiKey`, `authDomain`, `databaseURL`,
   `projectId`). Leave everything else in the file as-is.
4. Save the file and re-upload/re-host it wherever the team already
   accesses the app from (or just re-share the file — it works the same
   whether it's opened locally or hosted somewhere).

## 5. Test it

1. Open the app on two devices (or two browser tabs).
2. On one, go to **Schedule → Build / Edit Lineup**, add a song, and tap
   **Save Lineup**.
3. The other device's lineup should update within a second or two, with no
   refresh needed. You'll also see "☁️ Connected — lineup changes sync to
   every device automatically." under the builder.

If it still says lineups are saved on-device only, double check the 4
values were pasted correctly (especially `databaseURL`, which is the one
most often mistyped).

## What this does and doesn't affect

Only the **Setlist Builder / lineups** sync through Firebase. Availability,
practice reminders, and saved chord/lyric notes still stay on each person's
own device, as before. If you'd like those to sync too, that's a
straightforward extension of the same setup — just ask.
