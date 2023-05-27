# Chapter 06. 무식하게 풀기

## 6.1 도입

- 우아한 답안을 만들려다가 앞에 보이는 쉽고 간단하며 틀릴 가능성이 낮은 답안을 간과하기 쉽다 -> 무식하게 풀 수 있을 지 먼저 확인하자

## 6.2재귀 호출과 완전 탐색

- 재귀 함수란 자신이 수행할 작업을 유사한 형태의 여러 조각으로 쪼갠 뒤 그 중 한 조각을 수행하고, 나머지를 자기 자신을 호출해 실행하는 함수
- 기저 사례를 선택할 때는 존재하는 모든 입력이 항상 기저 사례의 답을 이용해 계산될 수 있도록 신경써야 함
- 반복문은 깊이가 깊어질 수록 반복문이 중첩되어야 하지만 재귀 함수는 깊이가 깊어져도 변함이 없음

## 6.7 최적화 문제

- 무식하게 풀기는 최적화 문제를 해결하는 여러 가지 방법 중 한가지
- 최적화 문제를 해결하는 방법으로는 동적 계획법, 조합 탐색, 결정 문제로 바꿔 해결하는 기법...
- 최적화 문제: 어떤 기준에 따라 가장 좋은 답을 찾아 내는 문제
- 예를 들어 n개의 사과 중 r개를 골라서 무게의 합을 최대화, 가장 무거운 사과와 가벼운 사과의 무게 차이를 최소화, ...

## 6.10 많이 등장하는 완전 탐색 유형
  
- 모든 순열 만들기
  - 가능한 순열의 수: N!
  - N 이 10을 넘어가면 시간 안에 모든 순열을 생성하기 어려우므로 완전 탐색 말고 다른 방법 생각
- 모든 조합 만들기
- 2^n 가지 경우의 수 만들기
  - n개의 질문에 대한 답이 예/아니오 중 하나라고 할 때 존재할 수 있는 답의 수