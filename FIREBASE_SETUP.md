# Firebase setup

The site works without Firebase: tests and the current test session use browser `localStorage`. Cloud synchronization requires a Firebase project and intentionally stays disabled until all four web-app configuration values are entered. There are no private credentials in the client.

## Create and configure a Firebase project

1. Create a new project in the [Firebase Console](https://console.firebase.google.com/).
2. In **Project settings > General**, register a Web app and copy its Firebase config values.
3. In **Authentication > Sign-in method**, enable **Anonymous**. Anonymous sign-in gives each browser a Firebase UID used to own its own records; students do not need to create accounts.
4. Create a **Cloud Firestore** database. In the Firestore **Rules** tab, replace the rules with the contents of `firestore.rules` and publish them. Do not use open read/write rules.
5. In `index.html`, fill the `FIREBASE_CONFIG` object with the Web app values:

   - `apiKey`
   - `authDomain`
   - `projectId`
   - `appId`

   These are Firebase client configuration values, not service-account credentials. Do not put a service-account JSON file or private key in this site.
6. Add the deployed website's domain under **Authentication > Settings > Authorized domains**, then deploy the static site.

With those values configured, the site anonymously signs in, synchronizes shared tests, and shows all public results. Tests and results have separate documents and include the creating account's `ownerUid`; the Firestore rules allow a student to remove only their own tests. Results are immutable after creation. Anyone signed in anonymously can create their own public nickname/result record, so scores are self-reported and nicknames/results are visible to all users of the board.

Restrict the Firebase API key to the website's domains in Google Cloud API key settings. Firebase API keys identify the project and are expected to be present in a browser; Firestore security rules and Authentication protect the data.
