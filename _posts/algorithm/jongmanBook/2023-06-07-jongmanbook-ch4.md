---
title: "[종만북] (2)알고리즘 분석 - CH4.알고리즘의 시간 복잡도 분석(java) "
excerpt : "프로그래밍 대회에서 배우는 알고리즘 문제 해결 전략 : p.87 ~ p.126"
categories: algorithm jongmanbook
tags:
    - [algorithm,종만북,java,subsequence,maxsum, 시간복잡도, 계산복잡도,계산복잡도클래스,p문제,n문제,n-hard,n-complete,SAT,환산,reduction,O표기법,이동평균,movingAverage,selectionSort,insertionSort,sort]
date : 2023-06-07
last_modified_at: 2023-06-07
toc : ture
toc_sticky : true
---
# CH4. 알고리즘의 시간 복잡도 분석

- 반복문의 수행 횟수가 알고리즘의 수행시간을 지배한다.

## ⚡수행시간의 형태

### 선형 시간 알고리즘 : O(N)

- 수행시간이 입력의 크기 N에 정비례하는 알고리즘
- 대개 우리가 찾을 수 있는 가장 좋은 알고리즘인 경우가 많다.

### 이동평균 구하기

- 그냥 냅다 구하면 O(N^2)
    
    ```java
    // O(N^2) : 그냥 구하기
        static ArrayList<Double> getMovingAverage(int[] arr,int M){
            ArrayList<Double> movingAvgs=new ArrayList<Double>();
    
            for(int i=M-1;i<arr.length;i++){
                int sum=0;
                for(int j=0;j<M;j++){
                    sum+=arr[i-j];
                }
                movingAvgs.add((double)sum/M);
            }
    
            return movingAvgs;
    
        }
    ```
    
- 중복계산을 줄이는게 관건
- 이미 계산되있는 값에 빼야할값, 더해야할 값만 추가적으로 계산해서 시간 복잡도를 선형시간으로 줄일 수 있음 O(N)
    
    ```java
    // O(N) : 선형 시간에 구하기 (중복계산 제거)
        static ArrayList<Double> getMovingAverage2(int[] arr,int M){
    
            ArrayList<Double> movingAvgs= new ArrayList<Double>();
            int sum=0;
            for(int i=0;i<M-1;i++){
                sum+=arr[i];
            }
    
            for(int i=M-1;i<arr.length;i++){
                sum+=arr[i];
                movingAvgs.add((double)sum/M);
                sum-=arr[i-M+1];
            }
    
            return movingAvgs;
        }
    ```
    

### 선형 이하 시간 알고리즘 : 이진탐색(Binary Search)알고리즘 : O(lnN)

- 정렬된 입력에서 사용
- 정의
    
    오름차순으로 정렬된 배열 A[]와 찾고 싶은 값 x가 주어질 때 A[i-1]<x<A[i]인 i를 반환한다.
    
    (이때, A[-1]=∞ , A[n]=∞ 로 가정)
    
- 대충 쉽게 풀어서 말하면
    
    오름차순으로 정렬된 배열 A에 x가 들어갈 수 있는 위치 중 가장 앞 위치를 찾는 것
    
    x가 없다면 x보다 큰 첫번째 원소를 반환하게 됨
    

### 지수 시간 알고리즘 : 다항시간 알고리즘

- 반복문의 수행 횟수를 입력크기 N의 다항식으로 표현할 수 있는 알고리즘
- 대충 O(N^2)이런것들

## ⚡시간 복잡도

- 알고리즘 수행시간 기준
- 알고리즘 실행동안 수행하는 기본적인 연산의 수를 입력의 크기에 대한 함수로 표현한 것
- 시간복잡도가 낮으면 큰 입력에서 효율적
- 입력의 크기가 매우작다면 큰 의미 없음

### 입력 종류에 따른 수행시간 변화

- 입력의 크기 뿐만 아니라 입력의 형태도 수행시간에 영향을 미침
- 입력에 따라 아래와 같은 수행시간이 될 수 있음
    - 최선의 수행시간
    - 최악의 수행시간
    - 평균적 경우에 수행시간
- 대체로 우리는 최악의 수행시간, 수행시간의 기대치를 사용

### 점근적 시간표기 : Big O

- 시간복잡도를 간단하게 표현하는방법
- 대략적인 함수의 상한을 나타냄
- 시간복잡도에서 수행시간에 큰 영향을 미치지 않는 상수 부분을 버리고 표기
- 알고리즘 입력 크기가 두 개 이상의 변수로 표현될 때 가장 빨리 증가하는 항들만 놓고 나머지 버림
    - $2^NM=O(2^NM)$
    - $\frac1{64} N^2M+64NM=O(N^2M)$
    - $N^2M+NlnM+NM^2=O(N^2M+NM^2)$
    - $42=O(1)$
