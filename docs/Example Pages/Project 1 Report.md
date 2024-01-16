</br>
</br>
</br>
</br>
</br>
</br>
</br>
</br>

**<center><font face="Times New Roman" size = 6 style = "margin-left:10%; margin-right:10%">Performance Measurement(Power)</font></center>**
</br>
</br>
**<center><font face="Times New Roman" size = 5>Author: WoChao He!</font></center>**
</br>
**<center><font face="Times New Roman" size = 4>Date: 2023-10-8</font></center>**

<div style="page-break-after: always;"></div>

</br>
</br>
</br>

**<center><font face="Times New Roman" size = 7>Menu</font></center>**
</br>

[TOC]

<div style="page-break-after: always;"></div>

</br>
</br>

#<div style="text-align:center;margin-left:12%;margin-right:12%"><font face="Times New Roman" >Chapter 1:Program</font></div>

</br>
</br>

##<div style="margin-left:12%;margin-right:10%">Introduction</div> 

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;This mission need to write a program to calculate a number’s power with integer exponent.</br>&emsp;&emsp;Even if we can use function pow() in &lt;math.h&gt;, it can be even slower than just using brute force, since it is designed for float exponent. Much worse, since the exponent is turned to float, accuracy problems can be triggered.</br>&emsp;&emsp;Therefore, we need to write a program which is suitable with integer exponent, and we hope that it can be as fast as possible.</br>
</div>

</br>

##<div style="margin-left:12%;margin-right:10%">Algorithm 1: Brute Force Algorithm</div>


<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">
&emsp;&emsp;We can easily come up with algorithm 1, which uses N - 1 multiplications. This is same as how we calculate it on our own. It's easy to calculate that the algorithm is O(N). Besides, since the function only contain some simple variables, the space complexity is O(1).
</div>

</br>

$$X^N =\overbrace{X \times X \times \cdots \times X}^{N-1 \text{ times of multiplications}}$$


<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;This is easy to realize in the C program as a function, the code is attached below.</br>
</div>

</br>


```c
                double Brute_Force(int n, double x)
                {
                  double ans = x;
                  int i;
                  for (i = 1; i < n; i++)
                  {
                     ans *= x;
                     // use n - 1 times mutiplications
                  }
                  return ans;
                }
```

<div style="page-break-after: always;"></div>

</br>
</br>

##<div style="margin-left:12%;margin-right:10%">Algorithm 2: Iterative</div>

</br>

###<div style="margin-left:12%;margin-right:10%">Introduction of Square</div>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Tips in the mission tells us that there problem can be solve more easily. You can use square in proper place. For example:</br>
</div>

$$
81 = 3 \times 3 \times 3 \times 3 \\
81 = (3 \times 3)^2
$$

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;We can use square to considerably simplify our algorithm. Then there's only one question to solve.</br>
</div>

</br>

###<div style="margin-left:12%;margin-right:10%">When should we square?</div>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Using number 81 seems to be too easy for the algorithm. We need to find more complex calculations to find out the answer. Then, a bigger number 59049 can be our choice.</br>
</div>

$$
59049 = 3 ^{10} \\
3^2 = 3 \times 3 = 9 \\
9^2 = 81 \\
81 ^ 2 = 6561 \\
6561 ^ 2 >> 59049 \text{  -- failed!} \\
81 \times 3 = 243 \\
243^2 = 59049 \text{ -- success!} \\
59049 = ((3^2)^2 \times 3)^2 
$$

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Observe when we implement multiplication. There are four squares, and we implement the multiplication in front of the second and fourth one.</br>&emsp;&emsp;Now we write the binary form of number 10. we get 1010. Ignoring the first bit, if we treat number 1 as the step that we should apply a multiplication, the binary number 010 is just what we want as the order. (Later I will give an explanation of ignoring first bit)
</div>
</br>

