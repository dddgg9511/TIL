디자인 패턴

- 적절한 객체를 식별
- 올바른 크기의 클래스와 인터페이스를 정의
- 클래스간의 상속
- 클래스간의 관계

지금 당장 문제를 해결할수도 있어야 하지만 나중에 생길수 있는 문제나 추가된 요구 사항들도 수용할수 있도록 일반적이고 포괄적이야 함

패턴의 요소

1. 패턴 이름은 간단하지만 설계 문제와 해법을 담고 있음. 패턴의 이름을 정의해두면 문서에서 이 이름을 사용하여 설계의 의도를 표현할수 있고 개발자끼리 의사소통이 원활.
2. 문제는 언제 패턴을 사용하는지를 말하며 해결할 문제와 배경을 설명.
3. 해법은 설계를 구성하는 요소와 요소들간의 관계와 책임, 협력관계를 말함. 구체적인 설계나 구현이 아닌 다양한 경우에 적용할 수 있는 템플릿. 추상적인 설명을 제공하고 문제를 해결하기 위한 방법을 제공
4. 결과는 패턴을 사용한 결과와 장단점을 말함. 시간을 중요한 관점으로 볼지 공간을 중요한 관점으로 볼지, 효율을 중요한 관점으로 볼지 등에 따라 패턴을 선택해야하고 언어에 따라 차이가 있음
- 추상 팩토리
구체적인 클래스를 지정하지 않고 관련성을 갖는 객체들의 집합을 생성하거나 서로 독립적인 객체들의 집합을 생성할 수 있는 인터페이스를 제공하는 패턴