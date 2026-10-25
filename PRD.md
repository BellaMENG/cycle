# Product requirements: Cycle

**Status:** Draft · **Date:** 25 October 2026 · **Working name:** Cycle

## 1. Summary

Cycle is a private, phone-first app for logging periods and symptoms, understanding cycle history, and seeing estimated upcoming dates. It runs on iOS and Android and has a responsive web interface. One account keeps data in sync across platforms. Core tracking stays free. Premium helps users interpret their own data through patterns, educational insights, and a shareable summary.

## 2. Users and problems

**Initial audience:** adults who menstruate and want a simple record of their cycles. The first release focuses on cycle tracking; trying-to-conceive and pregnancy experiences come later.

Users need to:

- Record period dates and symptoms quickly, including corrections to past entries.
- See what they logged, what is estimated, and how certain an estimate is.
- Notice changes or repeated patterns without diagnosing themselves.
- Use the same account on phone and web and control sensitive data.

## 3. Goals and success measures

| Goal | Measure |
| --- | --- |
| Make first use easy | Share of new users who log a period during onboarding |
| Build a useful habit | Share of active users who log at least once in a later cycle |
| Make estimates understandable | Share of users who can identify an estimated date versus a logged date in usability testing |
| Build trust | Successful export and account deletion flows; zero known cross-user data access defects |
| Prove Premium value | Share of eligible users who open a pattern insight or report; trial-to-paid conversion after launch |

Measure these as product events without sending raw symptom text or cycle dates to analytics.

## 4. Release scope

| Release | Included |
| --- | --- |
| **Core, free** | Sign-in, onboarding, period and symptom logging, calendar, history, basic estimates, reminders, export, deletion, responsive web |
| **Premium 1** | Cycle and symptom patterns, personalized educational insights, shareable clinician summary, subscription entitlement across devices |
| **Later** | Trying-to-conceive mode, pregnancy mode, partner sharing, guided courses, conversational assistant, condition self-assessments |

Paid access must never block users from viewing, editing, exporting, or deleting their own logs.

## 5. Core user journey

1. User signs in with Google. On iOS, user can also choose Sign in with Apple.
2. User enters last period start date, period length, and optional earlier periods. They can skip unknown answers.
3. Home shows today's cycle position, next period estimate, and a prominent **Log** action.
4. User logs a period or symptom. Home and calendar update. Prior entries remain editable.
5. After enough history exists, user sees a short summary. Premium can open deeper patterns and educational guidance.
6. User can switch between phone and web, review history, manage reminders, export data, or delete their account.

## 6. Functional requirements

### 6.1 Account and onboarding

- **AUTH-01:** Google sign-in works on iOS, Android, and web. Sign in with Apple is also offered on iOS.
- **AUTH-02:** Returning users retain access to the same records across platforms. Sign-out works on each device.
- **AUTH-03:** New users can skip optional cycle questions and fill them in later.
- **AUTH-04:** The app requests only account information needed to sign in. It does not request access to Google health, contacts, or calendar data.
- **AUTH-05:** Account deletion is available inside the app and removes the user's health records and account data through a documented process.

### 6.2 Logging and history

- **LOG-01:** Users can add, edit, and delete period days, flow level, symptoms, and private notes for any date.
- **LOG-02:** A calendar visually separates logged dates from estimated dates. A list view offers an accessible alternative.
- **LOG-03:** Home provides one-tap access to today's log and the next estimated period.
- **LOG-04:** Users can view past cycle lengths and period lengths. Correcting old dates recalculates summaries and future estimates.
- **LOG-05:** Data entered on phone or web appears on the other platform after sync. Show clear retry feedback when a save fails.
- **LOG-06:** Users can export their logs and history in a portable format.

### 6.3 Estimates and reminders

