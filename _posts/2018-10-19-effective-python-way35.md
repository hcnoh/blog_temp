---
layout: post
title: "WAY 35. 메타클래스로 클래스 속성에 주석을 달자"
date: 2018-10-19 18:57:12
tagline: "이펙티브 파이썬 코딩의 기술 책 스터디 정리"
categories:
- 이펙티브 파이썬 스터디
tags:
- python
image: /thumbnail-mobile.png
author: "Hyungcheol Noh"
permalink: /2018-10-19-effective-python-way35
---

## 데이터베이스 예제
- 고객 데이터베이스의 로우를 표현하는 새 클래스를 정의
  - 데이터베이스 테이블의 각 칼럼에 대응하는 클래스의 프로퍼티가 있어야 함
  - 프로퍼티를 칼럼 이름과 연결하는 데 사용할 디스크립터 클래스 정의

```python
class Field(object):
    def __init__(self, name):
        self.name = name
        self.internal_name = "_" + self.name
    
    def __get__(self, instance, instance_type):
        if instance is None:
            return self
        return getattr(instance, self.internal_name, "")
        
    def __set__(self, instance, value):
        setattr(instance, self.internal_name, value)
```

- `Field` 디스크립터에 저장할 칼럼 이름이 있으면 =>
  - 내장 함수 `setattr`과 `getattr`을 사용하여 모든 인스턴스별 상태를 인스턴스 딕셔너리에 보호 필드로 직접 저장
  
```python
class Customer(object):
    # 클래스 속성
    first_name = Field("first_name")
    last_name = Field("last_name")
    prefix = Field("prefix")
    suffix = Field("suffix")
```
  