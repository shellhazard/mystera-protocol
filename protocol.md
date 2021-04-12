# Mystera Legacy Protocol Documentation
Comprehensive documentation of client/server messages for Mystera Legacy. 
Last updated August 11th, 2019.

### Server-to-client (referred to as "server messages")

#### zip

**Purpose:** Messages that are zipped for compression, to be unzipped and then parsed. No recorded usage at this time.

**Structure:**
```
{
    "type":"zip",
    "data":"[String containing zipped messages]"
}
```

**Parameters:**

* `data`: String - Data string containing zipped messages.

----

#### pkg

**Purpose:** Wrapper for multiple plaintext messages in their original form to be sent in one message rather than multiple consecutive messages. messages are escaped with backslashes.

**Structure:**
```
{
	"type":"pkg",
	"data":"["{"type":"fx","tpl":"heal_up","x":21,"y":25,"s":"restore","d":2}","{"type":"hpp","id":211470,"o":1000,"n":1000}"]"
}
```

**Parameters:**

* `data`: String - Data string containing array of messages.

----

#### logmsg

**Purpose:** Error message to be displayed after failed login.

**Structure:** 
```
{
	"type":"logmsg",
	"text":"Invalid username or password."
}
```

**Parameters:**

* `text`: String - Message to display.

----

#### crtmsg

**Purpose:** Error message to be displayed after failed registration.

**Structure:** 
```
{
	"type":"crtmsg",
	"text":"Username is already taken."
}
```

**Parameters:** 

* `text`: String - Message to display.


----

#### music 

**Purpose:** Start and stop playing music on the client side.

**Structure:**
```
{
	"type":"music",
	"m":"rpgtitle",
	"s":0
}
```

**Parameters:**

* `m`: String - The file to play.
* `s`: Integer/Boolean - 0 to start music, 1 to stop music.

----

#### P

**Purpose:** Ping packet, sent as response to an identical packet sent by the client.

**Structure:**
```
{
	"type":"P"
}
```

**Parameters:** None.

----

#### accepted

**Purpose:** Server acknowledgement of a successful login.

**Structure:**
```
{
	"type":"accepted",
	"id":215456,
	"name":"Badbop",
	"tile":{
		"1":50,
		"2":150,
		"17":50,
		"247":-200,
		"254":100,
		"526":50
	},
	"mw":500,
	"mh":500
}
```

**Parameters:**

* `id`: Integer - The entity ID of the logged in user.
* `name`: String - The name of the logged in user.
* `tile`: Dictionary - A list of tile IDs with their associated speed modifiers.
* `mw`: Integer - Map width.
* `mh`: Integer - Map height.

----

#### death

**Purpose:** Tell the client that the currently logged in user has died or is dead.

**Structure:**
```
{
	"type":"death",
	"death":"<span style='color:#FF0000'>Badbop has fallen! They were level 6 and skilled at questing. Killed by a Polar Bear.</span>",
	"angel_dust":10
}
```

**Parameters:**

* `death`: String - The message to display in the death popup. Note: HTML tags are not displayed and color codes don't seem to take effect here.
* `angel_dust`: Integer - The amount of angel dust gained for the death.

----

#### quit

**Purpose:** Tell the client that the server is about to terminate the connection.

**Structure:**
```
{
	"type":"quit",
	"text":"bubye"
}
```

**Parameters:**

* `text`: String - Unused by the client.

----

#### fade

**Purpose:** Tell the client to fade the screen in or out.

**Structure:**
```
{
	"type":"fade",
	"f":0
}
```

**Parameters:**

* `f`: Integer/Boolean - 1 to fade into blackness, 0 to fade out.

----

#### message

**Purpose:** Log a message to the chatbox. This is used for chat messages and most kinds of game messages.

**Structure:**
```
{
	"type":"message",
	"text":"<span style="color:#FF44FF">MysteraPlayer</span>: <span style="color:#FF00FF">Hello world!</span>",
	"id":null,
	"x":0,
	"y":0,
	"sound":"restore"
	"error":"Disconnected from server"
}
```

**Parameter:**

* `text`: String - The message to display. Note: In the official client, if the message contains "color:#FFFFFF", the message will be displayed with the first specified hex color. If this is placed within a HTML tag, it will be stripped out before being displayed.
* `id`: Integer - The ID of the entity that sent the message to be used to display the talking box.
* `x`: Integer - The x coordinate of where the talking box should be displayed.
* `y`: Integer - The y coordinate of where the talking box should be displayed.
* `sound`: String - The name of the sound file to be played.
* `error`: String - An error message detailing why the player is to be disconnected. Note: Updates `jv.disconnect_error` in the official client.

----

#### script

**Purpose:** Launch the script editor on the client side with the specified name and script data.

**Structure:**
```
{
	"type":"script",
	"name":"test",
	"script":"function doSomething() {\n	console.log("done something");\n}"
}
```

**Parameters:**

* `name`: String - The name of the script/file being edited.
* `script`: String - The contents of the script/file being edited.

----

#### mt

**Purpose:** Map transition information.

**Structure:** 
```
{
	"type":"mt",
	"n":"newbiestore",
	"m":"shop",
	"s":1,
	"c":0,
	"f":0,
	"w":30,
	"h":30,
	"t":"Newbie Store"
}
```

**Parameters:**

* `n`: String - Internal name of the map being transitioned to. Assigned to `dlevel` by the official client.
* `m`: String - The name of the music file to play upon transitioning.
* `s`: Integer/Boolean - Whether or not the map is a safe zone. 1 = true.
* `c`: Integer - The type of cave wall associated with this map.
* `f`: Integer - The type of cave floor associated with this map.
* `w`: Integer - The width of this map.
* `h`: Integer - The height of this map.
* `t`: String - The full name/title of the map to be displayed upon transitioning.

