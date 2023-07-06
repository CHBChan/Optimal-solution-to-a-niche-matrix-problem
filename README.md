# Finding the last accessed cell of the matrix when traversing in a spiral pattern while skipping alternate cells

Recently I sent a job application for a Software Engineer position and received an email to take their online assessment shortly afterward.
I don't think I can tell you which company I applied to, but I was given 90 minutes to solve three technical problems. The first two problems were relatively easy so I finished them in around 20 minutes. But I thought the third problem I was tasked to solve was rather interesting.

## This is the general premise of the problem:
Given a matrix with **N** rows and **M** columns, find the last element you traverse given that you start at the cell at Row(0), Col(0), and traverse counter-clockwise while skipping alternate cells.

[Click here if all you want is the Solution](#solution)

## Here are two examples (Not taken from the assessments):
Ex 1.

![image](https://github.com/CHBChan/Finding-the-last-cell-of-the-matrix/assets/81986429/0812f395-d789-4d9c-87f3-9c925797da98)
1. You begin at the 0th cell of value 31 (labeled by the black integer)
2. You jump to the 1st cell of value 76
3. You jump to the 2nd cell of value 10
4. You jump to the 3rd cell of value 58
5. You finally jump to the 4th cell of value 44

Solution: Since there are no more cells left to traverse, your last traversed element is then 44.

Ex 2.

![image](https://github.com/CHBChan/Finding-the-last-cell-of-the-matrix/assets/81986429/59c53641-f953-4af8-bb3f-1380f80e0305)
1. You being at the 0th cell of value 64
2. You jump to the 1st cell of value 47
3. You jump to the 2nd cell of value 49
4. You jump to the 3rd cell of value 21
5. You jump to the 4th cell of value 52
6. You jump to the 5th cell of value 93
7. You jump to the 6th cell of value 39
8. You jump to the 7th cell of value 72
9. You jump to the 8th cell of value 93
10. You jump to the 9th cell of value 16
11. You jump to the 10th cell of value 79
12. You finally jump to the 11th cell of value 94

Solution: Since there are no more cells left to traverse, your last traversed element is then 94.

Looking at the problem, it would be tempting to just brute force the problem and obtain the solution. However, keep in mind that there were no constraints given to me on **N** or **M**. The only assumption I could make is that both must be at least 1 to qualify the input to be a matrix. Based on that, combined with the fact that the resulting code would be unnecessarily convoluted, I decided to instead find a generalized solution for any **N** ≥ 1 and **M** ≥ 1, a solution that has a lower time complexity than O(**N * M**). 

<sub>Spoiler: I was unable to complete the problem in the 70 minutes allocated.</sub>

After the assessment ended, I was left frustrated because I was certain that there was a pattern in the problem that I missed. But, I thought the problem itself was really interesting, so I decided to turn to my friend, who happened to major in Mathematics for advice. It so happened that this problem also interested him, and he throw out several suggestions that could potentially help me reach a generalized solution. Two of which were:
1. Make a table to see if there were any obvious patterns
2. Reduce the matrix to smaller ones

I was creating a table when I realized how to reduce the matrix into smaller ones that would take much less time to solve. 
**This method only works if you traverse the matrix in a spiral pattern normally or traverse alternate cells. It would not work if you were to skip multiple cells for each jump.**

The main concept you must grasp is the fact that due to the way the problem was set up, you will eventually traverse to cells that are directly diagonal to the starting cell at Row(0), Col(0). If we refer back to example two:

![image](https://github.com/CHBChan/Finding-the-last-cell-of-the-matrix/assets/81986429/59c53641-f953-4af8-bb3f-1380f80e0305)

We can see that we must eventually reach the 3rd, 8th, and 9th cells that are directly diagonal to the 0th cell. It meant that we could essentially discard the outer cells, that is cells 0 to 7, and solve it as though we're working with a 2x4 matrix instead of a 3x5. We can't discard anything else in this example as we must preserve the overall shape of the matrix to ensure that the new starting cell (8th cell) must be at Row(0), Col(0). You can see from here that we lowered the number of cells we needed to visit from 12 to only 4. If we expand the theoretical input to a matrix of size 1024x631, the effects are much more apparent, as this reduction can be done recursively until you are only working with a matrix of size 394x1. But there is more. Due to the fact that we are working with reduction, we must establish the patterns and base cases (courtesy to the table I made).

## Recursive reduction patterns:

if **N** < **M**, and **N** is odd, the matrix can be reduced to size **1 x (M - N + 1)**

if **N** < **M**, and **N** is even, the matrix can be reduced to size **2 x (M - N + 2)**

if **N** > **M**, and **M** is odd, the matrix can be reduced to size **(N - M + 1) x 1**

if **N** > **M**, and **M** is even, the matrix can be reduced to size **(N - M + 2) x 2**

# Solution

Keep in mind that this specific solution only works for the exact scenario in that:
1. You begin at [0][0]
2. You traverse counter-clockwise
3. You skip alternate cells

Condition 1 cannot be changed

Condition 2 can be changed to clockwise and the general process to derive the solution will still apply

Condition 3 can only be changed to not skipping any cells to retain the same process, aka this process fails when trying to skip n cells at each step (n > 1)

## Base cases:

For a **1 x M** matrix, the last element is at index [0][2*Ceil(**M**/2) - 2]

For a **N x 1** matrix, the last element is at index [2*Ceil(**N**/2) - 2][0]

For a **N x 2** matrix, where **N** > 1, the last element is at index [1][1]

For a **2 x M** matrix, where **M** > 1, the last element is at index [0][2]

if **N** = **M**, the last element is at index [Floor(**N**/2)][Floor(**M**/2)]

## The solutions for N > 2 and M > 2 (0 Indexed)

| Smaller value | Smaller value parity | Last accessed cell |
| ------------- | -------------------- | ------------------ |
| N = M         | N/A                  | [Floor(N/2)]  [Floor(M/2)] |
| N             | Odd                  | [Floor(N/2)]  [Floor(N/2) + 2*Ceil((M-N+1)/2) - 2] |
| N             | Even                 | [Floor((N-1)/2)]  [Floor((N-1)/2) + 2] |
| M             | Odd                  | [Floor(M/2) + 2*Ceil((N-M+1)/2) - 2]  [Floor(M/2)] |
| M             | Even                 | [Floor(M/2]  [Floor(M/2)] |

By using one of these equations instead, you can drastically reduce the time complexity of the problem from O(**N * M**) to O(**1**) without having to use any data structures. I really enjoyed solving this problem because it is a rare case of a problem that can be optimized relatively easily using only mathematics. 

On a side note, this problem is so niche I don't see any practical applications for it outside of maybe being used in grid-based games.
