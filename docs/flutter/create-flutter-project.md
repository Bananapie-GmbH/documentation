# Flutter App Template 🚀

To use the template please open: https://github.com/Bananapie-GmbH/app_template

A streamlined Flutter application template for Android and iOS development with automated code generation and modern architecture patterns.

## 🌟 Features

- **🏗️ BLoC Pattern**: State management with `flutter_bloc`
- **🧭 Navigation**: Declarative routing with `go_router`
- **🔥 Firebase Integration**: Analytics, Auth, Messaging, and Crashlytics
- **🧱 Mason Code Generation**: Automated feature scaffolding
- **⚡ Make Commands**: Streamlined development workflow
- **🌍 Internationalization**: Multi-language support
- **📱 Cross-Platform**: Android and iOS support

## 🚀 Quick Start

### Prerequisites
- Flutter SDK (3.7.2+)
- Mason CLI: `dart pub global activate mason_cli`

### Setup
1. **Clone and setup**:
   ```bash
   git clone <repo-url>
   cd app_template
   make app package_name=com.yourcompany.yourapp app_name=yourapp
   ```

2. **Install dependencies**:
   ```bash
   flutter pub get
   mason get
   ```

3. **Run the app**:
   ```bash
   flutter run
   ```

## 🏗️ Project Structure

```
lib/
├── common/
│   ├── api/          # API clients and services
│   ├── bloc/         # Shared BLoC components  
│   └── styles/       # Theme and styling
├── features/         # Feature modules (auto-generated)
├── l10n/            # Translations
└── main.dart        # App entry point

mason/               # Code generation templates
```

## ⚡ Make Commands

### App Configuration
```bash
# Update package name, app name, and display name
make app package_name=com.company.app app_name=myapp"
```

### Code Generation
```bash
# Generate a complete feature (page, cubit, state, providers)
make feature name=profile

# Generate action state cubit for complex operations
make action-state name=authentication

# Generate pagination state for lists
make pagination-state name=user_list

```

### Development
```bash
# Install Mason bricks
make get-bricks

# Generate translations
make translations
```

## 🧱 Feature Generation

Generate a complete feature with one command:

```bash
make feature name=profile
```

**This creates**:
```
lib/features/profile/
├── profile_page.dart              # Main page widget
├── bloc/
│   ├── profile_page_cubit.dart   # Business logic
│   └── profile_page_state.dart   # State management
├── handler/
│   ├── profile_page_provider.dart # BLoC provider
│   └── profile_page_builder.dart  # BLoC builder
└── widgets/                       # Feature-specific widgets
```

## 🎯 Cursor AI Integration

This template includes **Cursor Rules** (`@cli_rules`) that provide intelligent AI assistance for development workflows.

### **What are Cursor Rules?**
Cursor Rules are context-aware guidelines that help Cursor AI understand your project's specific commands and patterns. When you ask Cursor to generate features or perform tasks, it automatically uses the correct `make` commands instead of generic approaches.

### **Smart AI Assistance**
- **Automatic Command Selection**: Cursor knows to use `make feature name=login` instead of manual file creation
- **Project-Aware**: Understands your folder structure and naming conventions
- **Consistent Generation**: Always follows your project's architectural patterns
- **Time-Saving**: No need to explain the same commands repeatedly

### **Enhanced Developer Experience**
With Cursor Rules, you can simply ask:
- *"Create a profile feature"* → Cursor runs `make feature name=profile`
- *"Add authentication state"* → Cursor runs `make action-state name=authentication`  
- *"Generate translations"* → Cursor runs `make translations`

The AI understands your project context and executes the right commands automatically! 🚀 

## 🔥 Firebase Setup

### Prerequisites
1. **Install Firebase CLI**:
   ```bash
   npm install -g firebase-tools
   firebase login
   ```

2. **Install FlutterFire CLI**:
   ```bash
   dart pub global activate flutterfire_cli
   ```

### Project Configuration

#### Option 1: Automatic Setup (Recommended)
```bash
# Configure Firebase for your project automatically
flutterfire configure
```
This command will:
- Create a Firebase project (or select existing)
- Register your app for Android and iOS
- Download configuration files
- Generate `firebase_options.dart`



## 🌍 Internationalization

### Add new language:
1. Create `lib/l10n/app_[locale].arb`
2. Add translations following `app_en.arb` structure
3. Run `make translations`

### Usage:
```dart
Text(AppLocalizations.of(context)!.welcome)
```

## 🚀 Build & Deploy

Before building make sure you have ``production.config.json`` in your root directory.

### Android
```bash
# Debug APK
make android
```

### iOS
```bash
# Build for iOS
make ios
```

## 📦 Key Dependencies

- `flutter_bloc`: State management
- `go_router`: Navigation  
- `firebase_core`: Firebase integration
- `dio`: HTTP client
- `equatable`: Value equality
- `mason`: Code generation

---

**Ready to build amazing apps! 🎉**
