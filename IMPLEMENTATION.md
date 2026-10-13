# Lovaro v1 handoff

Lovaro is an iOS Flutter app using bundle ID `com.appsbyfazil.couplesapp` and the existing `assets/app_icon/app_icon.png`. The Dart package remains `amora`; the visible brand is Lovaro everywhere. There is no account, authentication flow, custom server, or shared cloud database.

## Implemented

- ARB localization from the first new UI component: English, Dutch, German, French, Italian, Spanish, Brazilian Portuguese, and a Portuguese base fallback required by Flutter. Most earlier copy remains English placeholders; the widget and analytics consent additions have translations in all eight requested languages, including Canadian French. ICU plurals and number formatting are generated through `AppLocalizations`; dates and reminder times use the active locale.
- Feature folders for content, profile, onboarding, home, player, games, remote play, favourites, and settings. Shared theme, router, onboarding helpers, database, notifications, widget bridge, analytics, paywall, and Remote Config live in `lib/core`.
- Riverpod generated AsyncNotifier, GoRouter, Freezed/JSON entities, Drift SQLite storage, and shared preferences for profile settings.
- Eleven offline decks, each with 30 placeholder cards: 330 unique card IDs. All requested locales are represented per card and option. Missing region translations fall back to the base language, then English.
- Five onboarding steps: relationship stage, goal, names/optional anniversary, notification time and permission, dismissible onboarding paywall. Answers persist locally. Completing or closing the paywall finishes onboarding.
- Home with the supplied palette, embedded Newsreader/Nunito fonts, serif questions, two-column pastel deck tiles with a full-width Spicy tile, Pro badges, daily question, usage progress, streak, play tiles, favourites/settings navigation, and remote-play entry. Settings now includes a persistent theme selector for Automatic, Rose, Halloween and Christmas; manual selection changes the app palette, Home decorations, seasonal banner and daily question immediately. Selecting Halloween or Christmas also makes that seasonal deck available for preview outside its calendar window. Automatic mode follows the actual calendar.
- Rose default theme; Halloween October 1 through November 1; Christmas November 20 through December 26, inclusive. Header/banner SVG decorations are from the supplied assets. Themes change at local midnight and on app resume. Daily questions are deterministic by date; free users receive questions from free decks, while Pro users can receive seasonal questions.
- Deck recommendations prioritize long-distance stage or the chosen goal.
- Question player: horizontal swipe gestures, next question, alternating names, intensity chips, heart toggle, image sharing, free-user watermark, completion state, and admission before displaying a new card. Backward navigation during a session does not spend another card.
- Would You Rather: both partners choose privately, pass-phone screen, reveal match, and ten-round score. Couples Quiz: ten questions, one partner answers about themselves, the other guesses, then a score. Quiz requires Pro or an offline remote host flag.
- Favourites survive restarts; saved premium content still respects access rules.
- Offline remote codes carry protocol version, fixed deck index, seed, host Pro flag, and typo checksum. Both phones shuffle deterministically. No names or answers are transmitted. Each partner moves through cards manually during their call.
- Free allowance: ten new cards per local day across decks, configurable by optional `free_daily_cards` Remote Config. Daily question is outside that allowance. SQLite transactions prevent double-spending the last card. Seasonal decks expose their first three cards as samples. Pro removes the daily limit and the share watermark.
- Streak recording, one freeze replenished per calendar week, one missed day bridged per freeze, and milestone cards at 7/30/100. A stale streak is not displayed as current after an unbridgeable gap.
- Local daily notifications with distinct localized questions for the next 30 days; local timezone and DST support, permission request after the in-app reason, and daily-question navigation on notification tap, including cold launch.
- Purchase UI with yearly selected first, monthly and lifetime behind View all plans, eligibility-driven free-trial copy, prices from StoreKit/RevenueCat only, restore flow, management link, dismissible limit/deck/quiz paywalls, and a reminder based on the actual trial expiration returned by RevenueCat.
- AppIntent WidgetKit source for small, medium, rectangular and circular accessories. The App Group carries seven localized daily questions plus anniversary duration, streak, entitlement and theme. See WIDGET_ANALYTICS.md for verification status.
- Settings for language, names, anniversary, reminder enable/time, subscription management/restore, optional anonymous analytics, feedback/rating/privacy/terms links.
- Firebase initializes from the existing Lovaro configuration. Analytics collection and application event emission remain disabled until consent; a typed service controls all application events. Names, anniversary, questions, chosen answers and remote codes are never logged.
- Dynamic text sizing, scrollable screens, wrapping tile layouts, minimum 44-point actions, button/heart/share semantics and tooltips, dark palettes, no debug buttons in release. The theme selector is a normal Settings preference.
- AST localization guard and unit tests covering limits, SQLite concurrency/midnight reset, streak/freeze, shuffle, seasons, English fallback, content integrity, and remote code checksum.

## Work still needed outside the source