- 읽을때는 O=Order

### 시간 복잡도 분석 연습

- 최악의 경우, 최선의 경우, 평균적 경우 모두 다른 시간 복잡도를 가질 수 있으므로 주의
- ***선택정렬(selection sort)***
    - 모든 i에 대해 A[i~N-1]에서 가장 작은 원소를 찾은 뒤 A[i]에 넣는 것을 반복
    - 시간 복잡도 : 최악의 경우=최선의 경우=O(n^2)
        
        원소들과 관계없이 N에 크기에만 결정되므로 최악의 경우=최선의 경우
        
        ```java
        void selectionSort(int[] arr){
                int N=arr.length;
                for(int i=0;i<N;i++){
                    int minIndex=i;
                    //해당 i~n-1의 가장작은 원소를 찾아 arr[i]에 넣음
                    for(int j=i+1;j<N;j++){
                        if(minIndex>arr[j]){
                            minIndex=j;
                        }
                    }
                    int temp=arr[i];
                    arr[i]=arr[minIndex];
                    arr[minIndex]=temp;
                }
            }
        ```
        
- ***삽입정렬(Insertion sort)***
    - 전체 배열 중 정렬된 부분배열에 새 원소를 끼워넣는 것을 반복
    - 배열을 순회하며(for) 선택된 인덱스의 숫자보다 작은 숫자가 나올 때까지 숫자를 앞으로 옮겨감(while)
    - 시간 복잡도
        - 최선의 경우 : O(1) → 이미 정렬된 배열이 주어지는 경우
        - 최악의 경우 : O(N^2) → 역순으로 정렬된 배열이 주어지는 경우
    
    ```java
    void insertionSort(int[] arr){
            int N=arr.length;
            for(int i=0;i<N;i++){
    
                int j=i;
                //선택된 인덱스의 값보다 작은 값이 나올때까지 앞으로 옮김
                while(j>0 && arr[j]<arr[j-1]){
                    if(arr[j]<arr[j-1]){
                        int temp=arr[j-1];
                        arr[j-1]=arr[j];
                        arr[j]=temp;
                        break;
                    }
                    j--;
                }
            }
        }
    ```
    
- ***선택정렬, 삽입정렬 비교***
    - 삽입정렬이 선택 정렬보다 빠름
    - 삽입정렬은O(N^2)정렬 중 가장 빠른 알고리즘
    

### 시간 복잡도의 분할 상환 분석(amortized analysis)

- 반복문의 수가 아니라 다른걸로 시간 복잡도 결정
- 전체 작업시간에 걸리는 시간을 작업의 개수로 나눈 평균시간을 사용

## ⚡수행시간 어림짐작하기

- 시간 복잡도와 입력 크기를 통해 알고리즘 동작시간을 짐작


💡 입력의 크기를 시간 복잡도에 대입해서 얻은 반복문 수행횟수에 대해 1초당 반복문 수행횟수가 1억(10^8)을 넘어가면 시간 제한 초과 가능성 있음(C++기준)
{: .notice}


- 예시) 입력의 최대크기=10000
    
    O(N^3)알고리즘의 경우 → (10000)^3 : 1억을 훨씬 초과하므로 시간제한 초과 가능성 있음
    
    O(NlnN)알고리즘의 경우 →10000ln(10000) : 1억에 못미치므로 나름 안전
    
- 그러나 다른 요소들이 수행속도에 큰 영향을 주는 경우도 있으므로 충분한 여유를 두고 사용이 필요
    - 수행횟수가 기준의 10%이하인 경우, 기준의 10배를 넘는 경우에만 사용

### 시간 복잡도 외의 고려할점

1. ***시간복잡도가 프로그램의 실제 수행속도를 반영하지 못하는 경우***
    - O표기에 의해 버려지는 상수항이 수행시간에 영향을 줄 수 있다.
2. ***반복문의 내부가 복잡한 경우***
    - 반복문 내부는 단순할 수록 좋다.
    - 반복문 내부가 복잡하면(실수연산, 파일 입출력 등) 수행시간이 길어질 수 있다.
3. ***메모리 사용 패턴이 복잡한 경우***
    - 캐시 메모리를 잘 활용하면 시간복잡도와 별개로 수행속도가 빨라질 수 있다.
