---
layout: single
title:  "스프링 부트와 AWS로 혼자 구현하는 웹 서비스 책 스터디 1장"
categories: spring
tag: [spring, springboot, java, backend]
toc: true
author_profile: true
---

<head>
  <style>
    table.dataframe {
      white-space: normal;
      width: 100%;
      height: 240px;
      display: block;
      overflow: auto;
      font-family: Arial, sans-serif;
      font-size: 0.9rem;
      line-height: 20px;
      text-align: center;
      border: 0px !important;
    }

    table.dataframe th {
      text-align: center;
      font-weight: bold;
      padding: 8px;
    }

    table.dataframe td {
      text-align: center;
      padding: 8px;
    }

    table.dataframe tr:hover {
      background: #b8d1f3; 
    }

    .output_prompt {
      overflow: auto;
      font-size: 0.9rem;
      line-height: 1.45;
      border-radius: 0.3rem;
      -webkit-overflow-scrolling: touch;
      padding: 0.8rem;
      margin-top: 0;
      margin-bottom: 15px;
      font: 1rem Consolas, "Liberation Mono", Menlo, Courier, monospace;
      color: $code-text-color;
      border: solid 1px $border-color;
      border-radius: 0.3rem;
      word-break: normal;
      white-space: pre;
    }

  .dataframe tbody tr th:only-of-type {
      vertical-align: middle;
  }

  .dataframe tbody tr th {
      vertical-align: top;
  }

  .dataframe thead th {
      text-align: center !important;
      padding: 8px;
  }

  .page__content p {
      margin: 0 0 0px !important;
  }

  .page__content p > strong {
    font-size: 0.8rem !important;
  }

  </style>
</head>


<h2>스프링 부트와 AWS로 혼자 구현하는 웹 서비스 책 스터디</h2>

<h3>Chapter 01 인텔리제이로 스프링 부트 시작하기</h3>


인텔리제이 장점


강력한 추천 기능

1. 훨씬 더 다양한 리팩토링과 디버깅 가능

2. 이클립스의 깃에 비해 훨씬 높은 자유도

3. 프로젝트 시작할 때 인덱싱을 하여 파일을 비롯한 자원들에 대한 빠른 검색 속도

4. 대학생들은 인텔리제이 Ultimate 버전을 학교 메일을 통해 무료로 이용할 수 있습니다.



[인텔리제이 학생 버전 페이지](https://www.jetbrains.com/ko-kr/community/education/#students)



<h4>chapter 1.4 그레이들 프로젝트를 스프링 부트 프로젝트로 변경하기</h4>



이 책에서는 스프링 이니셜라이저를 사용하지 않고,

그레이들 프로젝트를 만들고 build.gradle 파일을 수정하여

스프링부트 프로젝트로 변경하였습니다.


그 이유는 build.gradle 파일의 역할과, 의존성 추가가 필요하면 어떻게 해야할 지 알기 위해서였습니다.

build.gradle 파일


```java
plugins {
    id 'java'
    id 'org.springframework.boot' version '2.4.1'
    id 'io.spring.dependency-management' version '1.0.13.RELEASE'
}

group 'com.jojoldu.book'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.2'
    testImplementation 'junit:junit:4.13.1'
    testImplementation 'junit:junit:4.13.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.7.2'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'com.h2database:h2'
    compileOnly 'org.projectlombok:lombok:1.18.10'
    annotationProcessor 'org.projectlombok:lombok:1.18.10'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

test {
    useJUnitPlatform()
}
```

io.spring.dependency-management 플러그인은 스프링 부트의 의존성들을 관리해주는 플러그인이다.

뿐만 아니라 'java', 'org.springframework.boot'도 자바와 스프링부트를 사용하기 위한 필수 플러그인이다.


책에 나온 build.gradle 코드와 약간 다른 부분이 있는데, 스프링 부트의 버전과 gradle 4,5 버전 차이때문에
책에 나오는대로 코드를 작성하면 실행이 안될 수 있다. 

google 검색이나, 책 저자의 레포지토리 이슈 탭에 들어가서 각 버전 별 코드 작성을 유연하게 수정하여
실행해보았더니 오류없이 잘 실행되었다.



![gradle](/assets/images/gradle.png)

위 코드의 dependencies 코드 부분에서는 프로젝트 개발에 필요한 의존성들을 선언하는 곳이다.


위 사진의 코끼리 아이콘을 누르면 build.gradle 파일 변경 내용을 적용시킬 수 있다.


<h4>chapter 1.5 인텔리제이에서 깃과 깃허브 사용하기</h4>


.gitignore 파일을 생성하여 
git repository에 commit이 되지 않는 파일을 설정할 수 있다. 


jCenter 서비스 지원 중단으로 인해서
build.gradle 파일 내에 repository를 mavenCentral 로 마이그레이션 해야 하는 이슈가 있다. 



