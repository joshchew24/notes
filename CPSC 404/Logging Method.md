# Logging Method
- within [[Write-Ahead Logging Protocol|WAL]] framework, could follow any logging method for [[Crash Recovery]]
## Undo
- maintain log such that to recover from a crash, just undo incomplete transactions
## Redo
- maintain log such that to recover from a crash, just redo committed transactions
## Undo/Redo
- maintain log such that to recover from a crash, redo committed transactions **and** undo incomplete ones
- offers most flexibility