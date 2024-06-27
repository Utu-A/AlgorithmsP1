### Quick find[eager approach]

Data Structure:
- Integer array id[] of size N
- Interpretation: p and q are connected iff(if and only if) they have the same id

     0  1  2   3   4   5   6   7   8   9
id[] 0  1  1   8   8   0   0   1   8   8

0,5 and 6 connected
1,2,7 connected
3,4,8 and 9 connected

0       1       2       3-------4
|               |       |       |
5-------6       7       8       9


# Find: Check if p and q have the same id

id[6] = 0; id[1] =1
6 and 1 are not connected

# Union: To merge components containing p and q, change all enteries whose id equals id[p] to id[q]

     0  1  2   3   4   5   6   7   8   9
id[] 1  1  1   8   8   1   1   1   8   8
     ^                 ^   ^

after union of 6 and 1
problem: many values can change

### WE CHANGE THE SECOND ONE OCCURING WITH THE VALUE OF FIRST ONE OCCURING
### EXAMPLE: 

    0   1   2   3   4   5   6   7   8   9
    0   1   2   3   4   5   6   7   8   9
    
    union(4,3);

    0   1   2   3   4   5   6   7   8   9
    0   1   2   3   3   5   6   7   8   9
                ^

    now if we connect 3 and 4, to 8, we have to change both of those ids to 8 because we are connecting a pair

    union(3,8)

    0   1   2   3   4   5   6   7   8   9
    0   1   2   8   8   5   6   7   8   9
                ^   ^


public class QuickFindUF
{
    private int[] id;

    //to set id of each object to itself(N array access)
    public QuickFindUF(int N)//to set id of each object to itself(N array access)
    {
        id=new int[N];
        for(int i=0;i < N;i++){
            id[i]=i;
        }
    }

    //check whether p and q are in the same component
    public boolean connected(int p, int q)
    {
        
        if(id[p]==id[q])
            return true;
        else
            return false;
    }

    //change all entries with id[p] to id[q]
    public void union(int p, int q)
    {
        int pid=id[p];
        int qid=id[q];
        for(int i=0, i<n ; i++){
            if(id[i]==pid)
                id[i]=qid;
        }
    }
}


Complexity: 
    initialize: N
    find: 1
    union: N
There BigO notation is O(N^2)

Applications of Union Find:
・Percolation.
・Games (Go, Hex).
✓ Dynamic connectivity.
・Least common ancestor.
・Equivalence of finite state automata.
・Hoshen-Kopelman algorithm in physics.
・Hinley-Milner polymorphic type inference.
・Kruskal's minimum spanning tree algorithm.
・Compiling equivalence statements in Fortran.
・Morphological attribute openings and closings.
・Matlab's bwlabel() function in image processing.

### Percolation

A model for many physical systems:

So let's think of an N*N grid of squares that we call sites. And we'll say that each site is open.
Open => white in the diagram with probabability P
Blocked => black of the diagram with probability (1-P)

# System percolates iff top and bottom are connected by open sites.

We can think of this for various situations:

model            system       vacant site     occupied site      percolates

electricity      material     conductor       insulated          conducts

fluid flow       material     empty           blocked             porous

social-interaction population person          empty            communicates

p low => does not percolate
p high => percolates
# p medium => percolates?

## Percolation phase transition
When N is large, theory guarentess a sharp threshold p*
- p > p* : almost certainly percolates
- p < p* : does not percolate

Value of p* comes by fast union find algorithms and simulations
About 0.592746 for large square lattices

An example of fast union find algorithm is:
### Monte Carlo simulation
- Initialize N*N whole grid to be blocked.
- Declare random sites open until top connected to bottom
- Vacancy percentage estimates p*

to get the value of p*, we run the Monte Carlo simulation a lot of times and each time get a percentage of threshold
We first define the whole grid black, then we randomly open sites till we reach the other half, everytime we add an open site, we check to see if the system percolates and we keep going till the system percolates.We can show that the vacancy percentage at the time that it percolates is an estimate of this threshold value.

### Dynamic connectivity solution to estimate percolation threshold

Q. How to check whether an N-by-N system percolates?
・Create an object for each site and name them 0 to (N^2) – 1.
・Sites are in same component if connected by open sites.
・Percolates iff any site on bottom row is connected to site on top row

(open sites representing by 1, closed by 0)

1   1   0   1   0
0   0   0   1   0
0   1   0   1   1
1   0   1   0   0
1   1   0   1   1

we draw a Dynamic connectivity diagram of this grid

* - *   *   *   *   <- top row
            |
*   *   *   *   *
            |   
*   *   *   * - *

*   *   *   *   *
|
* - *   *   *   *   <- bottom row

So instead of Brute forcing to search if top connects to bottom
We create a virtual top and virtual bottom

        *   <- Virtual top

* - *   *   *   *   <- top row
            |
*   *   *   *   *
            |   
*   *   *   * - *

*   *   *   *   *
|
* - *   *   *   *   <- bottom row
 
        *   <- Virtual bottom

Q. How to open a new site?
A. Connect newly opened site to all its adjacent open sites[ upto 4 calls to union() ]. These can be 0 calls(if no adjacent site is open), 1,2,3 or 4 union calls.
