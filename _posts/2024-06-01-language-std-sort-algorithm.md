---
title:  "프로그래밍 언어별 정렬 알고리즘 사용"
excerpt: "기술 면접 질문 정리"

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - Algorithm
  - Sorting
last_modified_at: 2024-06-01T10:00:00-00:00
---
------------

정렬 알고리즘을 공부할 일이 과연 얼마나 있을까?

기술 면접을 준비하는 것이 아니라면 굳이 깊게 고민하거나 조사할 필요성을 못느끼는 현대 시대다. 언어가 제공하는 기본 표준 라이브러리(STL)에서 이제는 모두 정렬 알고리즘을 제공해주기 때문에 아무 생각 없이 사용하더라도 퍼포먼스에 큰 영향을 주지는 않는다.

그렇지만 최소한 프로그래밍 언어별 또는 라이브러리별로 어떠한 정렬 알고리즘을 이용하고, 속도는 어떤지 정도는 비교해서 알고 있으면 좋으리라 생각되서 요약 정리해본다.

---

## 언어별 표준 라이브러리의 정렬 알고리즘과 시간 복잡도
|언어|알고리즘|평균 시간 복잡도|최악 시간 복잡도|최적화 및 특징|
|---|---|---|---|---|
|Java|Dual-Pivot Quick Sort|O(n log n)|O(n^2)|Arrays.sort|
||Timsort|O(n log n)|O(n log n)|Collections.sort|
|JavaScript|Timsort (V8, Chrome)|O(n log n)|O(n log n)|실제 데이터에 따라 성능 최적화|
||Merge Sort (SpiderMonkey)|O(n log n)|O(n log n)||
|Python|Timsort|O(n log n)|O(n log n)|거의 정렬된 데이터에서 매우 빠름|
|C++|Introsort|O(n log n)|O(n log n)|Quick Sort와 Heap Sort, Insertion Sort의 조합|
|C#|Introsort|O(n log n)|O(n log n)|Quick Sort와 Heap Sort, Insertion Sort의 조합|
|Go|Quick Sort|O(n log n)|O(n^2)|작은 배열에 대해서는 Insertion Sort 사용, 피벗 선택 최적화|

## 한 언어에서도 다양한 정렬 알고리즘을 사용하는 이유

Java의 Collections.sort()와 Arrays.sort()는 다른 정렬 알고리즘을 사용하는데, 이는 각기 다른 데이터 구조에 최적화되어 있기 때문입니다. 구체적으로 살펴보면 다음과 같습니다:

### Java - Arrays.sort()
- 정렬 알고리즘: Dual-Pivot Quick Sort (기본적으로 원시 타입 배열에서 사용), Timsort (객체 배열에서 사용)
- 이유:
  - 원시 타입 배열: Dual-Pivot Quick Sort는 평균적으로 매우 빠르며, Dual-Pivot 기법을 사용하여 표준 Quick Sort보다 더 나은 성능을 보입니다. 원시 타입 배열은 메모리 접근이 빠르고, Quick Sort의 in-place 정렬 특성 덕분에 추가 메모리 사용이 적습니다.
  - 객체 배열: 객체 배열에서는 안정적인 정렬이 필요합니다. Timsort는 안정 정렬 알고리즘으로, 이미 정렬된 데이터나 거의 정렬된 데이터에서 매우 효율적입니다. 또한, Timsort는 추가 메모리를 사용하지만, 안정성을 보장합니다.

### Java - Collections.sort()
- 정렬 알고리즘: Timsort
- 이유:
  - 객체 리스트: Collections.sort()는 List 인터페이스를 구현하는 객체 리스트를 정렬합니다. 이 경우 안정적인 정렬이 필요하며, Timsort는 안정 정렬 알고리즘으로 안정성을 보장합니다. Timsort는 병합 정렬과 삽입 정렬의 혼합 알고리즘으로, 실제 데이터에 따라 성능이 최적화됩니다. 특히, 이미 정렬된 데이터나 거의 정렬된 데이터에서 매우 효율적입니다.

### 요약
`Arrays.sort()`는 원시 타입 배열과 객체 배열에 대해 다른 정렬 알고리즘을 사용합니다.

- 원시 타입 배열: Dual-Pivot Quick Sort
- 객체 배열: Timsort

`Collections.sort()`는 객체 리스트에 대해 Timsort를 사용합니다.

이는 **각 데이터 구조의 특성과 요구사항에 최적화된 정렬 알고리즘을 선택함으로써, 전반적인 성능과 안정성을 보장**하기 위한 것입니다. Dual-Pivot Quick Sort는 빠른 성능을 제공하며 메모리 사용이 적고, Timsort는 안정성을 보장하며 거의 정렬된 데이터에서 특히 효율적입니다.

## Tim Sort란?

먼저, 기존 정렬 알고리즘에 대해서 아래 비교 정리한 표를 보자.

![](/assets/images/posts/quick-sort-algorithm/2.png)

[참고] : (https://d2.naver.com/helloworld/0315536)