# 算法 - 简单（一）

<a name="ZMukp"></a>
### 1 给定一个整数数组 nums 和一个目标值 target ，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。<br />
<br />**示例：**

```
给定 nums = [2,7,11,15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0,1]
```

**解法1**：暴力法

暴力法很简单，遍历每个元素x，并查找是否存在一个值与 target - x 相等的目标元素。

```java
class Solution {
	public int[] twoSum(int[] nums, int target){
    	for(int i =0;i<nums.length;i++){
        	for(int j = i + 1;j<nums.length;j++){
                if(nums[j] == target - nums[i]){
                    return new int[]{i,j}
                }
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```

复杂度分析：

- 时间复杂度：$$O(n^2)$$，对于每个元素，我们试图通过遍历数组的其余部分来寻找它所对应的的目标元素，这将耗费$$O(n)
$$的时间。因此复杂度为$$O(n^2)$$。
- 空间复杂度：$$O(1)$$ 。


**解法2**：两遍哈希表

为了对运行时间复杂度进行优化，我们需要一种更有效的方法来检查数组中是否存在目标元素。如果存在，我们需要找出它的索引。保持数组中的每个元素与其索引相互对应的最好方法是什么？哈希表。

通过一空间换取速度的方式，我们可以将查找时间从 O(n) 降低到 O(1) 。哈希表正是为此目的而构建的，它支持以近似恒定的时间进行快速查找。我用“近似”来描述，是因为一旦出现冲突，查找用时可能会退化到 O(n)。但只要你仔细地挑选哈希函数，在哈希表中进行查找的用时应当被摊销为 O(1）。

一个简单的实现使用了两次迭代。在第一次迭代中，我们将每个元素的值和它的索引添加到表中。然后，在第二次迭代中，我们将检查每个元素所对应的目标元素（target - nums[i]）是否存在于表中。注意，该目标元素不能是 nums[i] 本身！

```java
class Solution {
	public int[] twoSum(int[] nums, int target){
    	Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0;i<nums.length;i++){
        	map.put(nums[i],i);
        }
        for(int i = 0;i<nums.length;i++){
        	int complement = target - nums[i];
            if(map.containsKey(comlement) && map.get(comlement) != i){
            	return new int[] {i,map.get(comlement)};
            }
        }
         throw new IllegalArgumentException("No two sum solution");
    }
}
```


复杂度分析：

- 时间复杂度：O(n)，我们把包含有 n 个元素的列表遍历两次。由于哈希表将查找时间缩短到 O(1)，所以时间复杂度为 O(n)。
- 空间复杂度：O(n)，所需的额外空间取决于哈希表中存储的元素数量，该表中存储了 n 个元素。


**解法3**：一遍哈希表

事实证明，我们可以一次完成。在进行迭代并将元素插入到表中的同时，我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。如果它存在，那我们已经找到了对应解，并立即将其返回。

```java
class Solution {
	public int[] twoSum(int[] nums,int target) {
    	Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0;i< nums.length; i++){
        	int complement = target - nums[i];
            if(map.containsKey(complement)){
            	return new int[] {map.get(complement),i};
            }
            map.put(nums[i], i};
        }
         throw new IllegalArgumentException("No two sum solution");
    }
}
```

复杂度分析：

- 时间复杂度：O(n)，我们只遍历了包含有 n 个 元素的列表一次。在表中进行的每次查找只花费 O(1) 的时间。
- 空间复杂度：O(n)，所需的额外空间取决于哈希表中存储的元素数量，该表最多需要存储 n 个元素。


---


<a name="uOwEs"></a>
### 2 给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1：
```
输入: 123
输出: 321
```

示例 2：
```
输入: -123
输出: -321
```

示例 3：
```
输入: 120
输出: 21
```

注意：假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为$$[-2^{31},2^{31}-1]$$**  **。请根据这个假设，如果反转后整数溢出那么就返回 0。

**解法1**：弹出和推入数字 & 溢出前进行检查思路<br />
<br />我们可以一次构建反转整数的以为数字。在这样做的时候，我们预先检查向原整数附加另一位数字是否会导致溢出。

算法：

- 反转整数的方法可以与反转字符串进行类比。
- 我们想重复“弹出”x 的最后一位数字，并将它“推入”到 rev 的后面。最后，rev 将与 x 相反。
- 要在没有辅助堆栈/数组的帮助下“弹出”和“推入”数字，我们可以使用数学方法。

```
//pop operation:
pop = x % 10;
x /= 10;

//push operation:
temp = rev * 10 + pop;
rev = temp;
```

但是，这种方法很危险，因为当 temp = rev.10 + pop 时会导致溢出。

幸运的是，实现检查这个语句是否会导致溢出很容易。

为了便于解释，我们假设 rev 是整数。

1. 如果 temp = rev.10 + pop 导致溢出，那么一定有  $$\text{rev} \geq \frac{INTMAX}{10}$$。
1. 如果  $$\text{rev} > \frac{INTMAX}{10}$$，那么 temp = rev.10 + pop 一定会溢出。
1. 如果  $$\text{rev} == \frac{INTMAX}{10}$$，那么只要 pop > 7，temp = rev.10 + pop 就会溢出。

当 rev 为负时可以应用类似地逻辑。

```java
class Solution {
	public int reverse(int x){
    	int rev = 0;
        while (x != 0){
        	int pop = x % 10;
            x /= 10;
            if(rev > Integer.MAX_VALUE / 10 || (rev == Integer.MAX_VALUE / 10
                                                && pop > Integer.MAX_VALUE % 10)){
            	return 0;
            } 
            if(rev<Integer.MIN_VALUE / 10 || (rev == Integer.MIN_VALUE / 10 
                                              && pop < Integer.MIN_VALUE % 10)){
            	return 0;
            }
            return rev * 10 + pop;
        }
        return rev;
    }
}
```

复杂度分析：

- 时间复杂度：O(log(x)), x 中大约有 $$log_{10}(X)$$ 位数字。
- 空间复杂度：O(1) 。


---

<a name="9eUpx"></a>
###  3 小A 和 小B在玩猜数字。小B每次从1,2,3中随机选择一个，小A 每次也从1,2,3中选择一个猜。他们一共进行三次这个游戏，请返回小A猜对几次？

输入的guess数组为小A每次的猜测，answer数组为小B每次的选择。guess和answer的长度都等于3。

示例1：
```
输入：guess = [1,2,3], answer = [1,2,3]
输出：3
解释：小A 每次都猜对了。
```

示例2：

```
输入：guess = [2,2,3], answer = [3,2,1]
输出：1
解释：小A 只猜对了第二次。
```


**解法1**：在3次的情况下完全没必要循环

```java
public int game(int[] guess,int[] answer){
	return (guess[0]==answer[0]?1:0)+(guess[1]==answer[1]?1:0)+(guess[2]==answer[2]?1:0);
}
```


---


<a name="x6eN9"></a>
### 4 判断一个整数是否是回文数。回文数是指正序（从左到右）和倒序（从右到左）读都是一样的整数。

示例1：
```
输入: 121
输出: true
```

示例2：
```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

示例3：
```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

**解法1**：反转一半数字

思路：映入脑海的第一个想法是将数字转换为字符串，并检查字符串是否为回文。但是，这需要额外的非常量空间来创建问题描述中所不允许的字符串。

第二个想法是将数字本身反转，然后将反转后的数字与原始数字进行比较，如果它们是相同的，那么这个数字就是回文。但是，如果反转后的数字大于 \text{int.MAX}int.MAX，我们将遇到整数溢出问题。

按照第二个想法，为了避免数字反转可能导致的溢出问题，为什么不考虑只反转 \text{int}int 数字的一半？毕竟，如果该数字是回文，其后半部分反转后应该与原始数字的前半部分相同。

例如，输入 1221，我们可以将数字 “1221” 的后半部分从 “21” 反转为 “12”，并将其与前半部分 “12” 进行比较，因为二者相同，我们得知数字 1221 是回文。

算法：首先，我们应该处理一些临界情况。所有负数都不可能是回文，例如：-123 不是回文，因为 `-` 不等于 `3`。所以我们可以对所有负数返回 false。

现在，让我们来考虑如何反转后半部分的数字。

对于数字 1221，如果执行 1221 % 10，我们将得到最后一位数字 1，要得到倒数第二位数字，我们可以先通过除以 10 把最后一位数字从 1221 中移除，1221 / 10 = 122，再求出上一步结果除以 10 的余数，122 % 10 = 2，就可以得到倒数第二位数字。如果我们把最后一位数字乘以 10，再加上倒数第二位数字，1 * 10 + 2 = 12，就得到了我们想要的反转后的数字。如果继续这个过程，我们将得到更多位数的反转数字。

现在的问题是，我们如何知道反转数字的位数已经达到原始数字位数的一半？

我们将原始数字除以 10，然后给反转后的数字乘上 10，所以，当原始数字小于反转后的数字时，就意味着我们已经处理了一半位数的数字。

```java
public class Solution{
	public boolean isPalindrome(int x) {
        
    	// 特殊情况：
        // 如上所述，当 x < 0 时，x 不是回文数。
        // 同样地，如果数字的最后一位是 0，为了使该数字为回文，
        // 则其第一位数字也应该是 0
        // 只有 0 满足这一属性
        if(x < 0 || (x % 10 == 0 && x != 0){
        	return false;
        }
        int revertedNumber = 0;
        while(x > revertedNumber){
        	revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }
           
        // 当数字长度为奇数时，我们可以通过 revertedNumber/10 去除处于中位的数字。
        // 例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 x = 12，revertedNumber = 123，
        // 由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除。
        return x == revertedNumber || x == revertedNumber / 10;
    }
}
```

复杂度分析：

- 时间复杂度：$$O(log_{10}(n))$$，对于每次迭代，我们会将输入除以10，因此时间复杂度为$$O(log_{10}(n))$$。
- 空间复杂度：$$O(1)$$。


**解法2**：整数转字符串（不推荐）

思路：将整数转为字符串，然后将字符串分割为数组只需要玄幻数组的一半长度进行判断对应元素是否相等即可。

```java
public class Solution{
	public boolean isPalindrome(int x) {
    	String reversedStr = (new StringBuilder(x + "")).reverse().toString();
        return (x + "").equals(reversedStr);
    }
}
```


---


<a name="dz7H3"></a>
### 5 罗马数字转整数

题目描述：




---

<a name="u5q4D"></a>
### 6 最长公共前缀


---

<a name="JM4C1"></a>
### 7 有效的括号

**题目描述**：

给定一个只包括 '(' , ')' , '['  , ']' , '{' , '}'  的字符串，判断字符串是否有效。<br />有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
1. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

示例1：
```
输入: "()"
输出: true
```

示例2：
```
输入: "()[]{}"
输出: true
```

示例3：
```
输入: "(]"
输出: false
```

示例4：
```
输入: "([)]"
输出: false
```

示例5：
```
输入: "{[]}"
输出: true
```

**解法1**：栈

关于有效括号表达式的一个有趣属性是有效表达式也应该是有效表达式。（不是每个子表达式）例如<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1573284839915-3b56d6ef-7f9b-4aab-88fa-53cd500c770f.png#align=left&display=inline&height=359&name=image.png&originHeight=383&originWidth=687&search=&size=32475&status=done&width=644)

算法：

1. 初始化栈 S。
1. 一次处理表达式的每个括号。
1. 如果遇到开括号，我们只需要将其推到栈上即可。这意味着我们将稍后处理它，让我们简单地转到前面的**子表达**。
1. 如果我们遇到一个闭括号，那么我们检查栈顶的元素。如果栈顶的元素是一个 相同类型的 左括号，那么我们将它从占中弹出并继续处理。否则，这意味着表达式无效。
1. 如果到最后我们剩下的栈中仍然有元素，那么这意味着表达式无效。

```java
class Solution{
	private HashMap<Character,Character> mappings;
    
    public Solution() {
    	this.mappings.put(')', '(');
    	this.mappings.put('}', '{');
    	this.mappings.put(']', '[');
    }
    
    public boolean isValid(String s) {
    	Stack<Character> stack = new Stack<Character>();
        
        for(int i = 0;i < s.length(); i++) {
        	char c = s.charAt(i);
            
            if(this.mappings.containsKey(c)){
            	char topElement = stack.empty() ? '#' : stack.pop();
                if(topElement != this.mappings.get(c)) {
                	return false;
                }
            }else {
            	stack.push(c);
            }
        }
        return stack.isEmpty();
    }
}
```

复杂度分析：

- 时间复杂度：O(n)，因为我们一次只遍历给定的字符串中的一个字符并在栈上进行 O(1) 的推入和弹出操作。
- 空间复杂度：O(n)，当我们将所有的开括号都推到栈上时以及在最糟糕的情况下，我们最终要把所有括号推到栈上。例如 (((((((。

---

<a name="WnPqe"></a>
### 8 合并两个有序链表

**题目描述：**

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有结点组成的。

示例：
```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

**解法1**：递归

思路：我们可以如下递归地定义在两个链表里的 merge 操作（忽略边界情况，比如空链表等）：



**解法2**：

---

<a name="WQl4m"></a>
### 9 删除排序数组中的重复项

**题目描述：**<br />
<br />给定一个排序数组，你需要在原地删除重复出现的元素，使得每一个元素只出现一次，返回移除后数组的新长度。<br />不要使用额外的数组空间，你必须在原地修改输入数组并在使用O(1)额外空间的条件下完成。

示例1：
```
给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
```

示例2：
```
给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。

```

说明：<br />为什么返回数值是整数，但输出的答案是数组呢？<br />请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。<br />你可以想象内部操作如下：
```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}

```

**解法1**：双指针法

算法：<br />数组完成排序后，我们可以放置两个指针 i 和 j ，其中 i 是慢指针，而 j 是快指针。只要 nums[i] = nums[j] ，我们就增加 j 以跳过重复项。

当我们遇到  $$nums[j] \neq nums[i]$$ 时，跳过重复项的运行已经结束，因此我们必须把它 （nums[j]）的值复制到 nums[i+1] 。然后递增 i ，接着我们将再次重复相同的过程，知道 j 到达数组的末尾为止。

```java
public int removeDupLicates(int[] nums){
	if(nums.length == 0){
    	return i=0;
    }
    int i = 0;
    for(int j=1; j<nums.length;j++){
    	if(nums[j] != nums[i]){
        	i++;
            nums[i] = nums[j];
        }
    }
    return i+1;
}
```

复杂度分析：

- 时间复杂度：O(n) ，假设数组的长度是 n ，那么 i 和 j 分别最多遍历 n 步。
- 空间复杂度：O(1) 。


---


<a name="Nq3bC"></a>
### 10 移除元素

**题目描述**：

给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。<br />元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

示例1：
```
给定 nums = [3,2,2,3], val = 3,

函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。

你不需要考虑数组中超出新长度后面的元素。
```

示例2：
```
给定 nums = [0,1,2,2,3,0,4,2], val = 2,

函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。
```

说明：<br />为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下：
```
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

解法1：双指针

思路：既然问题要求我们就地删除给定值的所有元素，我们就必须用 O(1)O(1) 的额外空间来处理它。如何解决？我们可以保留两个指针 ii 和 jj，其中 ii 是慢指针，jj 是快指针。

算法：当 nums[j] 与给定的值相等时，递增 j 以跳过该元素。只要 $$nums[j] \neq val$$ ，我们就复制 nums[j] 到 nums[i] 并同时递增两个索引。重复这一过程，直到 j 到达数组的末尾，该数组的新长度为 i。

```java
public int removeElement(int[] nums, int val){
	int i = 0;
    for (int j = 0; j < nums.length; j++){
    	if(nums[j] != val){
        	nums[i] = nums[j];
            i++;
        }
    }
    return i;
}
```

复杂度分析：

- 时间复杂度：O(n)，假设数组总共有 n 个元素， i 和 j 至少遍历 2n 步。
- 空间复杂度：O(1)。


解法2：双指针 —— 当要删除的元素很少时

思路：<br />现在考虑数组包含很少的要删除的元素的情况。例如，num = [1,2,3,5,4]，val = 4。之间的算法会对前四个元素做不必要的复制操作。另一个例子是 num = [4,1,2,3,5]，val = 4。似乎没有必要将[1,2,3,5] 这几个元素左移一步，因为问题描述中提到元素的顺序可以更改。

算法：<br />当我们遇到 nums[i] = val 时，我们可以将当前元素与最后一个元素进行交换，并释放最后一个元素。这实际使数组的大小减少了1。

请注意，被交换的最后一个元素可能是您想要移除的值。但是不要担心，在下一次迭代中，我们仍然会检查这个元素。

```java
public int removeElement(int[] nus, int val){
	int i = 0;
    int n = nums.length;
    while(i<n){
    	if(nums[i] == val){
        	nums[i] = nums[n-1];
            n--;
        }else{
        	i++;
        }
    }
    return n;
}
```

复杂度分析：

- 时间复杂度：O(n), i 和 n 最多遍历 n 步。在这个方法中，赋值操作的次数等于要删除的元素的数量。因此，如果要移除的元素很少，效率会更高。
- 空间复杂度：O(1)。




















