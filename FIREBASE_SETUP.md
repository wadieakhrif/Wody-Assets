# Firebase setup

1. In Firebase Console, enable **Authentication > Sign-in method > Google**.
2. Add the local development domain (for example `localhost`) and the production domain under **Authentication > Settings > Authorized domains**. Use a local HTTP server; `file://` pages are not a supported production origin for Google popup auth.
3. Copy the complete Web API key from **Project settings > Your apps > Web app** into `firebaseConfig.apiKey`. The short value previously present in the project is incomplete and causes `auth/api-key-not-valid`.
4. Deploy `firestore.rules` to the `wody-assets` project.
5. Sign in once with `wadieakhrif2@gmail.com`.
6. In Firestore, create this document manually:

`users/<the-admin-google-uid>`

```json
{
  "uid": "<the-admin-google-uid>",
  "role": "admin",
  "displayName": "Wadie Akhrif",
  "email": "wadieakhrif2@gmail.com"
}
```

The client never grants itself admin access. The Firestore Rules check the role document on every protected read/write. To add another administrator later, create another `users/<uid>` document with `role: "admin"` using the Firebase Console or a trusted server.

Reports are stored first in Firestore under `reports`. Email delivery is intentionally not performed from the browser. Add a trusted Cloud Function or other server-side mailer later if email notifications are required.

For local testing, serve this folder over HTTP, for example with VS Code Live Server, rather than opening `index.html` directly. The current IndexedDB asset editor remains available; Firebase stores authentication, user profiles, likes, favorites, ratings, comments, and reports.
