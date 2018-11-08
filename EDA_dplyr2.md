dplyr패키지를 이용한 레코드 필터링/슬라이싱
-------------------------------------------

-   **ggplot2 패키지에 내장된 diamonds 데이터셋 활용**

<!-- -->

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(ggplot2)
    data(diamonds)
    head(diamonds)

    ## # A tibble: 6 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ## 2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
    ## 3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ## 4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ## 5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ## 6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48

-   **데이터셋 샘플링 활용**

<!-- -->

    choice<-sample(1:NROW(diamonds),NROW(diamonds)*0.1,replace = FALSE)
    diamonds<-diamonds[choice,]
    glimpse(diamonds)

    ## Observations: 5,394
    ## Variables: 10
    ## $ carat   <dbl> 0.41, 0.41, 0.31, 1.04, 1.20, 1.11, 0.41, 0.95, 2.80, ...
    ## $ cut     <ord> Premium, Ideal, Premium, Premium, Very Good, Ideal, Id...
    ## $ color   <ord> E, E, D, E, E, H, F, J, I, D, D, E, I, J, D, H, G, E, ...
    ## $ clarity <ord> SI2, VS2, VS2, VS1, SI1, VS2, SI1, SI1, SI2, VS2, SI2,...
    ## $ depth   <dbl> 61.9, 61.8, 62.0, 58.6, 61.8, 64.0, 61.4, 63.0, 61.1, ...
    ## $ table   <dbl> 55, 54, 59, 59, 56, 58, 56, 56, 59, 56, 59, 56, 56, 53...
    ## $ price   <int> 876, 935, 942, 6765, 6534, 5632, 824, 3275, 15030, 403...
    ## $ x       <dbl> 4.80, 4.76, 4.36, 6.68, 6.75, 6.52, 4.77, 6.32, 9.03, ...
    ## $ y       <dbl> 4.77, 4.79, 4.32, 6.64, 6.82, 6.46, 4.80, 6.26, 8.98, ...
    ## $ z       <dbl> 2.96, 2.95, 2.69, 3.90, 4.19, 4.16, 2.94, 3.96, 5.50, ...

-   **특정 일치조건 1개 지정/제외 필터링**

<!-- -->

    #특정 일치조건 1개 지정 필터링
    #filter(diamonds, cut == "Ideal")
    diamonds%>%filter(cut=="Ideal")

    ## # A tibble: 2,174 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.41 Ideal E     VS2      61.8    54   935  4.76  4.79  2.95
    ##  2  1.11 Ideal H     VS2      64      58  5632  6.52  6.46  4.16
    ##  3  0.41 Ideal F     SI1      61.4    56   824  4.77  4.8   2.94
    ##  4  0.95 Ideal J     SI1      63      56  3275  6.32  6.26  3.96
    ##  5  0.31 Ideal E     VS1      61.6    56   942  4.38  4.35  2.69
    ##  6  2.53 Ideal J     SI2      61.5    53 15993  8.78  8.74  5.39
    ##  7  0.32 Ideal G     VS2      61.7    57   720  4.4   4.38  2.71
    ##  8  0.45 Ideal G     VVS1     61.7    56  1297  4.93  4.96  3.05
    ##  9  0.55 Ideal J     VS1      62.3    56  1550  5.26  5.24  3.27
    ## 10  1.13 Ideal I     SI1      61.4    57  4936  6.72  6.69  4.12
    ## # ... with 2,164 more rows

    #틀정 일치조건 1개 제외 필터링
    #filter(diamonds,cut!="Ideal")
    diamonds%>%filter(cut!="Ideal")

    ## # A tibble: 3,220 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.41 Premium   E     SI2      61.9    55   876  4.8   4.77  2.96
    ##  2  0.31 Premium   D     VS2      62      59   942  4.36  4.32  2.69
    ##  3  1.04 Premium   E     VS1      58.6    59  6765  6.68  6.64  3.9 
    ##  4  1.2  Very Good E     SI1      61.8    56  6534  6.75  6.82  4.19
    ##  5  2.8  Premium   I     SI2      61.1    59 15030  9.03  8.98  5.5 
    ##  6  0.26 Good      D     VS2      65.2    56   403  3.99  4.02  2.61
    ##  7  1.24 Premium   D     SI2      61      59  5311  6.93  6.97  4.24
    ##  8  1.14 Good      I     SI1      63.5    56  4702  6.58  6.62  4.19
    ##  9  0.77 Premium   D     VS2      61.3    60  3428  5.92  5.89  3.62
    ## 10  1    Premium   H     SI2      61.3    58  3920  6.45  6.41  3.94
    ## # ... with 3,210 more rows

