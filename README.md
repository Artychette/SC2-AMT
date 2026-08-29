# SC2-AMT

compiles a bunch of different tools for SC2 mods.

Some features require initialization to work, because they can affect performance while some people may not want the thing. They can be initialized with the "Init AMT feature" action. There is a "all at once" option that launch everything (except debug, which need to be run independently if you want it)
It's tested enough (been using that for several month - up to a year or two for some pieces - without trouble) but i won't expect it to be bullet proof.


Summary :

Game Pause : pause the game (duh ?). There is special support to handle timers ; see the doc for the details.
Transmission system : similar to the e-mail system but allow for a whole conversation between several characters (Obsolete, wait for the next update, if you don't use it already)
Automatic Production Queue Panel : "plug'n'play" production queue panel. Customizable, auto-hide during cinematic/victory screen
Control Group rally : when setting the rally of a training structure onto a unit, insert the trained unit into the relevant control group(s)
Combat state : extend (actually, replace) the built-in "in combat" system to make it closer to the WoW system (in particular, dealing damage put the unit in combat) and make easier some stuff like muta's rapid regen
Damage type update : change the "Type" field of ALL damage effect (if left to the default value) to fit the death type (e.g. make firebat actually deal fire damage, which can be used in DR)
Top Bar Builder : see the update post below
A bunch of other small stuff
