- 상세화된 서브클래스를 정의하지 않고도 서로 관련성이 있거나 독립적인 여러 객체의 군을 생성하기 위한 인터페이스를 제공
- 인터페이스를 이용하여 서로 연관된, 또는 의존하는 객체를 구상 클래스를 지정하지 않고 생성

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/13ba97f8-fa86-4d2b-a764-f787fede90b8/_2019-10-12__8.05.30.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/13ba97f8-fa86-4d2b-a764-f787fede90b8/_2019-10-12__8.05.30.png)

- AbstractFactory: 개념적 제품에 대한 객체를 생성하는 연산으로 인터페이스를 정의
- ConcreateFactor: 구체적인 제품에 대한 객체를 생성하는 연산을 구현
- AbstractProduct: 개념적 제품 객체에 대한 인터페이스를 정의
- ConcreateProduct: 구체적으로 팩토리가 생성할 객체를 정의하고 AbstractProduct가 정의하는 인터페이스를 구현
- Client: AbstractFactory와 AbstractProduct 클래스에 선언된 인터페이스를 사용
