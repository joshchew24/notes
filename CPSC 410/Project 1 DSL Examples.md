# Project 1 DSL Examples
#question can we include html colors in the DSL?
#question do we want to let the user define types and counters in DSL? imo yes...
#question should we surround all "user definable input" with quotations? or some special prefixes?
## Example 1
```
start game
	start rules
		types = [
			"normal", "water", "fire", "earth", "air"
		]
		counters: "water" -> "fire" -> "air" -> "earth" -> "water"
	end rules
	start towers
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
	start waves
		wave 1
			10 "basic_monkey"
		wave 2
			1 "water_monkey"
			1 "fire_monkey"
			1 "earth_monkey"
			1 "air_monkey"
		wave 3
			10 "basic_monkey"
			dynamic {
				// prefix custom mutable variables with an underscore
				_budget = 1000
				_budget += 2 * placed_towers.costs
				loop(_budget > 0) {
					enemy = random("water_monkey", "fire_monkey", "earth_monkey", "air_monkey")
					_budget -= enemy.health * enemy.speed
				}
			}
		wave 4
			dynamic {
				_fire_budget = 5 * placed_towers.count(type="fire")
				_water_budget = 10 * placed_towers.count(type="water")
				loop(_fire_budget > 0 AND _water_budget > 0)
					enemy = random(distribution=e"water_monkey", "fire_monkey")
					if enemy.type == "water"
						_water_budget -= enemy.health * enemy.speed
					if enemy.type == "fire"
						_fire_budget -= enemy.health * enemy.speed
			}
		wave x
			dynamic {
				_budget = 5000
				loop(_budget > 0)
					// choose random enemy based on type distribution of placed towers
					enemy = random_type_counter()
					_budget -= enemy.health * enemy.speed
			}

```