# JPA N+1 문제

### 문제 상황

- 특정 객체가 타 테이블의 목록 객체와 연관되어있으면 발생할 수 있음
- N+1의 의미는 쿼리는 1개 날렸는데 그 이후 추가 쿼리가 N개 나가는 것

**예시 코드**

```
class Post {
	List<Comment> comment;

}

class Comment {

}
```

- 위 예시에서 Post 객체를 조회하고 Comment를 가져올때 쿼리는
    - Post 1회 Comment N회 수행하게 된다.

### 해결 방법

- Join Fetch : ‘JOIN FETCH’를 사용하여 연관된 엔터티들을 한번의 쿼리로 조회할 수 있음
- Entity Graph : @BatchSize 애노테이션을 사용하여 한 번의 쿼리로 일정 양의 연관된 엔터티를 가져오도록 설정할 수 있음
- Lazy Loading: 기본적으로 연관된 엔터티는 Lazy Loading 전략을 사용. 이 전략은 엔터티에 접근할 때까지 데이터를 로드하지 않기 때문에 이를 통해 불필요한 쿼리 실행을 방지할 수 있음
    - Lazy Loading을 사용한 후 조회되는 연관 객체는 프록시로 조회된다.

### LAZY(지연로딩)와 EAGER(즉시로딩)

- Lazy → 실제로 연관된 객체의 메소드를 호출할 때 실제 쿼리가 수행됨
    - @OneToMany, @ManyToMany
- Eager → 실무에선 지연 로딩만 사용
    - 즉시 로딩을 활용하면 예상하지 못한 문제가 발생할 수 있음
    - 즉시로딩은 JPQL에서 N+1 문제를 발생시킴
    - @ManyToOne, @OneToOne
