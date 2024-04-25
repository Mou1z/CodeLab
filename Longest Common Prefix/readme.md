# Longest Common Prefix

- **Source:** https://leetcode.com/problems/longest-common-prefix
- **Language Used:** C++

### Results
<img src="">

### Explanation / Commentary
Initially, when I tried solving this problem, I ended up with this solution, and it was giving me a speed score of about 80%. I have to accept, the following code is quite ugly and inefficient according to my standards and there are a few mistakes you might be able to spot as well. However, by the end of this article, this will be turned into something much better.

```cpp
string longestCommonPrefix(vector<string>& strs) {
    int len = strs.size();

    if(len == 1)
        return strs[0];

    bool common;
    string prefix = "";
    for(int c = 0; c < strs[0].size(); c++) {
        common = true;

        for(int s = 1; s < len; s++) {
            if(strs[s][c] == '\0')
                return prefix;

            if(strs[s][c] != strs[0][c])
                common = false;
        }

        if(common)
            prefix += strs[0][c];
        else
            break;
    }
    return prefix;
}
```

So what it's doing is basically the following steps:
1. Store the size of `strs.size()` in `len` since it's used twice.
2. Check if `len` is `1` - which basically means that there's only one string in the vector. If it's true then return that string.
3. It creates two variables, `prefix` for storing the final result, and a bool called `common` which we will use to check if a certain character is common among all the strings - every iteration.
4. The first for-loop iterates through all the characters of the first string.
5. The second inner for-loop iterates through the remaining strings. For every string, it first checks if the current character of that string is equal to `\0` which is the **string termination character**, if it is then it returns the prefix since we've reached the end of a string in the vector so it doesn't make sense to continue. If the loop passes that condition then it checks for the next condition, which is **whether the current character of the string is equal to the character at same index of the first string**, and if it's not then `common` is set to `false` indicating that the character at that index is not common for all strings.
6. Outside of the inner loop, it checks if `common` is still `true`, if it's true then the recently read character has passed the two tests and it is added to the prefix. If `common` is `false` then the main for loop is broken and the function returns the `prefix`.


This however, can be improved much more. For-example, after the second condition, once `common` is set to `false`, we should also break the loop because there's no need to look further. However, wait a minute ... , so since we're breaking the loop anyways, and then returning `prefix` with extra steps later, why not just return under the second condition? Yes, that's what we're going to do, and this way we don't even need to use `common` anymore!

```cpp
string longestCommonPrefix(vector<string>& strs) {
    int len = strs.size();

    if(len == 1)
        return strs[0];

    string prefix = "";
    for(int c = 0; c < strs[0].size(); c++) {

        for(int s = 1; s < len; s++) {
            if(strs[s][c] == '\0')
                return prefix;

            if(strs[s][c] != strs[0][c])
                return prefix;
        }

        prefix += strs[0][c];
    }
    return prefix;
}
```

We can clean this up a bit more by using an OR (`||`) operator to join both the conditions since they're performing the same operation `return prefix;`:

```cpp
string longestCommonPrefix(vector<string>& strs) {
    int len = strs.size();

    if(len == 1)
        return strs[0];

    string prefix = "";
    for(int c = 0; c < strs[0].size(); c++) {

        for(int s = 1; s < len; s++) {
            if(strs[s][c] == '\0'       ||
               strs[s][c] != strs[0][c]  )
               
                return prefix;
        }

        prefix += strs[0][c];
    }
    return prefix;
}
```

It's already looking much better. However, the `prefix` string takes up extra memory, and in case of large strings, it will unnecessarily occupy alot of extra memory, which is what's bothering me a bit. So we can remove `prefix` and just use the `substr` method of strings to slice and return the relevant portion of the string. We already have a good indicator of till where we've read the first string, and that's the variable `c`. So we can slice up the first string up till `c` and return it:

```cpp
if(strs[s][c] == '\0'       ||
   strs[s][c] != strs[0][c]  )           
    return str[0].substr(0, c);
```

And for the return statement at the end of the function, we can simply return the first string itself since it means the main loop completed without being interrupted and hence the first string should be the largest substring. So the following will be the final code:

```
string longestCommonPrefix(vector<string>& strs) {
    int len = strs.size();

    if(len == 1)
        return strs[0];

    for(int c = 0; c < strs[0].size(); c++) {
        for(int s = 1; s < len; s++) {
            if(strs[s][c] == '\0'       ||
               strs[s][c] != strs[0][c]  )
               
                return strs[0].substr(0, c);
        }
    }
    
    return strs[0];
}
```


### Solution
For the final solution code I cleaned it up a bit more, removing the `len` variable since it didn't look too good and it didn't make any difference in terms of performance either.
```cpp
string longestCommonPrefix(vector<string>& strs) {
    for(int c = 0; c < strs[0].size(); c++) {
        for(int s = 1; s < strs.size(); s++) {
            if( strs[s][c] == '\0'      ||
                strs[s][c] != strs[0][c] )

                return strs[0].substr(0, c);
        }
    }
    return strs[0];
}
```