----

#### obj_tpl

**Purpose:** Define a new object template.

**Structure:**
```
{
	"type":"obj_tpl",
	"tpl":24,
	"name":"Hunter's Cloak*",
	"spr":755,
	"stack":0,
	"pickup":1,
	"block":0,
	"build":null
}

{
	"type":"obj_tpl",
	"tpl":1,
	"name":"Stone Wall",
	"spr":870,
	"stack":0,
	"pickup":0,
	"block":1,
	"build":"931b,s,932q0.6|,n,n,915fo12|"
}
```

**Parameters:**

* `tpl`: Integer - The ID of the object template being updated/created.
* `name`: String - The name of the object. Note: This uses the current name of the object, including runes/anvil renames, so for items it is often inaccurate.
* `spr`: Integer - The sprite of the object.
* `stack`: Integer/Boolean - Whether or not the object is stackable. 1 = true.
* `pickup`: Integer/Boolean - Whether or not the object can be picked up as an item. 1 = true.
* `block`: Integer/Boolean - Whether or not the object is solid and cannot be walked through. 1 = true.
* `build`: String - A data string containing the values required to visually "buildup" the object and make it appear 3D. 

----

#### plr_tpl

**Purpose:** Define a new entity template.

**Structure:**
```
{
	"type":"plr_tpl",
	"id":"chicken",
	"n":"Chicken",
	"t":"",
	"l":1,
	"s":5,
	"b":-1,
	"h":-1,
	"c":-1,
	"hc":6504471,
	"cc":14540253,
	"ec":9682175
}

{
	"type":"plr_tpl",
	"id":195807,
	"n":"Nazberry",
	"t":"Croatan",
	"pr":336,
	"p":12193106,
	"l":94,
	"s":147,
	"b":4,
	"h":8,
	"c":3,
	"hc":16772694,
	"cc":7059967,
	"ec":9682175
}
```

**Parameters:**

* `id`: Integer/String - The ID of the entity. If the entity is not a player, the ID will be a string.
* `n`: String - The name of the entity.
* `t`: String - The name of the entity's tribe.
* `pr`: Integer - The number of premium days (diamonds) the entity has remaining.
* `l`: Integer - The level of the entity.
* `s`: Integer - The sprite of the entity.
* `b`: Integer - The body of the entity.
* `h`: Integer - The hair of the entity.
* `c`: Integer - The clothes of the entity.
* `p`: Integer - The title color of the entity represented in decimal form.
* `hc`: Integer - The hair color of the entity represented in decimal form.
* `cc`: Integer - The clothes color of the entity represented in decimal form.
* `ec`: Integer - The eye color of the entity represented in decimal form.

----

#### fx_tpl

**Purpose:** Define a new effect template and attached code.

**Structure:**
```
{
	"type":"fx_tpl",
	"tpl":"chips",
	"code":"{sound: -1,x: 18,y: 226,dir: 0,template: 'chips',base_template: 'chips',start: function()\r\n{\r\n    this.life = 40;\r\n    for(i=0;i<7;i++)\r\n    {\r\n        var c = this.circle(0x663300,3);\r\n\t\tc.y += 8;\r\n\t\tc.oy = c.y;\r\n\t\tc.dy = Math.random()*2-2;\r\n\t\tc.alpha = 1;\r\n\t\tc.dx = Math.random()*4-2;\r\n\t\tc.life = 40;\r\n    }\r\n},run: function()\r\n{\r\n\r\n},move: function(p)\r\n{\r\n\tp.dy += 0.5;\r\n\tif(p.y > p.oy+12) p.dy = p.dy *-0.8;\r\n\tp.alpha -= 0.05;\r\n}}"
}
```

**Parameters:**

* `tpl`: String - The name of the effect template being updated.
* `code`: String - The code of the effect to be evaluated.

----

#### fx

**Purpose:** Trigger the display of an effect.

**Structure:**
```
{
	"type":"fx",
	"tpl":"chips",
	"x":18,
	"y":226,
	"s":-1,
	"d":0
}

{
	"type":"fx",
	"tpl":"arrow",
	"x":5,
	"y":175,
	"s":"swish",
	"d":[5,10,9,10,5]
}
```

**Parameters:**

* `tpl`: String - The name of the effect template to use.
* `x`: Integer - The x coordinate of where the effect is to be run.
* `y`: Integer - The y coordinate of where the effect is to be run.
* `s`: Integer/String - The name of the sound to be played when the effect is run.
* `d`: Integer/Array - The direction of the effect. If this value is an integer, then: 0 = north, 1 = east, 2 = south, 3 = west. If this value is an array and the `tpl` is `arrow`, the offsets are as follows: `[westernOffset, northernOffset, easternOffset, southernOffset, animationSpeed]`. This is used for projectiles, usually arrows.

----

#### obj

**Purpose:** Contains a packaged list of `o` messages [unconfirmed] and seems to trigger an additional object cleanup process.

**Structure:**
```
{
	"type":"obj",
	"data":[object messages]
}
```

**Parameters:**

* `data`: Array - List of object messages to parse.

----

#### pl

**Purpose:** Update nearby entities. Contains a packaged list of `p` messages and seems to trigger an additional entity cleanup process.

