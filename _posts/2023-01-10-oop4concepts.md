---
title: "OOP의 4가지 특징"
excerpt: "객체지향 프로그래밍 OOP의 4가지 중요한 특징"

categories: OOP
tags:
    - [oop]

toc: true
toc_sticky: true
author_profile: false
sidebar:
    nav: "docs"

date: 2023-01-10
last_modified_at: 2023-01-10
---







OOP 객체지향 프로그래밍의 4가지 핵심 개념에 대해 설명하려고 합니다.

---------------------------------------------------------------------------


# 1. 캡슐화 (Encapsulation)

캡슐화는 데이터, 그리고 그 데이터를 활용하는 함수를 캡슐 혹은 컨테이너 안에 두는 것을 의미한다.

여기서 캡슐은 class다.

유니티에서 스크립트 하나를 만들고 다음과 같이 적어보자.

```c#
public class HelloWorld : MonoBehaviour
{
    private void Start()
    {
        var elon = new Entrepreneur("elon", "musk", 1700000, "TSLA");
        var v = elon.CompanyValue();
        print(v);
    }
}

public class Entrepreneur
{
    private string _firstName;
    private string _lastName;
    private int _money;
    private string _company;

    public Entrepreneur(string firstName, string lastName, int money, string company)
    {
        _firstName = firstName;
        _lastName = lastName;
        _money = money;
        _company = company;
    }

    public int CompanyValue()
    {
        return _money * _company.Length;
    }
}
```
Entrepreneur 클래스안을 보면
```c#
private string _firstName;
private string _lastName;
private int _money;
private string _company;
```
```c#
public int CompanyValue()
{
    return _money * _company.Length;
}
```
이 두 부분은 서로 연관되어 있다. 그래서 Entrepreneur클래스로 묶어주었다.

캡슐화를 사용하면 클래스의 표시할 속성과 숨길 속성을 선택할 수 있다. 

이 경우 모든 필드가 private이기에 외부에서 접근할 수 없다. 그래서 아래와 같이 사용할 수 없다.
```c#
private void Start()
{
    var elon = new Entrepreneur("elon", "musk", 1700000, "TSLA");
    elon._money = 1000;
    elon._company = "AMAZON";
}
```
캡슐화는 어떻게 클래스 정보에 접근, 수정하는지를 결정하는 권한을 제공한다.
예를 들어, 이 기업가의 이름을 공개하고자 한다면
```c#
public string GetName()
{
    return _firstName + _lastName;
}
```
이를 위한 메소드를 만들어야 한다.

정리해보면 데이터와 그 데이터를 이용하는 함수를 class 안에 잘 정리하는 방법론이다. 덕분에 원하는 데이터는 숨길 수 있고 노출할 자료와 숨길 자료를 선택할 수 있다. 


# 2. 상속 (Inheritance)

상속 덕분에 코드를 더 작은 단위 Class로 쪼개고, 더 작은 단위로 나누고, 재사용 할 수 있다.

앞서 만든 기업가 클래스처럼 배우 클래스가 필요하다고 해보자.

```c#
public class Entrepreneur
{
    private string _firstName;
    private string _lastName;
    private int _money;
    private string _company;
}

public class Actor
{
    private string _firstName;
    private string _lastName;
    private int _oscars;
    private string _age;
}
```
배우도 기업가처럼 이름이 필요하다. 같은걸 두번이나 써줘야 하는거다. 그닥 좋아보이진 않는다. 
여기서 상속을 이용하면 어떻게 될까?
```c#
public class Person
{
    protected string _firstName;
    protected string _lastName;

    public void SayHi()
    {
        Debug.Log("Hi");
    }
}

public class Entrepreneur : Person
{
    private int _money;
    private string _company;
}

public class Actor : Person
{
    private int _oscars;
    private string _age;
}
```
Person클래스로 공통된것을 묶어줬다. 이렇게 하면 Entrepreneur,Actor클래스는 따로 써주지 않아도 Person의 속성을 가지게 된다.
그리고 함수가 필요하다면
```c#
private void Start()
{
    var elon = new Entrepreneur();
    elon.SayHi();

    var tom = new Actor();
    tom.SayHi();
}
```
이렇게 둘 다 SayHi를 할 수 있게 된다. 

