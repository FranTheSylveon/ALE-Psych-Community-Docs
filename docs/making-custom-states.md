---
title: Custom States
description: How Make Your Own Scripteable And Custom States.
credits: FranTheSylveon
---

# Custom States

For make Custom States in ALE Psych, go to `mods/<mod>/scipts/states`.

You have 2 options for this: Replacing the original ones typing like `TitleState.hx, MainMenuState.hx, etc`
or making new one without replacing.

in this case we gonna replace MainMenuState.hx because its more easier lmao, and we gonna make to put a image for example.
and for the image in the modfolder you need to create a new folder called "images" and put a random image that you want to show.

As you can see all here is blank but because we dont add the image and the code.
![](/images/custom-states/before.webp)

So lets put this in the code.
```haxe
var imagelol:FlxSprite = new FlxSprite(300, 60).loadGraphic(Paths.image('thenameoftheimageanddontputdotpng'));
add(imagelol);
```
yeah, thats all the code you need to put.

And now we hace a silly image here!.
![](/images/custom-states/after.webp)

And if you want to change a another state like freeplay just put this code below the imagelol thing.
```haxe
function postUpdate(elapsed:Float)
{
    if(Controls.ACCEPT)
    {
        CoolUtil.switchState(new CustomState(CoolVars.data.freeplayState));
    }
        
}
```

For the switchState if you want another different state just change the part of freeplayState and replace it with: `mainMenuState, initialState, optionsState`

And thats all folks ;3.

