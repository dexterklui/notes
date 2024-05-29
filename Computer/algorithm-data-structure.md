# Algorithm and Data Structure

## Recursion

- The two functions (`perm()` and `reverse()`) in the PowerPoint seems not the
  most straight forward and be too general. But they have the advantage that it
  doesn't require much **extra memory space**, unlike the straight forward one.

  - For example, you think the straight forward `perm()` could be something
    like:

    ```text
    if (array.size() <= 1) {
      return array;
    for (int i = 0; i < array.size(); ++i) {
      smaller_array = array.RemoveElement(i);
      return perm(smaller_array) + array[i];
    }
    ```

    But this algorithm creates many intermediate smaller arrays and quickly
    exhausts the memory when the initial size of array grows.

  - **_NOTE_** so remember these two algorithm of `perm()` and `reverse()`. They
    are simple (short) and efficient.

- Usually when designing a recursive function, there is one or more **extra
  parameters** that trace the progress of the recursion.

- When you write the code, at the same time you try to perform the Mathematical
  Induction proof.

  - Base case → proof of base case
  - Recursion → proof of inductive step

### advantage of recursion

- Easy to proof correctness
- Easy to calculate the execution time.

### Exercise

In general, if f(n) = ∑(i=0,i=r) a.i \* n^(r), you can use differencing method
to guess the general form. (The difference between n-th and (n-1)-th term.)

## Algorithms

### Designing a program

#### Two parts of the design

A program consists of two parts:

- **_Algorithm_**: the computational procedure which transforms an input into
  the desired output as specified by the problem statement.
- **_Data structures_**: the representation, organization and storage of
  information.

#### Plan to solve a computational problem

1. Recognizing a **problem**.
2. Build an **_abstract model_** of the problem - the set of operations allowed.
3. Design an **algorithm** to solve the problem within the model.
4. Analyse the algorithm to get an **upper bound** on the work needed to solve
   the problem.
5. If possible, analyse the problem to find a **lower bound** to the
   **problem**.
6. Improve or try new algorithm if not satisfied (specifically when there is a
   big gap between the upper bound and lower bound in (4) and (5))
7. Build **data structures** that allow the algorithm to be implemented
   efficiently.
   - Sometimes steps (3) and (7) are integrated.

- Can we prove the algorithm is correct?
- Lower bound says how complex the problem is
- Upper bound says how bad the algorithm is

#### Worst, average, best cost

- Usually the best cost is not very informative. For example, the best cost
  stays at 1 for linear search in an array.
- To calculate the average cost, we need to know $P(I)$, the probability
  distribution of all inputs of a given size $n$. But even if we know $P(I)$, it
  is usually harder to calculate the average cost than the worst cost.
- So we usually calculate the worst cost, and for very important algorithm in
  important problem, we calculate the average cost.

### Measuring algorithm

#### Parameters to measure

- **Problem size**
  - Should reflect the **complexity** of the problem instance.
- **Computational resources** required: expressed in terms of the problem size.
  - Should measure the resources we care about
  - Should be **quantitative**
  - Should be a good **predictor**: it is easy to predict the resource
    requirement when the algorithm is applied to a large set of problem
    instances.

#### Measuring methods

The most straightforward and perhaps convincing and final method is to write 2
programs and time their speeds. But in the design stage, better use the
following method.

Count some set of operations that the algorithm performs, then derive a rough
estimate of the numbers of such operations required. The result is a **function
of the problem's size**.

#### Count which operations?

Since different operations cost different time in different machines, we should
only care about the number of executions rather than the coefficient or constant
terms.

we are interested only in the **growth rate** of the **total number of
operations** as a function of the input size

#### Big Theta ($\Theta$) notation

Represent the **asymptotic efficiency** of the algorithm:

$\Theta (g(n)) = \{ f(n): \exists c_1 > 0, c_2 > 0, n_0 > 0 \text{ s.t. } 0 \leq
c_1g(n) \leq f(n) \leq c_2g(n) \forall n \geq n_0 \}$

- If $f(n) \in \Theta (g(n))$ we say that $g(n)$ is an **asymptotically tight
  bound** for $f(n)$.
- In short it represent the most significant term. (For this course this concept
  is enough)

##### Abusing notation

Informally, when we write $f(n) = \Theta (g(n))$, we mean
$f(n) \in \Theta (g(n))$.

So when we write $n^3 + \Theta (n^2) = \Theta (n^3)$, we mean the summation of
$n^3$ and any function in $\Theta (n²)$ equals to some function in
$\Theta (n³)$.

#### Big Oh ($O$) notation

Gives an upper bound $g(n)$ on a function, to within a constant factor.

$O(g(n)) = \{ f(n): \exists c > 0, n_0 > 0 \text{ s.t. } 0 \leq f(n) \leq cg(n)
\forall n \geq n_0 \}$

- E.g. $f(n) = 2n^2 + 4n - 1$ is a member of $O(n^2)$, as well as $O(n^3)$, but
  not $O(n)$.
- If $f_1(n) = O(g(n)), f₂(n) = O(g(n))$, then
  $f_3(n) = f_1(n) + f_2(n) = O(g(n))$

#### Big Omega ($\Omega$) notation

Gives us an asymptotic lower bound.

$\Omega (g(n)) = \{ f(n): \exists c > 0, n_0 > 0 \text{ s.t. } 0 \leq cg(n) \leq
f(n) \forall n \geq n_0 \}$

#### Little oh ($o$) notation

To denote an upper bound that is **not asymptotically tight**. (the "proper"
upper bound)

$o(g(n)) = \{ f(n): \forall c > 0, \exists n_0 > 0 \text{ s.t. } 0 \leq f(n) <
cg(n) \forall n \geq n_0 \}$

Note that $f(n) = o(g(n)) \iff \lim_{n \to \infin} \frac{f(n)}{g(n)} = 0$

#### Little Omega ($\omega$) notation

