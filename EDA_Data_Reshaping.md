EDA\_데이터 재구조화\_데이터셋간 결합
-------------------------------------

-   **rbind(), cbind()를 이용한 데이터 프레임간 결합**

<!-- -->

    #rbind()를 이용한 결합
    #데이터프레임간 행기준으로 결합
    #결합되는 데이터프레임 간의 변수컬럼 갯수/이름/특성 일치가 중요
    A <- data.frame(id=c("c01","c02","c03","c04"), 
                    last=c("Kim", "Lee", "Choi", "Park"),
                    gender = c("F", "M", "F", "M"), 
                    stringsAsFactors = FALSE)
    #A id,last,gender,n=4
    B <- data.frame(id = c("c05", "c06", "c07"), 
                    last = c("Bae", "Kim", "Lim"),
                    gender = c("F", "F", "M"), 
                    stringsAsFactors = FALSE)
    #B id,last,gender,n=3
    C <- data.frame(id = c("c08", "c09"), 
                    last = c("Lee", "Park"), 
                    stringsAsFactors = FALSE)
    #C id,last,n=2
    D <- data.frame(id = c("c10", "c11", "c12"), 
                    last = c("Bang", "Kang", "Rim"),
                    gender = c("M", "F", "M"),
                    job = c(4, 3, 1), 
                    stringsAsFactors = FALSE)
    #D id,last,gender,job,n=3
    E <- data.frame(id = c("c13", "c14", "c15"), 
                    first = c("Sangmin", "Hyori", "Daniel"),
                    gender = c("M", "M", "F"), 
                    stringsAsFactors = FALSE)
    #E id,first,gender,n=3

    rbind(A,B)

    ##    id last gender
    ## 1 c01  Kim      F
    ## 2 c02  Lee      M
    ## 3 c03 Choi      F
    ## 4 c04 Park      M
    ## 5 c05  Bae      F
    ## 6 c06  Kim      F
    ## 7 c07  Lim      M

    rbind(B,A)

    ##    id last gender
    ## 1 c05  Bae      F
    ## 2 c06  Kim      F
    ## 3 c07  Lim      M
    ## 4 c01  Kim      F
    ## 5 c02  Lee      M
    ## 6 c03 Choi      F
    ## 7 c04 Park      M

    #Error
    #rbind(A,C) id, last, gender <=에러발생=> id, last
    #rbind(A,D) id, last, gender <=에러발생=> id, last, gender, job
    #rbind(A,E) id, last, gender <=에러발생=> id, first, gender

    #cbind()를 이용한 결합
    #데이터프레임 간 열을 기준으로 결합
    #결합되는 데이터 프레임 간의 행 갯수 일치가 중요
    #주로 왼쪽에는 데이터프레임 식별자 프로필 데이터, 오른쪽에는 식별자의 활동정보로 결합
    P <- data.frame(age = c(20, 25, 19, 40), 
                    income = c(2500, 2700, 0, 7000), 
                    stringsAsFactors = FALSE)
    #P age,income,n=4
    Q <- data.frame(age = c(20, 25, 19, 40, 32, 39, 28), 
                    income = c(2500, 2700, 0, 7000, 3400, 3600, 2900), 
                    stringsAsFactors = FALSE)
    #Q age,income,n=7
    R <- data.frame(id=c("c01","c02","c03","c04"),
                    age = c(20, 25, 19, 40), 
                    income = c(2500, 2700, 0, 7000), 
                    stringsAsFactors = FALSE)
    #R age,income,gender n=4
    S <- data.frame(id=c("c03","c04","c07","c08"),
                    age = c(19, 40, 29, 30), 
                    income = c(1500, 3400, 3020, 4500), 
                    stringsAsFactors = FALSE)
    #S id,age,income,n=4
    cbind(A,P)

    ##    id last gender age income
    ## 1 c01  Kim      F  20   2500
    ## 2 c02  Lee      M  25   2700
    ## 3 c03 Choi      F  19      0
    ## 4 c04 Park      M  40   7000

    cbind(P,A)

    ##   age income  id last gender
    ## 1  20   2500 c01  Kim      F
    ## 2  25   2700 c02  Lee      M
    ## 3  19      0 c03 Choi      F
    ## 4  40   7000 c04 Park      M

    cbind(A,R)

    ##    id last gender  id age income
    ## 1 c01  Kim      F c01  20   2500
    ## 2 c02  Lee      M c02  25   2700
    ## 3 c03 Choi      F c03  19      0
    ## 4 c04 Park      M c04  40   7000

    #Error
    #cbind(A,Q)
    #cbind(A,S) 물리적 정상결합, 논리적 오류

-   **dplyr::join함수를 이용한 데이터프레임간 결합**

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

    #내부조인(Inner join)
    #양쪽 데이터셋에 같이 들어 있는 레코드를 기준으로 결합
    A%>%inner_join(S,by='id')

    ##    id last gender age income
    ## 1 c03 Choi      F  19   1500
    ## 2 c04 Park      M  40   3400

    #양쪽에 같이 있는 사람들간의 결합
    #주로 그룹간 분석시 용이

    #왼쪽 외부조인(Outer Join)
    #왼쪽 데이터셋을 기준자료로 삼아 해당 레코드를 모두 사용
    #오른쪽 데이터셋은 왼쪽 데이터셋과 일치하는 레코드만 결합
    A%>%left_join(S,by='id')

    ##    id last gender age income
    ## 1 c01  Kim      F  NA     NA
    ## 2 c02  Lee      M  NA     NA
    ## 3 c03 Choi      F  19   1500
    ## 4 c04 Park      M  40   3400

    #오른쪽 외부조인
    A%>%right_join(S,by='id')

    ##    id last gender age income
    ## 1 c03 Choi      F  19   1500
    ## 2 c04 Park      M  40   3400
    ## 3 c07 <NA>   <NA>  29   3020
    ## 4 c08 <NA>   <NA>  30   4500

    #세미조인
    A%>%semi_join(S,by='id')

    ##    id last gender
    ## 1 c03 Choi      F
    ## 2 c04 Park      M

    #내부조인처럼 양쪽에서 id가 일치하는 레코드를 찾는것은 동일하지만 변수컬럼은 A데이터셋에 존재하는 변수컬럼만 사용

    #안티조인
    A%>%anti_join(S,by='id')

    ##    id last gender
    ## 1 c01  Kim      F
    ## 2 c02  Lee      M

    #내보조인과 반대로 A용와 S 데이터셋을 비교해 id가 일치하지 않는 레코드만을 찾으며 A데이터셋에 존재하는 변수컬럼만 사요
