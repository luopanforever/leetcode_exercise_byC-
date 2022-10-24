给你两个字符串：`ransomNote` 和 `magazine` ，判断 `ransomNote` 能不能由 `magazine` 里面的字符构成。

如果可以，返回 `true` ；否则返回 `false` 。

`magazine` 中的每个字符只能在 `ransomNote` 中使用一次。

 

**示例 1：**

```
输入：ransomNote = "a", magazine = "b"
输出：false
```

**示例 2：**

```
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

**示例 3：**

```
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

 

**提示：**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` 和 `magazine` 由小写英文字母组成



**解题思路：**

利用map建立hash表，根据题意，该hash表不要求有序，故可以使用unordered_map提升效率，将ransomNote和magazine分别放入一个hash表中，key放字符acell值，value放acell值出现的次数，接着遍历ransomNote查看result2是否存在ransomNote的字符，如果存在，分两种情况，一是result2中的value大于1，则直接对value-1，二是result2中的value等于1，则直接对result2删除元素。同理对result2操作的同时也同样对result1操作就行。最后判断result1是否为空然后返回结果。为空的就为true。



**代码：**

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<int,int> result1;
        unordered_map<int,int> result2;
        for(auto ransomNote_f :ransomNote){
            result1[ransomNote_f]++;
        }
        for(auto magazine_f :magazine){
            result2[magazine_f]++;
        }
        for(auto ransomNote_f :ransomNote){
            unordered_map<int,int>::iterator it = result2.find(ransomNote_f);
            if(it != result2.end()){
                if(it->second == 1){
                    result2.erase(it);
                }else{
                    --it->second;
                }
                if(result1[ransomNote_f] != 1){
                    result1[ransomNote_f]--;
                }else{
                    result1.erase(ransomNote_f);
                }
            }
        }
        if(result1.empty()){
            return true;
        }
        return false;
        
    }
};
```

