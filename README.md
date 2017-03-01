# fibonacci
In mathematics, the Fibonacci numbers are the numbers in the following integer sequence, called the Fibonacci sequence, and characterized by the fact that every number after the first two is the sum of the two preceding ones:

1,1,2,3,5,8,13,21,34,55,89,144,...

Fibonacci numbers appear unexpectedly often in mathematics, so much so that there is an entire journal dedicated to their study. Applications of Fibonacci numbers include computer algorithms such as the Fibonacci search technique and the Fibonacci heap data structure, and graphs called Fibonacci cubes used for interconnecting parallel and distributed systems. They also appear in biological settings, such as branching in trees, phyllotaxis (the arrangement of leaves on a stem), the fruit sprouts of a pineapple, the flowering of an artichoke, an uncurling fern and the arrangement of a pine cone's bracts.

So, how would we calculate the fibonacci number fib(n).  Using the preceding definition, fib(0) = 1, fib(1) = 1, fib(n) = fib(n-1) + fib(n-2)
It is easy to see that this is a recursive definition.  So, lets write a function to compute the nth Fibonacci number.
```c++
#include <iostream>
using namespace std;

int fibonacci(int n) {
  if (n < 2)
    return 1;
  else
    return fibonacci(n - 1) + fibonacci(n - 2);
}

int main()
{
    int n;
    
    cout << "Enter an integer "<<endl;
    cin >> n;
    cout << "Fib returned "<<fibonacci(n)<<endl;
}
```
This seems like a pretty clean implementation, lets see how long it takes to compute.
```c++
#include <iostream>
#include <ctime>
#include <stdlib.h>
#include <math.h>
using namespace std;


int fibonacci(int n) {
  if (n < 2)
    return 1;
  else
    return fibonacci(n - 1) + fibonacci(n - 2);
}

int main()
{
    clock_t oldtime, newtime;
    double seconds;
    int n;
    
    
    cout << "Enter an integer "<<endl;
    cin >> n;
    oldtime = clock();
    rval = fibonacci(n);
    newtime = clock();
    seconds = (double)(newtime-oldtime)/CLOCKS_PER_SEC;
    cout << "Fib "<<n<<" = "<<rval<<"  took "<<seconds<<endl;
}
```
It seems to work pretty well up to about n=40, but what about n=60?  Lets figure out the Big O complexity of this algorithm.

If you look at the total computations performed it is obvious that every internal fib(x) requires two other fib computations to calculate.  So the total number of function calls is the number of nodes in a binary tree or O(2^n) computations.  This seems to be unreasonable for this kind of a computation.
```
n              *
n-1            **
n-2           ****  
...
2           ***********
1       ******************
0    ***************************
```
The problem is that the recursive version recomputes the same fib values over and over again.  Because of the redundant function calls, the time required to calculate fibonacci(n) increases exponentially with n. For example, if n is 100, there are approximately 2^100 activation frames. This number is approximately 10^30. If you could process one million activation frames per second, it would still take 10^24 seconds, which is approximately 10^16 years.

Lets look at an iterative approach.

```c++
#include <iostream>
#include <ctime>
#include <stdlib.h>
#include <math.h>
using namespace std;


int fibonacci(int n) {
  if (n < 2)
    return 1;
  else
    return fibonacci(n - 1) + fibonacci(n - 2);
}

int fib_iter(int n) {
  int result = 0;
  int prev = 1;
  int pprev = 1;
  
  if (n < 2)
    return 1;
    
  for (int i = 1; i < n; i++) {
    result = prev + pprev;
    pprev = prev;
    prev = result;
  }
  return result;
}
int main()
{
    clock_t oldtime, newtime;
    double seconds;
    int n;
    
    
    cout << "Enter an integer "<<endl;
    cin >> n;
    oldtime = clock();
    int rval = fib_iter(n);
    newtime = clock();
    seconds = (double)(newtime-oldtime)/CLOCKS_PER_SEC;
    cout << "Fib iter "<<n<<" = "<<rval<<"  took "<<seconds<<endl;
    oldtime = clock();
    rval = fibonacci(n);
    newtime = clock();
    seconds = (double)(newtime-oldtime)/CLOCKS_PER_SEC;
    cout << "Fib "<<n<<" = "<<rval<<"  took "<<seconds<<endl;
}
```
Notice that the iterative approach continues to have O(n) time even above 40, while the recursive version takes an exponential amount of time O(2^n).

Now lets see if there is a recursive version that would eliminate all of the redundant computations.
```c++
int fib(int current, int previous, int n) {
  if (n <= 1)
    return current;
  else
    return fib(current + previous, current, n-1);
}
```
This recursive function passes the two previous values, so it doesnt have to recompute them.  So this algorithm will have O(n) run time.  We will need a wrapper function to get things started.
```c++
int recursive_start(int n) {
  return fib(1, 1, n);
}
```
Test to see how long this version takes.

