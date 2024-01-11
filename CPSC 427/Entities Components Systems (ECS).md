# Entities Components Systems (ECS)

Entities with Components controlled by Systems
## Entities
- anything in the game, just an ID (and name)
	- e.g. player, enemy, tile, bullet, sound, etc.
- entities have AT MOST a single instance of a component
## Components
- properties attached to entities
	- e.g. position, texture, health, gravity
- purely data (structs)
## Systems
- code that drives behaviour based on components
	- e.g. movement, rendering, sound, physics