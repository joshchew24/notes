---
aliases:
  - Intersect
---
# Intersect Evaluation
- just natural join on all attributes
- e.g. $R(A,B,C) \cap S(A,B,C) \equiv R \bowtie S$
- e.g. $R(A,B,C) \cap S(D,E,F) \equiv \pi_{A,B,C}( R \bowtie_{(R.A = S.D) \land (R.B = S.E) \land (R.C = S.F)} S)$