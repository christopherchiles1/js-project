# Convoy

## Implementation Details
  Convoy is an RPG take on the classic game of Snake. The player controls a convoy of warriors, archers, and mages in an effort to defeat enemy forces surrounding the capitol.

### View
  * ::init - create Images, menu, game
  * .state - game or menu
  * #start - starts render tick (or requestAnimationFrame)
  * #draw()

### Images (singleton)
  * .menubg
  * .gamebg
  * .alphabet
  * .characters (warrior, archer, and mage)
  * .attacks (sword, arrow, and mage animations)
  * .items (gems and powerups)
  * #loadAll - loads all images

### Sprite
  * .image
  * .width
  * .height
  * .positions - array of tuples
  * #draw(pos) - draws the frame at positions[pos]. draws positions[0] if no input

### Menu
  * .options - nested array of MenuOptions
  * .selected - tuple
  * # navigate(dir)
  * # enter - runs the selected MenuOption's callback

### MenuOption
  * .text
  * .position
  * .callback

### Level
  * .enemies - array (queue) of enemy convoys
  * .spawnRate - in milliseconds
  * .difficulty - modifier for enemies
  * .bg - nested array of gamebg Sprites

### Game
  * .level - a level object
  * .player - a Convoy object with one hero
  * .enemies - array of current enemies
  * .items - array of items
  * .attacks - array of attacks
  * #draw - draws level, items, units, then attacks
  * #toggle - toggle tick on and off (pause game)
  * #tick - run all tasks for this tick (listed below)
  * #move - run move on player, enemies, then attacks
  * #collideLeader - run collision detect on player leader (end game)
  * #attack - run attack action for player if attack-tick, then enemies if attack-tick
  * #damage - run collision detect on attacks and damage associated units
  * #over - check if player convoy is empty (end game)
  * #collect - run collect action for player (player collision w/ items)

### Convoy < List
  * .leader - returns the head of the List
  * #turn - tells leader to turn (note: may need to use a queue to avoid unresponsive keypresses)

### Unit < ListNode
  * .frame - used to time attacks and animation (increment on move, mod by ticks/sec)
  * .type - Warrior, Archer, or Mage (defines animation and attack type)
  * .health
  * .range
  * .arc
  * .attackRate
  * .position - tuple
  * .direction - tuple
  * .turn - nested tuple with [pos, dir] of the turn
  * #move - runs updates position based on direction and turn. Passes turn on to next unit if executed
  * #collect - look for items in range and collect them (Note: PLAYER ONLY)
  * #attack - looks for enemies in range and creates an Attack
  * #takeDamage - lower health and remove if dead
  * #dropItem - create item at position with some chance

### Attack
  * .frame - used to time animation (increment on move)
  * .duration - remove attack when duration hits 0 (decrement on move)
  * .position
  * .direction
  * .strength
  * .range
  * .arc
  * #move - update position with direction

### Item (gems and powerups)
  * .position
  * .frame - used to time blinking before disappearing (increment on move)
  * .duration - remove item when duration hits 0 (decrement on move)
  * .callback - runs when item is collected

### Util
  * ::inherits(parent, child) - for object oriented programming
  * ::overlap(obj1, obj2) - returns true/false
  * ::step(position, delta) - returns new position tuple

---

### List
  * .head
  * .tail
  * #append(node)
  * #remove(node)

### ListNode
  * .next
  * .prev
  * .val

Note: Store canvas context globally (CJS.ctx)
