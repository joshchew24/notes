---
tags:
  - lsp
  - solid
  - cpsc310
---
# Liskov Substitution Principle

Post-Conditions can widen (loosen) but cannot narrow (strengthen)
i.e. the subclass can accept a wider range of inputs than the superclass.
i.e. if the subclass accepts a narrower range of inputs, then it violates LSP
e.g. A Penguin accepts a narrower range of inputs than a Bird, because it would not implement the fly method [(1)](#sources)

Pre-conditions can narrow (strengthen) but cannot loosen (widen)
i.e. the output range of a subclass are a subset of the superclass's outputs
i.e. if the subclass outputs something outside of the output range of the superclass, it violates LSP
e.g. a Square’s setWidth operation produces a wider range of outputs than a Rectangle’s because it changes both width and height [(1)](#sources)
e.g. if a Doctor’s bookAppointment operation accepted hours between 9am and 5pm, then a Specialist subtype of Doctor would be violating the LSP by implementing its bookAppointment operation as only permiting hours of 10am to 2pm, hence narrowing the range of acceptable inputs [(1)](#sources)

### sources
[1](obsidian://open?vault=josh_notes&file=LiskofHappySad_ICSE-SEET_2018.pdf)