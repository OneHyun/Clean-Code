- [9장 단위 테스트](#단위 테스트)
    
    [TDD 법칙 세 가지](#TDD-법칙-세-가지)

    [깨끗한 테스트 코드](#깨끗한-테스트-코드)

    [테스트 당 assert 하나](#테스트-당-assert-하나)

- [오늘의 기억하고 싶은 내용](#오늘의-기억하고-싶은-내용)

***

## 단위 테스트

이전까지 단위 테스트란, 프로그램이 돌아간다는 사실만 확인할 용도의 일회성 코드였었다.

물론 지금이라면 꼬치꼬치 따지며 제대로 작동하는지 확인하고, 모두 구현하고 통과한 후에 코드를 공개하거나, 테스트 코드와 자신의 코드를 확실하게 묶을 수 있을 것이다.

이 둘의 차이는, 이 분야가 성장을 이룸에 따라, 급하게 서두름에 따라, 제대로 된 테스트 케이스를 작성해야 한다는 사실을 놓친 것에 대해 다루는 장이다.

### TDD 법칙 세 가지

TDD(테스트 주도 개발)는 실제 코드를 작성하기 전에, 단위 테스트부터 작성하도록 요구한다.

이것을 요구하는 까닭은, 테스트를 먼저 만들고, 만드는 과정에서 작성하고 통과하는 코드를 만드는 과정을 반복하면서, 피드백을 수용하고 결론적으로 작동하는 깔끔한 코드를 만들기 위해서이다.

이러한 TDD에는 세 가지 규칙이 존재한다.

- 첫째: 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.

- 둘째: 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.

- 셋째: 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

위 세 가지 규칙을 따를 경우, 테스트 코드가 실제 코드보다 먼저 나올 뿐더러, 실제 코드를 전부 테스트하는 테스트 케이스가 나오게 된다.

반면, 실제 코드와 맞먹을 양의 테스트 코드는 심각한 관리 문제를 야기한다는 문제점도 존재한다.

***

### 깨끗한 테스트 코드

테스트 코드가 일회성으로 사용될 때, 테스트 코드와 실제 코드에 동일한 품질 기준을 적용하지 말아야 하는 경우도 있었다.   '지저분해도 빨리'가 주였기 때문이다.

하지만, 테스트 코드를 깨끗하게 유지하는 것은 중요할 수 밖에 없다.   실제 코드가 진화하고 바뀔 경우, 거기에 맞추어 테스트 코드도 변경된다.

이런 상황에서 테스트 코드가 지저분할 경우, 실제 코드보다 테스트 코드에 시간이 더욱 할애된다.

새 버전을 출시할 때 마다, 테스트 케이스에 대한 유지 보수 비용은 늘어나고, 개발자 사이에선 불만이 늘어난다.

테스트 코드를 없애기에는, 테스트 코드의 부재는 시스템의 안정성을 입증하지 못하고, 결함율이 높아질 수 밖에 없다.   의도하지 않은 결함이 많아질 수록, 개발자는 변경을 주저하게 된다.

그렇기에, 우리는 테스트 코드를 실제 코드만큼 중요하다 라고 한다.

깨끗하게 유지하지 않으면, 실제 코드의 유연성, 유지 보수성, 재사용성을 담당해주던 것이 없어지기 때문이다.

#### 깨끗한 테스트 코드 유지하기

깨끗한 테스트 코드를 만들려면, 세 가지가 필요하다. 가독성, 가독성, 가독성.

이는 가독성의 중요성을 강조하는 것이다. 실제 코드보다 테스트 코드에는 가독성이 더더욱 중요하다.

즉, 읽는 사람을 고려해야 한다는 것이다.

***

### 테스트 당 assert 하나

JUnit으로 테스트 코드를 짤 때는 함수마다 assert문 하나만 사용해야한다 라고 주장하는 학파가 있다.   (JUnit은 자바용 유닛 테스트 프레임워크이다.)

물론 그것에 대해서도, assert 문이 하나여서 결론이 하나이기에, 이해가 쉽고 빠르다는 장점이 있다.

아래 예제를 보자

<pre>
<code>
public void testGetPageHierarchyAsXml() throws Exception{
    givenPages("PageOne", "PageTwo", "PageThree");
    
    whenRequestIsIssued("root", "type:pages");

    thenResponseShouldBeXML();
}

public voide testGetPageHierarchyHasRightTags() throws Exception {
    givenPages("PageOne", "PageTwo");

    whenRequestIsIssued("root", "type:pages");
    ...
}
</code>
</pre>

위 처럼 테스트 코드를 분리하면, 읽기가 쉬워진다.   반면, 보다시피 분리하였기에 중복되는 코드가 많아진다.

물론 디자인 패터을 적용하여 해결할 수도 있다.

TEMPLATE METHOD 패턴을 사용하면 중복을 줄일 수 있다.   given-when 부분을 부모 클래스에 두고 then 부분을 자식 클래스에 두도록 하면 된다.   아니면, @Before 함수에 given/when 부분을 두고 @Test 함수에 then 부분을 넣어도 된다. 

하지만, 이 모든 것이 배보다 배꼽이 큰 경우라고 부를 수 있다.

그렇기에, 이 책은 assert 문을 여럿 사용하되, 테스트 함수마다 한 개념만 테스트 하는 것을 권해주고 있다.

***

## 오늘의 기억하고 싶은 내용

### F.I.R.S.T

깨끗한 테스트는 첫 철자만 따온 다섯 가지 규칙을 따른다.

#### 빠르게(Fast)

테스트는 빨리 돌아야 한다. 테스트가 느리면 자주 돌릴 엄두를 내지 못하고, 자주 돌리지 못하면, 문제를 찾아내 고치지 못하기 때문이다.

#### 독립적으로(Independent)

테스트는 서로 의존하면 안된다. 테스트가 서로에게 의존하면 하나가 실패할 경우, 나머지도 잇달아 실패할 수 있기 때문이다.

그렇기에, 테스트는 다음 테스트가 실행될 환경을 준비해서는 안되고, 어떤 순서로 실행해도 괜찮을 정도로 독립적이어야 한다.

#### 반복가능하게(Repeatable)

테스트는 어떤 환경에서도 반복 가능해야한다. 

테스트가 돌아가지 않는 환경이 하나라도 존재한다면, 테스트가 실패한 이유를 둘러댈 수도, 환경이 지원하지 않기에 수행하지 못하는 경우도 생기기 때문이다.

#### 자가검증하는(Self-Validating)

테스트는 bool 값으로 결과를 나타내야 한다. 또한 통과 여부를 알려고 로그 파일을 읽게 만들어서는 안된다.

#### 적시에(Timely)

테스트는 적시에 작성되어야 한다.

단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현되는데, 만약, 실제 코드를 구현한 후에 테스트 코드를 작성할 경우, 실제 코드가 테스트하기 어렵다는 사실을 발견할 수 있을지 모른다.

***

### 9장을 돌이켜보며

이 장의 결론에서도 말하듯이, 이 장은 테스트 코드라는 방대한 주제에서도 수박 겉핥기 정도만 훑었다고 알려주고 있다.

그럼에도 깨끗한 테스트 코드를 위해서, 여전히 놓친 부분은 많은 것 같다.

어째서, 테스트 코드가 실제 코드만큼 중요한지 부터, 그들을 통한 실제 코드의 유지보수성, 유연성 등이 보존되는지를 알 수 있었던 장이었기에.

***

## 해시태그 ##
#코딩 #개발자 #노마드코더 #북클럽 #노개북

***

## 작성시기 ##
2022.05.08

***