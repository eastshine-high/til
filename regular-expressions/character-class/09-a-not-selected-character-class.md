# 문자 클래스가 ^로 시작하면 지정된 문자가 선택되지 않는다

문자 클래스가 `^`로 시작하면 지정된 문자가 선택되지 않는다.

If a character class starts with `^`, then specified characters will not be selected

<br>

### Source

ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz 0123456789

<br>

### Case 1

Regular expression : **[^CDghi45]**

First match : `A`BCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz 0123456789

All matches : `AB`CD`EFGHIJKLMNOPQRSTUVWXYZ abcdef`ghi`jklmnopqrstuvwxyz 0123`45`6789`

<br>

### Case 2

Regular expression : **[^W-Z]**

First match : `A`BCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz 0123456789

All matches : `ABCDEFGHIJKLMNOPQRSTUV`WXYZ `abcdefghijklmnopqrstuvwxyz 0123456789`

<br>

### Reference

[https://zvon.org/comp/r/tut-Regexp.html#Pages~Page_9](https://zvon.org/comp/r/tut-Regexp.html#Pages~Page_9)