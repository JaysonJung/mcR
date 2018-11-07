사용자 정의함수
---------------

### 인수설정 & 미정의 인수 설정

    hello.person<-function(first,last="Doe",...){
      print(sprintf("hello,%s %s!",first,last))
    }

    hello.person("AVB","lander",extra="Good bye")

    ## [1] "hello,AVB lander!"

    hello.person("BDC","lander","Good morning")

    ## [1] "hello,BDC lander!"

### 자동화 사용

    # do.call함수의 이용
    do.call(hello.person,args=list(first="AAA",last="BBB"))

    ## [1] "hello,AAA BBB!"

    # 학생 명부 데이터 생성
    f.name <- c("John", "Ang", "Bull", "David", "Janice", 
                "Cheryl", "Reuven", "Greg", "Joel", "Mary")
    l.name <- c("Da", "ela", "winkle", "Jones", "Markhammer",
                "Cushing", "Ytzrhak", "Knox", "England", "Rayburn")
    math <- c(502, 600, 412, 358, 495, 512, 410, 625, 573, 522)
    science <- c(95, 99, 80, 82, 75, 85, 80, 95, 89, 86)
    english <- c(25, 22, 18, 15, 20, 28, 15, 30, 27, 18)
    roster <- data.frame(f.name, l.name, math, science, english, 
                         stringsAsFactors=FALSE)
    roster

    ##    f.name     l.name math science english
    ## 1    John         Da  502      95      25
    ## 2     Ang        ela  600      99      22
    ## 3    Bull     winkle  412      80      18
    ## 4   David      Jones  358      82      15
    ## 5  Janice Markhammer  495      75      20
    ## 6  Cheryl    Cushing  512      85      28
    ## 7  Reuven    Ytzrhak  410      80      15
    ## 8    Greg       Knox  625      95      30
    ## 9    Joel    England  573      89      27
    ## 10   Mary    Rayburn  522      86      18

    # do.call이용한 자동반복 시행
    do.call(hello.person, 
            args = list(first = roster$f.name, 
                        last = roster$l.name))

    ##  [1] "hello,John Da!"           "hello,Ang ela!"          
    ##  [3] "hello,Bull winkle!"       "hello,David Jones!"      
    ##  [5] "hello,Janice Markhammer!" "hello,Cheryl Cushing!"   
    ##  [7] "hello,Reuven Ytzrhak!"    "hello,Greg Knox!"        
    ##  [9] "hello,Joel England!"      "hello,Mary Rayburn!"

    # 일련의 데이터와 원하는 R함수를 인수값으로 입력하면 계산이 되도록 
    calc <- function(x, func = mean) {
      do.call(func, args = list(x))
    }

    calc(1:10)

    ## [1] 5.5

    calc(1:10, mean)

    ## [1] 5.5

    calc(1:10, sum)

    ## [1] 55

    calc(1:10, sd)

    ## [1] 3.02765
