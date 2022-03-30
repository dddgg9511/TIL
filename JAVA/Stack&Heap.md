# Stack과 Heap

![Untitled](https://user-images.githubusercontent.com/39615281/160877158-c8ccd88e-c216-49ff-b32f-46708d812a85.png)

# Stack

- Heap 영역에 생성된 Object 타입의 데이터의 참조값이 할당된다.
- 원시타입의 데이터가 값과 함께 할당된다.
- Stack에 존재하는 변수들은 scope에 따른 visibility를 가진다. 오직 활성화된 scope의 객체들만 사용이 가능하다.
- 각 Thread는 자신만의 stack을 가진다.

# Heap

- Jvm이 시작시에 생성되는 공간이다.
- 실제 객체가 저장된다.
- 모은 Object 타입은 heap 영역에 생성된다.
- Thread에 갯수와 상관없이 단 하나의 heap 영역만 존재한다.
