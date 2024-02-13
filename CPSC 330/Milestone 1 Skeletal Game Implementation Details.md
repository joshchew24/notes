# Milestone 1 Skeletal Game Implementation Details

my current idea for how this will work:

- new component `Boundary` or something like that
	- walls and floors are all entities with `Boundary` components
	- Gravity system somehow need to check these collisions to stop moving entities downwards
		- player should still move downwards if they're colliding with a `Boundary` to their left/right/above
	- consist of two points to form a straight line
		- could possibly extend this to have multiple points to support free-form drawings?

- `Motion` component has a new `acceleration` attribute
	- left/right motion will set the acceleration
		- there is friction to slow a player when the movement key is no longer pressed
	- jump will briefly accelerate the player upwards
		- gravity system will immediately decelerate them to bring them downwards
			- until they hit a `Boundary` beneath them

- levels will be some form of serialized data that 