**Structure:** 
```
{
	"type":"pl",
	"data":["{\"type\":\"p\",\"id\":219833,\"tpl\":219833,\"s\":250,\"d\":1,\"x\":24,\"y\":50,\"dx\":24,\"dy\":50}","{\"type\":\"p\",\"id\":268,\"tpl\":\"cat_67\",\"s\":497,\"d\":3,\"x\":13,\"y\":55,\"dx\":13,\"dy\":55}","{\"type\":\"p\",\"id\":3248,\"tpl\":3248,\"s\":750,\"d\":2,\"x\":12,\"y\":54,\"dx\":12,\"dy\":54}","{\"type\":\"p\",\"id\":160329,\"tpl\":160329,\"s\":250,\"d\":2,\"x\":17,\"y\":54,\"dx\":17,\"dy\":54}","{\"type\":\"p\",\"id\":195807,\"tpl\":195807,\"s\":260,\"d\":0,\"x\":37,\"y\":46,\"dx\":37,\"dy\":46}","{\"type\":\"p\",\"id\":222941,\"tpl\":222941,\"s\":250,\"d\":1,\"x\":33,\"y\":50,\"dx\":33,\"dy\":50}","{\"type\":\"p\",\"id\":266,\"tpl\":\"guard\",\"s\":397,\"d\":0,\"x\":30,\"y\":59,\"dx\":31,\"dy\":59}"]
}
```

**Parameters:**

* `data`: Array - List of player messages to parse.

----

#### o

**Purpose:** Create/update nearby objects.

**Structure:**
```
{
	"type":"o",
	"x":"0",
	"y":"250",
	"d":"86"
}

{
	"type":"o",
	"x":"0",
	"y":"250",
	"d":""
}
```

**Parameters:**

* `x`: Integer - The x coordinate of the tile to update.
* `y`: Integer - The y coordinate of the tile to update.
* `d`: Integer - The template ID of the object to display on the tile.

----

#### mp

**Purpose:** Used on the mapping server. Opens the mapping dialog.

**Structure:**
```
{
	"type":"mp"
}
```

**Parameters:** None.

----

#### stat

**Purpose:** Used to update the details in the Character window.

**Structure:**
```
{
	"type":"stat",
	"obj":{
		"level":6,
		"attack":1,
		"defense":0,
		"hp":"92/92",
		"speed":1750,
		"crit_chance":"0%",
		"traits":"wise",
		"exp_to_level":2164,
		"time_alive":"0 hrs 56 mins",
		"angel_dust":45,
		"dust_earned":9,
		"myst_refund":0,
		"weight":"176/1000",
		"steps":272,
		"exp_bonus":"54%",
		"myst_bonus":"10%",
		"daily_bonus":"20%"}
	}
}
```

**Parameters:**

* `obj`: Dictionary - Contains all the details needed to update the stat window. These are self-explainatory.

----

#### upg

**Purpose:** Used to update the details in the Myst Upgrades window.

**Structure:**
```
{
	"type":"upg",
	"obj":[
		{"n":"attack","c":50,"a":false,"t":"Combat Drills","d":"Hit harder! Damage +1","s":173,"l":1},
		{"n":"defense","c":50,"a":false,"t":"Defender","d":"Reduce the damage you take. Defense +1","s":174,"l":1},
		{"n":"weight","c":20,"a":false,"t":"Pack Rat","d":"Increases your inventory weight capacity: +250","s":191,"l":1},
		{"n":"hp","c":10,"a":false,"t":"Vitality","d":"Increase your maximum health: +25","s":189,"l":1},
		{"n":"exp","c":125,"a":false,"t":"Scholar","d":"Earn more experience for every action: +20%","s":175,"l":2},
		{"n":"ups","c":50,"a":false,"t":"Sage Wisdom","d":"Earn more myst for slaying foes: +10%","s":237,"l":2},
		{"n":"crit","c":60,"a":false,"t":"Savagery","d":"Chance to do critical damage: +1%","s":205,"l":1},
		{"n":"star","c":100,"a":false,"t":"Skill Mastery","d":"Unlock the next tier of skill stars.","s":25,"l":1},
		{"n":"dodge","c":50,"a":false,"t":"Moving Target","d":"Increases your chance to dodge attacks: +4%","s":157,"l":2},
		{"n":"precision","c":50,"a":false,"t":"Precision","d":"Increases your chance to land attacks: +4%","s":142,"l":2},
		{"n":"cap","c":10000,"a":false,"t":"Ascension","d":"Boost your level cap without reincarnating: +5 Levels","s":158,"l":1},
		{"n":"refund","c":500,"a":false,"t":"Lasting Legacy","d":"Gain back some of your spent myst after reincarnating: +10% Refunded","s":323,"l":1},
		{"n":"ability","c":100,"a":false,"t":"Academic","d":"Increase your active ability slots by 1","s":336,"l":1}
	]
}
```

**Parameters:**

* `obj`: Array - Contains a list of upgrade types and details used to update the Myst Upgrades window.

**Additional Information:** 
```
{
	"n": "attack", 
	"c": 50, 
	"a": false,
	"t": "Combat Drills",
	"d": "Hit harder! Damage +1",
	"s": 173,
	"l": 1
}
```

* `n`: String - Internal name of the upgrade.
* `c`: Integer - Cost of the next upgrade, in myst.
* `a`: Boolean - Whether or not the player can afford to purchase this upgrade.
* `t`: String - The full name/title of the upgrade.
* `d`: String - The description of the upgrade.
* `s`: Integer - The sprite of the upgrade.
* `l`: Integer - The level of the next upgrade.

----

#### reinc 

**Purpose:** Used to update the details in the Reincarnation dialog.

**Structure:**
```
{
	"type":"reinc",
	"obj":{
		"req":60,
		"skill":"questing",
		"myst":5,
		"exp":5,
		"desc":"Be born anew with a permanent blue star in your top skill! That is 10 skill levels you can never lose and will not count towards your player level. You will start over with new traits and your level, skills, upgrades and myst reset. Your items and everything else will remain. Your level cap will increase by 1 and you will permanently get an experience and myst boost.",
		"able":0
	}
}
```

**Parameters:**

* `obj`: Dictionary - Contains the details needed to update the Reincarnation window. 

