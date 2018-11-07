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

### 데이터 구조파악 및 기술통계

-   **특정 변수컬럼 지정/제외 인덱싱/셀렉션**

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
    ## $ carat   <dbl> 1.30, 1.01, 0.30, 0.35, 0.30, 1.27, 1.24, 0.38, 0.52, ...
    ## $ cut     <ord> Premium, Ideal, Ideal, Ideal, Ideal, Premium, Premium,...
    ## $ color   <ord> G, E, D, F, H, J, J, E, G, D, F, E, D, E, D, G, E, E, ...
    ## $ clarity <ord> SI2, SI2, SI1, VS2, SI1, VVS1, VS2, VS2, SI1, SI1, SI2...
    ## $ depth   <dbl> 61.8, 62.0, 61.6, 61.5, 61.9, 60.1, 61.4, 60.4, 62.8, ...
    ## $ table   <dbl> 57, 56, 57, 57, 54, 58, 59, 58, 61, 56, 56, 57, 55, 64...
    ## $ price   <int> 5421, 4694, 552, 706, 389, 5761, 5026, 744, 1244, 571,...
    ## $ x       <dbl> 7.02, 6.46, 4.28, 4.52, 4.33, 7.06, 6.91, 4.67, 5.11, ...
    ## $ y       <dbl> 6.94, 6.42, 4.32, 4.56, 4.36, 6.99, 6.83, 4.70, 5.14, ...
    ## $ z       <dbl> 4.32, 3.99, 2.65, 2.79, 2.69, 4.22, 4.22, 2.83, 3.22, ...

    #특정 변수 컬럼 인덱싱/셀렉션 하기
    select(diamonds,carat)#case 1

    ## # A tibble: 5,394 x 1
    ##    carat
    ##    <dbl>
    ##  1  1.3 
    ##  2  1.01
    ##  3  0.3 
    ##  4  0.35
    ##  5  0.3 
    ##  6  1.27
    ##  7  1.24
    ##  8  0.38
    ##  9  0.52
    ## 10  0.31
    ## # ... with 5,384 more rows

    #diamonds%>%select(carat)#case 2
    #select(diamonds,1)#case 3
    #diamonds%>%select(1)#case 4

    #특정 변수 컬럼 제외 인덱싱/셀렉션 하기
    select(diamonds, -carat) # 변수컬럼명으로 셀렉션

    ## # A tibble: 5,394 x 9
    ##    cut       color clarity depth table price     x     y     z
    ##    <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 Premium   G     SI2      61.8    57  5421  7.02  6.94  4.32
    ##  2 Ideal     E     SI2      62      56  4694  6.46  6.42  3.99
    ##  3 Ideal     D     SI1      61.6    57   552  4.28  4.32  2.65
    ##  4 Ideal     F     VS2      61.5    57   706  4.52  4.56  2.79
    ##  5 Ideal     H     SI1      61.9    54   389  4.33  4.36  2.69
    ##  6 Premium   J     VVS1     60.1    58  5761  7.06  6.99  4.22
    ##  7 Premium   J     VS2      61.4    59  5026  6.91  6.83  4.22
    ##  8 Very Good E     VS2      60.4    58   744  4.67  4.7   2.83
    ##  9 Very Good G     SI1      62.8    61  1244  5.11  5.14  3.22
    ## 10 Ideal     D     SI1      62.7    56   571  4.32  4.36  2.72
    ## # ... with 5,384 more rows

    #diamonds %>% select(-carat) # 변수컬럼명으로 셀렉션
    #select(diamonds, -1) # 변수컬럼 위치로 셀렉션
    #diamonds %>% select(-1) # 변수컬럼 위치로 셀렉션

-   **특정 변수컬럼 2개이상 지정/제외 인덱싱/셀렉션**

