# chap01.자바 8을 눈여겨봐야 하는 이유

# background
```java
Collections.sort(inventory, new Comparator<Apple>() {
    public int compare(Apple a1, Apple a2){
        return a1.getWeight().compareTo(a2.getWeight());
    }
});
```
->
```java
inventory.sort(comparing(Apple::getWeight));
```

### java 8의 핵심 사항
1. 간결한 코드
2. 멀티 프로세서의 간단한 활용

### 어떻게?
1. stream API : 개발자가 고수준 언어로 동작을 표현하면, 구현(stream API)에서 최적의 저수준 실행 방법을 선택한다. synchronized 키워드 사용하지 않아도 됨.
2. 메서드에 코드를 전달하는 기법 : Behavior Parameterization(동작 파라미터화)
3. 인터페이스의 디폴트 메서드

### 자바의 진화
- C, C++의 낮은 안정성 때문에 런타임 footprint가 여유 있는 어플리케이션은 java, C# 등 사용
    - foortprint : 하드웨어나 소프트웨어 단위가 차지하고 있는 공간의 크기(http://www.terms.co.kr/footprint.htm)
- 특정 분야에서 장점을 가진 언어는 다른 경쟁 언어를 도태시킨다.
- java : 잘 설계된 객체지향 언어로 시작 + JVM
- 병렬 프로세싱의 필요 : 빅데이터, 멀티코어 컴퓨터
    -> java8

1. stream processing
    - 유닉스에서는 여러 명령을 벙렬로 실행
        - ex) cat file1 file2 | tr "[A-Z]" "[a-z]" | sort | tail -3
    - = java8의 stream : 기존에는 한 번에 한 항목을 처리했지만, stream은 작업을 고수준으로 추상화해서 일련의 스트림으로 만들어 처리할 수 있다.
        + 공짜로 병렬성을 얻을 수 있다.(3에서 계속)
2. Behavior parameterization
    - 기존에는 Comparator 객체를 만들어서 sort에 넘겨줌 : 복잡함
    - java 8에서는 메서드를 다른 메서드의 인수로 넘겨주는 기능 제공
3. 병렬성과 공유 가변 데이터
    - stream method로 전달하는 코드는 다른 코드와 동시에 실행하더라도 안전하게 실행될 수 있어야 한다. = 공유 가변 데이터에 접근하지 않아야 한다.
    - pure, side-effect-free, stateless function

### 함수형 프로그래밍 패러다임의 핵심적인 사항
- 공유되지 않은 가변 데이터
- 메서드와 함수 코드를 다른 메서드로 전달하는 기능
---
- 함수형 프로그래밍에서는 우리가 하려는 작업이 최우선시되며
- 그 작업을 어떻게 수행하는지는 별개의 문제로 취급

## java function
- java 8에서는 함수를 새로운 값의 형식으로 추가했다.
- 함수 자체가 값?
    - 값?
        - 기본값 : int, double 
        - instance : new 또는 factory method, 라이브러리 함수를 이용해서 객체의 값을 얻음("abc", new Integer(1), new HashMap<Integer, String>(100), new int[100])
    - 프로그래밍 언어의 핵심은 값을 바꾸는 것
    - 이 값을 first-class 값, 또는 first-class citizen이라 함.
    - 자바 프로그래밍 언어의 다양한 구조체(class, method)가 값의 구조를 표현하는데 도움을 줌 but, 전달할 수 없는 구조체는 이급 시민(=class, method)

### how to [method -> first-class citizen]
1. method reference
    ```java
    File[] hiddenFiles = new File(".").listFiles(new FileFilter() {
        public boolean accept(File file){
            return file.isHidden();
        }
    })
    ```
    : FileFilter로 isHidden을 복잡하게 감싼 다음 instance를 생성해 인자로 넘김
    ->
    ```java
    File[] hiddenFiles = new File(".").listFiles(File::isHidden);
    ```
    : isHidden이라는 함수(method가 아닌 function이라는 용어를 사용했다는 사실에 주목)를 method reference :: 를 이용해서 listFiles에 전달

    - named method 뿐만 아니라 lambda(anonymous functions)도 값으로 취급할 수 있음.
        - ex) (int x) -> x + 1
