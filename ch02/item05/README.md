# CH 2. 객체 생성과 파괴

# 아이템5: 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라.

## Q1. 자원을 직접 명시하면 무슨 문제가 생기는가?

→ 클래스의 유연성, 재사용성이 매우 떨어지고 테스트 하기 어려운 클래스가 된다.

많은 클래스가 하나 이상의 자원에 의존하는데, 정적 유틸리티를 사용하거나, 싱글턴으로 구현을 한다.

### 자원 직접 명시

```java
// 정적 유틸리티를 잘못 사용한 예
public class SpellChecker {
	private static final Lexicon dictionanry = ...;

	private SpellChecker(){}

	public static boolean isValid(String word) {...}
	public List<String> suggestions(String typo) {...}

}
```

```java
// 싱글턴을 잘못 사용한 예
public class SpellChecker {
	private static final Lexicon dictionanry = ...;

	private SpellChecker(...){}
	public static SpellChecker INSTANCE = new SpellChecker(...);

	public static boolean isValid(String word) {...}
	public List<String> suggestions(String typo) {...}

}
```

- 이러한 방식은 여러 자원을 사용해야 하거나 사용해야 할 자원이 바뀌는 경우 수정이 어렵고 오류가 발생할 가능성이 높아진다.
- 또한 테스트 하기에도 어렵다는 단점이 있다.
- 따라서 사용하는 자원에 따라 동작이 달라지는 클래스에서 사용하기에는 적합하지 않다.

### 의존 객체 주입

- 의존 객체 주입은 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식이다.

```java
// 의존 객체 주입
public class SpellChecker {
	private final Lexicon dictionanry;

	private SpellChecker(Lexicon dictionanry){
		Assert.notNull(dictionary, "사전은 필수여야 합니다.");
		this.dictionary = dictionary;
	}

	public static boolean isValid(String word) {...}
	public List<String> suggestions(String typo) {...}

}
```

- 의존 객체 주입은 자원이 몇 개든 의존 관계가 어떻든 상관없이 잘 작동한다.
- 불변을 보장하여 여러 자원을 사용하려는 클라이언트가 의존 객체들을 안심하고 공유할 수 있다.
- 의존 객체 주입은 생성자, 정적 팩터리, 빌더 모두 똑같이 응용 가능하다.

### 정리

**의존 객체 주입 기법은 클래스의 유연성, 재사용성, 테스트 용이성을 매우 향상시킨다!!**
