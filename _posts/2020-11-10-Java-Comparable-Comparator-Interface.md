---
title: "[Java] Comparable, Comparator Interface"
last_modified_at: 2020-11-10T23:27
categories: 
  - java
tags: 
  - 'Java' 
  - 'comparable' 
  - 'comparator'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

Java에서 정렬을 하다 보면 `Comparable`과 `Comparator`를 자주 마주치게 된다.


**`Comparable`, `Comparator`하면 '정렬'**을 떠올려야한다.



**`Comparable`, `Comparator`은 Java에서 객체의 정렬 기준을 명시하는 두 가지 방법**이다.


<br>

Compare 하면 "비교", Comparable하면 "비교 가능한"으로 해석할 수 있어 "비교"가 먼저 떠오르지만,

**명확하게는 "정렬할 때 비교가 가능하다.", "정렬할 때 기준을 정할 수 있다."로 해석해야 한다.**
_ Comparable은 일반적인 정렬, Comparator는 특정한(customize) 정렬에 사용되고 "비교"가 아니다_

<br>


**`Comparable`, `Comparator`를 사용하는 이유**는 Java에서 **객체를 정렬할 때는 일정한 기준이 필요**한데, **`Comparable`과 `Comparator`를 활용해서 객체 정렬 기준을 정해주기 위해서다.**


**`Comparable`과 `Comparator`의 공통점**은 **객체 정렬의 기준을 정의한다는 것**이고,
**차이점**은 **정렬 기준이 일반적이냐 일반적이지 않냐**와 **`Object o1.compareTo(Object o2)` method를 구현하느냐 `compare(Object o1, Object o2)` method를 구현하느냐**가 전부다.

<br>

---
# Comparable

`Comparable`은 정렬 수행시 기본적으로 적용되는 정렬 기준이 되는 method를 정의해 놓는 Interface이다.

**객체 간의 일반적인 정렬이 필요할 때**, **`Comparable Interface`를 활용**해서 **정렬의 기준을 정의하는 `compareTo()` method를 구현**한다.

`Comparable interface`를 implements 한 뒤, 내부에 있는 `compareTo` method를 원하는 정렬 기준대로 구현하여 사용할 수 있다.


패키지 구조는 `java.lang.Comparable`로 되어 있다.

Java에서 제공되는 **정렬이 가능한 Class들은 모두 `Comparable` Interface를 구현**하고 있으며, **정렬시에 Comparable의 구현 내용에 맞춰 정렬이 수행**된다.
_Integer, Double 등의 클래스의 경우 비 내림차순(오름차순과 유사), String 클래스의 경우 사전순으로 정렬되게 구현_

즉, **`Comparable`은 Class의 기본 정렬 기준을 설정하는 Interface로 활용**된다.



# Comparator


`Comparator`는 정렬 가능한 Class(Comparable이 구현된 Class)들의 기본 정렬 기준과는 다른 방식으로 정렬하고 싶을 때 사용하는 Class이다.

**객체 간의 특정한 정렬이 필요할 때**, **`Comparator Interface`를 활용**해서 **특정 정렬 기준을 정의하는 `compare()` method를 구현**한다.

패키지 구조는 `java.util.Comparator`로 되어 있다.


활용 방법은 **`Comparator` Class를 생성**하여, 내부에 **compare() method를 원하는 정렬 기준대로 구현하여 사용**할 수 있다.
_주로 익명클래스(new Comparator(){ ... })로 사용되며, 기본적으로 오름차순이 정렬 기준인 것을 내림차순으로 정렬하는 등의 용도로 사용_


즉, **`Comparator`는 기본 정렬 기준과 다르게 정렬하고 싶을 때 사용하는 Class로 활용**된다.



<br>

---






# Comparable, Comparator 활용 예제


