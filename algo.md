## Merge-insertion sort = Ford-Johnson algorithm 
* modification of insertion sort
* takes into account this property of binary search: the maximal number of comparisons to perform a binary search on a sorted sequence is the same for $2^n$ elements and $2^{n+1}−1$ elements (looking for an element in a sorted sequence of 8 or 15 elements requires the same number of comparisons), ensures that the size of the insertion area is $2^n−1$ as often as possible
* the worst case: less comparaisons than insertion sort
* the worst case: less comparaisons than merge sort
* [Ford, Lester R., and Selmer M. Johnson. “A Tournament Problem.” The American Mathematical Monthly, vol. 66, no. 5, 1959, pp. 387–389. JSTOR](www.jstor.org/stable/2308750)
* [The Art of Computer Programming by Donald Knuth, Volume 3, section 5.3.1](https://warwick.ac.uk/fac/sci/dcs/teaching/material-archive/cs341/fj.pdf)
* [Ford-Johnson merge-insertion sort](https://codereview.stackexchange.com/questions/116367/ford-johnson-merge-insertion-sort)
* Mahmoud, Hosam M. Sorting: A Distribution Theory. John Wiley & Sons. (October 14, 2011)
* [Morwenn. “Ford-Johnson Merge-Insertion Sort”. Code Review Stack Exchange. 10 Jan. 2016](https://codereview.stackexchange.com/questions/116367/ford-johnson-merge-insertion-sort)
* https://en.wikipedia.org/wiki/Merge-insertion_sort#Algorithm  
* https://codereview.stackexchange.com/questions/116367/ford-johnson-merge-insertion-sort  
* https://github.com/decidedlyso/merge-insertion-sort/blob/master/README.md  
* https://github.com/PunkChameleon/ford-johnson-merge-insertion-sort  
* https://github.com/decidedlyso/merge-insertion-sort
* [A variant of the Ford–Johnson algorithm that is more space efficient](https://www.researchgate.net/publication/222571621_A_variant_of_the_Ford-Johnson_algorithm_that_is_more_space_efficient)
* [Improvements to the Ford-Johnson algorithm](https://link.springer.com/article/10.1007/BF01934989)
* [The Ford-Johnson Sorting Algorithm Is Not Optimal, Manacher, Glenn K. 1979](https://dl.acm.org/doi/10.1145/322139.322145)
* https://www.youtube.com/watch?v=w1QXGe295sI