-   **특정 일치조건 2개 지정/제외 필터링**

<!-- -->

    #지정 필터링
    filter(diamonds,cut %in% c("Ideal","Good"))

    ## # A tibble: 2,671 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.41 Ideal E     VS2      61.8    54   935  4.76  4.79  2.95
    ##  2  1.11 Ideal H     VS2      64      58  5632  6.52  6.46  4.16
    ##  3  0.41 Ideal F     SI1      61.4    56   824  4.77  4.8   2.94
    ##  4  0.95 Ideal J     SI1      63      56  3275  6.32  6.26  3.96
    ##  5  0.26 Good  D     VS2      65.2    56   403  3.99  4.02  2.61
    ##  6  0.31 Ideal E     VS1      61.6    56   942  4.38  4.35  2.69
    ##  7  1.14 Good  I     SI1      63.5    56  4702  6.58  6.62  4.19
    ##  8  2.53 Ideal J     SI2      61.5    53 15993  8.78  8.74  5.39
    ##  9  0.32 Ideal G     VS2      61.7    57   720  4.4   4.38  2.71
    ## 10  0.45 Ideal G     VVS1     61.7    56  1297  4.93  4.96  3.05
    ## # ... with 2,661 more rows

    #filter(diamonds,cut=="Ideal"|cut=="Good")

    #diamonds%>%filter(cut%in%c("Ideal","Good"))
    diamonds%>%filter(cut=="Ideal"|cut=="Good")

    ## # A tibble: 2,671 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.41 Ideal E     VS2      61.8    54   935  4.76  4.79  2.95
    ##  2  1.11 Ideal H     VS2      64      58  5632  6.52  6.46  4.16
    ##  3  0.41 Ideal F     SI1      61.4    56   824  4.77  4.8   2.94
    ##  4  0.95 Ideal J     SI1      63      56  3275  6.32  6.26  3.96
    ##  5  0.26 Good  D     VS2      65.2    56   403  3.99  4.02  2.61
    ##  6  0.31 Ideal E     VS1      61.6    56   942  4.38  4.35  2.69
    ##  7  1.14 Good  I     SI1      63.5    56  4702  6.58  6.62  4.19
    ##  8  2.53 Ideal J     SI2      61.5    53 15993  8.78  8.74  5.39
    ##  9  0.32 Ideal G     VS2      61.7    57   720  4.4   4.38  2.71
    ## 10  0.45 Ideal G     VVS1     61.7    56  1297  4.93  4.96  3.05
    ## # ... with 2,661 more rows

    #제외 필터링
    #filter(diamonds,cut!="Ideal"&cut!="Good")
    diamonds%>%filter(cut!="Ideal"&cut!="Good")

    ## # A tibble: 2,723 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.41 Premium   E     SI2      61.9    55   876  4.8   4.77  2.96
    ##  2  0.31 Premium   D     VS2      62      59   942  4.36  4.32  2.69
    ##  3  1.04 Premium   E     VS1      58.6    59  6765  6.68  6.64  3.9 
    ##  4  1.2  Very Good E     SI1      61.8    56  6534  6.75  6.82  4.19
    ##  5  2.8  Premium   I     SI2      61.1    59 15030  9.03  8.98  5.5 
    ##  6  1.24 Premium   D     SI2      61      59  5311  6.93  6.97  4.24
    ##  7  0.77 Premium   D     VS2      61.3    60  3428  5.92  5.89  3.62
    ##  8  1    Premium   H     SI2      61.3    58  3920  6.45  6.41  3.94
    ##  9  0.52 Very Good E     VS2      62.2    56  1665  5.16  5.17  3.21
    ## 10  0.34 Premium   G     VS2      61.2    58   596  4.46  4.5   2.74
    ## # ... with 2,713 more rows

-   **특정 비교조건,or,and 필터링**

