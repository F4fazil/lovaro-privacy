# Widget and analytics handoff — 11 October 2026

## Complete in source

- Consent preference `analytics_consent`: missing means unanswered and collection disabled. Allow and Not now appear together in the final notification onboarding screen. Settings → Privacy applies changes immediately. Declining closes the application event gate before native work and clears Analytics data.
- A single typed Riverpod analytics service owns all application `logEvent` calls. Only event IDs, deck IDs, counts, plan product IDs, theme IDs and fixed source/family/type values are sent. No questions, answers, names, anniversary dates or invite codes enter analytics. `is_pro` follows RevenueCat entitlement changes.
- Startup initializes Firebase, applies saved consent, sets `group.app.zavelo.lovaro`, sets the default allowance to 10 and fetches Remote Config without blocking the app.
- Widget sync debounces 500 ms, writes one versioned JSON payload and reloads kind `LovaroWidget`. Launch, resume, play history/streak, RevenueCat, theme, language and anniversary changes trigger sync. The payload contains seven date-seeded accessible questions. Free widgets use the default palette. The daily-question play-history record controls `streakAlive`.
- Swift AppIntent styles: question, days together and streak. Small, medium, rectangular and circular families are supported. Timelines contain now and the next six local midnights with `.atEnd`. Empty data and missing anniversary states use localized copy. Numbers use locale formatting; accented text supports tinted rendering.
- Native previews cover all styles/families, empty data, absent anniversary and a long German question.
- Widget links use `homeWidget`, support cold launch and stream clicks, defer through onboarding, and navigate Home → destination so Back works. Paywall links read RevenueCat only: free → paywall, Pro → theme picker. URI parameters never grant an entitlement.
- Settings includes the four illustrated Home Screen guide steps and Lock Screen instructions. Home offers a dismissible tip after three sessions; sessions include new launches and returns after 30 minutes away. Installed detection displays the added state and logs installation once, only with consent.
- All 27 added Flutter strings have translations in en, nl, de, fr, fr_CA, it, es and pt_BR. The Portuguese base fallback also has the keys. The Swift catalog has 15 keys in those eight languages.

## Done checklist

| Requested check | Result |
| --- | --- |
| `flutter analyze` clean; Runner simulator build | Passed: zero issues; Runner with embedded LovaroWidgetExtension builds in Debug for the simulator. |
| New strings in eight ARBs and Swift catalog | Passed: keys validated, localization guard passed, eight native `.lproj` bundles present in the built extension. |
| Daily widget URL with app running and killed | Commands executed successfully in both states. Destination verification is pending: iOS presents “Open in Lovaro?” and macOS denied assistive access to automate confirmation. |
| Paywall widget URL for free / Pro | Free URL command executed; confirmation blocks destination verification. Pro requires a real RevenueCat entitlement and remains unverified. No entitlement was fabricated. |
| Widget real question after launch / fresh empty state | Real version-1 payload saved in the simulator App Group with seven questions; every question belongs to a free deck for the observed free user. Rendering on an installed widget and fresh-install empty-state UI remain manual checks. Existing app data was preserved. |
| Tomorrow’s question without reopening app | Seven consecutive dates verified in the actual saved payload. Timeline schedules each local midnight. Actual clock-change/rendering check remains manual; the host clock was not changed. |
| Events visible in Firebase DebugView after Allow | App launched with `-FIRDebugEnabled`; Firebase console and UI consent interaction remain unverified. |
| No events after Not now | Source gate and default-off startup verified. Runtime decline/DebugView check remains manual because UI automation is unavailable. |

The full unit/widget test suite was not run. No purchases, trial eligibility, active Pro branch or App Store production signing were tested. The build used no RevenueCat key override; store-backed plan data still requires the project's real RevenueCat configuration.

## Manual checks and Xcode steps

1. Open `ios/Runner.xcworkspace`. Keep Runner ID `com.appsbyfazil.couplesapp`, extension target `LovaroWidgetExtension`, extension ID `com.appsbyfazil.couplesapp.LovaroWidget`, iOS 17.0 extension minimum, and App Group `group.app.zavelo.lovaro` on both targets. Choose your team and provision both development/distribution profiles. The extension is already attached and embedded.
2. Keep the `lovaro` URL scheme, `FlutterDeepLinkingEnabled = NO` and Analytics collection flag `NO`. Confirm the widget's string catalog remains in its resources. Local daily notifications do not need an APNs key or remote-push capability.
3. Run Lovaro with the real RevenueCat public key and offerings. Use an actual sandbox Pro entitlement to check the theme-picker link branch. Adding `isPro` to a URI must have no effect.
4. Confirm the iOS open prompt, then repeat while running and after terminating the app:

   ```sh
   xcrun simctl openurl booted "lovaro://daily?homeWidget"
   xcrun simctl openurl booted "lovaro://daily?start=1&homeWidget"
   xcrun simctl openurl booted "lovaro://paywall?variant=themes&homeWidget"
   ```

   Daily opens today’s preview; `start=1` enters its player. Back returns Home. Unknown hosts/schemes are ignored. Repeat with the real Pro entitlement.
5. Add each widget family, edit each style, and check tinted/clear rendering, StandBy and long German text. On a separate fresh install, add the widget before opening Lovaro and check its empty state. For days together, leave anniversary unset and check the localized empty state. Keep the existing simulator's personal data intact.
6. Verify the next local-midnight entry without reopening Lovaro. Open the app at least once every seven days to replenish future questions. A simulator status-bar time override changes only displayed time, so it does not verify a midnight transition.
7. In Xcode Runner scheme → Run → Arguments, enable `-FIRDebugEnabled`. On a fresh consent state, tap Not now, perform event-producing actions and confirm DebugView receives no new events. Enable Share anonymous usage data, repeat and check the typed events and `is_pro`; disable it again and confirm collection stops. Allow does not replay actions performed before consent.
8. Open the widget guide from Settings and the third-session tip. Add a widget and resume Lovaro; confirm the added state and one `widget_installed` event across future resumes with consent enabled.

## Created files

- `lib/core/analytics/analytics_consent.dart`
- `lib/core/analytics/analytics_consent.g.dart`
- `lib/core/analytics/analytics_consent_card.dart`
- `lib/core/analytics/analytics_service.dart`
- `lib/core/analytics/analytics_service.g.dart`
- `lib/features/widget/data/widget_sync_service.dart`
- `lib/features/widget/data/widget_sync_service.g.dart`
- `lib/features/widget/presentation/widget_link_handler.dart`
- `lib/features/widget/presentation/widget_guide_screen.dart`
- `lib/features/widget/presentation/daily_question_screen.dart`
- `lib/features/widget/presentation/theme_picker_screen.dart`
- `lib/l10n/app_fr_CA.arb` (Canadian French generated class is in the existing French localization file)
- `ios/LovaroWidget/Localizable.xcstrings`
- `docs/WIDGET_ANALYTICS.md`

## Updated areas

Startup/root lifecycle; app controller; Remote Config; RevenueCat service; Drift play-history query/repository; notifications; router; onboarding; Home; player; games; remote invitation sharing; Settings; shared paywall; existing ARBs/generated localization; Swift widget/intent/bundle; widget entitlements; Runner Info.plist and Xcode build phase order; analyzer build exclusions; mechanical analyzer fixes to existing source/check files; handoff inventory. Bundle IDs, Firebase configuration and the registered App Group were preserved.