**Additional Information:** 
```
{
	"req":60,
	"skill":"questing",
	"myst":5,
	"exp":5,
	"desc":"Be born anew with a permanent blue star in your top skill! That is 10 skill levels you can never lose and will not count towards your player level. You will start over with new traits and your level, skills, upgrades and myst reset. Your items and everything else will remain. Your level cap will increase by 1 and you will permanently get an experience and myst boost.",
	"able":0
}
```

* `req`: Integer - Required level for this player to reincarnate.
* `skill`: String - The skill that this player would get a blue star in if they were to reincarnate.
* `myst`: Integer - The permanent myst percentage bonus that would be added if this character was to reincarnate.
* `exp`: Integer - The permanent experience percentage bonus that would be added if this character was to reincarnate.
* `desc`: String - The message displayed in the dialog.
* `able`: Integer/Boolean - Whether or not the player is able to reincarnate. 1 = can, 0 = can't.

----

#### abc

**Purpose:** Updates the state of the specified ability button.

**Structure:**
```
{
	"type":"abc",
	"i":0
}
```

**Parameters:**

* `i`: Integer - The position of the target ability on the hotbar from right (0) to left

----

#### abl

**Purpose:** Set state of all abilities.

**Structure:**
```
{
	"type": "abl",
	"obj": [
		{
			"spr": 895,
			"cd": 10,
			"c": 1561109449679
		},
		{
			"spr": 909,
			"cd": 60,
			"c": 1551496172371
		},
		{
			"spr": 903,
			"cd": 25,
			"c": 1564426643865
		},
		{
			"spr": 905,
			"cd": 120,
			"c": 1551496175275
		}
	]
}
```

**Parameters:**

* `obj`: Array - Contains a list of abilities to display the hotbar.

**Additional Information:**
```
{
	"spr": 903,
	"cd": 25,
	"c": 1564426643865
}
```

* `spr`: Integer - The sprite of the ability icon.
* `cd`: Integer - The cooldown of the ability, in seconds.
* `c`: Integer - The unix timestamp of the last time the ability was used.

----

#### quest

**Purpose:** Used to update quests in the quest dialog.

**Structure:**
```
{
	"type": "quest",
	"obj": {
		"metal": {
			"name": "So Metal",
			"desc": "Make 10 Copper. Mine Malachite, drop it into a Clay Bowl (dig with a shovel in water for Clay), and place that onto a fire. See what other things you can make!",
			"prog": "0/10"
		},
		"repair": {
			"name": "Fixer Upper",
			"desc": "Build, equip, and use a Repair Kit to repair your walls so others don't break in! Requires some wood, flint, stone, and clay. Get clay by digging in water with a shovel.",
			"prog": ""
		}
	}
}
```

**Parameters:**

* `obj`: Dictionary - Contains dictionaries with quest names as keys with their data as values.

**Additional Information:**
```
"metal": {
	"name": "So Metal",
	"desc": "Make 10 Copper. Mine Malachite, drop it into a Clay Bowl (dig with a shovel in water for Clay), and place that onto a fire. See what other things you can make!",
	"prog": "0/10"
}
```

* `name`: String - Name of the quest.
* `desc`: String - Description of the quest requirements.
* `prog`: String - String containing quest progress.

----

#### skill

**Purpose:** Used to update skill levels in the skill window.

**Structure:**
```
{
  "type": "skill",
	"obj": {
		"destruction": [
		  0,
		  80,
		  80,
		  92
		],
		"hunting": [
		  1,
		  57,
		  57,
		  57
		],
		"exploration": [
		  0,
		  19,
		  25,
		  25
		],
		"questing": [
		  0,
		  20,
		  20,
		  20
		],
		"unarmed": [
		  0,
		  6,
		  6,
		  6
		]
	},
	"tier": 3
}
```

**Parameters:**

* `obj`: Dictionary - Contains skill names and values.

**Additional Information:**
```
"obj": {
	"destruction": [
		0,
		80,
		80,
		92
	],
	"hunting": [
		1,
		57,
		57,
		57
	],
	"exploration": [
		0,
		19,
		25,
		25
	]
}
```

* 1st element: Number of blue stars in that specific skill.
* 2nd element: Skill level capped but unbuffed.
* 3rd element: Skill level capped but buffed.
* 4th element: Skill level uncapped. (unbuffed?)

----

#### cb

**Purpose:** Update state of costume shop UI.

**Structure:**
```
{
	"type":"cb",
	"b":1,
	"pr":10,
	"r":"Not enough diamonds or gifts."
}
```

**Parameters:**

* `b`: Integer - The ID of the costume to unlock.
* `pr`: Integer - The number of premium days (diamonds) the logged in player has remaining.
* `r`: String - The message to display in the costume shop window.

----

#### costumes

**Purpose:** Update list of available costumes.

**Structure:**
```
{
	"type": "costumes",
	"l": {
		"1": 2,
		"2": 10,
		"3": 5,
		"4": 5,
		"5": 15,
		"6": 2,
		"7": 15,
		"8": 10,
		"9": 20,
		"10": 20,
		{ removed for brevity }
		"138": 250,
		"139": 60,
		"140": 50,
		"141": 80,
		"142": 250,
		"143": 60,
		"144": 300,
		"145": 90,
		"146": 90,
		"147": 60,
		"148": 150
	}
}
```

**Parameters:**

`l`: Dictionary - List of costumes IDs and attached prices. A costume's ID is also it's monster sprite ID.

----

#### game

**Purpose:** Set appearance window state based on premium status.

**Structure:**
```
{
  "type": "game",
  "lh": [
    5,
    9,
    10,
    11,
    12,
    14,
    15,
    16,
    17,
    18,
    19,
    20,
    21,
    22
  ],
  "lc": [
    6,
    7,
    8,
    9,
    10,
    12,
    13,
    14,
    15,
    16
  ],
  "lb": [
    9
  ],
  "pr": 0
}
```