To denote an lower bound that is not asymptotically tight. (the "proper" lower
bound). ω(g(n) = { f(n): ∀c>0, ∃n₀>0 s.t. ∀n≥n₀ ( 0 ≤ cg(n) < f(n) }

$\omega (g(n)) = \{ f(n): \forall c > 0, \exists n_0 > 0 \text{ s.t. } 0 \leq
cg(n) < f(n) \forall n \geq n_0 \}$

- $f(n) = \omega (g(n)) \iff \lim_{n \to \infin} \frac{g(n)}{f(n)} = 0$
- $f(n) = \omega (g(n)) \iff g(n) = o(f(n))$

#### Properties of the asymptotic notations

- Transitivity, reflexivity, symmetry, transpose
- Associate the notations with the comparison operators:
  - $O$: $\leq$
  - $\Theta$: $=$
  - $\Omega$: $\geq$
  - $o$: $<$
  - $\omega$: $>$

#### Rules to calculating complexity

1. loops

   At most the complexity of the statement inside the loop (including tests)
   times the number of iterations.

2. consecutive statements

   $\text{max}\{\text{complexity of segment 1, complexity of segment 2}\}$

3. if-then-else

   $\text{max}\{\text{complexity of (S1 + cond), complexity of (S2 + cond)}\}$,\
   which is\
   $\text{complexity of cond} + \text{max}\{\text{complexity of S1, complexity
   of S2}\}$

4. function call

   The complexity of a function should be analysed first before computing the
   complexity of the program fragment containing the call.

#### Some typical growth rates

- $O(1)$ is **constant time**
- $O(\log{n})$ is **logarithmic complexity**. Commonly occurs in programs that
  solve a big problem by transforming it into a series of smaller problems,
  cutting the problem size by some constant fraction at each step.
- $O(n)$, **linear complexity**. Commonly for algorithms that do constant amount
  of work on each input element.
- $O(n \log{n})$. When algorithms solve a problem by breaking it up into smaller
  sub problems, solving them independently, and then combining the solutions.
- $O(n^2)$, **quadratic complexity**. Typically for algorithms that process all
  pairs of data items.
- $O(n^3)$, cubic complexity.
- $O(2^n)$, exponential. Not likely to be practically useful for large input.
- **_NOTE_** that if $f(n) = 2n = O(n)$, then $(f(n))^n = 2^n n^n \neq O(n^n)$,
  because $2^n$ is not a constant coefficient that can hide behind the big oh
  notation.

#### Hard problems

- We say a problem is **_tractable_** if we have an algorithm of **$O(n^c)$**
  for some constant $c$.
- Problems with algorithm of **$O(r^n)$** are called **_computationally hard
  problems_**.

#### Fabonacci: calculate runtime efficiency

T(n) = aT(n-1) + bT(n-2) ∵ T(n) = c x^n ?, we have c x^n = a c x^(n-1) + b c
x^(n-2)

Characteristic equation: x² - ax - b = 0.

Generally, given the roots φ₁, φ₂, T(n) = c₁φ₁^n + c₂φ₂^n

#### Rounding error

To calculate an expression involving, say, $\sqrt{5}$. There will be rounding
errors. To deal with that, we can create a new **data type** to represent it:

- Type `(a,b)` represents $a + b\sqrt{5}$.
  - Addition `(a,b) + (c,d)`
  - multiplication
  - division

#### $x^n$ using $O(\log{n})$ runtime & space efficiency

Instead of multiply $x$ $n$ times, you can do it recursively:

$x^n = x^{\frac{n}{2}} x^{\frac{n}{2}}$

### Moving Stones (Past Final Question)

### Questions

- Do we assume all intrinsic operation operates at constant time? Like doing
  exponential? (When doing homework, $n^{2.5}$)
- Why that $g(n)$ at slide 44 is not an equal sign but $\geq$?

## Data

### Data types

A data type is:

- a set of values; and
- a collection of operations on those values.

### Abstract data type (ADT)

Extend the concept of **encapsulation** to more complicated data types, to have
an abstract data type:

- Information we want to maintain
- Collection of operations that manage the information

### Aggregation

3 common aggregate mechanisms:

- Array
- Record structure
- Pointer

### Realizing an ADT

- Build **data structures** to organize and represent the information
- Devise **algorithms** to implement the operations

These two steps are language-specific.

### Choosing a Data Structure

Devise data structures in such a way that:

- The associated operations can be supported efficiently, and
- The structures are space efficient.
- Most of the time one has to assess what is more important: time vs space.

Using **pointers** wastes extra space.

### Common Data Structures

#### Sets

##### sets

A set is a collection of elements. An element may have a **key** identifying the
element. (**_DUNNO_** but I think a set with no keys can be think of as a set
with elements whose keys are themselves)

- The key is incorporated into the element.

Things to think about the specification:

- Whether it is allowed to have multiple elements with the same key

##### basic operations

- `INIT(S)`: initialize a set S
- `SEARCH(S,k)`: retrieve the element with key = k
- `INSERT(S,x)`: insert element x into set S
- `DELETE(S,x)`: delete element x from S
- `DELETE_Key(S,k)`: delete element with key k from set S

If comparing of keys is defined:

- `MINIMUM(S)`: find the element in S with the smallest key value.
- `MAXIMUM(S)`: find the element in S with the largest key value.
- `SORT(S)`: sort the elements according to key values.

If the set is an **ordered-set**, e.g. each element is given a position:

- `SUCCESSOR(S,x)`: find the element in S which is ordered next
- `Predecessor(S,x)`: fine the element in S which is ordered in front of x.

#### List

A list is an **ordered** set of elements: $\{a_1, a_2, \dots, a_n\}$

##### basic operations

- `INIT(L)`
- `SEARCH(L,k)`: search for an element
- `INSERT(L,i,x)`: inserts the element x at position i.
- `DELETE(L,x)`
- `FIND_i-th(L)`: find the i-th element of list L.
  - **_DUNNO_** why not `FIND(L,i)`? I guess to make it clear that it is
    accessing the i-th element, but not searching for an element.

##### comparing simple array and linked list

|           | simple array | linked list |
| --------- | ------------ | ----------- |
| INIT      | Θ(1)         | Θ(1)        |
| SEARCH    | Θ(n)         | Θ(n)        |
| FIND_i-th | Θ(1)         |             |
| INSERT    | Θ(n)^        | Θ(1)\*      |
| DELETE    | Θ(n)         | Θ(1)\*      |

^ Takes $\Theta (1)$ if we don't care the position.

\* If we have already the pointer of the relevant node (the node before the
target place/node), or if we do it at the head of the linked list (when we don't
care the location e.g. for inserting).

Comparing:

- Linked list does not support **random-access** (so not good to find the i-th
  element)
- Linked list is more appropriate if we need to **rearrange** the items very
  often
- We don't need to declare the **maximum** size for linked list.
  - For array implementation, it is possible to expand the array, but it is also
    expensive.
- Insertion and deletion with linked lists requires **dynamic allocation** and
  deallocation of memory, these are typically **expensive** operations.

**_NOTE_** that when we ask about the complexity of **average case**, it depends
on the context.

##### linked list implementations

When doing an operation on linked list, sometimes we need to treat it as a
special case when we dealing with the first or last node. The ways to do this
are:

- Use guardians (if condition)
- Add a **_sentinel node_**, i.e. filler leading/trailing node
- Implement **circular** doubly-linked list
- If the list is expected to be short, the use of filler node(s) and doubly
  links could waste space.

##### implementation: circular doubly-linked list

Assuming a circular doubly-linked list with a filler node. You can use recursive
way for your methods. E.g.:

```c
Node* reverse(L) {
 if (L -> next == L) return L;

 // First, take the first real node out
 Node* p1 = L -> next;
 p1 -> next -> prev = L;
 L -> next = p1 -> next;

 // Second, recursive call to reverse the else
 L = reverse(L)

 // Now add the first real node as the last node
 p1 -> prev = L -> prev;
 L -> prev -> next = p1;

 // DUNNO: if need to update L -> next and L -> prev?
 return L;
}
```

This takes linear time. However, a recursive call will takes $\Theta (n)$ extra
memory. Using other method may not allow you to use recursion.

#### sorted list

Improves searching efficiency. No need to search through the whole list to know
an item is not already in the list.

#### Stack

A **_stack_** is a list with restriction that inserts and deletes only at one
end - the **_top_**. It maintains a **_LIFO_** (**last-in-first-out**) order.

##### main operations

- `push(x)` inserts element at the top.
- `pop()` returns and deletes the top element.
- `top()` returns the top element.

##### implementations

The stack ADT can be implemented with lined list or array.

Array avoids pointers and dynamic memory allocation (simpler and more
efficient), but specifying an array size may waste space or get stack
overflowed.

```c
struct Stack {
 int tos; // (the index of) top of stack. -1 means empty
 element stack_array[MAX_CAP];
};
```

#### Queue

A **_queue_** is a list that insert is only done at **_tail_** and delete is
only done at **_head_**. It maintains a **_FIFO_** (**first-in-first-out**)
order.

##### basic operations

- `enqueue(Q,x)` adds an element to the tail.
- `dequeue(Q)` removes and returns the element from the head.

##### implementation

- As a **doubly linked list** with a pointer to the **head** and another pointer
  to the **tail**.
- By an array that **wraps around**, with two variables indicating the **head**
  index and **tail** index, and one integer variable indicating length.
  - **_DUNNO_**: is the length really necessary? How about the linked list
    implementation? **_Answer_**: this is handy to check for queue
    overflow/underflow, especially consider the case when the queue is
    **empty**. For linked list there is no need for such a check.
  - The elements in a queue are stored in positions: head, head+1, ...,
    **maxlength-1, 0**, 1, ..., tail.
  - **_Tip_**: The wrap around feature makes modulus operator a good operation
    to calculate the new head/tail index.

#### Graph

A **_graph_** is a set of **_vertices_** (singular: **_vertex_**) and a set of
connections (**_edges_**) among the vertices.

##### use

To represent relationship between objects

##### Implementation: adjacency matrix

To represent an undirected graph, we can use a 2-dimensional array, called an
**_adjacency matrix_**. Entry $[i,j]$ is 1 if vertices $i$ and $j$ are
connected, otherwise 0.

##### Implementation: adjacency list

To represent an undirected graph, we can also use an array of linked lists,
called **_adjacency lists_**:

- For each vertex $v$, we keep a linked list, which contains one node for each
  vertex that is connected to $v$.

##### Comparing implementations

|                  | adjacency matrix | adjacency list |
| ---------------- | ---------------- | -------------- |
| space efficiency | $O(V^2)$         | $O(V+E)$       |

- Matrix is more efficient for operations that require determining whether two
  vertices are connected. (Random access)
- Lists is more efficient for operations that require processing all the edges
  in the graph, especially if the graph is sparse.
- If number of lists is close to $\frac{1}{2} \times V \times (V-1)$, then list
  is less space efficient because of the extra pointer of each node.

##### Breadth-first search

In **_breadth-first order_**, we start with a vertex $k$ and visit other
vertices in the **order of closeness** with $k$. A vertex that is not
"connected" to $k$ should not be visited.

First trial:

```c
void BFS(k,n) { // k = starting vertex; n = no. of vertices
  Queue Q;
  bit visited[n] = {}; // bit vector
  INIT(Q);
  Q.Enqueue(k);
  while (!empty(Q)) {
    i = Q.Dequeue();
    if (!visited[i]) {
      visited[i] = 1;
      output(i);
      for each neighbor j of i {
        if (!visited[j]) Q.Enqueue(j);
      }
    }
  }
}
```

Second trial:

```c
void BFS2(k, n) {
  Queue Q;
  bit visited[n] = {};
  INIT(Q);
  Q.Enqueue(k);
  visited[k] = 1;
  Output(k);
  while (!empty(Q)) {
    v = Q.Dequeue();
    for each vertex w in the adjacency list of v {
      if (!visited[w]) {
        Q.Enqueue(w);
        visited[w] = 1;
        output(w);
      }
    }
  }
}
```

Although both trials have the same asymptotic complexity: O(N+M) where N is
number of vertices, and M is the number of edges.

The second one is slightly more efficient because it doesn't put the same vertex
into the queue more than once.

##### Depth-first search

In `BFS()` (first trial), replacing queue with **stack** results in depth-first
search. Alternatively, use recursion:

```text
DFS2(k, visited) {
  visited[k] = true;
  output(k);
  for each vertex v in the adjacent list of k {
    if (!visited[v]) DFS2(v, visited);
  }
}
```

##### Breadth vs depth first search

Depth-first search finishes one branch first before going into the second. So
the memory needed for a time slice is much less than using breadth-first search.
For this reason, when implementing recursion, we usually use depth-first search:
i.e. with stack.

## Hashing

### Dictionary ADT

#### Dictionary

Is a set data type with key-value pairs and that supports 3 operations:

- `INSERT`
- `DELETE`
- `SEARCH(key)`

Usually, `SEARCH` is the most used operation, but normally it takes $O(n)$ time.
So if we want to improve the efficiency of `SEARCH`, we use hashing.

### Hashing

**_Hashing_** consists of 2 parts:

- A **table** (an array) of size m (`T[0 ... m-1]`)
- A **hash function** $h$ which maps a key (of an element) into an index
  (integer) between $0$ and $m-1$ inclusive.
- So $k$ is mapped to slot $h(k)$ in the table.

##### Radix notation

To generate a hash from a string key, we may use **_radix notation_**.

For example, there are 128 characters in the ASCII set, so the hash of a string
composed of ASCII characters can be calculated like:

$h(\text{"BEN"}) = [ 66 \times (128)² + 69 \times 128 + 78] \mod m$, where $m$
is the size of table.

##### Hash collision

But the above radix notation may result in **_collision_** where two **different
keys are mapped to the same hash**. That is, the hash function is **not
injective** over the domain.

To handle this, we need:

- A good hash function, such that collision does not occur often, though it will
  always be possible
- A collision resolution strategy

##### Hash function complexity

Should be $O(1)$, otherwise if it is $O(n)$ it defeats the purpose of hashing.

### Chaining (Open Hashing)

To resolve collision, **_chaining_** (a.k.a **_open hashing_**) keeps a linked
list of all elements that have the same hash value.

#### Chaining implementation

1. Define hash table

   ```c
   typedef struct node* node_ptr;
   struct node {
     element ele;
     node_ptr next;
   }
   ```

2. Declare hash table

   ```c
   typedef node_ptr LIST;
   LIST T[m];
   ```

3. Initialize hash table

   ```c
   Chained_Hash_Init(T) {
     int i;
     for (i = 0; i < m; i++)
       T[i] = NULL;
   }
   ```

4. Search an element

   ```c
   Chained_Hash_Search(T,k) {
     node_ptr p;
     p = T[h(k)];
     while ((p != NULL) && (p->ele.key != k))
     p = p->next;
     return(p);
   }
   ```

5. Insert an element

   ```c
   Chained_Hash_Insert(T,x) {
     node_ptr p;
     p = T[h(x.key)];
     /* insert x into the linked list pointed to by p */
     // note we might want to perform duplicate check
   }
   ```

6. Delete an element

   If we only have the key, we have to search for it first.

#### Analysis

##### Load factor (a)

The **_load factor_** a for a hash table $T$ is **$n/m$** where n is the stored
elements, and m is the size of the hash table.

Intuitively, a is the average number of nodes (elements) stored in a chain.

##### Insert

Takes $O(1)$ time.

##### Delete

$O(1)$ if we have the pointer to the node and the list is a doubly linked list.
Otherwise its running time is similar to **searching**.

##### Searching

**Worst case** when all keys stored are **mapped to the same slot**. So it is
searching in a linked list of size n, i.e. $O(n)$.

##### Unsuccessful search

**Average** complexity of unsuccessful search is $O(1+a)$, the $O(1)$ comes from
the operation of the hash function. $a=n/m$ is the average amount of element in
a slot.

##### Successful search

Average complexity of successful is $O(1+a/2) = O(1+a)$.

### Hash function for Chaining

#### Good hash functions

- Satisfy (approximately) the assumption of simple uniform hashing.
- Easy to compute (as efficient as possible)

##### Simple uniform hashing

A good hash function map keys as **uniformly** as possible over $m$. The ideal
hash function is called **_simple uniform hashing_**.

- If you randomly draw a key from the universe, it is equally likely that the
  key get mapped to any of the m slots.

#### Implementation

First, we need to transform the key to natural numbers (i.e. 0, 1, 2, ...)

##### Division method

- $h(k) = k \mod m$.
- The choice of **$m$** is very important.
- Good hash function should **use all the information** provided by the keys.

Rule of thumbs:

- $m$ should not be a power of 2. Because if $m = 2^p$, then $h(k) = k \mod m$
  is just the $p$ lowest order bits of $k$ (the higher bits are irrelevant)
- $m$ should not be a power of $10$ if the application deals with decimal
  numbers. Actually, it should **not be a power of whatever you use as a base**,
  for the same reason above.
- Good values of $m$ are **primes**.
- If it is too difficult to use a prime, pick $m$ such that it has no prime
  factor less than $20$ (e.g. $23 \times 31$).

For example of designing this kind of hash function, see _slide 24 of 04
Hashing_.

### Open Addressing (Closed Hashing)

#### Open Addressing

All elements are stored directly in the hash table. Empty slots store a special
value (called **_NIL_**) indicating that (e.g. an invalid key value).

#### No pointers

Open addressing avoids pointers:

- Saves time (no need memory (de)allocation
  - Also, when using arrays, the information is stored in consecutive memory
    locations, which is much faster to access than using linked list.
- Saves space (4 bytes for 32-bit machines, 8 bytes for 64-bit machines)

#### Collision resolution: probing

If there is a collision during inserting elements, alternative slots are tried
until an empty slot is found (**_probing_**).

The **_probe sequence_**: `<h(k,0), h(k,1), ..., h(k,m-1)>`, where:

$h(k,i) = [h_1(k) + f(i)] \mod m$, where $f(0) = 0$.

The probe sequence should be a permutation of `<0,1,...,m-1>`, so that every
slot is included. (Or at least most slots)

#### Operations implementation

For inserting, searching and deleting, all follow probe sequence. But deletion
needs special care.

##### Searching

Follow the probe sequence, until the key is found or `NIL`.

If the probe sequence is really good without overlap, then you only need to go
through m steps to search the who table. If the probe sequence is not ideal, you
may or may not want to go through steps $> m$, e.g. $2m$. (You may say goes
through $m$ is already enough.)

##### Insertion

Follow the probe sequence, until the slot is `NIL` or `DELETED` and use that
slot to insert.

Otherwise, throw hash table overflow error (which may not really mean the table
is full, especially when the probe sequence is not ideal, i.e. doesn't exhaust
the whole table).

**_NOTE_** that even encounter a `DELETED` slot, you should still follow the
probe sequence to check if there is duplicate.

##### Deletion

The entry to be deleted cannot just be replaced with `NIL`, because it causes
the "chain" to other entries that collided with the current entry broken such
that those other entries can be accessed no more.

One solution is to replace with the deleted slot with **another special value**
called `DELETED`, or we call them **_tombstone_**.

After a series of insertion and deletion, the length of probe sequence needed to
follow will increase because of more tombstones. The length will tend to
approach an equilibrium (as new insertion replaces the tombstone). But the time
will still be couple times longer than the initial state. To solve this problem
there're two solutions:

1. Do a **local reorganization upon deletion** to shorten the average path
   length. For example, after deleting a key, continue to follow the probe
   sequence of that key and swap records further down the probe sequence into
   the slot of recently deleted record (being careful not to remove any key from
   its probe sequence). This will not work for all collision resolution policy.
2. **Periodically rehash** the table by reinserting all records into a new hash
   table (or **once it accumulates too many tombstones**).

#### Load factor

Generally, the load factor should be **below 0.5**. That means at every step,
there are more than 0.5 probability to find an empty slot.

### Choosing Probe Sequence

#### Linear probing

$h(k,i) = [h_1(k) + i] \mod m$.

Advantages:

- This probe sequence exhaust the whole table.
- Easy and fast to compute.

Disadvantages:

- Elements tend to cluster together in the table: called **_primary
  clustering_**.
  - This also increase the tendency to grow the cluster, for when a key hit
    anywhere in the cluster, it is added to the end of the cluster.
  - This makes search inefficient. See _slide 46_ to see the inefficiency. It
    makes the search $O(n)$, similar to linked list.

#### Quadratic probing

$h(k,i) = [h_1(k) + i^2] \mod m$.

Advantage:

- The jump size grows much faster than the steps, to **prevent primary
  cluster**:

  $i^2 \gg i$, so it can jump out of the local cluster soon.

Disadvantage:

- The probe sequence **does not span** the whole table.
  - To alleviate this, the table size should be a **prime** number and that it
    should **never be > half full**.
- Two keys with the **same starting point** will have **same probe sequence**
  (called **_secondary clustering_**)

#### Double hashing

To make the probe sequence even more irregular than quadratic probing.
$h(k,i) = [h_1(k) + i \cdot h_2(k)] \mod m$.

- In general, $h_2(k)$ should not be a large factor of $m$. So it is also a good
  idea that $m$ is a prime number.
  - if $h_2(k)$ is co-prime with $m$, then the probe sequence can span the whole
    table.
- $h_2(k)$ must never be 0, and should not be 1 to prevent clustering.

### Rehashing

To control the load factor < 0.5, we rebuild a larger hash table by
**_rehashing_**.

#### New table size

Double the old size, and use the next good size (e.g. prime, not close to power
of 2 and 10, etc...)

## Tree

### Definition of a Tree

A **_tree_** is a collection of **_nodes_** in a **hierarchical** structure. One
node is the **_root_** with no parent. Once we identify the node, the
**_parent-child_** relation between two nodes connected by an **_edge_** (or
called a **_link_**) is well-defined.

Formally a tree can be defined **recursively**:

- A single node is a tree, with that node being the root.
- Suppose $n$ is a node and $T_1, T_2, \dots, T_k$ are trees with roots
  $n_1, n_2, \dots, n_k$, respectively. If we make $n$ the parent of
  $n_1, \dots, n_k$, the resulting structure is a tree. In this tree, $n$ is the
  root and $T_1, \dots, T_k$ are the **_subtrees_** of the root.
- Sometimes, we want to extend our definition from having at least one node to a
  set with no nodes - called the **_null tree_**.

### Terminology for a Tree

#### Ancestors, descendants and path

If $\left< n_1, n_2, \dots, n_k \right>$ is a sequence of nodes in a tree such
that $n_i$ is the parent of $n_{i+1}$ for $1 \leq i < k$, then this sequence is
called an **_ancestor-descendant path_** from $n_1$ to $n_k$.

The **_length_** of a path is the number of **links** in the path. I.e.
$\left<
n_1, n_2, \dots, n_k \right>$ is $k-1$.

If there is a path from node $a$ to $b$, then $a$ is an **_ancestor_** of $b$,
and $b$ is a **_descendant_** of $a$. Hence, for example, a node is an ancestor
of itself. A **_proper ancestor/descendant_** is an ancestor/descendant that is
not the node themself.

A node with **no children** is called a **_leaf node_** (or called an
**_external node_**). A node that is not a leaf node is called an **_internal
node_**.

A **_subtree of a tree_** is a node of the tree together with all its
descendants.

Two nodes are siblings if they share the same parent.

#### Degree and Depth

The number of nodes adjacent to $x$, including its parent if there's one, is
called the **_degree_** of $x$.

The length of the path from the root $r$ to a node $x$ is the **_depth_**. Root,
therefore, has depth 0.

The **_height_** of a tree is the largest depth.

A collection of trees is a **_forest_**.

In an **_ordered tree_**, the children of a node are ordered. The following two
ordered trees are different.

```text
 a   a
┌┴┐ ┌┴┐
b c c b
```

### Amount of Edges

A tree with $n$ nodes has $n-1$ edges. You can prove this by M.I, or structural
induction.

### Tree Implementation

#### Ordered Tree

If we know the max amount of children of each node, we can use an array of fixed
size:

```c
struct tree_node {
  element ele;
  tree_node *child[4];
}
```

Otherwise, we use a linked list

```c
struct tree_node {
  element ele;
  tree_node *right_sibling;
  tree_node *leftmost_child;
}
```

#### Children Map

We can use a map (e.g. a hash table, or a binary search tree) to link children
node from a parent, especially for unordered tree.

```text
struct tree_node {
  element ele;
  // There should be a field `key` here, or a key should be derivable from ele
  Map<key, tree_node> children;
}
```

Sometimes, you may want to have a sentinel root node that contains no element.
See my implementation for Oursky developer pretest 2023 question 1.

### Tree Traversal

- Preorder and postorder are two kinds of depth-first search
- E.g. copying files and folders in a file system uses preorder
- E.g. deleting files and folders in a file system uses postorder

#### Preorder

$O(n)$. A kind of depth-first search. Visit the parent first before its
children.

```text
PREORDER(node) {
  if (node == NULL) return;
  visit node;
  /* Suppose node has k subtrees T1 ... Tk ordered from left to right */
  for (i = 1 to k) PREORDER(T_i);
}
```

#### Postorder

$O(n)$. A kind of depth-first search. Visit children first before the parent.

```text
POSTORDER(node) {
  if (node == NULL) return;
  /* Suppose node has k subtrees T1 ... Tk ordered from left to right */
  for (i = 1 to k) POSTORDER(T_i);
  visit node;
}
```

#### Inorder

$O(n)$. For [binary trees](#binary-tree) only.

```text
INORDER(node) {
  if (node == NULL) return;
  INORDER(node->left_subtree);
  visit node;
  INORDER(node->right_subtree);
}
```

### Binary Tree

A **_binary tree_** is a tree in which all nodes have at most two children, a
left and a right child. A **_full binary tree_** is a binary tree with no node
that has only single child. A **_complete binary tree_** is a full binary tree
whose leaves are all at the same depth (at height $h$).

A complete binary tree of height $h$ has $\sum^{h}_{i=0} 2^i = 2^{h+1} - 1$
nodes.

```text
struct tree_node {
  element ele;
  tree_node *left;
  tree_node *right;
}
```

## Searching an Array

### Jump Search

Requires sorted list. Jump k index to filter/locate a segment of k elements,
then do linear search on that segment once the segment is located (or linear
search on the last remaining segment if jumped through all segments).

### Square-root Search

A variant of Jump search where the steps of each jump is about $\sqrt{n}$.
Requires sorted list. Takes $O(\sqrt{n})$ time.

### Binary Search

$O(\log n)$

```text
BinarySearch(L, k, low = 0, high = L.length - 1) {
  while (low <= high) {
    mid = floor((low + high) / 2)
    case L[mid] {
      < X: low = mid + 1;
      == X: return mid;
      > X: high = mid - 1;
    }
  }

  return -1;
}
```

### Interpolation Search

The average time is $O(\log \log n)$.

We guess where in the array $L is the target $k$ likely to be. For example, if
the array more or less uniformly partitions the set of numbers, then $k$ should
be near $L[pm]$ where $p = \frac{k - L[0]}{L[n-1] - L[0]}$.

### Binary vs Linear Search

For a sorted array, insertion and deletion takes $O(n)$ time. So if $n$ is small
or if search is done rarely, linear search is better. Binary search is better if

- Search is done very often
- There is a lot of data
- Data is relatively static (doesn't change much)

### Binary Search vs Hashing

Hashing is more difficult to design, but in general gives the best average
search time. However, hashing uses more memory and does not support
**range-search** efficiently.

### Lecture Notes

##### p.3

For open addressing, the propability to hitting empty slot is (1-a). When a is
small, the taylor series: 1/(1-a) = 1+a.

##### p.5-7

When the elements are not structured, then searching through other slots give
you **no information** about the content in slot i.

- So it has to be O(n).

##### missing part

jump search to binary search

- ?? - 1:06

##### p.23

My thought, use binary search until a point where it is more suitable to use
jump search.

Facts:

- Must always find critical height
- Minimize worst-case of no. of probs.
- You find a critical height when you find a please where egg doesn't break and
  that the next place it breaks.

1.  jump search: 10: worst case 50 + 9 times
2.  1 binary 9 jump: worst case 1 + 250/9 + 7 = 1 + 27 + 8 = 36
3.  2 - 8: worst case: 2 + 125/8 + 6 = 2 + 15 + 6 = 23

Lecturer:

- T(1 egg, n floors) = n (linear search)
- T(2,n) ≤ 2 √n
  - Choose step size to be p, p =~ √n
  - How to prove this is the lower bound: √n?
    - Scenario 1: Egg never breaks
      - After first probe, the algorithm will know it must be the upper
        positions. Then again... Until the highest floor is probed.
      - Let say the best algorithm makes L probes. Then the max gap x[max]
        between two probes is n-L.
      - average gap length = n/L - 1
      - ∃ i st x[i] ≥ n/L -1

## Binary Search tree

### Binary Search Tree

To be able to do binary search in a sorted linked list, we need to have a
pointer to the middle node. Now we take that node to be the root, and
recursively do the same thing in the subtrees, we got a binary search tree which
supports:

- search
- minimum
- maximum
- predecessor
- successor
- insert
- delete

All in $O(log n)$ worst case time if the tree is **kept balanced**.

#### Properties

If $x$ is a node, then $x$'s key > all keys in $x$'s left subtree and < all keys
in $x$'s right subtree.

### Implementation

#### Minimum

```c
Node* TreeMinimum(node* T) {
  if (T = nullptr) return nullptr;

  while (T -> left != nullptr) T = T -> left;
  return T;
}
```

#### Search

```c
Node* TreeSearch(node* T, key k) {
  if (T == nullptr) return nullptr;

  while (x -> left != nullptr) {
    if (k = x -> key) return x;
    x = k < x -> key ? x -> left : x -> right;
  }
  return nullptr;
}
```

#### Successor

If $x$ has a right subtree, $x$'s successor is the minimum node in its right
subtree. Otherwise, it's the first parent (from **left child relation**) that
has a larger key.

Find the first parent from **left child relation**:

```c
Node* Successor(node* x) {
  if (x -> right != nullptr) return TreeMinimum(x -> right);

  Node* y = x -> parent;
  while (y != nullptr && x == y -> right) {
    x = y;
    y = y -> parent;
  }

  return y;
}
```

Find the first parent with a **greater key**:

```c
Node* Successor(node* x) {
  if (x -> right != nullptr) return TreeMinimum(x -> right);

  Node* y = x;
  while (y -> parent != nullptr) {
    if (y -> parent -> key > x -> key) return y -> parent;
    y = y -> parent;
  }

  return nullptr;
}
```

#### Insertion

To insert a node $z$, we pretend that $z$ is already in the tree and "look for
it". When we encounter a `nullptr`, that is where $z$ is supposed to be.

```c
bool TreeInsert(node* &T, node* x) {
  Node* prev = nullptr;
  Node* curr = T;

  while (curr != nullptr) {
    if (x -> key == curr -> key) return false;
    prev = curr;
    curr = x -> key < curr -> key ? curr -> left : curr -> right;
  }

  x -> parent = prev;
  x -> left = nullptr;
  x -> right = nullptr;

  if (prev == nullptr) {
    T = x // T passed by reference
  } else if (x -> key < prev -> key) {
    prev -> left = x;
  } else {
    prev -> right = x;
  }

  return true;
}
```

#### Deletion

To delete a node $z$ from the tree, we need to consider 3 cases:

1. $z$ has no children

   We simply remove $z$ and set the pointer to $z$ to `NULL`

2. $z$ has one child

   We **_splice out_** $z$, i.e. linking $z$'s parent to $z$'s child.

3. $z$ has 2 children

   We find the successor of $z$, $y$, in the right subtree, and then use it to
   replace $z$'s position, and splice out $y$.

##### Lecture Notes Implementation

```c
Node TreeDelete(Node* &T, Node* z) {
  Node* y; // Node to be spliced out

  if ((z -> left == nullptr) || (z -> right == nullptr)) {
    y = z;
  } else {
    y = TreeMinimum(z -> right);
    z -> key = y -> key;
    /* copy other fields too */
  }

  Node* x; // Child node of y
  if (y -> left != nullptr) {
    x = y -> left;
  } else {
    x = y -> right;
  }

  if (y -> parent == nullptr) {
    T = x;
  } else {
    if (y -> parent -> left == y) {
      y -> parent -> left = x;
    } else {
      y -> parent -> right = x;
    }
  }

  x -> parent = y -> parent // Dunno why lecture note doesn't have this line

  // return y, or free y, ...
}
```

##### My Implementation

```c
bool SpliceOutNode(Node* &tree, Node* z) {
  if (tree == nullptr || z == nullptr) return false;
  if (z -> left != nullptr && z -> right != nullptr) return false;

  Node* child = z -> left == nullptr ? z -> right : z -> left;

  if (z -> parent == nullptr) {
    tree = child;
    return true;
  }

  if (z -> parent -> left == z) {
    z -> parent -> left = child;
  } else if (z -> parent -> right == z) {
    z -> parent -> right = child;
  } else {
    return false; // perhaps throw an error
  }

  child -> parent = z -> parent;
  return true;
}

bool DeleteNode(Node* &tree, Node* z) {
  if (z == nullptr || tree == nullptr) return false;

  // case 1 & 2: z has only one or no child
  if (z -> left = nullptr || z -> right = nullptr) {
    if (!SpliceOutNode(T, z)) return false;
  } else { // case 3: z has two children
    Node* successor = TreeMinimum(z -> right);

    if (!SpliceOutNode(T, successor)) return false;
    successor -> parent = z -> parent;
    successor -> left = z -> left;
    successor -> right = z -> right;

    if (z -> parent -> left = z) {
      z -> parent -> left = successor;
    } else if (z -> parent -> right = z) {
      z -> parent -> right = successor;
    } else {
      return false; // perhaps throw an error
    }
  }

  delete z;
  return true;
}
```

### Time Complexity

The time complexity of the operations are $O(h)$ where $h$ is the height of the
tree. For a balanced binary tree the height is $O(\log n)$, which is the same
for the average height of a randomly built binary tree. But our **deletion**
algorithm favours making the left subtrees deeper than the right, so the average
height is no longer $O(\log n)$. One way to improve this is to not always use
the successor to replace a deleted node, but also a "presessor".

### A Theorem

Let $r$ be the root of a tree and $x$ be a node in the tree. Let $T(x)$ denote
the tree rooted at $x$. Theorem:

$\forall m \in ( T(r) - T(x) )$ \
$\forall y \in T(x)$ \
either $x, y < m$ or $x, y > m$.

The intuition is this. Consider any subtrees $T(x)$ and any node $m$ external to
$T(x)$, we can always find their most near common ancestor $p$ (could be $m$
itself).

If $p$ is $m$, then $T(x)$ is in one subtree of $m$ and so $x$ and any
$y \in T(x)$ must be smaller or larger than $m$.

If $p$ is not $m$, then one of them ($x$ and $m$) is in $p$'s left subtree and
the other in the right subtree. So that $x$ and any $y \in T(x)$ must be either
smaller than $m$ or larger than $m$.

### Lecture Notes

##### p.5

Sometimes the field "parent" is optional

- E.g. for C and C++ the recursion already stacks the nodes of the parents.

##### p.6

- The property should be satisfied for all subtrees.
- And if keys are not distinct, the strictly relation may be not strict relation

##### p.7

This is in-order search. The algorithm is:

Recursively call the left subtree, then the root, then recursively call the
right subtree.

##### p.11

If there is a right child, the minimum value of the right subtree. If there is
no right child, then it is the first parent (from **left child relation**) that
has a larger key.

##### p.28

Use recursive function, see if the left and right subtree are also binary search
tree, and if their maximum height is the same.

```c
bool IsBst(Node* root) {
  if (root == nullptr) {
    return true;
  }

  bool is_left = IsBst(root -> left); // optimization: if at any recurssive
  //level IsBst is false, then immediately end recursion and return false

  bool is_right = IsBst(root -> right);

  int max_left = TreeMax(root -> left); // TreeMax(nullptr) = -ve infinity
  int min_right = TreeMin(root -> right); // TreeMin(nullptr) = +ve infinity

  if (max_left -> key < root -> key && min_right -> key > root -> key)
    return true;
  return false;
}
```

if (? max n = n), then "?" should be -∞. This gives you the base case.

##### p.29

Recursion again: middle element is root, then the left side is the left subtree,
and right side is the right subtree.

Base case: empty, if i > j

[\node* CreateBst(A, i, j) { ]\

[\ if (i > j) return null; ]\

[\ int k = (i+j) div 2; // middle elem. div~: round down to closest integer]\

[\ node* p = new node; ]\

[\ p -> key = A[k]; // and copy any other data ]\

[\ p -> left = CreateBst(A, i, k-1); ]\

[\ p -> right = CreateBst(A, k+1, j); ]\

[\ return p; ]\

[\} ]\

[\root = CreateBst(A, 0, n-1); ]\

This runs O(n);

##### p.30

A trivial solution is to make an array out of the linked list and use previous
algorithm. But this uses extra space. Also still wants to be efficient O(n).

Now makes the **unbalanced** binary tree to a **balanced** binary tree.

- Should not create new nodes and new arrays.

It would be easier to know how many (non-empty) nodes are there. Again, we use a
recursion similar to the previous algorithm.

[\n = Count(head); // count number of nodes. (exercise) ]\

[\ ]\

[\// Takes the first k nodes of a list and return a balanced tree ]\

[\// head is passed by reference, so that after traversing k nodes, the head]\

[\// pointer will be updated to point to the remaining list. ]\

[\node* MBalance(node*& head, int k) { ]\

[\ if (k == 0) return null; ]\

[\ int num_left = (k-1) div 2; ]\

[\ // the following two statements are separated for safety (head updating)]\

[\ node* left_root = MBalance(head, num_left); ]\

[\ head -> left = left_root; ]\

[\ ]\

[\ node* root = head; // save the root that will be returned. ]\

[\ ]\

[\ head = head -> right; ]\

[\ int num_right = k - (num_left + 1); ]\

[\ node* right_root = MBalance(head, num_right); ]\

[\ root -> right = right_root; ]\

[\ return root; ]\

[\} ]\

- Run time is O(n) (proof by induction).
- Each recursive level use O(1) space, there is O(log k) recursion, so totally
  O(log k) space.

### Past paper

##### (a)

if (root == null) return 0; return (root -> key + Sum(root -> left) + Sum(root
-> right));

##### (b)

if (root == null) return 0; int temp_root = 0; if (a ≤ root->key ≤ b) temp_root
= root->key; return (temp_root + SumRange(root->left, a, b) +
SumRange(root->right, a, b))

// You can do some optimization

##### (c)

if (root == null) return null; if (root->key > b) return MaxRange(root->left, a,
b); node\* temp_right = MaxRange(root->right, a, b); if (temp_right != null)
return temp_right; if (a ≤ root->key) return root; return null;

##### (d)

if (root == null) return null; node* temp_left = Transform(root->left, a, b);
node* temp_right = Transform(root->right, a, b); root -> key = a \* root ->
key + b; if (a < 0) { root->left = temp_right; root->right = temp_left; } return
root;

## L8 Balanced Tree

### AVL Tree

#### Definition

For any node, the height of its left and right subtree can differ by at most
**1**. (The height of a null tree is -1)

Note that AVL tree is not the most balanced binary search tree.

#### Motivation

It is costly to make a binary search tree as balance as possible (the number of
nodes at either subtree is as close as possible). So a less stringent condition
is provided for [[AVL tree]].

#### Height bound

##### max height of n-node AVL tree

1.44 lg(n+1) - 1.283

- O(lg n)

##### average height of n-node AVL tree

lg(n+1) + 0.25

- O(lg n)

#### Proof AVL has height O(log n)

Note the follow are two similar claim:

- With large n, the height is small: O(lg n)
- With huge h, the least possible number of nodes is much larger F(h).

See _p.5_.

#### Complexity

Worst case for search is O(lg n). But insertion and deletion now needs more time
for **rotation** to keep the tree balanced.

### Rotation

#### Definition

Rotation upon 2 nodes k₁ and k₂ **reverse their parent-child relation**.

      │     ────────────────────>       │ k₂    Right rotation at k₂        k₁ ┌───┴─┐                           ┌───┴───┐ k₁    Z                           X       k₂ ┌─┴─┐       Left rotation at k₁         ┌───┴───┐ X   Y      <────────────────────        Y       Z

##### right rotation

**Left** child becomes the parent. Left (right) subtree becomes shorter (taller)
than before.

##### left rotation

**Right** child becomes the parent. Right (left) subtree becomes shorter
(taller) than before.

##### effect

Note that in right rotation, it only decrease the height of the left branch
(k₁ - X) by 1, the height of the branch down Y is unchanged, also the height in
branch Z is increased by 1. So if the tree is unbalanced because of Y, you can't
solve the height difference with one single rotation.

#### Failure of single rotation

    3   ────────────────────>  1 ┌───┘   Right rotation at k₂   └───┐ 1                                  3 └─┐                              ┌─┘ 2     Left rotation at k₁      2 <────────────────────

The core problem is that the subtree Y (2 in the two cases) is taller than its
sibling subtree X or Z (left null node under 1 or right null node under 3 in the
two cases)

Remember the height of the branch down Y is unchanged with one single rotation.
For it only passes the problem of Y to another node.

#### Double rotation

      │ k₂ ┌───┴─┐ k₁    Z ┌─┴─┐ X   Y So if Y is taller than X, do a left rotation at k₁ first to make X and Y balanced, and then do the rotation at k₂.

##### e.g. left-right double rotation

    3  ────────────────────>     3  ────────────────────>    2 ┌───┘  Left rotation at 1      ┌─┘  Right rotation at 3    ┌─┴─┐ 1                              2                           1   3 └─┐                          ┌─┘ 2                          1

### Maintaining Balance

#### Which node may have their balancing condition violated?

- The ancestor**s** of the newly inserted node.
- When a node violates the balancing condition, at most you only need to perform
  a double rotation (but you don't need to trace further back and perform more
  rotations).

#### Insertion General Approach

1.  Follow the algorithm of binary search tree insertion
2.  Go from inserted node up to the root through parent pointers, updating the
    height information of the nodes along the path
    - **_NOTE_** that depending on your implementation, you may not really
      tracing from the new leaf to the root. When you make an insertion by
      **recursion**, you are tracing from the root to leaf, and each recursion
      will fix the AVL tree at that node. If you use this method, make sure when
      a recursive call returns, you need to pass all necessary information.
      - Since you can compare the key of the newly added node and that of the
        child, you don't even need to pass information up in order for the
        ancestor to know whether the direction changed and thus need a double
        rotation.
3.  If a node on the path violate AVL condition due to insertion, perform a
    rotation (single/double) to restore balance.

#### Node definition for AVL tree

[\struct tree_node { ]\

[\ int key; ]\

[\ tree_node* left; ]\

[\ tree_node* right; ]\

[\ tree_node* parent; // This is not necessary for recursion insertion]\

[\ int height; // May not be needed. Just info to check AVL condition ]\

[\} ]\

- For int height: AVL condition has only three cases: 2 bits is only needed, and
  further optimization may reduce to 1 bit (use red-black tree). But this space
  saving is not be important most of the time.

##### red-black tree

For leaf node, you don't need info to see if it is balanced. So they can
contribute the memory (for AVL condition) for parents. So a node storing the bit
"1" means this branch is deeper from the point of view of the parent. The bit
"0" means this branch is NOT deeper (may be equal or shallower) than the other
branch from the point of view of parent.

#### Changes to be made in rotation

      │     ────────────────────>       │ k₂    Right rotation at k₂        k₁ ┌───┴─┐                           ┌───┴───┐ k₁    Z                           X       k₂ ┌─┴─┐       Left rotation at k₁         ┌───┴───┐ X   Y      <────────────────────        Y       Z

- For k₁
  - A's parent pointer
  - A's right child pointer
- For k₂
  - B's parent pointer
  - B's left child pointer
- For ancestor of k₂
  - The left or right pointer
- For root node of Y
  - The parent pointer

#### Implementation of rotation

##### p.33

- T is passed by reference **_DUNNO_** but I guess T is root of the whole tree
- x->p is the parent of x
- Height of x, y and the nodes above may need to be updated. May be you want to
  update the height else where but not in the function for rotation.

##### my left rotation

[\LR(T, x) { ]\

[\ y = x -> right; ]\

[\ if (x -> p == NULL) { ]\

[\ T = y; ]\

[\ } else { ]\

[\ if (x -> p -> left == x) { // x is a left child of its parent]\

[\ x -> p -> left = y; ]\

[\ } else { // x is a right child ]\

[\ x -> p -> right = y; ]\

[\ } ]\

[\ } ]\

[\ y -> p = x -> p; ]\

[\ x -> p = y; ]\

[\ x - > right = y -> left; ]\

[\ if (y -> left != NULL) { ]\

[\ y -> left -> p = x; ]\

[\ } ]\

[\ y -> left = x; ]\

[\} ]\

##### my right-left rotation

[\RLR(T, x) { ]\

[\ y = x -> right; ]\

[\ RR(x -> right, y);]\

[\ LR(T, x); ]\

[\} ]\

#### Deletion

1.  Delete the node as in a binary tree.
2.  Go up to the root from the parent of the deleted node and do:
    - update the value of height
    - perform rotation to restore balance if the node violate AVL tree condition

## Sort

### Why Sorting

- Make searching efficient
- Check duplicates

When evaluating different sorting algorithms, consider two implementations:

- Array
- Linked list

### Bubble Sort

#### Algorithm

[\BubbleSort(arr) { ]\

[\ for (int i = len; i > 2; --i) ]\

[\ for (int j = 1; j < i; ++j) ]\

[\ if (arr[j] > arr[j+1]) ]\

[\ Swap(arr[j], arr[j+1]); ]\

[\} ]\

#### Complexity analysis

- Time complexity: Θ(n²)
- Average number of swaps: nC2 / 2 = n(n-1)/4 = Θ(n²)
  - nC2 is the number of combinations of choosing 2 out of n items
  - nC2 = n(n-1)/2

### Insertion sort

#### Idea

Maintain a sorted list. At each step, pick a number from the input, and insert
it into the list in its proper place.

#### Algorithm

[\InsertionSort(arr) { ]\

[\ for (int i = 1; i <= len; ++i) { ]\

[\ j = i - 1; ]\

[\ while ((j >= 0) && (arr[j] > arr[j+1])) {]\

[\ Swap(arr[j], arr[j+1]); ]\

[\ j--; ]\

[\ } ]\

[\} ]\

#### Complexity analysis

- Worst case time complexity:

  ∑[i=1, n-1] i = (n-1)n/2 = Θ(n²)

- Average case complexity: (I think it means average no. of swaps) ∑[i=1, n-1]
  i/2 = (n-1)n/4 = Θ(n²)
- Best case time complexity: Θ(n) because list is already sorted.

##### opinion

- Even you could use binary search to find the position of insertion in O(log n)
  time, but you still have to shift the array in order to make make space for
  insertion which is O(n). Therefore, here we still use binary comparison to
  find (and swap at the same time) the insertion position anyway.

##### advantage

- The advantage is, if the given array is already sorted (or partially sorted),
  then the time needed to do insertion sort is reduced. I.e. benefits from the
  fact that the input list is (partially) sorted
- Can be done **on-line**. I.e. can start sorting without obtaining the whole
  array.

### Sort That Swaps Adjacent Elements

Always has an average **case** complexity of Ω(n²). To beat the Ω(n²) bound, a
sorting algorithm must swap elements that are **far apart**:

i.e. must **eliminate more than one inversion per swap**.

### Selection Sort

#### Analysis

- The number of sort is O(n). Better.
- Still time complexity is O(n²). The same. This is because you need to make
  O(n²) comparisons: ∑[i=1, n-1] i = (n-1)n/2.

### Heap Sort

#### Heap

A [[heap]] is a **binary tree** with [[heap property]]:

- Value of any node **≥** values of its children (max heap)
  - (For other propose, the property may be ≤ instead. (min heap))

#### Algorithm

[\HeapSort(arr) { ]\

[\ BuildHeap(arr); ]\

[\ for (i=1; i<=n; ++i) { ]\

[\ put largest element in arr[n-i+1];]\

[\ rebuild heap; ]\

[\ } ]\

[\} ]\

- Each iteration picks the largest element (the root of heap) and put into the
  last spot of unsorted portion of the array.
- If you use pointers for the tree, it takes many extra space.

#### Fixing a heap

- Remove the root (swapping it with the last position of the unsorted portion)
- Swap the new root with its largest child (if the heap property is violated)
- Keep swapping it with its largest child until heap property is restored.

#### Time complexity

- O(nm), where m is the height of heap = lg(n) ideally.
- BuildHeap() = O(n)
- For loop = O(n log n)

#### Space complexity

- Naive way to build the tree cost extra O(n) space.

Instead, we can just use the original array itself. And applying arithmetic with
the indices of an array (starting from 1) can easy endorse a binary tree
structure:

- Given a node A[i], its children (if they exist) are A[2i] and A[2i+1].
- The parent of node A[j] is A[j/2]

So there is O(1) extra space.

#### Heapify

- Subarray from index i to n has the heap interpretation.
- You call Heapify if a subtree rooted at i has heap property violated exactly
  at A[i] (but everywhere else the property is maintained) (assume the property
  is maintained everywhere else beyond the subtree as well).
- So to build a heap (as a left-complete binary tree), we do it incrementally
  from the bottom level.
  - I.e. start with the node with the largest index and has children.
- Time complexity of building heap is O(n)

In _p. 45_ you can do recursion instead of while loop, but using recursion will
use more extra space O(height).

#### AVL tree

If you don't care extra space. You can just build an AVL tree, and you just do
an inorder visit to sort the array with O(n log n).

- But it uses extra O(n) space.

### Priority queue

#### Idea

- Each element's key has a notion of priority.
- Each removal, always remove the most front element with highest priority.

#### Usage example

- Implement to pick a job with highest priority.

#### Pages

_p. 54_ there can be an extra function: change priority of a node.

### Merge sort

#### Idea

1.  Divide the list into 2 roughly equal parts
2.  Sort the 2 parts individually
3.  Merge them afterwards

Depth of recursion is O(log n)

#### Space required is different depending on input

If the input was:

- array: O(n) extra space, because when merging, we need to compare two parts
  and copy one element each time to the new array (represented as sorted array)
- Linked list: O(1)

#### Algorithm

[\MergeSort(arr, low, high) { ]\

[\ /_ sort the subarray arr[low..high] _/ ]\

[\ if (high > low) { ]\

[\ mid = floor((low+high)/2); ]\

[\ MergeSort(arr, low, mid); ]\

[\ MergeSort(arr, mid+1, high); ]\

[\ Merge(arr, low, mid, high); // O(n) time + extra space ]\

[\ /_ merge the lists arr[low..mid] and arr[mid+1..high] _/]\

[\ } ]\

[\} ]\

#### Time complexity

For the above algorithm:

- T(n) = 2T(n/2) + O(n) = O(n log n)

#### Space complexity

- S(n) = max{S(n/2), O(n)}

### Quick Sort

#### Advantage

On average it is better than merge sort. But in some case it can be worse than
merge sort.

#### Idea and goal

- Want to use the idea of Merge Sort, but don't want the Merge part.
- To do this, we put the linear operation (i.e. O(n)) before the recursive sort:
- Pick any element in A, call it the pivot v.
- Move all elements ≤ v into the left of v.
- Move all element ≥ v into the right of v.
- Then recursively do a quick sort on both parts.

#### Algorithm

Partition(arr, p, r) { /_ partition the array segment arr[p..r] _/ }

#### Complexity

- Space O(d), where d is the depth of recursion
- Time:
  - Worst case is that the input is already sorted: O(n²)
    - So if the partition is uneven.
  - Average case complexity is O(n lg n)

### Count Sort

#### Not care auxilary data

- If you do not care about the auxilary data
- You just count how much elements is for a particular key, then generate a new
  array printing that number of keys from small to large key.
- O(n) time complexity

#### Stable sorting

- The relative order of elements with the same key is maintained in the output.

#### Counting sort

##### idea

- If we know 3 elements smaller than element x, then x must be at the 4th
  position.

##### assumption

Input is given in an array: [\A[1..n]]\. The output is [\B[1..n]]\ takes up
**extra space**. [\C[1..k]]\ is an array for counting.

##### algorithm

_slide p.32_ REMEMBER that the index (and key) starts from 1, not 0.

1.  initialize the count to 0.
2.  count the occurrence of each key. [\C[A[j].key]++]\
3.  update C[] to have cumulative sum.
4.  After copying the element to a correct position, lower the counter for that
    key by 1.
    - Scanning backward to achieve **stable sorting**.

##### time complexity

- O(n).
- This breaks the limit of Ω(n lg n) because we are not using pair-wise
  comparison algorithm, but exploiting the structure of the keys.

#### Linked list counting sort

1.  Create an array of k element: B[1..k]
2.  Iterate through the input list, and append each node to the appropriate
    linked list rooted at the appropriate element in B[1..k].
3.  Concatenate the k linked list in B[1..k].

### Radix Sort

Each key is [1..d^k]. So the range is large. But we can represent the integer in
base k.

#### Idea

- Each digit will be one iteration of counting sort.
- Sort the least significant digit first, most significant digit last.
- At each iteration, one particular digit is considered as **key**, other digits
  are considered **auxiliary data**. And do a **count sort**.

So there is d instances of count sort.

#### complexity

##### time complexity

- O((n+k)d) = O(nd) if k ≤ n. k is the base of the integer representation. k
  should never be ≥ n lg n.

##### space complexity

- Array: O(n+k)
- Linked list: O(k)

### Meta Analysis of Sorting Algorithm

- If you use comparison-based sorting, time complexity is Ω(n lg n).
- To break this limit, you must exploit some structure of the key, e.g. the
  known range of the keys.

### Past paper (Heap Sort)

#### 4e

- Observe that, in input, min. element must be in A[1..1+B] (inclusive).
- So we can use O(B) space to build a min heap with size ≤ B+1, from A[1..1+B].
  This gives us an array H[1..1+B].
- In each iteration:
  - Remove min.element from heap H, and insert back at the next available sot in
    A.
  - Insert the next element in the unprocessed part of A into H
  - Maintain heap.

### Questions

##### bubble sort

- The proof assumes the keys are unique?

## Huffman coding (Related to exam)

- To be able to uniquely decode an encoding, you need the encoding to be
  **prefix-free**.
  - My phone number 99943210 (999 - the prefix - is itself a phone number) is
    not prefix-free.
- If it is prefix-free encoding, the encoding can be represented by a binary
  tree.
- The **cost** of a particular encoding is the leaf depth times the (relative)
  frequency of this encoding
  - The average depth/length is the average cost calculated from relative
    frequency.

### Building an Efficient Tree Representation

- Sort the weight (frequency) of all encodings and sort them in ascending order.
  - Done by using min heap.
- The two least frequent encoding form a pair of siblings by creating a parent
  whose weight is the sum of the two weights of the siblings.
  - First remove the two encoding from the heap.
  - Add this parent node (with the sum of two weightings) back to the min heap
    and rebuild it. So that the process can be repeated to create an optimally
    efficient tree representation.

## Self note

To figure out an (best) algorithm

- 1st think about possible situations
- Consider extreme cases / limiting case
- How to distinguish it from normal/general case
