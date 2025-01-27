# Project 1 DSL Examples
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
		water_monkey
			health = 200
			speed = 10
			type = water
		fire_monkey
			health = 50
			speed = 30
			type = fire
		earth_monkey
			health = 500
			speed = 1
			type = earth
		air_monkey
			health = 75
			speed = 20
			type = air
	start waves
		wave 1
			basic_monkey 5


```