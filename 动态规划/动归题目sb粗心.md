一维`dp`遍历容量时`j`写成了`j++`

```c++
for(int i = 0; i < stones.size(); i++){
    for(int j = target; j >= stones[i]; j--){
    dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
    }
}
```

