
비즈니스 프로세스 모델 분석
STRUCTURE ANALYSIS TECHNIQUES 
A. STRUCTURAL CORRECTNESS ANALYSIS 
B. STRUCTURAL SAFENESS ANALYSIS

DYNAMIC ANALYSIS TECHNIQUES
A. ACTIVITY FIRING SEQUENCING RULES 
B. REACHABILITY ANALYSIS

PERFORMANCE ANALYSIS TECHNIQUES
A. SIMULATION-DRIVEN PERFORMANCE ANALYSIS 
B. HUMAN RESOURCE CAPABILITY PLANNING ANALYSIS

ORGANIZATIONAL BEHAVIORAL ANALYSIS TECHNIQUES 
A. AFFILIATION NETWORK ANALYSIS 
B. SOCIAL NETWORK ANALYSIS 
C. WORK TRANSFERENCE NETWORK ANALYSIS


STRUCTURAL CORRECTNESS ANALYSIS USING THE RULES
두 가지 구조를 만족해야 함
- SPLIT-GATEWAY ANOMAL
![[Pasted image 20251013143604.png]]
- JOIN-GATEWAY ANOMALY
![[Pasted image 20251013143436.png]]

![[Pasted image 20251013143827.png]]

STRUCTURAL SAFENESS ANALYSIS 
The essential requirement on modeling business process models
SOUND & SAFE CONTROL-FLOW STRUCTURE
비즈니스 프로세스 모델은 불필요한 단위업무-액티버티를 포함하고 있지 않아야 하며, 그로부터 생성되는 모든 워크케이스는 반드시 그 의 실행을 완전하고 성공적으로 완료되어야 하고, 실행이 종료된 후에는 해당 워크케이스의 실행상태를 나타내는 색깔-토큰들이 더 이 상의 존재하지 않아야 한다.

VERIFICATION OF SOUND & SAFE CONTROL-FLOW STRUCTURE
입증하기 위해서는 합리적 대체 속성 조건에 맞게 단계적으로 대체하면서 입증한다.
- 합리적 대체 속성: 조건 어느 한 단위업무-액티버티를 합리적/안전적 제어구조의 정보제어넷 모델로 대체하면, 그 결과의 정보제어넷 모델 역시 합리적/ 안전적 제어구조를 갖는다.

BUILDING-BLOCKS OF INFORMATION CONTROL NET
- 기본적으로 safe한 sound&safe structures
![[Pasted image 20251013144512.png]]


REACHABILITY ANALYSIS(도달 가능성 분석)
정보제어넷 비즈니스 프로세스 모델의 동적 분석요소인 색깔-토큰 및 그의 점화순서 개념을 융합하여 해당 프로세스를 구성하는 단위업무-액티버티들의 실행순서를 도달가능성 그래프 모델로 표현하는 동적 분석방법으로서 비즈니스 프로세스 모델링 오류 상황 분석 및 인 지 기법의 하나이다.



Performance analysis
마르코프 분석
대기행렬이론, 큐잉이론

MeanValueAnalysis: 1000명의 사용자가 사용할 때의 퍼포먼스를 분석하기 위해 1000까지 가는 과정을 워밍업이라 하고 1000명이 됐을 때부터 측정을 시작


비즈니스 프로세스의 모델링 제어구조와 자원할당정책에 따른 성능분석 및 비교 
A. Sequential 
(프로세스 모델링 상황 1) 단위업무-액티버티를 처리하기 위한 인적자원의 운용방법이 매우 경직되어 있는 상황으로서, 오직 하나의 단위업무- 액티버티를 처리하는데 두 명의 인적자원을 전적으로 할당한 정보제어넷 프로세스 모델 
B. Parallel 
(프로세스 모델링 상황 2) 비즈니스 프로세스 모델을 개발하는데 있어서 일반적으로 참고하는 가이드라 인들 중의 하나로 가능한 최대한으로 단위업무-액티버티들을 병렬로 구성하라는 점 을 상기할 필요가 있다. 이러한 가이드라인에 따라 순차적 제어구조의 단위업무-액티버티들을 병렬게이트웨이-액티버티를 이용한 병렬적 제어구조로 구성한 정보제어넷 프로세스 모델 
C. Composition 
(프로세스 모델링 상황 3) 때로는 두 개의 단위업무-액티버티들을 한 개의 존 더 큰 범위의 단위업무-액티버티로 결합시켜 처리하는 것이 더 우수한 효과를 보는 경우가 있다. 이러한 상황을 반영한 정보제어넷 프로세스 모델 
D. Flexibilization 
(프로세스 모델링 상황 4) 인적자원의 유연성에 대한 긍정적인 영향을 설명하기 위하여, 순차적 단위업무-액티버티에서 인적자원들을 특정 단위업무-액티버티에 배정하지 않고 두 개의 단 위업무-액티버터를 모두를 처리할 수 있도록 유연성을 제공한 정보제어넷 프로세스 모델 
E. Triage 
(프로세스 모델링 상황 5) 자원의 선별적 분배원칙(triage)로서 워크케이스의 유형을 수월(easy)-워크케이스와 난해(hard)-워크케이스로 구분한 정보제어넷 프로세스 모델 
F. Prioritization 
(프로세스 모델링 상황 6) 각 단위업무-액티버티의 처리에 있어서 수월-워크케이스에게 난해-워크케이스보다 우선적인 처리 기회를 제공한 정보제어넷 프로세스 모델

성능 분석

성능 분석 결과


CAPABILITY PLANNING & ANALYSIS OF BUSINESS PROCESS MODELS
비즈니스 프로세스 모델의 예측수용성 계획 및 분석의 정의

예측수용성 계획 정의: 예측수용성 계획의 의미는 어느 인적자원들을 또는 무슨 인적자원의 분류유형들을 각 단위 기간에 어떻게 배정할 것인가를 계획하는 것



비즈니스 프로세스 모델의 조직행동형태 분석의 정의
조직행동형태 모델 유형
- 소속성 네트워크 모델: 사람과 이벤트 간의 관계
- 사회성 네트워크 모델: 사람끼리 관계


pull-업무협력, push-업무협력

소시오 행렬: 생성된 생성된 업무-사회성 네트워크 모델을 대상으로 다양한 유형의 전통적인 소셜네트워크 분석기법을 적용하기 위해서는 해당 업무-사회성 네트워크 모델을 행렬형태 로 표현

업무 수행자의 소셜구심도 -> 높을수록 관계된 것이 많음