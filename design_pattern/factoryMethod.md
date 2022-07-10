# 팩토리 메서드

- 공통된 화면, 중복된 기능을 인터페이스로 청사진을 만들고 팩토리에서 객체를 생성해서 반환하는 패턴
- 공통된 화면의 추가 삭제가 편하다
- 생성된 클래스가 런타임시에 결정된다.
- NSNumber 가 팩토리 메서드로 되어있다
- 클래스 간의 결합도를 낮추기 위해 사용되고 클래스의 변경점이 생겼을 때 다른 클래스에 영향을 많이 주지 않게 한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bf019142-adec-4658-b948-1a6b78e801de/_2019-10-13__10.15.56.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bf019142-adec-4658-b948-1a6b78e801de/_2019-10-13__10.15.56.png)

- Product: 팩토리 메서드로 생성될 객체의 공통 인터페이스
- ConcreateProduct: 객체의 공통 인터페이스를 상속받아 구체적인 객체를 구현
- Creator: 팩토리 메서드를 갖는 클래스
- ConcreateCreator: 팩토리 메서드를 구현하는 클래스로 ConcreateProduct 객체를 생성