4. ***언어와 컴파일러 차이***
5. ***구형 컴퓨터를 사용하는 경우***

### 적용 : 1차원 배열에서 연속된 부분 구간 중 합이 최대인 구간의 합 찾기

1. 주어진 배열의 모든 부분 구간을 순회 하며 그 합을 계산
    - O(N^2)의 후보구간 검사, O(N)각 구간 합 계산
    - 따라서 시간복잡도는 O(N^3)
    
    ```java
    int inefficientMaxSum(int[] arr){
            int N=arr.length;
            int max=0;
            for(int i=0;i<N;i++){
                for(int j=i;j<N;j++){
                    //부분수열의 합 계산
                    int sum=0;
                    for(int k=i;k<j;k++){
                        sum+=arr[k];
                    }
                    max=Math.max(max,sum);
                }
            }
            return max;
        }
    ```
    
2. 개선
    - 중복계산 줄임
    - 이동평균 구할 때 처럼 이미 계산된 값을 활용
    - 시간복잡도가 O(N^2)으로 감소
    
    ```java
    int betterMaxSum(int[] arr){
            int N=arr.length;
            int max=0;
            for(int i=0;i<N;i++){
                int sum=0;
                for(int j=i;j<N;j++){
                    //기존에 계산했던 arr[0]~arr[j-1]에 arr[j]값만 더해서 활용
                    sum+=arr[j];
                    max=Math.max(max,sum);
                }
            }
    
           return max;
        }
    ```
    
3. 분할 정복기법 사용
    - 입력받은 배열을 절반으로 잘라 재귀호출과 탐욕법을 사용해 계산 (7장)
    - 이 경우 시간 복잡도는 O(NlnN)
    
    ```java
    int fastMaxSum(int[] arr, int lo,int hi){
    
            //구간의 길이가 1인경우
            if(lo==hi) return arr[lo];
    
            //1. 배열을 두조각으로 나눔 : arr[i~mid] , arr[mid+1,i]
            int mid=(lo+hi)/2; //중간 인덱스
    
            //2. 두 부분에 걸치는 최대구간 찾기 : 이 구간은 앞구간 일부+뒷구간 일부로 이루어짐
            int left=Integer.MIN_VALUE; // 앞구간의 최댓값
            int right=Integer.MIN_VALUE; //뒷구간의 최댓값
            int sum=0;
    
            //앞, 뒤부분 의 최대구간을 합치면 두 부분에 걸치는 가장 큰 최대구간을 찾을 수 있따.
    
            //앞구간의 최대 합 찾기
            for(int i=mid;i>lo;i--){
                sum+=arr[i];
                left=Math.max(left,sum);
            }
    
            //뒷 구간의 최대 합 찾기
            sum=0;
            for(int i=mid+1;i<hi;i++){
                sum+=arr[i];
                right=Math.max(right,sum);
            }
    
            //3. 최대구간이 두 구간중 한 구간에만 속해있는 경우의 답을 재귀호출로 찾는다.
            int single=Math.max(fastMaxSum(arr,lo,mid),fastMaxSum(arr,mid+1,hi));
    
            //4. 두 경우 중 최대치를 반환한다.
            return Math.max(left+right,single);
    
        }
    ```
    
4. 동적 계획법 사용
    - 이미 계산된 결과를 활용
    - 점화식 형태로 표현
        
        ***maxAt(i)=max(0,maxAt(i-1))+A[i]***
        
        *maxAt(i)* : A[i]를 오른쪽 끝으로 갖는 구간의 최대합
        
        *max(0,maxAt(i-1))* : 이전까지 최댓값이 음수라면 i부터 다시 구간을 세서 음수를 배제하는게 더 큰값을 얻을 수 있으므로 음수 구간을 배제하기 위해 이런 수식을 사용
        
    - 시간 복잡도는 O(N)
    
    ```java
    int fasterMaxSum(int[] arr){
            //maxAt(i)=max(0,maxAt(i-1))+A[i]
    
            int N=arr.length;
            int psum=0;
            int max=0;
            for(int i=0;i<N;i++){
                psum=Math.max(0,psum)+arr[i];
                max=Math.max(psum,max);
            }
    
            return max;
        }
    ```
    

## ⚡계산 복잡도 클래스 : P , NP , NP-Complete

- 시간 복잡도는 알고리즘의 특성
- 계산 복잡도는 문제의 특성
- 각 문제에 대해 이를 해결하는 알고리즘이 얼마나 빠른지

### 계산 복잡도 클래스