###<div style="margin-left:12%;margin-right:10%">Coincidence or Provable?</div>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Jaw-dropping as the conclusion could be, it can be proved though.
</div>

$$
3^{10} \\ = 3^{1\times 2^3 + 0 \times 2^2 + 1\times 2^1 + 0 \times 2^0}\\ = (3^{1\times 2^2 + 0 \times 2^1 + 1\times 2^0 })^2\\ = (3^{1\times 2^2 +0\times2^1}\times 3)^2 \\= ((3^{1\times2^1})^2\times 3)^2 \\= ((3^2)^2\times3)^2
$$

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;From above we can find that when we implement multiplication is actually linked to its binary bit. For other numbers, we can still use this to prove it right.
</div>

</br>
</br>
</br>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Thus, through this mathematical feature, we can develop an iterative algorithm whose time complexity is O(log(N)), which is much better than brute-force algorithm. The code is attached in the next page. Since we only used some simple variables, the space complexity is also O(1).
</div>

<div style="page-break-after: always;"></div>

</br>
</br>
</br>
</br>
</br>
</br>

```c
            #define Mod2(X) ((X) - (((X) >> 1) << 1))
            // use bit operation to increase speed.
            // x % 2 = x - x / 2 * 2 (x is an integer)

            double Fast_Exponent_iterative(int n, double x)
            {
              // iterative algorithm use binary form and bit operation
              int TotalBit = 0, CurrentBit;
              double ans = 1;
              // calculate TotalBit
              int buf = n;
              while ((buf >>= 1) > 0)
                  TotalBit++;
              CurrentBit = ++TotalBit;
              // traverse from the highest bit to the lowest bit
              while (CurrentBit)
              {  
                  //collecting current bit
                  // quite similar as x / 1000 % 10, etc.
                  if (Mod2(n >> (CurrentBit --))) // odd case
                     ans = ans * ans * x;
                  else // even case
                     ans = ans * ans;
              }
              return ans;
            }
```

<div style="page-break-after: always;"></div>

</br>
</br>

##<div style="margin-left:12%;margin-right:10%">Algorithm 3: Recursive</div>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;It seemed hard to observe the mathematical rule above just in a second. Thus we need a algorithm that is easier to come up. </br>
&emsp;&emsp;We used to think the answer from the initial case. But as our math intuition tells us, if we want to prove a theorem, we can think from both ways. That is, we can focus on the end of the equation -- the answer. </br>
&emsp;&emsp;Ask a few questions. Is the last step a square? If not, what is the other case? It is clearly that when the exponent is even, it can be the square of its square root. And for another case, when it is odd, we can first take a truth from it and execute square root.
</div>

$$
X^N = \begin{cases}(X^{\frac{N}{2}})^2, N \equiv 0 (\text{mod } 2) \\ (X^{\frac{N - 1}{2}})^2\times X, N \equiv 1 (\text{mod } 2)\end{cases}
$$

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;The quation transform f(N) into f(N/2), its easy to think that we can use recursive form to realize the algorithm. The recursion exit: N = 1, X^N = X. From the recursive equation we can find the time complexity O(log(N)). The code is attached on the next page. </br>&emsp;&emsp;By the way, since we called the function again, so it will have more space complexity. It is relative to the times the function call itself, which is O(log(N)).
</div>

<div style="page-break-after: always;"></div>

</br>
</br>
</br>
</br>
</br>
</br>

```c
            #define Mod2(X) ((X) - (((X) >> 1) << 1))
            // use bit operation to increase speed.
            // x % 2 = x - x / 2 * 2 (x is an integer)

            double Fast_Exponent_recursive(int n, double x)
            {
                // Exit circumstance
                // x^1 = x
                if (n == 1)
                    return x;
                // recursive process
                double y = Fast_Exponent_recursive(n >> 1, x);
                if (Mod2(n) == 1) // if n is odd
                {
                    return y * y * x;
                }
                else // if n is even
                {
                    return y * y;
                }
            }
```