<!-- -->

    #특정 변수 컬럼 2개이상 지정 인덱싱/ 셀렉션 하기
    select(diamonds, carat, price) # 변수컬럼명으로 셀렉션

    ## # A tibble: 5,394 x 2
    ##    carat price
    ##    <dbl> <int>
    ##  1  1.3   5421
    ##  2  1.01  4694
    ##  3  0.3    552
    ##  4  0.35   706
    ##  5  0.3    389
    ##  6  1.27  5761
    ##  7  1.24  5026
    ##  8  0.38   744
    ##  9  0.52  1244
    ## 10  0.31   571
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
    ##  1  1.3   5421
    ##  2  1.01  4694
    ##  3  0.3    552
    ##  4  0.35   706
    ##  5  0.3    389
    ##  6  1.27  5761
    ##  7  1.24  5026
    ##  8  0.38   744
    ##  9  0.52  1244
    ## 10  0.31   571
    ## # ... with 5,384 more rows

    target2 <- c(1, 7) # 임의의 변수컬럼 위치 벡터지정
    #select(diamonds, target2) # 변수컬럼 위치 벡터를 이용한 셀렉션
    diamonds %>% select(target2) # 변수컬럼 위치 벡터를 이용한 셀렉션

    ## # A tibble: 5,394 x 2
    ##    carat price
    ##    <dbl> <int>
    ##  1  1.3   5421
    ##  2  1.01  4694
    ##  3  0.3    552
    ##  4  0.35   706
    ##  5  0.3    389
    ##  6  1.27  5761
    ##  7  1.24  5026
    ##  8  0.38   744
    ##  9  0.52  1244
    ## 10  0.31   571
    ## # ... with 5,384 more rows

    #특정 변수컬럼 2개이상 제외 인덱싱/셀렉션
    select(diamonds, -carat, -price) # 변수컬럼명으로 셀렉션

    ## # A tibble: 5,394 x 8
    ##    cut       color clarity depth table     x     y     z
    ##    <ord>     <ord> <ord>   <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1 Premium   G     SI2      61.8    57  7.02  6.94  4.32
    ##  2 Ideal     E     SI2      62      56  6.46  6.42  3.99
    ##  3 Ideal     D     SI1      61.6    57  4.28  4.32  2.65
    ##  4 Ideal     F     VS2      61.5    57  4.52  4.56  2.79
    ##  5 Ideal     H     SI1      61.9    54  4.33  4.36  2.69
    ##  6 Premium   J     VVS1     60.1    58  7.06  6.99  4.22
    ##  7 Premium   J     VS2      61.4    59  6.91  6.83  4.22
    ##  8 Very Good E     VS2      60.4    58  4.67  4.7   2.83
    ##  9 Very Good G     SI1      62.8    61  5.11  5.14  3.22
    ## 10 Ideal     D     SI1      62.7    56  4.32  4.36  2.72
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
    ##  1 Premium   G     SI2      61.8    57  7.02  6.94  4.32
    ##  2 Ideal     E     SI2      62      56  6.46  6.42  3.99
    ##  3 Ideal     D     SI1      61.6    57  4.28  4.32  2.65
    ##  4 Ideal     F     VS2      61.5    57  4.52  4.56  2.79
    ##  5 Ideal     H     SI1      61.9    54  4.33  4.36  2.69
    ##  6 Premium   J     VVS1     60.1    58  7.06  6.99  4.22
    ##  7 Premium   J     VS2      61.4    59  6.91  6.83  4.22
    ##  8 Very Good E     VS2      60.4    58  4.67  4.7   2.83
    ##  9 Very Good G     SI1      62.8    61  5.11  5.14  3.22
    ## 10 Ideal     D     SI1      62.7    56  4.32  4.36  2.72
    ## # ... with 5,384 more rows

    target2 <- c(1, 7) # 임의의 변수컬럼 위치 벡터지정
    #select(diamonds, -target2) # 변수컬럼 위치 벡터를 이용한 셀렉션
    diamonds %>% select(-target2) # 변수컬럼 위치 벡터를 이용한 셀렉션

    ## # A tibble: 5,394 x 8
    ##    cut       color clarity depth table     x     y     z
    ##    <ord>     <ord> <ord>   <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1 Premium   G     SI2      61.8    57  7.02  6.94  4.32
    ##  2 Ideal     E     SI2      62      56  6.46  6.42  3.99
    ##  3 Ideal     D     SI1      61.6    57  4.28  4.32  2.65
    ##  4 Ideal     F     VS2      61.5    57  4.52  4.56  2.79
    ##  5 Ideal     H     SI1      61.9    54  4.33  4.36  2.69
    ##  6 Premium   J     VVS1     60.1    58  7.06  6.99  4.22
    ##  7 Premium   J     VS2      61.4    59  6.91  6.83  4.22
    ##  8 Very Good E     VS2      60.4    58  4.67  4.7   2.83
    ##  9 Very Good G     SI1      62.8    61  5.11  5.14  3.22
    ## 10 Ideal     D     SI1      62.7    56  4.32  4.36  2.72
    ## # ... with 5,384 more rows

-   **연이어 붙은 변수컬럼 지정/제외 인덱싱/셀렉션**

