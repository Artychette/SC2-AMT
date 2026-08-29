# Arty's Misc. Tools : General Documentation
All features that need initialization can be launched with the `Init AMT feature`. There is a "all at once" option that launch everything (except debug, which need to be run independently if you want it)

# Core (Trigger) Lib Extension 
### Condition (To be Released) 
- Added a “Is True” and “Is False” condition (to avoid "if(boolean==true/false)" monstrosities)
- Event (To be Released) : 
- Expose all the built-in events as registration functions (so they can be dynamically applied to triggers)

### Dynamic Container (To be Released) 
- Added a “Vector” struct (Record) to be used as a dynamic array or as a stack and a “Deque” struct to be used as a queue (can also be used as a dynamic array and a stack, but Vectors are more efficient for that). Work for (almost) all GUI variable type
- It’s all Data Table under the hood, so you should prefer using static buffer array, unless it’s really not possibly, or unless you were to shove everything into data table anyways 

### Cinematic 
- Added “Cinematic Mode Changed For Player” event : fire when a player enter/exit the cinematic mode. Use “Triggering Cinematic Player” to get the triggering player and “Triggering Cinematic Mode” to get the triggering mode (on = entering, off = exiting) 
(NOTE) natively, all player got signaled with no way to know which player is entering/exiting the mode, which caused me some trouble
- Added “Cinematic Mode” action : duplicate of the native action with the same name. Fix the issue that would prevent the console to play its “out” transition animation when entering cinematic mode and natively integrate the signaling above
 
### Player  
- Added “Player Property Changed” event : duplicate of the event of the same name but allow the use of the "Any Player" preset


### UI 
- Expose the Store/Restore preset (internally a boolean, Store = true, Restore = false)
- Added a “Convert String to UI Layout Frame” function to easily pass string into the “Create Dialog Item (In Panel) From Template” functions
### Unit 
- Added “Kill (resp. Remove) After Delay” actions : kill or remove the unit after the input duration (do not block the execution thread)
### User Data (To be Released) : 
- Added a few conversion function (Convert String to User Instance/Field + Convert User-Data Link+Instance id to User Reference)
### Visibility : 
- Fog of War Alpha related actions : 
- “Store Player Fog Alpha” : store in the current value of the fog of war alpha for the player (so it can be restored later)
- “Restore Fog Alpha For Player Over Time” : restore the (previously stored) value of the fog of war alpha for the the player over the inputted duration (dot not block the execution thread)
- “Set Fog of War Alpha For Player Over Time” : set value of the fog of war alpha for the the player over the inputted duration (dot not block the execution thread)
### Layout Misc : 
- Inserted 2 "Indicator" frame in the game UI to allow any frame to acknowledge when the player is in cinematic mode or displaying the victory panel (see the "AMT_Indicators" layout)

