---
title: "[Java] Stream"
last_modified_at: 2020-11-15T23:43
categories: 
  - java
tags: 
  - 'Java' 
  - 'stream'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ 참고사이트 🙇‍♂️

[Java Stream 기초 설명 (코딩 테스트 예제로)[사용자 홍아지]](https://gofnrk.tistory.com/77)

[프로그래밍언어-자바-10. 스트림 API[짧굵배::온라인 프로그래밍 강좌]](https://dinfree.com/lecture/language/112_java_10.html)

[Java Stream API[kskim.log]](https://velog.io/@kskim/Java-Stream-API)


<br>

# Stream?
**`Stream`**은 **Java 8에서 추가**되었고, **기존 Java I/O에서 나오는 InputStream, OutputStream과는 다른것**으로 **함수형 프로그램에서 단계적으로 정의된 계산을 처리하기 위해 Interface**이다.


**`Stream`**은 **stream 형태의 요소에 함수형 연산자를 지원해주는 Class**로 **stream은 Array, Collections와 같이 연속된 형태의 객체**이다. 

<br>

**하지만 stream은 자료구조는 아니다. **위와 같은 **데이터를 입력으로 받아 method로 처리할 뿐**이다.

**stream은 입력받은 원래의 자료구조를 바꾸지는 못한다.** 

대신, **파이프라인 형태로 연결된 method의 결과를 제공**한다. 

**원본 데이터를 바꾸지 못하는 특성 덕분에 side effect를 제거**할 수 있다.

**최종 연산자(terminal operation)는 stream의 끝을 의미**하며 **모든 연산자를 수행한 결과를 반환**한다. 

**최종 연산자까지 모두 수행한 stream은 재활용 할 수 없다.**

아래는 Stream을 사용한 간단한 예제이다.

```java
// 스트림 생성 -> 중간 연산자 -> 최종 연산자
int result = list.stream()  // 스트림 생성
        .filter( ... )      // 중간 연산자
        .map( ... )         // 중간 연산자
        .count();           // 최종 연산자
```

<br>



**`Stream`**은 **데이터의 흐름**으로 **배열 또는 컬렉션 인스턴스에 함수를 조합**하여 **원하는 결과를 필터링하고 가공된 결과를 손쉽게 처리**할 수 있다. 

**Java I/O Program에서 말하는`Stream`과 연속된 데이터의 흐름이라는 관점에서는 비슷**하지만 **내용적으로는 다르다.**

**`Stream`은 데이터 소스를 추상화** 하고 있으므로 **데이터 소스에 상관없이 같은 방식으로 처리할 수 있다는 장점**이 있으며 데이터를 다루는데 자주 사용되는 method들을 정의해 두고 있어 **기존 방식보다 간결하고 유연한 구현이 가능**하다.


---


# Stream 연산 구조

`Stream`은 **어떻게(How)가 아니라 무엇(What)을 할것인지에 목적을 두고 사용**해야 하며 연산의 파이프 라인은 스트림 생성(Create) -> 중간연산(Intermediate operating) -> 최종연산(Final operation) 의 형태를 가지며 이들은 .를 이용한 메서드 체이닝(Method Chanining)으로 구현된다.

```
Collections 등 Object 집합.스트림생성().중간연산().최종연산();
```

**중간연산 method는** **return type이 stream**이므로** 계속해서 다른 stream method를 연결해 사용**할 수 있다.

**최종연산 method는** **return type이 stream이 아닌것**으로 **method 체이닝을 끝내는 역할**을 한다.

최종연산이 실행 되어야 중간연산도 처리되기 때문에 **중간연산들만으로 구성된 method 체인은 실행되지 않는다.**


---

# 기존 방식과 비교

기존 방식과 Stream을 사용한 방식을 비교해보면 아래와 같다.

### 기존방식

```java
//동일한 데이터를 가지는 배열과 리스트를 선언.
//각각 데이터 정렬을 위한 메서드를 통해 데이터를 정렬.
//정렬된 값을 확인하기 위해 출력문을 이용해 출력.
String[] strArr = { "data1", "data2", "data3" }
List<String> strList = Arrays.asList(strArr);


//데이터 정렬을 위해 각각 Arrays, Collections 의 sort 메서드를 이용
//정렬한 다음 for 문을 이용해 결과를 출력하는 형식.

Arrays.sort(strArr);
Collections.sort(strList);

for(String str : strArr) {
  System.out.println(str);
}

for(String str : strList) {
  System.out.println(str);
}
```


### Stream API 사용

```java
//데이터 소스(배열 혹은 리스트)로 부터 스트림을 생성.
//정렬을 위해 sorted() 메서드를 호출.
//출력을 위해 forEach() 메서드를 호출.

strList.stream().sorted().forEach(System.out::println);
Arrays.stream(strArr).sorted().forEach(System.out::println);

//여기에서 forEach는 void forEach(Consumer<? super T> action) 로 정의되어 있음.
//Cosumer 함수형 인터페이스를 인자로 가진다. 
//메서드 레퍼런스를 사용하지 않고 람다식으로 표현하면 아래와 같다.

strList.stream().sorted().forEach(x -> System.out.println(x));
```


# Stream API 제공 method

## 생성 method

Collections, Array, String, File등으로 부터 stream을 생성 할 수 있다.

### Empty Stream
비어 있는 스트림을 생성하기 위해서는 empty() 메서드를 사용한다.
```java
Stream<String> streamEmpty = Stream.empty();
```
### Arrays.stream()

배열로 부터 스트림을 생성하는 방법은 여러가지가 있다.

```java
Stream<String> arrayStream = Stream.of("a", "b", "c");
String[] arr = new String[]{"a", "b", "c"};
Stream<String> arrayFullStream = Arrays.stream(arr);
Stream<String> arrayPartStream = Arrays.stream(arr, 1, 3);
```

### Collections.stream()

자바 컬렉션 인터페이스를 사용하는 Collection, List, Set 는 stream() 메서드와 parallelStream() 메서드를 사용할 수 있다.
Map 의 경우 Key 혹은 Value 값만 리스트로 추출한 다음 스트림을 만들어 사용할 수 있다.

```java
Collection<String> collection = Arrays.asList("a", "b", "c");
Stream<String> collectionStream = collection.stream();

List<String> names = new ArrayList<>();
names.add("Kang");
names.add("Hong");
names.stream().forEach(System.out::println);
```

### String Stream
문자열을 다루는 클래스인 String, StringBuffer, StringBuilder는 문자열 시퀀스를 반환하는 chars() 메서드를 가지고 있는데 이를 통해 스트림을 생성하게 된다.

```java
IntStream charsStream = "abc".chars();
String str = "Hello World";
str.chars().filter(....)
```

### File Stream
파일의 경우 자바 NIO 의 Files클래스를 이용해 문자열 스트림 생성이 가능하다.
```java
Path path = Paths.get("C:/Tmp/testfile.txt");
Stream<String> streamOfStrings = Files.lines(path);
Stream<String> streamWithCharset = Files.lines(path, Charset.forName("UTF-8"));
```

### build()

```java
Stream<String> generatedStream = Stream.<String>builder()
        .add("Hello")
        .add("World")
        .build();
```

### generate()

크기를 지정하지 않으면 무한하기 때문에 특정 사이즈만큼 생성하려면 반드시 limit을통해 최대 크기를 제한해야 한다.
```java
Stream<String> generatedStream = Stream.generate(() -> "gen").limit(5);
```

### iterate()
초기 값을 시작으로 계속해서 2씩 증가된 값을 생성한다. 
generate()와 마찬가지로 크기를 지정하지 않으면 무한하기 때문에 limit을 통해 크기를 제한해야 한다.

```java
Stream<Integer> iteratedStream = Stream.iterate(30, n -> n + 2).limit(5);
```

### 병렬 스트림(Parallel Stream)
병렬 스트림은 내부적으로 fork & join 프레임웍을 이용해 자동적으로 연산을 병렬로 수행한다.
병렬처리를 구현하기 위해 개발자가 신경써야 하는 많은 부분을 해결할 수 있으며 스트림 생성시 parallel() 메서드를 실행 하기만 하면 된다. 
병렬 스트림 처리에서 병렬처리를 중단하려면 sequential()을 호출하면 된다.

```java
int sum = strStream.parallel()
                   .mapToInt(s -> s.length())
                   .sum();
```


### 기본 타입형 스트림
IntStream, LongStream, DoubleStream
**제네릭을 사용하지 않고 기본 값을 생성하는 방법**이다. 

제네릭을 사용하지 않기 때문에 불필요한 오토 박싱(auto-boxing)이 발생하지 않는다.

**range는 [startPosition, endPosition) 범위**를 가진다.

**rangeClosed는 [startPosition, endPosition] 범위**를 가진다.

```java
IntStream intStream = IntStream.range(1, 5); // [1, 2, 3, 4]
LongStream longStream = LongStream.rangeClosed(1, 5); // [1, 2, 3, 4, 5]
```

필요한 경우** boxed 메서드를 통해 Integer 형태로 박싱할 수 있다.**

```java
Stream<Integer> boxedIntStream = IntStream.range(1, 5).boxed();
```
난수 스트림을 생성할 수도 있다.

```java
DoubleStream doubles = new Random().doubles(3); // 난수 3개 생성
```

## 중간 연산 method

중간 연산의 대표적 유형과 method는 다음과 같다.

- 스트림 필터링 : filter(), distinct()
- 스트림 변환 : map(), flatMap()
- 스트림 제한 : limit(), skip()
- 스트림 정렬 : sorted()
- 스트림 연산 결과 확인 : peek()
- 타입변환: asDoubleStream(), asLongStream(), boxed()

여기에서 사용되는 예제들은 모두 다음과 같은 리스트 데이터를 사용한다고 가정 한다.

```java
List<Integer> intList = Arrays.asList(1,2,3);
List<String> strList = Arrays.asList("Hwang", "Hong", "Kang");
```

### filter(), distinct()
스트림 요소를 필터링 하기 위한 method 이다.

**filter()는 스트림 요소 마다 비교문을 만족(true)하는 요소로 구성된 스트림을 반환** 한다. 
즉, 특정 조건에 맞는 값만 추리기 위한 용도로 사용한다. 
**distinct() 는 요소들의 중복을 제거하고 스트림을 반환**한다.

```java
intList.stream().filter(x -> x<=2).forEach(System.out::println);  // 1,2
Arrays.asList(1,2,3,2,5).stream().distinct().forEach(System.out::println); // 1,2,3,5
```

### map()
스트림의 **각 요소마다 수행할 연산을 구현할때 사용**한다.

```java
intList.stream().map(x -> x*x).forEach(System.out::println); // 1,4,9
```

### flatMap()
**기존의 요소를 새로운 요소로 대체한 스트림을 생성**한다.

```java
Arrays.asList(intList,Arrays.asList(2,5)).stream()
	.flatMap(i -> i.stream())
	.forEach(System.out::println); // 1,2,3,2,5

strList.stream()
	.flatMap(message -> Arrays.stream(message.split("an")))
	.forEach(System.out::println);  // Hw, a, Hong, K, g
```
앞의 distinct() 예제에서 중복데이터 추가를 위해 Arrays.asList()를 사용했는데 flatMap()을 이용하면 다음과 같이 작성할 수 있다.

```java
Arrays.asList(intList,Arrays.asList(2,5)).stream()
	.flatMap(i -> i.stream())
	.distinct().forEach(System.out::println); // 1,2,3,5
```







### limit()
스트림의 시작 요소로 부터 인자로 전달된 인덱스 까지의 요소를 추출해 새로운 스트림을 생성한다.

```java
intList.stream().limit(2).forEach(System.out::println); // 1,2
```

### skip()
스트림의 시작 요소로 부터 인자로 전달된 인덱스 까지를 제외하고 새로운 스트림을 생성한다.

```java
intList.stream().skip(2).forEach(System.out::println); // 3
```

### sorted
스트림 요소를 정렬하는 method로 기본적으로 오름차순으로 정렬한다.

sorted() 를 활용하는 방법은 몇가지가 있는데 스트림 원소 객체가 Comparable 인터페이스를 구현하고 있는 상태라면 다음과 같이 하면 된다.

Comparable 인터페이스 구현은 오름차순이라고 가정한다.

```java
Arrays.asList(1,4,3,2).stream().sorted().forEach(System.out::println); // 1,2,3,4
Arrays.asList(1,4,3,2).stream().sorted((a,b) -> b.compareTo(a)).forEach(System.out::println); // 4,3,2,1
Arrays.asList(1,4,3,2).stream().sorted( (a,b) -> 
-a.compareTo(b)).forEach(System.out::println); // 4,3,2,1
```
두번째와 세번째 방법은 오름차순 구현을 활용해 내림차순으로 처리할 때 사용할 수 있는 방법 이다.
내림차순 정렬을 위한 또다른 방법은 -a.compareTo(b) 를 사용하는 것인데 직관적이지 못해 권장하지는 않는다.

정렬에 사용되는 또다른 방법은 Comparator를 사용하는 것으로 새로운 정렬 조건을 지정하고자 한다면 sorted((a,b) -> { })와 같이 코드를 작성하면 된다.
```java
Arrays.asList(1,4,3,2).stream().sorted( Comparator.reverseOrder()).forEach(System.out::println); // 4,3,2,1
```

### peak()

**결과 스트림의 요소를 사용해 추가로 동작을 수행**한다.

원본 스트림을 이용하는 것이 아니므로** 스트림 연산 과정에서 중간 중간 결과를 확인할 때 사용할 수 있다.** 

최종 연산인 forEach() 처럼 반복해서 요소를 처리하는 메서드 이며 **중간연산이므로 최종연산 메서드가 실행되지 않으면 지연되기 때문에 반드시 최종연산 메서드가 호출되어야 동작한다.**

앞의 filter() 예제를 보면 최종 연산으로 forEach() 를 이용해 출력하고 있다. 
**만일 최종 연산이 forEach() 가 sum() 이나 다른 최종연산이라면 값을 출력해볼 방법이 없다. **
그렇다고 변환된 컬렉션을 가지고 출력을 위해 다시 스트림 연산을 한다면 불편할 수 있다. 
**이 경우 peak() 이 유용하게 사용될 수 있다.**

```java
int sum = intList.stream().filter(x -> x<=2)
	.peek(System.out::println)
	.mapToInt(Integer::intValue).sum();
System.out.println("sum: "+sum);
```
                                            
                                            
                                            

## 최종 연산 method


최종연산은 결과값만 리턴되므로 별도의 출력문을 연결해 사용하기 어렵다. 

각 메서드 설명에 사용된 예제에서는 주석으로 결과를 표기 했으며 일부 가능한 경우만 직접 출력하고 있다.

- 요소의 출력 : forEach()
- 요소의 소모 : reduce()
- 요소의 검색 : findFirst(), findAny()
- 요소의 검사 : anyMatch(), allMatch(), noneMatch()
- 요소의 통계 : count(), min(), max()
- 요소의 연산 : sum(), average()
- 요소의 수집 : collect()

### forEach()
스트림의 요소들을 **순환 하면서 반복해서 처리해야 하는 경우 사용**한다.

```java
intList.stream().forEach(System.out::println); // 1,2,3
intList.stream().forEach(x -> System.out.printf("%d : %d\n",x,x*x)); // 1,4,9
```

### reduce()
**map 과 비슷하게 동작하지만 개별연산이 아니라 누적연산이 이루어진다는 차이**가 있다.

두개의 인자 즉 n, n+1 을 가지며 연산결과는 n 이 되고 다시 다음 요소와 연산을 하게 된다. 
즉 1,2 번째 요소를 연산하고 그 결과와 3번째 요소를 연산하는 식이다.

```java
int sum = intList.stream().reduce((a,b) -> a+b).get();
System.out.println("sum: "+sum);  // 6
```

### findFirst(), findAny()
두 메서드는 **스트림에서 지정한 첫번째 요소를 찾는 메서드**이다.

보통 filter() 와 함께 사용되고 **findAny() 는 parallelStream()에서 병렬 처리시 가장 먼저 발견된 요소를 찾는 메서드로 결과는 스트림 원소의 정렬 순서와 상관 없다.**

```java
strList.stream().filter(s -> s.startsWith("H")).findFirst().ifPresent(System.out::println);  //Hwang
strList.parallelStream().filter(s -> s.startsWith("H")).findAny().ifPresent(System.out::println);  //Hwang or Hong
```
**findAny()를 parralelStream()과 함께 사용하는 경우 일반적으로 findFirst()와 결과가 같다.**
_반드시 보장되는 것은 아님_
**parallelStream() 과 사용한 경우 실제 스트림 순서와는 다르게 선택될 수 있다.**
**findFirst() 와 findAny() 의 리턴값은 Optional 이므로 ifPresent()를 이용해 출력**한다.

### anyMatch(), allMatch(), noneMatch()
스트림의 요소중 특정 조건을 만족하는 요소를 검사하는 메서드이다.

원소중 일부, 전체 혹은 일치하는 것이 없는 경우를 검사하고 boolean 값을 리턴한다. 
noneMatch()의 경우 일치하는 것이 하나도 없을때 true를 리턴한다.

```java
boolean result1 = strList.stream().anyMatch(s -> s.startsWith("H"));  //true
boolean result2 = strList.stream().allMatch(s -> s.startsWith("H"));  //false
boolean result3 = strList.stream().noneMatch(s -> s.startsWith("T")); //true
System.out.printf("%b, %b, %b",result1,result2, result3);
```
### count(), min(), max()
**스트림의 원소들로 부터 전체 개수, 최소값, 최대값을 구하기 위한 메서드**이다.

min(), max() 의 경우 Comparator 를 인자로 요구 하고 있으므로 기본 Comparator들을 사용하거나 직접 람다 표현식으로 구현해야 한다.

```java
intList.stream().count();	// 3
intList.stream().filter(n -> n !=2 ).count(); 	// 2
intList.stream().min(Integer::compare).ifPresent(System.out::println);; 		// 1
intList.stream().max(Integer::compareUnsigned).ifPresent(System.out::println);; // 3

strList.stream().count();	// 3
strList.stream().min(String::compareToIgnoreCase).ifPresent(System.out::println);	// Hong
strList.stream().max(String::compareTo).ifPresent(System.out::println);	// Kang
```

### sum(), average()
**스트림 원소들의 합계를 구하거나 평균을 구하는 메서드**이다.

**reduce() 와 map()을 이용해도 구현이 가능**하다.
이 경우 **리턴값이 Optional이기 때문에 ifPresent()를 이용해 값을 출력**할 수 있다.

```java
intList.stream().mapToInt(Integer::intValue).sum();	// 6
intList.stream().reduce((a,b) -> a+b).ifPresent(System.out::println); // 6

intList.stream().mapToInt(Integer::intValue).average();	// 2
intList.stream().reduce((a,b) -> a+b).map(n -> n/intList.size()).ifPresent(System.out::println); // 2
```

### collect()
**스트림의 결과를 모으기 위한 메서드**로 **Collectors 객체에 구현된 방법에 따라 처리하는 메서드**이다.

최종 처리후 데이터를 변환하는 경우가 많기 때문에 자주 사용된다.
용도별로 사용할 수 있는 Collectors의 메서드는 기능별로 다음과 같다.

- 스트림을 배열이나 컬렉션으로 변환 : toArray(), toCollection(), toList(), toSet(), toMap()
- 요소의 통계와 연산 메소드와 같은 동작을 수행 : counting(), maxBy(), minBy(), summingInt(), averagingInt() 등
- 요소의 소모와 같은 동작을 수행 : reducing(), joining()
- 요소의 그룹화와 분할 : groupingBy(), partitioningBy()
```java
strList.stream().map(String::toUpperCase).collect(Collectors.joining("/"));	 	// Hwang/Hong/Kang
strList.stream().collect(Collectors.toMap(k -> k, v -> v.length()));	// {Hong=4, Hwang=5, Kang=4}

intList.stream().collect(Collectors.counting());
intList.stream().collect(Collectors.maxBy(Integer::compare));
intList.stream().collect(Collectors.reducing((a,b) -> a+b));	// 6
intList.stream().collect(Collectors.summarizingInt(x -> x));	//IntSummaryStatistics{count=3, sum=6, min=1, average=2.000000, max=3}

Map<Boolean, List<String>> group = strList.stream().collect(Collectors.groupingBy(s -> s.startsWith("H")));
group.get(true).forEach(System.out::println);  // Hwang, Hong

Map<Boolean, List<String>> partition = strList.stream().collect(Collectors.partitioningBy(s -> s.startsWith("H")));
partition.get(true).stream().forEach(System.out::println);  // Hwang, Hong
```
- toMap() 을 사용해서 문자열 스트림의 값을 키로 하고 문자열의 길이를 값으로 하는 맵으로 변환.
- counting, maxBy, reducing 은 각각 count(), max(), reduce() 메서드와 동일한 결과.
- summarizingInt 는 IntSummaryStatistics 를 리턴하며 count, sum, min, average, max 값을 참조할 수 있음.
- groupingBy는 특정 조건에 따라 데이터를 구분해서 저장.
- partitioningBy는 특정 조건으로 데이터를 두그룹으로 나누어 저장.
