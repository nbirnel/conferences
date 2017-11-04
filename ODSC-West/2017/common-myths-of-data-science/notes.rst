https://github.com/WinVector/ODSCWest2017

If I have an overfit, it may not be the model, but your data is too wide.

"At least 3 rows per variable"
(but for complex models you might need more)
categorical variables (eg, zip codes) - you should view this variable as
as many variables as there are values.

1 get more data
2 variable pruning:
  * doman knowledge
  * permutation measure:
    scramble the data for a variable, and see if it has any impact. 
  * signifance pruning - "data scientists don't look at p-values much"

aborting a/b tests - "don't peek" - maybe is useless, but why forbid?

