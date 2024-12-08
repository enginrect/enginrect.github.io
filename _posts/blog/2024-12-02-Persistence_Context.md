---
title: Persistence Context
description: JPA에서 제공하는 핵심 개념인 영속성 컨텍스트 내용입니다.
author: jay
date: 2024-12-02 12:00:00 +0800
categories: [Blogging]
tags: [blog, Java, Spring]
pin: false
math: true
mermaid: true
---

영속성 컨텍스트(Persistence Context)는 JPA(Java Persistence API)의 핵심 개념으로, 엔티티를 영구 저장소(예: 데이터베이스)와 연결하여 애플리케이션에서 엔티티 객체를 관리하는 역할을 합니다. 이는 엔티티의 상태 변화를 추적하고, 데이터베이스와의 동기화를 통해 일관성을 유지합니다.

스프링 프레임워크에서는 EntityManager가 이러한 영속성 컨텍스트를 관리합니다. EntityManager는 엔티티의 생성, 수정, 삭제 등의 작업을 수행하며, 영속성 컨텍스트 내에서 엔티티의 상태를 추적합니다. 이를 통해 애플리케이션은 데이터베이스와의 상호작용을 간소화하고, 엔티티의 상태 변화를 효율적으로 관리할 수 있습니다.

스프링 데이터 JPA는 이러한 EntityManager를 활용하여 데이터 접근 계층을 구현합니다. 예를 들어, JpaRepository 인터페이스는 JPA의 기능을 확장하여 CRUD 작업을 간편하게 수행할 수 있도록 지원합니다. 이를 통해 개발자는 복잡한 데이터 접근 로직을 간소화하고, 생산성을 높일 수 있습니다.

아래는 영속성 컨텍스트를 사용하는 간단한 자바 예제입니다. 이 예제는 EntityManager를 통해 엔티티를 관리하는 방법을 보여줍니다.

### 1. 엔티티 클래스 정의

```java
import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class User {
    @Id
    private Long id;
    private String name;

    *// Getter and Setter*
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

### 2. EntityManager를 사용한 간단한 작업

```java
import jakarta.persistence.EntityManager;
import jakarta.persistence.EntityManagerFactory;
import jakarta.persistence.Persistence;

public class JpaExample {
    public static void main(String[] args) {
        *// EntityManagerFactory 생성*
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("example-unit");
        EntityManager em = emf.createEntityManager();

        *// 트랜잭션 시작*
        em.getTransaction().begin();

        *// 엔티티 생성 및 영속성 컨텍스트에 저장*
        User user = new User();
        user.setId(1L);
        user.setName("John Doe");
        em.persist(user); *// 영속 상태로 변경*

        *// 엔티티 조회*
        User foundUser = em.find(User.class, 1L);
        System.out.println("Found User: " + foundUser.getName());

        *// 엔티티 수정*
        foundUser.setName("Jane Doe"); *// 영속성 컨텍스트에 의해 자동 업데이트*

        *// 트랜잭션 커밋*
        em.getTransaction().commit();

        *// 엔티티 매니저 및 팩토리 종료*
        em.close();
        emf.close();
    }
}

```
### 3. 주요 동작 설명

	* 	em.persist(): 엔티티를 영속성 컨텍스트에 저장합니다.
	* 	em.find(): 영속성 컨텍스트에서 엔티티를 조회합니다. 데이터베이스에서 직접 조회하지 않고, 캐시된 엔티티를 반환할 수 있습니다.
	* 	**변경 감지(Dirty Checking)**: foundUser.setName("Jane Doe");와 같이 엔티티의 값을 변경하면, 영속성 컨텍스트가 이를 감지하고 자동으로 데이터베이스에 업데이트합니다.

### 실행 결과

```bash
Found User: John Doe
```


자세한 내용은 스프링 공식 문서의 [ApplicationContext](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)와 [JpaRepository](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/JpaRepository.html) 섹션을 참고하시기 바랍니다.