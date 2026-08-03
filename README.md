# US Holidays

Shows all US federal holidays plus state-specific holidays for whichever state
you pick from the dropdown.

## How the data works
- **Federal holidays**: fetched live from the free Nager.Date public holiday
  API when you have internet. The result is cached on-device automatically.
  If you're offline and a cache exists, it's used. If you're offline on first
  ever launch (nothing cached yet), the app falls back to a built-in,
  rule-based list of the 11 federal holidays so it's never empty.
- **State holidays**: come from a local dataset in `StateHolidays.kt` (there's
  no reliable free API covering all 50 states' individual observances). Edit
  `STATE_HOLIDAYS` in that file to add, remove, or correct entries for your
  state — some states have very few *legally distinct* state holidays beyond
  the federal ones, which the dataset notes.

## How to build (easiest: Android Studio)
1. Install Android Studio (free): https://developer.android.com/studio
2. Open Android Studio → **Open** → select this `holidays_app` folder.
3. Let it sync (downloads Gradle + dependencies automatically the first time —
   needs internet for this one-time step).
4. Click the green **Run ▶** button with a device/emulator selected, or
   **Build > Build Bundle(s) / APK(s) > Build APK(s)** to get an installable
   `app-debug.apk` under `app/build/outputs/apk/debug/`.

## How to build (command line, if you already have the Android SDK + Gradle)
```
cd holidays_app
gradle wrapper          # one-time, generates gradlew
./gradlew assembleDebug
```
The APK will be at `app/build/outputs/apk/debug/app-debug.apk`.

## Project structure
```
app/src/main/java/com/example/usholidays/
  MainActivity.kt        - UI wiring: state dropdown + list
  HolidayRepository.kt   - fetch/cache/offline-fallback logic
  FederalHolidays.kt     - built-in federal holiday rules (last-resort fallback)
  StateHolidays.kt       - editable per-state holiday dataset
  DateRules.kt           - "3rd Monday of January" style date math
  Holiday.kt             - data model
  HolidayAdapter.kt      - RecyclerView list rendering
```

## Notes / things you may want to adjust
- `minSdk` is set to 26 (Android 8.0+, ~99% of active devices) to use
  `java.time` directly without extra desugaring config.
- Default selected state is California — change `selectedStateAbbr` in
  `MainActivity.kt` if you'd like a different default.
- The color scheme uses navy + gold; edit `colors.xml` / `themes.xml` to
  restyle.