- **EST-01:** Show an estimated next period after at least one logged period and enough information to calculate a date. If history is thin, label the estimate as early or low confidence.
- **EST-02:** As more completed cycles are logged, update estimates using the user's own history. Never silently present an estimate as an observed event.
- **EST-03:** Fertile-window estimates, if shown, carry a clear informational label and must not be marketed as birth control or a guarantee of conception.
- **EST-04:** Users can opt into period reminders and change or disable them. Notification previews avoid sensitive detail by default.

### 6.4 Premium 1

- **PREM-01 — Cycle patterns:** Show cycle-length and period-length trends using the user's logged history. State how many cycles support each pattern.
- **PREM-02 — Symptom patterns:** Highlight a repeated symptom only when enough logs exist; otherwise explain what additional data is needed. The user can open the underlying logged dates.
- **PREM-03 — Personal insights:** Match short, expert-reviewed educational content to cycle stage and logged symptoms. Separate general education from personalized observations.
- **PREM-04 — Clinician summary:** Generate a dated, readable report of the selected history window, including cycle dates and user-selected symptoms. User reviews it before sharing or downloading.
- **PREM-05 — Entitlement:** A paid account sees Premium features on iOS, Android, and web. If a subscription expires, existing logs remain accessible.
- **PREM-06 — Paywall:** Explain the exact paid benefits, price, billing period, trial terms, and cancellation path before purchase. Users can dismiss offers and keep using Core.

### 6.5 Later experiences

- **Trying to conceive:** Add ovulation test and basal-temperature logs, fertility-focused views, and educational content. Keep estimates clearly labeled.
- **Pregnancy:** Let users set due date or gestational age; show week-by-week progress and relevant content. Define its free/Premium split before implementation.
- **Partner sharing:** Require explicit, revocable consent; show a preview of exactly what a partner can see. Private notes and individual symptoms stay hidden by default.
- **Guided courses:** Offer staged educational sessions with visible progress and a resume point.
- **Assistant and condition checkers:** Require separate clinical, safety, privacy, and regional review before scope or launch claims are set.

## 7. Platform and design requirements

- Use one Expo/React Native project with shared business logic and responsive layouts for iOS, Android, and web.
- Optimize common logging tasks for one hand on phones. Use wider calendar and history layouts on desktop web.
- Support screen readers, clear labels, sufficient contrast, keyboard navigation on web, and readable text sizes.
- Keep core navigation consistent: **Home, Calendar, Log, Insights, Settings**. Hide or adapt destinations that are not in the current release.
- Show loading, empty, offline, and error states. Never imply a save succeeded when it did not.

## 8. Data and privacy requirements

- Store account identity separately from cycle and symptom records. Restrict every health record to its owner through database access rules.
- Encrypt data in transit and at rest through the chosen service; keep secrets out of the client app.
- Collect only fields needed for active features. Do not use health logs for advertising targeting.
- Treat partner access and report sharing as separate, explicit user actions.
- Offer in-app export and deletion. Define retention, backups, and deletion timing before launch.
- Use aggregate product analytics; avoid sending raw health entries to analytics or error logs.

## 9. Acceptance criteria

**Core release is ready when:** a new user can sign in, log and correct a period, see clearly labeled history and estimates, receive an opted-in reminder, export records, and delete the account on a real iPhone, Android phone, and browser. Records sync across platforms. A signed-in user cannot access another user's records by changing a URL or request.

**Premium 1 is ready when:** a user with enough history can open a pattern and inspect its source entries, read relevant content, review and export a clinician summary, purchase or restore access on supported platforms, and retain all logs after canceling. Users without enough data see a clear empty state. Subscription terms are visible before purchase.

## 10. Out of scope for the first release

- Diagnosis, treatment advice, medical-device claims, or contraception claims.
- Social community and anonymous posting.
- Partner access, pregnancy tracking, and trying-to-conceive mode.
- AI-generated health advice or condition-risk scoring.

## 11. Open decisions

- Final product name, brand, and visual style.
- Launch countries, supported languages, and exact adult age policy.
- Symptom list, content review owner, and wording for estimate confidence.
- Subscription prices, billing implementation, and which insight previews remain free.
- Export format and retention period after account deletion.
