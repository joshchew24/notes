# CHECKIN
## Check-in 1
### Tower Defense Game Development

We decided to create a DSL for creating a Tower Defense Game. The target user of the DSL would be a game developer/designer. The DSL will be for writing game rules, levels, and tower/enemy types. The three rich features and an example program are below.

1. Defining game rules

   - how many levels are there
   - what is the map layout (what paths are valid for enemies and what are valid tower placement locations)
   - how many waves are there
   - what enemies compose each wave
   - what is the win/lose condition

2. Defining individual tower types and their logic

   - range
   - attack type
   - attack damage
   - attack speed
   - targeting behaviour
     - we will provide some functions for the user to use when defining custom targeting logic.
       - e.g. for enemies in range, shoot target with min distance to self
       - e.g. for all enemies in range, apply burn effect
       - e.g. for two random enemies in range, move them back to their previous location

3. Defining enemy types and pathfinding logic
   - speed
   - health
   - tower attack type resistances and weaknesses
   - pathfinding behaviour
     - conditional on map layout
       path selection at a junction based on different conditions
       - e.g. number of towers on each path
       - e.g. damage taken
       - e.g. shortest path, longest path
     - e.g. speed up when more damage taken

This is a very basic example of what a game defined by this DSL might look like.

```{}
level "MyFirstLevel" {

    // Define Towers
    tower "ArrowTower" {
        baseDamage: 3;
        cost: 50;
        range: 2.5;
        targeting: <targeting logic>;
    }

    tower "CannonTower" {
        baseDamage: 5;
        cost: 75;
        range: 1.8;
        splashRadius: 1.0;
        targeting: <targeting logic>;
    }

    // Define Enemies
    enemy "BasicEnemy" {
        health: 20;
        speed: 1.0;
        pathfinding: <pathfinding logic>;
    }

    enemy "FastEnemy" {
        health: 10;
        speed: 2.0;
        pathfinding: <pathfinding logic>;
    }

    // Define Path
    path "mainPath" {
        nodes: [ (0,0), (5,0), (5,5), (10,5) ];
    }

    // Define Waves
    wave 1 {
        timeStart: 5s;            // The wave starts 5 seconds into the level
        enemies: 5 "BasicEnemy";  // 5 BasicEnemies spawn
        path: "mainPath";
    }

    wave 2 {
        timeStart: 15s;                      // 15 seconds into the level
        enemies: 5 "BasicEnemy", 3 "FastEnemy";
        path: "mainPath";
    }

    // Win/Lose Conditions
    victoryCondition: "AllWavesCleared";
    loseCondition: "BaseHealth <= 0";
}
```

Here is an example of the targeting logic:

```{}
// Example logic for targeting the enemy closest to the tower
target = None
target_distance = range
for enemy in range:
    if distance(self, enemy) <= target_distance:
        target = enemy
shoot target
```
## Check-in 2
### Modular Design and Responsibilities

| Module                                                                  | Person |
| ----------------------------------------------------------------------- | ------ |
| Module 1 - DSL Tokenization and DSL Parsing                             | Josh   |
| Module 2 - Convert tokens to JSON AST                                   | Byron  |
| Module 3 - Convert JSON AST to Game Objects (Tower, Enemy)              | Julian |
| Module 4 - Convert JSON AST to Game Rules (Dynamic Waves, AttackEffect) | Chris  |
| Module 5 - Game Logic (Model)                                           | Ian    |
| Module 6 - Game Logic (View)                                            | Ian    |
| Module 7 - Game Logic (Controller)                                      | Ian    |
### Sync-up/Communication
- we meet after every class and use Discord to communicate
- we plan calls if we need to meet more
- finish before it is due
### Timeline
#### Jan 22 - Jan 29
- define data interfaces between modules
- complete MVC architecture
#### Jan 29 - Feb 15
- implement modules and unit test
#### Feb 5 - Feb 12
- integrate modules and test integration and end-to-end
#### Feb 12 - Feb 13
- try to finish everything by the 12th (last check-in)
#### Feb 14 (valentines)
- no plans
#### Feb 15 - 21 (reading week)
- if we suck, we have to work on the project 
