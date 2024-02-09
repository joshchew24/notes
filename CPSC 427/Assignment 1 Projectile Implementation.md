# Assignment 1 Projectile Implementation

- on key, spawn a projectile at chicken's pos with chicken's velocity
- step
	- advance projectile motion (should already be implemented)
	- check if bullets collide with any eagles or bugs
	- update `WorldSystem::handle_collision()` to check if `projectile` has collided with an NPC entity
		- if `eatable`, lose score? play a bad sound? spawn an extra eagle
		- if `deadly`, gain score
- render...
	- make a texture
	- try and render it idrk