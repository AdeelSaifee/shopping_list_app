# Shopping List App Flutter

A mobile application built with Flutter and Dart for managing and organizing shopping and grocery lists. The app allows users to add, categorize, track, and dismiss items while mastering robust form validation, custom form-aware input controls, clean widget tree architecture, and dynamic two-way asynchronous screen navigation.

Targeted and optimized strictly for Android and iOS.

## Features

- **Cloud Persistence via HTTP POST:** Sends newly created grocery items directly to Firebase Realtime Database using `http.post()`, serialized to JSON with `dart:convert`.
- **Backend HTTP Client Integration:** Added official `http` package with namespaced alias `as http` for structured REST API communication with cloud services.
- **Grocery Item Management:** Add new grocery items with custom name, numeric quantity, and category classification across 10 distinct categories (Vegetables, Fruit, Meat, Dairy, Carbs, Sweets, Spices, Convenience, Hygiene, Other).
- **Form Orchestration & GlobalKey Architecture:** Integrated Flutter's built-in `Form` widget bound to a `GlobalKey<FormState>()`, eliminating individual controller bloat and synchronizing validation, state saving, and form resetting from outside the widget tree.
- **Comprehensive Form Validation & Instant Error Feedback:** Inline validation enforcing name length boundaries (between 2 and 50 characters) and numeric constraints for quantity (valid, positive integer > 0), with instant error label feedback.
- **Form-Aware Dropdown Selection:** Custom `DropdownButtonFormField` displaying category titles along with corresponding visual RGB color indicator boxes.
- **Form Reset Mechanism:** Instant form clearance restoring initial default values and clearing error highlights via `_formKey.currentState!.reset()`.
- **Empty State Fallback (Conditional Rendering):** Dynamically renders a centered fallback view (*"No items added yet."*) when `_groceryItems.isEmpty`, automatically switching to the scrollable list upon item insertion.
- **Swipe-to-Delete (Dismissible):** Fluid swipe gesture deletion powered by `Dismissible` with strict `ValueKey(item.id)` tracking for accurate widget-to-data reconciliation and synchronized UI removal.
- **Asynchronous Two-Way Screen Navigation:** Decoupled navigation using `await Navigator.of(context).push<GroceryItem>()` and `Navigator.of(context).pop(GroceryItem(...))` with type safety and null checks.
- **Centralized Dark Theming:** High-contrast, clean dark theme configured via `ThemeData.dark().copyWith(...)` with customized surface, scaffold, and app bar palettes.

## Key Learnings & Flutter Concepts Mastered

Throughout this project, several critical Flutter and Dart concepts were studied, practiced, and integrated into production-ready code:

### 0. HTTP POST & Data Serialization
- **REST Endpoints (`Uri.https`):** Constructed secure target URLs pointing to Firebase collections with mandatory `.json` suffix.
- **JSON Encoding (`dart:convert`):** Serialized Dart Maps containing primitive fields and category titles into JSON payloads via `json.encode()`.
- **Server-Side Unique IDs:** Delegated ID generation to Firebase, eliminating client-side UUID collision concerns on POST operations.

### 1. Form Infrastructure vs Standalone Controllers
- **`Form` & `GlobalKey<FormState>`:** Mastered why `Form` with a global key is vastly superior to maintaining multiple `TextEditingController` instances for multi-input workflows.
- **Universal Remote Pattern:** Leveraged `_formKey.currentState!` to access internal form methods (`validate()`, `save()`, `reset()`) from external button handlers without context plumbing.

### 2. Form Validation & Data Extraction Lifecycle
- **Validator Functions:** Implemented pure validator closures returning error strings on failure or `null` on pass.
- **State Saving (`onSaved`):** Extracted validated inputs safely into local state variables only after `validate()` returns `true`.
- **Resetting:** Reset form inputs and purged error messages in one call using `_formKey.currentState!.reset()`.