<!-- -->

    #비교조건 지정 필터링
    #filter(diamonds,price>=1000)
    diamonds%>%filter(price>=1000)

    ## # A tibble: 3,914 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  1.04 Premium   E     VS1      58.6    59  6765  6.68  6.64  3.9 
    ##  2  1.2  Very Good E     SI1      61.8    56  6534  6.75  6.82  4.19
    ##  3  1.11 Ideal     H     VS2      64      58  5632  6.52  6.46  4.16
    ##  4  0.95 Ideal     J     SI1      63      56  3275  6.32  6.26  3.96
    ##  5  2.8  Premium   I     SI2      61.1    59 15030  9.03  8.98  5.5 
    ##  6  1.24 Premium   D     SI2      61      59  5311  6.93  6.97  4.24
    ##  7  1.14 Good      I     SI1      63.5    56  4702  6.58  6.62  4.19
    ##  8  2.53 Ideal     J     SI2      61.5    53 15993  8.78  8.74  5.39
    ##  9  0.77 Premium   D     VS2      61.3    60  3428  5.92  5.89  3.62
    ## 10  1    Premium   H     SI2      61.3    58  3920  6.45  6.41  3.94
    ## # ... with 3,904 more rows

    #and 이용 필터링
    filter(diamonds,carat>2,price<14000)

    ## # A tibble: 70 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  2.01 Ideal     I     SI1      62.8    57 10389  8.04  7.98  5.03
    ##  2  2.54 Very Good I     SI2      63.4    56 12095  8.68  8.64  5.49
    ##  3  2.25 Very Good J     SI2      58.4    63 13597  8.6   8.65  5.04
    ##  4  2.06 Very Good J     VS2      61.8    59 13948  8.14  8.2   5.05
    ##  5  2.02 Premium   F     SI2      61      59 11962  8.2   8.13  4.98
    ##  6  2.17 Premium   J     SI2      62.3    58 13556  8.31  8.26  5.16
    ##  7  2.21 Very Good J     SI2      60.6    62 12215  8.37  8.45  5.1 
    ##  8  2.02 Premium   D     SI2      61.1    57 12108  8.12  8.05  4.94
    ##  9  2.03 Very Good J     SI2      61.7    61 11968  8.04  8.18  5   
    ## 10  2.03 Ideal     J     SI1      62.6    55 13357  8.06  8.1   5.06
    ## # ... with 60 more rows

    #filter(diamonds,carat>2&price<14000)

    #diamonds%>%filter(carat>2,price<14000)
    diamonds%>%filter(carat>2&price<14000)

    ## # A tibble: 70 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  2.01 Ideal     I     SI1      62.8    57 10389  8.04  7.98  5.03
    ##  2  2.54 Very Good I     SI2      63.4    56 12095  8.68  8.64  5.49
    ##  3  2.25 Very Good J     SI2      58.4    63 13597  8.6   8.65  5.04
    ##  4  2.06 Very Good J     VS2      61.8    59 13948  8.14  8.2   5.05
    ##  5  2.02 Premium   F     SI2      61      59 11962  8.2   8.13  4.98
    ##  6  2.17 Premium   J     SI2      62.3    58 13556  8.31  8.26  5.16
    ##  7  2.21 Very Good J     SI2      60.6    62 12215  8.37  8.45  5.1 
    ##  8  2.02 Premium   D     SI2      61.1    57 12108  8.12  8.05  4.94
    ##  9  2.03 Very Good J     SI2      61.7    61 11968  8.04  8.18  5   
    ## 10  2.03 Ideal     J     SI1      62.6    55 13357  8.06  8.1   5.06
    ## # ... with 60 more rows

    #or 이용 필터링
    #filter(diamonds,carat<1|carat>5)
    diamonds%>%filter(carat<1|carat>5)

    ## # A tibble: 3,471 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.41 Premium   E     SI2      61.9    55   876  4.8   4.77  2.96
    ##  2  0.41 Ideal     E     VS2      61.8    54   935  4.76  4.79  2.95
    ##  3  0.31 Premium   D     VS2      62      59   942  4.36  4.32  2.69
    ##  4  0.41 Ideal     F     SI1      61.4    56   824  4.77  4.8   2.94
    ##  5  0.95 Ideal     J     SI1      63      56  3275  6.32  6.26  3.96
    ##  6  0.26 Good      D     VS2      65.2    56   403  3.99  4.02  2.61
    ##  7  0.31 Ideal     E     VS1      61.6    56   942  4.38  4.35  2.69
    ##  8  0.77 Premium   D     VS2      61.3    60  3428  5.92  5.89  3.62
    ##  9  0.32 Ideal     G     VS2      61.7    57   720  4.4   4.38  2.71
    ## 10  0.52 Very Good E     VS2      62.2    56  1665  5.16  5.17  3.21
    ## # ... with 3,461 more rows