2. 코드 넘겨주기 예제
    ```java
    public static List<Apple> filterGreenApples(List<Apple> inventory){
        List<Apple> result = new ArrayList<>();
        for(Apple apple : inventory){
            if("green".equals(apple.getColor())){
                result.add(apple);
            }
        }
        return result;
    }
    public static List<Apple> filterGreenApples(List<Apple> inventory){
        List<Apple> result = new ArrayList<>();
        for(Apple apple : inventory){
            if(apple.getWeight() > 150){
                result.add(apple);
            }
        }
        return result;
    }
    ```
    ->
    ```java
    public static boolean isGreenApple(Apple apple) {
        return "green".equals(apple.getColor());
    }
    public static boolean isHeavyApple(Apple apple){
        return apple.getWeight() > 150;
    }
    public interface Predicate<T> {
        boolean test(T t);
    }
    static List<Apple> fileterApples(List<Apple> inventory, Predicate<Apple> p){
        List<Apple> result = new ArrayList<>();
        for(Apple apple: inventory){
            if(p.test(apple)){
                result.add(apple);
            }
        }
        return result;
    }
    
    filterApples(inventory, Apple::isGreenApple);
    filterApples(inventory, Apple::isHeavyApple);
    ```
    - predicate : 인수로 값을 받아 true/false를 반환하는 함수
3. 메소드 전달 -> lambda
    ```java
    filterApples(inventory, (Apple a) -> "green".equals(a.getColor()));
    filterApples(inventory, (Apple a) -> a.getWeight() > 150);
    filterApples(inventory, (Apple a) -> a.getWeight() < 80 || "brown".equals(a.getColor()));
    ```
    - 한번만 사용할 메서드는 따로 정의를 구현할 필요가 없다.
    - but, lambda가 복잡해진다면 코드가 수행하는 일을 잘 설명하는 이름(naming)을 가지는 method를 정의하고, method reference를 활용하는 것이 바람직
    - **코드의 명확성이 우선**
    ---
    - 멀티코어 CPU가 아니었다면 여기까지였을 것이다. 몇몇 일반적인 라이브러리 method를 추가하는 방향으로 발전했을 수도 있다.
        ```java
        filterApples(inventory, (Apple a) -> a.getWeight() > 150);
        ->
        filter(inventory, (Apple a) -> a.getWeight() > 150);
        ```
    - but, 병렬성이라는 중요성 때문에 이런 설계 X
    - 대신, java 8에서는 filter와 비슷한 동작을 수행하는 연산집합을 포함하는
    - Stream API을 제공

## stream
```java
Map<Currency, List<Transaction>> transactionsByCurrencies = new HashMap<>();
for (Transaction t : transactions) {
    // 고가의 transaction filter
    if(t.getPrice() > 1000){
        // transaction의 통화 추출
        Currency c = t.getCurrency();
        List<Transaction> transactionsForCurrency = transactionsByCurrencies.get(c);
        // 현재 통화의 그룹화된 맵에 항목이 없으면 새로 만듦
        if(transactionsForCurrency == null) {
            transactionsForCurrency = new ArrayList<>();
            transactionsByCurrencies.put(c, transactionsForCurrency);
        }
        // 현재 탐색된 transaction을 같은 통화의 transaction list에 추가
        transactionsForCurrency.add(t);
    }
}
->
import static java.util.stream.Collectors.toList;
Map<Currency, List<Transaction>> transactionsByCurrencies =
    transactions.stream()
                .filter((Transaction t) -> t.getPrice() > 1000)
                .collect(groupingBy(Transaction::getCurrency));
```
- for-each 루프를 이용해서 각 요소를 반복하면서 작업 수행 : external iteration(외부 반복)
- stream API를 사용하면 신경쓸 필요가 없음. 라이브러리 내부에서 모두 처리 : internal iteration(내부 반복)

### stream filter 동작
1. forking stop : 큰 스트림을 작은 스트림으로 분리
2. filter : 필터링
3. 결과 합침
> stream 뜻 자체가, 흐름. 병렬로 처리하기 위해서 큰 stream을 작은 stream으로 나누고
> 
>작게 나뉜 stream을 멀티 코어에 나누어 멀티 쓰레딩으로 처리하는 것일까?
- process : memory에 올라온 program
- thread : process 안의 실행 단위


- 컬렉션은 어떻게 데이터를 저장하고 접근할지에 중점을 두는 반면
- stream은 데이터에 어떤 계산을 할 것인지 묘사하는 것에 중점을 둔다.
    - 컬렉션을 필터링할 수 있는 가방 빠른 방법은
    - 컬렉션을 스트림으로 바꾸고, **병렬로 처리**한 다음에, 리스트로 다시 복원하는 것이다.

## default method
> interface에 method를 추가하면 그 interface를 구현한 모든 class에서 해당 method를 구현해야한다.
> 이를 해결하기 위해 default method 가 추가된 듯.

- interface 규격명세에 default 라는 새로운 키워드 지원
- diamond inheritance problems 피하는 방법은 -> 9장에서

## 함수형 프로그램에서 가져온 다른 유용한 아이디어
- switch -> structural pattern matching
> 아직 이건 잘 모르겠음
```scala
def simplifyExpression(expr: Expr): Expr = expr match {
    case BinOp("+", e, Number(0)) => e
    case BinOp("*", e, Number(1)) => e
    case BinOp("/", e, Number(1)) => e
    case _ => expr
}
```
> 이건 또 뭔 소리야 ㅋㅋ

