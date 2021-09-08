---
title: "루비 1일 차 자율 학습"
category: "Ruby"
date: "2021-09-09"
desc: "브루스 테이트의 세븐 랭귀지를 정리했습니다"
thumbnail: "./images/default.jpg"
alt: "브루스 테이트의 세븐 랭귀지 책을 읽으며 1장 1일차에 자율 학습을 정리했습니다"
---

다음 내용을 찾아보라.

- 루비 API
- "프로그래밍 루비"(인사이트, 2007)의 온라인 무료 버전
- 문자열의 일부를 치환하는 메서드
- 루비의 정규표현식에 대한 정보
- 루비의 범위(range)에 대한 정보

## 루비 API

루비 API를 보고 저자가 말하고자 하는 API란 무엇일까 생각이 먼저 들었지만, 대강 API 문서를 찾아보라는 뜻으로 알고 검색해보았다.

Ruby API 란 단어로 구글링하니 제일 상단에 다음과 같은 사이트([참고](https://rubyapi.org))가 나타났고, 해당 사이트는 Ruby의 공식 GitHub([참고](https://github.com/ruby/ruby)) 문서를 사용성을 고려하여 만든 사이트라고 한다. 공부하다 관련된 내용을 찾고 싶을 때, 적절히 활용하면 좋을 것 같다.

그리고 어렵지 않게 Ruby 언어의 공식 문서를 찾을 수 있었는데([참고](https://www.ruby-lang.org/ko/)), 전반적인 Ruby에 대한 설명이나 필요한 공식 문서들은 한글로 번역되어 설명해주고 있지만, 공식 문서의 내용은 번역되지 않은 상태로 제공되고 있었다. 종종 공부할 때 참고하면 될 것 같다.

## "프로그래밍 루비"(인사이트, 2007)의 온라인 무료 버전

다행히도 한국어로 번역된 책이 있었다. "프로그래밍 루비"라는 이름으로 인사이트 출판사에서 나온 책으로, 현재 2판까지 나온 상태이다([참고](http://ebook.insightbook.co.kr/book/33)). 이 url로 들어가면 책 미리보기가 가능하여, 책 소개나 목차를 확인 할 수 있다.

하지만 한국어판이 온라인 무료 버전으로 풀린 적은 없었던 것 같아 찾을 수 없었고, 대신 영문판 pdf는 찾을 수 있었다([참고](http://www.cs.uni.edu/~wallingf/teaching/agile-may2010/ruby/programming-ruby.pdf)). 나중에 꼭 필요한 내용이 있으면 한 번 들여다 보면 좋을 것 같다.

아쉬운 마음에 다른 문서가 있을까 해서 찾아보니 한국 루비 사용자 모임에서 만든 사이트를 찾을 수 있었다([참고](http://rubykr.github.io/)). 공부할 때 참고하면 큰 도움이 될 것 같다.

## 문자열의 일부를 치환하는 메서드

Ruby에서 str.sub/str.gsub 가 문자열 일부를 치환하는 메서드이다. sub은 하나만, gsub은 전역으로 모두 치환한다. 간단한 예제는 다음과 같다.

```ruby
# Syntax
sentence.gsub(/match/, "replacement")

sentence = 'My name is Robert'
sentence.sub! 'Robert', 'Joe' // => 'My name is Joe'
치환 할 문자열이 없어도 예외를 일으키지 않음([]= 의 변형)

'mislocated cat, vindicating'.gsub('cat', 'dog')
=> "mislodoged dog, vindidoging"
전역 치환의 경우 개발작 원하는 결과값이 나오지 않는 경우가 있음

'mislocated cat, vindicating'.gsub(/\bcat\b/, 'dog')
=> "mislocated dog, vindicating"
정규표현식의 \b 와 같은 옵션을 사용하여 치환하는 방법을 사용해야 함
(Note. 단어 간 공백, 간격, 경계가 없을 경우 \b도 적용이 안될 수 있음)

sentence ["Robert"] = "Roger"
=> "Roger"
sentence
=> "My name is Roger"
흥미로운 방법이지만, 정답이 아님. 첫 번째 문자열만 치환함. 치환하려는 문자없으면 에러(IndexError) 발생
```

참고: [스택오버플로우](https://stackoverflow.com/questions/8381499/replace-words-in-a-string-ruby)

## 루비의 정규표현식에 대한 정보

### 문법

```ruby
string.match(pattern, offset = 0) -> matchdata or nil
string.match(pattern, offset = 0) {|matchdata|...} -> object
string.match?(pattern, offset = 0) -> true or false

'foo'.match('f', 1) # => nil
'foo'.match('o', 1) # => #<MatchData "o">
```

### 문자 클래스

문자 클래스([])는 대괄호 사이에 들어간 문자를 찾는다. 예를 들어 [aeiou] 라는 표현식은 모음을 찾는 패턴으로 "a, e, i, o, u 중 한 개"라는 의미이다. 아래는 Ruby를 이용한 정규식 예제이다.

```ruby
// a, b, c 중 한 개라도 포함되어 있는가? (가장 먼저 찾은 문자를 반환)
"Do you like dogs?".match(/[abc]/) // => nil
"Do you like cats?".match(/[abc]/) // => "c"
```

### 범위 range

하이픈(-)을 사용하면 "두 문자 사이의 범위"를 뜻한다.

```ruby
// 숫자를 포함하는가?
"Happy New Year 2018".match(/[0-9]/) // => "2"
```

### 자주 사용하는 문자 클래스

빈번하게 사용되는 메타문자는 다음과 같다.

```ruby
// 숫자
\d // [0-9]: isDigit?
\D // [^0-9]: !isDigit?

// 공백
\s // [\t\n\r\f\v]: isWhiteSpace?
\S // [^\t\n\r\f\v]: !isWhiteSpace?

// 문자
\w // [a-zA-Z0-9]: isCharater?
\W // [^a-zA-Z0-9]: !isCharater?
```

### Dot 임의의 한 글자

```ruby
"1aaaa 5a5 bbbb2".match(/\d.\d/) // => "5a5"
```

### 반복

```ruby
+      // 1 or more
*      // 0 or more
?      // 0 or 1
{3, 5} // between 3 and 5
```

### 시작과 끝

```ruby
^ // beginning of line
"Regex are cool".match(/^\w{4}/) => #<MatchData "Rege">

$ // end of line
"Regex are cool".match(/\w{4}$/) => #<MatchData "cool">
```

### 캡처 그룹

특정 패턴을 묶어서 추출하는 방법

```ruby
// 첫 번째 그룹 추출
m = "John 31".match /\w+ (\d+)/ 
=> #<MatchData "John 31" 1:"31">
m[1] // => "31"
```

이름을 지정 할 수 있음

```ruby
m = "David 30".match /(?<name>\w+) (?<age>\d+)/
=> #<MatchData "David 30" name:"David" age:"30">
m[:age] // => "30"
m[:name] // => "David"
```

### 참고 사이트

- [https://cloudstudying.kr/lectures/245](https://cloudstudying.kr/lectures/245)
- [https://www.rubyguides.com/2015/06/ruby-regex](https://www.rubyguides.com/2015/06/ruby-regex/) 
- [https://rubular.com/](https://rubular.com/)

## 루비의 범위(range)에 대한 정보

range는 시작과 끝이 있는 값의 집합으로 간격을 나타냅니다. range는 s..e(점 2개) 와 s...e(점 3개) 방식의 리터럴을 사용하거나 Range::new 를 사용하여 만들 수 있습니다. ..(점 2개)를 사용하여 구성된 range는 처음부터 끝까지를 모두 포함합니다. ...(점 3개)는 마지막 항목을 제외하고 모두 포함합니다. iterator로 사용될 때는 각 시퀀스의 value를 반환합니다.

```ruby
(-1..5).to_a     #=> []
(-5..-1).to_a    #=> [-5, -4, -3, -2, -1]
('a'..'e').to_a  #=> ["a", "b", "c", "d", "e"]
('a'...'e').to_a #=> ["a", "b", "c", "d"]

(1..5).include?(5)           #=> true
(1...5).include?(5)          #=> false
(1..4).include?(4.1)         #=> false
(1...5).include?(4.1)        #=> true
(1..4).to_a == (1...5).to_a  #=> true (4)
(1..4) == (1...5)            #=> false (5)
```

다음을 수행하라.

- "Hello, world"라는 문자열을 출력하라.
- "Hello, Ruby"라는 문자열에서 "Ruby"라는 단어의 인덱스를 찾아보라.
- 당신 자신의 이름을 열 번 출력하라.
- 1부터 10까지의 숫자를 대상으로 "This is sentence number 1"이라는 문자열을 출력하라.
- 파일에서 읽은 루비 프로그램을 실행하라.
- 보너스 문제: 더 많은 연습이 필요하다고 느낀다면, 임의의 수를 고르는 프로그램을 작성해보라. 사용자가 어떤 수를 추측하도록 하고, 추측한 값이 너무 큰지 아니면 작은지를 화면에 나타내라. (힌트: rand(10)은 0에서 9 사이 수 중에서 임의의 수를 하나 선택할 것이다. gets는 키보드로부터 정수로 바꿀 수 있는 문자열을 읽어온다.)

## "Hello, world"라는 문자열을 출력하라.

```ruby
puts "Hello, world"
=> "Hello, world"
```

## "Hello, Ruby"라는 문자열에서 "Ruby"라는 단어의 인덱스를 찾아보라.

```ruby
"Hello, Ruby".index 'Ruby'
=> 7
"Hello, Ruby".index('Ruby')
=> 7
```

## 당신 자신의 이름을 열 번 출력하라.

```ruby
x = 0
while x < 10
	puts 'CHOI HAN UL'
	x = x + 1
end
CHOI HAN UL
CHOI HAN UL
CHOI HAN UL
CHOI HAN UL
CHOI HAN UL
CHOI HAN UL
CHOI HAN UL
CHOI HAN UL
CHOI HAN UL
CHOI HAN UL
=> nil
```

## 1부터 10까지의 숫자를 대상으로 "This is sentence number 1"이라는 문자열을 출력하라.

```ruby
x = 1
=> 1
while x <= 10
	puts "This is sentence number #{x}"
	x = x + 1
end
This is sentence number 1
This is sentence number 2
This is sentence number 3
This is sentence number 4
This is sentence number 5
This is sentence number 6
This is sentence number 7
This is sentence number 8
This is sentence number 9
This is sentence number 10
=> nil
```

## 파일에서 읽은 루비 프로그램을 실행하라.

텍스트 편집기로 루비 파일을 만들고 실행해보자.

```ruby
$ vi test.rb

name = 'CHOI HAN UL'
puts "Hello #{name}"

$ ruby test.rb
Hello CHOI HAN UL
```

## 보너스 문제: 더 많은 연습이 필요하다고 느낀다면, 임의의 수를 고르는 프로그램을 작성해보라. 사용자가 어떤 수를 추측하도록 하고, 추측한 값이 너무 큰지 아니면 작은지를 화면에 나타내라. (힌트: rand(10)은 0에서 9 사이 수 중에서 임의의 수를 하나 선택할 것이다. gets는 키보드로부터 정수로 바꿀 수 있는 문자열을 읽어온다.)

```ruby
$ vi test2.rb

randomNumber = rand(10)
input = gets.chomp
selectedNumber = input.to_i

if randomNumber < selectedNumber
	puts '더 작은 숫자입니다. 다시 도전해보세요.'
elsif randomNumber > selectedNumber
	puts '더 큰 숫자였습니다. 다시 도전해보세요.'
else
	puts '정답입니다!!'
end

$ ruby test2.rb
```