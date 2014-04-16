pairs-iterators
===============

C++ iterators for iterating over pairs of elements from containers.

### Introduction

A longer introduction can be read here:

https://cwzx.wordpress.com/2014/04/15/iterating-over-distinct-pairs-in-cpp/

The short version is that these iterators allow you to iterate over pairs of elements from a container (or a pair of containers) without having to generate or store a container of pairs.

### Product

product(X,Y) is the Cartesian product of X and Y, which is the set of all pairs (x,y). There are N*M pairs, where N is the size of X and M is the size of Y.

pairs(X) = product(X,X). There are N^2 pairs.

### Distinct Pairs

distinct_pairs(X) is the set of all pairs (x,y) such that x and y are different instances and the order doesn't matter, so (x,y) is the same distinct pair as (y,x). These are known in combinatorics as '2-combinations', there are 'N choose 2' distinct pairs, which is N(N-1)/2.

### Zip

zip(X,Y) is the set of element-wise pairings, e.g.

zip( { 1, 2, 3 }, { 4, 5, 6 } ) = { (1,4), (2,5), (3,6) }

The number of pairs is min(M,N), i.e. if one of the lists is longer than the other, its extra elements will be ignored.

Usage Notes
-----------

The iterators' dereference operations return a pair of references, not a reference. Therefore you should receive this by-value, not by-reference (e.g. in the range-based for loop).

Examples
--------

```cpp
// a big vector
vector<int> v( 1 << 17 );
 
// fill the vector with ascending numbers starting with 1
iota( begin(v), end(v), 1 );
 
// count the number of distinct pairs whose sum is even
auto ps = distinct_pairs(v);
cout << count_if( begin(ps), end(ps),
    []( auto p ) {
        return ( p.first + p.second ) % 2 == 0;
    }
);
 
/* Output:
4294901760
*/
```

```cpp
vector<int> v { 1, 2, 3, 4 };
 
for( auto p : distinct_pairs(distinct_pairs(v)) ) {
    cout << "( ( " << p.first.first << ", " << p.first.second << " ), ( " << p.second.first << ", " << p.second.second << " ) )" << endl;
}
 
/* Output:
( ( 1, 2 ), ( 1, 3 ) )
( ( 1, 2 ), ( 1, 4 ) )
( ( 1, 2 ), ( 2, 3 ) )
( ( 1, 2 ), ( 2, 4 ) )
( ( 1, 2 ), ( 3, 4 ) )
( ( 1, 3 ), ( 1, 4 ) )
( ( 1, 3 ), ( 2, 3 ) )
( ( 1, 3 ), ( 2, 4 ) )
( ( 1, 3 ), ( 3, 4 ) )
( ( 1, 4 ), ( 2, 3 ) )
( ( 1, 4 ), ( 2, 4 ) )
( ( 1, 4 ), ( 3, 4 ) )
( ( 2, 3 ), ( 2, 4 ) )
( ( 2, 3 ), ( 3, 4 ) )
( ( 2, 4 ), ( 3, 4 ) )
*/
```