```java

import java.lang.Comparable;
import java.util.Arrays;
import java.util.Comparator;

public class ComparableAndComparatorExample {

    // Comaprable interface를 implements 해준다.
    // 이때 제너릭 타입은 Student Class를 설정 한다.
    static class Student implements Comparable<Student> {
        String name;
        int stuNo;
        double score;

        Student(String name, int stuNo, double score) {
            this.name = name;
            this.stuNo = stuNo;
            this.score = score;
        }

        // compareTo method를 override해준다.
        // 학번을 기준으로 정렬한다.
        @Override
        public int compareTo(Student compareStudent) {
            // 학번을 통한 정렬, 오름 차순
            // 앞에가 크면 1, 같으면 0, 작으면 -1 return
            int result = Integer.compare(stuNo, compareStudent.stuNo);
            return result;
        }
    }

    public static void main(String[] args) {
        Student student[] = new Student[4];

        student[0] = new Student("길록", 20140151, 3.5);
        student[1] = new Student("이후배", 20150001, 4.5);
        student[2] = new Student("강선배", 20110001, 3.5);
        student[3] = new Student("홍회장", 20130001, 2.8);

        // 정렬 전 출력 순서는 입력 순서대로 나온다.
        for (Student stu : student) {
            System.out.println("이름 : " + stu.name + ", 학번 : " + stu.stuNo + ", 학점 : " + stu.score);
        }

        System.out.println();

        // Quick Sort
        // Student Class는 Comparable interface에서 compareTo method를 구현해주어
        // 학번순으로 정렬해준다.

        // 만약 Student Class가 Comparable을 implements 해주지 않고 적용하려면 complie error가 발생한다.
        // Arrays.sort()는 compareTo method를 호출해서 정렬을 사용한다.
        Arrays.sort(student);

        // 출력 결과로 학번 순으로 정렬해준다.
        for (Student stu : student) {
            System.out.println("이름 : " + stu.name + ", 학번 : " + stu.stuNo + ", 학점 : " + stu.score);
        }

        System.out.println();

        // comparator Class를 익명 Class로 구현하여
        // 정렬 기준을 Customize 해준다.
        // 학점을 통해 정렬을 다시 수행해준다.
        Arrays.sort(student, new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                double s1Score = s1.score;
                double s2Score = s2.score;
                if (s1Score == s2Score) { // 학점이 같으면
                    return Double.compare(s1.stuNo, s2.stuNo); // 학번 오름차순
                }
                return Double.compare(s2Score, s1Score);// 학점 내림차순
            }
        });

        // 출력 결과로 학번 순으로 정렬된 데이터에
        // 학점 정렬을 추가로 수행하여 출력한다.
        for (Student stu : student) {
            System.out.println("이름 : " + stu.name + ", 학번 : " + stu.stuNo + ", 학점 : " + stu.score);
        }
    }
}
```

#### 출력 결과

```
이름 : 길록, 학번 : 20140151, 학점 : 3.5
이름 : 이후배, 학번 : 20150001, 학점 : 4.5
이름 : 강선배, 학번 : 20110001, 학점 : 3.5
이름 : 홍회장, 학번 : 20130001, 학점 : 2.8

이름 : 강선배, 학번 : 20110001, 학점 : 3.5
이름 : 홍회장, 학번 : 20130001, 학점 : 2.8
이름 : 길록, 학번 : 20140151, 학점 : 3.5
이름 : 이후배, 학번 : 20150001, 학점 : 4.5

이름 : 이후배, 학번 : 20150001, 학점 : 4.5
이름 : 강선배, 학번 : 20110001, 학점 : 3.5
이름 : 길록, 학번 : 20140151, 학점 : 3.5
이름 : 홍회장, 학번 : 20130001, 학점 : 2.8
```







<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[자바 Comparable, Comparator 하면 '정렬'을 떠올려라, 자바 객체 정렬의 '기준'을 정하는 방법![기본기를 쌓는 정아마추어 코딩블로그]](https://jeong-pro.tistory.com/173)

[[Java] Comparable와 Comparator의 차이와 사용법[heejeong Kwon]](https://gmlwjd9405.github.io/2018/09/06/java-comparable-and-comparator.html)

[[정렬] Comparable과 Comparator[코딩과 디버깅 사이]](https://m.blog.naver.com/occidere/220918234464)

[]()

[]()

[]()


