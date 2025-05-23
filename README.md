# Recentify

Automatically sync your 25 most recently liked Spotify songs into a custom playlist, updated every hour&#x20;

— fully cloud-based and no coding required.

---

## 🔥 What It Does

Recentify is a lightweight automation built with [Pipedream](https://pipedream.com) that:

* Pulls your 25 most recently saved (liked) songs on Spotify
* Replaces the contents of a designated playlist with those 25 tracks
* Runs every hour in the cloud (no app or MacBook required)

---

## ✅ Why Use Recentify?

* No app to run
* No coding knowledge needed
* No maintenance required
* Works in the background using free tools (Spotify + Pipedream)

---

## 🚀 Setup Instructions (No Coding Experience Needed)

### 📌 What You’ll Need

* A Spotify account (Premium or Free)
* A free [Pipedream](https://pipedream.com) account
* 15 minutes to set it up once, forever

---

### STEP 1: Create a Playlist in Spotify

1. Open [Spotify in your browser](https://open.spotify.com/)
2. Click **Create Playlist** in the left sidebar
3. Name the playlist something like `Recent 25`
4. Click into the playlist, and copy the Playlist ID from the URL:

   * For example, if the URL is:

     ```
     https://open.spotify.com/playlist/5NfUjpYHGcG8LZGnAxQsKl
     ```

     The playlist ID is the part after `playlist/` → `5NfUjpYHGcG8LZGnAxQsKl`

Save this ID — you’ll paste it into your automation later.

---

### STEP 2: Set Up a Spotify Developer App

This gives Pipedream permission to securely access your liked songs.

1. Go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)
2. Click **Create an App**

   * Name: `Recentify`
   * Description: `A playlist updater`
   * Redirect URI: `https://pipedream.com/auth/oauth/callback`
3. Click **Save**
4. Copy the **Client ID** and **Client Secret** (you’ll use them inside Pipedream)

---

### STEP 3: Set Up the Workflow in Pipedream

1. Go to [Pipedream](https://pipedream.com) and create a free account (or log in)
2. Click **+ New Workflow** and name it `Recentify`
3. **Set the Trigger:**

   * Click **Add Trigger**
   * Choose **Scheduler**
   * Select "Run every hour"

---

### STEP 4: Get Your 25 Most Recent Saved Tracks

1. Click the `+` to add a new step under the trigger
2. Search and select **“Spotify”** > **Build any Spotify API request**
3. Set it up like this:

   * Method: `GET`
   * Endpoint: `https://api.spotify.com/v1/me/tracks?limit=25`
   * Auth: Connect your Spotify account using your Client ID & Secret from Step 2
   * Test the step and confirm it returns 25 tracks

---

### STEP 5: Extract Track URIs

1. Click `+` and choose **Run Node.js code**
2. Paste this code:

   ```javascript
   export default defineComponent({
     async run({ steps }) {
       return steps.custom_request.$return_value.items.map(i => i.track.uri);
     }
   });
   ```
3. This will return an array of Spotify track URIs you’ll send to the playlist

---

### STEP 6: Replace the Playlist Contents

1. Click `+` again and choose **Build any Spotify API request**
2. Set it up like this:

   * Method: `PUT`
   * Endpoint:

     ```
     https://api.spotify.com/v1/playlists/YOUR_PLAYLIST_ID/tracks
     ```

     Replace `YOUR_PLAYLIST_ID` with the one you copied from Spotify in Step 1
   * Auth: Your connected Spotify account
   * Content-Type: `application/json`
   * Under **Body**, choose **Edit Key-Value Pairs**:

     * Key: `uris`
     * Value: `{{steps.top_25_ids.$return_value}}`
3. Click **Test** to run it. You should see the playlist update immediately.

---

## ✅ Final Step: Deploy It!

* Click the blue **Deploy** button in the top right of Pipedream
* You’re done! 🎉
* Every hour (or whatever you set the interval), this will now auto-update your playlist

---

## 🧠 Notes & Tips

* You can disable the "Create Playlist" step (if included)
* You only need to create the playlist once and paste its ID into the flow
* Spotify sometimes delays new liked songs by 2–3 minutes
* If data doesn’t update: try reconnecting your Spotify account in Pipedream

---

## 💻 Future Plans: Mac App

We plan to turn this into a one-click Mac app so you can:

* Log in with Spotify
* Create a playlist
* Click "Enable syncing"
  All without touching the workflow builder.

Want to help build it? Let us know.

---

## 📖 License

MIT

---

## 🤝 Contributing

Ideas, pull requests, and feedback welcome.
Let’s build more tools that automate things that should be free.

Made by n0rth
