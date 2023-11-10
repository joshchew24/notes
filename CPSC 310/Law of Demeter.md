---
tags:
  - cpsc310
---

- The Law of Demeter states that a program component should have limited information about other components in the system. These violations are usually easy to spot and arise when a function 'reaches into' another function to tell it how to behave. For example, if a method called `db.getResults().sort().print()`, not only does it know about `db` but it also needs to know about the return types to `getResults()` and `sort()` in order to call methods on them. The most straight forward way to avoid Law of Demeter violations is to not call methods on objects returned by another method.