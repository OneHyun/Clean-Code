- [5장. 형식 맞추기](#형식-맞추기)
    
    [형식을 맞추는 목적](#형식을-맞추는-목적)

    [적절한 행 길이를 유지하라](#적절한-행-길이를-유지하라)

    [개념은 빈 행으로 분리하라](#개념은-빈-행으로-분리하라)

    [세로 밀집도](#세로-밀집도)

    [가로 형식](#가로-형식)

- [5장을 돌이켜보며](#5장을-돌이켜보며)

***

## 형식 맞추기

### 형식을 맞추는 목적

코드 형식은 의사 소통의 일환이다.   의사소통은 전문 개발자의 의무이며, 그 동안 '돌아가는 코드'를 일차적인 의무라 여겼다면, 개선해나가길 바란다.

코드의 가독성은 앞으로 바뀔 코드의 품질에 지대한 영향을 끼친다.   코드가 바뀌어도 구현 스타일과 가독성 수준은 유지보수 용이성과 확장성에 계속 영향을 끼치기 때문이다.

***

### 적절한 행 길이를 유지하라

해당 장에서는 프로젝트 7개 가지의 그림 자료를 들고 있다.

또한 자료를 들어, 어떤 프로젝트는 가장 긴 파일이 400줄이고, 평균적으로 40-100줄인 반면, 어떤 프로젝트 들은 500줄을 넘어, 수천 줄을 넘어가는 파일이 존재한다고.

일반적으로 큰 파일보다 작은 파일이 이해하기가 쉽다.   그렇기에 엄격한 규칙은 아니지만, 가능한 염두에 둘 규칙으로 세웠으면 한다.

***

### 개념은 빈 행으로 분리하라

거의 모든 코드는 왼쪽에서 오른쪽으로 위에서 아래로 읽혀진다.

각 행은 수식이나 절을 나타내고, 행 묶음은 완결된 생각 하나를 나타낸다.

생각 사이에는 빈 행을 넣어 분리하는 것이 중요하다. 그것이 가독성을 살려주기 때문이다.

***

### 세로 밀집도

줄바꿈이 개념을 분리한다면, 세로 밀집도는 연관성을 의미한다.   즉, 서로 밀접한 코드 행은 세로로 가까이 놓여야 한다는 뜻이다.

아래 예제는 의미 없는 주석으로 두 인스턴스 변수를 떨어뜨려 놓을 때와 주석을 제거하여 붙여있을 때이다.

어떤 코드가 한눈에 보기 쉬운지는 예제를 통해 파악해보자

<pre>
<code>
public class ReportConfig {
    /**
     * 리포터 리스너의 클래스 이름 
     */
    private String m_className;
    
    /**
     * 리포터 리스너의 속성
     */
    private List<Property> m_properties = new ArrayList<Property>();
    public void addProperty(Property property){
        m_properties.add(property); 
    }
}

vs

public class ReportConfig {
    private String m_className;
    private List<Property> m_properties = new ArrayList<Property>();

    public void addProperty(Property property){
        m_properties.add(property); 
    }
}
</code>
</pre>

#### 수직 거리

작업을 하다보면, 함수 연관 관계와 동작 방식을 이해하고자 이 함수에서 저 함수로 오간 경험이 있을 것이다.

이런 경험은 공통적으로 시간과 노력을 소모한다.   달갑지도 않은 경험이고 말이다.

그렇기에 서로 밀접한 개념은 세로로 가까이 둬야한다. 세로 거리로 밀접한 개념을 표현하는 이유인 것이다.

##### 변수 선언 

변수는 사용하는 위치에 최대한 가까이 둔다.

##### 인스턴스 변수

인스턴스 변수는 클래스 맨 처음에 선언한다.

잘 설계한 클래스는 많은 클래스 메소드가 인스턴스 변수를 사용하기 떄문에, 변수 간에 세로로 거리를 두지 않도록 한다.

##### 종속 함수

한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치하면 좋다. 

##### 개념적 유사성 

개념적인 친화도가 높은 코드는 서로 붙어 있도록 작성한다.

***

### 가로 형식 

특정 구조를 강조하고자, 아래 예제와 같은 가로 정렬을 사용한 코드를 볼 수도, 못볼 수도 있다.

<pre>
<code>
public class FitNesseExpediter implements ResponseSender
{
    private Socket          socket;
    private InputStream     input;
    private OutputStream    output;
    private Request         request;
    private Response        response;
    private FitNesseContext context;
    protected long          requestParsingTimeLimit
    private long            requestProgress;
    private long            requestParsingDeadline;
    private boolean         hasError;
}
</code>
</pre>

결론적으로 말하자면 이와 같은 가로 정렬은 어셈블리어가 아니라면, 사용하지 말도록 하자.

이렇게 가로 정렬을 한다면, 결국 변수 유형을 보는게 아니라, 오른쪽 피연산자에 집중하게 중요한 부분을 놓치게 된다.

아래 예제처럼 말이다.

<pre>
<code>
public FitNessExpeditor(Socket s, 
                        FitNesseContext context){
    this.context = context;
    socket =       s;
    input =        s.getInputStream(); 
    output =       s.getOutputStream(); 
}
</code>
</pre>

***

### 5장을 돌이켜보며

그 동안, 의식하지 않았었지만 변수나 함수들의 구조를 어떻게 두어야 할지.   어떤 것이, 더 눈에 들어오는지 등을 직접적으로 경험과 함께 볼 수 있었던 좋았던 장이었다.

다만, 가장 머리에 남는 것은 규칙보다는 마지막 파트인 '팀 규칙' 이었던 것 같다.

해당 파트에서는 "팀은 한 가지 규칙에 합의해야 한다. 그리고 모든 팀원은 그 규칙을 따라야 하다"는 문구가 존재한다.

프로그래머라면 각자 선호하는 규칙이 있겠지만, 해당 문구는 팀에 속하고자 한다면 당연하면서도,   지금도, 앞으로도 짜고 있을 코드의 구조를 언제든지 바꿀 수 있다는 생각이 바탕이어야 한다고 생각하였기에.

규칙을 바탕으로 한, 요점이라는 생각을 버릴 수 없었다.

***

## 해시태그 ##
#코딩 #개발자 #노마드코더 #북클럽 #노개북

***

## 작성시기 ##
2022.05.02

***