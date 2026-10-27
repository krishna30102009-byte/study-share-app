# StudyShare

StudyShare is a student-focused mobile app for sharing and discovering study materials. Students can upload assignments, notes, manuals, previous-year questions, question banks, and other resources, then share them publicly or inside private study groups.

## Table of contents

- [What the app does](#what-the-app-does)
- [Features](#features)
- [Screens](#screens)
- [Material types](#material-types)
- [Points](#points)
- [Privacy and access](#privacy-and-access)
- [Technology](#technology)
- [Project structure](#project-structure)
- [Requirements](#requirements)
- [Setup](#setup)
- [Run the app](#run-the-app)
- [Firebase deployment](#firebase-deployment)
- [Checks and release status](#checks-and-release-status)
- [Roadmap](#roadmap)

## What the app does

- Helps students share useful study files with other students.
- Lets students find resources by category, title, subject, branch, and year.
- Supports public sharing and member-only group sharing.
- Lets uploaders choose whether a resource should be highlighted in a trending banner.
- Provides profiles, saved materials, download history, and group study tools.

## Features

### Accounts and profiles

- Sign up and sign in with email and password.
- Keep a private account profile separate from the public student profile.
- View your upload count, points, and unique downloads.
- Open a student profile from the uploader name on a resource.
- Public student profiles show safe profile details and public uploads; private email and group-only uploads are not displayed.

### Upload study materials

- Upload PDF files and images with a title, type, subject, branch, and year.
- Select **Public** visibility or choose a group for **Group** visibility.
- Opt a resource into the trending banner during upload:
  - Public resources may appear in the Home trending banner.
  - Group resources may appear only in that group's trending banner.
  - Trending is optional; existing resources are not automatically opted in.
- See upload progress and provider errors.
- Get a warning before continuing when a likely duplicate title already exists in the same subject, branch, year, and visibility scope.
- Delete your own uploaded material from the app listing.

### Browse and search

- Browse recent public uploads from Home.
- Open category-specific lists for Assignments, Manuals, PYQs, Notes, Question Banks, and Other materials.
- Use Browse for universal search across public materials.
- Search resource titles and metadata, and filter by type, subject, branch, and year.
- Sort results by newest, most downloaded, or title.
- See recently viewed resources and previously used filters.
- View trending resources when students have opted materials in; otherwise see rotating study quotes.

### Resource details, feedback, and downloads

- Open a resource to see its title, type, subject, branch, year, file size, upload date, uploader, and visibility.
- Preview supported PDFs and images or open them with a compatible app.
- Download resources and keep a download history.
- Save resources to a personal Saved Materials list.
- Pin downloaded files for offline access, view local storage use, and separately manage offline copies and download history.
- Ask a question or leave a comment on a resource; the uploader can reply.
- Edit or delete your own comments.
- Mark another student's resource helpful or not helpful. Uploaders cannot vote on their own resource.
- View aggregate helpfulness counts; helpfulness does not change points.

### Private study groups

- Create a group with a name and description, or join through an invite code.
- View group members and the group's material feed.
- Upload resources directly to a selected group.
- Leave a group; group creators can delete their group.
- Creators can promote co-admins; group admins can manage members according to their role.
- Rotate or revoke invite codes.
- Post group announcements, pin group materials, and opt into announcement notifications/unread indicators.
- Group-only resources and trending materials stay scoped to active group members.

### Contributor points and leaderboard

- Earn **10 points** when an upload is saved successfully.
- A resource uploader earns **1 point per other student who downloads that resource**; repeat downloads by the same student do not create another reward.
- Browse a leaderboard of up to 20 contributors.

### Interface

- Dark, mobile-first study interface with icon-based navigation.
- Animated login and feedback/confirmation dialogs.
- Subtle press animation on Home category cards and search.
- Loading, empty, and retry states on data-backed screens.

## Screens

- **Login / Sign up** — Account access and registration.
- **Home** — Trending highlights, category cards, and recent public uploads.
- **Browse** — Universal public-resource search, filters, sort, and recent items.
- **Category** — Search and browse one material category at a time.
- **Upload** — Add a file, metadata, visibility, group, and optional trending selection.
- **Resource detail** — Preview, download, save, and study feedback.
- **Groups** — Create a group, join with a code, and view your groups.
- **Group detail** — Members, group-only materials, announcements, and pinned resources.
- **My Uploads** — View and delete your own accessible uploads.
- **My Downloads** — Open downloads, save offline, and manage cached files/history.
- **Saved Materials** — View bookmarked resources that remain accessible.
- **Student profile** — View another student's public profile and public uploads.
- **Leaderboard** — See top contributors.
- **Profile** — View your profile statistics, uploads, downloads, and app actions.

## Material types

- Assignment
- Manual
- PYQ (previous-year questions)
- Notes
- Question Bank
- Other

## Points

- Successful upload: **+10 points** for the uploader.
- First download of a resource by another student: **+1 point** for the uploader.
- A student's repeat download of the same resource is deduplicated.
- Helpful/not-helpful votes do not affect points.

## Privacy and access

- Public resources are intended to be discoverable by all signed-in users.
- Group resources are intended to be readable only by active members of that group.
- Public profiles use a separate email-free `publicProfiles` projection.
- Bookmarks, browse preferences, downloads, and votes are user-scoped.
- Firestore rules are the actual access-control boundary. Review and deploy `firestore.rules` and `firestore.indexes.json` for the Firebase project before inviting users.
- The moderator/report queue was removed from the app at the user's request.

## Technology

- React Native and Expo SDK 58
- Expo Router
- TypeScript
- Firebase Authentication
- Cloud Firestore
- Cloudinary for PDF and image file hosting
- React Native Animated for interface motion
- Lucide icons through `@react-native-vector-icons/lucide`

## Project structure

```text
study-share-app/
├── assets/                 App icons and splash assets
├── src/
│   ├── app/                Expo Router screens and routes
│   ├── components/         Shared UI components and dialogs
│   ├── constants/          Theme constants
│   ├── context/            Authentication context
│   ├── hooks/              Shared data hooks
│   └── lib/                Firebase, Cloudinary, groups, files, and data logic
├── firestore.rules         Firestore access-control rules
├── firestore.indexes.json  Firestore composite indexes
├── firebase.json           Firebase deployment configuration
├── app.json                Expo application configuration
├── package.json            Scripts and dependencies
├── PLAN.md                 Development phases and status
└── SPEC.md                 Implementation constraints and invariants
```

## Requirements

- Node.js and npm.
- A Firebase project with Email/Password Authentication enabled and a Cloud Firestore database created.
- A Cloudinary account with an unsigned upload preset configured for public file delivery and an appropriate file-size limit.
- Expo Go for quick device previews, or Android Studio/Xcode for native simulators and builds.

## Setup

1. Clone the repository and open its directory:

   ```bash
   git clone <repository-url>
   cd study-share-app
   ```

2. Install the locked dependencies:

   ```bash
   npm ci
   ```

3. Configure the Firebase web app settings in `src/lib/firebase.ts` for your Firebase project.

4. Configure your Cloudinary cloud name and unsigned upload preset in `src/lib/cloudinary.ts`.
   - The app uses an unsigned upload preset; do not add a Cloudinary API secret to the client app.
   - Configure file-size and delivery/access policies in Cloudinary.

5. Enable Email/Password sign-in in Firebase Authentication and create the Firestore database.

6. Check `.firebaserc` and confirm the selected Firebase project before deploying rules or indexes.

## Run the app

Start the Expo development server:

```bash
npx expo start
```

- Scan the displayed QR code with Expo Go.
- Press `a` to start/open Android when Android tooling is installed.
- Press `i` to start/open iOS when running on macOS with Xcode installed.
- Start the web version with `npx expo start --web`.

Useful project checks:

```bash
npm run lint
npx tsc --noEmit
```

## Firebase deployment

Install and authenticate the Firebase CLI, review the target project, then deploy the Firestore rules and indexes:

```bash
firebase login
firebase use
firebase deploy --only firestore:rules,firestore:indexes
```

Deploying changes affects the selected live Firebase project. Verify the project and review the rules before running the deploy command.

## Checks and release status

- The app's source and local Android/iOS JavaScript bundles have been checked during development.
- Local lint, TypeScript, and Expo dependency/configuration checks have passed in the development environment.
- A successful JavaScript bundle export does not prove that Firebase/Cloudinary live operations, native release builds, or every device flow work correctly.
- Before a public release, deploy and review Firebase rules/indexes, test sign-up and sign-in, public and group uploads, previews/downloads, bookmarks, group privacy, and owner deletion on real devices.
- Ask students to test sharing with separate accounts, including a non-member account checking that private group content is inaccessible.

## Roadmap

See [PLAN.md](PLAN.md) for the detailed phase plan and completion notes. Final live-service deployment, real-device checks, and friend testing remain release tasks.
