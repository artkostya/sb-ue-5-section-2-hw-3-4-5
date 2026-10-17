# Section 2. Practical Assignment 3.8

Blueprint interaction on the Third Person map: pressing E toggles all lamps on or off when the player is within the button's interaction radius.

## Implementation

- Created the `BPI_Switchable` Blueprint Interface with `SwitchOn` and `SwitchOff` functions for controlling switchable objects.
- `BP_Button` implements the interface. Its state is stored in `bSwitchedOn` and exposed through the `IsSwitchedOn` function. When the state changes, the button broadcasts the `OnSwitched` Event Dispatcher.
- `BP_Lamp` implements the same interface. Its `SwitchOn` and `SwitchOff` functions turn the corresponding Point Light on and off.
- The character handles the E key input. The `IA_Interact` Input Action is added to `IMC_Default`. While the player is within a button's interaction radius, the button is stored in the `NearbyButtons` array. When E is pressed, the character checks the button's state and calls either `SwitchOn` or `SwitchOff` through the interface.
- The interaction radius is defined by the `InteractRadius` sphere component on the button (radius: 250, collision preset: `OverlapAllDynamic`). At startup, the Level Blueprint sets up overlap event handling for each `BP_Button` and calls `RegisterButton` or `UnregisterButton` on the character.
- The `ThirdPersonMap` contains one button and three lamps:
  - Button: `(1300, 1110, 8)`.
  - Lamps: `(1600, 850, 0)`, `(1600, 1100, 0)`, `(1600, 1350, 0)`.

The objects use meshes from the engine's basic shapes set: `SM_ChamferCube` for the button and `SM_Cylinder` for the lamps. The `SM_InteractZone` sphere visualizes the interaction area. `BP_Button`, `BP_Lamp`, `BPI_Switchable`, and `SM_InteractZone` live in `Content/HW_3_4_5/Interactables/`.

## Verification

1. Approach the button and press E — all three lamps turn on.
2. Press E again — all three lamps turn off.
3. Move outside the interaction radius and press E — the lamps' states remain unchanged.

# Section 2. Practical Assignment 4.4

UMG on the same Third Person map: inside a button's interaction radius a widget appears in front of the player and switches only the objects assigned to that button.

## Implementation

- `BP_Button` stores the controlled actors in the instance-editable array `ControlledActors`. `SwitchOn` and `SwitchOff` walk the array and call the same `BPI_Switchable` functions on each actor.
- `BP_Lamp` turns its Point Light on and off when the interface is called.
- `WBP_ButtonSwitch` shows the title `Controls` and the buttons `Switch On` and `Switch Off`. The widget keeps the current button in `ControlledButton` and calls the interface on it. It lives in `Content/HW_3_4_5/UI/`.
- `RegisterButton` creates the widget, adds it to the viewport, shows the mouse cursor, sets input mode to Game And UI, and ignores look input. When the player leaves every radius, `UnregisterButton` removes the widget and restores game-only input. The E key toggles the nearby button through the same interface.
- `ThirdPersonMap` has two buttons:
  - `(1300, 1110, 8)` controls lamps at `(1600, 850, 0)`, `(1600, 1100, 0)`, and `(1600, 1350, 0)`.
  - `(900, 1510, 8)` controls the lamp at `(900, 1810, 0)`.

## Verification

1. Approach the first button. The widget appears in the center of the screen.
2. Press Switch On — that button's three lamps turn on. Switch Off turns them off.
3. Leave the radius. The widget closes and mouse look works again.
4. Approach the second button. Switch On turns on only its lamp.

# Section 2. Practical Assignment 5.4

The same project was brought in line with the [ue5-style-guide](https://github.com/Allar/ue5-style-guide): asset prefixes, Blueprint variable and function names, and comments on graph blocks.

Only assets added for these assignments were moved into a project folder. Third Person template content stayed in `Content/ThirdPerson` (character, game mode, map, and input).

## Layout

```
Content/HW_3_4_5/
  Interactables/
    BPI_Switchable
    BP_Button
    BP_Lamp
    SM_InteractZone
  UI/
    WBP_ButtonSwitch
```

`IA_Interact` stays in `Content/ThirdPerson/Input/` next to `IMC_Default`.

## Naming and comments

- The button state variable is `bSwitchedOn`. The function that reads it is `IsSwitchedOn`.
- In `WBP_ButtonSwitch`, the controls are named `ButtonSwitchOn`, `ButtonSwitchOff`, and `TitleText`.
- `ControlledActors` has a tooltip and the `Config` category. Public functions on the button have descriptions.
- Comment boxes describe the switch loop on `BP_Button`, the point light on `BP_Lamp`, the E-key toggle on the character, both widget clicks, and the overlap setup in the Level Blueprint.
