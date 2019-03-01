# Leecode编程题

## 一、简单类型题

### 4、验证外星语言词典

某种外星语也使用英文小写字母，但可能顺序 `order` 不同。字母表的顺序（`order`）是一些小写字母的排列。给定一组用外星语书写的单词 `words`，以及其字母表的顺序 `order`，只有当给定的单词在这种外星语中按字典序排列时，返回 `true`；否则，返回 `false`。

**示例 1：**

```
输入：words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
输出：true
解释：在该语言的字母表中，'h' 位于 'l' 之前，所以单词序列是按字典序排列的。
```

**示例 2：**

```
输入：words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
输出：false
解释：在该语言的字母表中，'d' 位于 'l' 之后，那么 words[0] > words[1]，因此单词序列不是按字典序排列的。
```

**示例 3：**

```
输入：words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
输出：false
解释：当前三个字符 "app" 匹配时，第二个字符串相对短一些，然后根据词典编纂规则 "apple" > "app"，因为 'l' > '∅'，其中 '∅' 是空白字符，定义为比任何其他字符都小（更多信息）。
```

**提示：**

1. `1 <= words.length <= 100`
2. `1 <= words[i].length <= 20`
3. `order.length == 26`
4. 在 `words[i]` 和 `order` 中的所有字符都是英文小写字母。

#### 代码实现一：

```java
	public boolean isAlienSorted(String[] words, String order) {

		int mark = 0;
		for (int i = 1; i < words.length; i++) {
			//得到最短长度
			int minLenth = Math.min(words[i].length(), words[i - 1].length());
			//循环验证是否符合题目条件
			for (int j = 0; j < minLenth; j++) {
				//当前一个数据大于后一个数据，则直接返回false;
				//当前一个数据小于后一个数据，则表示是正确的，将标记改为1
				if (order.indexOf(words[i - 1].charAt(j)) > order.indexOf(words[i].charAt(j))) {
					return false;
				} else if (order.indexOf(words[i - 1].charAt(j)) < order.indexOf(words[i].charAt(j))) {
					mark = 1;
					break;
				}
			}
			//如果前一个单词的长度大于后一个单词的长度，并且标记还为1，则也不符合条件
			if (words[i - 1].length() > words[i].length() && mark != 1) {
				return false;
			} else {
				mark = 1;
			}
		}
		if (mark == 1) {
			return true;
		} else {
			return false;
		}
	}
```

