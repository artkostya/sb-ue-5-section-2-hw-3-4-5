# Section 2. Practical Assignment 3.8

Blueprint interaction on the Third Person map: pressing E toggles all lamps on or off when the player is within the button's interaction radius.

## Implementation

- Created the `BPI_Switchable` Blueprint Interface with `SwitchOn` and `SwitchOff` functions for controlling switchable objects.
- `BP_Button` implements the interface. Its state is stored in `IsSwitchedOn` and exposed through the `IsButtonSwitchedOn` function. When the state changes, the button broadcasts the `OnSwitched` Event Dispatcher.
- `BP_Lamp` implements the same interface. Its `SwitchOn` and `SwitchOff` functions turn the corresponding Point Light on and off. On `BeginPlay`, each lamp finds all `BP_Button` actors and subscribes to their `OnSwitched` Event Dispatchers.
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