1. Replace placeholder questions and supply translations in ARB/content JSON.
2. Supply the Lovaro RevenueCat public iOS key and product configuration, privacy/terms/feedback/App Store URLs. The example config intentionally contains empty values. Plans remain unavailable until configured; no invented prices or fake purchase success are shown.
3. Provision the existing Runner and LovaroWidgetExtension targets for your Apple team with group.app.zavelo.lovaro. The target is attached and the simulator build succeeds.
4. Firebase is connected through firebase_options.dart and GoogleService-Info.plist. Analytics require consent; Remote Config uses a cached/default allowance without blocking startup.
5. Complete the remaining device, entitlement, midnight and Firebase DebugView checks listed in WIDGET_ANALYTICS.md. The full analyzer, localization guard and Runner simulator build have passed.

## Dependency note

Packages were resolved from pub.dev with the installed Dart 3.12 SDK. Current stable Freezed 4 requires Dart 3.13+, so the lockfile uses stable Freezed 3.2.5 and its compatible Riverpod/Drift/JSON generator stack. Runtime libraries without that conflict use current compatible stable versions. The generator warns that its analyzer supports Dart 3.11 grammar; the new source uses supported syntax. No Flutter SDK upgrade was performed. Transitive dependencies can contain prerelease versions owned by the upstream packages.

## Configuring purchases and links

Copy `config/lovaro.example.json` to your own local config and fill the public SDK key and URLs. Set RevenueCat entitlement `pro`, or set `REVENUECAT_ENTITLEMENT` to your chosen ID. Configure the current offering with **monthly**, **annual**, and **lifetime** packages only. Set the products in App Store Connect to the requested US base prices: monthly €4.99, annual €29.99, lifetime €59.99 in the intended Euro storefront. Attach a seven-day introductory free trial to annual only; monthly has no trial. Configure other storefronts in App Store Connect as appropriate. Lifetime is a non-consumable granting the same Pro entitlement. Price display always comes from the localized store product, and trial text is shown only when RevenueCat confirms eligibility.

For your manual run, pass `--dart-define-from-file=config/lovaro.local.json` to Flutter. Never copy Zavelo's app-specific product IDs, keys, or Firebase project into Lovaro. `zavelonails` was the reusable Flutter source: subscription-service pattern, nonblocking analytics wrapper, local onboarding-answer persistence, staged flow, and dismissible package/restore paywall were adapted for Lovaro. Its Firebase user binding was removed.

Firebase is already configured. Analytics collection starts only after Allow in onboarding or the Privacy settings switch; the analytics_consent preference is authoritative. Default `free_daily_cards` is 10, and cached/offline operation does not block play.

## Manual Xcode steps

1. Open `ios/Runner.xcworkspace`. In Runner → Signing & Capabilities, select your Apple team and confirm bundle ID `com.appsbyfazil.couplesapp`. Runner's minimum iOS deployment version is 15.0 for the selected Firebase plugins. Native packages use the starter project's Swift Package Manager integration; let Xcode resolve package dependencies during your manual build.
2. Both existing targets use App Group `group.app.zavelo.lovaro`; ensure your development and distribution profiles include it.
3. Keep the existing Widget Extension target `LovaroWidgetExtension`, bundle ID `com.appsbyfazil.couplesapp.LovaroWidget`, minimum iOS **17.0**. Its source is in `ios/LovaroWidget/`; it uses AppIntentConfiguration.
4. Keep the extension embedded in Runner. The embed phase runs before Flutter's scripts to avoid the Xcode dependency cycle. Do not add a duplicate widget target or widget source to Runner.
5. Keep `Localizable.xcstrings` in the widget's resources, `FlutterDeepLinkingEnabled = NO` and `FIREBASE_ANALYTICS_COLLECTION_ENABLED = NO` in Runner, and the existing `lovaro` URL scheme.
6. Notification permission is requested in the app. **Local notifications do not require Push Notifications or Background Modes capabilities**, an APNs key, or a backend. Test on a device and confirm iOS Settings → Lovaro → Notifications allows alerts. The plugin is registered by Flutter. There is no remote push setup in v1.
7. Confirm Runner’s `lovaro` URL scheme remains present for widget navigation. Add `ios/Localization/*/InfoPlist.strings` as localized resources if you want native display-name localization. They are exported from ARB, not independently translated.
8. Configure App Store Connect in-app purchases/subscription group and RevenueCat's Apple connection, offering, entitlement and introductory offer. Test eligible and ineligible trial accounts, cancellations, expired entitlements, purchase failure, restore, and offline cached access manually.
9. Supply signing profiles for Runner and the widget, with the same App Group. An Archive should include the widget extension. First launch Lovaro to populate widget data before adding it to the home screen.

## Localization workflow

Edit `lib/l10n/app_en.arb` first and keep all locale key sets aligned. Add `app_<locale>.arb` with a localized `languageName`; region-specific languages also require the base-language ARB. Run `flutter gen-l10n`. Supported locales and settings choices come from generated localizations, so adding a language requires no Dart change. Add translated text/options to the content JSON as desired; missing translations fall back to English. Run `python3 tool/export_ios_localizations.py` if native display-name resources should be updated.

