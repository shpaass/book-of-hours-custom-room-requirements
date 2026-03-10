## Book of Hours: Customizable Room Unlock Requirements
This mod allows to change the room-unlock requirements in the game Book of Hours.

### How to play it?
The mod is not on the Steam Workshop yet, for it's a blank slate, so you need to install it manually:
1. Download the repository, unpack it. 
1. Put the folder `Customizable_Room_Unlock_Requirements` into `C:\Users\%Your_Username%\AppData\LocalLow\Weather Factory\Book of Hours\mods`.
1. Launch the game. In the main menu, click the hammers button on the bottom left. You should see the mod list.
1. Enable the mod and close the list. You don't need to restart the game, but feel free to do so if things don't work out.
1. Start the game. It can be an old save or a new run, it should work either way.
1. If you want to use the other stored terrain from the repository, then replace the `.json` file in the mod folder with the one you chose from the repository `terrains` folder.

### How to change it?
If you want to make your own custom adventure or to tinker with layouts, here's an example of how it can be done:
1. Enable the developer mode of the game - go to `C:\Users\%Your_Username%\AppData\LocalLow\Weather Factory\Book of Hours\config.ini`, and add `moonserp=1` on a new line at the bottom. Now you can enable the dev mode in-game by pressing ``Ctrl+` `` (located in the upper-left corner of the keyboard, usually).
1. Make a manual save of your current run to not mess it up.
1. Make a new run for development and save it manually after the starting sequence. You're ready for dev.

In the dev mode, you can unlock and re-lock the rooms by clicking on their background. Let's say you want to change the unlock-requirements for the Cucurbit Bridge:
1. If it's already unlocked, then lock it back by enabling the dev mode and clicking on the bridge. There are different layers, so some places can lock it when others don't.
1. Exit the dev mode and see how the unlock verb is called. In our case it's called Mist-Cloaked Bridge.
1. Search for "Mist-Cloaked Bridge" in the "terrain" `.json` file of the mod. Confirm that you're looking at the right thing by comparing the unlock-requirements in the file with what you see in the game.
1. Change the unlock-requirements to what you want. This part is explained in more detail below. For now, we can just change the value of `forge` from 1 to 2 in `required`.
1. Save the file. Copy it into the mod folder, replacing the old one.
1. In the game, get back to the main menu. Open the mod list, disable the mod, close the list so the changes are applied, and then re-enable the mod the same way.
1. Load the save. You should see your changes.
1. Repeat it for all the rooms you want to change.

Let's consider a more intricate example. Let's say you want the room Watchtower's Tower: First Floor to require both 2 Forge and 2 Lantern at the same time, when the Assistant must have used a Memory but must have not drunk any beverage. To spice things up, let's add that Assistant must have either 1 Edge or 1 Heart on top of that.  
We get the name of the verb, Gatehouse Stairs, and navigate to it in the file. The thing that interests us is the `preslots` array, in particular its members `required`, `forbidden`, and `essential`.  

`essential` contains things that all must be true at the same time. It's equivalent to `A and B and ...`, so let's put our forge and lantern requirements there. The aspects are listed below, in the Materials section of the guide.
```
"essential":{
    "assistance":1,
    "forge": 2,
    "lantern": 2
}
```

`required` contains things at least one of which must be true. It's equivalent to `A or B or ...`, so let's put Edge and Heart requirements there:
```
"required":{
    "edge": 1,
    "heart": 1
},
```

`forbidden` contains things none of which must be true. It's equivalent to `not(A or B or ...)`. Let's put the beverage requirement there. To get the needed ID of the name of the assistant-beverage aspect, we navigate to the game files: `Steam\steamapps\common\Book of Hours\bh_Data\StreamingAssets\bhcontent\core\elements\_aspects.json`. We  search for "exalted" and find that the aspect's ID is `exalted.beverage`. You can find other aspects you want the same way - remember how it's called in the game and search this file for an ID.
```
"forbidden":{
    "exalted.beverage": 1
},
```

Congratulations! A room now has a custom complex requirement.

### How to contribute?

1. Fork the repository. 
2. Clone your fork to your machine. You can use [Git Bash](https://git-scm.com/install/windows) for that, or any other version of Git you prefer. 
3. Commit the changes locally. 
4. Push the changes to your remote fork on Github.
5. Make a Pull Request to the main (this) repository. I'll review and merge your changes.
6. If you have troubles with these steps, then feel free to ask any LLM for clarifications or on a [discord server](https://discord.gg/JvbdTp7TfZ) for Book of Hours.

### Materials
This section has some bits and pieces of useful info that can help you in modding.  

The aspects list is `edge, forge, grail, heart, knock, lantern, moon, moth, nectar, rose, scale, sky, winter`. In other words, if you want a room to require Winter 2, then you add `"winter": 2` to the requirements. The other names of aspects, like assistant exalted with a memory, you can find in `Steam\steamapps\common\Book of Hours\bh_Data\StreamingAssets\bhcontent\core\elements\_aspects.json`

If you want to take a look at the files of other mods in Workshop, they are located at `Steam\steamapps\workshop\content`. If you navigate inside the numbered folders, the file `synopsis.json` has the name of the mod. Alternatively, you can unsubscribe and resubscribe to the mod, and then sort the folder by "Date modified" to see which folder changed most recently.

There is an extensive guide on modding the previous game on this engine, Cultist Simulator ( [link](https://docs.google.com/document/d/1BZiUrSiT8kKvWIEvx5DObThL4HMGVI1CluJR20CWBU0/edit?tab=t.0) ).
