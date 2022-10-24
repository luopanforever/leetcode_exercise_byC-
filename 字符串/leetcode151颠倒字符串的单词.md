#### 题目描述

给你一个字符串 `s` ，颠倒字符串中 **单词** 的顺序。

**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。

**注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

 

**示例 1：**

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**示例 2：**

```
输入：s = "  hello world  "
输出："world hello"
解释：颠倒后的字符串中不能存在前导空格和尾随空格。
```

**示例 3：**

```
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，颠倒后的字符串需要将单词间的空格减少到仅有一个。
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 包含英文大小写字母、数字和空格 `' '`
- `s` 中 **至少存在一个** 单词

#### 题解思路：

要想让空间复杂度为O（1），只能原地修改该字符串，可以通过三个步骤

- 去掉多余空格，去掉字符串前后的空格
- 翻转整个字符串
- 翻转其中的子字符串

**首先实现去掉多余空格的操作：**

利用快慢指针实现此操作，基本思路为，快指针来跳过最前面的空格和中间重复的空格，然后慢指针挨个替换快指针指向的有效值（有效值就是我们最终想得到的字符串里面的各个元素），最终慢指针指向的就是有效字符串的 **下一个位置** 。

```c++
void deleteblank(string &s) {
	int slowptr = 0;
	int fastptr = 0;
	//去除前面的空格
	while (s.size() > 0 && fastptr < s.size()&& s[fastptr] == ' ') {  //第一个条件是为了防止第三个条件访问nullptr
			fastptr++;
	}
	//利用快慢指针删除中间多余的空格
	for (; fastptr < s.size(); fastptr++) {
		if ((fastptr > 1)&&s[fastptr - 1] == ' '&&s[fastptr] == ' ') { //第一个条件是为了防止第二个条件访问nullptr
			continue;
		}
		else {
			s[slowptr++] = s[fastptr];
		}
	}
	//去掉后面多余的空格
    /*
    此条件不可以写成slowptr > 1 &&s[slowptr] == ' '
    因为在删除中间多余空格的时候对slowptr使用的后++
    所以上述循环结束的时候slowptr会在有效字符串的尾位置的下一个位置
    */
	if (slowptr > 1 &&s[slowptr - 1] == ' ') {/
		s.resize(slowptr - 1);
	}
	else {
		s.resize(slowptr);
	}
}
```



**实现翻转字符串的**

利用左右对撞指针即可，由于实现子字符串也需要用到reverseAll，所以多加了两个参数，更加灵活。

```c++
void reverseAll(string &s, int start, int end) {
	int left = start;
	int right = end;
	while (left < right) {
		swap(s[left++], s[right--]);
	}
}
```

**实现翻转子字符串**

在遇到空格的时候翻转字符串即可

```c++
string reverseWords(string s) {
	deleteblank(s);  //执行消除前后空格以及中间多余空格操作
	reverseAll(s, 0, s.size() - 1); //执行翻转整个数组操作
	int start = 0;	//翻转子字符串的起始位置
	int end = 0;	//翻转子字符串的终止位置
	bool inword = false;  //设置一个标志符，代表某层循环是否是处于子字符串内部
	for (int i = 0; i < s.size(); i++) {
		//将标志位置为true 使得下面的if语句的第一条件成立
        if (!inword) {
		 	start = i;
			inword = true;
		}
        //在第一条件成立的前提下（处于字符内部，寻找终止条件，将值赋给end）
		if (inword && s[i] == ' ' &&s[i - 1] != ' ') { //由于字符串前面的空格已经被删除，所以不需要添加i>1的条件
			end = i - 1;
			inword = false;
			 reverseAll(s, start, end);
		}
		//此if语句为最后一个单词服务
		if (inword && (i == s.size() - 1) && s[i] != ' ') {
			end = i;
			inword = false;
			reverseAll(s, start, end);
		}
	}
	return s;
}
```



**完整代码：**

```c++
class Solution {
public:
    void deleteblank(string &s) {
	    int slowptr = 0;
	    int fastptr = 0;
	    while (s.size() > 0 && fastptr < s.size()&& s[fastptr] == ' ') {  
	   		fastptr++;
	    }
	    for (; fastptr < s.size(); fastptr++) {
	    	if ((fastptr > 1)&&s[fastptr - 1] == ' '&&s[fastptr] == ' ') { //最后一个条件是为了防止第一个条件访问nullptr
	    		continue;
	    	}
	    	else {
	    		s[slowptr++] = s[fastptr];
	    	}
	    }
	    //去掉后面多余的空格
	    if (slowptr > 1 && s[slowptr - 1] == ' ') {
	    	s.resize(slowptr - 1);
	    }
	    else {
		    s.resize(slowptr);
	    }
	
    }
    void reverseAll(string &s, int start, int end) {
	    int left = start;
	    int right = end;
	    while (left < right) {
		    swap(s[left++], s[right--]);
	    }
    }

    string reverseWords(string s) {
	    deleteblank(s);
	    reverseAll(s, 0, s.size() - 1);
	    int start = 0;
	    int end = 0;
	    bool inword = false;
	    for (int i = 0; i < s.size(); i++) {
		    if (!inword) {
			    start = i;
			    inword = true;
		    }
		    if (inword && s[i] == ' ' &&s[i - 1] != ' ') {
			    end = i - 1;
			    inword = false;
			    reverseAll(s, start, end);
		    }

		    if (inword && (i == s.size() - 1) && s[i] != ' ') {
			    end = i;
			    inword = false;
			    reverseAll(s, start, end);
		    }
	    }
	    return s;
    }
};
```

**总结**

在条件判断的时候，如果有对索引+1或者-1操作的一定要注意防止访问空指针