The guard can be invoked manually with `dart run tool/check_localization.dart`; it rejects direct/interpolated string literals in Text/TextSpan, visible labels, and notification title/body arguments, plus mismatched ARB keys. It does not perform interprocedural data-flow analysis for strings hidden behind variables; review those for ARB or content origin. `flutter test` also includes that guard. The localization guard passed during the widget and analytics update; no unit or widget test suite was run.

## Practical limits

- Remote host Pro status is a trusted bit in a checksummed offline code. The checksum catches mistakes; it is not an entitlement signature. Secure cross-device entitlement verification needs an online authority. Both devices retain their own daily free allowance. Devices need matching v1 content to preserve order, and advance manually; there is no live answer or progress sync.
- iOS caps pending local notifications. A rolling 30-day schedule is refreshed on app open/resume and settings changes. It cannot deliver endlessly changing offline questions after months without reopening the app. Permission denial also prevents trial reminders.
- Widget timelines contain today plus the next six days. Open the app to replenish them. Small widgets have finite native space, so long questions and large accessibility text need device verification. The widget cannot display a meaningful snapshot before the app has populated shared data.
- Habit data and preferences are stored on this device. Export, cloud backup, multi-device syncing and account recovery are outside v1.
- The screenshots were implemented from the supplied reference; no simulator/browser screenshot or visual QA was run at your request.

## Widget and analytics update

See [WIDGET_ANALYTICS.md](WIDGET_ANALYTICS.md) for the current implementation, created files, verification checklist and remaining manual steps. This section supersedes earlier verification notes.

## Files

See [FILES.md](FILES.md) for the full source/generated file inventory and modified project files.

## Onboarding reference update

Five reference screens now precede the existing dismissible onboarding paywall: relationship stage (six choices), goals (six selectable tiles with persisted multi-selection), names and optional anniversary, reminder presets with a 24-hour wheel and 15-minute increments, and notification permission. The primary action stays at the bottom; content scrolls on smaller displays and with larger text. Selected goals drive Home recommendations. All added copy is in every ARB file. Reminder copy describes the offline daily notification; partner-answer alerts and an anniversary-only deck are not advertised because those features are not implemented. The paywall design awaits the next reference.

## Shared paywall reference update

Lovaro branding, a real question preview, emotional headline, and three primary benefits replace the placeholder paywall. Every variant uses the active ThemeData colours and the same plan/timeline/footer layout; Halloween and Christmas headlines are localized. Store-backed PaywallPrices supplies billing prices, annual monthly equivalents, eligible annual trial duration, and conservatively rounded annual savings against twelve monthly payments in the same currency. Monthly and lifetime never advertise a free trial. The footer changes immediately with the selected plan and remains fixed while content scrolls. Trial reminders are scheduled two days before a seven-day trial ends when notification permission is granted; timeline reminder copy is explicitly optional. Primary CTA and price text select a foreground with at least 4.5:1 contrast. There is no separate Pro theme in the current theme selector; the paywall also honours future ThemeData primary/onPrimary changes. Prices and introductory offers still require manual configuration in App Store Connect and RevenueCat. No store configuration or purchases were performed during this update.

## Paywall screenshot alignment

The paywall uses the updated reference: a tilted lavender Deep Talk card with a real deck question, a compact headline, three circular checkmark benefits, shorter timeline billing text, border-overlapping plan badges, a glowing fixed CTA, and inline legal links. Text and plan rows grow for larger text; content remains scrollable. Missing RevenueCat configuration now shows a setup message and a short disabled Continue button, instead of repeating the long connection error inside the CTA. A live annual price and eligibility are required to render the annual plan and trial timeline. Configure REVENUECAT_PUBLIC_KEY and the current offering before comparing those sections; selecting Rose in Settings matches the supplied reference palette. No invented prices or trial eligibility are substituted when StoreKit data is absent.

## RevenueCat catalog selection

The paywall selects offering `default` explicitly and accepts these package/product pairs:

| Package | Product | Trial advertised |
| --- | --- | --- |
| `$rc_annual` | `pro_yearly_couple` | Seven days, only if StoreKit reports that offer and RevenueCat confirms eligibility |
| `$rc_monthly` | `pro_monthly_couple` | None |
| `$rc_lifetime` | `pro_lifetime_couple` | None |

Entitlement defaults to `pro`. Billing prices, annual monthly equivalents and savings remain calculated from real StoreKit products. Set a seven-day free introductory offer on the yearly product in App Store Connect; monthly must have no free introductory offer. Apple applies eligible introductory offers during purchase, so hiding trial copy cannot remove a trial configured on a monthly product.

Startup loads the bundled `.env` with flutter_dotenv. RevenueCat uses `revenuecat_ios_api_key`, falling back to the `REVENUECAT_PUBLIC_KEY` Dart define if the environment value is absent or empty. The supplied .env key was checked without displaying it; .env is ignored by version control. Stop and rerun the app after adding the dependency/asset. Prices remain fetched from the store at runtime.
