# Converting Roman Numbers to Integers

- **Source:** https://leetcode.com/problems/roman-to-integer/
- **Language Used:** C++

The reason I'm sharing this solution is because apparently my solution is faster than 100% of the other submissions for C++, therefore I think this solution can be featured in this repository.

<img src="https://github.com/Mou1z/CodeLab/blob/main/romanToInt%20Problem/result.PNG">

```cpp
class Solution {
    public:
        int romanToInt(string s) {
            int result = 0, current, last = 1000;
            for(int i = 0; i < s.length(); i++) {
                if(s[i] == 'I')
                    current = 1;
                else if(s[i] == 'V')
                    current = 5;
                else if(s[i] == 'X')
                    current = 10;
                else if(s[i] == 'L')
                    current = 50;
                else if(s[i] == 'C')
                    current = 100;
                else if(s[i] == 'D')
                    current = 500;
                else if(s[i] == 'M')
                    current = 1000;
                
                if(current <= last) {
                    result += current;
                } else {
                    result -= last;
                    current -= last;
                    result += current;
                }
                last = current;
            }

            return result;
        }
};
```

### Explanation
The solution is quite simple, it creates three variables:
- `result` - To hold the final result;
- `current` - To hold the value of the letter which was / is read in the current interation.
- `last` - To hold the value of the letter which was read in the last iteration;

1. The for loop goes through each letter uses a simple if-else chain to figure out and store a relevant value in the variable `current`.
2. The next conditon checks if `current` is less or equal to `last`. If it is, it simply adds it to the `result` because the roman numerals are normally arranged in a descending order so it's a normal case. 
2. If `current` is NOT less than `last` then it means we need to substract `last` from `current` which we do (For-example in case 'IV', 1 should be substracted from 5). We also substract `last` from `result` because it was added to the `result` in the previous iteration however in this iteration we've figured out that it should've not. We then add the value of `current` to `result`.
3. Finally, we set the value of `last` to `current` to keep the variable `last` updated every iteration.

The code could've been fancier however I wanted to keep it readable.

### Interesting Discovery
While I was thinking of solving this problem, I write down the pattern `1, 5, 10, 50, 100, 500` on a paper and eventually I was able to derive a formula to mathematically generate this pattern.

Formula: 5ˣ . 2ʸ
- 'y' is equal to 'n // 2' where `n` is the term index, if starting from 0. (`//` is the symbol of floor division)
- 'x' is equal to 'n - y'

5⁰ . 2⁰ = 1
5¹ . 2⁰ = 5
5¹ . 2¹ = 10
5² . 2¹ = 50
5² . 2² = 100
5³ . 2² = 500
5³ . 2³ = 1000

This can be useful in calculating the value of a certain roman numeral if they're stored in an enum in ascending order:
```cpp
enum RomanNumeral {
    I = 0,
    V = 1,
    X = 2,
    L = 3,
    C = 4,
    D = 5,
    M = 6
};

int i = M;
int x = i / 2;

int value = pow(5, i - x) * (1 << x);

cout << value; // 1000
```