-   **특정 일치행 1개 범위 지정/제외 슬라이싱**

<!-- -->

    #지정 슬라이싱
    #slice(diamonds,5)
    diamonds%>%slice(5)

    ## # A tibble: 1 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1   1.2 Very Good E     SI1      61.8    56  6534  6.75  6.82  4.19

    #제외 슬라이싱
    #slice(diamonds,-5)
    diamonds%>%slice(-5)

    ## # A tibble: 5,393 x 10
    ##    carat cut     color clarity depth table price     x     y     z
    ##    <dbl> <ord>   <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.41 Premium E     SI2      61.9    55   876  4.8   4.77  2.96
    ##  2  0.41 Ideal   E     VS2      61.8    54   935  4.76  4.79  2.95
    ##  3  0.31 Premium D     VS2      62      59   942  4.36  4.32  2.69
    ##  4  1.04 Premium E     VS1      58.6    59  6765  6.68  6.64  3.9 
    ##  5  1.11 Ideal   H     VS2      64      58  5632  6.52  6.46  4.16
    ##  6  0.41 Ideal   F     SI1      61.4    56   824  4.77  4.8   2.94
    ##  7  0.95 Ideal   J     SI1      63      56  3275  6.32  6.26  3.96
    ##  8  2.8  Premium I     SI2      61.1    59 15030  9.03  8.98  5.5 
    ##  9  0.26 Good    D     VS2      65.2    56   403  3.99  4.02  2.61
    ## 10  1.24 Premium D     SI2      61      59  5311  6.93  6.97  4.24
    ## # ... with 5,383 more rows

-   **특정 일치행 복수 지정/제외 슬라이싱**

<!-- -->

    #지정 슬라이싱
    #slice(diamonds,c(2,7,3:5))
    diamonds%>%slice(c(2,7,3:5))

    ## # A tibble: 5 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  0.41 Ideal     E     VS2      61.8    54   935  4.76  4.79  2.95
    ## 2  0.41 Ideal     F     SI1      61.4    56   824  4.77  4.8   2.94
    ## 3  0.31 Premium   D     VS2      62      59   942  4.36  4.32  2.69
    ## 4  1.04 Premium   E     VS1      58.6    59  6765  6.68  6.64  3.9 
    ## 5  1.2  Very Good E     SI1      61.8    56  6534  6.75  6.82  4.19

    #제외 슬라이싱
    #slice(diamonds,-c(2,7,3:5))
    diamonds%>%slice(-c(2,7,3:5))

    ## # A tibble: 5,389 x 10
    ##    carat cut     color clarity depth table price     x     y     z
    ##    <dbl> <ord>   <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.41 Premium E     SI2      61.9    55   876  4.8   4.77  2.96
    ##  2  1.11 Ideal   H     VS2      64      58  5632  6.52  6.46  4.16
    ##  3  0.95 Ideal   J     SI1      63      56  3275  6.32  6.26  3.96
    ##  4  2.8  Premium I     SI2      61.1    59 15030  9.03  8.98  5.5 
    ##  5  0.26 Good    D     VS2      65.2    56   403  3.99  4.02  2.61
    ##  6  1.24 Premium D     SI2      61      59  5311  6.93  6.97  4.24
    ##  7  0.31 Ideal   E     VS1      61.6    56   942  4.38  4.35  2.69
    ##  8  1.14 Good    I     SI1      63.5    56  4702  6.58  6.62  4.19
    ##  9  2.53 Ideal   J     SI2      61.5    53 15993  8.78  8.74  5.39
    ## 10  0.77 Premium D     VS2      61.3    60  3428  5.92  5.89  3.62
    ## # ... with 5,379 more rows
