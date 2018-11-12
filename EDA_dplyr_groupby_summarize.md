summarize & group\_by 함수
--------------------------

-   **데이터셋 로딩**

<!-- -->

    library(ggplot2)
    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

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

### dplyr::summarize() 함수를 이용한 기술통계

-   **특정 변수 컬럼1개를 단순 요약**

<!-- -->

    #diamonds %>% dplyr::summarize(mean(price))
    diamonds %>% dplyr::summarize(Avg_Price = mean(price))

    ## # A tibble: 1 x 1
    ##   Avg_Price
    ##       <dbl>
    ## 1     3933.

    #산술평균값에 별도 이름 부여

-   **특정 변수 컬럼1개를 여러 기술통계로 요약**

<!-- -->

    diamonds %>% dplyr::summarize(Avg_Price = mean(price),
                                  Medi_Price = median(price), 
                                  Var_Price = var(price),
                                  SD_Price = sd(price)
                                  )

    ## # A tibble: 1 x 4
    ##   Avg_Price Medi_Price Var_Price SD_Price
    ##       <dbl>      <dbl>     <dbl>    <dbl>
    ## 1     3933.       2401 15915629.    3989.

    #한번에 여러가지 기술통계량을 쉽게 구할 수 있으며, 각 결과값에 별도 레이블 반영 용이

-   **요약집계 기준변수 1개로 요약집계 결과번수 1개를 분석**

<!-- -->

    diamonds %>% 
      group_by(cut) %>%
      dplyr::summarize(Avg_Price = mean(price)) %>% 
      arrange(desc(Avg_Price))

    ## # A tibble: 5 x 2
    ##   cut       Avg_Price
    ##   <ord>         <dbl>
    ## 1 Premium       4584.
    ## 2 Fair          4359.
    ## 3 Very Good     3982.
    ## 4 Good          3929.
    ## 5 Ideal         3458.

    #group_by()함수를 이용해 요약집계 기준변수로 cut변수컬럼을 지정
    #summarize()함수를 이용해 cut변수컬럼에 속한 5개 다이아몬드 커팅 품질에 따라 요약집계 결과변수인
    #다이아몬드 가격을 요약집계
    #arrange()함수를 사용해 요약집계한 평균가격을 내림차순으로 정렬함

-   **요약집계 기준변수 여러개로 요약집계 결과번수 여러개를 다차원분석**

<!-- -->

    diamonds %>% 
      group_by(cut, color) %>%
      dplyr::summarize(Avg_Price = mean(price),
                       Avg_Carat = mean(carat)) %>% 
      arrange(desc(Avg_Price), Avg_Carat)

    ## # A tibble: 35 x 4
    ## # Groups:   cut [5]
    ##    cut       color Avg_Price Avg_Carat
    ##    <ord>     <ord>     <dbl>     <dbl>
    ##  1 Premium   J         6295.      1.29
    ##  2 Premium   I         5946.      1.14
    ##  3 Very Good I         5256.      1.05
    ##  4 Premium   H         5217.      1.02
    ##  5 Fair      H         5136.      1.22
    ##  6 Very Good J         5104.      1.13
    ##  7 Good      I         5079.      1.06
    ##  8 Fair      J         4976.      1.34
    ##  9 Ideal     J         4918.      1.06
    ## 10 Fair      I         4685.      1.20
    ## # ... with 25 more rows

-   **데이터셋 로딩2**

<!-- -->

    data(mpg)
    head(mpg)

    ## # A tibble: 6 x 11
    ##   manufacturer model displ  year   cyl trans drv     cty   hwy fl    class
    ##   <chr>        <chr> <dbl> <int> <int> <chr> <chr> <int> <int> <chr> <chr>
    ## 1 audi         a4      1.8  1999     4 auto~ f        18    29 p     comp~
    ## 2 audi         a4      1.8  1999     4 manu~ f        21    29 p     comp~
    ## 3 audi         a4      2    2008     4 manu~ f        20    31 p     comp~
    ## 4 audi         a4      2    2008     4 auto~ f        21    30 p     comp~
    ## 5 audi         a4      2.8  1999     6 auto~ f        16    26 p     comp~
    ## 6 audi         a4      2.8  1999     6 manu~ f        18    26 p     comp~

-   **집계기준 변수 팩터화를 통한 레이블링**