<div style="page-break-after: always;"></div>

</br>
</br>

#<div style="text-align:center;margin-left:12%;margin-right:12%"><font face="Times New Roman" >Chapter 2: Testing</font></div>

</br>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Now we want to test the efficiency of three algorithms. This means we need to design several data, test them in three algorithms and compare their running time.</br>
</div>

</br>
</br>

##<div style="margin-left:12%;margin-right:10%">Test Data Design</div>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;To get a more convincible result, we need a wide range of N. Samely, X should be a constant that to avoid tiny effect. if N is big enough X^N can be very big. So we choose an X very close to 1 -- 1.0001. And N is ranged from 1000 to 100000.</br>
</div>

$$
N = 1000, 5000, 10000, 20000, 40000, 60000, 80000, 100000.
$$

</br>

##<div style="margin-left:12%;margin-right:10%">Test Program Design</div>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;The only point to be solved it to collect running time. This can be solved by including library &lt;time.h&gt;. Using function clock() to know the current time, record begin time and end time, we can calculate the duration by doing subtract. But to avoid the circumstance in which the duration is too small to test, we introduce a repeat time K, to run function repeatly for several times, and the duration will be TotalTime divided by K.
</div>

<div style="page-break-after:always"></div>

</br>
</br>

$$
Duration = \dfrac{TotalTime}{K} 
\\
~\\
TotalTime = \dfrac{EndTime - StartTime}{ClockPerSecond}
$$

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;To call the three functions in a program conveniently, I used a function pointer, and created a linked list to store all the three functions. So that I can call all the three functions in one 'for' statement. The main part of testing program is attached below. 
</div>

</br>
</br>
</br>
</br>
</br>


```c
            // time limit
            #define LIMITTIME 30.0

            typedef double (*Function)(int, double);
            double x, duration;
            int n;

            // use linked list to save function for further convenience
            typedef struct FunctionNode
            {
                Function Function;
                char *Name;
                struct FunctionNode *Next;
            } *FunctionNode;
```

```c
int main()
{
    // generate linked list
    FunctionNode func1 = malloc(sizeof(struct FunctionNode));
    func1->Function = Brute_Force;
    func1->Name = "Brute Force";
    FunctionNode func2 = malloc(sizeof(struct FunctionNode));
    func2->Function = Fast_Exponent_iterative;
    func2->Name = "Iterative";
    FunctionNode func3 = malloc(sizeof(struct FunctionNode));
    func3->Function = Fast_Exponent_recursive;
    func3->Name = "Recursive";
    func1->Next = func2;
    func2->Next = func3;
    func3->Next = NULL;
    // beginning of program
    int k, n;
    double TotalTime, ans, x, duration, tick;
    clock_t start;
    scanf("%lf%d", &x, &n);
    FunctionNode p;
    // here we use linked list traverse to execute every function
    for (p = func1; p != NULL; p = p->Next)
    {
        // For convenience, we set the Totaltime a constant
        // To make duration precise, and avoid too much execution time
        for (k = 0, TotalTime = 0, start = clock(); TotalTime <= LIMITTIME; k++)
        {
            ans = p->Function(n, x);
            tick = (double)(clock() - start);
            TotalTime = tick / CLK_TCK;
        }
        duration = TotalTime * 1000 / k;
        // output have three lines
        // answer to check if is right,
        // totaltime is the total processing time of functions
        // duration is the time of only one processing of the function
        printf("Algorithm:%s\nanswer:%.2lf\nTick:%d\nTotalTime:%.8lfs\nDuration:%.8lfms\n\n", 
        p->Name, ans, tick, TotalTime, duration);
    }
    return 0;
}
```

</br>
</br>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;If we want to get data easy to use, saving our time copying and pasting, we can use multifile programing. By creating CSV documents, we can easily get a copy of data which can be used by data processing software like Excel.</br>
&emsp;&emsp;The CSV document have format like below:
</div>

</br>

