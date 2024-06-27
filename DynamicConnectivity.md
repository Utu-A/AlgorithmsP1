# Given a set of N objects
# - Union command: connect two objects
# - Find/connected query: is there a path connecting two objects?


Suppose we give:

union(4,3)
union(3,8)
union(6,5)
union(9,4)
union(2,1)

this forms such a structure:

0     1 ---------- 2         3---------4
                             |         |
5-----6            7         8         9

now we give a query
connected(0,7)  - returns false
connected(8,9)  - returns true (because 8 is connected to 3, which is connected to 4, which is connected to 9; therefore 8 is connected to 9)

now let's say we say:
union(5,0)
union(7,2)
union(6,1)
union(1,0)

we get a new structure:

0 --- 1 ---------- 2         3---------4
|     |            |         |         |
5-----6            7         8         9

connected(0,7) - returns true

## Applications of this data structure involves manipulating objects of all types:

- Pixels in a digital photo
- Computers in a network
- Friends in a social network
- Transistors in a computer chip
- Elements in a mathematical set
- Variable names in Fortran program(Abstract things)
- Metallic sites in a composite system(Physical things)



When programming, convenient to name objects 0 to N-1:
- use integers as array index
- Suppress details not relevant to union-find



# We need few abstract properties these connections need to satisfy
# We assume "is connected to" is an equivalence relation:
- Reflexive: p is connected to p
- Symmetric: if p is connected to q, then q is connected to p
- Transitive: if p is connected to q, and q is connected to r, then p is connected to r

When we have an equivalence relation a set of objects and connections divide into subsets called connected components. 
# A connected component is a maximal set of objects that's mutually connected.

take a structure:

0   1      2---3
   /|      |  /|
  / |      | / |
4---5      6   7

{0}  {1,4,5}  {2,3,6,7}

## Implementation:

Find query: Check if two objects are in the same component
Union command: Replace components containing two objects with their union

Suppose we have

0   1      2---3                                0   1    2   3
   /|      |  /|        ==========>                /    /|  /|
  / |      | / |        union(2,5)                /    / | / |
4---5      6   7                                 4----5  6   7

{0}  {1,4,5}  {2,3,6,7}                         {0} {1,2,3,4,5,6,7}




### Union-find datatype(API)

Goal: Design efficient data structure for Union-find
- Number of objects N can be huge
- Number of operations M can be huge
- Find queries and union commands may be intermixed.

public class UF
    UF(int N)       initialize union-find data structure with N objects(0 to N-1)
    
    void union(int p, int q)    add connection between p and q

    boolean connected(int p, int q) are p and q in the same component?

    int find(int p) component identifier for p(0 to N-1)

    int count() number of components

    (1,2,7,8,9)(3,4)(0,5,6)




### Dynamic-connectivity client
Read in number of objects N from standard input.
・Repeat:
– read in pair of integers from standard input
– if they are not yet connected, connect them and print out pair


## Solution:

public static void main(String[] args)
{
    int N = StdIn.readInt();
    UF uf = new UF(N);
    while (!StdIn.isEmpty())
    {
        int p = StdIn.readInt();
        int q = StdIn.readInt();
        if (!uf.connected(p, q))
        {
            uf.union(p, q);
            StdOut.println(p + " " + q);
        }
    }
}