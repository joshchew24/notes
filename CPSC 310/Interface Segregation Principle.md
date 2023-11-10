---
tags:
  - isp
  - solid
  - todo
  - cpsc310
---
clients should not be required to depend on methods they do not need
no implementation should be forced to provide methods that do not fit into their abstractions

#todo upload pdf instead of using screenshots (lec slides 07)
e.g.![[Pasted image 20231110025610.png]]
BakeryInterface is a good design for BakeryApp but violates ISP if used for CoffeeShopApp.
![[Pasted image 20231110025647.png]]