---
layout: post
title:  "코딩의 기술 정리"
date:   2019-01-21 15:38:02 +0900
---

### 1. 읽기 좋은 코드를 작성하는 기술
* 1.1 읽기 좋은 코드란?
	- 1.1.1 코드의 보수성
		1. 개발 현장의 코드는 제대로 작동할 뿐만 아니라, 기능 추가 또는 지속적인 변경에도 잘 견딜 수 있어야 함
	- 1.1.2 읽기 좋은 코드를 작성하는 요령
		1. 코드의 의도를 명확하게 전달하자
		2. 복잡한 코드 문제를 작게 나눈다
		3. 읽기 좋은 이름을 붙여 정리하자
	
* 1.2 변수와 상수
	- 1.2.1 의미가 명확한 변수 이름 붙이기
		1. 변수에 의미 있는 이름을 붙히는 것은 코드 가독성 향상의 기본
		2. 사용 목적을 가능한 구체적으로 입력
	- 1.2.2 매직 넘버에 이름 붙이기
		1. 매직 넘버 : 코드 작성자만 알 수 있는, 코드 내부에 들어 있는 상수값
		2. 매직 넘버로 특정 상태를 구분하면 코드 전체의 흐름을 다른 사람이 이해하기 어렵다
		3. 열거형을 사용해서 매직 넘버에 이름을 붙히자

* 1.3 조건식과 계산식
	- 1.3.1 읽기 좋은 조건식을 작성하는 요령
		1. 복잡한 조건식을 구성하는 경우에는 '설명 전용 변수'를 사용하라(지역 변수를 사용해 조건식에 이름을 붙이는 방법)
		~~~
		const bool isJump = y > 0.0f;
		const bool isDamage = state == STATE_DAMAGE;
		const bool isDash = (speed >= 10.0f) && !isJump && !isDamage;
		if (isDash) {

		}
		~~~
		2. 조건식을 함수화하라
		3. 조건식을 함수화하면 여러곳에서 재사용할 수 있으며 단위 테스트도 간단해진다
	- 1.3.2 읽기 좋은 계산식을 작성하는 요령
		1. 조건식과 마찬가지로 계산식에도 설명 전용 변수를 사용하면 가독성이 향상
		2. 계산식을 함수화하라

* 1.4 assert 활용
	- 1.4.1 assert 사용 방법
		1. assert는 매개 변수의 조건식이 false인 경우 오류 메시지를 표시하고 프로그램을 중지시키는 매크로로, 디버그를 지원하기 위한 기능
		2. 주로 매개 변수와 계산 결과의 타당성 체크 등에 활용

