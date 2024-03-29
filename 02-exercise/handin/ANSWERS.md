# Exercises week 2

## Exercise 2.1

1. The method countFactors counts the number of prime factors a give input has. For that it needs to check first if the number provided is smaller than 2, because 1 is called a unit. It has no prime factors. After the check the number of factors (factorCount) is initialised with 1 and the starting prime factor of 2 in k. Now the program checks the given number until the given input p is smaller than the square root of the prime factor (while (p >= k * k)). If the number p is divisable by the prime factor, the factorCount should be incremented and the result of the division should be written in p. If the number is not divisable by the prime factor (k) it is incremented by one, to check next. This is done until the square of the checking prime factor is larger than the input number. The run time of the algorithm is depended on the while-loop and for that the upper bound would be: O(sqrt(p))

2. 10.3 seconds

3. 10.3 seconds

4. See code

5. Run time is reduced by 0.7 seconds and the result is the same.

6. No, there is no way to turn the 3-operation of read-add-write into an atomic operation.

7. Yes, because we are using it within a lambda. All variables that are declared outside of a lambda and use within need to be final.

## Exercise 2.2

1. The code would run, but there would be no optimization, because the cache wouldn't work. The visibility of the cache between the threads is not propagated to them.

2. Because it is only assigned once and never reassigned, the variables don't necessarily need to be final, but to ensure correctness and data integrity they should be declared final.

## Exercise 2.3

1. We need a final on the counts variable to ensure immutability on it. The increment and get methods need to be synchronized on the object's lock so no data is lost during the operations. The getSpan on the other hand does not have to be locked.

2. See code

3. It returns the same result and we can remove the locks from the methods, because the AtomicInteger makes sure the operations are atomic.

4. Same result

5. See code

6. See code

## Exercise 2.4

| Memoizer  | Difference                | Runtime | Results |
| :-------- | ------------------------- | ------: | ------: |
| Memoizer1 | synchronized              | 20 s    | 115,000 |
| Memoizer2 | ConcurrentHashMap         | 17 s    | 155,000 |
| Memoizer3 | FutureTask                | 14 s    | 117,077 |
| Memoizer4 | putIfAbsent               | 13 s    | 115,000 |
| Memoizer5 | AtomicReference           | 13 s    | 115,000 |
| Memoizer0 | Plain compute-if-absent   | 24 s    | 115,000 |

1. See code

2. Memoizer1: Yes it is.

3. Memoizer2: It does not need to wait for the other threads to finish, because its uses a ConcurrentHashMap.

4. Memoizer3: The number of computations done at the same time on the same number is reduced alot.

5. Memoizer4: Uses an extra if-statement before it computes the number to check if it exists or not `f = cache.putIfAbsent(arg, ft)`. This makes it highly unlikely that two threads compute the same number.

6. Memoizer5: The first thread to use the Memoizer will set the AtomicReference once it is ready to compute the result.
              An interleaving thread will see that the contents of its AtomicReference is null, 
              which indicates that it should not attempt to compute the result.
              In this manner, it becomes impossible for 2 threads to compute the same result.
              This is the result we see.

7. Memoizer0: The last memoizer is very slow compared to the similar one (Memoizer5). 
    This could be because the computeIfAbsent function potentially blocks other threads attempting to get the value.
    This is mostly a problem if large computations are done in the computeIfAbsent function, which is the case
    in this Memoizer, and not Memoizer5.
