# File inventory

Deliverable source, configuration, assets and generated Dart files. Package downloads, Flutter ephemeral native packages, plugin metadata, and local code-generation caches are omitted. Existing supplied SVG art and app icons are reused.

## Created (81)

- `assets/content/decks.json`
- `assets/ui/tab-home-active.svg`
- `assets/ui/stage-engaged.svg`
- `assets/ui/stage-married.svg`
- `assets/fonts/Newsreader-OFL.txt`
- `assets/fonts/Newsreader.ttf`
- `assets/fonts/Nunito-OFL.txt`
- `assets/fonts/Nunito.ttf`
- `config/lovaro.example.json`
- `docs/FILES.md`
- `docs/IMPLEMENTATION.md`
- `ios/Localization/de.lproj/InfoPlist.strings`
- `ios/Localization/en.lproj/InfoPlist.strings`
- `ios/Localization/es.lproj/InfoPlist.strings`
- `ios/Localization/fr.lproj/InfoPlist.strings`
- `ios/Localization/it.lproj/InfoPlist.strings`
- `ios/Localization/nl.lproj/InfoPlist.strings`
- `ios/Localization/pt-BR.lproj/InfoPlist.strings`
- `ios/Localization/pt.lproj/InfoPlist.strings`
- `ios/LovaroWidget/LovaroWidget.entitlements`
- `ios/LovaroWidget/LovaroWidget.swift`
- `ios/Runner/Runner.entitlements`
- `l10n.yaml`
- `lib/core/analytics/app_analytics.dart`
- `lib/core/app_controller.dart`
- `lib/core/app_controller.g.dart`
- `lib/core/database/app_database.dart`
- `lib/core/database/app_database.g.dart`
- `lib/core/l10n/l10n.dart`
- `lib/core/notifications/notification_service.dart`
- `lib/core/onboarding/onboarding_flow.dart`
- `lib/core/paywall/paywall_screen.dart`
- `lib/core/paywall/paywall_prices.dart`
- `lib/core/paywall/subscription_service.dart`
- `lib/core/remote_config/app_remote_config.dart`
- `lib/core/router/app_router.dart`
- `lib/core/theme/app_theme.dart`
- `lib/core/widget/widget_service.dart`
- `lib/core/widgets/common.dart`
- `lib/features/content/data/asset_content_repository.dart`
- `lib/features/content/domain/content.dart`
- `lib/features/content/domain/content.freezed.dart`
- `lib/features/content/domain/content.g.dart`
- `lib/features/content/domain/rules.dart`
- `lib/features/favorites/data/drift_favorites_repository.dart`
- `lib/features/favorites/domain/favorites_repository.dart`
- `lib/features/favorites/presentation/favorites_screen.dart`
- `lib/features/games/presentation/game_screen.dart`
- `lib/features/home/presentation/home_screen.dart`
- `lib/features/onboarding/presentation/onboarding_screen.dart`
- `lib/features/player/data/drift_play_repository.dart`
- `lib/features/player/domain/play_repository.dart`
- `lib/features/player/presentation/player_screen.dart`
- `lib/features/profile/data/profile_store.dart`
- `lib/features/profile/domain/profile.dart`
- `lib/features/profile/domain/profile.freezed.dart`
- `lib/features/profile/domain/profile.g.dart`
- `lib/features/remote/domain/remote_session.dart`
- `lib/features/remote/presentation/remote_screen.dart`
- `lib/features/settings/presentation/settings_screen.dart`
- `lib/l10n/app_de.arb`
- `lib/l10n/app_en.arb`
- `lib/l10n/app_es.arb`
- `lib/l10n/app_fr.arb`
- `lib/l10n/app_it.arb`
- `lib/l10n/app_nl.arb`
- `lib/l10n/app_pt.arb`
- `lib/l10n/app_pt_BR.arb`
- `lib/l10n/generated/app_localizations.dart`
- `lib/l10n/generated/app_localizations_de.dart`
- `lib/l10n/generated/app_localizations_en.dart`
- `lib/l10n/generated/app_localizations_es.dart`
- `lib/l10n/generated/app_localizations_fr.dart`
- `lib/l10n/generated/app_localizations_it.dart`
- `lib/l10n/generated/app_localizations_nl.dart`
- `lib/l10n/generated/app_localizations_pt.dart`
- `test/database_rules_test.dart`
- `test/domain_rules_test.dart`
- `test/localization_test.dart`
- `tool/check_localization.dart`
- `tool/export_ios_localizations.py`

## Updated

- `README.md`
- `analysis_options.yaml`
- `pubspec.yaml`
- `pubspec.lock`
- `lib/main.dart`
- `ios/Runner/Info.plist`
- `ios/Runner.xcodeproj/project.pbxproj`
- `ios/Runner/GeneratedPluginRegistrant.m`
- `ios/Runner/GeneratedPluginRegistrant.h`
- `lib/shared/widgets/custom_app_bar.dart`
- `lib/shared/widgets/custom_button.dart`
- `lib/shared/widgets/custom_loader.dart`
- `lib/shared/widgets/custom_text_field.dart`

The four existing shared widget files received formatting only. Native plugin registrants and Flutter ephemeral package metadata were refreshed by dependency setup. No builds, static-analysis run, localization-guard run, or tests were executed.

## Widget and analytics update

See [WIDGET_ANALYTICS.md](WIDGET_ANALYTICS.md#created-files) for files added during the widget and analytics update, modified areas, and verification status.
