## KMP 알고리즘

> 문제 상황

- 문자열 $H$가 존재할 때, 문자열 $N$(패턴)을 부분 문자열로 포함하는지 확인해야 하는 경우
<br>
- 필요하다면 $N$과 일치하는 부분 문자열의 시작 위치를 찾을 수 있다
<br>
- 문자열 H와 N의 길이가 매우 길 경우
<br><br>

> 아이디어

문자열 $H$에서 $N$과 대응하여 탐색하다가 **불일치**가 일어났을 때, 지금까지 일치한 **부분 문자열과** **부분일치 배열**을 이용해 **다음으로 시도해야 할 시작 위치**를 재탐색 없이 빠르게 찾아낸다.
<br><br>

> 부분일치 배열 $P[i]$

- 정의
   - 문자열 $N$의 접두사도 되고 접미사도 되는 문자열의 최대 길이
   - 문자열 탐색에서 건너 뛰는게 가능하려면, **건너 뛴 후의 탐색 문자열의 앞 부분과 원본 문자열의 뒷부분 이 동일해야** 그 후의 매칭이 가능하기 때문에 접두사와 접미사 개념을 사용한다. 
- 목적
   - 불일치가 일어났을 때 패턴 포인터가 돌아갈 곳을 계산하기 위함
- 계산 방법
   - 문자열 $N$의 각 인덱스마다 맨 앞부터 해당 인덱스까지의 부분 문자열중 접두사와 접미사가 일치하는 최대 길이로 계산
- 예시
   - "a b a b a c a"인 문자열 $N$이 주어졌을 때
   ![alt text](https://github.com/MJ-Kor/SSAFY_daily_study/blob/master/imgs/PartialMatchTable.png)
<br><br>

> 구현 및 시간 복잡도

```java
// 구현은 KMP 알고리즘(KMP), 그리고 부분일치 배열을 구하는 함수(makeTable) 2가지이다.

private static int[] makeTable(String N, int NLen)  {
    // 테이블 생성
    int[] pi = new int[NLen];

    // j: 접두사 인덱스
    int j = 0;

    // i: 문자열 N에서 부분문자열의 끝 인덱스
    for (int i = 1; i < NLen; i++){

        // 접두사와 접미사 불일치 발생
        while(j > 0 && N.charAt(i) != N.charAt(j)){

            // 직전 위치로 접두사 재설정
            j = pi[j - 1];
        }

        // 접두사와 접미사 일치
        if(N.charAt(i) == N.charAt(j)){

            // 접두사 위치 증가
            ++j;

            // 접두사와 접미사가 일치하는 가장 긴 길이 저장
            pi[i] = j;
        }
    }
    return pi;
}

private static KMP(int H, int N)  {
    int HLen = H.length();
    int NLen = N.length();

    // H에 N이 대응된 수, N의 포인터
    int idx = 0;

    // 부분일치 배열 생성
    int[] table = makeTable(N, NLen);

    for (int i = 0; i < HLen; i++){

        // 과거에 하나 이상 일치하고 현재 일치하지 않을 경우 
        // 이 while문에서는
        // 1. H의 0부터 불일치 위치의 접미사와
        // 2. N의 접두사
        // 가 가장 긴 길이로 일치하는 위치로 N의 포인터를 보내는 동작이 수행된다.
        while(idx > 0 && H.char(i) != N.charAt(idx))  {

            // N의 포인터를 직전에 일치한 위치로 이동
            idx = table[idx - 1];
        }

        // 현재 일치할 경우
        if(H.charAt(i) == N.charAt(idx))  {
            // 문자열 N과 일치한 모든 문자를 찾았을 경우, 개수, 시작 위치 저장
            if(idx == NLen - 1){
              ++total;
              sIdx.add(i - idx + 1);
              idx = table[idx];
            }
            // 아직 덜 찾았을 경우, 일치 개수 증가
            else {
              idx += 1;
            }
        }
    }
}
```
- 부분일치 배열을 구하는 반복문(NLen = N)
- KMP 매칭하는 반복문(HLen = M)
- 총 $O(M + N)$의 시간복잡도를 가진다.
