# 정규식은 대소문자를 구분한다

정규식은 대소문자를 구분합니다. 따라서 Case 1은 지정된 텍스트를 찾지만 Case 2는 찾지 않습니다.

Regular expressions are case sensitive. Therefore *Case 1* will find the specified text, but *Case 2* will not.

<br>

### Source

Hello, world!

<br>

### Case 1

Regular Expession : **Hello**

First match : `Hello`, world

All matches : `Hello`, world

<br>

### Case 2
Regular Expession : **Hello**

First match : hello, world

All matches : hello, world

<br>

### Reference

[http://zvon.org/comp/r/tut-Regexp.html#Pages~Page_1](http://zvon.org/comp/r/tut-Regexp.html#Pages~Page_1)