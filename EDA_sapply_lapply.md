sapply()&lapply()함수 사용예
----------------------------

-   **사용목적**
-   특정 데이터셋에 들어있는 각 요소항목별로 필요한 함수를 일괄 적용

<!-- -->

    #데이터 생성
    ex <- list(A = matrix(1:12, 3), B = 1:5, 
               C = matrix(1:4, 2), D = rep(c(2, 4), 3))
    ex

    ## $A
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    4    7   10
    ## [2,]    2    5    8   11
    ## [3,]    3    6    9   12
    ## 
    ## $B
    ## [1] 1 2 3 4 5
    ## 
    ## $C
    ##      [,1] [,2]
    ## [1,]    1    3
    ## [2,]    2    4
    ## 
    ## $D
    ## [1] 2 4 2 4 2 4

-   **sapply()와 lapply()를 이용한 리스트항목별 공통작업 적용**

<!-- -->

    #ex에 들어있는 각 리스트별 sum()함수 적용하고 결과를 벡터객체로 만들어냄
    sapply(ex,sum)

    ##  A  B  C  D 
    ## 78 15 10 18

    #ex에 들어있는 각 리스트별 sum()함수 적용하고 결과를 리스트객체로 만들어냄
    lapply(ex,sum)

    ## $A
    ## [1] 78
    ## 
    ## $B
    ## [1] 15
    ## 
    ## $C
    ## [1] 10
    ## 
    ## $D
    ## [1] 18

-   **학생명부 데이터셋 적용**

<!-- -->

    library(tibble)
    #데이터셋 생성
    student <- c("John Davis","Angela Williams","Bullwinkle Moose",
                 "David Jones","Janice Markhammer", "Cheryl Cushing",
                 "Reuven Ytzrhak", "Greg Knox","Joel England","Mary Rayburn")
    math <- c(72, 86, 59, 51, 71, 73, 59, 89, 82, 75)
    science <- c(95, 99, 80, 82, 75, 85, 80, 95, 89, 86)
    english <- c(80, 70, 60, 50, 70, 90, 50, 100, 90, 60)
    roster<-tibble(student,math,science,english)
    roster

    ## # A tibble: 10 x 4
    ##    student            math science english
    ##    <chr>             <dbl>   <dbl>   <dbl>
    ##  1 John Davis           72      95      80
    ##  2 Angela Williams      86      99      70
    ##  3 Bullwinkle Moose     59      80      60
    ##  4 David Jones          51      82      50
    ##  5 Janice Markhammer    71      75      70
    ##  6 Cheryl Cushing       73      85      90
    ##  7 Reuven Ytzrhak       59      80      50
    ##  8 Greg Knox            89      95     100
    ##  9 Joel England         82      89      90
    ## 10 Mary Rayburn         75      86      60

-   **학생이름 추출후 성과 이름 부분 추출**

<!-- -->

    #학생 성명을 성과 이름으로 분리
    out<-strsplit(roster$student,split = " ")
    out

    ## [[1]]
    ## [1] "John"  "Davis"
    ## 
    ## [[2]]
    ## [1] "Angela"   "Williams"
    ## 
    ## [[3]]
    ## [1] "Bullwinkle" "Moose"     
    ## 
    ## [[4]]
    ## [1] "David" "Jones"
    ## 
    ## [[5]]
    ## [1] "Janice"     "Markhammer"
    ## 
    ## [[6]]
    ## [1] "Cheryl"  "Cushing"
    ## 
    ## [[7]]
    ## [1] "Reuven"  "Ytzrhak"
    ## 
    ## [[8]]
    ## [1] "Greg" "Knox"
    ## 
    ## [[9]]
    ## [1] "Joel"    "England"
    ## 
    ## [[10]]
    ## [1] "Mary"    "Rayburn"

    first.name<-sapply(out,"[",1)
    first.name

    ##  [1] "John"       "Angela"     "Bullwinkle" "David"      "Janice"    
    ##  [6] "Cheryl"     "Reuven"     "Greg"       "Joel"       "Mary"

    last.name<-sapply(out,"[",2)
    last.name

    ##  [1] "Davis"      "Williams"   "Moose"      "Jones"      "Markhammer"
    ##  [6] "Cushing"    "Ytzrhak"    "Knox"       "England"    "Rayburn"
