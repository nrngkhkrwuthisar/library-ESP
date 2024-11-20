# MenuManager Library for Arduino (with I2C LCD)

This library allows you to easily manage menus on an I2C LCD display using simple button input for navigation and selection. It is designed to work specifically with the `LiquidCrystal_I2C` library for I2C-controlled LCDs.

---

## Features

- Supports navigation through a list of menus.
- Simple and intuitive API for adding menus.
- Works with I2C LCD displays (16x2, 20x4, etc.).
- Button-controlled menu navigation and selection.
- Customizable menu items and navigation controls.

---

## Installation  ทดสอบ

1. **Install `LiquidCrystal_I2C` Library**:
   - Go to **Sketch** → **Include Library** → **Manage Libraries**.
   - Search for `LiquidCrystal_I2C` and install it.

2. **Copy `MenuManager` Library to Arduino Libraries Folder**:
   - Download the `MenuManager.h` and `MenuManager.cpp` files.
   - Place them in the `libraries` folder of your Arduino IDE.

---

## How It Works

The `MenuManager` library is used to create and display menus on an I2C LCD. You can easily add new menus, navigate through them using buttons, and select them to display a confirmation message. The following functions are provided:

- `add(String menuName)`: Add a new menu item.
- `displayMenu()`: Display the current menu on the LCD.
- `handleInput(int btnNext, int btnSelect)`: Handle button presses for navigation and selection.
- `getSelectedMenu()`: Get the name of the currently selected menu.

---

## Example Code

Here is an example of how to use the `MenuManager` library to create a simple menu system with a 16x2 I2C LCD:

```cpp
#include <Arduino.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include "MenuManager.h"

// Initialize LCD with I2C address (0x27) and size 16x2
LiquidCrystal_I2C lcd(0x27, 16, 2);

// Create MenuManager object
MenuManager menu(&lcd);

// Define button pins
#define BTN_NEXT 7    // Button to navigate to the next menu
#define BTN_SELECT 8  // Button to select a menu

void setup() {
    // Initialize LCD
    lcd.begin();
    lcd.backlight();

    // Set button pins as INPUT_PULLUP
    pinMode(BTN_NEXT, INPUT_PULLUP);
    pinMode(BTN_SELECT, INPUT_PULLUP);

    // Add menu items
    menu.add("Menu 1: Option A");
    menu.add("Menu 2: Option B");
    menu.add("Menu 3: Option C");
    menu.add("Menu 4: Option D");
    menu.add("Menu 5: Option E");

    // Display the first menu
    menu.displayMenu();
}

void loop() {
    // Handle button input for menu navigation and selection
    menu.handleInput(BTN_NEXT, BTN_SELECT);

    // Optionally: Get the selected menu and print it to Serial Monitor
    if (digitalRead(BTN_SELECT) == LOW) {
        String selectedMenu = menu.getSelectedMenu();
        Serial.println("You selected: " + selectedMenu);
    }
}