<!-- -->

    #연이어 붙어있는 변수컬럼 동시에 지정 인덱싱/셀렉션
    #select(diamonds, carat:clarity) 
    diamonds %>% select(carat:clarity)

    ## # A tibble: 5,394 x 4
    ##    carat cut       color clarity
    ##    <dbl> <ord>     <ord> <ord>  
    ##  1  1.3  Premium   G     SI2    
    ##  2  1.01 Ideal     E     SI2    
    ##  3  0.3  Ideal     D     SI1    
    ##  4  0.35 Ideal     F     VS2    
    ##  5  0.3  Ideal     H     SI1    
    ##  6  1.27 Premium   J     VVS1   
    ##  7  1.24 Premium   J     VS2    
    ##  8  0.38 Very Good E     VS2    
    ##  9  0.52 Very Good G     SI1    
    ## 10  0.31 Ideal     D     SI1    
    ## # ... with 5,384 more rows

    #select(diamonds, 1:4)
    diamonds %>% select(1:4)

    ## # A tibble: 5,394 x 4
    ##    carat cut       color clarity
    ##    <dbl> <ord>     <ord> <ord>  
    ##  1  1.3  Premium   G     SI2    
    ##  2  1.01 Ideal     E     SI2    
    ##  3  0.3  Ideal     D     SI1    
    ##  4  0.35 Ideal     F     VS2    
    ##  5  0.3  Ideal     H     SI1    
    ##  6  1.27 Premium   J     VVS1   
    ##  7  1.24 Premium   J     VS2    
    ##  8  0.38 Very Good E     VS2    
    ##  9  0.52 Very Good G     SI1    
    ## 10  0.31 Ideal     D     SI1    
    ## # ... with 5,384 more rows

    #연이어 붙어있는 변수컬럼 동시에 제외 인덱싱/셀렉션
    #select(diamonds, -(carat:clarity)) 
    diamonds %>% select(-(carat:clarity))

    ## # A tibble: 5,394 x 6
    ##    depth table price     x     y     z
    ##    <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  61.8    57  5421  7.02  6.94  4.32
    ##  2  62      56  4694  6.46  6.42  3.99
    ##  3  61.6    57   552  4.28  4.32  2.65
    ##  4  61.5    57   706  4.52  4.56  2.79
    ##  5  61.9    54   389  4.33  4.36  2.69
    ##  6  60.1    58  5761  7.06  6.99  4.22
    ##  7  61.4    59  5026  6.91  6.83  4.22
    ##  8  60.4    58   744  4.67  4.7   2.83
    ##  9  62.8    61  1244  5.11  5.14  3.22
    ## 10  62.7    56   571  4.32  4.36  2.72
    ## # ... with 5,384 more rows

    #select(diamonds, -(1:4))
    diamonds %>% select(-(1:4))

    ## # A tibble: 5,394 x 6
    ##    depth table price     x     y     z
    ##    <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  61.8    57  5421  7.02  6.94  4.32
    ##  2  62      56  4694  6.46  6.42  3.99
    ##  3  61.6    57   552  4.28  4.32  2.65
    ##  4  61.5    57   706  4.52  4.56  2.79
    ##  5  61.9    54   389  4.33  4.36  2.69
    ##  6  60.1    58  5761  7.06  6.99  4.22
    ##  7  61.4    59  5026  6.91  6.83  4.22
    ##  8  60.4    58   744  4.67  4.7   2.83
    ##  9  62.8    61  1244  5.11  5.14  3.22
    ## 10  62.7    56   571  4.32  4.36  2.72
    ## # ... with 5,384 more rows

-   **변수컬럼명 시작/끝 일치여부 인덱싱/셀렉션**

<!-- -->

    #변수컬럼명 시작부분 일치여부 이용 인덱싱/셀렉션
    #select(diamonds, starts_with("c"))
    diamonds %>% select(starts_with("c"))

    ## # A tibble: 5,394 x 4
    ##    carat cut       color clarity
    ##    <dbl> <ord>     <ord> <ord>  
    ##  1  1.3  Premium   G     SI2    
    ##  2  1.01 Ideal     E     SI2    
    ##  3  0.3  Ideal     D     SI1    
    ##  4  0.35 Ideal     F     VS2    
    ##  5  0.3  Ideal     H     SI1    
    ##  6  1.27 Premium   J     VVS1   
    ##  7  1.24 Premium   J     VS2    
    ##  8  0.38 Very Good E     VS2    
    ##  9  0.52 Very Good G     SI1    
    ## 10  0.31 Ideal     D     SI1    
    ## # ... with 5,384 more rows

    #c로 시작하는 모든 변수컬럼 조회

    #변수컬렴명 끝부분 일치여부 이용 인덱싱/셀렉션
    #select(diamonds, ends_with("e"))
    diamonds %>% select(ends_with("e"))

    ## # A tibble: 5,394 x 2
    ##    table price
    ##    <dbl> <int>
    ##  1    57  5421
    ##  2    56  4694
    ##  3    57   552
    ##  4    57   706
    ##  5    54   389
    ##  6    58  5761
    ##  7    59  5026
    ##  8    58   744
    ##  9    61  1244
    ## 10    56   571
    ## # ... with 5,384 more rows

    #e로 끝나는 모든 변수컬럼 조회
