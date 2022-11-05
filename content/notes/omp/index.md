---
title: OMP overview
date: 2022-11-05T17:35:00+03:00
draft: false
summary: OMP directives with examples
author: Evans Owamoyo
cover:
    image: images/omp_logo.png
    alt: "omp logo" 
    relative: true 
categories:
  - Notes
tags:
  - omp
  - c++
  - concurrency
comments: false
slug: omp
---
## compiler directives

* special line of code with meaning only to certain compilers  
fortran : !$OMP  
c++ /c : #pragma omp  

* this means that the directives are ignored when compiled normally.

* directives contain additional information through clauses
e.g.
```c
#pragma omp parallel [clauses]
```
* clauses are comma or space separated

<!--
## runtime library routines
## environment variables
-->

# Parallel Region 
* defines a seciton of a program
uses the fork/join model
* every thread executes the statement insdie the parallel region
* the master thread waits at the end of the region

- Shared and Private data
inside a parallel region, we have private or shared
* all thread see the same copy of shared variables
* all threads can read or write shared variables
* each thread has its own copy of private variables, (invisible to others)
* a private var can only be r/w by it's thread

## Synchronisation
- Barrier
- Critical region
- Atomic update

### Barrier
* all threads must arrive at a barrier before any thread can proceed past e.g. delimiting phases of computation

### Critical 
* a **section of code** which only one thread at a time can enter
e.g. modification of shared variables

### atomic update
* an update to **a variable** which can be performed at a time
e.g. modification of shared  variables

* Notable envs, set this to as env to change number of threads
OMP_NUM_THREADS

* how to get the number of threads
```c
#include <omp.h>
int omp_get_num_threads();
```

### shared and private variables
* types: clauses
shared(list)
private(list)
default(shared|none)

* on entry private vars are uninited
* vars declared inside the scope of || are automatically private
* after || region ends, original var is unaffected by any changes to private copies
* in c++ objects are created using the default constructor
* not specifying a DEFAULT clasuse is the same as specifying DEFAULT(SHARED).
* always use default(none) because the compiler will complain and this will help prevent race conditions.

## Examples:
```c++
#pragma omp parallel default(none) private(i, myid) shared(a,n)
{
	myid = omp_get_thread_num() + 1;
	for (int i = 0; i < n; i++) {
		a[i,myid]= 1.0;
	}
}
```

### multi line directives
we use a backslash in c++ and the backslash must be the last character on that line.
```
#pragma omp parallel default(none) \
private (i,myid) shared(a,n)
```

### initialising private variables
type: clause
we can use `firstprivate` instead of `private` to init
firstprivate(list)
* it has uncommon use cases
* in c++ the default copy constructor is called to create and init the new object
example:
```c++
int main() {
int i = 10;
#pragma omp parallel firstprivate(i)
    {
        i++;
        printf("%d\n", i); // prints 11 11 11 11 11 ... (for each thread)
    }
    printf("The final value of i is %d \n", i); // prints 10
    return 0;
}
```

## Reductions
* a reduction produces a single value from associative ops such as add, x, max, min, and, or
* all thread reduce into a private copy, then reduce all these to give final result
* type: clause
`reduction(op:list)`

example:
```c++
int main() {
int i = 10;
#pragma omp parallel reduction(+:i) // 0 is inited
    {
        i++; 
        printf("%d\n", i); // prints 1 
    }
    printf("The final value of i is %d \n", i); // 18
    return 0;
}
```

# Work Sharing directives 
directives which appear inside a parallel region and indicate how work should be shared out between threads
- Parallel for loops
- single directive
- master directive

## Parallel for loops
* divides iter between threads
* there is a sync point at the end of the loop. all threads must finish their iter before any thread can proceed.
* there are restrictions in c++
it must be of form `for(var i=0;<>;<>)` and the body must not mutate the counter.

example
```c++
#pragma omp parallel
{
	#pragma omp for 
	for(int i=0;i<n;i++) {

	}
	
}
```
however there is a shorthand which combines the parallel and the for part
`#pragma omp parallel for`

### Clauses
* for directives can take `private`, `firstprivate` and `reduction` clauses.
* parallel loop index is private by default while other loop indices are not.
* parallel for can take all clauses available for parallel directive.
* parallel for != parallel != for

#### Schedule Clause
* gives a variety of options for specifying which loop iter are executed by which thread
`schedule(kind[,chunksize])`
where `kind` is one of (static, dynamic, guided, auto, runtime)
and `chunksize` is an integer expression with +ve value.

