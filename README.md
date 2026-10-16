# 🛒 Shopping List App

A modern, responsive, and robust Flutter application built as part of **Module 11: Handling User Input & Working with Forms** in *Maximilian Schwarzmüller's "Flutter & Dart - The Complete Guide"*.

---

## 📱 Features

- **Dynamic Grocery Management:** Add custom grocery items with custom names, quantities, and category associations.
- **Robust Form Validation:** Full form validation ensuring names are between 2 and 50 characters, and quantities are valid positive integers.
- **Categorized Inputs:** Form-aware category selection with visual color indicators.
- **Empty State Fallback:** Friendly centered fallback screen (*"No items added yet."*) automatically shown when the grocery list is empty.
- **Swipe-to-Delete (`Dismissible`):** Natural gesture support to swipe away items from the list with smooth UI animations and synchronized state updates.
- **Two-Way Navigation:** Passing newly created data objects from modal/pushed routes back to the main list screen via `Navigator.pop(newItem)` and `await Navigator.push<GroceryItem>()`.
- **Form Resetting:** One-tap form reset powered by `_formKey.currentState!.reset()`.

---

## 🏗️ Project Architecture & Structure

```
shopping_list_app/
├── lib/
│   ├── data/
│   │   ├── categories.dart        # Map of Categories with titles and theme colors
│   │   └── dummy_items.dart       # Sample initial items (used during initial scaffolding)
│   ├── models/
│   │   ├── category.dart          # Category model class & Categories enum
│   │   └── grocery_item.dart      # GroceryItem model class (id, name, quantity, category)
│   ├── widgets/
│   │   ├── grocery_list.dart      # Main screen: dynamic list, empty state, dismissible items
│   │   └── new_item.dart          # Form screen: GlobalKey, TextFormField, Dropdown, validation
│   └── main.dart                  # App entry point with dark theme configuration
└── README.md
```

---

## 💡 Core Flutter Concepts Mastered

### 1. `Form` & `GlobalKey<FormState>`
- Replaced multiple individual `TextEditingController` instances with an orchestrated `Form` widget.
- Used `final _formKey = GlobalKey<FormState>()` as a bridge to access internal form state from outside the widget tree.
- Calling `_formKey.currentState!.validate()` validates all fields at once without manual boilerplate.
- Calling `_formKey.currentState!.save()` executes `onSaved` callbacks on all fields to collect user input.
- Calling `_formKey.currentState!.reset()` wipes field contents and dismisses validation error labels.

### 2. Form-Aware Field Widgets
- **`TextFormField`:** Built-in validation with `validator: (value) => ...`, input decoration, and `onSaved: (value) => ...`.
- **`DropdownButtonFormField`:** Form-compatible dropdown selection, cleanly styled with colored category indicators.

### 3. Dismissible List Items
- Wrapped `ListTile` items in Flutter's `Dismissible` widget.
- Attached unique keys via `key: ValueKey(_groceryItems[index].id)` to ensure accurate tree reconciliation and animations.
- Attached `onDismissed` callback to remove the deleted item from the Dart data list inside `setState()`.

### 4. Two-Way Asynchronous Navigation
- Push screen expecting a typed result:
  ```dart
  final newItem = await Navigator.of(context).push<GroceryItem>(
    MaterialPageRoute(builder: (ctx) => const NewItem()),
  );
  ```
- Return data from child screen:
  ```dart
  Navigator.of(context).pop(GroceryItem(...));
  ```
- Guard against null returns (e.g. user pressing device back button).

---

## 🚀 Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/AdeelSaifee/shopping_list_app.git
   cd shopping_list_app
   ```

2. **Install dependencies:**
   ```bash
   flutter pub get
   ```

3. **Run the app:**
   ```bash
   flutter run
   ```
