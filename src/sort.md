Sorting algorithms.

To understand them, see how they work step by step on examples.

#sources

- <https://en.wikipedia.org/wiki/Sorting_algorithm#Comparison_of_algorithms>

    Compare them all

#the best algorithm

See this: <https://en.wikipedia.org/wiki/Sorting_algorithm#Comparison_of_algorithms>

It can be proven that the best a general algorithm can do is $O(n log(n))$ worst case time.

There are many algorithms that sort in-place, thus achieving $O(1)$ memory worst case.

There are algorithms that achieve both optimal time and space at the same time such as heapsort.

Another parameter to take into account is stability. Heapsort which achieves both $n log(n)$ worst case time
and $O(1)$ space is not stable.

TODO is there a stable algorithm that achieves $n log(n)$ time $O(1)$ space *and* is stable?

In practice, forgetting assymptotic worst cases merge sort ($n lon(n)$ time worst case $n$ space)
and quicksort ($n log(n)$ time average) have good performance and both have stable imlementations.

If extra information is known about the input, it is possible to reduce time worst case o $O(n)$.
For example, if the values are all integers between $0$ and $k$, and $k$ is $O(n)$, then
counting sort has $O(n)$ worst case.

#buble

Very slow.

Very simple.

Worst: $n^2$ time (extermelly rare) $1$ space (in place)

Average: $n^2$.

Bubble.

- quick. Worst: $n^2$ time, $n$ space.

##bubble visualization

Start with:

    4 2 1 3

We compare two by two and exchange if needed:

    4 2 1 3
    ^ ^

4 is larger than 2, so exchange, giving:

    2 4 1 3
    ^ ^

Move forward:

    2 4 1 3
      ^ ^

4 is larger than 1, so exchange, giving:

    2 1 4 3
      ^ ^

Move forward:

    2 1 4 3
        ^ ^

4 is larger than 3, so exchange, giving:

    2 1 3 4
        ^ ^

We reached the end. Restart:

    2 1 3 4
    ^ ^

Then:

    1 2 3 4
    ^ ^

    1 2 3 4
      ^ ^

2 is smaller than 3, don't exchange.

    1 2 3 4
        ^ ^

We reached the end, restart:

    1 2 3 4
    ^ ^

    1 2 3 4
      ^ ^

    1 2 3 4
        ^ ^