<!-- -->

    # cyl(실린더 유형) 변수 팩터화
    table(mpg$cyl)

    ## 
    ##  4  5  6  8 
    ## 81  4 79 70

    mpg$cyl.f <- factor(mpg$cyl, levels = c(4, 5, 6, 8),
                        labels = c("4기통", "5기통", "6기통", "8기통"),
                        ordered = TRUE)
    # drv(구동방식) 변수 팩터화
    table(mpg$drv)

    ## 
    ##   4   f   r 
    ## 103 106  25

    mpg$drv.f <- factor(mpg$drv, levels = c("4", "f", "r"),
                        labels = c("4륜구동", "전륜구동", "후륜구동"),
                        ordered = TRUE)
    # fl(연료유형) 변수 팩터화
    table(mpg$fl)

    ## 
    ##   c   d   e   p   r 
    ##   1   5   8  52 168

    mpg$fl.f <- factor(mpg$fl, levels = c("c", "d", "e","p", "r"),
                       labels = c("압축천연가스", "디젤", "에타놀", "프리미엄", "일반"),
                       ordered = TRUE)
    # class(차량유형) 변수 팩터화
    table(mpg$class)

    ## 
    ##    2seater    compact    midsize    minivan     pickup subcompact 
    ##          5         47         41         11         33         35 
    ##        suv 
    ##         62

    mpg$class.f <- factor(mpg$class, 
                          levels = c("2seater", "subcompact", "compact", 
                                     "midsize", "suv", "minivan", "pickup"),
                          labels = c("2인승", "경차", "소형차",
                                     "중형차", "SUV", "미니밴", "픽업트럭"),
                          ordered = TRUE)
    # trans(기어방식) 변수 범주화 & 팩터화 
    table(mpg$trans)

    ## 
    ##   auto(av)   auto(l3)   auto(l4)   auto(l5)   auto(l6)   auto(s4) 
    ##          5          2         83         39          6          3 
    ##   auto(s5)   auto(s6) manual(m5) manual(m6) 
    ##          3         16         58         19

    library(doBy)
    mpg$trans.f <- factor(recodeVar(mpg$trans, 
                                    src = list(c("auto(av)", "auto(l3)", "auto(l4)",
                                                 "auto(l5)", "auto(l6)", "auto(s4)",
                                                 "auto(s5)", "auto(s6)"),
                                               c("manual(m5)", "manual(m6)")),
                                    tgt = list("자동기어", "수동기어")), 
                          ordered = TRUE)

-   **요약집계 기준변수 1개로 요약집계 결과변수 1개를 분석**

<!-- -->

    mpg %>% 
      group_by(drv.f) %>%
      dplyr::summarize(Avg_hwy = mean(hwy, na.rm = TRUE)) %>% 
      arrange(desc(Avg_hwy))

    ## # A tibble: 3 x 2
    ##   drv.f    Avg_hwy
    ##   <ord>      <dbl>
    ## 1 전륜구동    28.2
    ## 2 후륜구동    21  
    ## 3 4륜구동     19.2

-   **요약집계 기준변수 여러 개 요약집계 결과변수 1개를 분석**

