<p align="center"><img src="https://raw.githubusercontent.com/awkisopen/arena-respawn/master/media/arena_respawn_brand_512.png" alt="Arena: Respawn" width="350px" /><br /><h2 align="center"><a href="http://steamcommunity.com/groups/ArenaRespawn">Steam Group</a> &bull; <a href="http://wiki.respawn.tf">Wiki</a> &bull; <a href ="https://github.com/Xerol/arena-respawn/raw/master/addons/sourcemod/plugins/arena_respawn.smx">Plugin Download</a></h2></p>

**Arena: Respawn** is a custom gamemode for [Team Fortress 2](http://www.teamfortress.com). Like Arena mode, the primary objective is to kill the other team before they kill yours. Unlike Arena mode, players can be respawned mid-round if a teammate captures the central control point.

This simple rule change has proven to be an interesting addition to the Arena gamemode, as mentioned in the [Official Team Fortress 2 blog](http://www.teamfortress.com/post.php?id=14487). If you'd like to try it out before installing, hop into a [server near you](http://steam.respawn.tf) and give it a spin.

## Before you download

This is a fork of awk's A:R plugin, intended to test changes internally. You're free to use this version, but keep in mind there may be untested modifications, bugs, or outright crashes if you attempt to use this version.

## Use and installation

### Prerequisites
1. A dedicated server running Team Fortress 2. ([Installation guide](https://wiki.teamfortress.com/wiki/Dedicated_server_configuration))
2. MetaMod/SourceMod. ([Installation guide](https://wiki.alliedmods.net/Installing_SourceMod_(simple)))

Once SourceMod is installed, you're good to go. **SourceMod is the only MetaMod requirement to run Arena: Respawn.**

This plugin will function on all official Arena maps and most community-made Arena maps. It contains the vanilla gamemode itself and nothing extra, although you can add some [extra stuff](#extra-stuff) if you want later. **[Download the plugin here](addons/sourcemod/plugins/arena_respawn.smx?raw=1)** (wgettable link) and drop it in your `addons/sourcemod/plugins` folder.

That's it! **Arena: Respawn** will automatically replace Arena mode. If you're interested in extra configuration or other cool stuff, read on.

### Configuration

**Arena: Respawn** comes with a bunch of configuration variables that you can set in your server's `server.cfg`. They're automatically set to sensible defaults, so you don't have to set these unless you want something other than the gamemode's default behavior.

#### `ars_loaded`

A simple variable that returns `1` if the plugin has been loaded, regardless of whether or not it is active. Meant for server scripting.

#### `ars_enable`
**Default value:** `1`

Turns **Arena: Respawn** on or off. It's recommended to unload the plugin manually instead of using this variable.

#### `ars_cap_time`
**Default value:** `1.0`  
**Recommended tournament value:** `1.25`

Approximate amount of time (in seconds) that it takes for a point to be captured. This value is multiplied by the number of control points present on a map squared.

#### `ars_invuln_time`
**Default value:** `3.0`  
**Recommended tournament value:** `1.0`

How long to flash Uber on a newly respawned player.

#### `ars_first_blood_threshold`
**Default value:** `14`  
**Recommended tournament value:** `-1`

Number of active players required to enable First Blood (5 second critboost on the first kill in the round). Set to `0` to always enable First Blood or `-1` to always disable it.

#### `ars_lms_critboost`
**Default value:** `8.0`  
**Recommended tournament value:** `-1`

For the Last Man Standing, a death on the opposing team will result in a critboost for this many seconds. Set to 8 seconds by default to match a Kritz charge.

#### `ars_lms_minicrits`  
**Default value:** `0`  
**Recommended tournament value:** `0`

If set to `1`, replaces the Last Man Standing critboost with a minicrit boost.

#### `ars_doublecap_time`
**Default value (Public):** `51`  
**Default value (Tournament):** `31`
 
Controls the amount of time between a point being captured and the point becoming available to double cap (for an instant win). 
 
## Extra stuff

### Running a 5v5 Arena: Respawn tournament
The console command `ars_tournament_start` will put the server into Tournament Mode. Activating Tournament Mode will put players into a SOAPDM-like deathmatch mode from which they can choose their classes, set their team names, ban classes, and ready up to begin the tournament.

To set **Arena: Respawn** back to public play mode, use the command `ars_tournament_stop`. To reset a tournament, use `ars_tournament_start` again. **Do not** use `mp_tournament_restart`; this will fail to put the server back into Pre-Tournament mode.

#### Tournament commands
* `!ready` - Sets the team state to Ready.
* `!unready` - Sets the team state to Not Ready.
* `!class <class>` - Switches the player to the given class. Arena mode blocks class changes, so it is difficult to change class in the pre-round deathmatch mode.
* `!banclass <class>` - Bans the given class from play for the duration of the map. This is a requirement for **Arena: Respawn** tournaments.
* `!teamname <name>` - Sets the team's name to `<name>`. Team names can be up to 15 characters long.

#### Tournament configuration
* [`cfg/sourcemod/respawn.pretournament.cfg`](cfg/sourcemod/respawn.pretournament.cfg)- Deathmatch mode settings. Deathmatch should have no map end conditions.
* [`cfg/sourcemod/respawn.tournament.cfg`](cfg/sourcemod/respawn.tournament.cfg) - Tournament settings. Set win conditions in this configuration file only.
* [`cfg/sourcemod/respawn.public.cfg`](cfg/sourcemod/respawn.public.cfg) - Public server settings. Executed upon exiting tournament mode with `ars_tournament_stop`.

#### Known bugs
Tournament Mode is essentially one giant hack to restore tournament-like functionality into Arena mode. If and when TF2 officially supports tournament setup time in Arena mode, **Arena: Respawn** will happily switch over to the in-game tournament logic.

Tournament Mode functions well overall, but occasionally a round win will trigger during pre-round Deathmatch time. These win screens are harmless and the humiliation period is cut short to get both teams back into Deathmatch mode as quickly as possible. (Note: It is possible to overcome this limitation with SourceMod extensions, but this would add another dependency to solve what is effectively a cosmetic issue.)

### Running an Arena: Respawn server alongside non-Arena maps
**Arena: Respawn** will automatically stop functioning if a non-Arena map is loaded or if `tf_gamemode_arena` is not set to `1`.

### Custom map fixes

Not all Arena maps are entirely plug-and-play. Some custom maps have made design decisions based on the rules of standard Arena that just don't work in **Arena: Respawn**, such as the train arriving late in Ferrous, the spawn doors shutting in Hardhat, or the point exploding and killing everyone in Blackwood Valley.

Fortunately, the excellent [Stripper: Source](http://www.bailopan.net/stripper/) tool allows you to apply on-the-fly custom map fixes as the map loads. Install Stripper: Source and copy the [appropriate files](addons/stripper/maps) into your `addons/stripper/maps` directory. (**Note:** You may need to download a [recent snapshot](http://www.bailopan.net/stripper/snapshots/1.2/) that works with recent versions of SourceMod.)

**Note:** I have deliberately avoided working these special, per-map cases into the plugin (and will not accept merge requests that do so) for the following reasons:
- While all per-map problems can be eventually be solved by SourceMod, this often involves tearing out elements of the map to do so. The Stripper: Source configuration files leave the map alone and modify its *functionality* instead of playing butcher to the hard work of a mapper.
- Stripper syntax is closely related to entity keyvalue data, which most mappers are familiar with. This allows a mapper to write their own **Arena: Respawn** modifications without having to learn an extra tool in the process.
- Large amounts of special case code are not just bad practice, but downright nightmarish to maintain.

### Using King of the Hill maps as Arena maps

**Arena: Respawn** is also designed to function on King of the Hill maps, but additional Stripper configuration is required to do so.

If you would like to run a King of the Hill map, copy or symlink [koth.cfg](addons/stripper/maps/koth.cfg) to a filename corresponding with the map you wish to use. For example, on a \*nix system, `ln -s koth.cfg koth_king.cfg` would create a symlink at `koth_king.cfg`, loading the special KOTH Stripper rules the next time `koth_king` is played.

Aside from this slightly manual configuration, everything else about the KOTH conversion is handled inside the plugin, including a full conversion of each spawn area to an Arena-style spawn.

### Compiling the plugin yourself

You'll need [smlib](https://www.sourcemodplugins.org/smlib/) and the source files provided under `addons/sourcemod/scripting`. After installing SourceMod, change directory to `addons/sourcemod/scripting` and run `compile.sh arena_respawn.sp` or `compile.exe arena_respawn.sp`. This produces `arena_respawn.smx` in the `addons/sourcemod/scripting/compiled` directory.

If you create a modified version of **Arena: Respawn** to run on a public server, you must release the source of your modified code under the terms of the [AGPL](LICENSE.txt). This way, **Arena: Respawn** remains an open gamemode that all server owners can benefit from, instead of a select few keeping potentially new and interesting changes to themselves.

**Arena: Respawn** has always been intended to be a plugin open for the entire TF2 community to experiment with, so play nice and keep it that way.

### LAGBOT's Notes

You will need an older version of sourcemod to compile the plugin (and possibly run it). The current release of smlib linked above does not seem to work at all for compiling A:R, due to requiring the Entity_AddOutput function. The current version [on github](https://github.com/bcserv/smlib) does have this function and works. The as-compiled plugin does not seem to work with SM 1.7 or above, so for now all builds and testing are being done with the last build of 1.6. Additionally, download links for SM versions prior to 1.7 are no longer listed on the main sourcemod site, but are still available [here](http://www.sourcemod.net/smdrop/1.6/). I managed to get the plugin to compile with sourcemod-1.6.4-git4625-windows.zip from that directory.

I have yet to try building under SM 1.7 or later, but I think the problem was with the old smlib version. There are also as-yet-unresolved issues with getting the proper metamod and stripper versions to work, but that is outside the scope of making changes to the plugin which this fork is focused on.
