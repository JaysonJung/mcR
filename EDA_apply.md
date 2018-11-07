Apply() 함수 사용
-----------------

-   사용목적
-   특정 데이터셋에 대해 Row or Column 단위로 필요한 함수를 일괄 적용

<!-- -->

    #간단 데이터 생성
    m<-matrix(1:9,nrow=3)
    m

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9

    #행렬데이터에 대한 Row & Column 방향 연산
    rowSums(m)

    ## [1] 12 15 18

    colSums(m)

    ## [1]  6 15 24

-**apply() 함수를 이용한 row&column 방향 연산**

    apply(m,1,sum)#row(1)방향으로 sum()함수 적용

    ## [1] 12 15 18

    apply(m,2,sum)#column(2)방향으로 sum()함수 적용

    ## [1]  6 15 24

-**학생 명부 데이터 프레임을 통한 예**

    student <- c("John Davis","Angela Williams","Bullwinkle Moose",
                 "David Jones","Janice Markhammer", "Cheryl Cushing",
                 "Reuven Ytzrhak", "Greg Knox","Joel England","Mary Rayburn")
    math <- c(502, 600, 412, 358, 495, 512, 410, 625, 573, 522)
    science <- c(95, 99, 80, 82, 75, 85, 80, 95, 89, 86)
    english <- c(25, 22, 18, 15, 20, 28, 15, 30, 27, 18)
    #data.frame 형태로 가공
    roster.df <- data.frame(student, math, science, english, 
                            stringsAsFactors=FALSE)
    roster.df

    ##              student math science english
    ## 1         John Davis  502      95      25
    ## 2    Angela Williams  600      99      22
    ## 3   Bullwinkle Moose  412      80      18
    ## 4        David Jones  358      82      15
    ## 5  Janice Markhammer  495      75      20
    ## 6     Cheryl Cushing  512      85      28
    ## 7     Reuven Ytzrhak  410      80      15
    ## 8          Greg Knox  625      95      30
    ## 9       Joel England  573      89      27
    ## 10      Mary Rayburn  522      86      18

    str(roster.df)

    ## 'data.frame':    10 obs. of  4 variables:
    ##  $ student: chr  "John Davis" "Angela Williams" "Bullwinkle Moose" "David Jones" ...
    ##  $ math   : num  502 600 412 358 495 512 410 625 573 522
    ##  $ science: num  95 99 80 82 75 85 80 95 89 86
    ##  $ english: num  25 22 18 15 20 28 15 30 27 18

    roster<-roster.df
    #Apply()함수 적용
    apply(roster[2:4], 1, mean)

    ##  [1] 207.3333 240.3333 170.0000 151.6667 196.6667 208.3333 168.3333
    ##  [8] 250.0000 229.6667 208.6667

    apply(roster[2:4], 2, mean)

    ##    math science english 
    ##   500.9    86.6    21.8

    #Scailing 안되서 보기 불편하다

-**After Scailing the roster data, get the mean scores**

    library(magrittr)
    roster$math100 <- round((roster$math * 100) / 700, 1)
    roster$eng100 <- ((roster$english * 100) / 30) %>% signif(1)
    #head(roster) 
    #스케일링된 데이터에 apply()함수 적용
    #개별학생들의 평균성적과 표준편차 도출
    roster$st.avg <- apply(roster[c(3, 5, 6)], 1, mean) %>% round(1) 
    roster$st.sd <- apply(roster[c(3, 5, 6)], 1, sd) %>% round(1)
    #roster
    roster[-c(2, 4)]

    ##              student science math100 eng100 st.avg st.sd
    ## 1         John Davis      95    71.7     80   82.2  11.8
    ## 2    Angela Williams      99    85.7     70   84.9  14.5
    ## 3   Bullwinkle Moose      80    58.9     60   66.3  11.9
    ## 4        David Jones      82    51.1     50   61.0  18.2
    ## 5  Janice Markhammer      75    70.7     70   71.9   2.7
    ## 6     Cheryl Cushing      85    73.1     90   82.7   8.7
    ## 7     Reuven Ytzrhak      80    58.6     50   62.9  15.4
    ## 8          Greg Knox      95    89.3    100   94.8   5.4
    ## 9       Joel England      89    81.9     90   87.0   4.4
    ## 10      Mary Rayburn      86    74.6     60   73.5  13.0

    #정렬기능(arrange) 사용을 위한 패키지 로딩
    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    arrange(roster, desc(st.avg), desc(st.sd))

    ##              student math science english math100 eng100 st.avg st.sd
    ## 1          Greg Knox  625      95      30    89.3    100   94.8   5.4
    ## 2       Joel England  573      89      27    81.9     90   87.0   4.4
    ## 3    Angela Williams  600      99      22    85.7     70   84.9  14.5
    ## 4     Cheryl Cushing  512      85      28    73.1     90   82.7   8.7
    ## 5         John Davis  502      95      25    71.7     80   82.2  11.8
    ## 6       Mary Rayburn  522      86      18    74.6     60   73.5  13.0
    ## 7  Janice Markhammer  495      75      20    70.7     70   71.9   2.7
    ## 8   Bullwinkle Moose  412      80      18    58.9     60   66.3  11.9
    ## 9     Reuven Ytzrhak  410      80      15    58.6     50   62.9  15.4
    ## 10       David Jones  358      82      15    51.1     50   61.0  18.2

    roster %>% arrange(desc(st.avg), desc(st.sd))

    ##              student math science english math100 eng100 st.avg st.sd
    ## 1          Greg Knox  625      95      30    89.3    100   94.8   5.4
    ## 2       Joel England  573      89      27    81.9     90   87.0   4.4
    ## 3    Angela Williams  600      99      22    85.7     70   84.9  14.5
    ## 4     Cheryl Cushing  512      85      28    73.1     90   82.7   8.7
    ## 5         John Davis  502      95      25    71.7     80   82.2  11.8
    ## 6       Mary Rayburn  522      86      18    74.6     60   73.5  13.0
    ## 7  Janice Markhammer  495      75      20    70.7     70   71.9   2.7
    ## 8   Bullwinkle Moose  412      80      18    58.9     60   66.3  11.9
    ## 9     Reuven Ytzrhak  410      80      15    58.6     50   62.9  15.4
    ## 10       David Jones  358      82      15    51.1     50   61.0  18.2
