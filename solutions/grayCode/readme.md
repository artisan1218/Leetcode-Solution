# Gray Code problem
![image](https://user-images.githubusercontent.com/25105806/133962180-9e988dfe-aaab-4d2e-bbda-ce54ead69865.png)

Leetcode link: https://leetcode.com/problems/gray-code/

<br/>

### Approach 1: Backtrack, grayCodeBacktrack()
The idea is straightforward, we use recursion to try all possible ways of modifying 1 bit of the previous digit in the list and append the valid digit to result list

```python
def grayCodeBacktrack(self, n: int) -> List[int]:
    def backtrack(n, curBin, result, used):
        result.append(curBin)
        used.add(curBin)

        if len(result) < 2**n:
            curBin = result[-1]
            for i in range(len(curBin)):
                # change one digit
                curBin = curBin[:i] + '1' + curBin[i+1:] if curBin[i]=='0' else curBin[:i] + '0' + curBin[i+1:]
                if curBin not in used:
                    # we've found a valid change
                    break
                else:
                    # change it back since this is not a valid option
                    curBin = curBin[:i] + '1' + curBin[i+1:] if curBin[i]=='0' else curBin[:i] + '0' + curBin[i+1:]

            backtrack(n, curBin, result, used)

    start = ''.join(['0' for _ in range(n)])
    binaryResult = []
    backtrack(n, start, binaryResult, set())

    return [int(binary, 2) for binary in binaryResult]
```

Actual running time:\
![image](https://user-images.githubusercontent.com/25105806/133962316-75b5fab0-8b1e-4f6a-baa0-7b1ed0e255c0.png)

<br/>

### Approach 2: Bit Manipulation, grayCodeBit()
Since each new digit is only depend on the previous digit, we can use iteration to add new digit iteratively, each new digit is generated by the bit manipulation `i^(i>>1)` where is the ith number

```python
def grayCodeBit(self, n: int) -> List[int]:
    result = []
    for i in range(1<<n):
        result.append(i^(i>>1))
    return result
```

Time complexity is O(2^n) because there are a total of 2^n digits in the list:\
![image](https://user-images.githubusercontent.com/25105806/133962477-c4e6c509-3a04-4ce7-9d8e-45b9ed2f6f44.png)