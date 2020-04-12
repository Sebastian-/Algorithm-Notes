# 1.4 Analysis of Algorithms

## Methods of Analysis

As algorithms are developed, questions regarding their running time and memory use must be addressed. The scientific method can be used to answer such questions.

- Observe/measure the performance of a program
- Hypothesize a model for it's performance
- Predict performance using the hypothesized model
- Verify the prediction through further observation
- Validate by repeating the process until hypothesis and observations agree

This process will be applied to a number of solutions of the 'Three Sum' problem. The problem asks how many unordered triples in a file of n distinct integers sum to 0.

```java
public class ThreeSum {

    public static int count(int[] a) {
        int n = a.length;
        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (a[i] + a[j] + a[k] == 0) {
                        count++;
                    }
                }
            }
        }
        return count;
    }

}
```

Running this program on a number of input sizes leads to the following observations

| Input   | Running Time |
| ------- | ------------ |
| 1K Ints | 0.3 s        |
| 2K Ints | 2.0 s        |
| 4K Ints | 15.7 s       |
| 8K Ints | 130.0 s      |

As the input doubles for each trial, the running time appears to take eight times longer. This suggests that T(n) = n<sup>3</sup>, since T(2n) = (2n)<sup>3</sup> = 8n. A similar hypothesis could be made by plotting and fitting the data.

Mathematical modelling is an alternative to running time trials when investigating the performance of an algorithm.

- Develop an input model, including a way of measuring the problem's 'size'
- Identify the 'inner loop' of the algorithm. The running time will be dominated by the operations of the algorithm which run most often
- Define a cost model - The operation(s) that scale inside the inner loop
- Determine the frequency of the cost model operations

For ThreeSum:

- The input model is the integer array of length n
- The inner loop is the most deeply nested for-loop
- The cost model is the number of array access being made in the inner loop's if-statement
- Since the inner loop runs ~n<sup>3</sup>/6 times, the brute force ThreeSum algorithm takes ~n<sup>3</sup>/2 array lookups

## Order-of-growth Classification

| Class        | Order of Growth | Algorithmic Strategy | Example           |
| ------------ | --------------- | -------------------- | ----------------- |
| Constant     | 1               | Statement            | Add two numbers   |
| Logarithmic  | log(n)          | Divide in Half       | Binary Search     |
| Linear       | n               | Loop                 | Find the Maximum  |
| Linearithmic | nlog(n)         | Divide and Conquer   | Mergesort         |
| Quadratic    | n<sup>2</sup>   | Double Nested Loop   | Check All Pairs   |
| Cubic        | n<sup>3</sup>   | Triple Nested Loop   | Check All Triples |
| Exponential  | 2<sup>n</sup>   | Exhaustive Search    | Check All Subsets |