**Parameters:**

* `lh`: Array - List of locked hairstyles for non-premium players.
* `lc`: Array - List of locked clothes for non-premium players.
* `lb`: Array - List of locked body/skin options for non-premium players.
* `pr`: Integer - The number of premium days (diamonds) the logged in player has remaining.

----

#### ping

**Purpose:** Seems to be unused by the server. Client uses this packet to calculate the connection speed.

**Structure:**
```
{
	"type":"ping",
	"c":0
}
```

**Parameters:**

* `c`: Integer - Unknown. (Maybe number of pings since first connection?)

---

#### pos

**Purpose:** Update the position of the player on the client.

**Structure:**
```
{
  "type": "pos",
  "x": 46,
  "y": 28
}
```

**Parameters:**

* `x`: Integer - The x coordinate of the player.
* `y`: Integer - The y coordinate of the player.

----

#### hpp

**Purpose:** Update the health bar of an entity.

**Structure:**
```
{
  "type": "hpp",
  "id": 143994,
  "o": 1000,
  "n": 388.4560119912795
}
```

**Parameters:**

* `id`: Integer - The ID of the entity the health bar is attached to.
* `o`: Float - The maximum value of the HP bar.
* `n`: Float - The current value of the HP bar. Bar is displayed with this value as a percentage of the maximum value.

----

#### hpo

**Purpose:** Update the health bar of an object at a specific coordinate.

**Structure:**
```
{
  "type": "hpo",
  "x": 69,
  "y": 8,
  "o": 942.4774774774775,
  "n": 942.4684684684685
}
```


**Parameters:**

* `x`: Integer - The x coordinate where the health bar should be displayed.
* `y`: Integer - The y coordinate where the health bar should be displayed.
* `o`: Float - The maximum value of the HP bar.
* `n`: Float - The current value of the HP bar. Bar is displayed with this value as a percentage of the maximum value.

----

#### t

**Purpose:** Unknown. No recorded usage, likely used to change the template of a target entity by ID.

**Structure:**
```
{
	"type":"t",
	"id": 1,
	"tpl": 1
}
```

**Parameters:**

* `id`: Integer - The ID of the entity who's template to update.
* `tpl`: Integer - The ID of the template to switch to.

----

#### p

**Purpose:** Update the position and state of a player.

**Structure:**
```
{
	"type": "p",
	"id": 55763,
	"tpl": 55763,
	"s": 284,
	"d": 2,
	"x": 48,
	"y": 19,
	"dx": 48,
	"dy": 19
}
```

**Parameters:**

* `id`: Integer - The ID of the entity who's state to update.
* `tpl`: Integer/String - The template of the entity who's state to update.
* `s`: Integer - The speed of the entity.
* `d`: Integer - The direction the entity is facing.
* `x`: Integer - The x coordinate of the entity.
* `y`: Integer - The y coordinate of the entity.
* `dx`: Integer - The derived x coordinate of the entity (?)
* `dy`: Integer - The derived y coordinate of the entity (?)

----

#### inv

**Purpose:** Update the inventory state of the logged in player.

**Structure:**
```
{
  "type": "inv",
  "data": [
    {
      "slot": "0",
      "spr": 580,
      "t": "bone_hammer",
      "eqp": 0,
      "n": "Bone Hammer",
      "qty": 1
    },
    {
      "slot": "1",
      "spr": 703,
      "t": "copper_dagger",
      "eqp": 0,
      "n": "Copper Dagger +1*",
      "qty": 1
    },
    {
      "slot": "2",
      "spr": 826,
      "t": "bandage",
      "eqp": 0,
      "n": "Bandage",
      "qty": 1
    },
    {
      "slot": "3",
      "spr": 507,
      "t": "short_bow",
      "eqp": 0,
      "n": "Short Bow*",
      "qty": 1
    },
    {
      "slot": "4",
      "spr": 236,
      "t": "meal",
      "eqp": 0,
      "n": "Fish Dish",
      "qty": 3
    },
	{
	  "slot": "5",
	  "spr": 767,
	  "t": "aloe_plant",
	  "eqp": 0,
	  "n": "Aloe",
	  "qty": 214
	}
  ]
}
```

**Parameters:**

* `data`: Array - Contains a list of dictionaries for each filled inventory slot.

**Additional Information:**
```
{
	"slot": "5",
	"spr": 767,
	"t": "aloe_plant",
	"eqp": 0,
	"n": "Aloe",
	"qty": 214
}
```

* `slot`: Integer - The inventory slot that the item stack is in.
* `spr`: Integer - The sprite of the item stack.
* `t`: String - The internal name of the item. Used for checking recipes and building.
* `eqp`: Integer/Boolean - Whether or not the item is equipped, or if it's damaged. 0 = unequipped, undamaged. 1 = equipped, undamaged. 2 = equipped, damaged (red border).
* `n`: String - The full display name of the item.
* `qty`: Integer - The number of items in the stack. Note: No indication of whether or not the item is stackable - if an item is unstackable it will always have a quantity of 1.

----

#### bld

**Purpose:** Sent on connect. Update the list of crafting recipes available to the client. 

