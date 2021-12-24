**Problem Overview**

Equity money management strategies are largely classified as either ‘active’ or ‘passive’. The most common passive strategy is that of “indexing” where the goal is to choose a portfolio that mirrors the movements of the broad market population or a market index. Such a portfolio is called an index fund. For example, the QQQ Index fund tracks the NASDAQ-100 index.

Constructing an index fund that tracks a specific broad market index could be done by simply purchasing all n stocks in the index, with the same weights as in the index. However, this approach is impractical (many small positions) and expensive (rebalancing costs may frequently be incurred, price response to trading). An index fund with m stocks, where m is substantially smaller than the size of the target population, n, seems desirable.

In this project, we will create an Index fund with m stocks to track the NASDAQ-100 index. We will do this in multiple steps. First, we will formulate an integer program that picks exactly m out of n stocks for our
portfolio. This integer program will take as input a ‘similarity matrix’, which we will call 𝜌. The individual elements of this matrix, 𝜌!", represent similarity between stock i and j. An example of this is the correlation between the returns of stocks i and j. But one could choose other similarity metrics 𝜌!".

Next, you will solve a linear program to decide how many of each chosen stock to buy for your portfolio and finally evaluate how well your index fund does as compared to the NASDAQ-100 index, out of sample. You will examine the performance for several values of m.

**Stock Selection**

The binary decision variables yj indicates which stocks j from the index are present in the fund (yj = 1 if j is selected in the fund, 0 otherwise). For each stock in the index, i = 1, . . . , n, the binary decision variable xij indicates which stock j in the index is the best representative of stock i (xij = 1 if stock j in index is the most similar stock i, 0 otherwise).

The first constraint selects exactly m stocks to be held in the fund. The second constraint imposes that each stock i has exactly one representative stock j in the index. The third constraint guarantees that stock i is best represented by stock j only if j is in the fund. The objective of the model maximizes the similarity between the n stocks and their representatives in the fund.

Another way of thinking is that, you have two sets. Set I has all stocks in the index. Set F has fund stocks. You want to create a link that maps elements of set I to elements of set F. The first constraint in the formulation above is equivalent to saying that not more than q elements of F can be mapped from set I. The second constraint suggests that each element in the set I will map to single element in set F . The third constraint suggests that if an element from set I gets mapped to an element in set F, then the element of set F better be present in the fund. The binary constraint on xij indicates if a link between element i in set I to element j in set F exists. The weight on that link is the correlation ρij between stock i in set I and stock j in set F. Many such mappings which satisfy the above constraints exist. Your objective gives you the best mapping. This is the basic idea of bipartite matching.






**Calculating Portfolio Weights**

To get the portfolio weights you will try to match the returns of the index as closely as possible. Let’s call 𝑟!" the return of stock i (where stock i is one of the chosen stocks from above) at time period t, 𝑞" the return of the index at time t, and 𝑤! the weight of stock i in the portfolio. You want to solve


The absolute value function is non-linear, so we must reformulate this optimization problem as a linear program. I’m not going to tell you exactly how to do this, but I will give you a very big hint.

Suppose I want to solve



It should be apparent that the answer to this problem is x=2. However, this is a non-linear program; here’s how you could formulate it as an LP.


Let’s think about what y1 does here. We are trying to minimize over y1 and we constrain y1 to be bigger than both x-1 and 1-x. Only one of x-1 or 1-x can be bigger than zero, so if y1 is bigger than both of them
then y1 must be bigger than |x-1|. If we’re trying to make y1 as small as possible and the only constraint on y1 is that y1 ≥ |x-1|, then y1 will be exactly equal to |x-1|. Now since we’re also optimizing over x, and x shows up in all the constraints for y1, y2, y3 then these two optimization problems are exactly the same. The first formulation is non-linear, and the second formulation is an LP!!! Take this hint and apply it to the portfolio weight construction.

Specifics