* 1.5 제어문
	- 1.5.1 if 조건문 감축과 단순화
		1. if 조건문은 하나씩 추가할 때마다 실행 경로가 증가
		2. 실행 경로는 일직선으로 구성되는 것이 가장 이상적
		3. 따라서 어떻게 해야 if 조건문을 사용하지 않고 코드를 구성할지 생각하자
		4. 하한값, 상한값 체크 단순화
		~~~
		if(x > 10) {
			x = 10;
		}
		STL의 min함수를 사용하면 다음과 같이 개선 가능
		x = std::min(x, 10);

		if( x > 10) {
			x = 10;
		} else if(x < 0) {
			x = 0;
		}
		min함수와 max함수를 함께 사용해서 개선 가능
		x = std::min(std::max(x, 0), 10);
		~~~
		5. 랩 어라운드의 함수화 
		6. 랩 어라운드란, 어떤 숫자가 상한값에 이르면 하한값으로 돌려주고 다시 계산하는 것
		7. 하한값에서 상한값 사이의 숫자를 반복하고 싶을 때 사용
		~~~
		if( x >= 10) {
			x = 0;
		}
		if 조건문 대신 나머지 연산자(%)를 사용
		x = x % 10;
		~~~
		8. 나머지 연산만으로는, 하한값 아래로 내려갔을 경우 상한값으로 바꾸는 마이너스 방향의 랩 어라운드는 불가능
		9. 아래의 방법으로 마이너스 방향의 랩 어라운드도 가능
		~~~
		if (x >= 10) {
			x = 0;
		} else if(x < 0) {
			x = 9;
		}

		int wrap(int x, int low, int high) {
			assert(low < high);
			const int n = (x - low) % (high - low);
			return (n >= 0) ? (n + low) : (n + high);
		}
		~~~
		10. 조기리턴 활용 : 함수의 입구에서 예외 조건을 확인하고, 조기에 리턴하는 것
		~~~
		void update() {
			if(health > 0) { // 체력이 있는지 확인
				if(lifeTime > 0) { // 수명이 남아있는지 확인
					// 살아있을 때의 처리
				}
			}
		}

		조기 리턴을 사용한다면
		void update() {
			if(health <= 0) return; // 체력이 없다면 리턴
			if(lifeTime <= 0) return; // 수명이 없다면 리턴
			// 살아있을 때의 처리
		}
		~~~
		11. 다음 예처럼 1개의 조건식 결과를 리턴하는 것뿐이면 if 조건문을 사용할 필요조차 없다
		~~~ 
		bool isDead() {
			if (health <= 0) return true;
			return false;
		}

		int bonus(int time) {
			if (time < 10) return 1000;
			return 0;
		}
		
		조건식을 bool로 리턴할 때는 조건식을 바로 return에 입력하고, 숫자로 리턴할 때는 삼항 연산자를 사용
		bool isDead() {
			return health <= 0; // 조건식의 결과를 bool로 리턴
		}

		int bonus(int time) {
			return (time < 10) ? 1000: 0; // 값의 경우는 삼항 연산자 사용
		}
		~~~
		12. 중첩된 여러 개의 if 조건문 내부에서 조건이 중복된다면, 중복이 일어나는 대상을 먼저 판정하라
		13. 조건식이 직접 관계하는 부분만 따로 떼어내서 국소화하라
		~~~
		if (isDash()) { // 대시 중인지 확인
			position += direction * 10.0f; // 대시 중에는 속도가 2배
		} else {
			position += direction * 5.0f;
		}

		속도 변화 부분을 따로 뺀다
		float speed = 5.0f;
		if (isDash()) { // 대시 중인지 확인
			speed = 10.0f; // 대시 중에는 속도가 2배
		}
		position += direction * speed;

		추가로 speed 연산 부분을 함수화
		float speed() {
			if (isDash()) return 10.0f;
			return 5.0f;
		}

		삼항 연산자를 사용하면 더 간단하다
		float speed() {
			return isDash() ? 10.0f: 5.0f;
		}

		속도 수치를 나타내는 매직 넘버에 이름을 붙힌다
		float speed() {
			return isDash() ? SPEED_DASH: SPEED_NORMAL;
		}
		position += direction * speed();
		~~~
		14. 배열을 활용한 if 조건문 제거
		~~~ 
		int id_to_num(int id) {
			if (id == 0) return 10;
			if (id == 1) return 15;
			if (id == 2) return 30;
			if (id == 3) return 50;
			assert(!" 부정확한 ID");
			return 0;
		}

		변환 전용 배열을 사용하면 if 조건문을 제거할 수 있다
		int id_to_num(int id) {
			static const int table[] = {10, 15, 30, 50};
			assert((0 <= id && id <= 3) && " 부정확한 ID");
			return table[id];
		}

		반대 변환도 가능
		int num_to_id(int num) {
			static const int table[] = {10, 15, 30, 50};
			for (int i = 0; i < 4; ++i) {
				if (table[i] == num)
					return i;
			}
			assert(! "부정확한 숫자");
			return 0;
		}
		~~~
		15. 복잡한 if 조건문을 간단하게 만드는 결정표를 사용하라
		~~~
		enum Hand {
			Rock 		= 0, // 바위
			Scissors 	= 1, // 가위
			Paper		= 2  // 보
		};

		enum Result {
			Win,	// 승리
			Lose,	// 패배
			Draw	// 무승부
		};

		// 가위바위보 판정
		Result judgement(Hand my, Hand target) {
			if (my == target) return Draw;
			if (my == Rock		&& target == Scissors) return Win;
			if (my == Scissors	&& target == Paper) return Win;
			if (my == Paper		&& target == Rock) return Win;
			return Lose;
		}

		결정표를 사용하면 if 조건문 없이도 승패 판정이 가능
		Result judgement(Hand my, Hand target) {
			// 가위바위보 판정
			static const Result result[3][3] {
				// 바위 가위 보(상대방)
				{Draw, Win, Lose},	// 바위(자신)
				{Lose, Draw, Win},	// 가위
				{Win, Lose, Draw}	// 보
			};
			return result[my][target];
		}
		~~~
		16. null 객체 도입 : null 객체란 null 포인터를 대신하는 더미 객체
		~~~
		객체의 존재를 확인하는 if 조건문이 많을 때 유용하게 사용할 수 있는 기술
		// 플레이어가 존재하는지 확인
		if (player != nullptr) {
			// 존재한다면 이동
			player->move();
		}
		... 생략
		if (player != nullptr) {
			// 존재한다면 렌더링
			player->draw();
		}

		이 기술을 사용하려면, 대상 클래스에 부모 클래스가 있어야 하며 모든 멤버 함수가 가상 함수여야 한다
		class Actor {
		... 생략
			// 이동
			virtual void move() = 0;
			// 렌더링
			virtual void draw() = 0;
		};

		null 포인터를 대신하는 null 객체 클래스를 작성한다
		// null 액터 클래스
		class NullActor : public Actor {
			... 생략
			// 이동
			virtual void move() override {} // 아무것도 하지 않음
			// 렌더링
			virtual void draw() override {} // 아무것도 하지 않음
		};
---
