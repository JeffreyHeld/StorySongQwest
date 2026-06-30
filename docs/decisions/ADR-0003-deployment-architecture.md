# ADR-0003: Deployment Architecture

## Status

Accepted

## Date

2026-06-30

## Context

`story-song-qwest` is the production runtime. It uses Next.js, Firebase, Cloudflare Workers, Firebase Functions, and exported Qwest bundles. Local development, production deployment, bundle publishing, and Firebase emulator workflows have separate operational needs.

The runtime README documents:

- `npm run dev` for local Next.js development.
- OpenNext Cloudflare integration through `initOpenNextCloudflareForDev()`.
- `npm run preview` for building with OpenNext and running locally in the Cloudflare Workers runtime.
- `npm run deploy` and `npm run upload` for Cloudflare Worker deployment workflows.
- Firebase emulators for local publish testing through `npm run dev:firebase`.
- Firebase Functions deployment through the Firebase CLI workflow.
- Cloudflare Worker runtime variables and build variables as separate configuration scopes.

## Decision

Use Cloudflare Workers through OpenNext as the deployed application runtime while retaining Firebase for auth, data, storage, functions, and emulator-backed local workflows.

Deployment ownership is split by runtime responsibility:

- Cloudflare Worker runtime hosts the live Next.js application.
- Cloudflare Worker runtime variables/secrets own server-side auth and email configuration.
- Cloudflare build variables own public Firebase and app environment configuration.
- Firebase Functions remain deployed and managed through the Firebase CLI workflow.
- Firebase emulators remain the local path for auth, Firestore, storage, and Functions testing.

Local publish testing must explicitly opt into Firebase emulators with `NEXT_PUBLIC_USE_FIREBASE_EMULATORS=true`; otherwise local admin publish actions call deployed Firebase Functions.

## Alternatives Considered

### Deploy the full app only through Firebase Hosting/Functions

- Pros: Single vendor for app hosting and backend services.
- Cons: Does not match the current OpenNext/Cloudflare Worker runtime direction and edge deployment needs.
- Rejected: The live app is documented as a Worker runtime.

### Move all backend behavior to Cloudflare immediately

- Pros: Single deployed runtime surface.
- Cons: Would require replacing existing Firebase auth, Firestore, storage, Functions, emulator workflows, and security rules.
- Rejected: Firebase remains the production data and Functions platform.

### Use local app fixes to test deployed publish behavior implicitly

- Pros: Faster local iteration if it worked.
- Cons: Without emulator opt-in, publish calls deployed Functions, so local source fixes do not affect new bundles.
- Rejected: The target must be explicit to avoid false confidence during publish testing.

## Consequences

- Deployment docs must distinguish app runtime, Cloudflare build variables, Cloudflare runtime variables/secrets, Firebase emulators, and Firebase Functions deployment.
- Operators must configure server-side auth and email settings in Cloudflare Worker runtime variables/secrets, not only build/Git integration variables.
- Local publish testing requires a separate Firebase emulator terminal and explicit environment flag.
- Firebase Functions changes require Firebase CLI deployment before deployed publish behavior changes.
- Runtime deployment and backend deployment remain coordinated but separate workflows.