**Structure:**
```
{
	"type":"bld",
	"data":"[{\\\\\\\"t\\\\\\\":\\\\\\\"aloe_potion\\\\\\\",\\\\\\\"r\\\\\\\":{\\\\\\\"silver\\\\\\\":1,\\\\\\\"aloe_plant\\\\\\\":3,\\\\\\\"mud\\\\\\\":1},\\\\\\\"s\\\\\\\":242,\\\\\\\"n\\\\\\\":\\\\\\\"Healing Potion\\\\\\\",\\\\\\\"p\\\\\\\":1},{\\\\\\\"t\\\\\\\":\\\\\\\"animal_gate\\\\\\\",\\\\\\\"r\\\\\\\":{\\\\\\\"wood\\\\\\\":20},\\\\\\\"s\\\\\\\":949,\\\\\\\"n\\\\\\\":\\\\\\\"Animal Gate\\\\\\\",\\\\\\\"p\\\\\\\":0},{\\\\\\\"t\\\\\\\":\\\\\\\"antidote\\\\\\\",\\\\\\\"r\\\\\\\":{\\\\\\\"silver\\\\\\\":10,\\\\\\\"aloe_potion\\\\\\\":1,\\\\\\\"poison_extract\\\\\\\":10}.......}]"
}
```

**Parameters:**

- `data`: String - Contains stringified JSON for all crafting recipes available to the player.

**Additional Information:**
```
{
	"t": "aloe_potion",
	"r": {
		"silver": 1,
		"aloe_plant": 3,
		"mud": 1
	},
	"s": 242,
	"n": "Healing Potion",
	"p": 1
}
```

- `t`: String - The internal name of the item.
- `r`: Dictionary - Contains key/value pairs for the items and quantities needed to complete the recipe.
- `s`: Integer - The sprite of the recipe.
- `n`: String - The display name of the item.
- `p`: Integer/Boolean - Whether or not the item should be placed in the crafter's inventory and not as a block in front of them. 0 = placed as block, 1 = placed in inventory.

----

#### remove

**Purpose:** Cleanup old entities.

**Structure:**
```
{
	"type":"remove",
	"id":123456
}
```

**Parameters:**

- `id`: Integer - The ID of the entity object to clear from the client.

----

#### object (deprecated)

**Purpose:** Unknown/unused by the client.

**Structure:** 
```
{
	"type":"object"
}
```

**Parameters:** None/unknown.

----

#### tile

**Purpose:** Update the sprite of a specific tile when a floor is placed, crop grows or ground is tilled.

**Structure:**
```
{
	"type": "tile",
	"x": 2,
	"y": 326,
	"tile": 17
}
```

**Parameters:**

* `x`: Integer - The x coordinate of the tile to update.
* `y`: Integer - The y coordinate te of the tile to update.
* `tile`: Integer - The ID of the tile sprite.

----

#### map

**Purpose:** Update surrounding tiles and objects.

**Structure:**
```
{
	"type": "map",
	"x": 46,
	"y": 443,
	"tiles": "21:36:36:36:36:36:36:36:36:36:36:36:36:36:21:36:36:36:36:21:36:21:36:36:36:247:36:36:36:36:36:21:36:21:36:21:36:36:36:36:36:36:36:36:21:21:36:21:21:36:36:325:36:21:21:36:36:36:36:36:36:36:21:36:36:36:36:36:21:36:21:36:36:21:21:21:36:325:36:36:36:36:36:21:36:21:21:36:36:21:36:36:36:36:21:36:36:21:36:36:36:36:21:247:21:36:21:21:36:21:36:21:36:36:36:36:21:36:21:36:36:21:21:36:36:36:36:36:21:325:36:21:36:36:21:21:36:21:36:36:21:36:36:21:21:36:21:36:36:21:36:36:36:36:21:325:21:21:21:36:21:21:21:36:21:36:21:36:36:21:21:21:36:36:36:21:21:21_1:325:36:36:325:21:21:36:36:36:36:21:36:21:36:36:36:36:36:36:36:36:36:21:36:36:325:325:36:36:325:36:36:21:36:36:36:21:36:21:36:36:36:36:36:36:21:36:36:36:21:21:325:325:36:21:325:36:21:36:36:36:36:36:21:36:36:36:36:21:36:21:21:36:36:21:36:36:36:21:36:21:21:21:36:21:36:21:21:21:21:36:21:36:21:36:36:21:21:36:36:36:36:36:36:36:36:36:36:36:21:36:21:36:21:36:36:21:21:36:36:36:36:21:21:21:36:36:36:36:36:21:36:36:36:36:36:36:36:36:21:21:21:36:36:36:21:36:21:36:21:21:36:36:36:21:36:21:21:36:36:21:36:21:36:21:36:21:36:36:36:36:36:21:21:36:36:36:36:21:21:36:21:36:36:21:21:36:36:36:36:36:36:36:36:21:36:21:21:36:21:21:36:21:36:21:36:36:36:21:21:21:36:21:36:36:21:36:36:36:36:36:36:36:36:21:36:21:36:36:36:36:21:36:21:21:36:21:36:36:21:36:36:36:21:21:21:36:21:36:36:36:36:36:36:21:21:21:21:21:36:36:36:36:36:21:36:21:36:21:36:21:36:36:21:36:21:21:36:36:21:21:21:36:21:21:36:36:21:36:21:36:36:36:21:21:36:21:36:21:36:36:21:21:21:36:36:36:36:36:21:36:36:36:21:21:21:21:36:21:36:36:36:36:36:36:36:36:21:36:21:21:36:36:36:21:36:21:21:21:36:36:36:36:21:36:36:36:36:21:21:21:36:36:21:36:36:36:36:36:36:36:21:36:36:21:36:36:36:21:36:36:36:36:21:36:21:36:36:36:21:21:21:21:36:21:21:36:36:36:21:21:36:36:36:36:36:36:21:21:21:36:21:21:21:36:36:21:21:36:36:36:36:36:21:36:36:21:36:36:36:36:36:21:36:21:36:36:21:21:21:36:36:21:36:36:21:21:21:21:36:21:36:36:21:36:36:21:21:21:36:36:36:36:36:36:21:21:36:36:36:36:21:21:36:36:36:21:36:36:21:36:36:21:36:36:21:21:21:21:36:21:36:36:36:21:21:36:36:21:21:36:36:36:21:36:21:21:21:36:36:21:36:36:36:21:36:36:36:36:21:36:36:36:21:21:21:21:36:36:36:21:21:36:21:36:36:36:36:21:21:36:21:36:21:36:36:36:36:36:36:21:36:36:36:36:36:36:21:36:36:21:21:21:36:21:36:21:21:36:36:21:36:36:36:21:21:21:36:36:21:21:36:21:36:36:36:21:36:21:36:21:36:36:21:36:36:21:36:36:21:21:36:21:21:36:36:36:21:21:21:21:36:36:36:36:21:36:36:21:36:21:36:21:36:36:21:36:21:21:36:21:21:36:21:36:21:36:36:36:21:36:36:21:36:36:36:36:21:36:36:36:36:21:36:36:36:36:36:21:21:21:36:21:21:21:36:36:36:21:21:36:36:36:21:21:21:36:36:36:21:36:36:21:36:21:21:21:21:21:36:36:36:36:36:36:36:21:21:36:21:21:21:21:36:21:21:21:21:21:21:36:21:36:21:36:36:21:21:36:36:36:36:36:36:36:36:36:21:36:36:36:36:21:21:21:36:36:21:36:21:36:36:36:36:36:21:36:21:21:36:21:36:36:21:21:36:36:21:21:21:36:21:36:21:21:36:21:21"
}
```

