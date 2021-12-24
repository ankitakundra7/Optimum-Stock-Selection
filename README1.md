**Problem Overview:**

Equity	money	management	strategies	are	largely	classified	as	either	‘active’	or	‘passive’.	The	most	common	
passive	strategy	is	that	of	“indexing” where	the	goal	is	to	choose	a	portfolio	that	mirrors	the	movements	of	
the	broad	market	population	or	a	market	index.	Such	a	portfolio	is	called	an	index	fund.	For	example,	the	QQQ	
Index	fund	tracks	the	NASDAQ-100	index.	
Constructing	an	index	fund	that	tracks	a	specific	broad	market	index	could	be	done	by	simply	purchasing	all	n	
stocks	in	the	index,	with	the	same	weights	as	in	the	index.	However, this	approach	is	impractical	(many	small	
positions)	and	expensive	(rebalancing	costs	may	frequently	be	incurred,	price	response	to	trading).	An	index	
fund	with	m stocks,	where	m is	substantially	smaller	than	the	size	of	the	target	population, n,	seems	desirable.	
In	this	project,	we	will	create	an	Index	fund	with	m stocks	to	track	the	NASDAQ-100	index.	We	will	do	this	in	
multiple	steps.	First,	we	will	formulate	an	integer	program	that	picks	exactly	m out	of	n	stocks	for	our	
portfolio.	This	integer	program	will	take	as	input	a	‘similarity	matrix’,	which	we	will	call	�.	The	individual	
elements	of	this	matrix,	�!", represent	similarity	between	stock	i	and	j.	An	example	of	this	is	the	correlation	
between	the	returns	of	stocks	i	and	j.	But	one	could	choose	other	similarity	metrics	�!".
Next,	you will	solve	a	linear	program	to	decide	how	many	of	each	chosen	stock	to	buy	for	your portfolio and	
finally	evaluate	how	well	your	index	fund	does	as	compared	to	the	NASDAQ-100	index,	out	of	sample. You	
will	examine	the	performance	for	several	values	of	m.
Stock Selection
The	binary	decision	variables	yj	indicates	which	stocks	j	from	the	index	are	present	in	the	fund	(yj	=	1	if	j	is	
selected	in	the	fund,	0	otherwise).	For	each	stock	in	the	index,	i	=	1,	.	.	.	,	n,	the	binary	decision	variable	xij	
indicates	which	stock	j	in	the	index	is	the	best	representative	of	stock	i	(xij	=	1	if	stock	j	in	index	is	the	most	
similar	stock	i,	0	otherwise).	
The	first	constraint	selects	exactly	m stocks	to	be	held	in	the	fund.	The	second	constraint	imposes	that	each	
stock	i	has	exactly	one	representative	stock	j	in	the	index.	The	third	constraint	guarantees	that	stock	i	is	best	
represented	by	stock	j	only	if	j	is	in	the	fund.	The	objective	of	the	model	maximizes	the	similarity	between	the	
n	stocks	and	their	representatives	in	the	fund.	
Another	way	of	thinking	is	that,	you	have	two	sets.	Set	I	has	all	stocks	in	the	index.	Set	F	has	fund	stocks.	You	
want	to	create	a	link	that	maps	elements	of	set	I	to	elements	of	set	F.	The	first	constraint	in	the	formulation	
above	is	equivalent	to	saying	that	not	more	than	q	elements	of	F	can	be	mapped	from	set	I.	The	second	
constraint	suggests	that	each	element	in	the	set	I	will	map	to	single	element	in	set	F	.	The	third	constraint suggests that if an element from set I gets mapped to an element in set F, then the element of set F better be 
present in the fund. The binary constraint on xij indicates if a link between element i in set I to element j in set 
F exists. The weight on that link is the correlation ρij between stock i in set I and stock j in set F. Many such 
mappings which satisfy the above constraints exist. Your objective gives you the best mapping. This is the 
basic idea of bipartite matching.  



Problem Overview

Equity money management strategies are largely classified as either ‘active’ or ‘passive’. The most common passive strategy is that of “indexing” where the goal is to choose a portfolio that mirrors the movements of the broad market population or a market index. Such a portfolio is called an index fund. For example, the QQQ Index fund tracks the NASDAQ-100 index.

Constructing an index fund that tracks a specific broad market index could be done by simply purchasing all n stocks in the index, with the same weights as in the index. However, this approach is impractical (many small positions) and expensive (rebalancing costs may frequently be incurred, price response to trading). An index fund with m stocks, where m is substantially smaller than the size of the target population, n, seems desirable.

In this project, we will create an Index fund with m stocks to track the NASDAQ-100 index. We will do this in multiple steps. First, we will formulate an integer program that picks exactly m out of n stocks for our
portfolio. This integer program will take as input a ‘similarity matrix’, which we will call 𝜌. The individual elements of this matrix, 𝜌!", represent similarity between stock i and j. An example of this is the correlation between the returns of stocks i and j. But one could choose other similarity metrics 𝜌!".

Next, you will solve a linear program to decide how many of each chosen stock to buy for your portfolio and finally evaluate how well your index fund does as compared to the NASDAQ-100 index, out of sample. You will examine the performance for several values of m.
![image](https://user-images.githubusercontent.com/65372245/147303949-c72a9995-9a11-4f39-9ba0-08420107c293.png)

