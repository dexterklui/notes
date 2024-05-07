---
date: 2024-04-24 (Wed)
---

# Firebase

## Overview

### Firebase projects

See
[Understanding Firebase projects](https://firebase.google.com/docs/projects/learn-more).

- Firebase projects contain registered apps and provisioned resources.
- Multiple Firebase Apps can be registered under the same project.
- Registered apps within a project share the same resources and services.
- Firebase Apps within a project share backends like Firebase Hosting,
  Authentication, Realtime Database, Cloud Firestore, Cloud Storage, and Cloud
  Functions.
- A Firebase project is built upon a **_Google Cloud project_**, with additional
  Firebase-specific configs and services enabled.

### Means of interacting with Firebase projects

- In [Firebase console](https://console.firebase.google.com) and Google Cloud
  console
- With
  [Firebase Management REST API](https://firebase.google.com/docs/reference/firebase-management/rest)
- With [Firebase CLI](https://firebase.google.com/docs/cli#management-commands)

### Firebase project identifiers

- _Project name_: User-defined and **internal-only** name for a project in the
  Firebase console, Google Cloud console, and the Firebase CLI.
- _Project number_: Google-assigned **globally unique canonical identifier** for
  the project. Use this when configuring integrations and making API calls.
- _Project ID_: User-defined unique identifier for the project across all of
  Firebase and Google Cloud. It's a convenience alias.
  - After Firebase provisions resources for a Firebase project, you cannot
    change its project ID. If you delete a project, the project ID is also
    deleted and can never be used again by any other project.

### Firebase config files and objects

When you register an app with a Firebase project, the Firebase console provides
a Firebase configuration file (Apple/Android apps) or a configuration object
(web apps) that you add directly to your local app directory.

- For Apple apps, you add a `GoogleService-Info.plist` configuration file.
- For Android apps, you add a `google-services.json` configuration file.
- For web apps, you add a Firebase configuration object.

Here are the **required, minimum "Firebase options"**:

- _API key_: a simple encrypted string used when calling certain APIs that don't
  need to access private user data (example value:
  `AIzaSyDOCAbC123dEf456GhI789jKl012-MnO`)
- _Project ID_: (example value: `myapp-project-123`)
- _Application ID_ ("_AppID_"): the unique identifier for the Firebase app
  across all of Firebase with a **platform-specific** format:
  - Firebase Apple apps: `GOOGLE_APP_ID` (example value:
    `1:1234567890:ios:321abc456def7890`)
  - Firebase Android apps: `mobilesdk_app_id` (example value:
    `1:1234567890:android:321abc456def7890`)
  - Firebase Web apps: `appId` (example value:
    `1:65211879909:web:3ae38ef1cdcb2e01fe5f0c`)

### General limits for Firebase projects, apps, and sites

See
[here](https://firebase.google.com/docs/projects/learn-more#general-limits-projects-apps-sites).

## Development Environments

See
[Overview of Environments](https://firebase.google.com/docs/projects/dev-workflows/overview-environments).

Key takeaway: using a separate Firebase project for each environment in your
development workflow.

### About environments

In software development, an **_environment_** is all the hardware and software
that are required to run an instance of an application or system of
applications.

Common environments:

- _Development_ (_dev_)
- _Test_ and _QA_
- _Staging_
- _Production_ (_prod_)

| Environments | Data                 | Used by         | Deploys                     | Lifespan                    | Purpose                          |
| ------------ | -------------------- | --------------- | --------------------------- | --------------------------- | -------------------------------- |
| Development  | Seed data            | Engineers       | Reloads constantly          | Recreated frequently        | Local testing during development |
| Test         | Seed data            | Machines        | Builds triggered by commits | Lives as long as a test run | Runs automated tests & QA        |
| Staging      | Anonymized user data | Entire dev team | Reloads per pull request    | Long lived, imitate prod    | Sandbox for a release            |
| Prod         | Private user data    | For end-users   | Deploys per launch/release  | Lives forever               | For customer to enjoy            |

The process of progressing a feature or release through these environments to
production is called a **_deployment pipeline_**.

Every app should have at least one pre-production environment that's isolated
from production data and resources.

### Data in each environments

#### Dev environment

Seeded with data that generally resembles the production data, but should never
contain any real users' data. It may also contain data that has caused bugs in
the past, like very long strings.

#### Test environment

Seeded with quality data that's generally representative of the production data,
along with data that represents corner cases and examples of data that have
caused bugs in the past

#### Staging environment

For realistic tests of how a release will work in production, you need a staging
environment that mimics production infrastructure as closely as possible. It's
common to have multiple staging instances if you need to test specific
integrations in isolation.

Here are common differences between staging and prod:

Staging may be missing some features or integrations that could cause side
effects. For example, staging may be set to not send email.

Staging may have anonymized data; the data can be fake, but it should be
realistic. Because staging is a place to safely debug problems, you might give
broader team access to staging data than production data. So, to protect user
privacy, you shouldn't use actual user data in staging.

#### Prod environment

In the Firebase console, we recommend tagging the Firebase project associated
with your production environment as a "production" environment type. This tag
can help remind you and your teammates that any changes could affect your
associated production apps and their data.

## Best Practice of Setting Up Firebase Projects

See
[General best practices for setting up Firebase projects](https://firebase.google.com/docs/projects/dev-workflows/general-best-practices).

- Ensure that all apps registered to a Firebase project are **platform variants
  of the same application** from an end-user perspective.
- If you have multiple build variants that **could share the same Firebase
  resources**, register the variants with the **same** Firebase project.
  - E.g. both free and paid version, or blog and web app in the same project
- If you have multiple build variants that are **based on release status**,
  register each variant with a specific Firebase project.
  - This is to prevent data pollution and overriding prod data.

Also, you should **avoid _multi-tenancy_**. That is, to avoid connecting several
logically independent apps (apps that don't share the same data and
configurations) to a single Firebase project.

## Security Guidelines

See
[General security guidelines for different development workflow environments](https://firebase.google.com/docs/projects/dev-workflows/general-security-guidelines).

## Using Firebase SDK in WebApp

1. Install Firebase npm package: `npm install firebase`
2. Initialize Firebase in your app and create a Firebase App object:

   ```typescript
   // @/lib/firebase.ts
   import { FirebaseApp, FirebaseOptions, initializeApp } from "firebase/app";

   // Use your config values. These values are supposed to be public, unlike
   // other API keys.
   const firebaseConfig: FirebaseOptions = {
     apiKey: "",
     authDomain: "",
     projectId: "",
     storageBucket: "",
     messagingSenderId: "",
     appId: "",
   };

   let firebaseApp: FirebaseApp;

   if (process.env.NODE_ENV === "development") {
     // In development mode, use a global variable so that the value
     // is preserved across module reloads caused by HMR (Hot Module Replacement).
     let globalWithFirebaseApp = global as typeof globalThis & {
       _firebaseApp?: FirebaseApp;
     };

     if (!globalWithFirebaseApp._firebaseApp) {
       globalWithFirebaseApp._firebaseApp = initializeApp(firebaseConfig);
     }

     firebaseApp = globalWithFirebaseApp._firebaseApp;
   } else {
     // In production mode, it's best to not use a global variable.
     firebaseApp = initializeApp(firebaseConfig);
   }

   export default firebaseApp;
   // Import the firebaseApp object from your main app
   ```

## Authentication

## Links

- [Firebase launch checklist](https://firebase.google.com/support/guides/launch-checklist)

## 🧭 Navigation

- [🔼 Back to top](#firebase)
- [📑 Notes Index](../../index.md)
