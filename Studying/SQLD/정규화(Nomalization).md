# 개념

- 정규화의 목표는 `테이블 간 중복된 데이터를 허용하지 않는 것`이다.

- 중복된 데이터를 만들지 않으면, 무결성을 유지할 수 있고, DB 저장 용량 또한 효율적으로 관리할 수 있다.

>1. 데이터의 중복을 없애면서 불필요한 데이터를 최소화시킨다.
>2. 무결성을 지키고, 이상 현상을 방지한다.
>3. 테이블 구성을 논리적이고 직관적으로 할 수 있다.
>4. 데이터베이스 구조를 확장에 용이해진다.

- 정규화에는 여러가지 단계가 있지만, 대체적으로 1~3단계 정규화까지의 과정을 거친다.


## 제 1정규화(1NF)

- 테이블 컬럼이 원자값(하나의 값)을 갖도록 테이블을 분리시키는 것을 말한다.

>1. 어떤 릴레이션에 속한 모든 도메인이 원자값만으로 되어 있어야한다.
>2. 모든 속성에 반복되는 그룹이 나타나지 않는다.
>3. 기본키를 사용하여 관련 데이터의 각 집합을 고유하게 식별할 수 있어야 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcJpNOf%2Fbtr0zSUXjY1%2Fab4ygSlzPCfc86x0rY3yPk%2Fimg.png)
- 위 테이블은 전화번호를 여러개 가지고 있어 원자값이 아니다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbzLvaN%2Fbtr0zFVCEI1%2FRcO7O4f7asnt1MH6PGntbK%2Fimg.png)
- 전화번호 컬럼을 1정규화한 모습


## 제 2정규화(2NF)

- 테이블의 모든 컬럼이 완전 함수적 종속을 만족해야 한다.

- 완전 함수적 종속을 만족한다는 말은, 테이블에서 기본키가 복합키(키1, 키2)로 묶여있을 때, 두 키 중 하나의 키만으로 다른 컬럼을 결정지을 수 있으면 안된다는 뜻이다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKzFJy%2Fbtr0qYvQCuO%2FcPnW9A03QOKbkPYnEBTbz1%2Fimg.png)

- Manufacturer과 Model이 키가 되어 Model Full Name을 알 수 있다.
- Manufacturer Country는 Manufacturer로 인해 결정된다. (부분 함수 종속)
- 따라서, Model과 Manufacturer Country는 아무런 연관관계가 없는 상황이다.(완전 함수적 종속 충족을 못함)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6RvAZ%2Fbtr0yis7uW8%2FY9iHl6U1RceXIgLvAlhigk%2Fimg.png)
- 연관 관계가 없는 컬럼을 다른 테이블로 2차 정규화!



## 제 3정규화(3NF)

- 2NF가 진행된 테이블에서 이행적 종속을 없애기 위해 테이블을 분리하는 것이다.
- 이행적 종속 : A → B, B → C면 A → C가 성립된다

> 릴레이션이 2NF에 만족한다.
> 기본키가 아닌 속성들은 기본키에 의존한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdlcMJw%2Fbtr0xLoPoRA%2Fn8famNYdQwEudKPAozi3Ak%2Fimg.png)
- 현재 테이블에서는 Tournament와 Year이 기본키다. Winner는 이 두 복합키를 통해 결정된다.
- 하지만 Winner Date of Birth는 기본키가 아닌 Winner에 의해 결정되고 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FejIGjG%2Fbtr0vde5UpJ%2FGwSr36xcQi8Oc4fBYt7tUk%2Fimg.png)
- 따라서 위와 같이 분리하고 조인을 통해 연결을 해줄 수 있다!
