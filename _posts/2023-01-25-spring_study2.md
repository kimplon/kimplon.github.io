---
layout: single
title:  "스프링 부트와 AWS로 혼자 구현하는 웹 서비스 책 스터디 2장!"
categories: spring
tag: [backend, java, spring, springboot]
toc: true
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

<h3>Chapter 02 스프링부트에서 테스트 코드를 작성하자</h3>



<h4>chapter 2.1 테스트 코드 소개</h4>



단위 테스트와 TDD는 개발에 매우 중요한 개념이다. 

TDD : 테스트가 주도하는 개발(Test-Driven-Development)로 테스트 코드를 먼저 작성하는 것부터 시작 



단위 테스트 : TDD의 첫 번째 단계인 기능 단위의 테스트 코드를 작성하는 것으로 TDD와 달리 테스트 코드를 꼭 먼저 작성해야 하는 것도 아니고, 리팩토링도 포함되지 않으며 순수하게 테스트 코드만 작성하는 것



❓왜 테스트 코드를 작성해야 할까❓

위키피디아는 다음과 같이 이야기 했다.



단위 테스트는 개발 단계 초기에 문제를 반견하게 도와준다.

단위 테스트틑 개발자가 나중에 코드를 리팩토링하거나 라이브러리 업그레이드 등에서 기존 기능이 올바르게 작동하는지 확인할 수 있다.

단위 테스트는 기능에 대한 불확실성을 감소시킬 수 있다.

단위 테스트는 시스템에 대한 실제 문서를 제공한다. 즉, 단위 테스트 자체가 문서로 사용할 수 있다.



👍테스트 코드의 장점

매번 코드를 수정할 때마다 계속 톰캣을 내렸다가 다시 실행하는 일을 반복하지 않아도 된다.

System.out.println()을 통해 눈으로 검증하지 않고 자동 검증이 가능하다.

개발자가 기존에 만든 기능을 안전하게 보호해 준다.



JUnit은 자바 테스트 코드를 작성하는데 도와주는 도구이다.



<h4>chapter 2.2 HelloController 테스트 코드 작성하기</h4>



일반적으로 패키지명은 웹 사이트 주소의 역순으로 한다. 

예를 들어 book.jojoldu.com이라는 사이트라면 패키지명은 com.jojoldu.book으로 하면 된다.



```python
package com.hee.study.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

Application 클래스는 앞으로 만들 프로젝트의 메인 클래스가 된다.

@SpringBootApplication

- 스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성을 모두 자동으로 설정된다.

- @SpringBootApplication이 있는 위치부터 설정을 읽어가기 때문에

이 클래스는 항상 프로젝트 최상단에 위치해야만 한다.

SpringApplication.run으로 인해 내장 WAS를 실행한다.

- 내장 WAS : 별도로 외부에 WAS를 두지 않고 애플리케이션을 실행할 때

내부에서 WAS를 실행하는 것이다.

- 이렇게 되면 항상 서버에 톰캣을 설치할 필요가 없게 되고,

스프링 부트로 만들어지 Jar파일(실행 가능한 Java 패키징 파일)로 실행하면 된다.

- 언제 어디서나 같은 환경에서 스프링 부트를 배포할 수 있다.


테스트를 위한 Controller를 생성한다.

web이라는 하위패키지를 생성한 후 HelloController라는 클래스를 생성한다.



```python
package com.hee.study.springboot.web;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController //1.
public class HelloController {

    @GetMapping("/hello") //2.
    public String hello(){
        return "hello";
    }

}
```

1. @RestController

- 컨트롤러를 JSON을 반환하는 컨트롤러로 만들어준다.

- 예전에는 @ResponseBody를 각 메소드마다 선언했던 것을 한번에 사용할 수 있게 해준다고 생각하면 된다.

2. @GetMapping

- HTTP Method인 Get의 요청을 받을 수 있는 API를 만들어준다.

- 예전에는 @RequestMapping(method = RequestMethod.GET)으로 사용되었다.

이제 이 프로젝트트는 /hello로 요청이 오면 문자열 hello를 반환하는 기능을 가지게 되었다.


테스트 코드로 검증해 보기위해 src/text/java 디렉토리에 앞에 생성했던 패키지를 그대로 다시 생성해준다.


테스트 코드를 작성할 클래스(이하 테스트 클래스)를 생성한다.



```python
package com.hee.study.springboot;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;

import com.hee.study.springboot.web.HelloController;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest(HelloController.class) //1.
public class HelloControllerTest {

    @Autowired //2.
    private MockMvc mvc; //3.

