---
tags:
  - cpsc310
  - ocp
  - solid
---

class must be closed for internal change, but open to extension

superclass should not require modification when new specialized behaviour is desired

e.g. superclass site contains getBillableAmount method that returns the amount based off "base" and "tax"
subclass residentialSite and Lifeline site calculate this amount differently. superclass should contain getBaseAmount and getTaxAmount used by getBillableAmount, and subclasses can override the helpers to extend the behaviour for their specific use case

red flag: making behavioural changes based on internal flags or "instanceof"
