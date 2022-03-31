# Quicksort



## N0: Abrupt termination

If both quicksort and merge-sort are abruptly terminated at some stage of execution, quicksort finds itself in a better position than merge-sort. This is a key difference between the two algorithms. Quicksort partitions first while merge-sort merges last. As a result, when quicksort is terminated at some stage, the array is partially sorted. One way to measure this is to see the number of inversions. The number of inversions in a sorted array is zero. For an unsorted array, the number of inversions gives an estimate of its "un-sortedness".



## References

[Hoare's paper](http://rabbit.eng.miami.edu/class/een511/quicksort.pdf)