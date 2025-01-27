# Project 1 DSL Examples
#question can we include html colors in the DSL?
#question do we want to let the user define types and counters in DSL? imo yes...
#todo explain random function
#todo explain specific design decisions: quotes for user-defined input, underscore prefix for mutable variables
#question start/end vs wrapping in braces
#todo list out all members of placed_towers, enemy(?)
#question is our wave generation complex enough?
#todo flesh out the game rules section

## Example 1
```
start game
	start rules
		types = "normal", "water", "fire", "earth", "air"
		counters: "water" -> "fire" -> "air" -> "earth" -> "water"
		player_health = 100
		enemy_damage = 1
	end rules
	start effects
		burn(_damage) {
			target.health -= _damage * 0.5
		}
		slow() {
			target.speed *= target.speed * 0.8
		}
		stun() {
			target.speed = 0
		}
		push(_dist) {
			target.posn -= _dist
		}
	end effects
	start towers
		"basic_tower" {
			cost = 100
			damage = 15
			cooldown = 1
			speed = 20
			splash = 0
		}
		"fire_tower" {
			cost = 200
			damage = 20
			cooldown = 3
			speed = 25
			splash = 2
			type = "fire"
			effect = burn(damage)
			effect_duration = 5
		}
		"water_tower" {
			cost = 300
			damage = 10
			cooldown = 5
			speed = 10
			splash = 5
			type = "water"
			effect = slow()
			effect_duration = 5
		}
		"earth_tower" {
			cost = 400
			damage = 30
			cooldown = 10
			speed = 15
			splash = 3
			type = "earth"
			effect = stun()
			effect_duration = 5
		}
		"air_tower" {
			cost = 500
			damage = 5
			cooldown = 15
			speed = 30
			splash = 4
			type = "air"
			effect = push(3)
			effect_duration = 1
		}
	end towers
	start enemies
		"basic_monkey" {
			health = 100
			speed = 15
			type = "normal"
			color = brown
		}
		"water_monkey" {
			health = 200
			speed = 10
			type = "water"
			color = blue
		}
		"fire_monkey" {
			health = 50
			speed = 30
			type = "fire"
			color = red
		}
		"earth_monkey" {
			health = 500
			speed = 1
			type = "earth"
			color = green
		}
		"air_monkey" {
			health = 75
			speed = 20
			type = "air"
			color = white
		}
	end enemies
	start waves
		wave 1
			10 "basic_monkey"
		wave 2
			1 "water_monkey"
			1 "fire_monkey"
			1 "earth_monkey"
			1 "air_monkey"
		wave 3 // random generation with uniform distribution
			10 "basic_monkey"
			dynamic {
				// prefix custom mutable variables with an underscore
				_budget = 1000
				_budget += 2 * placed_towers.costs
				random = "water_monkey", "fire_monkey", "earth_monkey", "air_monkey"
				loop(_budget > 0) {
					enemy = random() // no value for distribution parameter means uniform
					_budget -= enemy.health * enemy.speed
				}
			}		
		wave 4 // random generation with simple distribution
			dynamic {
				_budget = 10000
				random = "water_monkey", "fire_monkey", "earth_monkey", "air_monkey"
				loop(_budget > 0)
					enemy = random(0.75, 0.05)
					// the first two values get assigned to first two items, rest distributed
					_budget -= 2 * enemy.health
			}
		wave 5 // random generation with separate type budgets
			dynamic {
				_fire_budget = 5 * placed_towers.count(type="fire")
				_water_budget = 10 * placed_towers.count(type="water")
				random = "water_monkey", "fire_monkey"
				loop(_fire_budget > 0 || _water_budget > 0)
					enemy = random()
					if enemy.type == "water" AND _water_budget > 0
						_water_budget -= enemy.health * enemy.speed
					if enemy.type == "fire" AND _fire_budget > 0
						_fire_budget -= enemy.health * enemy.speed
			}
		wave 6 // random generation based on tower type distribution
			dynamic {
				total = placed_towers.count() // no parameter means all
				_w = placed_towers.count(type="water") / total
				_f = placed_towers.count(type="fire") / total
				_e = placed_towers.count(type="earth") / total
				_a = placed_towers.count(type="air") / total
				_budget = 10000
				random = "water_monkey", "fire_monkey", "earth_monkey", "air_monkey"
				loop(_budget > 0)
					enemy = random(_w, _f, _e, _a)
					_budget -= 10 * enemy.speed
			}

		wave 6 // alternative
			dynamic {
				_budget = 5000
				loop(_budget > 0)
					// autochoose random enemy based on type distribution of placed towers
					enemy = random_type_counter()
					_budget -= enemy.health * enemy.speed
			}
	end waves
end game

```