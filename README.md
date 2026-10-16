# Section 2. Practical Assignment 3.8

Blueprint interaction on the Third Person map: pressing E toggles all lamps on or off when the player is within the button's interaction radius.

## Implementation

- Created the `BPI_Switchable` Blueprint Interface with `SwitchOn` and `SwitchOff` functions for controlling switchable objects.
- `BP_Button` implements the interface. Its state is stored in `IsSwitchedOn` and exposed through the `IsButtonSwitchedOn` function. When the state changes, the button broadcasts the `OnSwitched` Event Dispatcher.
- `BP_Lamp` implements the same interface. Its `SwitchOn` and `SwitchOff` functions turn the corresponding Point Light on and off.
- The character handles the E key input. The `IA_Interact` Input Action is added to `IMC_Default`. While the player is within a button's interaction radius, the button is stored in the `NearbyButtons` array. When E is pressed, the character checks the button's state and calls either `SwitchOn` or `SwitchOff` through the interface.
- The interaction radius is defined by the `InteractRadius` sphere component on the button (radius: 250, collision preset: `OverlapAllDynamic`). At startup, the Level Blueprint sets up overlap event handling for each `BP_Button` and calls `RegisterButton` or `UnregisterButton` on the character.
- The `ThirdPersonMap` contains one button and three lamps:
  - Button: `(1300, 1110, 8)`.
  - Lamps: `(1600, 850, 0)`, `(1600, 1100, 0)`, `(1600, 1350, 0)`.

The objects use meshes from the engine's basic shapes set: `SM_ChamferCube` for the button and `SM_Cylinder` for the lamps. The `SM_InteractZone` sphere visualizes the interaction area.

## Verification

1. Approach the button and press E — all three lamps turn on.
2. Press E again — all three lamps turn off.
3. Move outside the interaction radius and press E — the lamps' states remain unchanged.

# Section 2. Practical Assignment 4.4

UMG on the same Third Person map: inside a button's interaction radius a widget appears in front of the player and switches only the objects assigned to that button.

## Implementation

- `BP_Button` stores the controlled actors in the instance-editable array `ControlledActors`. `SwitchOn` and `SwitchOff` walk the array and call the same `BPI_Switchable` functions on each actor.
- `BP_Lamp` turns its Point Light on and off when the interface is called.
- `WBP_ButtonSwitch` shows the title `Controls` and the buttons `Switch On` and `Switch Off`. The widget keeps the current button in `ControlledButton` and calls the interface on it.
- `RegisterButton` creates the widget, adds it to the viewport, shows the mouse cursor, sets input mode to Game And UI, and ignores look input. When the player leaves every radius, `UnregisterButton` removes the widget and restores game-only input. The E key toggles the nearby button through the same interface.
- `ThirdPersonMap` has two buttons:
  - `(1300, 1110, 8)` controls lamps at `(1600, 850, 0)`, `(1600, 1100, 0)`, and `(1600, 1350, 0)`.
  - `(900, 1510, 8)` controls the lamp at `(900, 1810, 0)`.

## Verification

1. Approach the first button. The widget appears in the center of the screen.
2. Press Switch On — that button's three lamps turn on. Switch Off turns them off.
3. Leave the radius. The widget closes and mouse look works again.
4. Approach the second button. Switch On turns on only its lamp.