1)	Two csv files are included with this assignment. One of those files contains daily prices of the index as well as the component stocks of the NASDAQ-100 in 2019. The first column is the date, the second column is the index price (NDX), and columns 3-102 are the prices of the component stocks. The second file is the same, except it’s 2020 prices. There are actually 103 stocks in the NASDAQ-100 right now, but 3 of them are new stocks that don’t have data for all of 2019, so I just removed them. You’re going to use the 2019 file for all your portfolio construction tasks and then analyze the performance on the 2020 file; that is, how well does your portfolio, constructed with 2019 data, track the index in 2020? When we grade your assignment, we will use different csv files with potentially a different number of days and a different index with a different number of component stocks. You will need to calculate the returns of the stocks in the 2019 file to calculate the correlation matrix for stock selection and weight construction, and you will need the returns of the index for weight construction. You will also need the daily returns in 2020 for both the index and the stocks to evaluate the performance of your portfolio
in 2020. Use the correlation matrix of returns as 𝜌.


1)	Start with m=5. Find the best 5 stocks to include in your portfolio and the weights of those 5 stocks, using the 2019 data. How well does this portfolio track the index in 2020? That is,
calculate ∑+
8𝑞" − ∑)
𝑤!𝑟!"8 using the 2020 data (except wi is from your 2019 solution…).
2)	Redo step (2) with m = 10, 20, …, 90, 100 (obviously when m=100 you don’t need to solve for which stocks to include, because they’re all included). Analyze the performance of the portfolio for each value of m. How does the performance change? Is there some value of m, where there are diminishing returns of including more stocks in the portfolio? You can also look at the in- sample performance. That is, evaluate the performance in 2019 using 2019 portfolio construction and 2019 data. How is performance in 2019 different than performance in 2020? Why is it different?  Be sure to write your code so that if there are more or fewer than 100 stocks in the csv file it stops at the right place.
3)	Another way you could solve this problem is to completely ignore the stock selection IP and re- formulate the weight selection problem to be an MIP that constrains the number of non-zero weights to be an integer. To do this take the weight selection problem and replace m with n so
that you are optimizing over ALL weights: min ∑-	|𝑞+  − ∑'	𝑤% 𝑟%+|.  Now define some
*	+()
%()
binary variables y1, y2, …, yn and add some constraints that force wi = 0 if yi = 0 using the ‘big M’ technique (What’s the smallest value of big M you could use?). You also need to add a constraint that the sum of y’s is equal to m (m and M are different things here…). It turns out that this is a VERY hard problem to solve. After 24 hours running on my desktop gurobi didn’t find a solution! We can force gurobi to quit looking for a solution after a specific amount of time by setting the TimeLimit value, in units of seconds, in the params list (the place where we tell gurobi to shut up). Redo parts 2 and 3 with this new method to find weights. For each value of m, limit gurobi to work for 1 hour. Which method works better on the 2020 data, the original method or this new method? Note that your code will need to run for up to 10 hours to create your final output. I would suggest that you plan ahead and set this to run overnight.  You can set your Python file to save your results for this part to a csv file, and then you can also have it check to see if that csv file exists, if it exists grab those results without spending the 10 hours, and if it doesn’t then re-solve the problem. That way you only have to do the big solution once and you can still work on the formatting of your Python file. In your Python file create a clearly obvious variable at the top, that is equal to 3600, that you reference to limit gurobi’s time. This way when we grade your solutions, we can set it to something smaller to make sure your code works without having to wait 10 hours for everyone’s code to run.
4)	Pretend you are an analyst at a mutual fund. You boss has asked your team to come up with a recommendation for how many component stocks to include in the fund and how to pick their weights going forward. Write this project as if this is what you’re going to deliver to your boss. Your boss is pretty technical and understands optimization, so don’t be afraid to include quantitative material. Your boss is also busy, so be sure to include some visualizations to get the important points across. For the purpose of your recommendations, you can assume that your boss is interested in the data posted with the project.








