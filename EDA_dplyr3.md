Mutate()함수
------------

-   **ggplot2 패키지에 내장된 diamonds 데이터셋 활용**

<!-- -->

    #ggplot2라이브러리&데이터셋 로딩
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

    library(magrittr)
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

    #샘플링
    choice<-sample(1:NROW(diamonds),NROW(diamonds)*0.1,replace = FALSE)
    diamonds<-diamonds[choice,]

-   **새로운 변수컬럼을 추가(mutate)하기**

<!-- -->

    #1캐럿당 가격($)에 대한 별도 변수컬럼 추가
    diamonds<-mutate(diamonds,ppc=price/carat)
    diamonds

    ## # A tibble: 5,394 x 11
    ##    carat cut       color clarity depth table price     x     y     z   ppc
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl> <dbl>
    ##  1  0.72 Good      H     SI2      64.1    58  1799  5.68  5.65  3.63 2499.
    ##  2  1.22 Ideal     H     VS2      62.8    56  7584  6.81  6.75  4.26 6216.
    ##  3  0.33 Ideal     D     VS2      61.9    57  1002  4.47  4.42  2.75 3036.
    ##  4  1.7  Ideal     H     I1       62.6    54  7197  7.63  7.58  4.77 4234.
    ##  5  1.01 Ideal     I     SI1      62.4    56  4584  6.36  6.4   3.98 4539.
    ##  6  0.55 Ideal     G     VVS2     62.5    56  2022  5.22  5.25  3.27 3676.
    ##  7  1.3  Ideal     F     SI2      62.2    56  6442  6.96  7     4.34 4955.
    ##  8  0.31 Ideal     G     VVS1     61.2    56  1046  4.38  4.34  2.67 3374.
    ##  9  0.7  Very Good E     SI2      63.5    54  2310  5.69  5.64  3.6  3300 
    ## 10  0.9  Good      H     SI2      62.7    64  2934  6     6.09  3.79 3260 
    ## # ... with 5,384 more rows

    #diamonds%<>%mutate(ppc2=price/carat)

-   **연속변수 구간범위지정을 통한 변수 리코딩으로 새로운 변수컬럼
    추가**

<!-- -->

    #데이터셋 준비
    dd<-select(diamonds,carat,cut)
    dd

    ## # A tibble: 5,394 x 2
    ##    carat cut      
    ##    <dbl> <ord>    
    ##  1  0.72 Good     
    ##  2  1.22 Ideal    
    ##  3  0.33 Ideal    
    ##  4  1.7  Ideal    
    ##  5  1.01 Ideal    
    ##  6  0.55 Ideal    
    ##  7  1.3  Ideal    
    ##  8  0.31 Ideal    
    ##  9  0.7  Very Good
    ## 10  0.9  Good     
    ## # ... with 5,384 more rows

    #mutate()함수 이용 변수 리코딩
    #carat의 값이 1.0이하면 low로 리코딩
    #carat의 값이 1.0초과 3.0이하면 middle로 리코딩
    #carat의 값이 3.0초과이면 high로 리코딩
    dd%<>%mutate(cr.grd=cut(dd$carat,breaks = c(-Inf,1,3,Inf),include.lowest = FALSE,right = TRUE,labels = c("low","middle","high")))
    dd

    ## # A tibble: 5,394 x 3
    ##    carat cut       cr.grd
    ##    <dbl> <ord>     <fct> 
    ##  1  0.72 Good      low   
    ##  2  1.22 Ideal     middle
    ##  3  0.33 Ideal     low   
    ##  4  1.7  Ideal     middle
    ##  5  1.01 Ideal     middle
    ##  6  0.55 Ideal     low   
    ##  7  1.3  Ideal     middle
    ##  8  0.31 Ideal     low   
    ##  9  0.7  Very Good low   
    ## 10  0.9  Good      low   
    ## # ... with 5,384 more rows

-   **범주형변수 항목선택을 통한 변수리코딩으로 새로운 변수컬럼 추가**

<!-- -->

    ee <- select(diamonds, carat, cut)
    #ee데이터셋에 있는 cut 변수컬럼 범주항목들을 적당히 묶어서 변수리코딩
    #cut의 값이 Fair, Good, Very Good이면 Normal로 리코딩
    #cut의 값이 Premium,Ideal이면 Special로 리코딩

    #dplyr::mutate() +dplyr::recode()
    ee%<>%mutate(cut.grd=dplyr::recode(ee$cut,"Fair"="Normal",
                                       "Good"="Normal",
                                       "Very Good"="Normal",
                                       "Premium"="Special",
                                       "Ideal"="Special"))
    #dplyr::mutate()+dplyr::if_else()
    ee%<>%mutate(cut.grd2=
                   if_else(dd$cut%in%c("Fair","Good","Very Good"),"Normal","Special"))
    ee

    ## # A tibble: 5,394 x 4
    ##    carat cut       cut.grd cut.grd2
    ##    <dbl> <ord>     <ord>   <chr>   
    ##  1  0.72 Good      Normal  Normal  
    ##  2  1.22 Ideal     Special Special 
    ##  3  0.33 Ideal     Special Special 
    ##  4  1.7  Ideal     Special Special 
    ##  5  1.01 Ideal     Special Special 
    ##  6  0.55 Ideal     Special Special 
    ##  7  1.3  Ideal     Special Special 
    ##  8  0.31 Ideal     Special Special 
    ##  9  0.7  Very Good Normal  Normal  
    ## 10  0.9  Good      Normal  Normal  
    ## # ... with 5,384 more rows