# 3. 추상화 (Abstraction)

c++의 아버지 Bjarne Stroustrup는 추상화를 '구현 세부 정보를 숨기는 일반 인터페이스를 지정하는 행위'로 정의했다고 한다.

예를 들어, 차량을 운전할때 이를 위한 인터페이스를 사용한다. 여기서 인터페이스는 핸들, 페달, 버튼들 등등.. 이것들은 차량 제조사에 의해 노출 되어있다. 우리는 해당 인터페이스만으로 차량을 조종할 수 있다. 하지만 구현 세부 정보는 노출되어 있지 않아서 운전자는 브레이크나 엔진이 어떻게 작동하는지 몰라도 운전을 할 수 있다. 

```c#
public class BetterArray<T>
{
    private readonly T[] _array;
    private readonly int _index;

    public BetterArray(int capacity)
    {
        _array = new T[capacity];
        _index = 0;
    }
    public void AddItem(int i, T item)
    {
        if (i > _array.Length)
        {
            Debug.Log("인덱스가 배열 크기보다 크다");
            return;
        }

        if (i < 0)
        {
            Debug.Log("인덱스가 0보다 작습니다");
            return;
        }

        _array[_index + i] = item;
    }
    public int Length() => _array.Length;
}
```
누군가 "BetterArray"를 사용하게 된다면 노출된 인터페이스를 사용해 배열과 상호작용할 수 있다.
쓰는 사람은 각 메소드의 구현 세부정보를 알 필요가 없다. 그래서 다음과 같이 사용할 수 있다. 
```c#
private void Start()
{
    var a = new BetterArray<int>(10);
    a.AddItem(1, 5);
    print(a.Length());
}
```
또 다른 큰 장점은, 만약 더 빠른 속도를 위해, AddItem()메소드를 수정한다면 이걸 사용하는 쪽에서는 뭔가 바꿀 필요가 없다. 왜냐면 내부의 내용이 바뀌어도 인터페이스는 그대로 유지되기 때문이다.

# 4. 다형성 (Polymorphism)

"Poli"는 여러개를 뜻한다. "morphos"는 형태, 모양을 뜻한다.
따라서 polymorphism은 여러개의 형태 라는 뜻이다.

polymorphism을 이해하기 위해서는 '상속'에서 부모클래스의 속성이 자식 클래스에게 상속되는 것을 기억해야 한다. 

예를 들어보자. Korean, Canadian 클래스는 Person 클래스를 상속 받는다.
Person에는 SayHi 메소드가 있다. 이 때 Korean과 Canadian은 서로 인사 방식이 다르다.
```c#
public class Person
{
    public virtual string SayHi()
    {
        return "Hi";
    }
}

public class Korean : Person
{
    public override string SayHi()
    {
        return "안녕";
    }
}

public class Canadian : Person
{
    public override string SayHi()
    {
        return "Hello";
    }
}
```
이렇게 하고 Korean과 Canadian 객체를 생성해 SayHi를 호출하면 같은걸 상속받고 같은 이름의 메소드를 실행했지만 결과는 서로 다르게 나온다. 만약 부모 클래스와 같은 결과를 내고 싶다면
```c#
public override string SayHi()
{
    base.SayHi();
}
```
이렇게 적어주면 된다. 그리고 자식과 부모의 SayHi메소드는 반환값이 같아야한다. 
이것이 OOP의 다형성이다. 보다시피, 부모의 메소드를 오버라이딩 할 수 있는데 그럼에도 메소드가 어떻게 작동해야하는지 규칙이 정해져 있다.
덕분에 클래스의 핵심은 그대로 있고, 구현방식은 달라지게 되는 것이다. 다형성을 이용하면, 한국인과 캐내다인 클래스가 서로 다른 언어로 인사할 수 있게 된다.

긴 글 읽어 주셔서 감사합니다.