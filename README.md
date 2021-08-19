# howlerWrapper
A wrapper for howlerJS that manages howl objects for you, so you can focus on playing the damn audio.

HowlerJS is an amazing library, but in its quest for making a complete system that works everywhere, it somewhat derailed from simplicity and usability.

So I made a wrapper that aims at managing Howl objects for you, so you can focus on actually playing the audio you want to play.

This wrapper was initially made as an alternative to Construct's audio addon, so it follows some (questionable) design patterns to keep in line with how audio is managed in Construct.
People are however free and welcome to make changes, fork this repo, make pull requests or suggest improvements.


## Quick documentation:

### init
  Use this to set the audio folder's path, and the supported file types.
  
  Example: 
  ```Javascript
  HowlerWrapper.init("/audio/", [".webm", ".ogg"]);
  HowlerWrapper.play("myMusic",  "music"); 
  //This will create a Howl object if none exist for myMusic, and will give it /audio/myMusic.webm and /audio/muMusic.ogg as src
  ```

### dbToLinear / linearToDb / dbToLinear_nocap / linearToDb_nocap
  Methods used to convert from the linear range to the dB range, 0 being -60dB and 1 being 0dB. It will also, auto convert any number under -60db to 0 instead of approaching 0 as the dBs approach -Infinity.
 
  You never need to use these methods, but it helps knowing they exist.
  
### play
  Play an audio and put it in a group.
  
  Example: 
  ```Javascript
  HowlerWrapper.play("myMusic",  "music");
  ```
  
  Note: This will reuse existing Howls when possible, and will create one Howl per group-audio couple. So if you want to play one audio file in multiple groups, it will create one Howl per group. I could arguably refactor that and manage individual Sounds when that's the case, but it would be overcomplicated for no reason.

### setPaused
  pause or resume a group.
  
  If no group is passed, it will act on every group.
  
  This pauses all the playing sounds and keeps them in an array for reference to be resumed later.
  
  Note: In HowlerJS, using play() on a paused Howl only works if that Howl has a single paused sound in it. If you have more than one, it will create a new Sound instead, which is dumb.

### setMuted
  mute or unmute a group.
  
  if no group is passed, affects every group.
  
### setLooping
  activate or deactivate looping in a group.
  
  if no group is passed, affects every group.
  
  Note: This is troublesome because ideally, I'd want finer control on this. I started experimenting with a tag system to select individual sounds, but did not implement it because I did not need it at the time. I will probably refactor this in the future though.

### setVolume / setLinearVolume
  set the volume for a group.
  
  if no group is passed, affects every group.
  
  setVolume sets the volume as dB, and setLinearVolume sets the volume as a number from 0 to 1.

### stop
  stop a group.
  
  if no group is passed, affects every group.
  
### unload
  unloads a given audio from a given group.
  
  Example:
  ```Javascript
  HowlerWrapper.unload("myMusic",  "music"); //unloads the myMusic Howl object from the "music" group
  HowlerWrapper.unload("myMusic"); //unloads the myMusic Howl objects from every group that has one
  HowlerWrapper.unload(null, "music"); //unloads all Howl objects from the "music" group
  HowlerWrapper.unload(); //unloads all Howl objects from every group
  ```
  
### load
  load a given audio in a given group.
  
  Example:
  ```Javascript
  HowlerWrapper.load("myMusic",  "music"); //creates a Howl object for "myMusic" in the "music" group if none exist.

  HowlerWrapper.load("myMusic"); //creates a new unassigned Howl object for "myMusic" if none exist.
  HowlerWrapper.play("myMusic", "music"); 
  //if the "music" group has no assigned Howl object for "myMusic", it uses the unassigned object 
  //instead of creating a new one, and assigns it to the "music" group
  ```
  
### isPlaying
  return true if a given group has any playing audio, returns false otherwise.

  If no group is specified, it checks all groups, and returns true if any audio is playing in any group, false otherwise.
  
### getVolume / getLinearVolume
  returns the volume of a given group. If no group is specified returns the masterVolume.
  
  getVolume returns the volume in dB, getLinearVolume returns the volume as a number from 0 to 1.
  
  Note: In HowlerJS, setting and getting the volume is managed by the same method. I don't like that, so I decided to have this as a separate method instead.
  
