# Growth Curve and Mixed Models
Michael Clark  
`r Sys.Date()`  



# Intro

I wanted to compare latent growth curve (LGC) and mixed models for settings with low(ish) numbers of clusters.  As an example, having 50 individuals with measured at 4 time points each wouldn't cause many to think twice in the mixed model setting, while running a structural equation model with 50 individuals has never been recommended. Growth curve models are notably constrained SEM, as many parameters are fixed, which helps matters, but I wanted to investigate this further. 

# Setup

I put together some quick code for simulation (it hasn't really been inspected too thoroughly but seemed okay in initial tests). The parameters are as follows:

- Number of clusters: 10, 25, 50 
- Time points within cluster: 5, 10, 25
- Correlation of random effects: -.5 to .5 

Thus sample sizes range from the ridiculously small (10\*5 = 50 total observations) to fairly healthy (50\*25 = 1250 total). Random effects were drawn from a multivariate normal and had that exact correlation (i.e. there was no distribution for the correlation; using the <span class="pack">MASS</span> package with `empirical = TRUE`).  Fixed effects are 3 and .75 for the intercept and effect of 'time'. Variances of the random effects and residuals were `1.0`.  Note that the mixed model estimated residual variance separately for each time point as would be the default in the LGC model. Number of simulated data sets for each of the 45 settings is 500.

Growth curve models were estimated with the R package <span class="pack">lavaan</span>, the mixed models were estimated with <span class="pack">nlme</span> with `varIdent` weights argument.

# Results
I'm showing and summarizing the results here, but I've not checked them too closely yet.


## Parameter estimates
Bias here refers to the difference in the mean estimated parameter value vs. the true.

### Fixed effects


Maybe slight issues with the smallest samples sizes for the intercept, but nothing worrisome.


```
   nClusters nWithinCluster corRE biasLGC_Int biasMM_Int biasLGC_Time biasMM_Time
1         10              5 -0.50        0.02       0.02        -0.01       -0.01
2         10              5 -0.25       -0.02      -0.02         0.00        0.00
3         10              5  0.00        0.01       0.01         0.00        0.00
4         10              5  0.25        0.00       0.00         0.00        0.00
5         10              5  0.50        0.02       0.02        -0.01        0.00
6         10             10 -0.50       -0.01      -0.01         0.00        0.00
7         10             10 -0.25       -0.01      -0.01         0.00        0.00
8         10             10  0.00        0.02       0.02         0.00        0.00
9         10             10  0.25       -0.01      -0.01         0.00        0.00
10        10             10  0.50        0.00       0.00         0.00        0.00
11        10             25 -0.50        0.01       0.01         0.00        0.00
12        10             25 -0.25        0.01       0.01         0.00        0.00
13        10             25  0.00       -0.01      -0.01         0.00        0.00
14        10             25  0.25        0.00       0.00         0.00        0.00
15        10             25  0.50        0.00       0.00         0.00        0.00
16        25              5 -0.50       -0.01      -0.01         0.00        0.00
17        25              5 -0.25        0.00       0.00         0.00        0.00
18        25              5  0.00        0.00       0.00         0.00        0.00
19        25              5  0.25        0.00       0.00         0.00        0.00
20        25              5  0.50        0.00       0.00         0.00        0.00
21        25             10 -0.50        0.00       0.00         0.00        0.00
22        25             10 -0.25        0.00       0.00         0.00        0.00
23        25             10  0.00        0.00       0.00         0.00        0.00
24        25             10  0.25       -0.01      -0.01         0.00        0.00
25        25             10  0.50        0.00       0.00         0.00        0.00
26        25             25 -0.50        0.00       0.00         0.00        0.00
27        25             25 -0.25        0.00       0.00         0.00        0.00
28        25             25  0.00       -0.01      -0.01         0.00        0.00
29        25             25  0.25        0.00       0.00         0.00        0.00
30        25             25  0.50       -0.01      -0.01         0.00        0.00
31        50              5 -0.50        0.00       0.00         0.00        0.00
32        50              5 -0.25        0.00       0.00         0.00        0.00
33        50              5  0.00       -0.01      -0.01         0.00        0.00
34        50              5  0.25        0.00       0.00         0.00        0.00
35        50              5  0.50        0.00       0.00         0.00        0.00
36        50             10 -0.50        0.00       0.00         0.00        0.00
37        50             10 -0.25        0.00       0.00         0.00        0.00
38        50             10  0.00        0.00       0.00         0.00        0.00
39        50             10  0.25        0.00       0.00         0.00        0.00
40        50             10  0.50        0.00       0.00         0.00        0.00
41        50             25 -0.50        0.00       0.00         0.00        0.00
42        50             25 -0.25        0.00       0.00         0.00        0.00
43        50             25  0.00        0.00       0.00         0.00        0.00
44        50             25  0.25        0.00       0.00         0.00        0.00
45        50             25  0.50        0.00       0.00         0.00        0.00
```


### Random effect variance
Both underestimate random effects variance at these settings, and more so with smaller overall sample sizes, but neither more than the other.


```
   nClusters nWithinCluster corRE biasLGC_Int biasMM_Int biasLGC_Time biasMM_Time
1         10              5 -0.50       -0.12      -0.12        -0.09       -0.12
2         10              5 -0.25       -0.09      -0.09        -0.11       -0.13
3         10              5  0.00       -0.09      -0.09        -0.08       -0.10
4         10              5  0.25       -0.09      -0.09        -0.12       -0.13
5         10              5  0.50       -0.08      -0.08        -0.09       -0.11
6         10             10 -0.50       -0.09      -0.09        -0.10       -0.10
7         10             10 -0.25       -0.06      -0.06        -0.10       -0.10
8         10             10  0.00       -0.10      -0.10        -0.10       -0.10
9         10             10  0.25       -0.13      -0.13        -0.09       -0.09
10        10             10  0.50       -0.12      -0.12        -0.10       -0.10
11        10             25 -0.50       -0.09      -0.09        -0.10       -0.10
12        10             25 -0.25       -0.11      -0.11        -0.10       -0.10
13        10             25  0.00       -0.12      -0.12        -0.10       -0.10
14        10             25  0.25       -0.09      -0.09        -0.10       -0.10
15        10             25  0.50       -0.10      -0.10        -0.10       -0.10
16        25              5 -0.50       -0.07      -0.07        -0.04       -0.04
17        25              5 -0.25       -0.05      -0.05        -0.04       -0.04
18        25              5  0.00       -0.05      -0.05        -0.04       -0.05
19        25              5  0.25       -0.04      -0.04        -0.05       -0.05
20        25              5  0.50       -0.05      -0.05        -0.04       -0.05
21        25             10 -0.50       -0.03      -0.03        -0.04       -0.04
22        25             10 -0.25       -0.05      -0.05        -0.04       -0.04
23        25             10  0.00       -0.06      -0.06        -0.04       -0.04
24        25             10  0.25       -0.03      -0.03        -0.04       -0.04
25        25             10  0.50       -0.06      -0.06        -0.04       -0.04
26        25             25 -0.50       -0.03      -0.03        -0.04       -0.04
27        25             25 -0.25       -0.05      -0.05        -0.04       -0.04
28        25             25  0.00       -0.04      -0.04        -0.04       -0.04
29        25             25  0.25       -0.05      -0.05        -0.04       -0.04
30        25             25  0.50       -0.04      -0.04        -0.04       -0.04
31        50              5 -0.50       -0.04      -0.04        -0.02       -0.02
32        50              5 -0.25       -0.03      -0.03        -0.02       -0.02
33        50              5  0.00       -0.03      -0.03        -0.02       -0.02
34        50              5  0.25       -0.02      -0.02        -0.02       -0.02
35        50              5  0.50       -0.03      -0.03        -0.02       -0.02
36        50             10 -0.50       -0.03      -0.03        -0.02       -0.02
37        50             10 -0.25       -0.02      -0.02        -0.02       -0.02
38        50             10  0.00       -0.03      -0.03        -0.02       -0.02
39        50             10  0.25       -0.03      -0.03        -0.02       -0.02
40        50             10  0.50       -0.03      -0.03        -0.02       -0.02
41        50             25 -0.50       -0.03      -0.03        -0.02       -0.02
42        50             25 -0.25       -0.02      -0.02        -0.02       -0.02
43        50             25  0.00       -0.03      -0.03        -0.02       -0.02
44        50             25  0.25       -0.02      -0.02        -0.02       -0.02
45        50             25  0.50       -0.02      -0.02        -0.02       -0.02
```


### Random effect correlation
With few time points (5, 10) and/or smaller number of clusters (10, 25), both generally seem to overestimate the correlation when it is positive, maybe more so with LGC.


```
   nClusters nWithinCluster corRE biasLGC_corRE biasMM_corRE
1         10              5 -0.50          0.00         0.06
2         10              5 -0.25          0.08         0.15
3         10              5  0.00          0.03         0.10
4         10              5  0.25          0.20         0.18
5         10              5  0.50          0.20         0.09
6         10             10 -0.50         -0.02        -0.01
7         10             10 -0.25          0.01         0.01
8         10             10  0.00          0.01         0.02
9         10             10  0.25          0.04         0.04
10        10             10  0.50          0.06         0.06
11        10             25 -0.50         -0.01        -0.01
12        10             25 -0.25         -0.01        -0.01
13        10             25  0.00          0.00         0.00
14        10             25  0.25          0.01         0.01
15        10             25  0.50          0.01         0.01
16        25              5 -0.50          0.01         0.01
17        25              5 -0.25          0.02         0.02
18        25              5  0.00          0.05         0.05
19        25              5  0.25          0.08         0.06
20        25              5  0.50          0.09         0.06
21        25             10 -0.50          0.00         0.00
22        25             10 -0.25          0.02         0.02
23        25             10  0.00          0.01         0.02
24        25             10  0.25          0.01         0.01
25        25             10  0.50          0.02         0.02
26        25             25 -0.50          0.00         0.00
27        25             25 -0.25         -0.01        -0.01
28        25             25  0.00          0.00         0.00
29        25             25  0.25          0.00         0.00
30        25             25  0.50          0.00         0.00
31        50              5 -0.50          0.00         0.00
32        50              5 -0.25          0.01         0.01
33        50              5  0.00          0.01         0.01
34        50              5  0.25          0.02         0.02
35        50              5  0.50          0.03         0.03
36        50             10 -0.50          0.00         0.00
37        50             10 -0.25          0.00         0.00
38        50             10  0.00          0.00         0.00
39        50             10  0.25          0.01         0.01
40        50             10  0.50          0.01         0.01
41        50             25 -0.50          0.00         0.00
42        50             25 -0.25          0.00         0.00
43        50             25  0.00          0.00         0.00
44        50             25  0.25          0.00         0.00
45        50             25  0.50          0.00         0.00
```


## Interval width
95% confidence intervals were taken as the quantiles of the simulation estimates.

### Fixed effects
General trend of narrower interval estimates for mixed models with smaller sample settings, and low number of clusters generally.


```
   nClusters nWithinCluster corRE widthGrowth_Int widthMixed_Int widthGrowth_Time widthMixed_Time
1         10              5 -0.50            1.23           1.11             0.58            0.48
2         10              5 -0.25            1.27           1.11             0.53            0.44
3         10              5  0.00            1.17           1.10             0.53            0.48
4         10              5  0.25            1.24           1.15             0.53            0.47
5         10              5  0.50            1.25           1.16             0.52            0.45
6         10             10 -0.50            0.93           0.89             0.18            0.17
7         10             10 -0.25            0.86           0.85             0.16            0.15
8         10             10  0.00            0.95           0.87             0.16            0.16
9         10             10  0.25            0.89           0.85             0.17            0.16
10        10             10  0.50            0.90           0.89             0.16            0.16
11        10             25 -0.50            0.54           0.54             0.04            0.04
12        10             25 -0.25            0.56           0.56             0.04            0.04
13        10             25  0.00            0.56           0.56             0.04            0.04
14        10             25  0.25            0.54           0.54             0.04            0.04
15        10             25  0.50            0.53           0.53             0.04            0.04
16        25              5 -0.50            0.69           0.66             0.30            0.30
17        25              5 -0.25            0.67           0.67             0.27            0.27
18        25              5  0.00            0.66           0.66             0.29            0.29
19        25              5  0.25            0.65           0.63             0.27            0.26
20        25              5  0.50            0.62           0.62             0.26            0.26
21        25             10 -0.50            0.48           0.48             0.09            0.09
22        25             10 -0.25            0.48           0.48             0.09            0.09
23        25             10  0.00            0.49           0.49             0.09            0.09
24        25             10  0.25            0.47           0.47             0.09            0.09
25        25             10  0.50            0.54           0.54             0.10            0.10
26        25             25 -0.50            0.30           0.30             0.02            0.02
27        25             25 -0.25            0.31           0.31             0.02            0.02
28        25             25  0.00            0.33           0.33             0.02            0.02
29        25             25  0.25            0.30           0.30             0.02            0.02
30        25             25  0.50            0.31           0.31             0.02            0.02
31        50              5 -0.50            0.42           0.42             0.19            0.19
32        50              5 -0.25            0.41           0.41             0.18            0.18
33        50              5  0.00            0.45           0.45             0.19            0.19
34        50              5  0.25            0.49           0.49             0.20            0.20
35        50              5  0.50            0.46           0.46             0.18            0.18
36        50             10 -0.50            0.33           0.33             0.06            0.06
37        50             10 -0.25            0.32           0.32             0.06            0.06
38        50             10  0.00            0.33           0.33             0.06            0.06
39        50             10  0.25            0.33           0.33             0.06            0.06
40        50             10  0.50            0.33           0.33             0.06            0.06
41        50             25 -0.50            0.23           0.23             0.02            0.02
42        50             25 -0.25            0.21           0.21             0.02            0.02
43        50             25  0.00            0.20           0.20             0.02            0.02
44        50             25  0.25            0.21           0.21             0.02            0.02
45        50             25  0.50            0.23           0.23             0.02            0.02
   mixedWidthMinusgrowthWidth_Int mixedWidthMinusgrowthWidth_Time
1                           -0.12                           -0.10
2                           -0.16                           -0.09
3                           -0.07                           -0.05
4                           -0.09                           -0.06
5                           -0.09                           -0.07
6                           -0.04                           -0.01
7                           -0.01                           -0.01
8                           -0.08                            0.00
9                           -0.04                           -0.01
10                          -0.01                            0.00
11                           0.00                            0.00
12                           0.00                            0.00
13                           0.00                            0.00
14                           0.00                            0.00
15                           0.00                            0.00
16                          -0.03                            0.00
17                           0.00                            0.00
18                           0.00                            0.00
19                          -0.02                           -0.01
20                           0.00                            0.00
21                           0.00                            0.00
22                           0.00                            0.00
23                           0.00                            0.00
24                           0.00                            0.00
25                           0.00                            0.00
26                           0.00                            0.00
27                           0.00                            0.00
28                           0.00                            0.00
29                           0.00                            0.00
30                           0.00                            0.00
31                           0.00                            0.00
32                           0.00                            0.00
33                           0.00                            0.00
34                           0.00                            0.00
35                           0.00                            0.00
36                           0.00                            0.00
37                           0.00                            0.00
38                           0.00                            0.00
39                           0.00                            0.00
40                           0.00                            0.00
41                           0.00                            0.00
42                           0.00                            0.00
43                           0.00                            0.00
44                           0.00                            0.00
45                           0.00                            0.00
```

### Random effect variance
General trend of narrower interval estimates for mixed models with smaller sample settings, and low number of clusters generally.


```
   nClusters nWithinCluster corRE widthGrowth_Int widthMixed_Int widthGrowth_Time widthMixed_Time
1         10              5 -0.50            2.86           2.09             1.21            0.95
2         10              5 -0.25            3.13           2.21             1.08            0.92
3         10              5  0.00            3.17           2.32             1.00            0.89
4         10              5  0.25            3.35           2.39             0.95            0.87
5         10              5  0.50            3.12           2.34             1.01            0.86
6         10             10 -0.50            1.99           1.82             0.30            0.29
7         10             10 -0.25            1.92           1.88             0.31            0.29
8         10             10  0.00            1.90           1.86             0.35            0.32
9         10             10  0.25            1.99           1.88             0.33            0.31
10        10             10  0.50            1.88           1.79             0.33            0.31
11        10             25 -0.50            1.09           1.09             0.07            0.07
12        10             25 -0.25            1.14           1.14             0.08            0.08
13        10             25  0.00            1.04           1.04             0.08            0.08
14        10             25  0.25            1.19           1.19             0.08            0.08
15        10             25  0.50            1.08           1.08             0.08            0.08
16        25              5 -0.50            1.62           1.61             0.53            0.52
17        25              5 -0.25            1.68           1.67             0.54            0.54
18        25              5  0.00            1.59           1.57             0.52            0.51
19        25              5  0.25            1.71           1.64             0.58            0.58
20        25              5  0.50            1.52           1.45             0.56            0.55
21        25             10 -0.50            1.01           1.01             0.17            0.17
22        25             10 -0.25            1.05           1.05             0.17            0.17
23        25             10  0.00            1.02           1.02             0.18            0.18
24        25             10  0.25            1.09           1.09             0.18            0.18
25        25             10  0.50            1.05           1.05             0.19            0.19
26        25             25 -0.50            0.65           0.65             0.05            0.05
27        25             25 -0.25            0.63           0.63             0.04            0.04
28        25             25  0.00            0.64           0.64             0.04            0.04
29        25             25  0.25            0.66           0.66             0.04            0.04
30        25             25  0.50            0.62           0.62             0.04            0.04
31        50              5 -0.50            1.07           1.07             0.37            0.37
32        50              5 -0.25            1.10           1.10             0.41            0.41
33        50              5  0.00            1.15           1.15             0.39            0.39
34        50              5  0.25            1.16           1.16             0.38            0.38
35        50              5  0.50            1.10           1.08             0.41            0.41
36        50             10 -0.50            0.70           0.70             0.12            0.12
37        50             10 -0.25            0.72           0.72             0.13            0.13
38        50             10  0.00            0.68           0.68             0.12            0.12
39        50             10  0.25            0.74           0.74             0.12            0.12
40        50             10  0.50            0.67           0.67             0.13            0.13
41        50             25 -0.50            0.45           0.45             0.03            0.03
42        50             25 -0.25            0.47           0.47             0.04            0.04
43        50             25  0.00            0.43           0.43             0.03            0.03
44        50             25  0.25            0.48           0.48             0.03            0.03
45        50             25  0.50            0.45           0.45             0.04            0.04
   mixedWidthMinusgrowthWidth_Int mixedWidthMinusgrowthWidth_Time
1                           -0.77                           -0.26
2                           -0.92                           -0.16
3                           -0.85                           -0.11
4                           -0.96                           -0.08
5                           -0.78                           -0.15
6                           -0.17                           -0.01
7                           -0.04                           -0.02
8                           -0.04                           -0.03
9                           -0.11                           -0.02
10                          -0.09                           -0.02
11                           0.00                            0.00
12                           0.00                            0.00
13                           0.00                            0.00
14                           0.00                            0.00
15                           0.00                            0.00
16                          -0.01                           -0.01
17                          -0.01                            0.00
18                          -0.02                           -0.01
19                          -0.07                            0.00
20                          -0.07                           -0.01
21                           0.00                            0.00
22                           0.00                            0.00
23                           0.00                            0.00
24                           0.00                            0.00
25                           0.00                            0.00
26                           0.00                            0.00
27                           0.00                            0.00
28                           0.00                            0.00
29                           0.00                            0.00
30                           0.00                            0.00
31                           0.00                            0.00
32                           0.00                            0.00
33                           0.00                            0.00
34                           0.00                            0.00
35                          -0.02                            0.00
36                           0.00                            0.00
37                           0.00                            0.00
38                           0.00                            0.00
39                           0.00                            0.00
40                           0.00                            0.00
41                           0.00                            0.00
42                           0.00                            0.00
43                           0.00                            0.00
44                           0.00                            0.00
45                           0.00                            0.00
```


### Random effect correlation
The actual interval estimates for LGC were not confined to [-1,1], and results are somewhat mixed until overall number of observations exceeds 100, where there is no preference the approaches.


```
   nClusters nWithinCluster corRE widthGrowth widthMixed mixedWidthMinusgrowthWidth
1         10              5 -0.50        1.60       2.00                       0.40
2         10              5 -0.25        1.65       2.00                       0.35
3         10              5  0.00        1.89       1.80                      -0.09
4         10              5  0.25        2.30       1.34                      -0.96
5         10              5  0.50        2.95       1.14                      -1.81
6         10             10 -0.50        0.96       0.96                       0.00
7         10             10 -0.25        1.00       1.04                       0.04
8         10             10  0.00        1.03       1.13                       0.10
9         10             10  0.25        1.10       1.20                       0.10
10        10             10  0.50        1.17       0.96                      -0.21
11        10             25 -0.50        0.46       0.46                       0.00
12        10             25 -0.25        0.63       0.63                       0.00
13        10             25  0.00        0.64       0.64                       0.00
14        10             25  0.25        0.61       0.61                       0.00
15        10             25  0.50        0.56       0.56                       0.00
16        25              5 -0.50        0.63       0.69                       0.06
17        25              5 -0.25        0.77       0.79                       0.02
18        25              5  0.00        0.95       0.99                       0.04
19        25              5  0.25        1.22       1.12                      -0.10
20        25              5  0.50        1.22       0.91                      -0.31
21        25             10 -0.50        0.42       0.42                       0.00
22        25             10 -0.25        0.45       0.45                       0.00
23        25             10  0.00        0.55       0.55                       0.00
24        25             10  0.25        0.54       0.54                       0.00
25        25             10  0.50        0.56       0.56                       0.00
26        25             25 -0.50        0.30       0.30                       0.00
27        25             25 -0.25        0.30       0.30                       0.00
28        25             25  0.00        0.37       0.37                       0.00
29        25             25  0.25        0.31       0.31                       0.00
30        25             25  0.50        0.29       0.29                       0.00
31        50              5 -0.50        0.36       0.36                       0.00
32        50              5 -0.25        0.53       0.53                       0.00
33        50              5  0.00        0.56       0.56                       0.00
34        50              5  0.25        0.64       0.64                       0.00
35        50              5  0.50        0.81       0.78                      -0.03
36        50             10 -0.50        0.28       0.28                       0.00
37        50             10 -0.25        0.31       0.32                       0.01
38        50             10  0.00        0.35       0.35                       0.00
39        50             10  0.25        0.35       0.35                       0.00
40        50             10  0.50        0.35       0.35                       0.00
41        50             25 -0.50        0.19       0.19                       0.00
42        50             25 -0.25        0.22       0.22                       0.00
43        50             25  0.00        0.23       0.23                       0.00
44        50             25  0.25        0.23       0.23                       0.00
45        50             25  0.50        0.20       0.20                       0.00
```

# Summary
LGC probably should not be run with very few time points, at least for these cluster sizes.  Both approaches underestimate the random effects variance at these cluster sizes. Where there are differences which aren't in many of the tested situations, mixed models tend to have the preferable results.

# Future

Balanced vs not

Varying residual variance

vs. Bayesian mixed




# Code
[link](https://github.com/mclark--/Miscellaneous-R-Code/tree/master/SC%20and%20TR/mixedModels/growthCurvevsMixedModel.R)