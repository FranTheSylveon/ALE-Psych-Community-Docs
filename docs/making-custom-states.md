---
title: Custom States
description: How to create and use custom states in ALE Psych.
---

# Custom States

ALE Psych can load custom states from `mods/<mod>/scripts/states`.

You usually have two ways to use them:

- replace an existing state, such as `TitleState`, `MainMenuState`, or `FreeplayState`
- create a brand-new state and switch to it from another script

## Basic Setup

Create a new Haxe script in the states folder for your mod.

Example:

```text
mods/<mod>/scripts/states/CustomState.hx
```

Inside that file, define your state class and add whatever UI or logic you need.

## Example State

```haxe
class CustomState extends MusicBeatState
{
    override function create()
    {
        super.create();

        var bg = new FlxSprite().makeGraphic(1280, 720, FlxColor.BLACK);
        add(bg);

        var label = new FlxText(0, 0, FlxG.width, "Custom State");
        label.setFormat(null, 32, FlxColor.WHITE, "center");
        label.screenCenter();
        add(label);
    }
}
```

## Switching To It

From another state or script, switch to your custom state when needed.

```haxe
if (controls.ACCEPT)
{
    CoolUtil.switchState(new CustomState());
}
```

## Replacing An Existing State

If you want to replace an engine state instead of adding a new one, use the same filename and class name as the original state you want to override.

For example:

- `TitleState`
- `MainMenuState`
- `FreeplayState`
- `OptionsState`

## Notes

- Keep the class name and file name aligned.
- Put shared assets in your mod's `images` folder if your state loads custom graphics.
- If you only need a menu entry or option, consider whether a custom state is the right tool first.
