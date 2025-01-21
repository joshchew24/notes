# Modular Design Draft
## Rich Features
### Dynamic Enemy Waves
- the DSL user can choose to statically declare the composition of a wave
- alternatively, use loops and conditionals to determine the wave composition based on the players' tower placements
- **this language feature uses mutable variables, loops, conditionals**
#### Example 1: Static
```
wave 1 {
	enemies: [
		slime 10
		zombie 5
	]
}
```
#### Example 2: Simple Loop
```
wave 1 {
	enemy_budget = 1000
	dynamic_enemies: {
		loop (enemy_budget > 0) {
			add_enemy(slime)
			enemy_budget -= slime.cost
		}
	}
}
```
#### Example 3: Simple Dynamic Generation
```
wave 1 {
	enemy_budget = 1000
	// tower_costs is determined at runtime, is the total cost of towers placed by player
	enemy_budget += tower_costs * 2
	dynamic_enemies: {
		loop (enemy_budget > 0) {
			add_enemy(slime)
			enemy_budget -= slime.cost
		}
	}
}
```
#### Example 4: Dynamic Generation
```
wave 1 {
	enemy_budget = 1000
	// tower_costs is determined at runtime, is the total cost of towers placed by player
	enemy_budget += tower_costs * 2
	dynamic_enemies: {
		loop (enemy_budget > 0) {
			add_enemy(slime)
			enemy_budget -= slime.cost
		}
	}
}
```
#### Example 5: Complex Wave
```
wave 1 {
	enemies: [
		slime 10
		zombie 5
	]
	enemy_budget = 1000
	// tower_costs is determined at runtime, is the total cost of towers placed by player
	enemy_budget += tower_costs * 2
	dynamic_enemies: {
		loop (enemy_budget > 0) {
			add_enemy(slime)
			enemy_budget -= slime.cost
		}
	}
}
```
### On-tick Effect Functions
- the user can create on-hit effects that are applied by tower attacks
- the on-hit effect function will be called once per game tick, and cleared after three(?) ticks
#### Example 1: Burn
```
tower "ArrowTower" {
	base_damage 5;
	range 10;
	onhit burn(base_damage);
}

effect burn(base_damage) {
	damage = base_damage * 0.2
	target.health -= damage
}
```
#### Example 2: Freeze
```
effect freeze() {
	target.speed = 0.5
}
```
## Modules
### Lexer and Parser
#### Input
- DSL
```

```
#### Output
- Parse Tree (ANTLR)
### Parse Tree to AST Converter
#### Input
- Parse Tree (ANTLR)
#### Output
- AST
- subtrees
	- basic game rules
		- path/walls
		- win/loss conditions
	- tower definitions
		- damage
		- cost
		- range
		- type
		- on-hit effect
	- effect definitions
	- enemy definitions
	- wave definitions
		- dynamic wave generation procedure
```
```
### Tower Definer
#### Input
- AST (tower subtree)
#### Output
- map of tower name to tower_definition object
- there will be a tower factory function that consumes the tower_definition object, and uses the parameters to create instances of a tower object 
##### Example
```
tower_definition = (cost, damage, range, on_hit_effect_func)

func tower_factory(tower_definition) {
	// return a tower instance
}
```

### Effect Definer
#### Input
* AST (effect subtree)
#### Output
- functions that get called per game tick
### Enemy Definer
#### Input
- AST (enemy subtree)
#### Output
- enemy definitions
- similar to tower definer
### Tower Placer
#### Input
- tower dictionary (from tower definer)
- user should have be able to see what tower options are available
#### Output
- user should be able to place towers in the game
### Wave Setup
#### Input
- AST (wave definition subtree)
#### Output
- wave definition object
	- contains order and number of enemies to spawn
### Game Loop
#### Input
- nothing (?)
#### Output
- tick the game state