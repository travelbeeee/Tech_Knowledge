# PathVariable vs QueryParameter

 `Get Method`를 통해 데이터를 넘길 경우 `HTTP Body`에 데이터를 넣을 수는 있지만, `Get`메소드의 설계는 `HTTP Body`를 비우도록 권장합니다. 따라서, 정보를 넘기기 위해 `Path Variable` 또는 `Query Paramter`를 사용해야 됩니다.

 `Path Variable`과 `Query Parameter`는 어떤 차이가 있는지, 어떤 상황에서 접합한지 정리해보겠습니다.

<br>

### 1) Path Variable vs Query Parameter

 `Path Variable`은 이름에서 알 수 있듯이 경로를 변수로서 사용합니다. 

```
/user/123 # 123번 유저를 검색
```

 `Query Parameter`는 경로 뒤에 입력 데이터를 함께 제공하는 방식입니다.

```
/user?user_id=123
```

 스프링에서는 `Path Variable`은 `@Pathvariable` 애노테이션을 통해 바로 값을 받을 수 있고, `Query Parameter`는 `HttpServletRequest`객체를 통해 값을 읽어올 수 있습니다.

 만약에 123번 유저가 없다면, 존재하는 데이터가 없으므로 `Path Variable`은 404에러가 발생합니다. `Query Parameter`는 개발자가 값이 없다는 것을 확인하고 에러 핸들링을 통해 에러를 처리해야됩니다.

 이처럼 어떤 자원을 식별해야하는 상황에서는 `Path Variable`이 더 적합하고, `Query Parameter`는 자원의 식별이 아닌 자원의 정렬, 또는 필터를 원할 때 사용해야합니다. ( If you want to identify a resource, you should use Path Variable. But if you want to sort or filter items, then you should use query parameter. )