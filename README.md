# GetX Starter CLI

A zero-config interactive CLI that scaffolds a production-ready Flutter + GetX boilerplate in seconds. Answer a few prompts and get a fully wired project with theming, routing, networking, and storage — ready to run.

---

## Features

- **Interactive setup** — guided prompts for project name, display name, bundle ID, and target platforms
- **GetX architecture** — controllers, bindings, and routing pre-configured
- **Light / Dark theming** — `ThemeController` with persistent preference via `SharedPreferences`
- **Network layer** — `NetworkCaller` wrapping `http` with GET / POST / PATCH / DELETE, timeout handling, and Bearer token injection
- **Local storage** — `StorageService` managing user session, tokens, and theme preference
- **Responsive UI** — `flutter_screenutil` initialised with a 390×884 design canvas
- **Environment config** — `.env` loaded via `flutter_dotenv`; `ApiConstants` reads `BASE_URL` safely
- **Snackbar helpers** — `AppHelperFunctions` using `awesome_snackbar_content` for success / error / warning toasts
- **Custom theme system** — `AppColorsExtension` (Material 3 theme extension) with full light and dark palettes
- **Multi-platform** — choose any combination of Android, iOS, Web, macOS, Windows, Linux at creation time

---

## Prerequisites

| Tool | Version |
|------|---------|
| Flutter SDK | ≥ 3.11.1 |
| Dart SDK | ≥ 3.0.0 |
| Bash | 3.2+ (macOS/Linux) |

---

## Installation

### Option 1 — One-line install (recommended)

Downloads the script and installs it globally so you can run `create_app` from anywhere:

```bash
curl -fsSL https://raw.githubusercontent.com/nuhanchowdhurylinkdin-a11y/Getx-starter-cli/main/create_app -o /usr/local/bin/create_app && chmod +x /usr/local/bin/create_app
```

Then just run:

```bash
create_app
```

### Option 2 — Download and run locally

```bash
curl -fsSL https://raw.githubusercontent.com/nuhanchowdhurylinkdin-a11y/Getx-starter-cli/main/create_app -o create_app && chmod +x create_app && ./create_app
```

### Option 3 — Clone the repository

```bash
git clone https://github.com/nuhanchowdhurylinkdin-a11y/Getx-starter-cli.git
cd Getx-starter-cli
chmod +x create_app
./create_app
```

---

## Usage

Navigate to the directory where you want the new project created, then run:

```bash
./create_app
```

The CLI will walk you through four steps:

| Step | Prompt | Example |
|------|--------|---------|
| 1 | Project name (snake_case) | `my_awesome_app` |
| 2 | App display name | `My Awesome App` |
| 3 | Organization / bundle ID prefix | `com.example` |
| 4 | Target platforms (comma-separated) | `1,2` → Android + iOS |

A summary is shown before anything is written to disk. Type `n` to cancel or press Enter to confirm.

---

## Generated Project Structure

```
<project_name>/
├── .env                          # BASE_URL and other secrets (git-ignored)
├── pubspec.yaml                  # Pre-configured dependencies
└── lib/
    ├── main.dart                 # Entry point — dotenv + StorageService init
    ├── app.dart                  # GetMaterialApp with theming and routing
    ├── core/
    │   ├── bindings/
    │   │   └── controller_binder.dart   # Registers all GetX controllers
    │   ├── common/
    │   │   ├── styles/
    │   │   │   └── global_text_style.dart  # Google Fonts / Inter text style helper
    │   │   └── widgets/                    # Shared widget components
    │   ├── controller/
    │   │   └── theme_controller.dart       # Light / Dark / System theme toggle
    │   ├── models/
    │   │   └── response_data.dart          # Unified API response model
    │   ├── services/
    │   │   ├── network_caller.dart         # HTTP client with auth headers
    │   │   └── storage_service.dart        # SharedPreferences wrapper
    │   └── utils/
    │       ├── constants/
    │       │   ├── api_constants.dart      # Base URL from .env
    │       │   └── colors.dart             # AppColors palette
    │       ├── helpers/
    │       │   └── app_helper.dart         # Snackbar & date formatting utilities
    │       └── theme/
    │           ├── theme.dart              # AppTheme (light + dark ThemeData)
    │           ├── app_colors_extension.dart  # ThemeExtension with semantic tokens
    │           └── custom_themes/
    │               ├── app_bar_theme.dart
    │               ├── elevated_button_theme.dart
    │               ├── text_theme.dart
    │               └── text_field_theme.dart
    ├── features/
    │   └── home/
    │       ├── controller/
    │       │   └── home_controller.dart    # Counter example (increment/decrement/reset)
    │       └── views/screens/
    │           └── home_screen.dart        # Demo screen with theme toggle
    └── routes/
        └── app_routes.dart                 # Centralized GetX route definitions
```

---

## Pre-installed Dependencies

| Package | Purpose |
|---------|---------|
| `get` | State management, routing, dependency injection |
| `http` | HTTP networking |
| `shared_preferences` | Persistent local storage |
| `flutter_screenutil` | Responsive sizing |
| `google_fonts` | Inter and other web fonts |
| `flutter_dotenv` | `.env` file loading |
| `awesome_snackbar_content` | Styled snackbar notifications |
| `skeletonizer` | Skeleton loading shimmer |
| `iconsax_flutter` | Iconsax icon set |
| `intl` | Date / number formatting |
| `url_launcher` | Open URLs in browser |

---

## Configuration

### Environment Variables

Edit `.env` in the project root before running:

```env
BASE_URL=https://your-api.example.com
```

`ApiConstants.baseUrl` reads this value at runtime and asserts it is set.

### Adding API Endpoints

Open `lib/core/utils/constants/api_constants.dart` and add static getters:

```dart
static String get login => '$baseUrl/auth/login';
static String get profile => '$baseUrl/users/me';
```

### Theme Colors

Primary brand colors live in `lib/core/utils/constants/colors.dart`. Semantic tokens (card background, text, borders, etc.) are defined per brightness in `AppColorsExtension`.

---

## Making HTTP Requests

```dart
final caller = NetworkCaller();

// GET
final result = await caller.getRequest(ApiConstants.profile);
if (result.isSuccess) {
  // result.responseData contains the decoded JSON
}

// POST with body
final result = await caller.postRequest(
  ApiConstants.login,
  body: {'email': email, 'password': password},
);
```

Tokens from `StorageService` are injected automatically unless overridden.

---

## Theming

Toggle theme from any widget:

```dart
final themeCtrl = Get.find<ThemeController>();
themeCtrl.toggle(); // cycles System → Light → Dark

// Or set explicitly
await themeCtrl.setThemeMode(ThemeMode.dark);
```

Access semantic colors from `BuildContext`:

```dart
final c = context.appColors;
Container(color: c.cardBg, child: Text('Hello', style: TextStyle(color: c.textPrimary)));
```

---

## Adding a New Feature

1. Create the folder: `lib/features/<feature_name>/`
2. Add `controller/`, `views/screens/`, and optionally `views/widgets/`
3. Register the controller in `ControllerBinder`:
   ```dart
   Get.lazyPut<MyController>(() => MyController(), fenix: true);
   ```
4. Add the route in `AppRoute.routes`:
   ```dart
   GetPage(name: myScreen, page: () => const MyScreen()),
   ```

---

## Platform Notes

- **Android** — display name is set in `AndroidManifest.xml` automatically
- **iOS** — `CFBundleName` in `Info.plist` is updated automatically
- **Web / macOS / Windows / Linux** — display name must be updated manually after generation

---

## License

MIT License. See [LICENSE](LICENSE) for details.