# Game Pause
- Pause/Unpause the game (almost). In Effect, similar to how the SoA pause the game while targeting.
  - Pause AI time, slow down global time scale to a crawl (so, technically, nothing is paused, but unless you let the game run for an hour after pausing, you won't see the difference)
  - Pause registered timers (functions are provided to register timers and have them automatically pause/unpause)
  - Game Time is untouched with regard to triggers. In particular, wait functions relying on game time will run as usual, so be careful to synchronise any AI-related script with AI Time.
  - Do not affect the game clock on the console ; you have the (built-in) “Pause Mission Time” action for that
- All pause functions are in the "Game" category, timers related ones are in the "Timer" category

# Transmission system
___Obselete___
# Automatic Production Queue Panel
- Once activated, display all units being currently produced/built and upgrades being researched.
- Can be set to be horizontal or vertical (default is vertical) - See "Set Production Queue Panel Orientation" action
- Can be set to separate trained unit/built stuff/researched upgrade in separate queues (in this order from right to left in vertical orientation, or top to bottom in horizontal) or feed everything to one queue regardless of produced type (default is separate) - See "Merge/Split Production Queue Panel Queues" action
- Color theme match the current console skin of the player (can be customized) - See "Set Production Queue Panel Background Image" action
- Can be moved anywhere on the screen - See "Move Production Queue Panel" action
- Automatically hide itself during cinematic
- Can be manually hide/shown back (still remain hidden in cinematic, regardless) - See "Show/Hide Production Queue Panel" action
- By default the unit icon is taken from the "uniticon" of the actor with the same ID as the unit. To avoid name convention failure (e.g. spectre) or multiple units sharing the same unit actor (e.g. thor) being an issue, you can input custom images to be used instead for any unit. It can be done at any time- See "Override Production Queue Panel Icon for [Unit/Research]" action
- To avoid some emergent use of train/build ability to flood the UI (e.g. creep tumor), you can make the script ignore some pair (builder/built) or any builder or any built unit. It can be done at any time and is revertible - See "Exclude/Include Back Builder-Built from Production Queue Panel" action
	- By default The 3 edge cases above are already taken care of. If there are others i missed, please, yell at me.
- The queues themselves and all customizations are on a "Per Player" basis, so it should be safe to use in a Multi-player setting (but it is absolutely not tested in this situation)
- All Production Queue Panel related functions are in the UI category
- Possibly CPU hungry (at least in theory), so it is shut down by default until you initialize it



# Control Group rally
- When Setting the rally of a training facility onto a unit, trained unit are automatically added into the control group(s) the unit rallied onto is in
- Follow the inclusion/exclusion request of the production queue panel ; more explicitly : this apply on a trained unit if, and only if, said unit is allowed to show up on the production queue panel 
- Possibly CPU hungry (at least in theory) or an unwanted feature, so it is shut down by default until you initialize it

# Combat state
- Extend the built-in "in combat" system to one somewhat closer to WoW, to make my life easier when making stuff like muta's rapid regen. Also allows for smarter autocast for stim-like abilities
- Dealing damage to an enemy or taking damage to an enemy puts a unit in combat, using an ability against an enemy or against an ally in combat puts a unit in combat.
- Provided a bunch of validator so you can choose how long that "In Combat" state must stay relevant (current available thresholds are : .25s, .5s, 1s, 2.5s, 5s, 7.5s,10s )
- Possibly CPU hungry (at least in theory) or an unwanted feature, so it is shut down by default until you initialize it

# Damage type update
- Browse through ALL damage effects of your mod and its dep and set the "kind" field of all the one with "unknown" kind to match the death type (e.g., now firebat actually deal fire damage, which can be filtered in/out by damage responses)
- Slow down the map init or an unwanted feature, so it's not run by default. You can run it through the "init AMT" function 

# Top Bar Builder
- Too many things to explain here, see (Top Bar Builder Ref) for the full “how to use”
- The init function need several parameters so it is its own things instead of wrapped into the “Init AMT feature” one
- Allow to build easily a top bar by assembling a few things into a user-type, as long as you only want the basic stuff (that is, a fancy command card locked to a (hidden) global caster, with support for easy to make SoA-like abilities)  : 
  - A global caster (user’s responsibility) already equipped with the right abilities and a set up command card
  - A layout template for the top bar itself (there is already a dozen ready use, but you can build your own and easily inject it in the “Top Bar Template” user-type)
  - A layout template for the Targeting (there is a protoss one ready to use, but you can build your own and easily inject it in the “Top Bar Targeting UI” user-type)
  - For SoA T2-like abilities (Orbital Strike, Solar Lance, Temporal Field) or anything that you want to trigger the targeting UI, an entry into the “Top Bar Extended Ability” user-type. Work pretty much like a “Create Persistent" effect in how the sequence is played.
  - For more fancy behaviour (say Zeratul/Fenix/Tychus command bar) you will need to get your hands dirty, but, in principle, you should be able to shove the coop layout into your mod and hook the right things into a “Top Bar Template” instance so the basic functionality (showing/hidden the bar, locking to the global caster) will already be taken care of, and focus on the “additional” stuff 


# Fake Energy
- A couple of Behavior/validator to allow a uniform way of emergent use of the energy bar while not being considered as actually having energy.
- Work like the Frenzy setup : units have the "Fake Energy" behavior to signal other units that they have not actual energy, and other units use the "no fake energy" validator to ignore them.
- Feedback and EMP are modified to ignore any unit with the "Fake Energy" while maintaining the vanilla behavior regardless of the blizzard dependency (up to N:Co dep, didn't test with Coop). If you can think of others abilities/behavior in (non war3) blizzard dep that touch target energy (to burn it or regen it), please yell at me.
- The respect of that feature by your own custom abilities/behaviors are your responsibility (again, same as Frenzy).

# Misc.
- Drag own Flyer setup : merely a way to uniformize abilities that drag flyers down to the ground among the different factions i made, to allow possible emergent stuff to happen and ease my life.
- "Die on caster death" behavior (with some death type variations), which does exactly what it says (because i was sick of redoing it again and again ; yes i like summoned stuff)
- “Suppress Supply Cost” behavior that remove the supply cost of the target
- "Add to Summon Count" behavior that allows usage of requirements for Magazine-like setup which do not use actual Arm Magazine ability. When added to a summon, it will add a stack of the "Summon Counter" behavior on the caster (and remove one upon death) - Yes i do that a lot and was sick of duplicating the setup.
- “Hide If Caster Hidden” behavior that hides the target whenever the caster is Hidden. Comes with a "Teleport when unhidden" variant that teleports the target next to the caster when the last stop being hidden.
- SOpLookEast,SOpLookNorth,SOpLookSouth,SOpLookWest SiteOperation actors that reorient the model to look (resp.) East, North, South, West.
- A bunch of Unit Filter validators based on attributes (Target is Biological, Mechanical, etc…)
- Some vector calculus stuff : use the built-in "Point" type as a vector, to compute vectors from a point to another, dot product and vector normalization. I mainly made that for a quick and easy way to compute coordinate change, so i can make an AI less dumb at defending a ramp (niche, but handy)
- A (layout) template for a button that use texture set for which the "pressed" and "unpressed" texture are in 2 separate files (say the command card button textures)

# Debug Commands 
(must be initialized independently with the `Debug` option)<br/><br/>
- !tag : display the tag of the selected unit (or the first in the selected group). Very handy to debug faulty caster/target hooks
- !heal : fully refill life/shield/energy for all selected units
- !position : display the position of the selected unit (or the first in the selected group) in the (X,Y) format
- !kill : instant kill all the selected units (yes, i use that a lot)
- !duplicate : duplicate the selected units (and add the duplicates to selection)
- !animplay [anim]: force the model of the selected unit to play (forever) the "anim" animation. Follow the actor message format (meaning you need to replace the spaces with comma in the animation name)
- !entitycount : display the total count of units present in the map 
- !race [ID of any race] : set the race of triggering player to the inputed one
- !pause : toggle/untoggle the global pause

- (some more - left disabled - for testing purpose of the tool mod itself ; don't mind them if you browse the Debug folder)


