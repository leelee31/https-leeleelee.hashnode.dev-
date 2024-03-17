---
title: "Querydsl 학습 1. 환경설정"
datePublished: Sun Mar 17 2024 14:56:24 GMT+0000 (Coordinated Universal Time)
cuid: cltvn3llt000209l28puoechy
slug: querydsl-1
tags: querydsl

---

# 라이브러리 추가 시 주의할 점

라이브러리 추가 외에도 **빌드와 관련된 설정**을 해줘야 한다.

```plaintext
build.gradle (스프링 부트 2.x 기준)

plugins {
 id 'org.springframework.boot' version '2.2.2.RELEASE'
 ...
 //querydsl 추가
 id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
}

//querydsl 추가
implementation 'com.querydsl:querydsl-jpa'기타 설정

...

//querydsl 추가
def querydslDir = "$buildDir/generated/querydsl" //디렉토리 설정

querydsl { // querydsl 플러그인의 설정
 jpa = true // jpa 사용 여부
 querydslSourcesDir = querydslDir // 경로
}
sourceSets { // gradle의 sourceSets
 main.java.srcDir querydslDir // main sourceSets에 querydslDir 경로 추가
}
configurations {
 querydsl.extendsFrom compileClasspath
}
compileQuerydsl {
 options.annotationProcessorPath = configurations.querydsl
 // querydsl 구성을 어노테이션 프로세서 경로에 추가
}
```

### 왜 설정을 해줘야 하지?

JPA 엔티티에 대한 **Querydsl 쿼리 타입 클래스(Q클래스)를 생성하기 위한 설정이기 때문**이다.
<br>
<br>

### Querydsl 쿼리 타입 클래스를 왜 생성해야 되나?

ORM 프레임워크 중 하나인 하이버네이트는 SQL과 매우 유사한 문자열 기반 쿼리 언어 JPQ을 제안한다.

그러나 JPQL은 타입 불안전성, 정적 쿼리 검사 부재 등 단점이 있다.

이러한 단점을 보완하기 위해 메타 모델 클래스에 대한 아이디어가 생겨났으며 Querydsl는 Q클래스라는 메타 모델 클래스를 사용하는 것이다.
<br>
<br>
<br>

참고

인프런 &lt;실전! Querydsl&gt;

[https://www.baeldung.com/intro-to-querydsl](https://www.baeldung.com/intro-to-querydsl)