- ***P(Polynomial)문제*** : 다항시간 알고리즘이 존재하는 문제 집합
- ***NP(Non-Deterministic Polynomial)문제*** : 답이 주어졌을때 이것이 정답인지를 다항 시간내에 확인 할 수 있는 문제
- ***NP-Hard(난해)문제*** : 모든 NP문제보다 어려운 문제
    - 따라서 모든 NP문제를 NP-Hard로 환산하여 풀 수 있음
    - ex ) SAT 문제
- ***NP-Complete(완비)문제*** : NP문제이면서 NP-Hard문제인 문제 집합
    - NP문제이면서 모든 NP문제를 환산하여 풀 수 있는 문제
    - ex) 부분집합의 합 문제

- 모든 P 문제는 NP문제에 포함됨 (P⊂NP)

### 두 문제의 난이도 비교 : 환산(reduction)

- 한 문제를 다른 문제로 바꿔서 푸는 기법
- 쉽다 : 수행시간이 빠르다.
- 어렵다 : 수행시간이 느리다.
- 문제 A,B비교하기
    1. 문제 A의 입력을 문제 B의 입력으로 변환
    2. 변환된 입력을 문제B를 푸는 가장 빠른 알고리즘으로 품
    3. 출력된 해를 A의 출력으로 변환
    4. 이때 B의 알고리즘으로 A가 해결되었다면 A를 해결할 수 있는 가장 빠른 알고리즘은 적어도 B 알고리즘이거나 이보다는 빠를것임
    5. 따라서 B는 A이상으로 어려울 것이라고 판단할 수 있음 (A가 더 쉽다는 것을  알수 있음)
- 어렵다면 설명이 잘 되어있어 도움을 받은 [이 블로그](https://isoomni.tistory.com/entry/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-NP-%EC%99%84%EC%A0%84-%EB%AC%B8%EC%A0%9C-1)을 보는 것을 추천!
- 쉬운 문제는 어려운 문제로 환산하여 풀기 가능

### 어려운 문제의 기준 : SAT(Satisfiability) 문제

- N개의 boolean값 변수로 구성된 논리식을 참으로 만드는 변수값들의 조합을 찾는 문제
- 예시
    - boolean변수 a,b,c가 있다고 가정할 때, 아래 논리식은 세 변수의 값이 특정 조합을 이루어야 만족
    - ((a\|\|b\|\|!c)&&(!c\|\|!a)&&((!a&&b)\|\|(b&&!c)))&&(!b\|\|!a&&!c))
    - 이때 위 논리식을 참으로 만드는 a,b,c의 조합을 찾는게 SAT문제
- 모든 SAT문제는 NP문제 이상으로 어려움(수행시간이 긺) (Cook-Levin Theorem)
    - 따라서 SAT(어려운문제)를 다항시간에 풀 수 있으면 NP(쉬운문제)를 다항시간에 풀 수 있음 (환산)
- 위와 같이 모든 NP문제보다 어려운 문제를 ***NP-hard문제***라고 함

### P=NP 문제

- P와 NP가 같은지 확인하는 문제
- NP-Hard 문제 중 하나를 다항시간에 풀 수 있다면 이 알고리즘으로 NP에 속한 모든 문제를 다항 시간에 풀 수 있음
- 이렇게 되면 모든 NP문제를 다항시간에 풀 수 있으므로P=NP가 된다.
- 반대로 NP-Hard문제가 다항시간에 푸는 방법이 없음을 증명한다면 P≠NP임이 증명된다.

- 아직 NP-Hard를 **다항시간에 풀 수 있다는것**과 **푸는방법이 없다는것**을 증명하지 못했으므로 미해결상태
- 따라서 어떤문제를 풀 때 이 문제에 임의의 변환(환산)을 거치면 NP-Complete나 NP-Hard를 풀 수 있다는 것은 아래를 의미한다.
    - 풀고있는 문제는 NP-Hard보다 어려우므로 문제를 환산하여 NP-Hard를 풀 수 있어야 한다.
    - NP-Hard를 다항시간에 푸는 방법은 밝혀지지 않았으므로 풀고 있는 문제는 더 빠르게 풀 수 있는 방법을 찾기 매우 어려울 것이다.
    - 따라서 모델링을 달리하거나 근사해를 찾는 등의 방법이 합리적일것이다.

## ⚡기타

### 마스터 정리 (Master Theorem)

- 재귀적 알고리즘의 시간 복잡도 계산하는 쉬운 방법
- 어떤 함수의 수행시간이 특정 형태의 함수로 표현될 때 O표기법을 쉽게 계산할 수 있도록 도와줌

---