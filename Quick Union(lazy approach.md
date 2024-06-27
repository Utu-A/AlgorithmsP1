### Quick Union(lazy approach)

Data Structure
- Integer array id[] of size N
- Interpretation: id[1] is parent of i
- Root of i is id[id[id[...id[i]...]]]  --> keep going until it doesn't change

        0   1   2   3   4   5   6   7   8   9
id[]    0   1   9   4   9   6   6   7   8   9


        0       1       9       6       7       8
                        /\      |
                       /  \     |
                      2    4    5
                           |
                           3

# Note: While drawing, first draw the ones that are connected to themselves

# Find: Check if p and q have the same root

# Union: to merge components containing p and q
set id of p's root to the id of q's root

union(6,9)

        0   1   2   3   4   5   6   7   8   9
id[]    0   1   9   4   9   6   6   7   8   6
                                            ^
                                            |
                                            only one value changes


        0       1               6    7      8
                               /|
                              9 5        
                             /\    
                            /  \     
                            2   4    
                                |
                                3



Note: We try that there is not a lot of changes that have to be made in the id array

union(3,8) => we take the first item, and make it a child of the second item(i.e. 3 becomes child of 8)

## Suppose if the root containing 4 is 8, and we perform union(9,4), so now the root of 9 is 8 directly, rather than connecting it below 4


# Suppose if 6 is a child of 0, and 1 has it's own children, root of itself, then if union(6,1) makes 0 a child of 1, which is the parent of 6



Quick Union:
Initialize: N
Union: N
Find: N


#### WEIGHTED QUICK UNION
Data Structure:
Same as quick union, but maintain extra array sz[i] to count number of objects in the tree rooted at i

### Find: Identical to quick union:
return root(p)==root(q)

### Union: Modify quick-union to:
- link root of smaller tree to root of larger tree
- update the sz[] array

int i=root(p)
int j=root(q)
if(i==j) return ;
if(sz[i] < sz[j]){
        id[i]=j;
        sz[j]+=sz[i]
}
else{
        id[j]=i;
        sz[i]+=sz[j]
}

### Running time:
## Find: takes time proportional to depth of p and q
## Union: takes constant time, given roots

## Proposition: Depth of any node x is at most log(base 2) N

Weighted Quick Union:
initialize: N
Union: log(base 2)N
Find: log(base 2) N

#### Improvement 2
### Quick unoin with path compression: Just after computing the root of p, set the id of each examined node to point at that root


So here instead of a sub-root pointing at it's parent, which is pointing at the main root, we directly mention the sub-root to point at that main root

Two pass implementation: Add second loop to root() to set id[] of each examined node to the root

Simpler one pass variant: Make every other node in path point to it's grand parent(main parent root) thereby halving path length

Code:
        private int root(int i)
        {
                while(i!=id[i])
                {
                        id[i]=id[id[i]];
                        i=id[i];
                }
                return i;
        }

log * N