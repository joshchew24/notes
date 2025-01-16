# Grammar example
## Input Language
```
Hello!
make me a circle called Fido please
make me a square called Biff please
connect Fido to Biff please
Thanks!

digraph G {
	Fido[shape=circle]
	Biff[shape=circle]
	Fido->Biff
}
```

```
program: hello instruction+ thanks
hello: 'Hello!'
instruction: (make | connect)*

```