**Parameters:**

* `x`: Integer - The x coordinate of the origin or center of the tiles sent.
* `y`: Integer - The x coordinate of the origin or center of the tiles sent.
* `tiles`: String - Colon-separated list of tiles and objects to be displayed.

**Map format overview:**

_TODO: This section will need to be used as a direct point of reference for the mapping implementation. Please provide as much additional detail as possible._

`tiles` string contains a colon-separated list of tiles and objects. There are a total of 36 columns, each containing 26 tiles (36x26). Tiles are rendered surrounding the provided origin point.

Each tile is an underscore-separated list of values:

* The first value is the sprite ID of the map tile.
* Each following value is a template ID for objects displayed on the map. Template IDs are different for _each login session_ and sent to the client dynamically based on what needs to be rendered around them.
* Because of this, map tile data alone is useless - in order to build a recreation of the map you would need the client's object dictionary up to date as of the time that map packet was sent.
* The origin point is not the exact center of the tile data, but close to it. (please elaborate on this later)

[ FURTHER DETAILS REQUIRED ]

----

#### s

**Purpose:** Update status bars and buff/debuff icons.

**Structure:**
```
{
	"type": "s",
	"h": 100,
	"f": 53.6,
	"p": 1303,
	"k": 11.92,
	"e": 12.701492537313433,
	"t": "destruction",
	"b": [
		{
			"s": 8,
			"t": "Infamy: 19s"
		}
	],
	"u": 0,
	"q": 0
}
```

**Parameters:**

* `h`: Float - The percentage of the player's health remaining.
* `f`: Float - The percentage of the player's fullness remaining.
* `p`: Integer - The amount of myst the player has.
* `k`: Float - The percentage of the player's xp towards the next level of the skill that last gained xp.
* `e`: Float - The percentage of the player's xp towards their next character level.
* `t`: String - The name of the last skill the player gained experience in.
* `b`: Array - An array of dictionaries containing details about the current buffs and debuffs applied to the player.
* `u`: Integer - The sprite of the top item in the stack below the player, to display on the pickup button UI element.
* `q`: Integer - The sprite of the currently equipped weapon or tool, to display on the action button UI element. 

**Additional Information:**
```
{
	"s": 8,
	"t": "Infamy: 19s"
}
```

* `s`: Integer - The sprite of the buff/debuff.
* `t`: String - The display text for the buff/debuff when it's clicked.

----

#### effect (deprecated)

**Purpose:** Unknown/unused by the client.

**Structure:**
```
{
	"type":"effect"
}
```

**Parameters:** None/unknown.

----

#### move (deprecated)

**Purpose:** Calls a non-existant function. Source indicates that it was previously used to set the destination of an entity, potentially so that it's travel could be animated by the client.

**Structure:**
```
{
	"type":"move",
	"id":123456,
	"x":25,
	"y":50
}
```

**Parameters:**

* `id`: Integer - The ID of the entity.
* `x`: Integer - The destination x value of where the entity is travelling.
* `y`: Integer - The destination y value of where the entity is travelling.

----

#### mload (deprecated)

**Purpose:** Unknown/unused by the client.

**Structure:** 
```
{
	"type":"mload"
}
```

**Parameters:** None/unknown.

----

#### pass (deprecated)

**Purpose:** Unknown/unused by the client.

**Structure:**
```
{
	"type":"pass"
}
```

**Parameters:** None/unknown.

----

#### mi (deprecated)

**Purpose:** Unknown/unused by the client.

**Structure:** 
```
{
	"type":"mi"
}
```

**Parameters:** None/unknown.

----

### Client-to-server (referred to as "client messages")

#### client

**Purpose:** Provide the server with information about the client.

**Structure:**
```
{
	"type": "client",
	"ver": "4.9.1",
	"mobile": false,
	"agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/75.0.3770.90 Chrome/75.0.3770.90 Safari/537.36"
}
```

**Parameters:**

* `ver`: String - Client version number.
* `mobile`: Boolean - Whether or not the client is running on a mobile device.
* `agent`: String - The browser's user agent string.

----

#### p

**Purpose:** Ping packet. Sent periodically to check if the client is still connected to the server.

**Structure:**
```
{
	"type":"P"
}
```

**Parameters:** None/unknown.


----

#### login

**Purpose:** Log in, register an account or respawn from death.

