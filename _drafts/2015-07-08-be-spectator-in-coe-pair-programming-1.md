---
layout: post
title: บันทึกการนั่งชม CoE Pair Programming
description:
tags: [pair-programming,tdd,spectator,practice,python]
category: articles
toc: true
---

นี่เป็นบันทึกการชม pair programming ครั้งแรกของผมครับ ในวันที่ 6 กรกฎาคม 2558 มีการ pair programming ภายใน Computer Engineering, Prince of Songkla University

ระหว่าง [@ibotdotout] และ [@superizer]
โดยโจทย์ในวันนี้คือ [Kata Poker Hands]
และใช้ภาษา python 2 ในการฝึกครับ

ซึ่งผมก็ได้มีโอกาสเป็นผู้ชมการ pair programming กันสดๆ โดยผ่านทาง [wemux] ครับ
โดย source code การชมการ Pair Programming ครั้งนี้ได้[ที่นี่](https://github.com/ibotdotout/coe-pairing/tree/poker_hands_v1)เลยครับ


## จุดประสงค์ในการฝึก
- ฝึก [TDD](http://www.somkiat.cc/what-we-learn-from-tdd/)
- ฝึกการเขียน Unit Test

## ปูพื้นฐาน
- [แนวคิดการพัฒนาแบบ Test-Driven Development (TDD) มี 3  สถานะ Failed > Passed > Refactor](http://www.somkiat.cc/move-state-in-tdd/)
- [การจับคู่เขียนโปรแกรม (Pair Programming)](http://www.pairprogramwith.me/)

## โครงสร้างไฟล์

โดยไฟล์ที่ใช้ในการเขียน unit test มีตามนี้

```python
poker_hands.py				# ตัวไฟล์โค๊ดหลัก
tests/poker_hands_test.py	# ไฟล์ test
watchdog_python.py			# ไฟล์ที่ทำหน้าที่ test อัตโนมัติ
requirements.txt			# รายการ package python ที่ต้องใช้
```

## อธิบายสัญลักษณ์
- `ฝั่ง code` คือไฟล์ `poker_hands.py` สำหรับเขียน code ตัวโปรแกรมหลัก
- `ฝั่ง test` คือไฟล์ `tests/poker_hands_test.py` สำหรับเขียน test ของตัวโปรแกรมหลัก
- สัญลักษณ์ แต่ละสถานะของ TDD ซึ่งทั้ง 3 สถานะของ TDD สามารถอ่านเพิ่มได้[ที่นี่](http://www.somkiat.cc/move-state-in-tdd/)
    - <a class="pure-button button-xsmall button-error">Fail</a> (Red) สถานะทดสอบไม่ผ่าน 
    - <a class="pure-button button-xsmall button-success">Pass</a> (Green) สถานะทดสอบผ่าน
    - <a class="pure-button button-xsmall button-secondary">Refactor</a> สถานะปรับปรุง code
   
    
## มาเริ่มกันเลยครับ
### เตรียมไฟล์

```python
# tests/poker_hands_test.py

import poker_hands as pk
import unittest

class PokerHandsTest(unittest.TestCase):
    pass
```

จำไม่ได้แล้ว่าใครเขียนตรงไหนบ้าง เอาเป็น ตา A, ตา B ละกัน

### วางแผน

> [@ibotdotout]: เราก็ต้องมาคิดกันก่อนว่าจะเริ่มจากตรงไหน  ทำส่วนเล็กๆ ที่สุด
น่าจะตรงส่วนของ ส่งการ์ดไป แล้ว ส่งค่า กลับมา เช่น
>
```
ฟังก์ชั่น get_card_score
รับ AD ได้ 14
รับ 3S ได้ 3
รับ KC ได้ 13
```

เริ่มปิงปอง

### 1. ตา A เขียน
<a class="pure-button button-xsmall button-error">Fail</a>
เขียน test ให้ fail

```python
# tests/poker_hands_test.py
class CardScoreTest(unittest.TestCase):

    def test_give_3C_should_be_3(self):
        card = "3C"
        answer = 3
        result = pk.get_card_score(card)
        self.assertEqual(result, answer)
```
### 2. ตา B เขียน
<a class="pure-button button-xsmall button-success">Pass</a>
เขียน code ให้ pass

เขียนให้พอ test ผ่านพอไม่ต้องดี เน้นเร็ว ค่อยมา refactor code ทีหลัง ในนี้เลย return เลข 3 เอาดื้อๆเลย ตาม testcase ตรงๆ เลย ถ้าทำมากกว่านี้จะเป็น over design

```python
# poker_hands.py
def get_card_score(card):
    return 3
```
<a class="pure-button button-xsmall button-secondary">Refactor</a>
เนื่องจากเพิ่งเริ่มเขียน เลยไม่มีอะไรให้ refactor เลยข้ามขั้นตอนนี้ไปก่อนครับ

<a class="pure-button button-xsmall button-error">Fail</a>
เขียนให้ test แล้ว test fail เลย เพิ่มว่า ถ้ารับ 4S จะได้ 4

```python
# tests/poker_hands_test.py
class CardScoreTest(unittest.TestCase):

    def test_give_4S_should_be_4(self):
        card = "4S"
        answer = 4
        result = pk.get_card_score(card)
        self.assertEqual(result, answer)
```
### 3. ตา A เขียน
<a class="pure-button button-xsmall button-success">Pass</a>
แก้โค๊ดให้ test ผ่าน

```python
# poker_hands.py
def get_card_score(card):
    return int(card[0])
```

<a class="pure-button button-xsmall button-secondary">Refactor</a>
Refactor ในส่วน test จาก

```python
# tests/poker_hands_test.py
class CardScoreTest(unittest.TestCase):

    def test_give_3C_should_be_3(self):
        card = "3C"
        answer = 3
        result = pk.get_card_score(card)
        self.assertEqual(result, answer)

    def test_give_4S_should_be_4(self):
        card = "4S"
        answer = 4
        result = pk.get_card_score(card)
        self.assertEqual(result, answer)
```

เป็น

```python
# tests/poker_hands_test.py
class CardScoreTest(unittest.TestCase):

    def _helpper_assert(self, card, answer):
        result = pk.get_card_score(card)
        self.assertEqual(result, answer)

    def test_give_3C_should_be_3(self):
        card = "3C"
        answer = 3
        self._helpper_assert(card, answer)

    def test_give_4S_should_be_4(self):
        card = "4S"
        answer = 4
        self._helpper_assert(card, answer)
```

<a class="pure-button button-xsmall button-error">Fail</a>
เพิ่ม test ให้ ฝั่งโค๊ด fail

```python
    def test_give_AS_should_be_14(self):
        card = "AS"
        answer = 14
        self._helpper_assert(card, answer)
```

แสดงว่าตอนนี้ฝั่งโค๊ด test fail

### 4. ตา B
<a class="pure-button button-xsmall button-success">Pass</a>
แก้ให้โค๊ด test ผ่าน



<a class="pure-button button-xsmall button-secondary">Refactor</a>
Refactor ฝั่ง code ซะ

```python
def get_card_score(card):
    first_letter = card[0]
    score_table = map(str, range(2, 10)) + ['T', 'J', 'Q', 'K', 'A']
    return score_table.index(first_letter) + 2
```

## Resources
- โจทย์วันนี้ [Kata Poker Hands]
- [ดาวโหลด source code การปิงปองครั้งนี้ได้ที่นี่](https://github.com/ibotdotout/coe-pairing/tree/poker_hands_v1)
- [wemux] เครื่องมือที่ช่วยทำให้ ใช้ [tmux] ได้หลายๆ คนพร้อมๆ กัน

[@ibotdotout]: https://github.com/ibotdotout
[@superizer]: https://github.com/superizer
[wemux]: https://github.com/zolrath/wemux
[tmux]: http://tmux.github.io/
[Kata Poker Hands]: http://www.codingdojo.org/cgi-bin/index.pl?KataPokerHands

