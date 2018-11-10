`{r setup, include=FALSE} knitr::opts_chunk$set(echo = TRUE)`

EDA\_dplyr 패키지\_요약집계\_기본관계식설정방식
-----------------------------------------------

-   **요약집계 분석개념**
-   요약집계 기준변수에 따라 요약집계 결과변수의 특성치를 비교분석하는
    방법
-   요약집계 기준변수는 범주형 변수로 여러개의 그룹/집단으로 구성
-   요약집계 결과변수는 연속형 변수로 다양한 기술통계함수를 적용해
    특성차이를 비교
-   인수 설정방식
    -   요약집계 함수에 기준변수와 결과변수를 일반적인 기능함수의
        인수/인자/파라미터 설정방식으로 입력
    -   stats::aggregate(), base::tapply()함수가 대표적
-   관계식 설정방식
    -   요약집계 함수에 기준변수와 결과변수간 관계를 일종의
        관계식(formula)형식으로 설정
    -   관계식 방식을 사용하면 인수설정방식에 비해서 변수간의 관계를
        직접적으로 묘사해 직관적 이해 가능
    -   stats::aggregate(), doBy::summaryBy()함수가 대표적
-   **데이터셋 로딩**

        library(ggplot2)
        library(doBy)
        data(mpg)
        head(mpg)

-   **집계기준 대상변수들 팩터화를 통해 레이블 작업**

<!-- -->

    #cyl 변수 팩터화
    mpg$cyl.f<-factor(mpg$cyl,levels=c(4,5,6,8),
                      labels = c("4기통","5기통","6기통","8기통"),
                      ordered = TRUE)
    #fl 변수 팩터화
    mpg$fl.f<-factor(mpg$fl,levels = c("c","d","e","p","r"),
                     labels = c("압축천연가스","디젤","에탄올","프리미엄","일반"),ordered=TRUE)
    #drv 변수 팩터화
    mpg$drv.f <- factor(mpg$drv, levels = c("4", "f", "r"),
                        labels = c("4륜구동", "전륜구동", "후륜구동"),
                        ordered = TRUE)
    # class 변수 팩터화
    mpg$class.f <- factor(mpg$class, 
                          levels = c("2seater", "subcompact", "compact", 
                                     "midsize", "suv", "minivan", "pickup"),
                          labels = c("2인승", "경차", "소형차",
                                     "중형차", "SUV", "미니밴", "픽업트럭"),
                          ordered = TRUE)

    # trans 변수 범주화 & 팩터화 
    mpg$trans.f <- factor(recodeVar(mpg$trans, 
                                    src = list(c("auto(av)", "auto(l3)", "auto(l4)",
                                                 "auto(l5)", "auto(l6)", "auto(s4)",
                                                 "auto(s5)", "auto(s6)"),
                                               c("manual(m5)", "manual(m6)")),
                                    tgt = list("자동기어", "수동기어")), 
                          ordered = TRUE)

-   **stats::aggregate() 이용 요약집계 작업**

<!-- -->

    #요약집계 기준변수 다차원 + 요약집계 결과변수 1개인 상황
    aggregate(formula = hwy ~ drv, data = mpg, FUN = mean, na.rm = TRUE)
    aggregate(formula = hwy ~ drv.f, data = mpg, FUN = mean, na.rm = TRUE)

    #formula, data, FUN 옵션사용없이 바로 인수값 설정도 가능
    #요약집계 기준변수를 1개에서 필요한 만큼 추가해 다차원 분석이 가능
    aggregate(hwy ~ fl + drv + trans, mpg, mean, na.rm = TRUE)
    aggregate(hwy ~ fl.f + drv.f + trans.f, mpg, mean, na.rm = TRUE)

    #요약집계변수 다차원 + 요약결과변수 다차원인 상황
    #요약집계 기준변수를 1개에서 필요한 만큼 추가할 수 있으며,동시에 요약집계 결과변수도 1개에서 필요한 만큼 추가해 다차원 분석이 가능함
    #요약집계 기준변수는 단순히 플러스(+) 기호로 추가하면 되며, 요약집계 결과변수는 cbind() 함수로 연결
    aggregate(cbind(hwy, cty) ~ drv.f, mpg, mean, na.rm = TRUE)
    aggregate(cbind(hwy, cty) ~ drv.f + trans.f, mpg, mean, na.rm = TRUE)
    aggregate(cbind(hwy, cty, displ) ~ fl.f + drv.f + trans.f, mpg, mean, na.rm = TRUE)

-   **doBy::summaryBy() 이용 요약집계 작업**

<!-- -->

    #요약집계 기준변수 다차원 + 요약집계 결과변수 1개인 상황
    summaryBy(formula = mpg ~ cyl, data = mtcars, FUN = mean, na.rm = TRUE)

    #formula, data 옵션은 생략가능하지만 FUN 옵션명은 필수로 표기해야함
    #요약집계 기준변수를 1개에서 필요한 만큼 추가해 다차원 분석이 가능함
    summaryBy(mpg ~ cyl + gear, mtcars, FUN = mean, na.rm = TRUE)
    summaryBy(mpg ~ cyl + gear + am, mtcars, FUN = mean, na.rm = TRUE)

    #요약집계변수 다차원 + 요약결과변수 다차원인 상황
    summaryBy(mpg + disp ~ cyl, mtcars, FUN = mean, na.rm = TRUE)
    summaryBy(mpg + disp ~ cyl + gear, mtcars, FUN = mean, na.rm = TRUE)
    summaryBy(mpg + disp + hp ~ cyl + gear + am, mtcars, FUN = mean, na.rm = TRUE)
    #요약집계 기준변수를 1개에서 필요한 만큼 추가할 수 있으며, 동시에 요약집계 결과변수도 1개에서 필요한 만큼 추가해 다차원 분석이 가능
    #요약집계 기준변수와 결과변수 모두 단순히 플러스(+) 기호로 추가

This is an R Markdown document. Markdown is a simple formatting syntax
for authoring HTML, PDF, and MS Word documents. For more details on
using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that
includes both content as well as the output of any embedded R code
chunks within the document. You can embed an R code chunk like this:

`{r cars} summary(cars)`

Including Plots
---------------

You can also embed plots, for example:

`{r pressure, echo=FALSE} plot(pressure)`

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.