    @Test
    void hello가_리턴된다() throws Exception {
        String hello = "hello";

        mvc.perform(get("/hello")) //4.
                .andExpect(status().isOk()) //5.
                .andExpect(content().string(hello)); //6.
    }
}
```

1. @WebMvcTest

- 여러 스프링 테스트 어노테이션 중, Web(Spring MVC)에 집중할 수 있는 어노테이션이다.

- 선언할 경우 @Contorller, @ControllerAdvice등을 사용할 수 있다.

- 단, @Service, @Component, @Repository등은 사용할 수 없다.

- 여기서는 컨트롤러만 사용하기 때문에 선언한다.

2. @Autowired

- 스프링이 관리하는 빈(Bean)을 주입받는다.

3. private MockMvc mvc

- 웹 API를 테스트할 때 사용한다.

- 스프링 MVC 테스트의 시작점이다.

- 이 클래스를 통해 HTTP GET, POST등에 대한 API 테스트를 할 수 있다.**

4. mvc.perform(get("/hello"))

- MockMvc를 통해 /hello 주소로 HTTP GET 요청을 한다.

- 체이닝이 지원되어 아래와 같이 여러 검증 기능을 이어서 선언할 수 있다.

5. andExpect(status().isOk())

- mvc.perform의 결과를 검증한다.**

- HTTP Header의 Status를 검증한다.

- 우리가 흔히 알고 있는 200, 404, 500등의 상태를 검증한다.

- 여기선 OK 즉, 200인지 아닌지를 검증한다.

6. .andExpect(content().string(hello))

- mvc.perform의 결과를 검증한다.

- 응답 본문의 내용을 검증한다.

- Controller에서 "hello"를 리턴하기 때문에 이 값이 맞는지 검증한다.


테스트 코드를 실행하기 위해 메소드 왼쪽의 화살표를 클릭한다.



테스트가 성공적으로 실행되는 것을 확인하고 나면

이제 수동으로 실행해서 정상적으로 값이 출력되는지 확인해본다.


실행해보면 테스트 실행과 마찬가지로 스프링 부트 로그가 보인다.

로그에서 톰캣 서버가 8080 포트로 실행된 것을 알 수 있다.



웹 브라우저를 열어 localhost:8080/hello로 접속한다.

그럼 다음과 같이 hello가 브라우저에 출력된 것을 볼 수 있다.


❗무조건 테스트 코드로 먼저 검증 후, 정말 못 믿겠다는 생각이 들 때만 프로젝트를 실행해 확인해야한다.


<h4>chapter 2.3. Lombok 소개 및 설치하기</h4>


lombok의 역할

롬복은 자바 개발할 때 자주 사용되는 코드 Getter, Setter, 기본생성자, toString 등 어노테이션으로 자동 생성해주는 역할이 있다.


이미 설치 되어있어서, lombok 설치 과정은 생략.



```python
import lombok.Getter;
import lombok.RequiredArgsConstructor;

@Getter //1.
@RequiredArgsConstructor //2.
public class HelloResponseDto {
    
    private final String name;
    private final int amount;
    
}
```

1. @Getter

- 선언된 모든 필드의 get 메소드를 생성해준다.

2. @RequiredArgsConstructor

- 선언된 모든 final 필드가 포함된 생성자를 생성해준다.

- final이 없는 필드는 생성자에 포함되지 않는다.



```python
import com.hee.study.springboot.web.dto.HelloResponseDto;
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.AssertionsForClassTypes.assertThat;

public class HelloResponseDtoTest {

   @Test
   public void 롬복_기능_테스트(){
       //given
       String name = "test";
       int amount = 1000;

       //when
       HelloResponseDto dto = new HelloResponseDto(name, amount);

       //then
       assertThat(dto.getName()).isEqualTo(name); //1. , 2.
       assertThat(dto.getAmount()).isEqualTo(amount);
   }

}
```

1. assertThat

- assertj라는 테스트 검증 라리브러리릐 검증 메소드이다.

- 검증하고 싶은 대상을 메소드 인자로 받는다.

- 메소드 체이닝이 지원되어 isEqualTo와 같이 메소드를 이어서 사용할 수 있다.

2. isEqualTo

- assertj의 동등 비교 메소드이다.

- assertThat에 있는 값과 isEqualTo의 값을 비교해서 같을 때만 성공이다.



Junit의 기본 assertThat이 아닌 assertj의 assertThat을 사용한 이유



CoreMathcer와 달리 추가적으로 라이브러리가 필요하지 않다.



Junit의 assertThat을 쓰게되면 is()와 같이 CoreMatchers 라이브러리가 필요하다.

자동완성이 좀 더 확실하게 지원된다.



```python
import com.hee.study.springboot.web.dto.HelloResponseDto;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController //1.
public class HelloController {

    @GetMapping("/hello") //2.
    public String hello(){
        return "hello";
    }

    @GetMapping("/hello/dto")
    public HelloResponseDto helloDto(@RequestParam("name") String name, //3.
                                     @RequestParam("amount") int amount){
        return new HelloResponseDto(name, amount);
    }

}
```

3. @RequestParam

외부에서 API로 넘긴 파라미터를 가져오는 어노테이션이다.

여기서는 외부에서 name(@RequestParam("name"))이라는 이름으로 넘긴 파라미터를

메소드 파라미터 name(String name)에 저장하게 된다.


Dto??

DTO는 데이터 전송 객체입니다. 



이름에서 알 수 있듯이 DTO는 데이터 전송을 위해 생성되는 객체를 말합니다.



```python
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.hamcrest.Matchers.is;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

import com.hee.study.springboot.web.HelloController;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest(HelloController.class) //1.
public class HelloControllerTest {

    @Autowired //2.
    private MockMvc mvc; //3.

    @Test
    void hello가_리턴된다() throws Exception {
        String hello = "hello";

        mvc.perform(get("/hello")) //4.
                .andExpect(status().isOk()) //5.
                .andExpect(content().string(hello)); //6.
    }

    @Test
    public void helloDto가_리턴된다() throws Exception{
        String name = "hello";
        int amount = 1000;

        mvc.perform(
                get("/hello/dto")
                        .param("name", name) //7.
                        .param("amount", String.valueOf(amount)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name", is(name))) //8.
                .andExpect(jsonPath("$.amount", is(amount)));
    }
}
```

7. param

API 테스트할 때 사용될 요청 파라미터를 설정한다.

단, 값은 String만 허용된다.

그래서 숫나/날짜 등의 데이터도 등록할 때는 문자열로 변경해야만 가능하다.

8. jsonPath

JSON 응답값을 필드별로 검증할 수 있는 메소드이다.

$를 기준으로 필드명을 명시한다.

* 여기서는 name과 amount를 검증하니 $.name, $.amount로 검증한다.


