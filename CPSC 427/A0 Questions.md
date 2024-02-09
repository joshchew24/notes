# A0 Questions
- is the ComponentContainer maintaining a list of objects? a list of pointers to objects? but insert is passed by value not reference
	- can I copy the object to the heap and save the address?
	- how does this work with templates?
- for associating components to entities, is this a map?
	- maintaining parallel vectors (seems like a bad idea)
	- <mark style="background: #FF5582A6;">if using a map, are the objects copied between the component list or shared by reference?</mark>
		- need to know this to implement remove