# Grammar example
## Input Language
```
Hello!
make me a circle called Fido please
make me a square called Biff please
connect Fido to Biff please
Thanks!

makes 

digraph G {
	Fido[shape=circle]
	Biff[shape=square]
	Fido->Biff
}

or

Hello!
make me a dgfhsldfj called nico please
make me a triangle called oksana please
connect nico to oksana please
Thanks!

# is this valid? 
```

```
PROGRAM: 'Hello!' INSTRUCTION* 'Thanks!' ;
INSTRUCTION: MAKE | CONNECT ;
MAKE: 'make me a ' SHAPE ' called ' NAME ' please' ;
CONNECT: 'connect ' NAME ' to ' NAME ' please' ;
SHAPE: 'circle' | 'square' | 'triangle' ;
	NAME: TEXT ;
TEXT: [a-zA-Z]* ;
```