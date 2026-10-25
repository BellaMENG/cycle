# Cross-platform cycle app: build plan

Status: proposed technical direction, 24 October 2026. Product requirements: [PRD](PRD.md).

## Goal

Build a phone-first cycle tracking app for **iOS and Android**, with a responsive **web interface** using the same account and data. Users can sign in with Google. Start with useful free tracking; add Premium after the core flow works.

## Recommended stack

| Part | Choice | Reason |
| --- | --- | --- |
| App | **Expo + React Native + Expo Router** | One project can serve iOS, Android, and web. Share screens and logic; adjust layout where phones and desktop differ. [Expo Router docs](https://docs.expo.dev/router/introduction/) |
| Backend | **Supabase Postgres** | Store profiles, cycles, symptoms, and settings centrally so phone and web use the same data. |
| Sign-in | **Supabase Auth + Google** | Supported on Expo native and web. Use separate platform redirect handling and test each platform. [Supabase Expo guide](https://supabase.com/docs/guides/auth/quickstarts/with-expo-react-native-social-sign-in) |
| Access control | **Supabase Row Level Security** | Each signed-in user can read and change only their own health records. Client route guards improve navigation but do not replace database rules. [Supabase Auth docs](https://supabase.com/docs/guides/auth), [Expo protected routes](https://docs.expo.dev/router/advanced/authentication/) |

**iOS sign-in requirement:** If Google signs users into their primary app account, Apple's App Review guideline 4.8 generally requires another sign-in option with specified privacy protections. Plan for **Sign in with Apple on iOS** alongside Google. Google can remain the main requested option on Android and web. [Apple App Review guideline 4.8](https://developer.apple.com/app-store/review/guidelines/)

## First release

1. Google sign-in on web, Android, and iOS; Apple sign-in on iOS; sign-out and account deletion.
2. Onboarding: goal, last period date, typical cycle length, optional prior periods.
3. Calendar: logged periods, estimated next period, estimated fertile window, history.
4. Daily log: period flow, symptoms, notes; edit and delete entries.
5. Home screen: today's cycle status, next period estimate, quick log button.
6. Settings: reminders, privacy controls, export and deletion requests.
7. Web layout: same tracking and history on a wider screen, with keyboard and mouse support.

Fertility estimates are informational. Do not present them as contraception, diagnosis, or a guarantee of conception. Keep health data private by default, collect only needed fields, and make deletion straightforward.

## Premium, after the first release

- Cycle and symptom pattern summaries.
- Personalized educational insights tied to cycle phase and logs.
- Exportable clinician summary.
- Partner sharing with explicit consent and narrow data access.
- Expert-reviewed content library.

Keep period logging, calendar, history, and basic estimates free. Start with simple, explainable patterns before adding any condition checker or AI health assistant. See the [PRD](PRD.md) for release scope and acceptance criteria.

## Delivery order

1. Create Expo project and responsive navigation.
2. Set up Supabase schema, Row Level Security, and Google/Apple auth.
3. Build logging, calendar, and history on all three platforms.
4. Add estimates, reminders, data export, and account deletion.
5. Test sign-in, data isolation, and layout on iPhone, Android, and browser.
6. Design Premium entitlements and billing after the core app is stable.

## Decisions still needed

- Product name and visual style.
- First launch region and age range.
- Whether pregnancy and trying-to-conceive modes belong in the first release or later.
- Whether Premium uses subscriptions and which features it unlocks.