We did nothing on this lastr run (since we returned to the first case, so we are done.

##correctness

TODO

##complexity

Input of length $n$, un upper limit for the worst possible case is $n^2$ because TODO.

But then for each $n$ we can generate a case which takes $n^2$ TODO

Therefore the w4 2 1 3orst case complexity must be $n^2$.

#insert

TODO

#merge

Worst: $n log(n)$ time $n$ space

Average: same.

Principle:

- merging two ordered lists os size `n` can be done in `O(n)` time `O(n)` memory.
- start with lists of size 1 (always ordered), of items side by side, then 2, then 4, etc. `log(n)` times.

##recursion step

The input comes into an array:

    input: 4 2 1 3

We first divide it in 1 sized chunks:

    4 | 2 | 1 | 3

The first step is to merge:

- two sorted lists: one containing only `2` and the other only `4`

        4 | 2 | 1 3
        ^   ^

    This gives:

        2 | 4 | 1 3
        ^   ^

- two sorted lists: one containing only `1` and the other only `3`

        2 4 | 1 | 3
              ^   ^

    Since they are already sorted, this gives:

        2 4 | 1 | 3
              ^   ^

Keep in mind that an array with a single element is always sorted.

Next we split things into 2 sized chunks:

    2 4 | 1 3

And we merge:

- two sorted lists `2 4` and `1 3` with $n=2$, which gives:

    1 2 3 4

If there were still more elements, we would take sizes 4, 8 and so on.

In [another section](merge-step) we will go into detail on how to do the merge of two sorted arrays efficiently.

##merge step

The merge step takes two *ordered* arrays of same size and merges them.

It is the key for the efficiency of this algorithm.

Let's merge the two following ordered arrays: `2 4` and `1 3`, both os size `n = 2`.

They are both on a single input array side by side:

    input: 2 4 | 1 3

The `|` is just to ease the visualization: it does not exist on the array.

That will be our input.

First make a copy of the input (`O(n)` memory):

    input:  2 4 | 1 3

    output: 2 4 | 1 3

Now point to their elements with indexes `k`, `i` and `j` as:

    input:  2 4 | 1 3
            ^     ^
            i     j

    output: 2 4 | 1 3
            ^
            k

$i$ points to the first of the first list, and $j$ to the fist of the second list.

$j$ points to the first of the output.

Compare values of $A[i]$ and $A[j]$.

Put the smallest one into $A[k]$:

    input:  2 4 | 1 3
            ^     ^
            i     j

    output: 1 4 | 1 3
            ^
            k


So the smallest was 1 (the owther was 2).

Move $k$, and the smallest of $i$ and $j$ forward:

    input:  2 4 | 1 3
            ^       ^
            i       j

    output: 1 4 | 1 3
              ^
              k

$k$ moved from 1 to 4, $j$ moved from 1 to 3 because it was the smallest and not $i$.

This is the end of the step.

In total we did:

- one comparison: $A[i]$ and $A[j]$
- one value copy: $A[i]$ or $A[j]$ to $A[k]$
- two increments: $i$ and $j$

Repeat step:

    input:  2 4 | 1 3
              ^     ^
              i     j

    output: 1 2 | 1 3
                  ^
                  k

Repeat step:


    input:  2 4 | 1 3
              ^       ^
              i       j

    output: 1 2 | 3 3
                    ^
                    k


Oops, $j$ fell out.

Last rule: if one of them falls out, it equals infinity.

We can now just copy the remaining elements of the other side into the output
with an efficient `memcpy` (in this case a single element `4`):

    input:  1 2 | 3 4
                      ^
                      k

    output: 2 4 | 1 3
                ^     ^
                i     j

Opps, k fell out. This means that the merge has ended.

We took each step exactly $2n$ times, where $n$ is the length of each half,
because:

- $k$ increases once per step
- the step ends when $k$ falls out of the output.

Therefore each merge step of two arrays of size $n$ takes $O(n)$ steps.

##complexity recursive analysis

Let $S(n)$ be the number of steps for size $n$.

We have:

$S(n) = 2*S(n/2) + C*n$

We can then verify that the solution has the form:

$A*n*log(n)$

where:

$A = TODO$

##complexity non recursive

Let $N$ be the number of elements.

We need to work with powers of 2, so let $2^m$ be the first power of two larger than $N$.

For a simple analysis, we can just fill all values between $A[N + 1] and $2^m$ with infinities.

We are doing:

- $N/2$ merges with $n = 1$
- $N/4$ merges with $n = 2$
- $N/8$ merges with $n = 4$
- ...
- $N/2^{m+1}$ merges with $n = 2^m$

Since each merge of size $n$ takes $C*n$ operations, we have:

- $N/2$ times $Cn$ operations
- $N/4$ times $2*C*n$ operations
- $N/8$ times $4*C*n$ operations
- ...
- $N/2^{m+1}$ times $n = 2^m$

which makes a total of:

- $C*N/2$ operations
- $C*N/2$ operations
- ...
- $C*N/2$ operations

m times.

But since $2^m >= N$, we have in the worst case: $D*N*log(N)$, with $D = C/2$,
where we recall that C was the cost of:

- one comparisond
- one value copy
- two increments

The memory worst case was all on the last merge, which required merging $N/2$ arrays,
so we actually made a copy of th entire original array.

#quick

Worst: $n^2$ time (extermelly rare) TODO space (in place)

Average: $log(n)$ time.

Even if it is quadratic worst time,
it is still quite used in practice because of $log(n)$ average and low memory usage,
and because it has good cache reutilization.

##visualization of one step

Quick but imprecise view: [gif gisualization](http://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif).

Input array:

    8 2 7 1 3 5 6 4

Pointer setup:

    8 2 7 1 3 5 6 4
    ^             ^
    i             r
    j

Visualization: we will separate things into two groups: smaller or equal to $A[r]$, and larger than $A[r]$:

          | 8 2 7 1 3 5 6 | 4
          | ^             | ^
          | i             | r
          | j             |
    small | large         | pivot


- $i$ is the first of the large side.
- $j$ is the current one we are checking now.
- $r$ is a recursive input of this step. It will move only between two steps. It starts at the rightmost element.

The rule is simple:

- $A[j] > A[r]$?

    - Yes:  $j++$
    - No: exchange $A[i]$ and $A[j]$, $j++$, $i++$

So: $8 > 4$? Yes. $j++$:

          | 8 2 7 1 3 5 6 | 4
          | ^ ^           | ^
          | i j           | r
          |               |
    small | large         | pivot

$2 > 4$? No. Exchange $A[i]$ and $A[j]$:

          | 2 8 7 1 3 5 6 | 4
          | ^ ^           | ^
          | i j           | r
          |               |
    small | large         | pivot

$i++$ and $j++$:

        2 | 8 7 1 3 5 6 | 4
          | ^ ^         | ^
          | i j         | r
          |             |
    small | large       | pivot

$7 > 4$? Yes. $j++$:

        2 | 8 7 1 3 5 6 | 4
          | ^   ^       | ^
          | i   j       | r
          |             |
    small | large       | pivot

$1 > 4$? No. Exchange $A[i]$ and $A[j]$:

      2 1 | 7 8 3 5 6 | 4
          | ^   ^     | ^
          | i   j     | r
          |           |
    small | large     | pivot

So: $3 > 4$? No. Exchange $A[i]$ and $A[j]$:

      2 1 | 3 8 7 5 6 | 4
          | ^   ^     | ^
          | i   j     | r
          |           |
    small | large     | pivot

$i++$ and $j++$:

    2 1 3 | 8 7 5 6 | 4
          | ^   ^   | ^
          | i   j   | r
          |         |
    small | large   | pivot

And so on:

    2 1 3 | 8 7 5 6 | 4
          | ^     ^ | ^
          | i     j | r
          |         |
    small | large   | pivot

And:

    2 1 3 | 8 7 5 6 | 4
          | ^       | ^
          | i       | r
          |         | j
    small | large   | pivot

Oops: $j$ reached $r$. We are done, just exchange $A[r]$ and $A[i]$:

    2 1 3 4 | 7 5 6 8
          ^ |
          i |
            |
      small | large

Note how we effectivelly split things into two sides: one larger than $4$, the other smaller.

Intuitively, why does it work? At each step, if we find a small number we throw it to the left,
and the small side increases, eating that number up.

What to do next? Recurse down twice:

- from $0$ to $i - 1$
- from $i + 1$ to $N$

    2 1 3 | 4 | 7 5 6 8
    ^   ^ |   | ^     ^
    i   r |   | i     r
    j     |   | j

Showing only the left side:

    2 1 3
    ^   ^
    i   r
    j

$2 < 3$:

    2 1 3
      ^ ^
      j r
      i

$1 < 3$:

    2 1 3
        ^
        r
        i
        j

Exchange $A[r]$ and $A[i]$ ($3$ by itself, doing nothing):

    2 1 3
        ^
        r
        i
        j

Recurse again:

    2 1 | 3
    ^ ^ | ^
    i r | i
    j   | j
        | r

Left side:

    2 1
    ^ ^
    i r
    j

$2 < 1$:

    1 2
      ^
      r
      i
      j

Done. Recurse again:

    1 | 2
    ^ | ^
    r | r
    i | i
    j | j

Left side:

    1
    ^
    r
    i
    j

Oops, $i = j = r$ and there is a single element. This is the base case. Do nothing.

#heap

TODO

#count