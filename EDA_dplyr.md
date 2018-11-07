dplyr패키지를 활용한 데이터다루기
---------------------------------

-   **ggplot2 패키지에 내장된 diamonds 데이터셋 활용**

<!-- -->

    #ggplot2 로딩 & diamonds 데이터셋 로딩
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

    #데이터 샘플링을 통해서 진행
    choice<-sample(1:NROW(diamonds),NROW(diamonds)*0.1,replace = FALSE)
    NROW(choice)

    ## [1] 5394

    diamonds<-diamonds[choice,]

-   **데이터 구조파악 및 기술통계**

<!-- -->

    #dplyr패키지 로딩
    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    glimpse(diamonds)

    ## Observations: 5,394
    ## Variables: 10
    ## $ carat   <dbl> 0.35, 2.03, 1.17, 0.90, 0.55, 0.33, 0.57, 1.20, 1.23, ...
    ## $ cut     <ord> Very Good, Very Good, Premium, Premium, Very Good, Ide...
    ## $ color   <ord> F, F, J, G, D, D, F, J, G, I, F, F, G, J, H, E, E, I, ...
    ## $ clarity <ord> SI1, SI2, I1, VS2, VS2, VS2, SI1, VS1, VS1, SI1, SI2, ...
    ## $ depth   <dbl> 63.1, 59.8, 60.2, 61.0, 59.8, 62.5, 58.6, 59.1, 62.2, ...
    ## $ table   <dbl> 56, 56, 61, 58, 57, 55, 57, 61, 58, 61, 57, 59, 58, 58...
    ## $ price   <int> 748, 15811, 2825, 4022, 1867, 781, 1364, 5107, 5786, 1...
    ## $ x       <dbl> 4.51, 8.21, 6.90, 6.20, 5.31, 4.41, 5.37, 7.00, 6.78, ...
    ## $ y       <dbl> 4.46, 8.30, 6.83, 6.13, 5.35, 4.45, 5.59, 6.94, 6.74, ...
    ## $ z       <dbl> 2.83, 4.94, 4.13, 3.76, 3.19, 2.77, 3.21, 4.12, 4.27, ...

    #특정 변수 컬럼 인덱싱/셀렉션 하기
    select(diamonds,carat)#case 1

    ## # A tibble: 5,394 x 1
    ##    carat
    ##    <dbl>
    ##  1 0.35 
    ##  2 2.03 
    ##  3 1.17 
    ##  4 0.9  
    ##  5 0.55 
    ##  6 0.33 
    ##  7 0.570
    ##  8 1.2  
    ##  9 1.23 
    ## 10 2.06 
    ## # ... with 5,384 more rows

    #diamonds%>%select(carat)#case 2
    #select(diamonds,1)#case 3
    #diamonds%>%select(1)#case 4

    #특정 변수 컬럼 제외 인덱싱/셀렉션 하기
    select(diamonds, -carat) # 변수컬럼명으로 셀렉션

    ## # A tibble: 5,394 x 9
    ##    cut       color clarity depth table price     x     y     z
    ##    <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 Very Good F     SI1      63.1    56   748  4.51  4.46  2.83
    ##  2 Very Good F     SI2      59.8    56 15811  8.21  8.3   4.94
    ##  3 Premium   J     I1       60.2    61  2825  6.9   6.83  4.13
    ##  4 Premium   G     VS2      61      58  4022  6.2   6.13  3.76
    ##  5 Very Good D     VS2      59.8    57  1867  5.31  5.35  3.19
    ##  6 Ideal     D     VS2      62.5    55   781  4.41  4.45  2.77
    ##  7 Very Good F     SI1      58.6    57  1364  5.37  5.59  3.21
    ##  8 Premium   J     VS1      59.1    61  5107  7     6.94  4.12
    ##  9 Premium   G     VS1      62.2    58  5786  6.78  6.74  4.27
    ## 10 Premium   I     SI1      60.9    61 14488  8.32  8.16  4.99
    ## # ... with 5,384 more rows

    #diamonds %>% select(-carat) # 변수컬럼명으로 셀렉션
    #select(diamonds, -1) # 변수컬럼 위치로 셀렉션
    #diamonds %>% select(-1) # 변수컬럼 위치로 셀렉션

    #특정 변수 컬럼 2개이상 지정 인덱싱/ 셀렉션 하기
    select(diamonds, carat, price) # 변수컬럼명으로 셀렉션

    ## # A tibble: 5,394 x 2
    ##    carat price
    ##    <dbl> <int>
    ##  1 0.35    748
    ##  2 2.03  15811
    ##  3 1.17   2825
    ##  4 0.9    4022
    ##  5 0.55   1867
    ##  6 0.33    781
    ##  7 0.570  1364
    ##  8 1.2    5107
    ##  9 1.23   5786
    ## 10 2.06  14488
    ## # ... with 5,384 more rows

    #diamonds %>% select(carat, price) # 변수컬럼명으로 셀렉션
    #select(diamonds, 1, 7) # 변수컬럼 위치로 셀렉션
    #diamonds %>% select(1, 7) # 변수컬럼 위치로 셀렉션

    target <- c("carat", "price") # 임의의 변수컬럼명 벡터지정
    #select(diamonds, target) # 변수컬럼명 벡터를 이용한 셀렉션
    diamonds %>% select(target) # 변수컬럼명 벡터를 이용한 셀렉션

    ## # A tibble: 5,394 x 2
    ##    carat price
    ##    <dbl> <int>
    ##  1 0.35    748
    ##  2 2.03  15811
    ##  3 1.17   2825
    ##  4 0.9    4022
    ##  5 0.55   1867
    ##  6 0.33    781
    ##  7 0.570  1364
    ##  8 1.2    5107
    ##  9 1.23   5786
    ## 10 2.06  14488
    ## # ... with 5,384 more rows

    target2 <- c(1, 7) # 임의의 변수컬럼 위치 벡터지정
    #select(diamonds, target2) # 변수컬럼 위치 벡터를 이용한 셀렉션
    diamonds %>% select(target2) # 변수컬럼 위치 벡터를 이용한 셀렉션

    ## # A tibble: 5,394 x 2
    ##    carat price
    ##    <dbl> <int>
    ##  1 0.35    748
    ##  2 2.03  15811
    ##  3 1.17   2825
    ##  4 0.9    4022
    ##  5 0.55   1867
    ##  6 0.33    781
    ##  7 0.570  1364
    ##  8 1.2    5107
    ##  9 1.23   5786
    ## 10 2.06  14488
    ## # ... with 5,384 more rows

    #특정 변수컬럼 2개이상 제외 인덱싱/셀렉션
    select(diamonds, -carat, -price) # 변수컬럼명으로 셀렉션

    ## # A tibble: 5,394 x 8
    ##    cut       color clarity depth table     x     y     z
    ##    <ord>     <ord> <ord>   <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1 Very Good F     SI1      63.1    56  4.51  4.46  2.83
    ##  2 Very Good F     SI2      59.8    56  8.21  8.3   4.94
    ##  3 Premium   J     I1       60.2    61  6.9   6.83  4.13
    ##  4 Premium   G     VS2      61      58  6.2   6.13  3.76
    ##  5 Very Good D     VS2      59.8    57  5.31  5.35  3.19
    ##  6 Ideal     D     VS2      62.5    55  4.41  4.45  2.77
    ##  7 Very Good F     SI1      58.6    57  5.37  5.59  3.21
    ##  8 Premium   J     VS1      59.1    61  7     6.94  4.12
    ##  9 Premium   G     VS1      62.2    58  6.78  6.74  4.27
    ## 10 Premium   I     SI1      60.9    61  8.32  8.16  4.99
    ## # ... with 5,384 more rows

    #diamonds %>% select(-carat, -price) # 변수컬럼명으로 셀렉션
    #select(diamonds, -1, -7) # 변수컬럼 위치로 셀렉션
    #diamonds %>% select(-1, -7) # 변수컬럼 위치로 셀렉션

    target <- c("carat", "price") # 임의의 변수컬럼명 벡터지정
    #select(diamonds, -target) # 변수컬럼명 벡터를 이용한 셀렉션
    diamonds %>% select(-target) # 변수컬럼명 벡터를 이용한 셀렉션

    ## # A tibble: 5,394 x 8
    ##    cut       color clarity depth table     x     y     z
    ##    <ord>     <ord> <ord>   <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1 Very Good F     SI1      63.1    56  4.51  4.46  2.83
    ##  2 Very Good F     SI2      59.8    56  8.21  8.3   4.94
    ##  3 Premium   J     I1       60.2    61  6.9   6.83  4.13
    ##  4 Premium   G     VS2      61      58  6.2   6.13  3.76
    ##  5 Very Good D     VS2      59.8    57  5.31  5.35  3.19
    ##  6 Ideal     D     VS2      62.5    55  4.41  4.45  2.77
    ##  7 Very Good F     SI1      58.6    57  5.37  5.59  3.21
    ##  8 Premium   J     VS1      59.1    61  7     6.94  4.12
    ##  9 Premium   G     VS1      62.2    58  6.78  6.74  4.27
    ## 10 Premium   I     SI1      60.9    61  8.32  8.16  4.99
    ## # ... with 5,384 more rows

    target2 <- c(1, 7) # 임의의 변수컬럼 위치 벡터지정
    #select(diamonds, -target2) # 변수컬럼 위치 벡터를 이용한 셀렉션
    diamonds %>% select(-target2) # 변수컬럼 위치 벡터를 이용한 셀렉션

    ## # A tibble: 5,394 x 8
    ##    cut       color clarity depth table     x     y     z
    ##    <ord>     <ord> <ord>   <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1 Very Good F     SI1      63.1    56  4.51  4.46  2.83
    ##  2 Very Good F     SI2      59.8    56  8.21  8.3   4.94
    ##  3 Premium   J     I1       60.2    61  6.9   6.83  4.13
    ##  4 Premium   G     VS2      61      58  6.2   6.13  3.76
    ##  5 Very Good D     VS2      59.8    57  5.31  5.35  3.19
    ##  6 Ideal     D     VS2      62.5    55  4.41  4.45  2.77
    ##  7 Very Good F     SI1      58.6    57  5.37  5.59  3.21
    ##  8 Premium   J     VS1      59.1    61  7     6.94  4.12
    ##  9 Premium   G     VS1      62.2    58  6.78  6.74  4.27
    ## 10 Premium   I     SI1      60.9    61  8.32  8.16  4.99
    ## # ... with 5,384 more rows

    #연이어 붙어있는 변수컬럼 동시에 지정 인덱싱/셀렉션
    #select(diamonds, carat:clarity) 
    diamonds %>% select(carat:clarity)

    ## # A tibble: 5,394 x 4
    ##    carat cut       color clarity
    ##    <dbl> <ord>     <ord> <ord>  
    ##  1 0.35  Very Good F     SI1    
    ##  2 2.03  Very Good F     SI2    
    ##  3 1.17  Premium   J     I1     
    ##  4 0.9   Premium   G     VS2    
    ##  5 0.55  Very Good D     VS2    
    ##  6 0.33  Ideal     D     VS2    
    ##  7 0.570 Very Good F     SI1    
    ##  8 1.2   Premium   J     VS1    
    ##  9 1.23  Premium   G     VS1    
    ## 10 2.06  Premium   I     SI1    
    ## # ... with 5,384 more rows

    #select(diamonds, 1:4)
    diamonds %>% select(1:4)

    ## # A tibble: 5,394 x 4
    ##    carat cut       color clarity
    ##    <dbl> <ord>     <ord> <ord>  
    ##  1 0.35  Very Good F     SI1    
    ##  2 2.03  Very Good F     SI2    
    ##  3 1.17  Premium   J     I1     
    ##  4 0.9   Premium   G     VS2    
    ##  5 0.55  Very Good D     VS2    
    ##  6 0.33  Ideal     D     VS2    
    ##  7 0.570 Very Good F     SI1    
    ##  8 1.2   Premium   J     VS1    
    ##  9 1.23  Premium   G     VS1    
    ## 10 2.06  Premium   I     SI1    
    ## # ... with 5,384 more rows

    #연이어 붙어있는 변수컬럼 동시에 제외 인덱싱/셀렉션
    #select(diamonds, -(carat:clarity)) 
    diamonds %>% select(-(carat:clarity))

    ## # A tibble: 5,394 x 6
    ##    depth table price     x     y     z
    ##    <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  63.1    56   748  4.51  4.46  2.83
    ##  2  59.8    56 15811  8.21  8.3   4.94
    ##  3  60.2    61  2825  6.9   6.83  4.13
    ##  4  61      58  4022  6.2   6.13  3.76
    ##  5  59.8    57  1867  5.31  5.35  3.19
    ##  6  62.5    55   781  4.41  4.45  2.77
    ##  7  58.6    57  1364  5.37  5.59  3.21
    ##  8  59.1    61  5107  7     6.94  4.12
    ##  9  62.2    58  5786  6.78  6.74  4.27
    ## 10  60.9    61 14488  8.32  8.16  4.99
    ## # ... with 5,384 more rows

    #select(diamonds, -(1:4))
    diamonds %>% select(-(1:4))

    ## # A tibble: 5,394 x 6
    ##    depth table price     x     y     z
    ##    <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  63.1    56   748  4.51  4.46  2.83
    ##  2  59.8    56 15811  8.21  8.3   4.94
    ##  3  60.2    61  2825  6.9   6.83  4.13
    ##  4  61      58  4022  6.2   6.13  3.76
    ##  5  59.8    57  1867  5.31  5.35  3.19
    ##  6  62.5    55   781  4.41  4.45  2.77
    ##  7  58.6    57  1364  5.37  5.59  3.21
    ##  8  59.1    61  5107  7     6.94  4.12
    ##  9  62.2    58  5786  6.78  6.74  4.27
    ## 10  60.9    61 14488  8.32  8.16  4.99
    ## # ... with 5,384 more rows

    #변수컬럼명 시작부분 일치여부 이용 인덱싱/셀렉션
    #select(diamonds, starts_with("c"))
    diamonds %>% select(starts_with("c"))

    ## # A tibble: 5,394 x 4
    ##    carat cut       color clarity
    ##    <dbl> <ord>     <ord> <ord>  
    ##  1 0.35  Very Good F     SI1    
    ##  2 2.03  Very Good F     SI2    
    ##  3 1.17  Premium   J     I1     
    ##  4 0.9   Premium   G     VS2    
    ##  5 0.55  Very Good D     VS2    
    ##  6 0.33  Ideal     D     VS2    
    ##  7 0.570 Very Good F     SI1    
    ##  8 1.2   Premium   J     VS1    
    ##  9 1.23  Premium   G     VS1    
    ## 10 2.06  Premium   I     SI1    
    ## # ... with 5,384 more rows

    #c로 시작하는 모든 변수컬럼 조회

    #변수컬렴명 끝부분 일치여부 이용 인덱싱/셀렉션
    #select(diamonds, ends_with("e"))
    diamonds %>% select(ends_with("e"))

    ## # A tibble: 5,394 x 2
    ##    table price
    ##    <dbl> <int>
    ##  1    56   748
    ##  2    56 15811
    ##  3    61  2825
    ##  4    58  4022
    ##  5    57  1867
    ##  6    55   781
    ##  7    57  1364
    ##  8    61  5107
    ##  9    58  5786
    ## 10    61 14488
    ## # ... with 5,384 more rows

    #e로 끝나는 모든 변수컬럼 조회
