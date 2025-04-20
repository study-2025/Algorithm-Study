# 큰 수 만들기

## 문제 접근

number가 최대 1,000,000자리이기 때문에 O(n^2)은 불가능하다고 판단했음. 따라서 중첩 반복문을 사용한 완탐은 불가능함. 문제 유형을 알고 풀었기 때문에 바로 그리디 알고리즘 관점으로 들어갔음. 개인적으로 느끼기에 그리디의 핵심은 “일단 포함시키기”, “멀리 보지말고 가까운 것부터 냅다 받아들이기(?)” 이런 느낌이다(말 그대로 Greedy). 이런 플로우로 생각해봤을 때, 스택처럼 숫자 하나씩 앞에서부터 바로바로 추가하는데(일단 포함시키기), 다음에 들어올 숫자가 top보다 큰 숫자일 경우 계속 pop 시키면 된다고 생각했다. 이렇게 반복적으로 숫자 push, pop하다가 나중에 자릿수를 채우기 위해 남은 모든 숫자를 집어넣어야 할 때가 오면 더 이상 숫자 크기를 비교하지 않고 모두 push한다.

## 1차 문제 풀이(Python)

```python
#1시간 10분
#완탐 안됨
'''
하나씩 집어넣을건데, 바로 앞이랑 비교했을때 더 큰 숫자가 들어오려고 하면 pop (안클때까지반복)
작거나 같으면 그냥 push
근데 이 조건을 언제까지 보냐면, 남은 자릿수 확인(필요한 자릿수와 체크해야할 수가 같아질때)
'''
def solution(number, k):
    
    answer = []
    res = ''
    arr = list(map(int, number))
    total_len = len(arr)-k
    answer.append(arr[0])
    
    for i in range(1,len(arr)):
        while len(answer)>0 and answer[-1] < arr[i]: #더 큰 숫자가 들어오려하면
            if len(arr)-i == total_len - len(answer): #채워야할 자릿수와 체크할 수의 개수가 같다면
                break
            else:
                answer.pop()
        if len(answer)+1 <= total_len: #자릿수를 유지하기 위한 조건
            answer.append(arr[i])
    
    for i in answer:
        res += str(i)
    
    return res
```

## 다른 풀이와의 차이

나의 경우는 미리 k를 빼서 자릿수를 정해두고(number가 1324이고, k=1이라면 자릿수는 3 이런식으로..), 이 자릿수를 기준으로 계산했는데 다른 사람들은 보통 k를 감소시키는 형식으로 한 것 같다. 그러다보니 조건문 처리할 때 코드 가독성이 더 좋다. 마지막 문자열 합치는 부분도 한 줄로 더 깔끔하게 표시하면 좋을 것 같다.

```python
def solution(number, k):
    answer = [] 
    
    for num in number:
        while k > 0 and answer and answer[-1] < num:
            answer.pop()
            k -= 1
        answer.append(num)
        
    return ''.join(answer[:len(answer) - k])
```

## 2차 문제 풀이(JS)

```jsx
function solution(number, k) {
    const answer = [];

    for (let num of number) {
        while (k > 0 && answer.length > 0 && answer[answer.length - 1] < num) {
            answer.pop();
            k--;
        }
        answer.push(num);
    }

    return answer.slice(0, answer.length - k).join('');
}
```

## 문제점 & 깨달은 점

- 파이썬은 고급 언어다. 이를 위한 문법을 더 잘 활용해보자.
- 필요한 TC 짜는 법을 아직도 잘 모르겠다. 무슨 좋은 방법이 있을까? (방법을 아는 사람이 있다면 제안plz)

## Reference

[https://velog.io/@soo5717/프로그래머스-큰-수-만들기-파이썬](https://velog.io/@soo5717/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%ED%81%B0-%EC%88%98-%EB%A7%8C%EB%93%A4%EA%B8%B0-%ED%8C%8C%EC%9D%B4%EC%8D%AC)