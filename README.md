# My-Document

이것저것 기술 관련 쌓아 가는 공간

> 주관적인 의견도 있기에 다른 의견이 있을 수 있습니다. 내가 뭐 정답도 아니고 엄청나게 잘 안다 하는것도 아닙니다~

📖Contents
1. [Design Pattern](./design-pattern)
2. [JAVA](./JAVA)
3. [Spring](./Spring)


### 계기
이직준비를 하며 면접을 보다보면 간혹 아주 원초적인 Java 관련 질문들을 받게 된다. 예를들어 메모리의 구조라던가... GC의 변천사 각GC 별 작동 원리등 Foundation 관련 질문들 객체지향의 5대 원칙 등등   
음... 사실 이부분에 한해서는 JAVA를 처음 배울 시절 그러니까 지금으로 부터.. 약 10년 이상 전에 보고 이 후 거의 신경을 쓰지 않은 부분이다. 객체지향이란 개념을 그 당시에 배우고 내가 누군가를 교육하는 입장이 아니기에 용어에 크게 신경은쓰지 않았고 뭔가 당연하게 몸에 배여 쓰고 있고 확장 가능성을 고려하며 설계 했던 부분들인데 아 저런명칭이였구나.. 지금까지 개발자로 일하면서 JAVA는 객체지향이고 왜 객체지향이 나왔고 아 이런 비지니스는 이런식으로 설계를 해야 유지보스 측면에.. 이런? 뭐랄까 당연 했다고 해야하나   
헌데 요 몇년사이 객체지향 블라블라 OOP에 대해 아시나요~? OOP가 뭔가요? 처음엔 뭔가 예전에 내가 알던 객체지향에서 요즘 트랜드에 맞춰 변화가 있었나보다 했다. 그런데 뭐 똑같은 개념이다 그냥 헌데 지금까지 살면서 들었던 객체지향이란 단어보다 요 몇년사이에 듣는 우오 객체지향! 객체지향이란 말이죠 블라블라 5대원칙이란~~ 하는걸 더 자주 듣는듯 하다;;   
사람마다 느끼는 바가 다르겠지만 용어가 짱이지 암기야 하며 외우고 줄줄히 설명하고 이게 과연 잘하는 건가 하는 생각은 조금 든다. 모르는 것이 아닌 뭐랄까... 논문으로 글로써 암기를 하고 남에게 전달을 해주기보다 그냥 몸에 베여서 설계를 하거나 개발을 할때 당연스럽게 그런 부분을 고려하는 그런거 말이다...   
원칙과 규칙을 법을 우리가 외우진 않지 않나??? 세상을 살아가며 알게 되고 행동하지 정확하게 단어를 외우면서 그에 맞춰 행동하진 않지 않냐는 말이다...   

그런데.. 시대의 흐름이 그런건가;; 하는 생각과 아 요즘엔 이렇게 생각하는게 트랜드인가? 하는 생각과 내부 교육 차원에서 나 스스로도 리마인드 해보고자 정리를 시작해본다.   

사실 조금은 그런생각도 했다. 프레임워크를 만들거나, 특정 솔루션을 만들어 제품을 판매 하는 용도의 개발이 아니고서야 특정 진형 (나의 경우는 Spring이 해당)의 프레임워크위에 작업을 하게 될 텐데.... 하며 저런 부분에 대해 크게 생각을 안했다.   
너무나도 좋은 오픈소스나 프레임워크들이 나오고 있다 보니 코어한 부분을 위주로 파서 해결 하기보다 그 시간에 잘 버무려서 비지니스를 빠르게 만들어서 런칭 하는게 더 이득 아닌가 하는 생각이다.   
(물론, 비지니스나 유지보수에 맞춰 잘짠 프로그램이라는 전제하에....)

과거에는 장비의 가격도 비싸고 한번 리소스를 다 먹게 되면 장비를 내렸다 올리기도 리스크가 컸기에 성능과 메모리등 자원을 신경을쓰는 설계를 많이 했었다. 실제 나도 배치나 데몬등을 돌릴때 터미널을 열어 어플리케이션이 구동되는동안 메모리의 변화나 IO의 변화를 계속 모니터링 하곤 했다.   
하지만 오늘날에는 일단 가격이 과거에 비해 싸지고, 클라우드나 가상화 환경에서 서버를 돌리기에 Scale in.out이 매우 쉽다. JAVA의 특성상 GC가 메모리 관리를 해주기에 메모리 누수의 위험도 10년이상 어플리케이션 개발을 JAVA로 하며 단 한번도 없었으니 정말 개념 없이 마구잡이로 자원을 끌어쓰지 않는한 크게 문제가 발생 하지 않을거라고 생각한다.   

주절주절 두서 없이 말을 썼는데 결국 어플리케이션 개발을 위해서는 대충 어떻게 돌아가는지 그림과 개념정도만 알면 되지 않을까 하는 생각이다.   

> 진짜! 내 생각이 잘못되고 틀린거 일수도 있다. 정말 내 개인적 주관적 생각이다.   
> 다시 말하지만 내가 엄청 뛰어난 개발자 여서 이런 글을 쓴다기 보다 순수하게 제가 느낀 바를 쓰는 것입니다.
> 모든 의견은 겸허히 받아들이고 수긍하겠습니다. 논의은 환영이지만 싸움은 싫어요!!!
