**파일 설명**
- talent_donation.pptx: 중산고 재능기부 내용 파워포인트 파일 (맥에서 작성됨)
- talent_donation.pdf: 중산고 재능기부 내용 pdf 파일
- saved_result.zip: rJava 오류에 대비하여 각 키워드에 따른 형태소 분석 결과가 저장된 RData 파일을 압축 저장.
- README.md: 현재 보고 있는페이지


## 1. 검색할 단어를 컴퓨터에게 알려줍니다.
~~~
install.packages("rvest") # 크롤링을 위한 패키지 설치, 한번만 실행!!
library(rvest) # 패키지 불러오기
search_query <- readline("검색어를 입력해 주세요: ") # 검색하고 싶은 주제를 입력.
~~~



## 2. 네이버 뉴스 기사를 검색하고 100페이지 분량의 제목을 컴퓨터에 저장합니다.
~~~
temp <- c()

for (i in 1:100)
{
  url <- URLencode(paste0("https://search.naver.com/search.naver?&where=news&query=", search_query, "&sm=tab_pge&sort=0&photo=0&field=0&reporter_article=&pd=0&ds=&de=&docid=&nso=so:r,p:all,a:all&mynews=0&start=", 10*(i-1) + 1 ,"&refresh_start=0"))
  html <- read_html(url, encoding = 'utf-8')
  add <- html %>% html_nodes("._sp_each_title") %>% html_text()
  temp <- c(temp, add)
  print(paste0("[", i, "]", "번째 페이지까지 완료하였습니다. ", "  [예시]: ", add[1]))
  rm(add)
}
~~~



## 3. 각 문장에서 ‘단어’들만 뽑아낸 후 상위 100개의 단어를 살펴봅니다.
~~~
install.packages("KoNLP") # 문장에서 명사를 꺼내오기 위한 패키지, 한번만 실행 !!
library(KoNLP) # 패키지 불러오기
useNIADic() # 패키지에 내장 된 사전 불러오기

result <- lapply(temp, function(x) extractNoun(x)) # 명사만 꺼내오기
result <- unlist(result)
result <- table(result[nchar(result) > 1]) # 두 단어 이상인 명사만 저장하기
result <- result[order(result, decreasing = T)]

print(result[1:100])
~~~



## 4. 워드클라우드를 통해서 단어를 살펴보기
~~~
install.packages("wordcloud")
install.packages("RColorBrewer")
library(wordcloud)
library(RColorBrewer)

wordcloud(names(result[1:100]), result[1:100], colors = brewer.pal(8, "Dark2"), random.order = T, random.color = T)
~~~


### 참고) 저장 된 파일 불러오기
~~~
setwd(“파일 다운 받은 경로”)
load("result_4차산업혁명.RData")
load("result_인공지능.RData")
load("result_빅데이터.RData")
load("result_블록체인.RData")
~~~


### 참고) rJava 에러 발생 시 아래 경로에서 java 설치
~~~
http://www.java.com/ko
~~~
