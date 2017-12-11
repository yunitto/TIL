# Questions

## Q1

0 뒤로 밀어넣기
> def( [0, 1, 0, 5, 3] ) &rarr; [1, 5, 3, 0, 0]
* 0을 제외한 수는 순서를 유지하며 앞으로, 0은 모두 뒤로.
* 제약사항: 새로운 배열 할당 없이 할 것

정답 O(n)
```
def sort(arr):
    count = 0
    idx = 0
    for num in arr:
        if num != 0:
            arr[idx] = num
            idx += 1
        else:
            count += 1
    
    for i in range(idx, len(arr)):
        arr[i] = 0

    return arr
```
좋지 않은 코드( O(n^2) )
```
def sort(arr):
    for i in range(len(arr)):
        if arr[i] == 0:
            for j in range(i, len(arr)):
                if arr[j] != 0:
                    break
            arr[i], arr[j] = arr[j], arr[i]

    return arr
```

## Q2

majority number 찾기
> def( [1, 2, 1, 2, 2, 2] ) &rarr; 2
* majority number: 해당 숫자의 총 개수 > (length/2)
* 제약사항: 새로운 자료구조 없이 할 것
* 시간복잡도: O(n)

정답
```
def majority(arr):
    count = 0
    idx = 0

    for i in range(len(arr)):
        if count == 0:
            idx = i
            count = 1
        elif arr[idx] == arr[i]:
            count += 1
        else:
            count -= 1

    return arr[idx]
```
 
 여기까지 쓰고나니 드는 의문
 
 1. 만약 `[1, 1, 1, 2, 2, 2]` 라면 `candidate_idx = 5, count = 1 ` 이라서 2를 리턴하지만, majority number 가 되려면 개수가 4개 이상이어야 하는데?

 2. `[6, 1, 6, 2, 6, 3, 4, 6]` 라면 `4`을 리턴하는데, Is this majority number? What about `6`?

 # Boyer-Moore majority vote algorithm
[Wikipedia](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_majority_vote_algorithm)

> In its simplest form, the algorithm finds a majority element, if there is one: that is, an element that occurs repeatedly for more than half of the elements of the input. However, if there is no majority, the algorithm will not detect that fact, and will still output one of the elements. A version of the algorithm that makes a second pass through the data can be used to verify that the element found in the first pass really is a majority.

> The algorithm will not, in general, find the mode of a sequence (an element that has the most repetitions) unless the number of repetitions is large enough for the mode to be a majority. It is not possible for a streaming algorithm to find the most frequent element in less than linear space, when the number of repetitions can be small.

unique 한 숫자가 2개 뿐이라면 이 알고리즘으로 majority number을 찾을 수 있지만 그 이상일 경우에는 보장되지 않는다. 또한 정말 majority 인지 확인하려면 추가적인 방법을 써야한다. 

> 오늘의 교훈: 면접할 때 너무 많은 케이스를 생각하면 오히려 못푼다. 우선 주어진 조건만 생각하기.
