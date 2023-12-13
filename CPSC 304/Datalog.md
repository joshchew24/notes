# Datalog
- if i know the thing on the right, I know the thing on the left
![[Pasted image 20231213011604.png]]
## Projection
1. find the name of all products
	- `Ans(N) :- Product(P, N, PR, C, M)`
2. find the countries of all the companies
	- `Ans1(Co) :- Company(C, N, S, Co)`
	- make sure C =/= Co
## Selection
1. find all purchases with same buyer and seller
	- `Ans1(B,B,S,P) :- Purchase(B,B,S,P)
2. find all canadian companies
	- `Ans2(C,N,S,'Canada') :- Company(C,N,S,'Canada')`
	- `Ans3(C,N,S,Co) :- Company(C,N,S,Co), Co='Canada'`
		- don't do it this way
3. find purchases of products with pid 42
	- RA: $\sigma_{pid=42}(Purchase)$ 
	- Datalog: `Ans(B,S,St,42) :- Purchase(B,S,St,42)`
4. Find all products over $99.99
	- RA: $\sigma_{price>99.99}(Product)$
	- Datalog: `Ans(P,N,Pr,C,M) :- Product(P,N,Pr,C,M), Pr > 99.99`
5. Find all English companies with stock prices less than $100
	- RA: $\sigma_{stock price < 100}(Company)\cup\sigma_{country = England}(Company)$
	- Datalog: `Ans(I,N,S,'England') :- Company(I,N,S,'England'), S < 100`

## Selection and Projection
1. Find the names of all products over $99.99
	- RA: $\pi_{name}(\sigma_{price > 99.99}(Product))$
	- Datalog: `Ans(N) :- Product(I,N,P,C,M), P > 99.99`

## Joins
- performed using same variable in different relations
1. Find store names where Fred bought something
	- RA: $\pi_{store}(\sigma_{name='Fred'}(Person)\bowtie_{sin=buyer-sin}Purchase)$
	- Datalog: `S(N) :- Person(S,"Fred",T,C), Purchase(S,L,N,P)`
		- same variable means results will have the same value for that attribute in both relations

## Anonymous Variables
- each `_` means a fresh, new variable
- `Ans(N) :- Person(S,N,_,_), Purchase(S,_,"Gizmo Store",_)`
- we only see the attributes we care about
1. find the names of all products
	- `Ans(N) :- Product(_,N,_,_,_)`
2. Find the names of people who have bought products from themselves
	- RA: $\pi_{name}(\sigma_{buyer-sin=seller-sin}(Purchase)\bowtie_{sin=buyer-sin}Person$
	- `Ans1(B) :- Person(B,N,_,_), Purchase(B,B,_,_)`
## Multiple Datalog Rules
1. Find names of people that are either buyers or sellers
```prolog
A(N) :- Person(S,N,A,B), Purchase(S,C,D,E)
A(N) :- Perosn(S,N,A,B), Purchase(C,S,D,E)
```


## Exercises
1. Find SINs of people who bought Products in the "computers" category
	- RA: $\pi_{buyer-sin}(Purchase\bowtie \sigma_{category='computers'}(Product))$
		- natural join??
	- `Ans(B) :- Purchase(B,_,_,P), Product(P,_,_,'computers',_)`
2. Find the SIN of people who bought Canadian products
	- RA: $\pi_{buyer-sin}(Purchase \bowtie Product \bowtie_{maker-cid = cid} \sigma_{country = 'Canada'}(Company))$
		- natural join??
	- `Ans(B) :- Purchase(B,_,_,P), Product(P,_,_,_,C), Company(C,_,_,'Canada')`
3. Find names of people who bought products sold by a Canadian company that cost under $50
	- RA: $\pi_{name}(People \bowtie_{sin=buyer-sin} Purchase \bowtie_{pid=pid} \sigma_{price < 50}(Product) \bowtie_{maker-cid=cid} \sigma_{country = 'Canada'}(Company))$
	- `Ans(N) :- People(B,N,_,_), Purchase(B,_,_,P), Product(P,_,Pr,_,C), Company(C,_,_,'Canada'), Pr < 50`
4. Consider `Unknown(o,d)`
	- ![[Pasted image 20231213021032.png]]
	- Compute `Secret(A,B) :- Unknown(B,A), Unknown(C,A), C &ne; B`
	- Ans: Find the case where there are two different o's with the same d