##### static
- with no chunksize, the iteration space is divided and one chunk is assigned to each thread (block schedule) e.g. 1,2,3,4
- with chunksize, each iteration space is divided into chunks each of `chunksize` iterations, and these chunks are assigned cyclically to each thread in order (block cyclic schedule). schedule(static,2)
1,2,1,2,3,3

##### dynamic
it divides iter space into chunks of size chunksize and assigns them to threads on a first-come-first-served basis.
* ie. as a thread finishes a chunk, it is assigned the next chunk in the list.
* it defaults to 1 chunksize when not provided

##### guided
* similar to dynamic, but the chunks start off large and gets smaller exponentially. 
* the size of the next chunk is proportional to the number of remaining iter / the no of threads.
* chunksize is min size of chunks
* when no chunksize is specified, it defaults to 1

##### auto
* lets the runtime decide
* if the parallel loop is executed many times, the runtime can evolve a good schedule which has good balance and low overheads.

#### Choosing a schedule
* static best for load balanced loops
* static, n good for loops with mild or smooth load imbalance, but can induce overheads
* dynamic, useful if iterations have widely varying loads but runs data locality
* guided often less expensive than DYNAMIC, but beware of loops where the first iterations are the most expensive.
* auto if executed many times over.

#### Single Directive
* indicates that a block of code is to be executed by a single thread only
* the first thread to reach the SINGLE directive will exec the block
* there is a sync point at the end of the block: all the other threads wait until block has been executed

```c
#pragma omp single [clauses]
```

e.g.
```c++
#pragma omp parallel
{
	setup(x); // all the threads do this
	#pragma omp single  // all of them block while the first thread to enter executes this block
	{
		input(y); // only one thread does this, others wait
	}
	work(x, y); // every thread resumes
}
```
* single directive can take private and firstprivate clauses.
* directive must contain a structured block; cannot branch in and out of it. e.g. `break;`

#### Master directive
* indicates that a block of code should be exec by the master thread (thread 0) only.
* like single but instead of first thread, only master thread.
* N.B. there is no sync point at the end of the block i.e. other threads skip the block and continue execing. 
`#pragma omp master`

### Barrier Directive
* no threads can proceed past a barrier until all the other threads have arrived.
* code can deadlock if a thread is stuck
* note that there is implicit barrier at the end of for, section and single directives.

#### Example
```c++

int main() {
float arr[8], brr[8];
int myid, neighb;
#pragma omp parallel private(myid, neighb) default(shared)
    {
        myid = omp_get_thread_num();
        neighb = myid - 1;
        if (myid == 0) {
            neighb = omp_get_num_threads() - 1;
        }
        arr[myid] = float(myid) * 3.5;
#pragma omp barrier // all threads wait for owner to write to a before checking neighbours
        {
            brr[myid] = arr[neighb] + 4;
            printf("i am %d and the value of brr[myid] is %f\n", myid, brr[myid]);
        }
    }
    return 0;
}
```

### Critical section
* a block of code which can be executed by only one thread at a time.
```c
#pragma omp critical 
{

}
```

### Atomic directive
* used to protect a single update to a shared variable
* applies to a single statement
* statement must have one of the forms
x=x op expr, x = expr op x, x = intr(x, expr) or x = intr(expr, x)
> op is one of +, *, -, /, ..and.., .or., .eqv., or .negv.
> intr is one of max, min, iand, ior, ieor
example
```c
// since vertices are shared by edges, to prevent race conditions when updating vertices, action has to be atomic.

// atomic > critical here because the array is not locked, 
// but instead, only the memory at ith edge is locked
#pragma omp parallel for
	for(j = 0; j < nedges; j++) {
		#pragma omp atomic
			degree[edge[j].vertex1]++;
		#pragma omp atomic
			degree[edge[j].vertex2]++;
	}
```

### Lock Routines
```c++
#include <omp.h>
void omp_init_lock(omp_lock_t *lock);
void omp_set_lock(omp_lock_t *lock);
int omp_test_lock(omp_lock_t *lock);
void omp_unset_lock(omp_lock_t *lock);
void omp_destroy_lock(omp_lock_t *lock);
```

## Nested parallelism
* if a parallel directive is encountered within another parallel it will create new threads
OMP_NESTED=FALSE/TRUE