### 3. Form-Aware Inputs & Horizontal Constraints
- **Unconstrained Width Dilemma:** Understood why `TextFormField` and `DropdownButtonFormField` crash inside `Row` widgets due to unbounded horizontal constraints.
- **`Expanded` Wrappers:** Applied `Expanded` widgets to constrain horizontal inputs while dividing row space proportionally.

### 4. Two-Way Asynchronous Navigation Flow
- **Typed Route Futures:** Utilized `push<GroceryItem>` generics for compiler-enforced return type checking.
- **Modal Dismissal with Result:** Returned constructed `GroceryItem` instances using `Navigator.of(context).pop(result)`.
- **Defensive Null Handling:** Guarded against cancellations (e.g. system back button or AppBar return) with `if (newItem == null) return;`.

### 5. High-Performance Lists & Gestures
- **`ListView.builder`:** Leveraged on-demand item rendering for lightweight memory usage and fluid scrolling.
- **`Dismissible` & `ValueKey`:** Enforced exact item identity during swipe gestures using `ValueKey(_groceryItems[index].id)`, preventing UI glitches and visual ghosting.
- **State Synchronization:** Synced visual dismissals with underlying Dart state via `_groceryItems.remove(item)` inside `setState()`.

### 6. Clean Conditional UI Rendering
- **Local Variable Pattern:** Replaced unreadable nested ternaries with a top-level local variable `Widget content = const Center(...);` conditionally reassigned to `ListView.builder` when `_groceryItems.isNotEmpty`.

## Technical Architecture & Design Decisions

### 1. Form-Driven Data Pipeline
- **Single Point of Validation:** Form submission is gated behind a strict short-circuit `if (_formKey.currentState!.validate())` check.
- **Model Creation on Boundary:** Domain objects (`GroceryItem`) are constructed only at the point of confirmed validity before being popped across navigation boundaries.

### 2. Navigation & State Separation
- **Loose Coupling:** `NewItem` screen is an independent form widget that knows nothing about `GroceryList`'s internal storage or list implementation; it simply yields a `GroceryItem`.
- **Single Source of Truth:** The grocery list is owned and updated exclusively within `_GroceryListState`.

### 3. Strongly Typed Category System
- **Enum & Category Model:** Paired `Categories` enum with a strongly-typed `Category` class, mapping identifiers to human-readable titles and bespoke `Color.fromARGB` color swatches.

## Project Structure

```
shopping_list_app/
|-- lib/
|   |-- main.dart                       # App entry point & dark theme setup
|   |-- data/
|   |   |-- categories.dart             # Category map with titles & RGB colors
|   |   `-- dummy_items.dart            # Initial dummy data used during development
|   |-- models/
|   |   |-- category.dart               # Category model class & Categories enum
|   |   `-- grocery_item.dart           # GroceryItem data model (id, name, quantity, category)
|   `-- widgets/
|       |-- grocery_list.dart           # Main list dashboard with empty fallback & Dismissible rows
|       `-- new_item.dart               # Input form with GlobalKey, TextFormField & Dropdown
|-- pubspec.yaml                        # Project metadata & dependencies
`-- README.md                           # Project documentation
```

## Getting Started

### Prerequisites

- Flutter SDK (v3.13.0 or higher recommended)
- Android Studio / VS Code / Xcode for emulation or physical device testing

### Dependencies

Defined in `pubspec.yaml`:

- `cupertino_icons: ^1.0.8`
- `http: ^1.6.0` (REST API communication & HTTP client)

### Running the App

1. **Clone the repository:**
   ```bash
   git clone https://github.com/AdeelSaifee/shopping_list_app.git
   cd shopping_list_app
   ```

2. **Fetch dependencies:**
   ```bash
   flutter pub get
   ```

3. **Run static code analysis:**
   ```bash
   flutter analyze
   ```

4. **Launch the application:**
   ```bash
   flutter run
   ```
