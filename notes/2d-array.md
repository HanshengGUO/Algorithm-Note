# 2D数组的遍历
## 151. 反转字符串中的单词
调用API的话则题目非常简单，主要是Java Api的一些记忆，列举如下：
```Java
class Solution {
    public String reverseWords(String s) {
        // 除去开头和末尾的空白字符
        s = s.trim();
        // 正则匹配连续的空白字符作为分隔符分割
        // 用Split来分割字符串，注意正则匹配规则
        List<String> wordList = Arrays.asList(s.split("\s+")); // 这里\s+表示多个空格 
        // Collections提供字符串反转的API，和Python类似
        Collections.reverse(wordList);
        return String.join(" ", wordList);
    }
}
```
### 正则表达式
```agsl
() 是为了提取匹配的字符串。表达式中有几个()就有几个相应的匹配字符串。(\s*)表示连续空格的字符串。
[]是定义匹配的字符范围。比如 [a-zA-Z0-9] 表示相应位置的字符要匹配英文字符和数字。[\s*]表示空格或者*号。
{}一般用来表示匹配的长度，比如 \s{3} 表示匹配三个空格，\s{1,3}表示匹配一到三个空格。
(0-9) 匹配 '0-9′ 本身。 [0-9]* 匹配数字（注意后面有 *，可以为空）[0-9]+ 匹配数字（注意后面有 +，不可以为空）{1-9} 写法错误。
[0-9]{0,9} 表示长度为 0 到 9 的数字字符串
```
方法二：可以利用双端队列（或者一个队列+双指针）
```Java
class Solution {
    public String reverseWords(String s) {
        int left = 0, right = s.length() - 1;
        while (s.charAt(left) == ' '){
            left += 1;
        }
        while (left < right && s.charAt(right) == ' '){
            right -= 1;
        }

        Deque<String> queue = new ArrayDeque<String>();
        StringBuffer sb = new StringBuffer();

        while (left <= right){
            char c = s.charAt(left);
            if (c == ' ' && sb.length() != 0){
                queue.addFirst(sb.toString());
                sb.setLength(0);
            }else if (c != ' '){
                sb.append(c);
            }
            left ++;
        }
        queue.addFirst(sb.toString());

        return String.join(" ", queue);
    }
}
```
### 原地操作的解法