<div style = "width:auto;margin-left:30%; margin-right:39%">

| Class1 | Class2 | Class3 |
| :----: | :----: | :----: |
| Data1  | Data2  | Data3  |
| Data4  | Data5  | Data6  |

<center>original table<center>

</br>

```csv
Class1,Class2,Class3
Data1,Data2,Data3
Data4,Data5,Data6
```

<center>CSV File</center>

</div>

</br>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;And this is easy to realize with some simple calls of files function. The code is attached on the next page.
</div>

<div style="page-break-after: always;"></div>

</br>
</br>
</br>
</br>
</br>


```c
int main(int argc, char *argv[])
{
    long k, n, tick;
    double TotalTime, ans, x, duration, LimitTime = 20.0;
    clock_t start;
    start = clock();
    sscanf(argv[1], "%lf", &x);
    sscanf(argv[2], "%ld", &n);
    if (argc == 4)
        sscanf(argv[3], "%lf", &LimitTime);
    // For convenience, we set the Totaltime a LimitTime, the default is 20.0s.
    // To make duration precise, and avoid too much execution time
    for (k = 0, TotalTime = 0; TotalTime <= LimitTime; k++)
    {
        ans = Brute_Force(n, x);
        tick = (long)(clock() - start);
        TotalTime = (double)tick / CLK_TCK;
    }
    duration = TotalTime * 1000 / k;
    // output have three linesS
    // answer to check if is right,
    // totaltime is the total processing time of functions
    // duration is the time of only one processing of the function
    // use csv document to save data
    FILE *Try = fopen(".\\Brute_Force.csv", "r");
    FILE *CSV = fopen(".\\Brute_Force.csv", "a+");
    if (Try == NULL)
        fprintf(CSV, "N,Iteration,Ticks,TotalTime,Duration\n");
    fprintf(CSV, "%ld,%ld,%ld,%.4lf,%.8lf\n", n, k, tick, TotalTime, duration);
    return 0;
}
```

<div style="page-break-after: always;"></div>

</br>
</br>

##<div style="margin-left:12%;margin-right:10%">Result processing and comparing</div>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;For more precise and clearer datas, we set the total running times 40.0 seconds, which means a function should repeat in 40.0 seconds and get the final duration. The result comes out as below:
</div>

</br>
</br>

<div style="text-align:center;margin-left:5%;margin-right:5%">

| Algorithm | N              | 1000       | 5000       | 10000      | 20000      |
| --------- | -------------- | ---------- | ---------- | ---------- | ---------- |
| 1         | Iterations(K)  | 18162267   | 2506330    | 1254825    | 627762     |
| 1         | Ticks          | 40001      | 40001      | 40001      | 40001      |
| 1         | TotalTime(sec) | 40.001     | 40.001     | 40.001     | 40.001     |
| 1         | Duration(ms)   | 0.00220242 | 0.01595999 | 0.03187775 | 0.06372001 |
| 2         | Iterations(K)  | 1125625470 | 843671292  | 768789583  | 696530845  |
| 2         | Ticks          | 40001      | 40001      | 40001      | 40001      |
| 2         | TotalTime(sec) | 40.001     | 40.001     | 40.001     | 40.001     |
| 2         | Duration(ms)   | 0.00003554 | 0.00004741 | 0.00005203 | 0.00005743 |
| 3         | Iterations(K)  | 714671967  | 499169955  | 460694971  | 432258058  |
| 3         | Ticks          | 40001      | 40001      | 40001      | 40001      |
| 3         | TotalTime(sec) | 40.001     | 40.001     | 40.001     | 40.001     |
| 3         | Duration(ms)   | 0.00005597 | 0.00008014 | 0.00008683 | 0.00009254 |

</div>

<div style="page-break-after:always;"></div>

</br>
</br>
</br>
</br>
</br>
</br>
</br>

<div style="text-align:center;margin-left:5%;margin-right:5%">