<!-- -->

    mpg %>% 
      group_by(trans.f, drv.f) %>%
      dplyr::summarize(Avg_hwy = mean(hwy, na.rm = TRUE)) %>% 
      arrange(desc(Avg_hwy))

    ## # A tibble: 6 x 3
    ## # Groups:   trans.f [2]
    ##   trans.f  drv.f    Avg_hwy
    ##   <ord>    <ord>      <dbl>
    ## 1 수동기어 전륜구동    29.5
    ## 2 자동기어 전륜구동    27.3
    ## 3 수동기어 후륜구동    24.1
    ## 4 수동기어 4륜구동     20.8
    ## 5 자동기어 후륜구동    19.5
    ## 6 자동기어 4륜구동     18.6

    mpg %>% 
      group_by(trans.f, drv.f, fl.f) %>%
      dplyr::summarize(Avg_hwy = mean(hwy, na.rm = TRUE)) %>% 
      arrange(desc(Avg_hwy))

    ## # A tibble: 20 x 4
    ## # Groups:   trans.f, drv.f [6]
    ##    trans.f  drv.f    fl.f         Avg_hwy
    ##    <ord>    <ord>    <ord>          <dbl>
    ##  1 수동기어 전륜구동 디젤            44  
    ##  2 자동기어 전륜구동 디젤            41  
    ##  3 자동기어 전륜구동 압축천연가스    36  
    ##  4 수동기어 전륜구동 일반            28.9
    ##  5 수동기어 전륜구동 프리미엄        28.5
    ##  6 자동기어 전륜구동 프리미엄        27.2
    ##  7 자동기어 전륜구동 일반            27.1
    ##  8 수동기어 4륜구동  프리미엄        25.7
    ##  9 수동기어 후륜구동 일반            24.2
    ## 10 수동기어 후륜구동 프리미엄        24  
    ## 11 자동기어 4륜구동  프리미엄        21.7
    ## 12 자동기어 후륜구동 프리미엄        21.3
    ## 13 수동기어 4륜구동  일반            19.8
    ## 14 자동기어 4륜구동  디젤            19.5
    ## 15 자동기어 후륜구동 일반            19.5
    ## 16 자동기어 4륜구동  일반            18.3
    ## 17 자동기어 전륜구동 에타놀          17  
    ## 18 자동기어 후륜구동 에타놀          15  
    ## 19 자동기어 4륜구동  에타놀          12.4
    ## 20 수동기어 4륜구동  에타놀          12

-   **요약집계 기준변수 다차원 + 요약집계 결과변수 다차원 분석**

<!-- -->

    mpg %>% 
      group_by(trans.f, drv.f) %>%
      dplyr::summarize(Avg_hwy = mean(hwy, na.rm = TRUE), 
                       Avg_cty = mean(cty, na.rm = TRUE)) %>% 
      arrange(desc(Avg_hwy), desc(Avg_cty))

    ## # A tibble: 6 x 4
    ## # Groups:   trans.f [2]
    ##   trans.f  drv.f    Avg_hwy Avg_cty
    ##   <ord>    <ord>      <dbl>   <dbl>
    ## 1 수동기어 전륜구동    29.5    21.3
    ## 2 자동기어 전륜구동    27.3    19.1
    ## 3 수동기어 후륜구동    24.1    15.8
    ## 4 수동기어 4륜구동     20.8    15.6
    ## 5 자동기어 후륜구동    19.5    13.3
    ## 6 자동기어 4륜구동     18.6    13.9

    mpg %>% 
      group_by(trans.f, drv.f, fl.f) %>%
      dplyr::summarize(Avg_hwy = mean(hwy, na.rm = TRUE), 
                       Avg_cty = mean(cty, na.rm = TRUE)) %>% 
      arrange(desc(Avg_hwy), desc(Avg_cty))

    ## # A tibble: 20 x 5
    ## # Groups:   trans.f, drv.f [6]
    ##    trans.f  drv.f    fl.f         Avg_hwy Avg_cty
    ##    <ord>    <ord>    <ord>          <dbl>   <dbl>
    ##  1 수동기어 전륜구동 디젤            44      34  
    ##  2 자동기어 전륜구동 디젤            41      29  
    ##  3 자동기어 전륜구동 압축천연가스    36      24  
    ##  4 수동기어 전륜구동 일반            28.9    20.8
    ##  5 수동기어 전륜구동 프리미엄        28.5    20.4
    ##  6 자동기어 전륜구동 프리미엄        27.2    18.4
    ##  7 자동기어 전륜구동 일반            27.1    19.2
    ##  8 수동기어 4륜구동  프리미엄        25.7    18  
    ##  9 수동기어 후륜구동 일반            24.2    16.2
    ## 10 수동기어 후륜구동 프리미엄        24      15.2
    ## 11 자동기어 4륜구동  프리미엄        21.7    15.1
    ## 12 자동기어 후륜구동 프리미엄        21.3    13.7
    ## 13 수동기어 4륜구동  일반            19.8    15.2
    ## 14 자동기어 4륜구동  디젤            19.5    15.5
    ## 15 자동기어 후륜구동 일반            19.5    13.4
    ## 16 자동기어 4륜구동  일반            18.3    13.9
    ## 17 자동기어 전륜구동 에타놀          17      11  
    ## 18 자동기어 후륜구동 에타놀          15      11  
    ## 19 자동기어 4륜구동  에타놀          12.4     9.4
    ## 20 수동기어 4륜구동  에타놀          12       9
