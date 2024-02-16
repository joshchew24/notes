# Milestone 1 Skeletal Game Implementation Details

my current idea for how this will work:

- new component `Boundary` or something like that
	- walls and floors are all entities with `Boundary` components
	- Gravity system somehow need to check these collisions to stop moving entities downwards
		- player should still move downwards if they're colliding with a `Boundary` to their left/right/above
		- maybe an additional `is_floor` flag or seperate `Floor` and `Wall` components? 
			- this removes extensibility for using this component to detect collisions with drawn entities
	- consist of two points to form a straight line
		- could possibly extend this to have multiple points to support free-form drawings?

- `Motion` component has a new `acceleration` attribute
	- left/right motion will set the acceleration
		- there is friction to slow a player when the movement key is no longer pressed
	- jump will briefly accelerate the player upwards
		- gravity system will immediately decelerate them to bring them downwards
			- until they hit a `Boundary` beneath them

- levels will be some form of serialized data 
	- list of `Boundary`s to generate the wall/floor entities
	- collection of textures?
		- use sprites for player and other character models
	- we may be able to just render lines for each `Boundary` 
		- good for now, can make it look nicer for the final game. will give the "hand-drawn" aesthetic

## Input system
- press key
- InputSystem adds key to set of pressed keys while it is held
- InputSystem iterates through its set and calls the respective handlers
- release key
- InputSystem removes key from set
- InputSystem iterates calls the respective release handler
```cpp
struct input_event {
	int key;
	int action;
	bool hold_to_repeat;
};

class InputSystem {
private:
	std::
}
```
## Movement System
- MovementSystem remembers most recently pressed keys
```cpp
bool left;
bool right;
int last;