| Algorithm | N              | 40000      | 60000      | 80000      | 100000     |
| --------- | -------------- | ---------- | ---------- | ---------- | ---------- |
| 1         | Iterations(K)  | 314265     | 210090     | 157949     | 128158     |
| 1         | Ticks          | 40001      | 40001      | 40001      | 40009      |
| 1         | TotalTime(sec) | 40.001     | 40.001     | 40.001     | 40.009     |
| 1         | Duration(ms)   | 0.1272843  | 0.19039935 | 0.25325263 | 0.31218496 |
| 2         | Iterations(K)  | 626728183  | 603236092  | 559750608  | 559056221  |
| 2         | Ticks          | 40001      | 40001      | 40001      | 40001      |
| 2         | TotalTime(sec) | 40.001     | 40.001     | 40.001     | 40.001     |
| 2         | Duration(ms)   | 0.00006383 | 0.00006631 | 0.00007146 | 0.00007155 |
| 3         | Iterations(K)  | 408381434  | 377832441  | 368109113  | 365562805  |
| 3         | Ticks          | 40001      | 40001      | 40001      | 40001      |
| 3         | TotalTime(sec) | 40.001     | 40.001     | 40.001     | 40.001     |
| 3         | Duration(ms)   | 0.00009795 | 0.00010587 | 0.00010867 | 0.00010942 |

</div>

<div style="page-break-after:always;"></div>

</br>
</br>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Tables might not completely show all the informations directly, so I just made a graph to bring them to light.
</div>

</br>
</br>

<img src=".\\asset\\Algorithm Comparison.png" alt="Algorithm Comparison">

</br>
</br>
</br>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Graph above have shown that Algorithm 2 and 3, which is O(log(N)), are significantly faster than algorithm 1. Furthermore, we can see that the result of algorithm 1 implies the running time seems to be linear with N, proving that the algorithm is O(N).</br>
&emsp;&emsp;However, we can't tell if algorithm 2 and 3 is linear since the durations of them are too small to observe. Thus, we need a clearer graph to show the relation.
</div>

<div style="page-break-after:always"></div>
</br>
</br>

<img src=".\\asset\\Algorithm Comparison(2&3).png" alt="Algorithm Comparison(2&3)">

</br>
</br>
</br>
<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;From the graph, we can see that the algorithm is nearly logarithmic, ignoring tiny errors. And we can see algorithm 3, the recursive algorithm, is a litter slower than iterative one. This can be possibly caused by calling stacks.
</div>

<div style="page-break-after: always;"></div>

</br>
</br>

#<div style="text-align:center;margin-left:12%;margin-right:12%"><font face="Times New Roman" >Chapter 3: Open Discussion</font></div>

</br>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;From the graph 2 we know that both algorithm 2 and 3 has some tiny errors from standard logarithmic relation.What may cause the error?</br>
&emsp;&emsp;In my opinion, it should be attribute to selection of data. 100000 is about 2^17, this can be very small in an logarithmic algorithm. So the difference inside execution will make huge effect to the result. For example, we took number 60000 and 80000.
</div>

$$
80000 = (0001 \space 0011 \space 1000 \space 1000 \space 0000)_2 \\
60000 = (0000 \space 1110 \space 1010 \space 0110 \space 0000)_2
$$

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:18px">&emsp;&emsp;Number 1 means two multiplications, and number 0 means one. There are 5 number 1s when N = 80000, but 7 1s when N = 60000, so sometimes N = 60000 can be slower than N = 80000.
</div>

</br>
</br>
</br>
</br>
</br>

<div style="margin-left:12%;margin-right:9%;font-family:'Times New Roman';font-size:30px">

**Declaration: I hereby declare that all the work done in this project titled Performance Measurement(Pow) is of my independent effort.**
</div>

<!--Thanks for watching all the markdown doc. Hope you learned a lot. I'm a beginer of markdown and HTML language up to now, hope we can be better at future!-->