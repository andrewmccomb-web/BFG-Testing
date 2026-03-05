# iOS Fitness Hub — Product Scope (MVP)

## 1) Product vision
A personal iOS app (single-user oriented) that combines:
- strength + weight-loss programming,
- workout logging,
- daily weight tracking,
- calories/macros + hydration tracking,
- fatigue-aware workout generation based on available home gym equipment,
- motivational analytics and reminders.

Primary outcomes:
1. Better bodyweight trend over time.
2. Higher workout consistency (5–6 sessions/week target).
3. Better workout quality while reducing overuse (push/pull/legs fatigue balancing).

## 2) Confirmed user preferences
- Focus: **strength + weight loss**.
- Success metrics:
  - Daily/weekly bodyweight trend.
  - Workouts per week.
  - Subjective "better workouts" (captured via simple post-session rating).
- User scope: personal use; multi-user not required now.
- Auth: login is okay in phase 1.
- Logging detail: exercise, sets, reps, load (no RIR/rest-time requirement).
- Workout mode: templates + intelligent adaptation from previous workouts.
- Training frequency: 5 (possibly 6) days/week.
- Injury constraints: none currently.
- Recovery controls: required (prevent overloading same movement patterns).
- Cardio: integrated into fatigue logic.
- Nutrition: calories + macros.
- Barcode scanning: use API if possible; manual fallback accepted.
- Hydration: daily target + reminders.
- Body metrics: daily weight.
- Integrations desired: Apple Health + Apple Watch.
- Technical preference: SwiftUI, modern iOS, offline-first acceptable.
- UX priority: motivation + visual analytics + notifications.

## 3) Home gym equipment profile (for exercise library constraints)
- Adjustable bench: flat, 20°, 45° incline.
- Treadmill with incline.
- Elliptical / cross trainer.
- Exercise bike.
- Dumbbells:
  - pair 5 kg
  - pair 12.5 kg
  - pair 17.5 kg
- Kettlebell: 24 kg.

## 4) MVP feature set (Phase 1)

### A. Onboarding + profile
- Create account/login (email/password or Sign in with Apple).
- Input goals (strength + fat loss), target training days, baseline bodyweight.
- Confirm available equipment (pre-populated from list above, editable).

### B. Workout engine
- Template library (Full Body, Upper/Lower, Push-Pull-Legs, Conditioning).
- Daily generated session based on:
  - recent sessions,
  - muscle group stress,
  - push/pull/hinge/squat/core/cardio balance,
  - available equipment.
- Guardrails:
  - avoid heavy same-pattern repeat within 48h,
  - keep weekly pattern balance,
  - deload suggestion when fatigue signals accumulate.

### C. Workout logger
- Start session, add exercises, sets/reps/load, complete session.
- Save training volume per muscle group.
- Quick post-workout score (e.g., 1–5) for perceived quality.

### D. Nutrition + hydration
- Log meals with calories/macros.
- Barcode scan (if API integrated) with manual-entry fallback.
- Water tracker with daily goal and reminders.

### E. Metrics + motivation
- Dashboard:
  - streaks,
  - workouts/week,
  - weight trend (7-day average),
  - calories/macros adherence,
  - hydration completion.
- Motivational nudges and reminders:
  - planned workout,
  - water target,
  - daily weigh-in.

### F. Apple ecosystem integration
- Read/write key health metrics with Apple Health (workouts, body mass, active energy where available).
- Apple Watch support as Phase 1.5 if full watch app is too large for initial MVP.

## 5) Recommended technical architecture
- Client: SwiftUI (iOS 17+).
- Local persistence: SwiftData/Core Data for offline-first behavior.
- Sync/auth backend: Firebase or Supabase (simple personal scale).
- Notifications: UserNotifications.
- Health integration: HealthKit.
- Barcode scanning:
  - Camera capture with VisionKit/AVFoundation.
  - Food lookup via third-party nutrition API.

## 6) Data model (initial)
- `UserProfile`
- `EquipmentItem`
- `WorkoutTemplate`
- `WorkoutSession`
- `WorkoutExercise`
- `ExerciseSet`
- `MuscleLoadSnapshot` (daily/rolling fatigue indicators)
- `BodyWeightEntry`
- `NutritionEntry`
- `HydrationEntry`
- `ReminderPreference`

## 7) Fatigue logic (simple MVP algorithm)
For each muscle group and movement pattern:
- Maintain rolling 48h and 7-day load score.
- Exercise load score = sets × reps × relative intensity factor.
- Session generator rules:
  1. Filter out high-fatigue patterns in last 48h.
  2. Prioritize undertrained patterns over 7 days.
  3. Ensure push/pull/lower/core/cardio distribution target is met.
  4. Choose alternatives fitting available equipment.

## 8) Phase breakdown

### Phase 1 (2–4 weeks): core usable app
- Auth + onboarding
- Equipment profile
- Workout templates + logging
- Basic fatigue-aware session suggestions
- Daily weight + hydration + nutrition manual entry
- Dashboard + reminders

### Phase 2 (2–4 weeks): intelligence + integrations
- Barcode nutrition lookup
- Apple Health sync
- Improved adaptation and progression logic
- Stronger visual analytics

### Phase 3 (later)
- Apple Watch app/companion flows
- Advanced periodization
- Habit coaching and adaptive goals

## 9) Build-start checklist
1. Initialize SwiftUI iOS app (iOS 17 target).
2. Implement local models + persistence.
3. Build onboarding + equipment setup.
4. Ship workout logger + template execution.
5. Add fatigue scoring and next-session recommendation.
6. Add dashboard charts and weekly goals.
7. Add notifications.
8. Integrate backend auth/sync.
9. Add HealthKit and barcode integration.

## 10) Immediate next task
Create a clickable low-fidelity app flow (screens + navigation map) before coding:
- Onboarding
- Home dashboard
- Start workout
- Log set flow
- Nutrition/hydration logging
- Progress tab
- Settings/integrations

This will reduce rework and clarify interaction details before implementation.
