# Project 1 DSL Examples
#question can we include html colors in the DSL?
## Example 1
```
start game
	start rules
	start towers
	start enemies
		basic_monkey
			health = 100
			speed = 15
			type = normal
			color = brown
		water_monkey
			health = 200
			speed = 10
			type = water
			color = blue
		fire_monkey
			health = 50
			speed = 30
			type = fire
			color = red
		earth_monkey
			health = 500
			speed = 1
			type = earth
			color = green
		air_monkey
			health = 75
			speed = 20
			type = air
			color = white
	start waves
		wave 1
			10 basic_monkey
		wave 2
			1 water_monkey
			1 fire_monkey
			1 earth_monkey
			1 air_monkey
		wave 3
			10 basic_monkey
			dynamic {
				budget = 1000
				budget += 2 * placed_tower_values
				loop(budget > 0) {
					enemy = random(water_monkey, fire_monkey, earth_monkey, air_monkey)
					budget -= enemy.health * enemy.speed
				}
			}
		wave 4
				

```