**Structure:**
```
// Registration
{
	"type": "login",
	"user": "QmFkYm9w",
	"pass": "Tm90QVJlYWxQYXNzd29yZA==",
	"email": "ZXhhbXBsZUBleGFtcGxlLm9yZw=="
}

// Login
{
	"type": "login",
	"user": "QmFkYm9w",
	"pass": "Tm90QVJlYWxQYXNzd29yZA==",
}

// Respawning after death
{
	"type": "login",
	"data": "/me"
}
```

**Parameters:**

* `user`: String - The provided username, base64 encoded.
* `pass`: String - The provided password, base64 encoded.
* `email`: String - The provided email, base64 encoded.
* `data`: String - A data string indicating a special kind of login. The string "/me" is provided when respawning after death, but it isn't neccesary - as long as the parameter is present the player will respawn.

----

#### guest

**Purpose:** Skip the login or registration process and create a new guest account.

**Structure:**
```
{
	"type":"guest"
}
```

**Parameters:** None/unknown.

----

#### chat

**Purpose:** Send a chat message.

**Structure:**
```
{
	"type":"chat",
	"data":"Hello world!"
}
```

**Parameters**:

* `data`: String - The message to send. Chat messages to specific channels are prefixed with their respective slash command - /b for broadcast chat, /tc for tribe chat, /t <player> for private chat. If no prefix is provided, the message is sent to local chat.

----

#### h

**Purpose:** Change walking direction or stop walking.

**Structure:**
```
{
	"type":"h",
	"x":25,
	"y":50,
	"d":1
}
```

**Parameters:**

* `x`: Integer - The client provided x coordinate of the player when they started moving. Both `x` and `y` must be present for the server to process movement but their values are unchecked.
* `y`: Integer - The client provided y coordinate of the player when they started moving. Both `x` and `y` must be present for the server to process movement but their values are unchecked.
* `d`: Integer - The direction of movement - 0 = north, 1 = east, 2 = south, 3 = west.

----

#### m

**Purpose:** Change facing direction.

**Structure:**
```
{
	"type":"m",
	"x":25,
	"y":50,
	"d":1
}
```

**Parameters:**

* `x`: Integer - The client provided x coordinate of the player. Both `x` and `y` must be present for the server to process rotation but their values are unchecked.
* `y`: Integer - The client provided y coordinate of the player. Both `x` and `y` must be present for the server to process rotation but their values are unchecked.
* `d`: Integer - The direction to face - 0 = north, 1 = east, 2 = south, 3 = west.

----

#### A

**Purpose:** Start actioning.

**Structure:**
```
{
	"type":"A"
}
```

**Parameters:** None/unknown.

----

#### a

**Purpose:** Stop actioning.

**Structure:**
```
{
	"type":"a"
}
```

**Parameters:** None/unknown.

----

#### u

**Purpose:** Use the item in the selected slot.

**Structure:**
```
{
	"type":"u",
	"slot":2
}
```

**Parameters:**

* `slot`: Integer - The slot number of the item to be used, equipped or consumed.

----

#### d

**Purpose:** Drop the item in the selected slot.

**Structure:**
```
{
	"type":"d",
	"slot":2,
	"amt":"all"
}
```

**Parameters:**

* `slot`: Integer - The slot number of the item to be used, equipped or consumed.
* `amt`: String - The amount of items in the stack to drop. If a non-numeric value is provided, the entire stack is dropped.

----

#### g

**Purpose:** Grab the top item in the item stack below the player, if any

**Structure:**
```
{
	"type":"g"
}
```

**Parameters:** None/unknown.

----

#### c

**Purpose:** Assorted actions and dialogue requests - "clicks". Used for activating abilities, buying upgrades, updating the selected mapping tile and most information dialogues.

**Structure:**
```
{
	"type":"c",
	"r":"ab",
	"a":0
}

{
	"type": "c",
	"r": "ub",
	"u": "defense"
}

{
	"type": "c",
	"r": "rn"
}

{
	"type": "c",
	"r": "ap",
	"c": 1,
	"b": 7,
	"h": 2,
	"cc": 14540253,
	"hc": 6504471,
	"ec": 9682175,
	"nc": 16777215
}
```

* `r`: String - The type of special action to perform.
* `a`: Integer - If the type is `ab`, an ability needs to be specified. Abilities are numbered 0-5 from right to left in order of client display.
* `u`: String - If the type is `ub`, this optional parameter can be used to specific the target upgrade to buy.
* `c`: Integer - If the type is `ap`, this parameter is used to specify what style of clothing to change to.
* `b`: Integer - If the type is `ap`, this parameter is used to specify what type of body to change to.
* `h`: Integer - If the type is `ap`, this parameter is used to specify what type of hair to change to.
* `cc`: Integer - If the type is `ap`, this parameter is used to specify the clothing color to change to.
* `hc`: Integer - If the type is `ap`, this parameter is used to specify the hair color to change to.
* `ec`: Integer - If the type is `ap`, this parameter is used to specify the eye color to change to.
* `nc`: Integer - If the type is `ap`, this parameter is used to specify the name color to change to.

**Additional Information:**

There a number of possible values for `r`. Known ones are listed here:

* `ab`: Activate ability
* `up`: Fetch upgrade dialog info.
* `ub`: Purchase an upgrade and fetch upgrade dialog info.
* `rn`: Fetch reincarnation dialog info.
* `re`: Reincarnate the character.
* `st`: Fetch character stat dialog info.
* `sk`: Fetch skill dialog info.
* `hk`: Fetch/show help dialog info (uses fx_tpl).
* `cr`: Fetch/show credit dialog info (uses fx_tpl).
* `ap`: Update character appearance.
* `qs`: Fetch quest dialog info.

----

#### t

**Purpose:** Select a new target. The player will automatically rotate to face the player and action if they are within range.

**Structure:**
```
{
	"type":"t",
	"id":111243
}
``` 

**Parameters:**

* `id`: Integer - The ID of the entity to target.

----
