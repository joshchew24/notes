# Modular Design Draft
## Rich Features
### Dynamic Enemy Waves
- the DSL user can choose to statically declare the composition of a wave
- alternatively, use loops and conditionals to determine the wave composition based on the players' tower placements
#### Example 1: Static
```
wave 1 {
	enemies: [
		slime 10
		zombie 5
	]
}
```
#### Example 2: Simple Dynamic
```
wave 1 {
	enemy_budget = 1000
	// tower_costs is determined at runtime, is the total cost of towers placed by player
	dynamic_enemies: {
		enemy_budget += tower_costs * 2
		loop (enemy_budget > 0) {
			add_enemy(slime)
			enemy_budget -= 
		}
	}
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
		- special
```
```
### Tower Definer
###