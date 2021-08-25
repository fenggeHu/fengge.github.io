完结。以后直接写在力扣的笔记里。

欢迎关注俺的力扣 [csjue](https://leetcode-cn.com/u/csjue/)

### Q1两数之和 

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
难度 简单
示例:
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

~~~java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int [] answer = new int[2];
        for(int i = 0;i<nums.length;i++){
            for(int j = i+1;j<nums.length;j++){
                if(nums[i]+nums[j]==target){
                    answer[0] = i;
                    answer[1] = j;
                    return answer;
                }
            }
        }
        return answer;
    }
}
~~~
这题也很容易想到n2的解法。竟然也击败了43%

~~~java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int [] answer = new int[2];
        HashMap<Integer,Integer> m = new HashMap<>();
        for(int i = 0;i<nums.length;i++){
            if(m.get(target-nums[i])!=null){
                answer[0] = m.get(target-nums[i]);
                answer[1] = i;
                return answer;
            }
            else{
                m.put(nums[i],i);
            }
        }
        return answer;
    }
}
~~~
改用map来写，map存nums[i]和位置。复杂度n。击败99%

### Q387字符串中的第一个唯一字符
给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。
难度 简单
示例：
s = "leetcode"
返回 0
s = "loveleetcode"
返回 2

~~~java
class Solution {
    public int firstUniqChar(String s) {
        if(s.length()==1)
            return 0;
        for(int i = 0;i<s.length();i++){
            int flag = 1;
            for(int j = 0;j<s.length();j++){
                if(i!=j&&s.charAt(i) == s.charAt(j)){
                    flag = 0;
                    break;
                }
            }
            if(flag==1)
                return i;
        }
        return -1;
    }
}
~~~

随便写了个n2的算法，击败8.25%

改用HaspMap
~~~java
class Solution {
    public int firstUniqChar(String s) {
        HashMap<Character,Integer> count = new HashMap<>();
        for(int i = 0;i < s.length();i++){
            count.put(s.charAt(i),count.getOrDefault(s.charAt(i),0)+1);
        }
        for(int i = 0;i<s.length();i++){
            if(count.get(s.charAt(i))==1)
                return i;
        }
        return -1;
    }
}
~~~
之前没怎么用过这个方法
default V getOrDefault(Object key, V defaultValue) 
返回到指定键所映射的值，或 defaultValue如果此映射包含该键的映射。  

这个n的算法，没想到才击败29%

看了下最快的
~~~java
class Solution {
    public int firstUniqChar(String s) {
        int res = Integer.MAX_VALUE;
        for (char ch = 'a'; ch <= 'z'; ch++) {
            int index = s.indexOf(ch);
            if ( index!= -1 && index == s.lastIndexOf(ch)) {
                res = Math.min(res, index);
            }
        }
        return res == Integer.MAX_VALUE ? -1 : res;
    }
}
~~~
有点投机啊，赌都在a~z中。

前面的算法，基本都像上者，笃定是a~z中

### Q2两数相加
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
难度 中等
示例：
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

之前用C语言解出来过。这一题的通过率只有40%+，我还在想：就这？

接下来一小时，力扣教育了我（也就交了十一次

~~~java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode p1 = l1,p2=l2,pFro = l1;
        int more=0;
        while(p1!=null&&p2!=null){
            p1.val = p1.val + p2.val +more;
            more = p1.val/10;
            p1.val%=10;
            pFro = p1;
            p1 = p1.next;
            p2 = p2.next;
        }
        if(p1==null&&p2!=null){
            pFro.next = p2;
            p2.val = p2.val+more;
            more = p2.val/10;
            p2.val%=10;
            while(more!=0){
                if(p2.next!=null)
                    p2.next.val += more;
                else if(p2.next==null){
                    p2.next = new ListNode(more);
                }
                more = p2.next.val/10;
                p2.next.val%=10;
                p2=p2.next;
                
            }
            
            return l1;
        }
        else if(p1!=null&&p2==null){
            p1.val = p1.val+more;
            more = p1.val/10;
            p1.val%=10;
            while(more != 0){
                if(p1.next!=null)
                    p1.next.val += more;
                else{
                    p1.next = new ListNode(more); 
                }
                more = p1.next.val/10;
                p1.next.val%=10;
                p1 = p1.next;
            }
           
            return l1;
        }
        else{
            if(more !=0){
                pFro.next = new ListNode(more);
            }
            return l1;
        }
    }
}
~~~
思路其实很简单。第一个递归很容易就写出来了。但是递归完，有三种情况，l1长，l2长，l1 l2一样长。前两种，只要把next改一下就好了。想到这我就交了一次

结果没有考虑。 1->9;9;这种情况，然后又稍微改了一下。

又没有考虑9->9->9;1;这种情况...

期间由于魔改代码，多次改错，有时连一个例子都没过。

最后写出来，击败99% 

### Q19删除链表的倒数第N个节点
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
难度 中等
示例：
给定一个链表: 1->2->3->4->5, 和 n = 2.
当删除了倒数第二个节点后，链表变为 1->2->3->5.
说明：
给定的 n 保证是有效的。

进阶：
你能尝试使用一趟扫描实现吗？


这个题之前应该是写过类似的，数据结构笔试的时候好像有个删除倒二个节点，相比这个要简单得多，只要三个指针就搞定了。

直接尝试一趟扫描。用四个指针，f2为删除的那个点，f1之前那个，f3之后那个。l为最后一个节点。有点类似快慢指针，只要f3比l慢n-2个就可以了 
但是就是四个指针法只能应付比较长的数组

我们就得解决另外三种情况。
1：f1 null  f2  不为null  f3不为null
2：f1 null  f2  null      f3不为null
3: f1 null  f2  null      f3 null
只要稍微调几次参数，人肉跑一遍代码，基本就能调出来。

~~~java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode f1=null,f2=null,f3=null, l = head;
        if(n==1){
            if(l.next==null)
                return null;
            for(int i = 0;l.next!=null;l = l.next){
                f1 = l;
            }
            f1.next = null;
            return head;
        }
        for(int i = 0;l.next!=null;i++,l=l.next){
            if(i>=n-2){
                if(f3 == null){
                    f3 = head.next;
                    f2 = head;
                }
                else{
                    f1 = f2;
                    f2 = f3;
                    f3 = f3.next;
                }
            }
        }
        if(f1!=null)
            f1.next = f3;
        else if(f2!=null&&f3!=null)
            return f3;
        else if(f3!=null)
            return f3;
        return head;
    }
}
~~~
击败100%
### Q61旋转链表
给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。
难度 中等
示例 1:
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
示例 2:
输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL

其实就是先把链表弄成环，然后再找到新的头。

算准新头的位置很重要。 1-2-3-4-5，k=1时 头是5，k=2时 头是4。可以看出来 头的位置大概就是length - k%length 

~~~java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        ListNode last = head;
        if(k==0) return head;
        if(head==null) return head;  
        int num = 1;
        while(last.next!=null){
            last = last.next;
            num++;
        }
        last.next = head;
        int pos = k%num;
        ListNode p = head,realHead;
        int i = 1;      
        while(i<num- pos){
            p = p.next;
            i++;
        }
        realHead = p.next;
        p.next =null;
        return realHead;
    }
}
~~~

写的时候 头的位置一直没调好，花了一二十分钟调参。
击败89%

### Q206反转链表
反转一个单链表。
难度中等
示例:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

~~~java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null) return null;
        ListNode pre = null, now = head,next = head.next;
       
        while(now!=null){
            now.next = pre;
            pre = now;
            now = next;
            next = (next!=null)?next.next:null;
        }
        return pre;
    }
}
~~~

这题算是比较常规的一道题，只要有三个指针 pre now next，把now的next修改一下 每次向前移一下就好了。
击败100%

### Q25K个一组翻转链表
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。
难度 困难
示例：
给你这个链表：1->2->3->4->5
当 k = 2 时，应当返回: 2->1->4->3->5
当 k = 3 时，应当返回: 3->2->1->4->5

看到这题是困难，第一次做就直接跳过了。后来仔细一想，其实并不困难。

就只要两个指针加一个栈就好了。头指针指向k个之前那个，尾指针指向k个之后的那个，然后把中间的全塞进栈里。

栈塞满后，就把里面的东西一次加在链上。

特殊情况
可能栈塞不满（tflag）：直接退出即可
需要创立一个头节点。
要修改最后一个节点的next为null（防止成环）

~~~java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        Stack<ListNode> s = new Stack<>();
        ListNode first,last = head;
        first = new ListNode(0);
        first.next = head;
        head = first;
        int flag = 0,tflag = 0;
        while(last!=null){
            flag = 0;
            for(int i = 0;i<k;i++){
                if(last==null){
                    flag = 1;
                    break;
                }
                s.push(last);
                last = last.next;
            }
            if(flag == 1&& s.size()!=k){
                tflag = 1;
                break;
            }
            while(s.empty()==false){
                first.next = s.pop();
                first = first.next;
            }
            first.next=last;
        }
        if(tflag == 0)
            first.next = null;
        return head.next;
    }
}
~~~

这个困难的题其实通过率也有60%

竟然一发就搞定了。挺奇怪的是，按理用栈效率要递归高，竟然才击败6%

### Q138复制带随机指针的链表
给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。
要求返回这个链表的 深拷贝。 
我们用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：
val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。
难度 中等

示例 1：
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
示例 2：
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
示例 3：
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
示例 4：
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。

提示：
-10000 <= Node.val <= 10000
Node.random 为空（null）或指向链表中的节点。
节点数目不超过 1000 。

通过率只有56%
刚看这题感觉特别麻烦，但是也想到了用hashmap来做。不过突然犹豫了下，map存的到底是新的节点，还是旧的地址。

过了一天再来做的。map存的应该是地址。所以就很简单了，只要每创立一个节点，就在hashmap存下原节点和新链的节点，按照next的顺序遍历（顺带把random建立出来），每次检查hashmap里有没有对应的，没有就创建放进map里
~~~java
class Solution {
    public Node copyRandomList(Node head) {
        HashMap<Node,Node> map = new HashMap<Node,Node>();
        Node first=null,t1=null,t2=null,ran=null,last=null,h1 = head;
        int count = 0;
        while(h1 != null){
            if(map.containsKey(h1)==false){
                t2 = new Node(h1.val);
                map.put(h1,t2);
                if(count == 0){
                    count++;
                    first = t2;
                }
            }
            else{
                t2 = map.get(h1);    
            }
            if(t1!=null)
                t1.next = t2;
            if(h1.random!=null&&map.containsKey(h1.random)==false){
                ran = new Node(h1.random.val);
                map.put(h1.random,ran);
                t2.random = ran;
            }
            else if(h1.random!=null){
                t2.random = map.get(h1.random);    
            }
            h1 = h1.next;
            t1 = t2;
        }
       return first;
    }
}
~~~
击败100%

### Q3无重复字符的最长子串
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。
难度 中等

示例 1:
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

自己只能想到n2的暴力算法。看到官方的题解

利用窗口（两个指针 first last）来解。建立一个HashMap存下某个字符最后一次出现的位置。first指向第一个，last一直往后走。遇到重复的字符，若在窗口内，将first移到记录位置的下一个，将窗口大小改变。

~~~java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length()==0)
            return 0;
        HashMap<Character,Integer> map = new HashMap<>();
        int f=0,l=0,length = 1,maxLength = 1;
        map.put(s.charAt(f),f);
        for(int i=1;i<s.length();i++){
            int pos = map.getOrDefault(s.charAt(i),-1);
            map.put(s.charAt(i),i);
            if(pos<f){
                //map.put(s.charAt(i),i);
                l = i;
                length = l-f+1;
                maxLength=(length>maxLength)?length:maxLength;
            }
            else{
                f = pos+1;
                l = i;
                length = l-f+1;
                maxLength=(length>maxLength)?length:maxLength;
            }
        }
        return maxLength;
    }
}
~~~
击败69%

### Q11盛最多水的容器
给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
说明：你不能倾斜容器，且 n 的值至少为 2。
图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

难度 中等

示例：
输入：[1,8,6,2,5,4,8,3,7]
输出：49

去年就尝试过这个题，放弃了。
今天自己写了一遍，写的时候没用max min函数，写的特别复杂。用了四个指针来写，搞的特别复杂。

借鉴了下别人的。其实就是两指针，当哪边是短板时，就往中间移。（证明：长为x+1的底一定比长为x的底长，如果高没有变大，水一定比原来少。所以一定是移短板才有可能增高。

~~~java
class Solution {
    public int maxArea(int[] height) {
        int left=0,right=height.length-1,ans=Math.min(height[left],height[right])*(right-left);
        while(left<right){
            ans = Math.max(Math.min(height[left],height[right])*(right-left),ans);
            if(height[left]>height[right]) --right;
            else ++left;
        }
        return ans;
    }
}
~~~
击败67%

### Q15三数之和
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
注意：答案中不可以包含重复的三元组。

难度 中等

示例：
给定数组 nums = [-1, 0, 1, 2, -1, -4]，
满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]

这题一开始除了 排序+暴力 没啥特别的思路。看到题解里的标题都写着 排序+双指针，就大概懂了，这题没有特别好的解法。

先利用java自带的排序，然后for-each遍历（一个指针），期间另外两个指针往中间走。

但没写好，写成了三个for，不出意外挂了。仔细想了下。不应该这么写，应该是像上一题的短板问题 >0:则f3过大 <0：则f2过大。这样可以进一步优化。

还有个比较棘手的就是不能有重复的三元组。应该每次找到一组，就得让f2 f3往前走。（仔细想想应该让f2 f3一直都跳过重复的数字，可以提高效率）

~~~java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> l = new ArrayList<>();
        for(int f1 = 0;f1<nums.length-2;){
            int f2 = f1+1,f3 = nums.length-1;
            while(f2<f3){
                if(nums[f1]+nums[f2]+nums[f3]==0){
                    List<Integer> tempt = new ArrayList<>();
                    tempt.add(nums[f1]);
                    tempt.add(nums[f2]);
                    tempt.add(nums[f3]);
                    l.add(tempt);
                    int numf2 = nums[f2], numf3 = nums[f3];
                    while(f2<f3&&numf2 == nums[f2])
                        f2++;
                    while(f3>f2&&numf3==nums[f3])
                        f3--;
                }
                else{
                    while(f2<f3&&nums[f1]+nums[f2]+nums[f3]<0)
                        f2++;
                    while(f3>f2&&nums[f1]+nums[f2]+nums[f3]>0)
                        f3--;
                }
                
            }
            int pref1 = f1;
            while(f1<nums.length&&nums[f1]==nums[pref1])
                f1++;
        }
        return l;
    }
}
~~~
击败84%

### Q16最接近的三数之和
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

难度 中等

示例：
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。

这一题的思路跟上题几乎一样。所以我就直接cv了。魔改之后，却总是过不了。

区别还是有些的。target相等/逼近。相等很容易判断出来。而哪个接近总是要麻烦一点，每次移指针基本都要判断一次，经常还要用abs函数 
~~~java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int ans = nums[0] + nums[1] + nums[2];
        Arrays.sort(nums);
        for (int f1 = 0; f1 < nums.length - 2; f1++) {
            int f2 = f1 + 1, f3 = nums.length - 1;
            while (f2 < f3) {
                if (nums[f1] + nums[f2] + nums[f3] == target) {
                    return target;
                }
                if (Math.abs(nums[f1] + nums[f2] + nums[f3] - target) < Math.abs(ans - target))
                    ans = nums[f1] + nums[f2] + nums[f3];

                while (f2 < f3 && nums[f1] + nums[f2] + nums[f3] < target) {
                    f2++;

                    if (f2 < f3 && Math.abs(nums[f1] + nums[f2] + nums[f3] - target) < Math.abs(ans - target))
                        ans = nums[f1] + nums[f2] + nums[f3];
                }
                while (f3 > f2 && nums[f1] + nums[f2] + nums[f3] > target) {
                    f3--;
                    if (f2 < f3 && Math.abs(nums[f1] + nums[f2] + nums[f3] - target) < Math.abs(ans - target))
                        ans = nums[f1] + nums[f2] + nums[f3];
                }
            }

        }
        return ans;
    }
}
~~~
只击败了32%，可能是指针移动的部分，优化还不够。

仿照官方题解优化了一下，竟然只击败了19%，反而还降低了。

然后发现自己少了相等的情况。

~~~java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int ans = nums[0] + nums[1] + nums[2];
        Arrays.sort(nums);
        for (int f1 = 0; f1 < nums.length - 2; ) {
            int f2 = f1 + 1, f3 = nums.length - 1;
            while (f2 < f3) {
                int sum = nums[f1] + nums[f2] + nums[f3];
                if (Math.abs(sum - target) < Math.abs(ans - target))
                    ans = sum;
                if(sum<target)
                {
                    int f = f2;
                    while(f<f3&&nums[f]==nums[f2])
                        f++;
                    f2 = f;
                }
                else
                    {
                        int f = f3;
                        while(f>f2&&nums[f]==nums[f3])
                            f--;
                        f3 = f;
                    }
            }
            int f = f1;
            while(f<nums.length&&nums[f]==nums[f1])
                f++;
            f1 = f;
        }
        return ans;
    }
}
~~~

~~~java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int ans = nums[0] + nums[1] + nums[2];
        Arrays.sort(nums);
        for (int f1 = 0; f1 < nums.length - 2; ) {
            int f2 = f1 + 1, f3 = nums.length - 1;
            while (f2 < f3) {
                int sum = nums[f1] + nums[f2] + nums[f3];
                if(sum == target)
                    return sum;
                if (Math.abs(sum - target) < Math.abs(ans - target))
                    ans = sum;
                if(sum<target)
                {
                    int f = f2;
                    while(f<f3&&nums[f]==nums[f2])
                        f++;
                    f2 = f;
                }
                else
                    {
                        int f = f3;
                        while(f>f2&&nums[f]==nums[f3])
                            f--;
                        f3 = f;
                    }
            }
            int f = f1;
            while(f<nums.length&&nums[f]==nums[f1])
                f++;
            f1 = f;
        }
        return ans;
    }
}
~~~
加了一行代码，击败96%。没想到特殊情况竟然对整体影响这么大（Amadha定律

### Q26删除排序数组中的重复项
给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

难度 简单

示例 1:
给定数组 nums = [1,1,2], 
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
你不需要考虑数组中超出新长度后面的元素。

示例 2:
给定 nums = [0,0,1,1,1,2,2,3,3,4],
函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
你不需要考虑数组中超出新长度后面的元素。

看到是简单，还以为很简单。想了一会也没有思路。看了评论里的解答，才反应过来，就是双指针问题。

j一直向后走，而i记录有效的位置。
自己写了下，由于不是很简洁，就借鉴了
~~~java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums==null)
            return 0;
        if(nums.length==1)
            return 1;
        int i=0,j=1;
        while(j<nums.length)
            if(nums[i] == nums[j]){
                j++;
            }else{
                i++;
                nums[i] = nums[j];
                j++;
            }
            return i+1;
    }
}
~~~
击败98%

### Q42接雨水
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 感谢 Marcos 贡献此图。

难度 困难

示例:
输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6

去年就尝试写这个题，放弃了。这题没啥思路去写，暴力也不是很好写。

看了下题解，里面提到了找到最高值。这个就是关键点。只要你找到了最高值，这一题就迎刃而解了。从两边向最高值靠拢，每次找到的最高值，就是装水的板子。（因为有中间最高的板兜底）。所以每次向中间靠拢，如果有比最高值大就更新，小就增加水。

~~~java
class Solution {
    public int trap(int[] height) {
        int max = 0,water = 0;
        for(int i=0;i<height.length;i++)
            if(height[max]<height[i])
                max = i;
        int lmax = 0,rmax = 0;
        for(int i = 0;i<max;i++){
            if(lmax>height[i])
                water+=(lmax-height[i]);
            else{
                lmax = height[i];
            }
        }
        for(int i = height.length-1;i>max;i--){
            if(rmax>height[i])
                water+=(rmax-height[i]);
            else{
                rmax = height[i];
            }
        }
        return water;
    }
}
~~~
击败99%

### Q121买卖股票的最佳时机
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
注意：你不能在买入股票前卖出股票。

难度 简单

示例 1:
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2:
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

是一个究极简单的题目。刚开始把自己绕晕了，竟然交了五发还没过。

其实就是遍历一次，然后记住自己能买到的最低价格，然后拿当前价格减去最低价格就可以获得利润，拿去与最大利润相比较。
~~~java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length==0)    return 0;
        int ans = 0,lmin = prices[0];
        for(int i = 1;i<prices.length;i++){
            if(ans<prices[i]-lmin)
                ans = prices[i]-lmin;
            if(lmin>prices[i])
                lmin = prices[i];
        }
        return ans;
    }
}
~~~
击败98%

### Q209长度最小的子数组
给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

难度中等

示例：
输入：s = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。

进阶：
如果你已经完成了 O(n) 时间复杂度的解法, 请尝试 O(n log n) 时间复杂度的解法。

我感觉这个进阶比较迷。

题目不是很难，一个滑窗问题，两个针就可以解决。后面的针每次往后移一下，就检测前面针可以移几次。

但是被特例搞了两次，数组大小为0时，数组的和都没有目标大时

用数组时，一定要注意有没有越界。java在这方面还比较好，有空指针报错

~~~java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        if(nums.length==0) return 0;
        if(nums.length==1)  return 1;
        int f=0,l =0,sum=nums[0],minLength=1;
        while(sum<s){
            l++;
            minLength++;
            if(l==nums.length) return 0;
            sum+=nums[l];
        }
        while(l<nums.length){
            while(sum-nums[f]>=s){
                sum-=nums[f];
                f++;
            }
            if(l-f+1<minLength)
                    minLength = l-f+1;
            l++;
            if(l<nums.length)
                sum+=nums[l];
        }
        return minLength;
    }
}
~~~

### Q141环形链表
给定一个链表，判断链表中是否有环。
为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

难度 简单

示例 1：
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。


示例 3：
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。

进阶：
你能用 O(1)（即，常量）内存解决此问题吗？

一看到这题就想到了HashMap，这题跟之前一个原地复制 随机指针的题有很多共同点。
~~~java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null) return false;
        HashMap<ListNode,Integer> m = new HashMap<>();
        while(head.next!=null){
            m.put(head,1);
            if(m.containsKey(head.next)==true)
                return true;
            head =  head.next;
        }
        return false;
    }
}
~~~
没想到效率特别低，只击败16%

看了下题解，有个龟兔赛跑的视频，还挺有意思的。兔子一次走两个，乌龟一次走一个。理论上他们永远不会相遇，但是如果赛道变了，有个环后，他们可能就会相遇。
~~~java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null) return false;
        ListNode fast=head,low=head;
        while(fast!=null&&fast.next!=null){
            fast = fast.next.next;
            low = low.next;
            if(fast==low)
                return true;
        }
        return false;
    }
}
~~~
击败100%

### Q202快乐数
编写一个算法来判断一个数 n 是不是快乐数。
「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。
如果 n 是快乐数就返回 True ；不是，则返回 False 。

难度 简单 

示例：
输入：19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

这个题虽然是简单，但也没想出来啥方法。一开始就放弃了暴力法，觉得复杂度太高了。然后就一直想不出来。

其实沿着暴力法走下去，可能就能想出来了。 要判断true，很简单，只要为1就可以了。但是false有点麻烦，因为false一直不等于1。那怎么判断呢？ 无限循环！！！ 如果有环，其实就跟上一题一回事。只要用快慢指针就可以解决了
~~~java
class Solution {
    public boolean isHappy(int n) {
        int slow = fun(n);
        int fast = fun(n);
        fast = fun(fast);
        while(slow != fast){
            slow = fun(slow);
            fast = fun(fast);
            fast = fun(fast);
            if(fast == 1)
                return true;
        }
        if(fast == 1)
            return true;
        return false;
    }
    
    public int fun(int x){
        int sum = 0;
        while(x!=0){
            sum+=((x%10)*(x%10));
            x/=10;
        }
        return sum;
    }
}
~~~
击败100%

### Q876链表的中间结点
给定一个带有头结点 head 的非空单链表，返回链表的中间结点。
如果有两个中间结点，则返回第二个中间结点。

难度简单

示例 1：
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
示例 2：
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。

提示：
给定链表的结点数介于 1 和 100 之间。

做完前面两题，这题就是乱杀。就是快慢指针
~~~java
class Solution {
    public ListNode middleNode(ListNode head) {
        if(head==null) return null;
        if(head.next==null) return head;
        ListNode slow = head.next;
        ListNode fast = head.next.next;
        while(fast!=null){
            fast = fast.next;
            if(fast!=null){
                fast = fast.next;
                slow = slow.next;
            }
        }
        return slow;
    }
}
~~~
击杀100%

### Q56合并区间
给出一个区间的集合，请合并所有重叠的区间。

难度 中等 

示例 1:
输入: intervals = [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:
输入: intervals = [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
注意：输入类型已于2019年4月15日更改。 请重置默认代码定义以获取新方法签名。
提示：
intervals[i][0] <= intervals[i][8]

其实不是很复杂。先按intervals[x][0]排序。窗口，记住左边，跟右边。如果下一个，左边界在我们范围内，而右边界比我们大，就更新。左边界不在我们范围内，就把窗口后移。

只是我不太会写数组排序算法、不太懂lambada函数怎么写。还有最后需要拷贝数组的函数也没用过。然后中间把变量名搞混了。导致找了半天bug（大概1h），第一次自己在本机里写main函数，一点点调试（等俺有钱了就冲会员 （可能有钱就不需要刷这个了。

~~~java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length==0) return intervals;
        Arrays.sort(intervals,(v1, v2) -> v1[0] - v2[0]);
        int left=intervals[0][0],right =intervals[0][9];
        int[][] ans = new int[intervals.length][10];
        int count = 0;
        int flag = 0;
        for(int i = 0; i<intervals.length; i++){
            flag = 0;
            if(intervals[i][0]<=right&&intervals[i][11]>right){
                right = intervals[i][12];
            }else if(intervals[i][0]>right){
                ans[count][0] = left;
                ans[count][13] = right;
                left = intervals[i][0];
                right = intervals[i][14];
                count++;
                flag = 1;
            }
            flag = 0;
        }if(flag == 0){
            ans[count][0] = left;
            ans[count][15] = right;
            count++;
        }
        return Arrays.copyOf(ans,count);
    }
}
~~~
击败92%

### Q6Z字形变换
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
请你实现这个将字符串进行指定行数变换的函数：
string convert(string s, int numRows);

难度中等

示例 1:
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
示例 2:
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
L     D     R
E   O E   I I
E C   I H   N
T     S     G

题目讲的花里胡哨的，我还以为要输出解释这样的。就是一会正、一会反就好了。我们就创个数组，一会正序加到各个数组，一会反向。

~~~java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows == 1||s.length()==1) return s;
        String[] ans = new String[numRows];
        for(int i = 0; i < numRows; i++){
            ans[i] = "";
        }
        int pos = 0, change = 0;
        for (int i = 0; i < s.length(); i++) {
            ans[pos]+=s.charAt(i);
            if(change==1){
                if(pos==0){
                    change = 0;
                    pos++;
                }else{
                    pos--;
                }
            }else{
                if(pos==numRows-1){
                    change=1;
                    pos--;
                }else{
                    pos++;
                }
            }
        }
        String re = new String();
        for (String s1 : ans) {
            re += s1;
        }
        //System.out.println(ans[0]);
        return re;
    }
}
~~~
但没想到的是我这个n的算法 只击败了17%

改用StringBuilder
~~~java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1 || s.length() == 1)
            return s;
        StringBuilder[] ans = new StringBuilder[numRows];
        for(int i=0;i<ans.length;i++){
            ans[i]=new StringBuilder();
        }
        int pos = 0, change = 0;
        for (int i = 0; i < s.length(); i++) {
            ans[pos].append(s.charAt(i));
            if (change == 1) {
                if (pos == 0) {
                    change = 0;
                    pos++;
                } else {
                    pos--;
                }
            } else {
                if (pos == numRows - 1) {
                    change = 1;
                    pos--;
                } else {
                    pos++;
                }
            }
        }
        String re = new String();
        for (StringBuilder s1 : ans) {
            re += s1;
        }
        // System.out.println(ans[0]);
        return re;
    }
}
~~~
击败44%
看了下最快解，他用的是char数组，其他区别应该不大。

### Q14最长公共前缀
编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串 ""。

难度 简单

示例 1:
输入: ["flower","flow","flight"]
输出: "fl"
示例 2:
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:
所有输入只包含小写字母 a-z 。

简单的遍历。
~~~java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length==0) return "";
        int x = -1;
        char a;
        int flag = 0;
        for(int i = 0;i<strs[0].length();i++){
            a= strs[0].charAt(i);
            for(int j = 0;j<strs.length;j++){
                if(strs[j].length()==i)
                    {flag = 1;break;}
                if(strs[j].charAt(i)!=a){
                    flag = 1;
                    break;
                }
            }
            if(flag == 1)
                break;
            x = i; 
        }
        if(x == -1) return "";
        return strs[0].substring(0,x+1);
    }
}
~~~
击败87%

### Q763划分字母区间
字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个字符串片段的长度的列表。

难度中等

示例 1：
输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。

看了官方题解，贪心算法。每次找到最后一个相同字母的地址。窗口一定得包含最后一个地址。

~~~java
class Solution {
    public List<Integer> partitionLabels(String S) {
        int left=0,right=0;
        List<Integer> l = new ArrayList<>();
        for(int i = 0;i<S.length();i++){
            for(int j =S.length()-1;j>=left;j--){
                if(S.charAt(i)==S.charAt(j)){
                    if(i>right){
                    l.add(right-left+1);
                    right = left = i;
                }if(j>right){    
                        right = j;
                        break;
                    }
                }
            }
        }
        l.add(right-left+1); 
        return l;
    }
}
~~~
击杀5%？？？？

仿照官方题解改了一下。这一题跟买卖股票那题还是有一丝相似。创建数组记录字母的最后一个。然后遍历数组，一直更新窗口大小（记最大的），当遍历到窗口最后一个，就完成了一个子字符串。

~~~java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> l = new ArrayList<>();
        int[] pos = new int[26];
        for(int i = S.length()-1;i>=0;i--)
            pos[S.charAt(i)-'a']=(pos[S.charAt(i)-'a']>i)?pos[S.charAt(i)-'a']:i;
        int j = 0,last = pos[S.charAt(j)-'a'];
        int preJ = 0;
        while(j!=S.length()){
            if(last<pos[S.charAt(j)-'a'])
                last = pos[S.charAt(j)-'a'];
            if(j==last){
                l.add(j-preJ+1);
                preJ = j+1;
            }
            j++;
        }
        return l;
    }
}
~~~
击败78%

### Q7整数反转
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

难度 简单

示例 1:
输入: 123
输出: 321
示例 2:
输入: -123
输出: -321
示例 3:
输入: 120
输出: 21
注意:
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

看到这题就在想：就这？ （没看到注意

一下子就写出来了。结果来了个1534236469，会暴int。还有点懵。看了下评论，用long算最后在返回int
~~~java
class Solution {
    public int reverse(int x) {
        long ans = 0;
        while(x!=0){
            ans*=10;
            ans+=x%10;
            x/=10;
        }
        return (int)ans==ans?(int)ans:0;
    }
}
~~~
击败100%

### Q8字符串转换整数 (atoi)
请你来实现一个 atoi 函数，使其能将字符串转换成整数。
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：
如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0 。

提示：
本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

难度 中等

示例 1:
输入: "42"
输出: 42
示例 2:
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
示例 3:
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
示例 4:
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
示例 5:
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231)。

知道这题肯定会爆int，用了long。没想到连long也会爆，看他的数据，感觉搞不好long long都会暴。
只能每次计算判断有没有爆了
~~~java
class Solution {
    public int myAtoi(String str) {
        if(str.length()==0) return 0; 
        long ans = 0,preAns = 0;
        int pos = 0;
        int minus = 0;
        while(pos<str.length()&&str.charAt(pos)==' '){
            pos++;
        }
            ans = -ans;
        if(pos==str.length())
            return 0;
        if(str.charAt(pos)=='-'){
            minus = 1;
            pos++;
        }
        else if(str.charAt(pos)=='+')
            pos++;
        else if(str.charAt(pos)>='0'&&str.charAt(pos)<='9')
            ;
        else 
            return 0;
        while(pos!=str.length()&&str.charAt(pos)>='0'&&str.charAt(pos)<='9'){
            preAns = ans;
            ans*=10;
            ans+=(str.charAt(pos)-'0');
            if(ans<preAns){
                if(minus==1)
                    return Integer.MIN_VALUE;
                return Integer.MAX_VALUE;
            }
            pos++;
        }
        if(minus==1)
        if((int)ans==ans)
            return (int)ans;    
        if(minus==1)
            return Integer.MIN_VALUE;
        return Integer.MAX_VALUE;
    }
}
~~~
击败99%

### Q9回文数
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

难度 简单

示例 1:
输入: 121
输出: true
示例 2:
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3:
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
进阶:
你能不将整数转为字符串来解决这个问题吗？

这个题还是比较简单的。直接转换成string做
~~~java
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0) return false;
        String s = Integer.toString(x);
        int i = 0,j = s.length()-1;
        while(i<j){
            if(s.charAt(i)!=s.charAt(j))
                return false;
            i++;
            j--;
        }
        return true;
    }
}
~~~
只击败了27%

### Q415字符串相加
给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。

难度 简单

提示：
num1 和num2 的长度都小于 5100
num1 和num2 都只包含数字 0-9
num1 和num2 都不包含任何前导零
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式

本来先做的是43字符串相乘。de了半天bug，做不下去，看了官方题解。原来还要一题是415，刚好要用到我的一个子函数。就cv过来调了一下。
~~~java
class Solution {
    public String addStrings(String num1, String num2) {
        int i = num1.length() - 1, j = num2.length() - 1;
        StringBuilder ans = new StringBuilder();
        int ci = 0;
        while (i >= 0 && j >= 0) {
            if (num1.charAt(i)-'0' + num2.charAt(j)-'0' + ci <= 9) {
                ans.append(num1.charAt(i)-'0' + num2.charAt(j)-'0' + ci);
                ci = 0;
            } else {
                ans.append(num1.charAt(i)-'0' + num2.charAt(j)-'0' + ci - 10 );
                ci = 1;
            }
            i--;
            j--;
        }//System.out.println(ans.toString());
        
        while (i >= 0) {
            if (num1.charAt(i) + ci <= '9') {
                ans.append(num1.charAt(i)-'0' + ci);
                ci = 0;
            } else {
                ans.append(num1.charAt(i)-'0' + ci - 10);
                ci = 1;
            }
            i--;
        }
        while (j >= 0) {
            if (num2.charAt(j)-'0' + ci <= 9) {
                ans.append(num2.charAt(j)-'0' + ci);
                ci = 0;
            } else {
                ans.append(num2.charAt(j)-'0' + ci - 10);
                ci = 1;
            }
            j--;
        }
        if (ci == 1) {
            ans.append('1');
        }
        //System.out.println(ans.toString());
        return ans.reverse().toString();
    }
}
~~~
思路很简单，就是一位位相加看有没有超过10。击败73% 。但是也暴露了，我对char int运算不熟。期间写出了类似'0'+'1'<'2',这种代码。

还有StringBuilder.append()函数，我之前写StringBuilder.append('0'+6),我以为会加'6'，其实char与int运算，会自动转换成int，而append由于多态，是可以加int类型，他会把int再次转换成char，这却与我的期望不符合。（多态瞬间不香了。
~~~java
public class stringbuild {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        sb.append('1'+6);
        System.out.println(sb.toString());
    }
}

PS D:\daima> cd "d:\daima\" ; if ($?) { javac stringbuild.java } ; if ($?) { java stringbuild }
55
~~~

### Q43字符串相乘
给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。

难度 中等

示例 1:
输入: num1 = "2", num2 = "3"
输出: "6"
示例 2:
输入: num1 = "123", num2 = "456"
输出: "56088"
说明：
num1 和 num2 的长度小于110。
num1 和 num2 只包含数字 0-9。
num1 和 num2 均不以零开头，除非是数字 0 本身。
不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。

这题应该是写到目前 我觉得特别烦的一题。

题目学过计组基本就知道思路。num1×num2，先让ans = 0；每次取出num2最右边那个数t，ans+=(num1×t)。ans向左移一位。一直重复，知道num2 = 0；

把这分成两个函数，一个函数负责加法，一个函数负责单乘。

这之中出现了很多细节的bug。比如：写单乘受加法影响，进位只考虑为1的情况。char计算多次搞错。string 在函数应该是值传递。

写bug 1min ，找bug 1h。

~~~java
class Solution {
    public String multiply(String num1, String num2) {
        if(num1.equals("0")||num2.equals("0"))  
            return "0";
        StringBuilder ans = new StringBuilder("0");
        while (num2.equals("") != true) {
            ans = add(ans, mult(num1, num2));
            num2 = num2.substring(1, num2.length());
            if (num2.equals("") != true)
                ans.append('0');
        }
        return ans.toString();
    }

    public StringBuilder add(StringBuilder num1, StringBuilder num2) {
        int i = num1.length() - 1, j = num2.length() - 1;
        StringBuilder ans = new StringBuilder();
        int ci = 0;
        while (i >= 0 && j >= 0) {
            if (num1.charAt(i) - '0' + num2.charAt(j) - '0' + ci <= 9) {
                ans.append(num1.charAt(i) - '0' + num2.charAt(j) - '0' + ci);
                ci = 0;
            } else {
                ans.append(num1.charAt(i) - '0' + num2.charAt(j) - '0' + ci - 10);
                ci = 1;
            }
            i--;
            j--;
        } // System.out.println(ans.toString());

        while (i >= 0) {
            if (num1.charAt(i) + ci <= '9') {
                ans.append(num1.charAt(i) - '0' + ci);
                ci = 0;
            } else {
                ans.append(num1.charAt(i) - '0' + ci - 10);
                ci = 1;
            }
            i--;
        }
        while (j >= 0) {
            if (num2.charAt(j) - '0' + ci <= 9) {
                ans.append(num2.charAt(j) - '0' + ci);
                ci = 0;
            } else {
                ans.append(num2.charAt(j) - '0' + ci - 10);
                ci = 1;
            }
            j--;
        }
        if (ci == 1) {
            ans.append('1');
        }
        // System.out.println(ans.toString());
        return ans.reverse();
    }

    public StringBuilder mult(String num1, String num2) {
        int y = num2.charAt(0) - '0', x = num1.length() - 1;
        StringBuilder s1 = new StringBuilder("");
        if (y == 0) {
            return new StringBuilder("0");
        }
        int ci = 0;
        while (x >= 0) {
            int ch = num1.charAt(x) - '0';
            ch *= y;
            if (ch + ci >= 10) {
                s1.append((char) ((ch + ci)%10 + '0'));
                ci = (ch + ci) / 10;
            } else {
                s1.append((char) (ch + ci + '0'));
                ci = 0;
            }
            x--;
        }
        if (ci != 0) {
            s1.append(ci);
        }
        return s1.reverse();
    }

}
~~~
只击败了33%
### Q172阶乘后的零
给定一个整数 n，返回 n! 结果尾数中零的数量。

难度 简单

示例 1:
输入: 3
输出: 0
解释: 3! = 6, 尾数中没有零。
示例 2:
输入: 5
输出: 1
解释: 5! = 120, 尾数中有 1 个零.
说明: 你算法的时间复杂度应为 O(log n) 。

这题一点都不简单把...我参考了题解。求多少个零就是求因子有多少个10，也就是求又少个5，有多少个2。我们很容易可以看出来，每出现一个5，都会有好几个2。所以其实就是求有多少个5因子。

1 2 3 4 **5** 6 7 8 9 **10** 11 12 13 14 **15** 16 17 18 19 **20** 21 22 23 24 **25**

5：1个，10：2个，15：3个，20：4个，25：6个。25/5+25/5/5，看到这，我就直接想出了个简单代码，也没验证是否正确，就直接提交了
~~~java
class Solution {
    public int trailingZeroes(int n) {
        int i = 1,count=0;
        while(n/Math.pow(5,i)!=0){
            count += n/Math.pow(5,i);
            ++i;
        }
        return count;
    }
}
~~~
击败99% 

### Q258各位相加
给定一个非负整数 num，反复将各个位上的数字相加，直到结果为一位数。

难度 简单

示例:
输入: 38
输出: 2 
解释: 各位相加的过程为：3 + 8 = 11, 1 + 1 = 2。 由于 2 是一位数，所以返回 2。
进阶:
你可以不使用循环或者递归，且在 O(1) 时间复杂度内解决这个问题吗？

又是一个明明是难题，却写着简单...

这题还挺有意思的。其实他求的东西叫做 树根 ，知乎上有个[帖子](https://www.zhihu.com/question/30972581/answer/50203344)对此进行了讨论。

通过找规律可以看出
原数: 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
数根: 1 2 3 4 5 6 7 8 9  1  2  3  4  5  6  7  8  9  1  2  3  4  5  6  7  8  9  1  2  3 

转自力扣[题解](https://leetcode-cn.com/problems/add-digits/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-5-7)
~~~java
class Solution {
    public int addDigits(int num) {
        return (num-1)%9+1;
    }
}
~~~
击败100%

### Q54螺旋矩阵
给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

难度 中等

示例 1:
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
示例 2:
输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]

这题之前遇到过。大一C语言的时候，上机题有这题，虽然不是我的场次。当时的题目是让你 print一个1-n的螺旋正方形矩阵。跟这题几乎一样

只要四个方向转来转去，就可以写出这题，然后用一个矩阵记录哪些已经输出过了。

但是有点迷，第一个样例，总是会多输出一个4，具体原因也懒得查了。只要加入数组前多一次判断就行了。
~~~java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if(matrix.length==0)    return new ArrayList<>();
        int[][] m = new int[matrix.length][matrix[0].length];
        List<Integer> l = new ArrayList<>();
        int i = 0,j = 0;
        int flag=0;
        int t = 0;
        while(true){
            if(m[i][j]!=1)
                l.add(matrix[i][j]);
            m[i][j] = 1;
            t=0;
            if(flag==0){
                if(j+1<m[0].length&&m[i][j+1]!=1){
                    j++;
                }else{
                    flag = 1;
                    t++;
                }
            }if(flag==1){
                if(i+1<m.length&&m[i+1][j]!=1){
                    i++;
                }else{
                    flag=2;
                    t++;
                }
            }if(flag==2){
                if(j-1>=0&&m[i][j-1]!=1){
                    j--;
                }else{
                    flag = 3;
                    t++;
                }
            }if(flag==3){
                if(i-1>=0&&m[i-1][j]!=1){
                    i--;
                }else{
                    flag = 0;
                    t++;
                }
            }if(t==4){
                break;
            }
        }
        return l;
    }
}
~~~
击败100%

### Q73矩阵置零
给定一个 m x n 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用原地算法。

难度 中等

示例 1:
输入: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
输出: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
示例 2:
输入: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
输出: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
进阶:
一个直接的解决方案是使用  O(mn) 的额外空间，但这并不是一个好的解决方案。
一个简单的改进方案是使用 O(m + n) 的额外空间，但这仍然不是最好的解决方案。
你能想出一个常数空间的解决方案吗？

看到这题，我就在想 这题也配中等？ 进阶说O（mn） O（m+n）的空间复杂度。我立马就能写个O（1）的。

刚开始就写了个二十行左右，原地修改的代码。 先遍历数组，遇见0把行首 列首改成0。(我此时还在想如果在这时把整个数组改了岂不是更简单) 然后再一次遍历数组，把应该清零的清零

力扣立马用样例教育了我。0,1  1,1本来还剩一个1，但是如果你修改了行首、列首，就出问题了。（前几天在橙书算法1.5课后题也遇到一个找bug题，也是因为原地修改导致循环后续出问题

好吧，那我们第二次遍历，不修改行首行列。这样也还是过不了。毕竟存在一种情况，需要把行首/列首搞成0

那我们就最开始检查行首/列首。最后再处理行首/列首。
~~~java
class Solution {
    public void setZeroes(int[][] matrix) {
        int r= 0,c=0;
        for(int i=0;i<matrix[0].length;i++){
            if(matrix[0][i]==0){
                r = 1;
                break;
            }
        }for(int i=0;i<matrix.length;i++){
            if(matrix[i][0]==0){
                c = 1;
                break;
            }
        }for(int i = 0;i<matrix.length;i++)
            for(int j = 0;j<matrix[0].length;j++)
                if(matrix[i][j]==0){
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
        for(int i = 1;i<matrix.length;i++){
            if(matrix[i][0]==0){
                for(int j = 1;j<matrix[0].length;j++){
                    matrix[i][j] = 0;
                }
            }
        }for(int i = 1;i<matrix[0].length;i++){
            if(matrix[0][i]==0){
                for(int j = 1;j<matrix.length;j++){
                    matrix[j][i] = 0;
                }
            }
        }if(r==1){
            for(int i =0;i<matrix[0].length;i++){
                matrix[0][i] = 0;
            }
        }if(c==1){
            for(int i=0;i<matrix.length;i++)
                matrix[i][0] = 0;
        }
        return ;        
    }
}
~~~
时间击败100%，空间击败76%

### Q78子集
给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。

难度 中等

示例:
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

这题我之前做过类似的，南科大的一次acm校赛。这题应该是a题，最简单的。当时用C语言搞了半天，没搞出来。我记得我是用递归写的，特别复杂。

今天直接看题解了，先创建一个空集合[],然后遍历nums数组，先是1，然后把1放进空集合里。就获得两个集合[] [3]，然后取出2,把2 再放进[] [4]中。获得[] [5] [2] [1,2]。就这样遍历下去，就可以得到所有集合。

注意遍历集合的集合时，要把初始size记录下来，否则size一直增长，进入死循环。

~~~java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> l = new ArrayList<>();
        l.add(new ArrayList<>());
        for(int i:nums){
            int size = l.size();
            for(int j=0;j<size;j++){
                List<Integer> tempt =new ArrayList<>(l.get(j));
                tempt.add(i);
                l.add(tempt);
            }
        }return l;
    }
}
~~~
击败99%

还有一种是利用二进制位运算，00000 则表示都没有 00001 则表示有一个有，诸如此类，去穷举。

其他的 回溯法什么的没怎么看懂

### Q581最短无序连续子数组
给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。
你找到的子数组应是最短的，请输出它的长度。

难度 简单

示例 1:
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
说明 :
输入的数组长度范围在 [1, 10,000]。
输入的数组可能包含重复元素 ，所以升序的意思是<=。

又是一个假简单题，感觉通过率有时比难度更可信，这题通过率就不是很高。做了半天，没有想出O（n）的算法。就直接用了排序。
~~~java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] numscopy = new int[nums.length];
        numscopy = Arrays.copyOf(nums,nums.length);
        Arrays.sort(nums);
        int f = -1, l=-1;
        for(int i = 0;i<nums.length;i++){
            if((f==-1||f>i)&&numscopy[i]!=nums[i])
                f = i;
            if(l<i&&numscopy[i]!=nums[i])
                l = i;
        }if(l==-1||f==-1)   return 0;
        return l-f+1;
    }

}
~~~
竟然也击败了57%

### Q945使数组唯一的最小增量
给定整数数组 A，每次 move 操作将会选择任意 A[i]，并将其递增 1。
返回使 A 中的每个值都是唯一的最少操作次数。

难度 中等

示例 1:
输入：[1,2,2]
输出：1
解释：经过一次 move 操作，数组将变为 [1, 2, 3]。
示例 2:
输入：[3,2,1,2,1,7]
输出：6
解释：经过 6 次 move 操作，数组将变为 [3, 4, 1, 2, 5, 7]。
可以看出 5 次或 5 次以下的 move 操作是不能让数组的每个值唯一的。
提示：
0 <= A.length <= 40000
0 <= A[i] < 40000

我一想，这不就典型的map题吗

~~~java
class Solution {
    public int minIncrementForUnique(int[] A) {
        int ans = 0;
        HashMap<Integer,Integer> m = new HashMap<>();
        for(int i:A){
            m.put(i,m.getOrDefault(i,0)+1);
        }
        for(int i = 0;i<A.length;i++){
            if(m.get(A[i])!=1){
                int j = A[i]+1;
                while(m.getOrDefault(j,0)!=0)
                    j++;
                ans += (j-A[i]);
                m.put(j,1); 
                m.replace(A[i],m.get(A[i])-1);
            }
        }return ans;
    }
}
~~~
结果56/59 超时挂掉了。感觉是每次j++太耗时间了。[1 ,1,2,3,4,5,6,....100]j一次加1，非常耗时间。

想了半天优化j++。太复杂了。看了下题解。他们先排序，然后从[1]开始检索，只要小于等于前一个，就把这个设置成前一个+1，（不管后面的，实际上你这时管不管后面的，结果都一样）。

~~~java
class Solution {
    public int minIncrementForUnique(int[] A) {
        int ans = 0;
        Arrays.sort(A);
        for(int i = 1;i<A.length;i++){
            if(A[i]<=A[i-1]){
                int pre = A[i];
                A[i] = A[i-1]+1;
                ans += (A[i]-pre);
            }
        }
        return ans;
    }
}
~~~
击败70%

### Q20有效的括号
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
有效字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

难度 简单

示例 1:
输入: "()"
输出: true
示例 2:
输入: "()[]{}"
输出: true
示例 3:
输入: "(]"
输出: false
示例 4:
输入: "([)]"
输出: false
示例 5:
输入: "{[]}"
输出: true


学过栈应该都知道怎么做了
~~~java
class Solution {
    public boolean isValid(String s) {
        if(s.length()==0) return true;
        Stack<Integer> s1 = new Stack<>();
        for(int i = 0;i<s.length();i++){
            char ch = s.charAt(i);
            if(ch=='('){
                s1.push(1);
            }else if(ch=='['){
                s1.push(2);
            }else if(ch=='{'){
                s1.push(3);
            }else if(ch==')'){
                if(s1.empty()==true)
                    return false;
                if(s1.pop()!=1)
                    return false;
            }else if(ch==']'){
                if(s1.empty()==true)
                    return false;
                if(s1.pop()!=2)
                    return false;
            }else if(ch=='}'){
                if(s1.empty()==true)
                    return false;
                if(s1.pop()!=3)
                    return false;
            }
        }if(s1.empty()!=true)
            return false;
        return true;
    }
}
~~~
击败78%

### Q155最小栈
设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。
push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。

难度 简单 

示例:
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]
输出：
[null,null,null,null,-3,null,0,-2]
解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.

提示：
pop、top 和 getMin 操作总是在 非空栈 上调用。

看到这个题还以为要自己实现一个大小任意的栈，我想这也太复杂了。看了官方题解，他也是用的轮子。那我也用轮子吧。

~~~java
class MinStack {
    Stack<Integer> s;
    Stack<Integer> min;
    /** initialize your data structure here. */
    public MinStack() {
        s = new Stack<>();
        min = new Stack<>();
        min.push(Integer.MAX_VALUE);
    }
    
    public void push(int x) {
        int m = min.peek();
        if(m>x){
            s.push(x);
            min.push(x);
        }else{
            s.push(x);
            min.push(m);
        }
    }
    
    public void pop() {
        min.pop();
        s.pop();
    }
    
    public int top() {
        return s.peek();
    }
    
    public int getMin() {
        return min.peek();
    }
}
~~~
击败97%

### Q224基本计算器
实现一个基本的计算器来计算一个简单的字符串表达式的值。
字符串表达式可以包含左括号 ( ，右括号 )，加号 + ，减号 -，非负整数和空格  。

难度 困难

示例 1:
输入: "1 + 1"
输出: 2
示例 2:
输入: " 2-1 + 2 "
输出: 3
示例 3:
输入: "(1+(4+5+2)-3)+(6+8)"
输出: 23
说明：
你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。

没想到这个也能算困难，学过栈的应该都写过，而且还能带乘除的。
~~~java
class Solution {
    public int calculate(String s) {
        Stack<Integer> s1 = new Stack<>();
        Stack<Integer> s2 = new Stack<>();
        int t = 0, flag = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                t = t * 10 + s.charAt(i) - '0';
                flag = 1;
            } else if (s.charAt(i) == '(') {
                if (flag == 1)
                    s1.push(t);
                flag = 0;
                t=0;
                s2.push(0);
            } else if (s.charAt(i) == ')') {
                if (flag == 1)
                    s1.push(t);
                flag = 0;
                t=0;
                int c = s2.pop();
                while (c != 0) {
                    if (c == 1) {
                        int x = s1.pop();
                        int y = s1.pop();
                        s1.push(x + y);
                    } else if (c == 2) {
                        int x = s1.pop();
                        int y = s1.pop();
                        s1.push(y - x);
                    }
                    c = s2.pop();
                }
            } else if (s.charAt(i) == '+') {
                if (flag == 1)
                    s1.push(t);
                flag = 0;
                t=0;
                if (s2.empty() != true && s2.peek() == 1) {
                    int x = s1.pop();
                    int y = s1.pop();
                    s1.push(x + y);
                    s2.pop();
                } else if (s2.empty() != true && s2.peek() == 2) {
                    int x = s1.pop();
                    int y = s1.pop();
                    s1.push(y - x);
                    s2.pop();
                }
                s2.push(1);
            } else if (s.charAt(i) == '-') {
                if (flag == 1)
                    s1.push(t);
                flag = 0;
                t=0;
                if (s2.empty() != true && s2.peek() == 1) {
                    int x = s1.pop();
                    int y = s1.pop();
                    s1.push(x + y);
                    s2.pop();
                } else if (s2.empty() != true && s2.peek() == 2) {
                    int x = s1.pop();
                    int y = s1.pop();
                    s1.push(y - x);
                    s2.pop();
                }
                s2.push(2);
            }
        }
        if (flag == 1)
            s1.push(t);
        flag = 0;
        t=0;
        if (s2.empty() != true) {
            
            if (s2.peek() == 1) {
                int x = s1.pop();
                int y = s1.pop();
                s1.push(x + y);
                
            } else if (s2.peek() == 2) {
                int x = s1.pop();
                int y = s1.pop();
                s1.push(y - x);
            }
        }
        return s1.pop();
    }
}
~~~
击败24%

### Q232用栈实现队列
使用栈实现队列的下列操作：
push(x) -- 将一个元素放入队列的尾部。
pop() -- 从队列首部移除元素。
peek() -- 返回队列首部的元素。
empty() -- 返回队列是否为空。

难度 简单 

示例:
MyQueue queue = new MyQueue();
queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false

说明:
你只能使用标准的栈操作 -- 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。

这题很简单，只需要两个栈来回倒腾就好了。看到题目序号，莫名喜感。
~~~java
class MyQueue {
    Stack<Integer> s1;
    Stack<Integer> s2;
    /** Initialize your data structure here. */
    public MyQueue() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        while(s1.empty()!=true){
            s2.push(s1.pop());
        }int t = s2.pop();
        while(s2.empty()!=true){
            s1.push(s2.pop());
        }return t;
    }
    
    /** Get the front element. */
    public int peek() {
        while(s1.empty()!=true){
            s2.push(s1.pop());
        }int t = s2.peek();
        while(s2.empty()!=true){
            s1.push(s2.pop());
        }return t;
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return s1.empty();
    }
}
~~~
击败100%

### Q316去除重复字母
给你一个仅包含小写字母的字符串，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

难度 困难

示例 1:
输入: "bcabc"
输出: "abc"
示例 2:
输入: "cbacdcbc"
输出: "acdb"
注意：该题与 1081 https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters 相同

这题是没啥思路，试了下自己写，问题很多，他这个字典序很麻烦。看了官方题解 贪心算法。先用HashMap记录下字母最后出现的位置。建立一个双端队列，然后遍历字符串，如果队列的最后一个元素大于当前元素，且后面还有相同元素则将它弹出。然后将当前元素插入队列。最后将队列转换成字符串

~~~java
class Solution {
    public String removeDuplicateLetters(String s) {
        HashMap<Character,Integer> map = new HashMap<>();
        LinkedList<Character> l = new LinkedList<>();
        for(int i = 0;i<s.length();i++){
            map.put(s.charAt(i),i);
        }
        for(int i = 0;i<s.length();i++){
            if(!l.contains(s.charAt(i))){
                char ch = s.charAt(i);
                while(!l.isEmpty()&&l.peekLast()>ch&&map.get(l.peekLast())>i){
                    l.removeLast();
                }l.add((char)ch);
            }
        }StringBuilder sb = new StringBuilder();
        for(int i = 0;i<l.size();i++)
            sb.append((char)l.get(i));
        return sb.toString();
    }
}
~~~
击败63%

LinkedList有peek第一 也有peekLast最后。也有poll pollLast。他的empty还是isEmpty 

### Q215数组中的第K个最大元素
在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

难度 中等

示例 1:
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:
你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

我用库函数作弊了.
~~~java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length-k];
    }
}
~~~
击败91%

### Q347前 K 个高频元素
给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

难度 中等

示例 1:
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
示例 2:
输入: nums = [1], k = 1
输出: [1]
提示：
你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
你可以按任意顺序返回答案。

这个问题也叫topK问题

先用hashmap遍历一遍，记录次数。再用priorityqueue，遍历一遍hashmap。当peek.value<value,再将value放进去。

之前没用过priorityqueue，他的原理类似于堆，每次可以提供一个最值。

~~~java
Queue<Integer> q = new PriorityQueue<>(new Comparator<Integer>(){
        public int compare(Integer a,Integer b){
            return m.get(a)-m.get(b);
        }
    });
~~~
他需要一个Comparator类来提供方法 或者 是实现comparable的类。

其他操作于普通队列相似

完整代码如下
~~~java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer,Integer> m = new HashMap<>();
        Queue<Integer> q = new PriorityQueue<>(new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                return m.get(a)-m.get(b);
            }
        });
        for(int i:nums){
            m.put(i,m.getOrDefault(i,0)+1);
        }
        for(int i:m.keySet()){
            if(q.size()<k){
                q.add(i);
            }else if(m.get(i)>m.get(q.peek())){
                q.remove();
                q.add(i);
            }
        }
        int[] ans = new int[k];
        for(int i = 0 ;i<k;i++){
            ans[i] = q.poll();
        }
        return ans;

        
    }
}
~~~
击败73%

### Q21合并两个有序链表
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

难度 简单

示例：
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

这题没啥好说，直接上代码
~~~java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null&&l2==null) return l1;
        else if(l1!=null&&l2==null) return l1;
        else if(l2!=null&&l1==null) return l2;
        ListNode ans,p;
        if(l1.val>l2.val) {p = l2;l2 = l2.next;}
        else {p = l1;l1 = l1.next;}
        ans = p;
        while(l1!=null&&l2!=null){
            if(l1.val>l2.val){
                p.next = l2;
                l2 = l2.next;
            }else {
                p.next = l1;
                l1 = l1.next;
            }p = p.next;
        }if(l1!=null){
            p.next = l1;
        }else if(l2!=null){
            p.next = l2;
        }return ans;
    }
}
~~~
击败63%

### Q101对称二叉树
给定一个二叉树，检查它是否是镜像对称的。

难度 简单

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3


进阶：
你可以运用递归和迭代两种方法解决这个问题吗？

还是太菜了，参照了官方题解。利用递归求解。对称的指针，一个向左移，另一个向右移

~~~java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root,root);
    }
    public boolean check(TreeNode p,TreeNode q){
        if(p==null&&q==null)
            return true;
        if(p==null||q==null)
            return false;
        return p.val == q.val && check(p.left,q.right) &&check(p.right,q.left);
    }
}
~~~

击败100%

### Q104. 二叉树的最大深度
给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。

难度 简单

示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。

~~~java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null)    return 0;
        int x = maxDepth(root.left);
        int y = maxDepth(root.right);
        return (x>y?x:y)+1;
    }
}
~~~
击败100%

### Q226翻转二叉树
翻转一棵二叉树。

难度 简单

示例：
输入：

     4
   /   \
  2     7
 / \   / \
1   3 6   9
输出：

     4
   /   \
  7     2
 / \   / \
9   6 3   1
备注:
这个问题是受到 Max Howell 的 原问题 启发的 ：
谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。

这题很简单啊，用个递归。
~~~java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        change(root);
        return root;
    }
    public void change(TreeNode root){
        if(root==null) return;
        TreeNode r = root.right;
        root.right = root.left;
        root.left = r;
        change(root.right);
        change(root.left);
    }
}
~~~
击败100%，没想到我这么强，比Homebrew作者强啊（逃

### Q236二叉树的最近公共祖先
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

难度 中等

示例 1:
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
示例 2:
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。

说明:所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。

这题也没啥思路，自己写了个递归，写一半就知道不对。也就看了题解。（不知道我会不会太容易放弃了

这个根 的左子树一定含一个点，右子树一定含另一个点。所以我们只要递归，找到这两个点，然后返回回去。 如果返回值 一个是有 一个是没有，说明这个点不是，继续返回有的那个点。 如果返回值同时都有了，就说明这个点是根

~~~java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null||root == p||root==q) return root;
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
        if(left !=null && right!=null)  return root;
        if(left != null) return left;
        if(right != null) return right;
        return null;
    }
}
~~~
击败63%

### Q23合并K个升序链表
给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。

难度 困难 

示例 1：
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
示例 2：
输入：lists = []
输出：[]
示例 3：
输入：lists = [[]]
输出：[]
提示：
k == lists.length
0 <= k <= 10^4
0 <= lists[i].length <= 500
-10^4 <= lists[i][j] <= 10^4
lists[i] 按 升序 排列
lists[i].length 的总和不超过 10^4

暴力合并，每次找数组中最小的。
~~~java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0) return null;
        ListNode ans = null,head;
        int max = 0;
        for(int i = 0;i < lists.length;i++){
            if((ans==null&&lists[i]!=null)||(lists[i]!=null&&ans.val>lists[i].val)){
                ans = lists[i];
                max = i;
            }
        }
        head = ans;
        if(lists[max]==null) return null;
        lists[max] = lists[max].next;
        int flag = 1;
        while(flag!=0){
            flag = 0;
            for(int i = 0;i < lists.length;i++){
                if((ans.next==null&&lists[i]!=null)||(lists[i]!=null&&ans.next.val>lists[i].val)){
                    ans.next = lists[i];
                    max = i;
                    
                }if(lists[i]!=null) flag = 1;
            }
            if(flag==1&&lists[max]!=null)
                lists[max] = lists[max].next;
            if(flag==1&&ans.next!=null)
                ans = ans.next;
        }
        return head;
    }
}
~~~
击败5%

改用优先队列，先把所有节点装进优先队列里，然后再一个个取出来。
~~~java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0) return null;
        Queue<ListNode> q = new PriorityQueue<>(new Comparator<ListNode>(){
            public int compare(ListNode a,ListNode b){
                return a.val-b.val;
            }
        });
        for(int i = 0;i<lists.length;i++){
            while(lists[i]!=null){
                q.add(lists[i]);
                lists[i] = lists[i].next;
            }
        }
        ListNode ans,tempt;
        tempt = q.poll();
        ans = tempt;
        while(q.isEmpty()!=true){
            tempt.next = q.poll();
            tempt = tempt.next;
        }
        if(tempt!=null)
            tempt.next = null;
        return ans;
    }
}
~~~
击败58%。优先队列真香

我想这个题是有序链表，如果我一股脑丢尽优先队列，岂不是浪费了。我每次只丢头进去，每次取出一个，再把下一个丢进去。

结果还是击败58%。

### Q33搜索旋转排序数组
假设按照升序排序的数组在预先未知的某个点上进行了旋转。
( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。
搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。
你可以假设数组中不存在重复的元素。
你的算法时间复杂度必须是 O(log n) 级别。

难度 中等

示例 1:
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
示例 2:
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1

第一百道题

本来二分法就拉跨，还搞个这么复杂的二分法。期间还拜托朋友找二分法的bug。硬生生写了两个多小时

学会分情况太重要了

这个图特别重要，只要这个图，看懂了就会写了。

首先是左图，target落在左边一定要卡严格。一定是 target>nums[0] && target<nums[mid]，其他情况target一定在mid右边

而右图，如法炮制，target一定落在右边一定要卡死。一定是 target>nums[mid] && target<nums[nums.length-1]。其他情况一定在右边。

如何判断现在的情况是左图，还是右图呢。nums[mid]>nums[0] nums[mid]<nums[0]

~~~java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length==0)  return -1;
        if(nums.length==1)  return (nums[0]==target)?0:-1;
        int l=0,r=nums.length-1,mid=(l+r)/2;
        while(l<r){
            if(nums[mid]==target)   return mid;
            if(nums[mid]>=nums[0]){
                if(target<nums[mid]&&target>=nums[0]){
                    r = mid-1;
                }else{
                    l = mid+1; 
                }
            }else{
                if(target>nums[mid]&&target<=nums[nums.length-1]){
                    l = mid+1;
                }else{
                    r = mid-1;
                }
            }mid = (l+r)/2;
        }if(nums[mid]==target)
            return mid;
        return -1;
    }
}
~~~
击败100%

### Q34在排序数组中查找元素的第一个和最后一个位置
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
你的算法时间复杂度必须是 O(log n) 级别。
如果数组中不存在目标值，返回 [-1, -1]。

难度 中等

示例 1:
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
示例 2:
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]

又是一个复杂的二分法。先找左边那个点，r尽量往左靠。再找右边那个点，l尽量往右靠。
由于循环跑出来的结果，我也不太确定，也就多写了几个if 来判断
~~~java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        int ans[] = new int[2];
        if (nums.length == 0) {
            ans[0] = -1;
            ans[1] = -1;
            return ans;
        } else if (nums.length == 1) {
            if (nums[0] == target) {
                ans[0] = 0;
                ans[1] = 0;
                return ans;
            }
            ans[0] = -1;
            ans[1] = -1;
            return ans;
        }
        // boolean left = true;
        int mid = (l + r) / 2;
        while (l < r) {
            if (nums[mid] < target) {
                l = mid;
            } else if (nums[mid] > target) {
                r = mid;
            }else if (nums[mid] == target) {
                r = mid-1;
            }
            mid = (l + r) / 2;
            if(l==r-1) break;
        }
        if (nums[l] == target)
            ans[0] = l;
        else if (nums[r] == target)
            ans[0] = r;
        else if (r+1<nums.length && nums[r+1] == target){
            ans[0] = r+1;
        }
        else {
            ans[0] = -1;
            ans[1] = -1;
            return ans;
        }
        l = mid;
        r = nums.length - 1;

        while (l < r) {
            if (nums[mid] > target) {
                r = mid;

            } else if (nums[mid] < target) {
                l = mid;
            } else if (nums[mid]== target){
                l = mid+1;
            }
            mid = (l + r) / 2;
            if(l == r-1) break;
        }
        if (nums[r] == target)
            ans[1] = r;
        else if (nums[l]==target)
            ans[1] = l;
        else if (l-1>=0&&nums[l-1] == target)
            ans[1] = l-1;
        return ans;
    }
}
~~~
击败100%

### Q5最长回文子串
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

难度 中等

示例 1：
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：
输入: "cbbd"
输出: "bb"

第一次写动态规划。先学了一下什么是动态规划。非常像高中的归纳法。总结出 从n-1到n，然后再去看 0 的时候。

放到这题，就是s（i，j）要为回文串，s（i+1，j-1）为回文串&&s（i）==s（j）。
s（i，i）一定为回文串。s（i，i+1）需要判断s（i）==s（i+1）

然后就是写循环，从长度为0开始，到长度为n。
~~~java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        String ans = new String("");
        for (int l = 0; l < n; l++) {
            for (int i = 0; i + l < n; i++) {
                int j = i + l;
                if (l == 0) {
                    dp[i][j] = true;
                } else if (l == 1) {
                    if (s.charAt(i) == s.charAt(j)) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = false;
                    }
                } else {
                    if (dp[i + 1][j - 1] == true && s.charAt(i) == s.charAt(j)) {
                        dp[i][j] = true;
                    } else
                        dp[i][j] = false;
                }
                if (dp[i][j] == true && j - i + 1 > ans.length()) {
                    ans = s.substring(i, j + 1);
                }
            }
        }
        return ans;
    }
}
~~~
击败33%
### Q53最大子序和
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

难度 简单

示例:
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
进阶:
如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

刚写完上一题，学了锤子，就觉得哪都是钉子，写了半天，搞出了一个n2的算法，还内存超出了。

看了官方题解，才发现这题其实很简单，就是滑窗问题，只要看加上这个，还是只有这个哪个大。

~~~java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 0) return 0;
        int n = nums.length;
        int max =nums[0];
        int pre = 0;
        for(int i:nums){
            pre = Math.max(pre+i,i);
            max = Math.max(max,pre);
        }
        return max;
    }
}
~~~
击败95%

### Q62不同路径
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
问总共有多少条不同的路径？

难度 中等

例如，上图是一个7 x 3 的网格。有多少可能的路径？

示例 1:
输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右
示例 2:
输入: m = 7, n = 3
输出: 28
提示：
1 <= m, n <= 100
题目数据保证答案小于等于 2 * 10 ^ 9

这题其实特别简单啊。到(i,j)有多少种方法，就是到(i-1,j),(i,j-1)有多少种方法。 然后再把(0,j)(i,0)置为1

~~~java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for(int i = 0; i<m ;i++){
            dp[i][0] = 1;
        }for(int i = 0; i<n;i++)    
            dp[0][i] = 1;
        for(int i = 1; i<m;i++){
            for(int j = 1; j<n;j++){
                dp[i][j] = dp[i-1][j]+dp[i][j-1];
            }
        }return dp[m-1][n-1];
    }
}
~~~

### Q64最小路径和
给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
说明：每次只能向下或者向右移动一步。

难度 中等

示例:
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。

这题也很简单啊，跟上一题很类似，dp(i,j)的距离为 dp(i-1,j)+grim(i,j)和dp(i,j-1)+grim(i,j)最小值，先计算边上的距离，最后返回右小角的值。

~~~java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length,n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for(int i=1;i<m;i++){
            dp[i][0] = dp[i-1][0]+grid[i][0];
        }for(int i=1;i<n;i++){
            dp[0][i] = dp[0][i-1]+grid[0][i];
        }for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j] = Math.min(dp[i-1][j]+grid[i][j],dp[i][j-1]+grid[i][j]);
            }
        }return dp[m-1][n-1];
    }
}
~~~
击败87%

### Q70爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

难度 简单

示例 1：
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
示例 2：
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶

这题也很简单，dp[1] == 1 dp[2]==2，dp[n]=dp[n-2]+dp[n-1]

~~~java
class Solution {
    public int climbStairs(int n) {
        if(n==0) return 0;
        if(n==1) return 1;
        if(n==2) return 2;
        int[] dp =new int[n+1];
        dp[1] = 1;
        dp[2] = 2;
        for(int i =3;i<=n;i++){
            dp[i] = dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
}
~~~
击败100%

### Q118杨辉三角
给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。

难度 简单

在杨辉三角中，每个数是它左上方和右上方的数的和。
示例:
输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]

很简单，当每行第一个 或者 最后一个就add（1），其他就加上肩上两个
~~~java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>>  ans = new ArrayList<List<Integer>>();
        for(int i = 0;i<numRows;i++){
            ans.add(new ArrayList<>());
            for(int j = 0;j<i+1;j++){
                if(j == 0||j==i){
                    ans.get(i).add(1);
                }else {
                    ans.get(i).add(ans.get(i-1).get(j-1)+ans.get(i-1).get(j));
                }
            }
        }return ans;
    }
}
~~~
击败77%

### Q300最长上升子序列
给定一个无序的整数数组，找到其中最长上升子序列的长度。

难度 中等

示例:
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:
可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。
进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?

写了个n2的算法，dp记录长度，只要i>j nums[i]>nums[j]，就比较dp[i] dp[j]+1哪个大。

~~~java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        if(n==0) return 0;
        int[] dp = new int[n];
        dp[0] = 1;
        int max = 1;
        for (int i = 1; i < n; i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[j] + 1, dp[i]);
                    max = Math.max(max, dp[i]);
                }
            }
        }
        return max;
    }
}
~~~
击败19%

### Q1143最长公共子序列
给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。
一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。
若这两个字符串没有公共子序列，则返回 0。

难度 中等 

示例 1:
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
示例 2:
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc"，它的长度为 3。
示例 3:
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0。
提示:
1 <= text1.length <= 1000
1 <= text2.length <= 1000
输入的字符串只含有小写英文字符。

dp的题真有意思，就是写不出递推方程...

我自己写了一个dp方程，dp记录text1[i]能构成长度，last代表text1[i]在text2出现的地址，维度只有一维。不出所料挂掉了。因为这样，没有穷举所有的可能。

看了别人写的题解。（这题竟然没有官方题解），两维的dp数组,dp[i][j]记录的是text1[0，i],text[0,j]能构成最长的长度。if(text[i]==text[j]) dp[i][j] = dp[i-1][j-1]; 相当于从i-1，j-1增加到i，j。else dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]) 

然后额外处理下第一行/第一列
~~~java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        if (text1 == null || text2 == null)
            return 0;
        int n = text1.length();
        int m = text2.length();
        int[][] dp = new int[n][m];
        int flag = 0;
        for (int i = 0; i < n; i++) {
            if (text1.charAt(i) == text2.charAt(0))
                flag = 1;
            if (flag == 1)
                dp[i][0] = 1;
        }
        flag = 0;
        for (int j = 0; j < m; j++) {
            if (text1.charAt(0) == text2.charAt(j))
                flag = 1;
            if (flag == 1)
                dp[0][j] = 1;
        }
        for(int i = 1; i < n; i++){
            for(int j = 1; j < m; j++){
                if(text1.charAt(i)==text2.charAt(j)){
                    dp[i][j] = dp[i-1][j-1]+1;
                }else
                    dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j]);
            }
        }
        return dp[n-1][m-1];
    }

}
~~~
击败80%

### Q1227飞机座位分配概率
有 n 位乘客即将登机，飞机正好有 n 个座位。第一位乘客的票丢了，他随便选了一个座位坐下。
剩下的乘客将会：
如果他们自己的座位还空着，就坐到自己的座位上，
当他们自己的座位被占用时，随机选择其他座位
第 n 位乘客坐在自己的座位上的概率是多少？

难度 中等

示例 1：
输入：n = 1
输出：1.00000
解释：第一个人只会坐在自己的位置上。
示例 2：
输入: n = 2
输出: 0.50000
解释：在第一个人选好座位坐下后，第二个人坐在自己的座位上的概率是 0.5。
提示：
1 <= n <= 10^5

这题好迷，写着是dp

1号登机，有三种选择 1）坐1号 1/n 2）坐n号 1/n 3）其他位置i号 1/n × n;

i号登机，有三种选择 1）坐i号 1/n 2）坐n号 1/n 3）其他位置j号 1/(n-i+1) × (n-i+1);

所以i 与 1 是相同的

f(i) = 1/i + Σf(n-j+1)/i

就把i化解成 Σ(n-j+1),实际上就是从1求和到i-1；

然后我也按照这样写了，结果竟然超时了。

评论区说 return (n==1)?1:0.5;

~~~java
class Solution {
    public double nthPersonGetsNthSeat(int n) {
        if(n==1) return 1;
        return 0.5;
    }
}
~~~

### Q22括号生成
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

难度 中等

示例：

输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]

我知道思路，可是我写出来就是怪怪的...

看了官方题解，用一个void 函数，把结果存在实参数组里

~~~java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        StringBuilder s = new StringBuilder("");
        next(result,s,n,0,0);
        return result;
    }
    public void next(List<String> l,StringBuilder s,int n,int x,int y){
        if(n==x&&n==y){
            l.add(s.toString());
            return ;
        }
        if(x<n){
            s.append("(");
            next(l,s,n,x+1,y);
            s.deleteCharAt(s.length()-1);
        }if(y<x){
            s.append(")");
            next(l,s,n,x,y+1);
            s.deleteCharAt(s.length()-1);
        }
        return ;
    }
}
~~~
击败96

### Q46全排列
给定一个 没有重复 数字的序列，返回其所有可能的全排列。

难度 中等

示例:
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

这题之前做过类似的，没做出来。

知道是回溯法，但不知道咋实现。

官方题解的思路是，从first = 0开始填写，然后把nums[first] 与 nums[nums.length-1] 依次交换。

~~~java
import java.util.*;
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        for(int i:nums) {
            temp.add(i);
        }
        dfs(ans,temp,0,nums.length);
        return ans;
    }
    public void dfs(List<List<Integer>> ans,List<Integer> temp,int first,int n){
        if(first == n){
            ans.add(new ArrayList<Integer>(temp));
            return;
        }
        for(int i=first;i<n;i++){
            Collections.swap(temp, first, i);
            dfs(ans, temp, first+1,n);
            Collections.swap(temp, first, i);
        }
    }
}
~~~
击败99% 

### Q94二叉树的中序遍历
给定一个二叉树，返回它的中序 遍历。

示例:

输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

用递归这题真是白给
~~~java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        in(result, root);
        return result;
    }

    public void in(List<Integer> l ,TreeNode e){
        if(e==null) return;
        in(l,e.left);
        l.add(e.val);
        in(l,e.right);
    }
}
~~~
击败100% 迭代算法是啥？

### Q102二叉树的层序遍历
给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

难度 中等

示例：
二叉树：[3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]

这题也是白送，用队列一下就写出来了
~~~java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root==null) return new ArrayList<>();
        Queue<Integer> level = new LinkedList<>();
        List<List<Integer>> l = new ArrayList<>();
        Queue<TreeNode> nodes = new LinkedList<>();
        List<Integer> temp = new ArrayList<>();
        
        level.add(1);
        nodes.add(root);
        int level1 = 1;
        while (nodes.size()!=0){
            int level2 = level.poll();
            if(level1!=level2){
                l.add(temp);
                temp = new ArrayList<>();
                level1 = level2;
            }
            TreeNode t = nodes.poll();
            temp.add(t.val);
            if(t.left!=null){
                nodes.add(t.left);
                level.add(level1+1);
            }
            if(t.right!=null){
                nodes.add(t.right);
                level.add(level1+1);
            }
        }
        l.add(temp);
        return l;
    }
}
~~~
击败92%

### Q110平衡二叉树
给定一个二叉树，判断它是否是高度平衡的二叉树。
本题中，一棵高度平衡二叉树定义为：
一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

难度 简单

示例 1:
给定二叉树 [3,9,20,null,null,15,7]

    3
   / \
  9  20
    /  \
   15   7
返回 true 。

示例 2:
给定二叉树 [1,2,2,3,3,null,null,4,4]

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
返回 false 。

这题虽说是简单，但是想要准确无误写出来，应该还是有点难度。用dfs，递归左右子树深度，如果相差大于1，返回-1。如果左右有一个为-1，返回-1

~~~java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(high(root) == -1)  return false;
        return true;
    }
    public int high(TreeNode root) {
        if(root == null) return 0;
        int left = high(root.left);
        int right = high(root.right);
        if(left==-1||right==-1||Math.abs(left-right)>1) return -1;
        return Math.max(left+1,right+1);
    }
}
~~~
击败99% 

### Q144二叉树的前序遍历
给定一个二叉树，返回它的 前序 遍历。

难度 中等

示例:
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

又是一题白送
~~~java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        dfs(l, root);
        return l;
    }
    public void dfs(List<Integer> ans, TreeNode root) {
        if(root == null) return;
        ans.add(root.val);
        dfs(ans, root.left);
        dfs(ans, root.right);
    }
}
~~~
击败100%

### Q145二叉树的后序遍历
给定一个二叉树，返回它的 后序 遍历。

难度 中等

示例:
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

如上
~~~java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        dfs(l, root);
        return l;
    }
    public void dfs(List<Integer> ans, TreeNode root) {
        if(root == null) return;
        dfs(ans, root.left);
        dfs(ans, root.right);
        ans.add(root.val);
    }
}
~~~
击败100%

### Q98验证二叉搜索树
给定一个二叉树，判断其是否是一个有效的二叉搜索树。
假设一个二叉搜索树具有如下特征：
节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

难度 中等

示例 1:
输入:
    2
   / \
  1   3
输出: true
示例 2:
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。

中序遍历，看顺序是否正确

~~~java
class Solution {
    public boolean isValidBST(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        dfs(l, root);
        for(int i=0; i<l.size()-1; i++){
            if(l.get(i)>=l.get(i+1))
                return false;
        }
        return true;
    }
    public void dfs(List<Integer> ans, TreeNode root) {
        if(root == null) return;
        dfs(ans, root.left);
        ans.add(root.val);
        dfs(ans, root.right);
    }
}
~~~
击败31%

感觉这样效率也太低了，改用递归写，递归左边，就修改上界；递归右边，就修改下界。

本来最初界限使用Integer.MAX_VALUE 写的，结果样例被卡。改用Long.MAX_VALUE

~~~java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        if (root.left!=null&&dfs(root.left, root.val, Long.MIN_VALUE) == false)
            return false;
        if (root.right!=null&&dfs(root.right, Long.MAX_VALUE, root.val) == false)
            return false;
        return true;
    }

    public boolean dfs(TreeNode root, long high, long low) {
        if(root.val>=high||root.val<=low) return false;
        boolean left=true, right=true;
        if (root.left != null) {
            if (high > root.val)
                left = dfs(root.left, root.val, low);
            else
                left = dfs(root.left, high, low);
        }
        if (root.right != null) {
            if (low > root.val)
                right = dfs(root.right, high, low);
            else
                right = dfs(root.right, high, root.val);
        }
        if (left == false || right == false)
            return false;
        return true;
    }
}
~~~
击败100%

### Q701二叉搜索树中的插入操作
给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 保证原始二叉搜索树中不存在新值。
注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回任意有效的结果。

难度 中等

例如, 
给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和 插入的值: 5
你可以返回这个二叉搜索树:

         4
       /   \
      2     7
     / \   /
    1   3 5
或者这个树也是有效的:

         5
       /   \
      2     7
     / \   
    1   3
         \
          4

这题也没啥难度，只要先找到位置，然后新建个节点

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) return new TreeNode(val);
        TreeNode t1 = root, t2 = root;
        while (t2 != null) {
            if (val > t2.val) {
                t1 = t2;
                t2 = t2.right;
            }
            else if (val < t2.val) {
                t1 = t2;
                t2 = t2.left;
            }
        }
        if (t1.val > val) {
            t1.left = new TreeNode(val);
        } else {
            t1.right = new TreeNode(val);
        }
        return root;
    }
}
~~~
击败100%

### Q450删除二叉搜索树中的节点
给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。
一般来说，删除节点可分为两个步骤：
首先找到需要删除的节点；
如果找到了，删除它。
说明： 要求算法时间复杂度为 O(h)，h 为树的高度。

难度 中等

示例:
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。

一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。

    5
   / \
  4   6
 /     \
2       7

另一个正确答案是 [5,2,6,null,4,null,7]。

    5
   / \
  2   6
   \   \
    4   7

他这个样例给的太简单，会给人造成一种很简单的假象

翻了下数据结构，就是把p的右分支接到c的最右分支上，但是有很多细节要处理。

f为null，p为null，c为null，删除的节点就在root

~~~java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        TreeNode f = null, p = root;
        while (p != null && p.val != key) {
            if (p.val < key) {
                f = p;
                p = p.right;
            } else if (p.val > key) {
                f = p;
                p = p.left;
            }
        }
        if (p == null) {
            return root;
        }
        TreeNode c = p.left;
        TreeNode pr = p.right;
        if (f == null) {
            if (c == null) {
                return pr;
            }
            TreeNode temp = c;
            while (temp.right != null)
                temp = temp.right;
            temp.right = pr;
            return c;
        }
        if (c == null) {
            if (f.left!=null&&f.left.val == key) {
                f.left = pr;
                return root;
            } else {
                f.right = pr;
                return root;
            }
        }
        TreeNode temp = c;
        while (temp.right != null)
            temp = temp.right;
        temp.right = pr;
        if (f.left!=null&&f.left.val == key) {
            f.left = c;

        } else {
            f.right = c;
        }
        return root;
    }
}
~~~

### Q338比特位计数
给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

难度 中等

示例 1:
输入: 2
输出: [0,1,1]
示例 2:
输入: 5
输出: [0,1,1,2,1,2]
进阶:
给出时间复杂度为O(n*sizeof(integer))的解答非常容易。但你可以在线性时间O(n)内用一趟扫描做到吗？
要求算法的空间复杂度为O(n)。
你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 __builtin_popcount）来执行此操作。

动态规划问题，复杂度O（n），如果n为奇数，个数比前一个大一。如果n为偶数，则个数与n/2相同（n/2向左移即为n）。

~~~java
class Solution {
    public int[] countBits(int num) {
        int[] x = new int[num + 1];
        for (int i = 1; i <= num; i++) {
            if (x[i] != 0) {

            } else if (i % 2 != 0) {
                x[i] = x[i - 1] + 1;
            } else {
                x[i] = x[i/2];
            }
        }
        return x;
    }
}
~~~
击败74%

### Q198打家劫舍
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

难度 简单

示例 1：
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2：
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。

提示：
0 <= nums.length <= 100
0 <= nums[i] <= 400

动态规划问题，我一开始想得很复杂，看到别人的dp方程是 dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]),我在想 3 1 1 2这种情况可以通过吗？

dp[i-2]+nums[i] ,并不代表是偷了[i-2]。

换个角度来看，应该是选择偷不偷，偷就是i-2中的最大与i之和，不偷是i-1最大。
（其实感觉还是有很多疑惑
~~~java
class Solution {
    public int rob(int[] nums) {
        if(nums.length==0) return 0;
        int[] dp = new int[nums.length];
        for(int i = 0; i < nums.length; i++){
            if(i==0) dp[0] = nums[0];
            else if(i==1) dp[1] = Math.max(nums[0], nums[1]);
            else{
                dp[i] = Math.max(dp[i-1], dp[i-2]+nums[i]);
            }
        }
        return dp[nums.length-1];
    }
}
~~~
击败100%

### Q617合并二叉树
给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。
你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

难度 简单

示例 1:
输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
注意: 合并必须从两个树的根节点开始。

深度搜索
~~~java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null)
            return null;
        if (t1 != null && t2 == null)
            return t1;
        if (t1 == null && t2 != null)
            return t2;
        if (t1 != null && t2 != null)
            t1.val += t2.val;
        dfs(t1, t2);
        return t1;
    }

    public void dfs(TreeNode t1, TreeNode t2) {
        if (t1 == null || t2 == null)
            return;

        if (t1.left == null && t2.left == null)
            ;
        else if (t1.left == null && t2.left != null)
            t1.left = t2.left;
        else if (t1.left != null && t2.left == null)
            ;
        else {
            t1.left.val += t2.left.val;
            dfs(t1.left, t2.left);
        }

        if (t1.right == null && t2.right == null)
            ;
        else if (t1.right == null && t2.right != null)
            t1.right = t2.right;
        else if (t1.right != null && t2.right == null)
            ;
        else {
            t1.right.val += t2.right.val;
            dfs(t1.right, t2.right);
        }
        return;
    }
}
~~~
击败64%
### Q39组合总和
给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的数字可以无限制重复被选取。
说明：
所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 

难度 中等

示例 1：
输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
示例 2：
输入：candidates = [2,3,5], target = 8,
所求解集为：
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
提示：
1 <= candidates.length <= 30
1 <= candidates[i] <= 200
candidate 中的每个元素都是独一无二的。
1 <= target <= 500

又是一个溯源，不太会写。

溯源要记住那个二叉树，一边是加上这个点，一边是不加这个点。一边是增加点 递归，另一边是删除点 递归。

~~~java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if(candidates.length == 0) return null;
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        f(result, temp, candidates, target, 0, 0);
        return result;
    }

    public void f(List<List<Integer>> l, List<Integer> temp, int[] candidates, int target, int sum, int pos) {
        if (sum == target) {
            l.add(new ArrayList<Integer>(temp));
            return;
        }
        if (sum > target)
            return;
        if (pos == candidates.length)
            return;
        while (sum <= target) {
            temp.add(candidates[pos]);
            sum += candidates[pos];
            f(l, temp, candidates, target, sum, pos + 1);
        }
        while (temp.size() >= 1 && temp.get(temp.size() - 1) == candidates[pos]) {
            temp.remove(temp.size() - 1);
            sum -= candidates[pos];
        }
        f(l, temp, candidates, target, sum, pos + 1);
    }
}
~~~
基本就是暴力法了，击败14%

### Q17电话号码的字母组合
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

难度 中等

示例:
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

回溯法。当数字为i，则字符串增加对应字母，多次递归。

~~~java
class Solution {
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0) return new ArrayList<>();
        List<String> ans = new ArrayList<>();
        String s = new String();
        fun(ans,s,0,digits);
        return ans;
    }
    public void fun(List<String> l,String s,int pos,String digits) {
        if(pos == digits.length()){
            l.add(s);
            return ;
        }
        if(digits.charAt(pos)=='2'){
            fun(l,new StringBuilder(s).append('a').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('b').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('c').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='3'){
            fun(l,new StringBuilder(s).append('d').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('e').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('f').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='4'){
            fun(l,new StringBuilder(s).append('g').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('h').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('i').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='5'){
            fun(l,new StringBuilder(s).append('j').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('k').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('l').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='6'){
            fun(l,new StringBuilder(s).append('m').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('n').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('o').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='7'){
            fun(l,new StringBuilder(s).append('p').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('q').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('r').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('s').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='8'){
            fun(l,new StringBuilder(s).append('t').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('u').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('v').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='9'){
            fun(l,new StringBuilder(s).append('w').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('x').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('y').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('z').toString(),pos+1,digits);
        }
    } 
}
~~~
击败88%

### Q114二叉树展开为链表
给定一个二叉树，原地将它展开为一个单链表。

难度 中等

例如，给定二叉树

    1
   / \
  2   5
 / \   \
3   4   6
将其展开为：

1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6

本来想着原地修改就得，边递归边改。其实不用。Java的list存的是指针，开销不会太大，前序遍历存进list里

~~~java
class Solution {
    public void flatten(TreeNode root) {
        if(root == null) return;
        List <TreeNode> l = new ArrayList<>();
        preorder(l, root);
        TreeNode t = root;
        t.left = null;
        for(int i=1;i<l.size();i++){
            t.left = null;
            t.right = l.get(i);
            t = t.right;
        }
        t.right = null;
    }
    public void preorder(List<TreeNode> l,TreeNode root) {
        if(root==null) return;
        l.add(root);
        preorder(l,root.left);
        preorder(l,root.right);
    }
}
~~~
击败45%

### Q238除自身以外数组的乘积
给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

难度 中等

示例:
输入: [1,2,3,4]
输出: [24,12,8,6]
提示：题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。
说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。
进阶：
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）

一下子就想到用除法...

看了下题解用L R数组分别记录，i左边和右边的乘积，L只要从左向右遍历一次，R只要从右向左遍历。

~~~java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if(nums.length == 0)    return new int[0];
        int[] L = new int[nums.length];
        int[] R = new int[nums.length];
        L[0] = 1;
        R[nums.length - 1] = 1;
        for(int i = 1; i < nums.length; i++){
            L[i] = nums[i-1]*L[i-1];
            R[nums.length-i-1] = nums[nums.length-i]*R[nums.length-i];
        }
        int[] ans = new int[nums.length];
        for(int i = 0; i < nums.length; i++){
            ans[i] = R[i]*L[i];
        }
        return ans;
    }
}
~~~
击败34%

### Q136只出现一次的数字

难度 简单

给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**说明：**

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

**示例 1:**

```
输入: [2,2,1]
输出: 1
```

**示例 2:**

```
输入: [4,1,2,1,2]
输出: 4
```

这题一下子想到了hashmap，然而。

利用异或解决 n^n = 0;n^0 = n;

~~~java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for(int i : nums)
            ans^=i;
        return ans;
    }
}
~~~

击败99%

### Q[48旋转图像](https://leetcode-cn.com/problems/rotate-image/)

难度 中等

给定一个 *n* × *n* 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

**说明：**

你必须在**[原地](https://baike.baidu.com/item/原地算法)**旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要**使用另一个矩阵来旋转图像。

**示例 1:**

```
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**示例 2:**

```
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

跟之前一个螺旋矩阵有一丝相似。

每次定位四个点，四个点互相交换。每次向里进一层。

~~~java
           class Solution {
    public void rotate(int[][] matrix) {
        if(matrix.length == 0) return ;
        int n = matrix.length;
        for(int t = 0;t<matrix.length/2;t++){
            int x1 = t,y1 = t;
            int x2 = t,y2 = n-t-1;
            int x3 = n-t-1,y3 = n-t-1;
            int x4 = n-t-1,y4 = t;
            for(int i = t;i<matrix.length-t-1;i++){
                int temp = matrix[x1][y1];
                matrix[x1][y1] = matrix[x4][y4];
                int temp2 = matrix[x2][y2];
                matrix[x2][y2] = temp;
                temp = matrix[x3][y3];
                matrix[x3][y3] = temp2;
                matrix[x4][y4] = temp;
                y1++;
                x2++;
                y3--;
                x4--;
            }
        }
    }
}
~~~

击败100%

### Q[96不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

难度 中等

给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？

**示例:**

```
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

看到就知道应该是dp问题了，但是还是不会写。

G（n）表示有多少种结构，F（i，n）表示以i为根 n为长度

G（n）=ΣF（i，n）

F（i，n）=G（i-1）×G（n-i） （乘法原则 左右子树）

G（0）= G（1）= 1

~~~java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i=2;i<=n;i++){
            for(int j=1;j<=i;j++){
                dp[i] += dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
}
~~~

由于递推公式的n与代码的n不同，写了个挺深的bug

击败100%

### [Q55跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

难度 中等

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

**示例 1:**

```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```

**示例 2:**

```
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```

这题是白送的，只要记录能跳到最远位置就可以了

~~~java
class Solution {
    public boolean canJump(int[] nums) {
        int max = 0;
        for(int i = 0; max<nums.length && i <= max;i++){
            max = max>i+nums[i]?max:i+nums[i];
        }
        if(max >= nums.length-1) return true;
        return false;
    }
}
~~~

击败99%

### [Q287寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

难度 中等

给定一个包含 *n* + 1 个整数的数组 *nums*，其数字都在 1 到 *n* 之间（包括 1 和 *n*），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

**示例 1:**

```
输入: [1,3,4,2,2]
输出: 2
```

**示例 2:**

```
输入: [3,1,3,4,2]
输出: 3
```

**说明：**

1. **不能**更改原数组（假设数组是只读的）。
2. 只能使用额外的 *O*(1) 的空间。
3. 时间复杂度小于 *O*(*n*2) 。
4. 数组中只有一个重复的数字，但它可能不止重复出现一次。

这题限制条件很多，不好写。用快慢指针法，龟兔赛跑，可以判断出环。但是没法猜出哪个是入口

a为起点到入口距离。b为环中已走，c为环剩下。L为环长

L = b+c

2(a+b) = kL+a+b



a = kL-b

此时把慢指针移到起点，快慢指针一次只走一步。 当慢指针走到起点时 走了a，快指针kL + a + b +kL - b，快指针也走到路口。

~~~java
class Solution {
    public int findDuplicate(int[] nums) {
        int fast = 0,slow = 0;
        do{
            slow = nums[slow];
            fast = nums[nums[fast]];
        }while(slow!=fast);
        slow = 0;
        while(slow!=fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
~~~

击败100%

### Q[739每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

难度中等520

请根据每日 `气温` 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

例如，给定一个列表 `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。

**提示：**`气温` 列表长度的范围是 `[1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。



一开始写了个n2的算法。看了题解，只要每个元素记住最近的，比他大的元素即可（不一定是最大）

T[i-1]<T[i],这种情况直接赋值

T[i-1]>=T[i]就去查询big[i] 查离他最近的元素

记得赋值-1

~~~java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        if(T.length == 0) return new int[0];
        int[] big = new int[T.length];
        big[T.length-1] = -1;
        int[] pos = new int[T.length];
        int temp = 0;
        for(int i = T.length-1;i >= 1 ;i--) {
            big[i-1] = -1;
            if(T[i-1]<T[i]){
                pos[i-1] = 1;
                big[i-1] = i; 
            }
            else{
                temp = big[i];
                while(temp!=-1&&T[i-1]>=T[temp]){
                    temp = big[temp];
                }
                if(temp!=-1&&T[i-1]<T[temp]){
                    pos[i-1] = temp-i+1;
                    big[i-1] = temp; 
                }
            }
        }
        return pos;
    }
}
~~~

击败98%

### Q[169多数元素](https://leetcode-cn.com/problems/majority-element/)

难度 简单

给定一个大小为 *n* 的数组，找到其中的多数元素。多数元素是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1:**

```
输入: [3,2,3]
输出: 3
```

**示例 2:**

```
输入: [2,2,1,1,1,2,2]
输出: 2
```



投票算法，先假定第一个是候选人maj，然后遍历数组，当x==maj count++，x!=maj count--，当count==0换届。

不用害怕真正的候选人选不上。当他不是候选人时，会把假的票下去，当他是真的，假的不会把他压倒。

~~~java
class Solution {
    public int majorityElement(int[] nums) {
        int maj = nums[0];
        int count = 0;
        for(int i :nums){
            if(i == maj){
                count ++;
            }
            else{
                count --;
                if(count == 0)
                {    
                    maj = i;
                    count++;
                }
            }
        }
        return maj;
    }
}
~~~

击败99%

### Q[283移动零](https://leetcode-cn.com/problems/move-zeroes/)

难度 简单

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例:**

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**说明**:

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

两个指针，i遍历数组，j从i+1开始寻找0

~~~java
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0,j = 0;
        while(i < nums.length){
            if(nums[i] == 0){
                break;
            }
            i++;
        }
        while(i < nums.length&&j<nums.length){
            if(nums[i] == 0){
                j = i+1;
                while(j < nums.length){
                    if(nums[j]!=0)
                        break;
                    j++;
                }
                if(j>=nums.length)
                    break;
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
            }
            i++;
        }
        return ;
    }
}
~~~

击败12%



### [Q49字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

难度 中等

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。

这题刚开始以为是回溯法，并不是，是按照要求分组。

建立一个hashmap 在String integer，为了准确记录应存放位置，我们对String进行一个排序。把String转换成char[] 然后排序。

~~~java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if(strs.length == 0) return new ArrayList<>();
        List<List<String>> ans = new ArrayList<>();
        List<String> temp = new ArrayList<>();
        int count = 0;
        HashMap<String, Integer> m = new HashMap<>();
        for (String str : strs) {
            String s = sortChar(str);
            int index =m.getOrDefault(s,-1);
            if(index==-1){
                temp = new ArrayList<>();
                temp.add(str);
                ans.add(temp);
                m.put(s,count);
                count++;
            }
            else{
                ans.get(index).add(str);
            }
        }
        return ans;
    }
    public String sortChar(String str) {
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        return new String(chars);
    }
}
~~~

击败73%

### Q[448找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

难度 简单

给定一个范围在 1 ≤ a[i] ≤ *n* ( *n* = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, *n*] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为*O(n)*的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

**示例:**

```
输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]
```

要求原地修改，就把对应的数字改成负数，然后最后在看哪些是负数

~~~java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int num = Math.abs(nums[i]);
            if (nums[num - 1] > 0)
                nums[num - 1] = -nums[num - 1];
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                ans.add(i + 1);
            }
        }
        return ans;
    }
}
~~~

击败70%

### [Q867转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

难度 简单

给定一个矩阵 `A`， 返回 `A` 的转置矩阵。

矩阵的转置是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

**示例 1：**

```
输入：[[1,2,3],[4,5,6],[7,8,9]]
输出：[[1,4,7],[2,5,8],[3,6,9]]
```

**示例 2：**

```
输入：[[1,2,3],[4,5,6]]
输出：[[1,4],[2,5],[3,6]] 
```

**提示：**

1. `1 <= A.length <= 1000`
2. `1 <= A[0].length <= 1000`

这个题白送

~~~java
class Solution {
    public int[][] transpose(int[][] A) {
        int n = A.length;
        int m = A[0].length;
        if(n == 0) return new int[0][0];
        int[][] x = new int[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                x[i][j] = A[j][i];
            }
        }
        return x;
    }
}
~~~

击败100%

### [Q117填充每个节点的下一个右侧节点指针 II](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)

难度中等269

给定一个二叉树

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`。

 

**进阶：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

 

**示例：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/15/117_sample.png)

```
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。
```

 

**提示：**

- 树中的节点数小于 `6000`
- `-100 <= node.val <= 100`

看到感觉很奇怪，这题咋会中等，用个队列就搞定了

~~~java
class Solution {
    public Node connect(Node root) {
        if(root == null) return null;
        Queue<Node> queue = new LinkedList<>();
        Queue<Integer> level = new LinkedList<>();
        queue.add(root);
        level.add(1);
        int prel = 0;
        Node preN = root;
        Node n;
        while (queue.isEmpty() != true) {
            n = queue.poll();
            int l = level.poll();
            if (l == prel) {
                preN.next = n;
            }else{
                preN.next = null;
            }
            preN = n;
            prel = l;
            if (n.left != null) {
                queue.add(n.left);
                level.add(l + 1);
            }
            if (n.right != null) {
                queue.add(n.right);
                level.add(l + 1);
            }
        }
        return root;
    }
}
~~~

击败7%，真是打扰了

原来得用递归啊。

### Q[977有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

难度 简单

给定一个按非递减顺序排序的整数数组 `A`，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。



**示例 1：**

```
输入：[-4,-1,0,3,10]
输出：[0,1,9,16,100]
```

**示例 2：**

```
输入：[-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

 

**提示：**

1. `1 <= A.length <= 10000`
2. `-10000 <= A[i] <= 10000`
3. `A` 已按非递减顺序排序。

这题还不简单，我一开始想先找到零点，后来发现很复杂，要考虑很多特例。然后用了优先队列来求（杀鸡用牛刀

~~~java
class Solution {
    public int[] sortedSquares(int[] A) {
        if(A.length==0) return new int[0];
        Queue<Integer> q = new PriorityQueue<>(new Comparator<Integer>() {
            public int compare(Integer a, Integer b) {
                return a-b;
            }
        });
        for(int i:A){
            q.add(i*i);
        }
        int[] t = new int[q.size()];
        int i = 0;
        while(q.size()!=0){
            t[i++] = q.poll();
        }
        return t;
    }
}
~~~

不出意外 击败6%

~~~java
class Solution {
    public int[] sortedSquares(int[] A) {
        if (A.length == 0)
            return new int[0];
        int[] result = new int[A.length];
        int i = A.length - 1;
        int l = 0, r = A.length - 1;
        while (r != l && l < A.length && r > -1) {
            if (A[r] * A[r] >= A[l] * A[l]) {
                result[i--] = A[r] * A[r];
                r--;
            } else {
                result[i--] = A[l] * A[l];
                l++;
            }
        }
        if (l < A.length && r > -1)
            result[0] = A[r] * A[r];
        return result;
    }
}
~~~

改用双指针，从两边进，先赋值最大的。击败66%

### Q[1528重新排列字符串](https://leetcode-cn.com/problems/shuffle-string/)

难度 简单

给你一个字符串 `s` 和一个 **长度相同** 的整数数组 `indices` 。

请你重新排列字符串 `s` ，其中第 `i` 个字符需要移动到 `indices[i]` 指示的位置。

返回重新排列后的字符串。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/26/q1.jpg)

```
输入：s = "codeleet", indices = [4,5,6,7,0,2,1,3]
输出："leetcode"
解释：如图所示，"codeleet" 重新排列后变为 "leetcode" 。
```

**示例 2：**

```
输入：s = "abc", indices = [0,1,2]
输出："abc"
解释：重新排列后，每个字符都还留在原来的位置上。
```

**示例 3：**

```
输入：s = "aiohn", indices = [3,1,4,2,0]
输出："nihao"
```

**示例 4：**

```
输入：s = "aaiougrt", indices = [4,0,2,6,7,3,1,5]
输出："arigatou"
```

**示例 5：**

```
输入：s = "art", indices = [1,0,2]
输出："rat"
```

 

**提示：**

- `s.length == indices.length == n`
- `1 <= n <= 100`
- `s` 仅包含小写英文字母。
- `0 <= indices[i] < n`
- `indices` 的所有的值都是唯一的（也就是说，`indices` 是整数 `0` 到 `n - 1` 形成的一组排列）。

char数组一个个设

~~~java
class Solution {
    public String restoreString(String s, int[] indices) {
        char[] st = new char[s.length()];
        for(int i=0;i<s.length();i++) 
            st[indices[i]] = s.charAt(i);
        return String.valueOf(st);
    }
}
~~~

击败53%

### Q[1323 6和9 组成的最大数字](https://leetcode-cn.com/problems/maximum-69-number/)

难度简单33

给你一个仅由数字 6 和 9 组成的正整数 `num`。

你最多只能翻转一位数字，将 6 变成 9，或者把 9 变成 6 。

请返回你可以得到的最大数字。

 

**示例 1：**

```
输入：num = 9669
输出：9969
解释：
改变第一位数字可以得到 6669 。
改变第二位数字可以得到 9969 。
改变第三位数字可以得到 9699 。
改变第四位数字可以得到 9666 。
其中最大的数字是 9969 。
```

**示例 2：**

```
输入：num = 9996
输出：9999
解释：将最后一位从 6 变到 9，其结果 9999 是最大的数。
```

**示例 3：**

```
输入：num = 9999
输出：9999
解释：无需改变就已经是最大的数字了。
```

 

**提示：**

- `1 <= num <= 10^4`
- `num` 每一位上的数字都是 6 或者 9 。

把string和num来回倒腾就可以了

~~~java
class Solution {
    public int maximum69Number (int num) {
        String s = ""+num;
        for(int i=0;i<s.length();i++){
            if(s.charAt(i) == '6'){
                StringBuilder sb = new StringBuilder(s);
                sb.setCharAt(i,'9');
                return Integer.parseInt(sb.toString());
            }
        }
        return num;
    }
}
~~~

击败24%



### Q[260只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

难度中等296

给定一个整数数组 `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。

**示例 :**

```
输入: [1,2,1,3,2,5]
输出: [3,5]
```

**注意：**

1. 结果输出的顺序并不重要，对于上面的例子， `[5, 3]` 也是正确答案。
2. 你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？

利用Hashmap求解

~~~java
class Solution {
    public int[] singleNumber(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i : nums) {
            map.put(i, map.getOrDefault(i, 0) + 1);
        }
        int[] ans = new int[2];
        int pos = 0;
        for (int i : nums) {
            if (map.get(i) == 1)
                ans[pos++] = i;
            if (pos == 2)
                return ans;
        }
        return ans;
    }
}
~~~

击败15%

### Q[933最近的请求次数](https://leetcode-cn.com/problems/number-of-recent-calls/)

难度  简单

写一个 `RecentCounter` 类来计算最近的请求。

它只有一个方法：`ping(int t)`，其中 `t` 代表以毫秒为单位的某个时间。

返回从 3000 毫秒前到现在的 `ping` 数。

任何处于 `[t - 3000, t]` 时间范围之内的 `ping` 都将会被计算在内，包括当前（指 `t` 时刻）的 `ping`。

保证每次对 `ping` 的调用都使用比之前更大的 `t` 值。

 

**示例：**

```
输入：inputs = ["RecentCounter","ping","ping","ping","ping"], inputs = [[],[1],[100],[3001],[3002]]
输出：[null,1,2,3,3]
```

 

**提示：**

1. 每个测试用例最多调用 `10000` 次 `ping`。
2. 每个测试用例会使用严格递增的 `t` 值来调用 `ping`。
3. 每次调用 `ping` 都有 `1 <= t <= 10^9`。

 这个题也是白送。只要两个指针，一个记住空位置，一个记住最近的时间

~~~java
class RecentCounter {
    int[] x;
    int pos;
    int times;

    public RecentCounter() {
        x = new int[10000];
        pos = 0;
        times = 0;
    }
    
    public int ping(int t) {
        x[pos++] = t;
        while(times<pos&&t-x[times]>3000){
            times++;
        }
        return pos-times;
    }
}
~~~

击败73%

### Q[474一和零](https://leetcode-cn.com/problems/ones-and-zeroes/)

难度 中等

在计算机界中，我们总是追求用有限的资源获取最大的收益。

现在，假设你分别支配着 **m** 个 `0` 和 **n** 个 `1`。另外，还有一个仅包含 `0` 和 `1` 字符串的数组。

你的任务是使用给定的 **m** 个 `0` 和 **n** 个 `1` ，找到能拼出存在于数组中的字符串的最大数量。每个 `0` 和 `1` 至多被使用**一次**。

 

**示例 1:**

```
输入: strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出: 4
解释: 总共 4 个字符串可以通过 5 个 0 和 3 个 1 拼出，即 "10","0001","1","0" 。
```

**示例 2:**

```
输入: strs = ["10", "0", "1"], m = 1, n = 1
输出: 2
解释: 你可以拼出 "10"，但之后就没有剩余数字了。更好的选择是拼出 "0" 和 "1" 。
```

 

**提示：**

- `1 <= strs.length <= 600`
- `1 <= strs[i].length <= 100`
- `strs[i]` 仅由 '0' 和 '1' 组成
- `1 <= m, n <= 100`

dp问题，日常写不出来

可以看出来 需要三维数组。一维是strs的第i个，一维是1个数，一维是0个数

dp[k+1] [i] [j] = dp[k] [i] j

dp[k+1] [i] [j] = max(dp[k] [i - zeros] [j - ones] +1, dp[k+1] [i] [j])

好像也不用怎么初始化

~~~java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][][] dp = new int[strs.length + 1][m + 1][n + 1];
        for (int j = 0; j < strs.length; j++) {
            int zeros = 0;
            for (int i = 0; i < strs[j].length(); i++) {
                if (strs[j].charAt(i) == '0')
                    zeros++;
            }
            int ones = strs[j].length() - zeros;
            for (int e = 0; e <= m; e++) {
                for (int r = 0; r <= n; r++) {
                    dp[j + 1][e][r] = Math.max(dp[j][e][r], dp[j + 1][e][r]);
                    if (e - zeros >= 0 && r - ones >= 0) {
                        dp[j + 1][e][r] = Math.max(dp[j + 1][e][r], dp[j][e - zeros][r - ones] + 1);
                    }
                }
            }
        }
        return dp[strs.length][m][n];
    }
}
~~~

击败21%

### Q[452用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

难度 中等

在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以y坐标并不重要，因此只要知道开始和结束的x坐标就足够了。开始坐标总是小于结束坐标。平面内最多存在104个气球。

一支弓箭可以沿着x轴从不同点完全垂直地射出。在坐标x处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

**Example:**

```
输入:
[[10,16], [2,8], [1,6], [7,12]]

输出:
2

解释:
对于该样例，我们可以在x = 6（射爆[2,8],[1,6]两个气球）和 x = 11（射爆另外两个气球）。
```

先排个序，然后搞个窗口，如果下个一个在窗口内，就是同一箭，不在就下一箭

贪心算法

~~~java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points.length == 0) return 0;
        Arrays.sort(points, new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                return a[0] - b[0];
            }
        });
        int l = points[0][0] , r = points[0][1];
        int arrows = 1;
        for(int i = 0; i < points.length;i++){
            if(l<=points[i][0]&&r>=points[i][0]){
                l = points[i][0];
                r = Math.min(points[i][1],r);
            }
            else{
                arrows++;
                l = points[i][0];
                r = points[i][1];
            }
        }
        return arrows;
    }
}
~~~

击败44%

### Q[309最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

难度 中等

给定一个整数数组，其中第 *i* 个元素代表了第 *i* 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

- 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

**示例:**

```
输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

dp问题都好有意思 就是不会解。

dp[ day ] [ status ] = money

status = 0 为已经持有股票

status = 1 为冷冻期

status = 2 为无股票期

![image-20200930112100492](https://raw.githubusercontent.com/csjue/csjue.github.io/master/_posts/images/20200930112100.png)

就是三个状态相互转换

~~~java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0) return 0;
        int[][] dp = new int[prices.length][3];
        dp[0][0] = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            dp[i][1] = dp[i - 1][0] + prices[i];
            dp[i][0] = Math.max(dp[i-1][0], dp[i - 1][2] - prices[i]);
            dp[i][2] = Math.max(dp[i - 1][1], dp[i - 1][2]);
        }
        return Math.max(dp[prices.length - 1][2], dp[prices.length - 1][1]);
    }
}
~~~

击败99%


### [LCP19秋叶收藏集](https://leetcode-cn.com/problems/UlBDOe/)

难度 中等

小扣出去秋游，途中收集了一些红叶和黄叶，他利用这些叶子初步整理了一份秋叶收藏集 `leaves`， 字符串 `leaves` 仅包含小写字符 `r` 和 `y`， 其中字符 `r` 表示一片红叶，字符 `y` 表示一片黄叶。
出于美观整齐的考虑，小扣想要将收藏集中树叶的排列调整成「红、黄、红」三部分。每部分树叶数量可以不相等，但均需大于等于 1。每次调整操作，小扣可以将一片红叶替换成黄叶或者将一片黄叶替换成红叶。请问小扣最少需要多少次调整操作才能将秋叶收藏集调整完毕。

**示例 1：**

> 输入：`leaves = "rrryyyrryyyrr"`
>
> 输出：`2`
>
> 解释：调整两次，将中间的两片红叶替换成黄叶，得到 "rrryyyyyyyyrr"

**示例 2：**

> 输入：`leaves = "ryr"`
>
> 输出：`0`
>
> 解释：已符合要求，不需要额外操作

**提示：**

- `3 <= leaves.length <= 10^5`
- `leaves` 中只包含字符 `'r'` 和字符 `'y'`

每日一题。一个dp问题

dp[i] [color]

0为左r 1为中y 2为右r,根据当前颜色写出状态方程

~~~java
class Solution {
    public int minimumOperations(String leaves) {
        if(leaves.length() == 0) return 0;
        int[][] dp = new int[leaves.length()][3];
        if(leaves.charAt(0) == 'r'){
            dp[0][0] = 0;
            dp[0][1] = 100000;
            dp[0][2] = 100001;
        }else{
            dp[0][0] = 1;
            dp[0][1] = 100000;
            dp[0][2] = 100001;
        }
        for(int i=1;i<leaves.length();i++){
            if(leaves.charAt(i)=='r'){
                dp[i][0] = dp[i-1][0];
                dp[i][1] = Math.min(dp[i-1][1]+1,dp[i-1][0]+1);
                dp[i][2] = Math.min(dp[i-1][2],dp[i-1][1]);
            }else{
                dp[i][0] = dp[i-1][0]+1;
                dp[i][1] = Math.min(dp[i-1][1],dp[i-1][0]);
                dp[i][2] = Math.min(dp[i-1][2]+1,dp[i-1][1]+1);
            }
        }
        return dp[leaves.length()-1][2];
    }
}
~~~

击败25%

### [Q771宝石与石头](https://leetcode-cn.com/problems/jewels-and-stones/)

难度 简单

 给定字符串`J` 代表石头中宝石的类型，和字符串 `S`代表你拥有的石头。 `S` 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

`J` 中的字母不重复，`J` 和 `S`中的所有字符都是字母。字母区分大小写，因此`"a"`和`"A"`是不同类型的石头。

**示例 1:**

```
输入: J = "aA", S = "aAAbbbb"
输出: 3
```

**示例 2:**

```
输入: J = "z", S = "ZZ"
输出: 0
```

**注意:**

- `S` 和 `J` 最多含有50个字母。
-  `J` 中的字符不重复。

每日一题，今天挺简单，试着用hashmap写了一下

~~~java
class Solution {
    public int numJewelsInStones(String J, String S) {
        if(S.length() == 0 || J.length() == 0) return 0;
        HashMap<Character,Integer> m = new HashMap<>();
        for(int i=0;i<S.length();i++)
            m.put(S.charAt(i),m.getOrDefault(S.charAt(i),0)+1);
        int ans = 0;
        for(int i=0;i<J.length();i++){
            ans+=m.getOrDefault(J.charAt(i),0);
        }
        return ans;
    }
}
~~~

击败47%

然后我发现这题我之前写过，原来写的是1ms，现在是2ms，我以为是因为原来是C语言，结果也是java，就是暴力写的



### Q[75颜色分类](https://leetcode-cn.com/problems/sort-colors/)

难度中等600

给定一个包含红色、白色和蓝色，一共 *n* 个元素的数组，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

**注意:**
不能使用代码库中的排序函数来解决这道题。

**示例:**

```
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

**进阶：**

- 一个直观的解决方案是使用计数排序的两趟扫描算法。
  首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
- 你能想出一个仅使用常数空间的一趟扫描算法吗？

这题并不是很难，两次遍历。第一次数出0、1个数。第二次原地修改数组

~~~java
class Solution {
    public void sortColors(int[] nums) {
        if(nums.length == 0) return;
        int zeros = 0, ones = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                zeros++;
            } else if (nums[i] == 1) {
                ones++;
            }
        }
        for (int i = 0; i < nums.length; i++) {
            if (zeros > 0) {
                if (nums[i] == 0) {
                    zeros--;
                    continue;
                }
                int pos = nextInt(0, i,nums);
                nums[pos] = nums[i];
                nums[i] = 0;
                zeros--;
            } else if (ones > 0) {
                if (nums[i] == 1) {
                    ones--;
                    continue;
                }
                int pos = nextInt(1,i, nums);
                nums[pos] = nums[i];
                nums[i] = 1;
                ones--;
            } else {
                return;
            }
        }
    }

    public int nextInt(int i, int pos,int[] x) {
        for (int j = pos; j < x.length; j++) {
            if (x[j] == i)
                return j;
        }
        return -1;
    }
}
~~~

没想到击败100%，因为毕竟还是两次遍历

### Q[208实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

难度 中等

实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。

**示例:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

**说明:**

- 你可以假设所有的输入都是由小写字母 `a-z` 构成的。
- 保证所有输入均为非空字符串。

稍微看了下答案写的，刚开始以为是用char数组存的，写成一个链表状的。但是这样字母可以随机组合成一个新单词。

应该用Trie数组来存，Trie[26] 对应的是null则无这个字母。需要用一个boolean来记录这个节点是不是最后一个来配合search操作

~~~java
class Trie {
    public Trie[] next;
    boolean isEnd;    

    /** Initialize your data structure here. */
    public Trie() {
        next = new Trie[26];
    }

    /** Inserts a word into the trie. */
    public void insert(String word) {
        Trie temp = this;
        for (int i = 0; i < word.length(); i++) {
            if (temp.next[word.charAt(i)-'a'] == null){
                temp.next[word.charAt(i)-'a'] = new Trie();
            }
            temp = temp.next[word.charAt(i)-'a'];
        }
        temp.isEnd = true;
    }

    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie temp = this;
        for (int i = 0; i < word.length(); i++) {
            if (temp.next[word.charAt(i)-'a'] == null) {
                return false;
            }
            temp = temp.next[word.charAt(i)-'a'];
        }
        if (temp.isEnd != true)
            return false;
        return true;
    }

    /**
     * Returns if there is any word in the trie that starts with the given prefix.
     */
    public boolean startsWith(String prefix) {
        Trie temp = this;
        for (int i = 0; i < prefix.length(); i++) {
            if (temp.next[prefix.charAt(i)-'a'] == null)
                return false;
            temp = temp.next[prefix.charAt(i)-'a'];
        }
        return true;
    }
}
~~~

击败62%

### [Q105从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

难度中等703

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

这题按着标答写的。利用递归函数，每次确定先序的范围 中序的范围，返回先序第一个结点

先序遍历 根 左子树 右子树

中序遍历 左子树 根 右子树

preorder_left, preorder_right, inorder_left ,inorder_right

当前结点就是先序第一个

通过inorder确定inorder_root的位置，也就可以确定size_left = inorder_root - inorder_left

root.left 的范围 preorder_left+1, preorder_left+size_left, inorder_left, inorder_root-1

root.right 的范围 preorder_left+size_left+1, preorder_right, inorder_root+1, inorder_right

当preorder_left>preorder_right : return null;

~~~java
import java.util.*;

class Solution {
    public HashMap<Integer, Integer> m;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = preorder.length;
        m = new HashMap<Integer, Integer>();
        for (int i = 0; i < inorder.length; i++) {
            m.put(inorder[i], i);
        }
        return dfs(preorder, inorder, 0, n - 1, 0, n - 1);
    }

    public TreeNode dfs(int[] preorder, int[] inorder, int preorder_left, int preorder_right, int inorder_left,
            int inorder_right) {
        if (preorder_left > preorder_right)
            return null;
        int inorder_root = m.get(preorder[preorder_left]);
        TreeNode root = new TreeNode(preorder[preorder_left]);
        int size_left = inorder_root - inorder_left;
        root.left = dfs(preorder, inorder, preorder_left + 1, preorder_left + size_left, inorder_left,
                inorder_root - 1);
        root.right = dfs(preorder, inorder, preorder_left + size_left + 1, preorder_right, inorder_root + 1,
                inorder_right);
        return root;
    }
}
~~~

 击败73%

### Q[406根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

难度中等493

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对`(h, k)`表示，其中`h`是这个人的身高，`k`是排在这个人前面且身高大于或等于`h`的人数。 编写一个算法来重建这个队列。

**注意：**
总人数少于1100人。

**示例**

```
输入:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

输出:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

这题一直没有思路，暴力也不懂怎么写。看了标答，**原来高的人是看不见矮的**..., 所以先排高的。反正矮的他们也看不到...

先排个序，然后按照数组第二个值去插入

~~~java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, new Comparator<int[]>() {
            public int compare(int[] o1, int[] o2) {
                return o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0];
            }
        });
        List<int[]> result = new ArrayList<>();
        for (int[] i : people) {
            result.add(i[1],i);
        }
        return result.toArray(new int[result.size()][2]);
    }
}
~~~

我的Java知识也影响我不太会实现这个算法...

击败95%

### [Q18四数之和](https://leetcode-cn.com/problems/4sum/)

难度 中等

给定一个包含 *n* 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 *a，**b，c* 和 *d* ，使得 *a* + *b* + *c* + *d* 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

**注意：**

答案中不可以包含重复的四元组。

**示例：**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

两层循环+双指针法。注意要剔除重复的元组。

~~~java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp;
        for (int i = 0; i < nums.length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1])
                continue;
            for (int j = i + 1; j < nums.length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1])
                    continue;
                int l = j + 1;
                int r = nums.length - 1;
                while (l < r) {
                    if (nums[i] + nums[j] + nums[l] + nums[r] == target) {
                        temp = new ArrayList<>();
                        temp.add(nums[i]);
                        temp.add(nums[j]);
                        temp.add(nums[r]);
                        temp.add(nums[l]);
                        result.add(temp);
                        while (l < r && nums[l] == nums[l + 1])
                            l++;
                        l++;
                        while (l < r && nums[r] == nums[r - 1])
                            r--;
                        r--;
                    } else if (nums[i] + nums[j] + nums[l] + nums[r] > target) {
                        r--;
                    } else {
                        l++;
                    }
                }
            }
        }
        return result;
    }
}
~~~

击败28%

### Q[148排序链表](https://leetcode-cn.com/problems/sort-list/)

难度 中等

在 *O*(*n* log *n*) 时间复杂度和常数级空间复杂度下，对链表进行排序。

**示例 1:**

```
输入: 4->2->1->3
输出: 1->2->3->4
```

**示例 2:**

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

用优先队列。

~~~java
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null) return null;
        Queue<ListNode> q = new PriorityQueue<>(new Comparator<ListNode>() {
            public int compare(ListNode a, ListNode b) {
                return a.val - b.val;
            }
        });
        ListNode temp = head;
        while(temp != null) {
            q.add(temp);
            temp = temp.next;
        }
        ListNode ans = q.poll();
        temp = ans;
        while(q.peek() != null){
            temp.next = q.poll();
            temp = temp.next;
        }
        temp.next = null;
        return ans;
    }
}
~~~

击败17%

### Q[647回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

难度 中等

给定一个字符你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

 

**示例 1：**

```
输入："abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```

**示例 2：**

```
输入："aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```

 

**提示：**

- 输入的字符串长度不会超过 1000 。

不会dp，直接暴力穷举，每次穷举中心点，逐渐增大长度。（奇数长度，偶数长度）

~~~java
class Solution {
    public int countSubstrings(String s) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = 0; i + j < s.length() && i - j >= 0; j++) {
                if (s.charAt(i + j) == s.charAt(i - j))
                    count++;
                else
                    break;
            }
        }
        for (int i = 0; i < s.length() - 1; i++) {
            if (s.charAt(i) != s.charAt(i + 1))
                continue;
            for (int j = 0; i - j >= 0 && i + 1 + j < s.length(); j++) {
                if (s.charAt(i - j) == s.charAt(i + j + 1))
                    count++;
                else
                    break;
            }
        }
        return count;
    }
}
~~~

击败57%

### [Q538把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

难度 中等

给出二叉 **搜索** 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 `node` 的新值等于原树中大于或等于 `node.val` 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

- 节点的左子树仅包含键 **小于** 节点键的节点。
- 节点的右子树仅包含键 **大于** 节点键的节点。
- 左右子树也必须是二叉搜索树。

**注意：**本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同

 

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)**

```
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**示例 2：**

```
输入：root = [0,null,1]
输出：[1,null,1]
```

**示例 3：**

```
输入：root = [1,0,2]
输出：[3,3,2]
```

**示例 4：**

```
输入：root = [3,2,4,1]
输出：[7,9,4,10]
```

 

**提示：**

- 树中的节点数介于 `0` 和 `104` 之间。
- 每个节点的值介于 `-104` 和 `104` 之间。
- 树中的所有值 **互不相同** 。
- 给定的树为二叉搜索树。

这题就是dfs，注意左右子树需要区别对待，先计算右子树和，再把和传给左子树

~~~java
class Solution {
    public TreeNode convertBST(TreeNode root) {
        dfs(root,0);
        return root;
    }
    public int dfs(TreeNode root,int x){
        if(root == null) return 0;
        int max = dfs(root.right,x);
        if(max==0)
            root.val += x;
        else
            root.val += max;
        if(root.left != null)
            return dfs(root.left,root.val);
        return root.val;
    }
}
~~~

击败98%

### [Q337打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

难度 中等

在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

**示例 1:**

```
输入: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
```

**示例 2:**

```
输入: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.
```

又是动态规划，又不会写

map f(node)代表取出这个点 等于左右两点不取与自身的和

map g(node)代表不取出 等于左右两点最大之和

~~~java
class Solution {
    Map<TreeNode,Integer> f = new HashMap<>();
    Map<TreeNode,Integer> g = new HashMap<>();
    public int rob(TreeNode root) {
        f.put(null,0);
        g.put(null,0);
        dfs(root);
        return Math.max(f.get(root),g.get(root));
    }
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        dfs(root.right);
        f.put(root,g.get(root.left)+g.get(root.right)+root.val);
        g.put(root,Math.max(f.get(root.left),g.get(root.left))+Math.max(f.get(root.right),g.get(root.right))); 
    }
}
~~~

击败43%

### Q[279完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

难度 中等

给定正整数 *n*，找到若干个完全平方数（比如 `1, 4, 9, 16, ...`）使得它们的和等于 *n*。你需要让组成和的完全平方数的个数最少。

**示例 1:**

```
输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
```

**示例 2:**

```
输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```

还是dp问题，从1一直找到n

我刚开始手推的时候，只写到10，发现只要检查sqrt(n)那个就可以了。但是样例12，总是过不了。sqrt(12) = 3

12=9+1+1+1其实应该是12 = 4+4+4，所以得检索 1~sqrt(n)*sqrt(n)，复杂度n sqrt(n)

~~~java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        for (int i = 1; i * i <= n; i++) {
            dp[i * i] = 1;
        }
        for (int i = 1; i <= n; i++) {
            int r = (int) Math.sqrt(i);
            if (r * r != i) {
                dp[i] = 4;
                for (int j = 1; j <= r; j++) {
                    dp[i] = Math.min(1 + dp[i - j * j],dp[i]);
                }
            }
        }
        return dp[n];
    }
}
~~~

击败77%

### Q[160相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

难度简单835

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点 c1 开始相交。

 

**示例 1：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

 

**示例 2：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

 

**示例 3：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```

 

**注意：**

- 如果两个链表没有交点，返回 `null`.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。

力扣不决问哈希图

直接用HashMap

~~~java
import java.util.*;
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashMap<ListNode,Integer> m = new HashMap<>();
        while(headA!=null){
            m.put(headA,1);
            headA = headA.next;
        }
        while(headB!=null){
            if(m.getOrDefault(headB,0)==1)
                return headB;
            headB = headB.next;
        }
        return null;
    }
}
~~~

击败8%.... 最快也就O(n) 我这也不过是O(2n)

双指针法

~~~java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
         if(headA == null || headB == null) return null;
        ListNode A = headA,B = headB;
        while(A!=B){
            A = A==null?headB:A.next;
            B = B==null?headA:B.next;
        }
        return A;
    }
}
~~~

击败99%，感觉差别不大吧。

### Q[344反转字符串](https://leetcode-cn.com/problems/reverse-string/)

难度 简单

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `char[]` 的形式给出。

不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符。

 

**示例 1：**

```
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

**示例 2：**

```
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

直接上代码

~~~java
class Solution {
    public void reverseString(char[] s) {
        char ch;
        int n = s.length;
        if(n == 0) return;
        for (int i = 0; i < n / 2; i++) {
            ch = s[i];
            s[i] = s[n - i - 1];
            s[n - i - 1] = ch;
        }
    }
}
~~~

击败99%

### Q[437路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

难度 中等

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

**示例：**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```

刚开始看了题解，用一个arraylist记录路径，每次更新一遍

~~~java
import java.util.*;
class Solution {
    int count = 0;
    public int pathSum(TreeNode root, int sum) {
        ArrayList<Integer> list = new ArrayList();
        dfs(root,list,sum);
        return count;
    }
    public void dfs(TreeNode root,ArrayList<Integer> list,int sum) {
        if(root == null) return;
        for(int i = 0; i < list.size(); i++){
            list.set(i,list.get(i)+root.val);
            if(list.get(i) == sum)
                count++;
        }
        list.add(root.val);
        if(root.val == sum)
            count++;
        dfs(root.left,new ArrayList<Integer>(list),sum);
        dfs(root.right,new ArrayList<Integer>(list), sum);
    }
}
~~~

击败9%，感觉每次生成一个arralist浪费了很多时间

参照一个据说击败99%的修改了一下。它搞的比较巧妙，用一个静态数组就搞定了。每次只记录当前节点的值，每次重新遍历整个数组，计算temp值。用形参p，来记录当前遍历的末尾

~~~java
import java.util.*;
class Solution {
    int count = 0;
    public int pathSum(TreeNode root, int sum) {
        int[] list = new int[1001];
        dfs(root,list,sum,0);
        return count;
    }
    public void dfs(TreeNode root,int[] list,int sum,int p) {
        if(root == null) return;
        if(sum == root.val) count++;
        int temp = root.val;
        for(int i = p-1; i >=0 ; i--){
            temp += list[i];
            if(temp == sum)
                count++;
        }
        list[p] = root.val;
        dfs(root.left,list,sum,p+1);
        dfs(root.right,list, sum,p+1);
    }
}
~~~

击败81%

### Q[394字符串解码](https://leetcode-cn.com/problems/decode-string/)

难度 中等

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 *encoded_string* 正好重复 *k* 次。注意 *k* 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 *k* ，例如不会出现像 `3a` 或 `2[4]` 的输入。

 

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

从头到尾遍历字符串。遇到字母直接加上。遇到数字 进入递归处理。

~~~java
class Solution {
    int i = 0;
    public String decodeString(String s) {
        if(s.length() == 0) return "";
        StringBuilder ans = new StringBuilder();
        int times = 0;
        for (; i < s.length(); i++) {
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                times = times * 10 + s.charAt(i) - '0';
            } else if (s.charAt(i) == '[') {
                StringBuilder temp = fun(s, i + 1);
                for (int j = 0; j < times; j++) {
                    ans.append(temp);
                }
                while (s.charAt(i) != ']') {
                    i++;
                }
                times = 0;
            } else {
                ans.append(s.charAt(i));
            }
        }
        return ans.toString();
    }

    public StringBuilder fun(String s, int pos) {
        StringBuilder ans = new StringBuilder();
        int times = 0;
        for (i = pos; i < s.length(); i++) {
            if(s.charAt(i) == ']')
                break;
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                times = times * 10 + s.charAt(i) - '0';
            } else if (s.charAt(i) == '[') {
                StringBuilder temp = fun(s, i + 1);
                for (int j = 0; j < times; j++) {
                    ans.append(temp);
                }
                while (s.charAt(i) != ']') {
                    i++;
                }
                times = 0;
            } else {
                ans.append(s.charAt(i));
            }
        }
        return ans;
    }
}
~~~

击败100%

### Q[142环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

难度中等642

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。

**说明：**不允许修改给定的链表。

 

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

**示例 2：**

```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

**示例 3：**

```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

 

**进阶：**
你是否可以不用额外空间解决此题？

快慢指针。之前做过一次类似的。假设头到环距离为k，从环开始到相遇距离为t，环长l。慢指针走过的路k+l，快指针走过2（k+l） = k + t +l；l = k+t。此时把快指针挪到头结点，两个指针每次只走一。（快指针离头结点距离 k，慢指针离头结点距离l-t = k）相遇点即为环开始

~~~java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null) return null;
        ListNode quick = head, slow = head;
        slow = slow.next;
        quick = quick.next;
        if(quick == null) return null;
        quick = quick.next;
        while(quick!=null&&quick != slow){
            quick = quick.next;
            if(quick == null) return null;
            quick = quick.next;
            slow = slow.next;
        }
        if(quick == null)
            return null;
        quick = head;
        while(quick != slow){
            quick = quick.next;
            slow = slow.next;
        }
        return quick;
    }
}
~~~

不知道为啥只击败38%

### Q[543二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

难度简单496

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

 

**示例 :**
给定二叉树

```
          1
         / \
        2   3
       / \     
      4   5    
```

返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

 

**注意：**两结点之间的路径长度是以它们之间边的数目表示。

没啥难度，每次合并下左右，与最大值去比较。

~~~java
class Solution {
    int maxlength = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        rootlength(root);
        return maxlength;
    }
    public int rootlength(TreeNode root) {
        if(root == null) return 0;
        int left = rootlength(root.left);
        int right = rootlength(root.right);
        maxlength = maxlength>right+left?maxlength:right+left;
        return Math.max(left,right)+1;
    }
}
~~~

击败18%，不知道为啥这么低

### Q[621任务调度器](https://leetcode-cn.com/problems/task-scheduler/)

难度 中等

给定一个用字符数组表示的 CPU 需要执行的任务列表。其中包含使用大写的 A - Z 字母表示的26 种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。CPU 在任何一个单位时间内都可以执行一个任务，或者在待命状态。

然而，两个**相同种类**的任务之间必须有长度为 **n** 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的**最短时间**。

 

**示例 ：**

```
输入：tasks = ["A","A","A","B","B","B"], n = 2
输出：8
解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B.
     在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。 
```

 

**提示：**

1. 任务的总个数为 `[1, 10000]`。
2. `n` 的取值范围为 `[0, 100]`。

这道题真的还是有点复杂。有点类似cpu的流水线。以n+1为一个单位来分配任务。但是这里会遇到一个问题，在最后一个流水线中，搞不清到底应该是这个n+1干完活，还是实际不需要n+1，还是还需要一个周期。

这里就要贪心算法。我们先安排任务量大的。每次算完都排序。原来任务量排序为k，在下一周期，排名不会小于k（k前面都比它大，所有同时-1），这样就可以不管时间间隔安排。

~~~java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] t = new int[26];
        int all = 0;
        int time = 0;
        int count = 0;
        int pos = 0;
        for (char ch : tasks) {
            if(t[ch-'A'] == 0)
                pos++;
            t[ch - 'A']++;
            all++;
        }
        Arrays.sort(t);
        int high = Math.min(n + 1, 26);
        while (all > 0) {
            count = n+1;
            for (int i = 25; i > 25-high; i--) {
                if (all == 0) {
                    return time;
                }
                if (t[i] > 0) {
                    t[i]--;
                    all--;
                    time++;
                    count--;
                }
            }
            time += count;
            Arrays.sort(t);
        }
        return time;
    }
}
~~~

击败54%



### Q[146LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

难度 中等

运用你所掌握的数据结构，设计和实现一个 [LRU (最近最少使用) 缓存机制](https://baike.baidu.com/item/LRU)。它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。

获取数据 `get(key)` - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 `put(key, value)` - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

 

**进阶:**

你是否可以在 **O(1)** 时间复杂度内完成这两种操作？

 

**示例:**

```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得关键字 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得关键字 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```

一下子就想到了hashmap 和 prioityqueue，hashmap用于存取，prioityqueue用于删除。

但是问题来了，每次times++，prioityqueue并不会重新调整。 **prioityqueue只会在添加点 删除点时调整** 。这个问题困扰了我很久

~~~java
class LRUCache {
    int size;
    int num;
    Queue<Point> q;
    Map<Integer, Point> m;

    public LRUCache(int capacity) {
        size = capacity;
        num = 0;
        q = new PriorityQueue<>();
        m = new HashMap<>();
    }

    public int get(int key) {
        Point p = m.get(key);
        if (p == null) return -1;
        Queue<Point> tmp = new PriorityQueue<>();
        for (Point t : q) {
            if (t.equals(p)) {
                t.times = 0;
            } else {
                t.times++;
            }
            tmp.add(t);
        }
        q = tmp;
        return p.value;
    }

    public void put(int key, int value) {
        if(m.get(key) != null){
            m.get(key).value = value;
            get(key);
            return;
        }
        if (num == size) {
            Point p = q.remove();
            m.remove(p.key);
            num--;
        }
        num++;
        Point p = new Point();
        p.key = key;
        p.value = value;
        p.times = 0;
        m.put(key, p);
        q.add(p);
    }
}

class Point implements Comparable<Point> {
    public int key;
    public int value;
    public int times;

    public Point() {
    }

    public Point(Point p) {
        key = p.key;
        value = p.value;
        times = p.times;
    }

    @Override
    public int compareTo(Point o) {
        return o.times - times;
    }
}
~~~

击败5%

学习了java的linkedhashmap

大概就是有序图，可以自己定制一些操作。比如：按插入顺序（默认）、按访问顺序



构造函数

public LinkedHashMap(int initialCapacity,float loadFactor,boolean accessOrder)
构造一个空的 LinkedHashMap实例，具有指定的初始容量，负载因子和排序模式。 

参数 
initialCapacity - 初始容量 
loadFactor - 负载因子
accessOrder - 订购模式 - true的访问顺序， false的插入顺序 

方法（利用继承 重写）

protected boolean     removeEldestEntry(Map.Entry<K,V> eldest)  如果此地图应删除其最老的条目，则返回 true 。

~~~java
class LRUCache extends LinkedHashMap{
    int capacity;

    public LRUCache(int capacity) {
        super(capacity,0.75f,true);//0.75f是个加载因子与泊松有关，true是按照访问顺序
        this.capacity = capacity;
    }

    public int get(int key) {
        return (int)super.getOrDefault(key,-1);
    }

    public void put(int key, int value) {
        super.put(key,value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry eldest) {
        return size()>capacity;
    }
}
~~~

击败95%

### Q[416分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

难度 中等

给定一个**只包含正整数**的**非空**数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**注意:**

1. 每个数组中的元素不会超过 100
2. 数组的大小不会超过 200

**示例 1:**

```
输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
```

 

**示例 2:**

```
输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集.
```

刚开始用回溯法，挂在了时间超时

看题解用dp，dp[nums.length] [target] 看到这基本式子就会了

之间把target sum搞混，多交了好几发

~~~java
class Solution {
    public boolean canPartition(int[] nums) {
        if (nums.length == 1) return false;
        int sum = 0;
        for (int i : nums)
            sum += i;
        if (sum % 2 == 1)
            return false;
        int target = sum / 2;
        boolean[][] dp = new boolean[nums.length][target + 1];
        if (nums[0] > target)
            return false;
        dp[0][nums[0]] = true;
        dp[0][0] = true;
        for (int i = 1; i < nums.length; i++) {
            dp[i][0] = true;
            if (nums[i] > target)
                return false;
            dp[i][nums[i]] = true;
            for (int j = 0; j < target + 1; j++) {
                if (dp[i - 1][j]) {
                    dp[i][j] = true;
                    if (j + nums[i] <= target)
                        dp[i][j + nums[i]] = true;
                }
            }
            if (dp[i][target])
                return true;
        }
        return dp[nums.length - 1][target];
    }
}
~~~

击败69%

### [Q322零钱兑换](https://leetcode-cn.com/problems/coin-change/)

难度中等859

给定不同面额的硬币 `coins` 和一个总金额 `amount`。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

你可以认为每种硬币的数量是无限的。

 

**示例 1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**示例 2：**

```
输入：coins = [2], amount = 3
输出：-1
```

**示例 3：**

```
输入：coins = [1], amount = 0
输出：0
```

**示例 4：**

```
输入：coins = [1], amount = 1
输出：1
```

**示例 5：**

```
输入：coins = [1], amount = 2
输出：2
```

 

**提示：**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

照着上一题的思路写的。dp[coins.length] [amount]

~~~java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount == 0) return 0;
        if (coins.length == 0) return -1;
        int[][] dp = new int[coins.length][amount + 1];

        for (int i = 0; i < coins.length; i++) {
            int c = coins[i];
            int plus = coins[i];
            int num = 1;
            if (i != 0)
                for (int j = 0; j <= amount; j++) {
                    dp[i][j] = dp[i - 1][j];
                }
            while (c <= amount) {
                if (dp[i][c] == 0)
                    dp[i][c] = num;
                else
                    dp[i][c] = Math.min(num, dp[i][c]);
                if (i != 0)
                    for (int j = 0; j <= amount; j++) {
                        if (dp[i - 1][j] != 0) {
                            if (dp[i][j] == 0) {
                                dp[i][j] = dp[i - 1][j];
                            } else {
                                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j]);
                            }
                            if (j + c <= amount && dp[i][j + c] == 0) {
                                dp[i][j + c] = dp[i - 1][j] + num;
                            } else if (j + c <= amount) {
                                dp[i][j + c] = Math.min(dp[i - 1][j] + num, dp[i][j + c]);
                            }
                        }
                    }
                c += plus;
                num++;
            }

        }
        if (dp[coins.length - 1][amount] == 0)
            return -1;
        return dp[coins.length - 1][amount];
    }
}
~~~

击败5%

### Q[1367二叉树中的列表](https://leetcode-cn.com/problems/linked-list-in-binary-tree/)

难度 中等

给你一棵以 `root` 为根的二叉树和一个 `head` 为第一个节点的链表。

如果在二叉树中，存在一条一直向下的路径，且每个点的数值恰好一一对应以 `head` 为首的链表中每个节点的值，那么请你返回 `True` ，否则返回 `False` 。

一直向下的路径的意思是：从树中某个节点开始，一直连续向下的路径。

 

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_1_1720.png)**

```
输入：head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
解释：树中蓝色的节点构成了与链表对应的子路径。
```

**示例 2：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_2_1720.png)**

```
输入：head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
```

**示例 3：**

```
输入：head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：false
解释：二叉树中不存在一一对应链表的路径。
```

 

**提示：**

- 二叉树和链表中的每个节点的值都满足 `1 <= node.val <= 100` 。
- 链表包含的节点数目在 `1` 到 `100` 之间。
- 二叉树包含的节点数目在 `1` 到 `2500` 之间。

这题不就dfs

~~~java
class Solution {
    public boolean isSubPath(ListNode head, TreeNode root) {
        return dfs(head,head,root);
    }
    public boolean dfs(ListNode head,ListNode h,TreeNode t){
        if(h == null) return true;
        if(t == null) return false;
        if(h.val == t.val){
            boolean l = dfs(head,h.next,t.left);
            boolean r = dfs(head,h.next,t.right);
            if(l||r) return true;
        }
        boolean l = dfs(head,head,t.left);
        boolean r = dfs(head,head,t.right);
        if(l||r) return true;
        return false;
    }
}
~~~

这个超时了，然后发现 几个月前我也因为超时挂了

按照题解修改

~~~java
class Solution {
    public boolean isSubPath(ListNode head, TreeNode root) {
        if(root == null) return false;
        return dfs(head,root)||isSubPath(head,root.left)||isSubPath(head,root.right);
    }
    public boolean dfs(ListNode h,TreeNode t){
        if(h == null) return true;
        if(t == null) return false;
        if(h.val != t.val) return false;
        return dfs(h.next,t.left)||dfs(h.next,t.right);
    }
}
~~~

其实不太懂区别在哪，第二个还击败100%

### Q[530二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

难度 简单

给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。

 

**示例：**

```
输入：

   1
    \
     3
    /
   2

输出：
1

解释：
最小绝对差为 1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。
```

 

**提示：**

- 树中至少有 2 个节点。
- 本题与 783 https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/ 相同

中序遍历可以获得排序好的队列。所以在这一题，我们可以直接中序遍历，保存前一个结点的值就可以了

~~~java
class Solution {
    int pre = Integer.MAX_VALUE;
    int ans = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        inorder(root);
        return ans;
    }
    public void inorder(TreeNode t){
        if(t == null) return;
        inorder(t.left);
        ans = Math.min(ans, Math.abs(pre - t.val));
        pre = t.val;
        inorder(t.right);
    }
}
~~~

击败82%

### Q[200岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

难度 中等

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

 

**示例 1:**

```
输入:
[
['1','1','1','1','0'],
['1','1','0','1','0'],
['1','1','0','0','0'],
['0','0','0','0','0']
]
输出: 1
```

**示例 2:**

```
输入:
[
['1','1','0','0','0'],
['1','1','0','0','0'],
['0','0','1','0','0'],
['0','0','0','1','1']
]
输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```

dfs 遇到1，就把1抹成0

~~~java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for(int i = 0;i<grid.length;i++){
            for(int j = 0; j< grid[0].length;j++){
                if(grid[i][j]=='1') {
                    dfs(grid, i, j);
                    count ++;
                }
            }
        }
        return count;
    }
    public void dfs(char[][] grid, int x, int y){
        grid[x][y] = '0';
        if(x+1<grid.length&&grid[x+1][y] == '1'){
            dfs(grid, x+1, y);
        }
        if(x-1>=0&&grid[x-1][y] == '1'){
            dfs(grid, x-1, y);
        }
        if(y+1<grid[0].length&&grid[x][y+1] == '1'){
            dfs(grid, x, y+1);
        }
        if(y-1>=0&&grid[x][y-1] == '1'){
            dfs(grid, x, y-1);
        }
    }
}
~~~

击败97%

### Q[438找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

难度 中等

给定一个字符串 **s** 和一个非空字符串 **p**，找到 **s** 中所有是 **p** 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 **s** 和 **p** 的长度都不超过 20100。

**说明：**

- 字母异位词指字母相同，但排列不同的字符串。
- 不考虑答案输出的顺序。

**示例 1:**

```
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```

 **示例 2:**

```
输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。
```

 

滑窗，当right 添加的东西不需要，就移left

~~~java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        char[] schar = s.toCharArray();
        char[] pchar = p.toCharArray();

        int[] window = new int[26];
        int[] need = new int[26];

        for(char ch : pchar)
            need[ch-'a']++;

        List<Integer> ans = new ArrayList<>();

        int left = 0, right = 0;
        while(right<schar.length){
            int ch = schar[right] - 'a';
            window[ch]++;
            right++;
            while(left<right&&window[ch]>need[ch]){
                int c = schar[left]-'a';
                window[c]--;
                left++;
            }
            if(right-left == pchar.length)
                ans.add(left);
        }
        return ans;
    }
}
~~~

击败99%

### Q[494目标和](https://leetcode-cn.com/problems/target-sum/)

难度 中等

给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 `+` 和 `-`。对于数组中的任意一个整数，你都可以从 `+` 或 `-`中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

 

**示例：**

```
输入：nums: [1, 1, 1, 1, 1], S: 3
输出：5
解释：

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
```

dp问题，先求数组和sum，建立dp数组 dp[nums.length] [2*sum + 1] 存取方法数

~~~java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for (int i : nums)
            sum += i;
        if(Math.abs(S)>sum)
            return 0;
        int[][] dp = new int[nums.length][2 * sum + 1];
        dp[0][nums[0] + sum] ++;
        dp[0][-nums[0] + sum] ++;
        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < 2 * sum + 1; j++) {
                if (dp[i - 1][j] != 0) {
                    dp[i][j - nums[i]]+=dp[i-1][j];
                    dp[i][j + nums[i]]+=dp[i-1][j];
                }
            }
        }
        return dp[nums.length - 1][S + sum];
    }
}
~~~

击败74%



### Q[24两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

难度 中等

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

 

**示例:**

```
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

没啥难度

~~~java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode l1 = null, l2 = null, l3 = null, l4 = head;
        while (l4 != null) {
            for (int i = 0; i < 2; i++) {
                if(l4 == null)
                    return head;
                l1 = l2;
                l2 = l3;
                l3 = l4;
                l4 = l4.next;
                
            }
            if(l2!=null&&l3!=null) {
                l2.next = l4;
                l3.next = l2;
                if (l1 != null)
                    l1.next = l3;
                else
                    head = l3;
                ListNode temp = l2;
                l2 = l3;
                l3 = temp;
            }
        }
        return head;
    }
}
~~~

9个月前仅用了一发。今天三发。击败100%



### Q[234回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

难度 简单

请判断一个链表是否为回文链表。

**示例 1:**

```
输入: 1->2
输出: false
```

**示例 2:**

```
输入: 1->2->2->1
输出: true
```

**进阶：**
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

肯定要用O（n）时间 O（1）来解决。要不然也太没意思

先找到中间结点。把后半端翻转过来。然后开始比较。思路很简单，就是得保证三个模块没有bug

~~~java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null) return true;
        if(head.next == null) return true;
        ListNode last = head;
        ListNode mid = head;

        while (last.next != null) {
            last = last.next;
            if (last.next != null) {

                mid = mid.next;
                last = last.next;
            }
        }
        ListNode l1 = null, l2 = mid.next, l3 = l2.next;
        while (l2 != null) {
            l2.next = l1;
            l1 = l2;
            l2 = l3;
            if (l3 != null)
                l3 = l3.next;
        }
        while(head!=null&&last!=null&&head!=last){
            if(head.val!=last.val)
                return false;
            head = head.next;
            last = last.next;
        }
        return true;
    }
}
~~~

击败99%

### Q[1002查找常用字符](https://leetcode-cn.com/problems/find-common-characters/)

难度 简单

给定仅有小写字母组成的字符串数组 `A`，返回列表中的每个字符串中都显示的全部字符（**包括重复字符**）组成的列表。例如，如果一个字符在每个字符串中出现 3 次，但不是 4 次，则需要在最终答案中包含该字符 3 次。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：["bella","label","roller"]
输出：["e","l","l"]
```

**示例 2：**

```
输入：["cool","lock","cook"]
输出：["c","o"]
```

 

**提示：**

1. `1 <= A.length <= 100`
2. `1 <= A[i].length <= 100`
3. `A[i][j]` 是小写字母

建立一个数组min、temp，temp每次统计字符，与min去比较

~~~java
class Solution {
    public List<String> commonChars(String[] A) {
        int[] min = new int[26];
        int[] temp;
        for (int i = 0; i < A.length; i++) {
            String s = A[i];
            temp = new int[26];
            for (int j = 0; j < s.length(); j++) {
                int pos = s.charAt(j) - 'a';
                temp[pos]++;
            }
            if (i == 0)
                for (int j = 0; j < 26; j++) {
                    min[j] = temp[j];
                }
            else
                for (int j = 0; j < 26; j++) {
                    min[j] = Math.min(temp[j], min[j]);
                }
        }
        List<String> l = new ArrayList<>();
        for (int i = 0; i < 26; i++) {
            while (min[i] != 0) {
                l.add("" + (char) (i + 'a'));
                min[i]--;
            }
        }
        return l;
    }
}
~~~

击败43%



### Q[79单词搜索](https://leetcode-cn.com/problems/word-search/)

难度 中等

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

**示例:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false
```

 

**提示：**

- `board` 和 `word` 中只包含大写和小写英文字母。
- `1 <= board.length <= 200`
- `1 <= board[i].length <= 200`
- `1 <= word.length <= 10^3`

简单的dfs，要记住标记走过的路

~~~java
class Solution {
    boolean[][] flag;

    public boolean exist(char[][] board, String word) {
        flag = new boolean[board.length][board[0].length];
        for (int i = 0; i < board.length; i++)
            for (int j = 0; j < board[0].length; j++)
                if (board[i][j] == word.charAt(0)) {
                    flag[i][j] = true;
                    if (dfs(board, word, 1, i, j))
                        return true;
                    flag[i][j] = false;
                }
        return false;

    }

    public boolean dfs(char[][] board, String word, int pos, int x, int y) {
        if (pos == word.length()) return true;
        if (x - 1 >= 0 && !flag[x - 1][y] &&board[x - 1][y] == word.charAt(pos)) {
            flag[x-1][y] = true;
            if(dfs(board, word, pos + 1, x - 1, y))
                return true;
            flag[x-1][y] = false;
        }
        if (x +1 < board.length && !flag[x + 1][y] &&board[x +1][y] == word.charAt(pos)) {
            flag[x+1][y] = true;
            if(dfs(board, word, pos + 1, x + 1, y))
                return true;
            flag[x+1][y] =false;
        }
        if (y - 1 >= 0 && !flag[x][y - 1] &&board[x][y-1] == word.charAt(pos)) {
            flag[x][y-1] = true;
            if(dfs(board, word, pos + 1, x, y-1))
                return true;
            flag[x][y-1] = false;
        }
        if (y +1 <board[0].length && !flag[x][y + 1] &&board[x][y+1] == word.charAt(pos)) {
            flag[x][y+1] = true;
            if(dfs(board, word, pos + 1, x, y+1))
                return true;
            flag[x][y+1] = false;
        }
        return false;
    }
}
~~~

击败97%



### Q[139单词拆分](https://leetcode-cn.com/problems/word-break/)

难度 中等

给定一个**非空**字符串 *s* 和一个包含**非空**单词的列表 *wordDict*，判定 *s* 是否可以被空格拆分为一个或多个在字典中出现的单词。

**说明：**

- 拆分时可以重复使用字典中的单词。
- 你可以假设字典中没有重复的单词。

**示例 1：**

```
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
```

**示例 2：**

```
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
```

**示例 3：**

```
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

dp问题，dp[i] 表示到index:i-1都可以分隔开。dp[i] = dp[j] && set.contains(s.substring(j,i))。dp[0] = true

~~~java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> set = new HashSet<>();
        boolean[] dp = new boolean[s.length()+1];
        dp[0] = true;
        for(String s1: wordDict)
            set.add(s1);
        for(int i = 1; i <= s.length(); i++){
            for(int j = 0; j < i; j++){
                if(dp[j]&&set.contains(s.substring(j,i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
~~~

### Q[116填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

难度中等299

给定一个**完美二叉树**，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`。

 

**示例：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/15/116_sample.png)

```
输入：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":{"$id":"6","left":null,"next":null,"right":null,"val":6},"next":null,"right":{"$id":"7","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}

输出：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":{"$id":"6","left":null,"next":null,"right":null,"val":7},"right":null,"val":6},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"7","left":{"$ref":"5"},"next":null,"right":{"$ref":"6"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"7"},"val":1}

解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。
```

 

**提示：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

层次遍历，用队列

~~~java
class Solution {
    public Node connect(Node root) {
        if(root == null) return null;
        Queue<Node> q = new ArrayDeque<>();
        Queue<Integer> level = new ArrayDeque<>();
        Node n1 = root;
        q.add(root);
        level.add(1);
        int l = 0;
        Node preNode = root;
        Node temp;
        while (q.size() != 0) {
            int l1 = level.poll();
            temp = q.poll();
            if (l == l1)
                preNode.next = temp;
            l = l1;
            preNode = temp;
            if (temp.left != null) {
                q.add(temp.left);
                level.add(l + 1);
            }
            if (temp.right != null) {
                q.add(temp.right);
                level.add(l + 1);
            }
        }
        return root;
    }
}
~~~

击败5%

### Q[152乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

难度 中等

给你一个整数数组 `nums` ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

 

**示例 1:**

```
输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**示例 2:**

```
输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

初看这题 感觉挺简单的。维护一个前值就够了。遍历数组，每次取最大

问题就在负数。如果有偶数个负数，这种方法就会失效。

还得维护一个最小值，就是这个最大值，最小值的初值也得想一想

~~~java
class Solution {
    public int maxProduct(int[] nums) {
        if(nums.length == 0) return 0;
        int[] min = new int[nums.length];
        int[] max = new int[nums.length];
        min[0] = nums[0];
        max[0] = nums[0];
        int ans = max[0];
        for(int i = 1; i < nums.length; i++){
            min[i] = nums[i];
            max[i] = nums[i];
            min[i] = Math.min(min[i-1]*nums[i],Math.min(nums[i],max[i-1]*nums[i]));
            max[i] = Math.max(min[i-1]*nums[i],Math.max(nums[i],max[i-1]*nums[i]));
            ans = Math.max(max[i],ans);
        }
        return ans;
    }
}
~~~

六发。击败41%

### Q[240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

难度 中等

编写一个高效的算法来搜索 *m* x *n* 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

**示例:**

现有矩阵 matrix 如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定 target = `5`，返回 `true`。

给定 target = `20`，返回 `false`。

![](https://pic.leetcode-cn.com/1602309177-SsaQGG-image.png)

看了这个图就会了

我原来用dfs，超时

~~~java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix.length == 0) return false;
        int x = 0;
        int y =matrix[0].length-1;
        while(x<matrix.length&&y>=0&&matrix[x][y]!=target){
            if(matrix[x][y]>target)
                y--;
            else
                x++;
        }
        if(x<matrix.length&&y>=0&&matrix[x][y] == target)
            return true;
        return false;
    }
}
~~~

击败99%

### Q[221最大正方形](https://leetcode-cn.com/problems/maximal-square/)

难度 中等

在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

**示例:**

```
输入: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```

dp问题自己想很复杂，抄答案很简单

dp[ i ] [j] = Math.min(dp[i - 1] [ j - 1], Math.min(dp[i - 1] [j], dp[i] [j - 1])) + 1;

~~~java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix.length == 0) return 0;
        int max = 0;
        int[][] dp = new int[matrix.length][matrix[0].length];
        for (int i = 0; i < matrix.length; i++) {
            if (matrix[i][0] == '1') {
                dp[i][0] = 1;
                max = 1;
            }
        }
        for (int j = 0; j < matrix[0].length; j++) {
            if (matrix[0][j] == '1') {
                dp[0][j] = 1;
                max = 1;
            }
        }
        for (int i = 1; i < matrix.length; i++) {
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][j] == '1') {
                    dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
                    max = Math.max(max, dp[i][j]);
                }
            }
        }
        return max*max;
    }
}
~~~

击败16%

### Q[1277统计全为 1 的正方形子矩阵](https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones/)

难度中等109

给你一个 `m * n` 的矩阵，矩阵中的元素不是 `0` 就是 `1`，请你统计并返回其中完全由 `1` 组成的 **正方形** 子矩阵的个数。

 

**示例 1：**

```
输入：matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
输出：15
解释： 
边长为 1 的正方形有 10 个。
边长为 2 的正方形有 4 个。
边长为 3 的正方形有 1 个。
正方形的总数 = 10 + 4 + 1 = 15.
```

**示例 2：**

```
输入：matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
输出：7
解释：
边长为 1 的正方形有 6 个。 
边长为 2 的正方形有 1 个。
正方形的总数 = 6 + 1 = 7.
```

 

**提示：**

- `1 <= arr.length <= 300`
- `1 <= arr[0].length <= 300`
- `0 <= arr[i][j] <= 1`

上题稍微改下又是这道题

~~~java
class Solution {
    public int countSquares(int[][] matrix) {
        if (matrix.length == 0) return 0;
        int ans = 0;
        int[][] dp = new int[matrix.length][matrix[0].length];
        for (int i = 0; i < matrix.length; i++) {
            dp[i][0] = matrix[i][0];
            ans += dp[i][0];
        }
        for (int j = 1; j < matrix[0].length; j++) {
            dp[0][j] = matrix[0][j];
            ans += dp[0][j];
        }
        for (int i = 1; i < matrix.length; i++) {
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][j] == 1) {
                    dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
                    ans += dp[i][j];
                }
            }
        }
        return ans;
    }
}
~~~

### Q[560和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

难度 中等

给定一个整数数组和一个整数 **k，**你需要找到该数组中和为 **k** 的连续的子数组的个数。

**示例 1 :**

```
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```

**说明 :**

1. 数组的长度为 [1, 20,000]。
2. 数组中元素的范围是 [-1000, 1000] ，且整数 **k** 的范围是 [-1e7, 1e7]。

维护一个前缀和map，前缀和-前缀和 = k。

~~~java
class Solution {
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        int count = 0;
        map.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (map.containsKey(sum-k)) {
                count += map.get(sum-k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
~~~

击败63%

### Q[52N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/)

难度困难154

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/8-queens.png)

上图为 8 皇后问题的一种解法。

给定一个整数 *n*，返回 *n* 皇后不同的解决方案的数量。

**示例:**

```
输入: 4
输出: 2
解释: 4 皇后问题存在如下两个不同的解法。
[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

 

**提示：**

- **皇后**，是[国际象棋](https://baike.baidu.com/item/国际象棋)中的棋子，意味着[国王](https://baike.baidu.com/item/国王)的妻子。皇后只做一件事，那就是“[吃子](https://baike.baidu.com/item/吃子)”。当她遇见可以吃的棋子时，就迅速冲上去吃掉棋子。当然，她横、竖、斜都可走一或 N-1 步，可进可退。（引用自 [百度百科 - 皇后](https://baike.baidu.com/item/皇后/15860305?fr=aladdin) ）

其实并不难，回溯法。重点在于如何判断是否在一条斜线上。有点类似解析几何。在一条斜线上，他们x+y，或者x-y的和应该是相等的

~~~java
class Solution {
    public int totalNQueens(int n) {
        Set<Integer> col = new HashSet<>();
        Set<Integer> dia1 = new HashSet<>();
        Set<Integer> dia2 = new HashSet<>();
        return backtract(n,0,col,dia1,dia2);
    }
    public int backtract(int n, int row, Set<Integer> col, Set<Integer> dia1, Set<Integer> dia2){
        if(n == row)
            return 1;
        int count = 0;
        for(int i = 0; i < n; i++){
            if(!col.contains(i) && !dia1.contains(row+i) && !dia2.contains(i-row)){
                col.add(i);
                dia1.add(row+i);
                dia2.add(i-row);
                count += backtract(n,row+1,col,dia1,dia2);
                col.remove(i);
                dia1.remove(row+i);
                dia2.remove(i-row);
            }
        }
        return count;
    }
}
~~~

击败23%

### Q[51N 皇后](https://leetcode-cn.com/problems/n-queens/)

难度困难635

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/8-queens.png)

上图为 8 皇后问题的一种解法。

给定一个整数 *n*，返回所有不同的 *n* 皇后问题的解决方案。

每一种解法包含一个明确的 *n* 皇后问题的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

 

**示例：**

```
输入：4
输出：[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。
```

 

**提示：**

- 皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上。

顺手解决这题

~~~java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        Set<Integer> col = new HashSet<>();
        Set<Integer> dia1 = new HashSet<>();
        Set<Integer> dia2 = new HashSet<>();
        List<List<String>> ans = new ArrayList<>();
        List<String> l = new ArrayList<>();
        backtract(n, 0, col, dia1, dia2, ans,l);
        return ans;
    }

    public void backtract(int n, int row, Set<Integer> col, Set<Integer> dia1, Set<Integer> dia2, List<List<String>> ans,List<String> temp) {
        if (n == row) {
            ans.add(new ArrayList<>(temp));
        }
        int count = 0;
        for (int i = 0; i < n; i++) {
            if (!col.contains(i) && !dia1.contains(row + i) && !dia2.contains(i - row)) {
                col.add(i);
                dia1.add(row + i);
                dia2.add(i - row);
                temp.add(builder(n,i));
                backtract(n, row + 1, col, dia1, dia2,ans,temp);
                temp.remove(temp.size()-1);
                col.remove(i);
                dia1.remove(row + i);
                dia2.remove(i - row);
            }
        }
        
    }

    public String builder(int n, int pos) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            if (i == pos)
                sb.append('Q');
            else
                sb.append('.');
        }
        return sb.toString();
    }
}
~~~

击败45%

### Q[31下一个排列](https://leetcode-cn.com/problems/next-permutation/)

难度 中等

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须**[原地](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
`1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1`

仿照标答写的。先从右往左，找到num[i-1] < num[i], 然后从右往左，找一个刚好比i-1 大的与他交换。再把i之后的反转。

相当于每次认为 i-1之后已经是最后一个排列了。

~~~java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        int i = n-1;
        while(i>0&&nums[i-1]>=nums[i]){
            i--;
        }
        i--;
        if(i>=0){
            int j = n - 1;
            while (i <= j && nums[j] <= nums[i]) {
                j--;
            }
            swap(nums, i, j);
        }
        reverse(nums,i+1);
    }
    public void swap(int[] nums, int i, int j){
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
    public void reverse(int[] nums, int i){
        int j = nums.length-1;
        while(i<j){
            swap(nums,i,j);
            i++;
            j--;
        }
    }
}
~~~

击败99%

### Q[122买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

难度 简单

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

**示例 2:**

```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 

**提示：**

- `1 <= prices.length <= 3 * 10 ^ 4`
- `0 <= prices[i] <= 10 ^ 4`

简单dp，在持有股票与未持有直接转移

~~~java
class Solution {
    public int maxProfit(int[] prices){
        int[][] dp = new int[prices.length][2];
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for(int i = 1; i < prices.length; ++i){
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]-prices[i]);
            dp[i][1] = Math.max(dp[i-1][1],dp[i-1][0]+prices[i]);
        }
        return dp[prices.length-1][1];
    }
}
~~~

击败14%

### Q[189旋转数组](https://leetcode-cn.com/problems/rotate-array/)

难度 中等

给定一个数组，将数组中的元素向右移动 *k* 个位置，其中 *k* 是非负数。

**示例 1:**

```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

**示例 2:**

```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

**说明:**

- 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
- 要求使用空间复杂度为 O(1) 的 **原地** 算法。

看题解，先反转整个数组，将k之后反转，将0-k-1反转

~~~java
class Solution {
    public void rotate(int[] nums, int k) {
        if (k > nums.length) k %= nums.length;
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int tempt = nums[left];
            nums[left] = nums[right];
            nums[right] = tempt;
            left++;
            right--;
        }
        left = k;
        right = nums.length - 1;
        while (left < nums.length && left < right) {
            int tempt = nums[left];
            nums[left] = nums[right];
            nums[right] = tempt;
            left++;
            right--;
        }
        left = 0;
        right = k - 1;
        while (left < right) {
            int tempt = nums[left];
            nums[left] = nums[right];
            nums[right] = tempt;
            left++;
            right--;
        }
    }
}
~~~

击败100%

### Q[217存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)

难度 简单

给定一个整数数组，判断是否存在重复元素。

如果任意一值在数组中出现至少两次，函数返回 `true` 。如果数组中每个元素都不相同，则返回 `false` 。

 

**示例 1:**

```
输入: [1,2,3,1]
输出: true
```

**示例 2:**

```
输入: [1,2,3,4]
输出: false
```

**示例 3:**

```
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
```

set

~~~java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for(int i: nums){
            if(s.contains(i))
                return true;
            s.add(i);
        }
        return false;
    }
}
~~~

击败56%

hashmap击败49%，性能差不多

### Q[350两个数组的交集 II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

难度 简单

给定两个数组，编写一个函数来计算它们的交集。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
```

**示例 2:**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
```

 

**说明：**

- 输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
- 我们可以不考虑输出结果的顺序。

***\*进阶\**：**

- 如果给定的数组已经排好序呢？你将如何优化你的算法？
- 如果 *nums1* 的大小比 *nums2* 小很多，哪种方法更优？
- 如果 *nums2* 的元素存储在磁盘上，内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

看进阶 我还以为已经排好序了，浪费了一发

~~~java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        ArrayList<Integer> l = new ArrayList<>();
        int n1 = 0, n2 = 0;
        while(n1<nums1.length&&n2<nums2.length){
            if(nums1[n1] == nums2[n2]){
                l.add(nums1[n1]);
                ++n1;
                ++n2;
            }else if(nums1[n1]>nums2[n2]){
                n2++;
            }else{
                n1++;
            }
        }
        int[] ans = new int[l.size()];
        for(int i = 0; i < l.size(); ++i)
            ans[i] = l.get(i);
        return ans;
    }
}
~~~

击败87%

### Q[66加一](https://leetcode-cn.com/problems/plus-one/)

难度 简单

给定一个由**整数**组成的**非空**数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

**示例 1:**

```
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。
```

**示例 2:**

```
输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。
```

这题也是随便写

~~~java
class Solution {
    public int[] plusOne(int[] digits) {
        int pos = digits.length-1;
        while(pos>=0&&digits[pos]==9){
            digits[pos] = 0;
            pos--;
        }
        if(pos == -1){
            int[] ans = new int[digits.length+1];
            ans[0] = 1;
            return ans;
        }
        digits[pos]++;
        return digits;
    }
}
~~~

击败100%

### Q[36有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)

难度 中等

判断一个 9x9 的数独是否有效。只需要**根据以下规则**，验证已经填入的数字是否有效即可。

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

上图是一个部分填充的有效的数独。

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

**示例 1:**

```
输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
```

**示例 2:**

```
输入:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: false
解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

**说明:**

- 一个有效的数独（部分已被填充）不一定是可解的。
- 只需要根据以上规则，验证已经填入的数字是否有效即可。
- 给定数独序列只包含数字 `1-9` 和字符 `'.'` 。
- 给定数独永远是 `9x9` 形式的。

搞几个数组记录就好了

~~~java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int[][] row = new int[9][10];
        int[][] col = new int[9][10];
        int[][] mat = new int[9][10];
        for (int i = 0; i < board.length; ++i) {
            for (int j = 0; j < board[0].length; ++j) {
                if (board[i][j] == '.')
                    continue;
                int ch = board[i][j] - '0';
                if (row[i][ch] == 1 || col[j][ch] == 1 || mat[(j / 3 + i / 3 * 3)][ch] == 1)
                    return false;
                row[i][ch]++;
                col[j][ch]++;
                mat[(j / 3 + i / 3 * 3)][ch]++;
            }
        }
        return true;
    }
}
~~~

击败96%

### Q[844比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)

难度 简单

给定 `S` 和 `T` 两个字符串，当它们分别被输入到空白的文本编辑器后，判断二者是否相等，并返回结果。 `#` 代表退格字符。

**注意：**如果对空文本输入退格字符，文本继续为空。

 

**示例 1：**

```
输入：S = "ab#c", T = "ad#c"
输出：true
解释：S 和 T 都会变成 “ac”。
```

**示例 2：**

```
输入：S = "ab##", T = "c#d#"
输出：true
解释：S 和 T 都会变成 “”。
```

**示例 3：**

```
输入：S = "a##c", T = "#a#c"
输出：true
解释：S 和 T 都会变成 “c”。
```

**示例 4：**

```
输入：S = "a#c", T = "b"
输出：false
解释：S 会变成 “c”，但 T 仍然是 “b”。
```

 

**提示：**

1. `1 <= S.length <= 200`
2. `1 <= T.length <= 200`
3. `S` 和 `T` 只含有小写字母以及字符 `'#'`。

 

**进阶：**

- 你可以用 `O(N)` 的时间复杂度和 `O(1)` 的空间复杂度解决该问题吗？

当然是直接写O（N），从后往前读，遇到#，就往前跳

~~~java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        if (S.length() == 0 && T.length() == 0) return true;
        if (S.length() == 0 || T.length() == 0) return false;
        int pos1 = S.length() - 1;
        int return1 = 0;
        int pos2 = T.length() - 1;
        int return2 = 0;
        while (pos1 >= 0 && pos2 >= 0) {
            if (S.charAt(pos1) == '#') {
                pos1--;
                return1++;
            }
            while (pos1 >= 0 && (S.charAt(pos1) == '#' || return1 > 0)) {
                if (S.charAt(pos1) == '#') {
                    return1++;
                    pos1--;
                } else {
                    pos1--;
                    return1--;
                }
            }
            if (T.charAt(pos2) == '#') {
                pos2--;
                return2++;
            }
            while (pos2 >= 0 && (T.charAt(pos2) == '#' || return2 > 0)) {
                if (T.charAt(pos2) == '#') {
                    return2++;
                    pos2--;
                } else {
                    pos2--;
                    return2--;
                }
            }
            if (pos1 < 0 || pos2 < 0)
                break;
            if (T.charAt(pos2) != S.charAt(pos1))
                return false;
            else {
                pos1--;
                pos2--;
            }
        }
        while (pos1 >= 0 && (S.charAt(pos1) == '#' || return1 > 0)) {
            if (S.charAt(pos1) == '#') {
                return1++;
                pos1--;
            } else {
                pos1--;
                return1--;
            }
        }
        while (pos2 >= 0 && (T.charAt(pos2) == '#' || return2 > 0)) {
            if (T.charAt(pos2) == '#') {
                return2++;
                pos2--;
            } else {
                pos2--;
                return2--;
            }
        }
        if (pos1 >= 0 || pos2 >= 0) return false;
        return true;
    }
}
~~~

写得很复杂。漏考虑很多下细节。几乎每个地方都浪费了一发

六发解决。击败96%

### Q[242有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

难度 简单

给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

**说明:**
你可以假设字符串只包含小写字母。

**进阶:**
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

简单题，秒杀

~~~java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() == 0 && t.length() == 0) return true;
        if (s.length() == 0 || t.length() == 0) return false;
        int[] chars = new int[26];
        for (int i = 0; i < s.length(); i++) {
            chars[s.charAt(i) - 'a']++;
        }
        for (int i = 0; i < t.length(); i++) {
            chars[t.charAt(i) - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (chars[i] != 0)
                return false;
        }
        return true;
    }
}
~~~

击败48%

### Q[125验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

难度 简单

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明：**本题中，我们将空字符串定义为有效的回文串。

**示例 1:**

```
输入: "A man, a plan, a canal: Panama"
输出: true
```

**示例 2:**

```
输入: "race a car"
输出: false
```

刚开始还以为 不考虑数字

~~~java
class Solution {
    public boolean isPalindrome(String s) {
        if (s.length() == 0) return true;
        s = s.toLowerCase();
        int left = 0, right = s.length() - 1;
        while (left < right) {
            while (left < right && !Character.isLetter(s.charAt(left)) && !Character.isDigit(s.charAt(left))) {
                left++;
            }
            while (left < right && !Character.isLetter(s.charAt(right)) && !Character.isDigit(s.charAt(right))) {
                right--;
            }
            if (s.charAt(left) != s.charAt(right))
                return false;
            left++;
            right--;
        }
        return true;
    }
}
~~~

击败49%

### Q[143重排链表](https://leetcode-cn.com/problems/reorder-list/)

难度 中等

给定一个单链表 *L*：*L*0→*L*1→…→*L**n*-1→*L*n ，
将其重新排列后变为： *L*0→*L**n*→*L*1→*L**n*-1→*L*2→*L**n*-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**示例 1:**

```
给定链表 1->2->3->4, 重新排列为 1->4->2->3.
```

**示例 2:**

```
给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
```

先找中点，反转中点之后，然后再把后半部分一个个插入到前面

~~~java
class Solution {
    public void reorderList(ListNode head) {
        if(head == null) return;
        ListNode middle = head, last = head, preMiddle = head, next = head;
        while (last != null) {
            last = last.next;
            if (last != null)
                last = last.next;
            else
                break;
            preMiddle = middle;
            middle = middle.next;
        }
        preMiddle.next = null;
        preMiddle = null;
        while (middle != null) {
            next = middle.next;
            middle.next = preMiddle;
            preMiddle = middle;
            middle = next;
        }
        ListNode l1p1 = head, l1p2 = head.next;
        ListNode l2p1 = preMiddle, l2p2 = l2p1.next;
        while (l1p1 != null && l2p1 !=null) {
            l1p1.next = l2p1;
            if(l1p2== null&&l2p2!=null)
                ;
            else
                l2p1.next = l1p2;
            l1p1 = l1p2;
            l2p1 = l2p2;
            if (l1p2 != null)
                l1p2 = l1p2.next;
            if (l2p2 != null)
                l2p2 = l2p2.next;
        }
    }
}
~~~

击败79%

### Q[28实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

难度简单600

实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回 **-1**。

**示例 1:**

```
输入: haystack = "hello", needle = "ll"
输出: 2
```

**示例 2:**

```
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

**说明:**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)) 定义相符。

写自闭了，直接用库

~~~java
class Solution {
    public int strStr(String haystack, String needle) {
        return haystack.indexOf(needle);
    }
}
~~~



### Q[620有趣的电影](https://leetcode-cn.com/problems/not-boring-movies/)

难度 简单

SQL架构

某城市开了一家新的电影院，吸引了很多人过来看电影。该电影院特别注意用户体验，专门有个 LED显示板做电影推荐，上面公布着影评和相关电影描述。

作为该电影院的信息部主管，您需要编写一个 SQL查询，找出所有影片描述为**非** `boring` (不无聊) 的并且 **id 为奇数** 的影片，结果请按等级 `rating` 排列。

 

例如，下表 `cinema`:

```
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   1     | War       |   great 3D   |   8.9     |
|   2     | Science   |   fiction    |   8.5     |
|   3     | irish     |   boring     |   6.2     |
|   4     | Ice song  |   Fantacy    |   8.6     |
|   5     | House card|   Interesting|   9.1     |
+---------+-----------+--------------+-----------+
```

对于上面的例子，则正确的输出是为：

```
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   5     | House card|   Interesting|   9.1     |
|   1     | War       |   great 3D   |   8.9     |
+---------+-----------+--------------+-----------+
```

试试sql

~~~mysql
# Write your MySQL query statement below
SELECT * FROM cinema WHERE description != "boring" AND id%2!=0 ORDER BY rating DESC
~~~

击败41%

### Q[182查找重复的电子邮箱](https://leetcode-cn.com/problems/duplicate-emails/)

难度 简单

SQL架构

编写一个 SQL 查询，查找 `Person` 表中所有重复的电子邮箱。

**示例：**

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

根据以上输入，你的查询应返回以下结果：

```
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

**说明：**所有电子邮箱都是小写字母。

学习学习

~~~mysql
SELECT distinct a.Email 
    FROM Person a
    INNER JOIN Person b
    ON a.Email = b.Email AND a.Id <> b.Id
~~~

击败63%

### Q[595大的国家](https://leetcode-cn.com/problems/big-countries/)

难度 简单

SQL架构

这里有张 `World` 表

```
+-----------------+------------+------------+--------------+---------------+
| name            | continent  | area       | population   | gdp           |
+-----------------+------------+------------+--------------+---------------+
| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |
| Albania         | Europe     | 28748      | 2831741      | 12960000      |
| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |
| Andorra         | Europe     | 468        | 78115        | 3712000       |
| Angola          | Africa     | 1246700    | 20609294     | 100990000     |
+-----------------+------------+------------+--------------+---------------+
```

如果一个国家的面积超过 300 万平方公里，或者人口超过 2500 万，那么这个国家就是大国家。

编写一个 SQL 查询，输出表中所有大国家的名称、人口和面积。

例如，根据上表，我们应该输出:

```
+--------------+-------------+--------------+
| name         | population  | area         |
+--------------+-------------+--------------+
| Afghanistan  | 25500100    | 652230       |
| Algeria      | 37100000    | 2381741      |
+--------------+-------------+--------------+
```

~~~mysql
# Write your MySQL query statement below
SELECT name,population,area From World WHERE area > 3000000 OR population > 25000000;
~~~

击败54%

### Q[627变更性别](https://leetcode-cn.com/problems/swap-salary/)

难度 简单

SQL架构

给定一个 `salary` 表，如下所示，有 m = 男性 和 f = 女性 的值。交换所有的 f 和 m 值（例如，将所有 f 值更改为 m，反之亦然）。要求只使用一个更新（Update）语句，并且没有中间的临时表。

注意，您必只能写一个 Update 语句，请不要编写任何 Select 语句。

**例如：**

```
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |
```

运行你所编写的更新语句之后，将会得到以下表:

```
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |
```

学会了新语句 if(sex = 'm','f','m')

~~~mysql
# Write your MySQL query statement below
Update salary SET sex = if(sex = 'm','f','m');
~~~

击败31%

### Q[175组合两个表](https://leetcode-cn.com/problems/combine-two-tables/)

难度 简单

SQL架构

表1: `Person`

```
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键
```

表2: `Address`

```
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键
```

 

编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：

 

```
FirstName, LastName, City, State
```

~~~mysql
# Write your MySQL query statement below
SELECT a.FirstName FirstName,a.LastName LastName,b.City City,b.State State 
From Person a
LEFT OUTER JOIN Address b
ON a.PersonId = b.PersonId;
~~~

### Q[181超过经理收入的员工](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)

难度 简单

SQL架构

`Employee` 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

给定 `Employee` 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

```
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

~~~mysql
# Write your MySQL query statement below
SELECT a.Name Employee
FROM Employee a, Employee b
WHERE b.Id = a.ManagerId
    AND
        a.Salary > b.Salary;
~~~

击败78% 

### Q[183. 从不订购的客户](https://leetcode-cn.com/problems/customers-who-never-order/)

难度简单174

SQL架构

某网站包含两个表，`Customers` 表和 `Orders` 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

`Customers` 表：

```
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```

`Orders` 表：

```
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

例如给定上述表格，你的查询应返回：

```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

~~~mysql
# Write your MySQL query statement below
select a.Name Customers
from Customers a
Left Join Orders b 
ON a.Id = b.CustomerId
WHERE b.Id is Null;
~~~

击败92%

### Q[925长按键入](https://leetcode-cn.com/problems/long-pressed-name/)

难度 简单

你的朋友正在使用键盘输入他的名字 `name`。偶尔，在键入字符 `c` 时，按键可能会被*长按*，而字符可能被输入 1 次或多次。

你将会检查键盘输入的字符 `typed`。如果它对应的可能是你的朋友的名字（其中一些字符可能被长按），那么就返回 `True`。

 

**示例 1：**

```
输入：name = "alex", typed = "aaleex"
输出：true
解释：'alex' 中的 'a' 和 'e' 被长按。
```

**示例 2：**

```
输入：name = "saeed", typed = "ssaaedd"
输出：false
解释：'e' 一定需要被键入两次，但在 typed 的输出中不是这样。
```

**示例 3：**

```
输入：name = "leelee", typed = "lleeelee"
输出：true
```

**示例 4：**

```
输入：name = "laiden", typed = "laiden"
输出：true
解释：长按名字中的字符并不是必要的。
```

 

**提示：**

1. `name.length <= 1000`
2. `typed.length <= 1000`
3. `name` 和 `typed` 的字符都是小写字母。

 记住之前那个字符就好了。

~~~java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        if(typed.length() == 0 && name.length() == 0) return true;
        if(typed.length() == 0 || name.length() == 0) return false;
        char pre = name.charAt(0);
        int i = 0, j = 0;
        while (i<name.length()&&j<typed.length()){
            if(name.charAt(i)==typed.charAt(j)){
                pre = name.charAt(i);
                ++i;
                ++j;
            }else if(pre == typed.charAt(j)){
                ++j;
            }else {
                return false;
            }
        }
        while (i == name.length()&&j<typed.length()){
            if(pre == typed.charAt(j)){
                ++j;
            }else {
                return false;
            }
        }if(i<name.length()||j<typed.length())
            return false;
        return true;
    }
}
~~~

击败86%

### Q[38外观数列](https://leetcode-cn.com/problems/count-and-say/)

难度 简单

给定一个正整数 *n*（1 ≤ *n* ≤ 30），输出外观数列的第 *n* 项。

注意：整数序列中的每一项将表示为一个字符串。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

第一项是数字 1

描述前一项，这个数是 `1` 即 “一个 1 ”，记作 `11`

描述前一项，这个数是 `11` 即 “两个 1 ” ，记作 `21`

描述前一项，这个数是 `21` 即 “一个 2 一个 1 ” ，记作 `1211`

描述前一项，这个数是 `1211` 即 “一个 1 一个 2 两个 1 ” ，记作 `111221`

 

**示例 1:**

```
输入: 1
输出: "1"
解释：这是一个基本样例。
```

**示例 2:**

```
输入: 4
输出: "1211"
解释：当 n = 3 时，序列是 "21"，其中我们有 "2" 和 "1" 两组，"2" 可以读作 "12"，也就是出现频次 = 1 而 值 = 2；类似 "1" 可以读作 "11"。所以答案是 "12" 和 "11" 组合在一起，也就是 "1211"。
```

之间模拟过程

~~~java
class Solution {
    public String countAndSay(int n) {
        String s = new String("1");
        StringBuilder temp = new StringBuilder();
        int i = 1;
        while(i<n){
            char ch = s.charAt(0);
            int count = 0;
            for(int j = 0; j < s.length(); j++){
                if(ch == s.charAt(j)){
                    count++;
                }else{
                    temp.append(count);
                    temp.append(ch);
                    ch = s.charAt(j);
                    count = 1;
                }
            }
            temp.append(count);
            temp.append(ch);
            s = temp.toString();
            temp = new StringBuilder();
            i++;
        }
        return s;
    }
}
~~~

击败25%

### Q[108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

难度 简单

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。

**示例:**

```
给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```

分治法

~~~java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return helper(nums,0,nums.length-1);
    }
    public TreeNode helper(int[] nums, int left, int right){
        if(right<left)
            return null;
        int mid = (left+right)/2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = helper(nums,left,mid-1);
        root.right = helper(nums,mid+1,right);
        return root;
    }
}
~~~

击败100%

### Q[278第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

难度 简单

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 `n` 个版本 `[1, 2, ..., n]`，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 `bool isBadVersion(version)` 接口来判断版本号 `version` 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

**示例:**

```
给定 n = 5，并且 version = 4 是第一个错误的版本。

调用 isBadVersion(3) -> false
调用 isBadVersion(5) -> true
调用 isBadVersion(4) -> true

所以，4 是第一个错误的版本。 
```

mid = i + (j - i)/2;与mid = (i+j)/2有啥区别呢

i + j 可能会溢出 

~~~java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int i = 1,j = n;
        int mid = i + (j - i) / 2;
        while(i<j){
            if(isBadVersion(mid)){
                j = mid;
            }else{
                i = mid+1;
            }
            mid = i + (j - i) / 2;
        }
        return i;
    }
}
~~~

击败25%

### Q[384打乱数组](https://leetcode-cn.com/problems/shuffle-an-array/)

难度 中等

打乱一个没有重复元素的数组。

 

**示例:**

```
// 以数字集合 1, 2 和 3 初始化数组。
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。
solution.shuffle();

// 重设数组到它的初始状态[1,2,3]。
solution.reset();

// 随机返回数组[1,2,3]打乱后的结果。
solution.shuffle();
```

~~~java
class Solution {
    int[] data;
    int[] copy;
    public Solution(int[] nums) {
        data = new int[nums.length];
        copy = new int[nums.length];
        for(int i = 0; i < nums.length; i++) {
            data[i] = nums[i];
            copy[i] = nums[i];
        }
    }

    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        return data;
    }

    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        Random r = new Random();
        for(int i = 0; i < copy.length; i++){
            int ran1 = r.nextInt(copy.length);
            int ran2 = r.nextInt(copy.length);
            int temp = copy[ran1];
            copy[ran1] = copy[ran2];
            copy[ran2] = temp;
        }
        return copy;
    }
}
~~~

击败61%

### Q[412Fizz Buzz](https://leetcode-cn.com/problems/fizz-buzz/)

难度 简单

写一个程序，输出从 1 到 *n* 数字的字符串表示。

\1. 如果 *n* 是3的倍数，输出“Fizz”；

\2. 如果 *n* 是5的倍数，输出“Buzz”；

3.如果 *n* 同时是3和5的倍数，输出 “FizzBuzz”。

**示例：**

```
n = 15,

返回:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

~~~java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> ans = new ArrayList<>();
        StringBuilder s = new StringBuilder();
        String s1 = new String("FizzBuzz");
        for(int i = 1; i <= n; i++){
            if(i%3==0 && i%5==0)
                ans.add(new String("FizzBuzz"));
            else if(i%3==0)
                ans.add("Fizz");
            else if(i%5==0)
                ans.add("Buzz");
            else
                ans.add(""+i);
        }
        return ans;
    }
}
~~~

击败36%

### Q[204计数质数](https://leetcode-cn.com/problems/count-primes/)

难度 简单

统计所有小于非负整数 *`n`* 的质数的数量。

 

**示例 1：**

```
输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

**示例 2：**

```
输入：n = 0
输出：0
```

**示例 3：**

```
输入：n = 1
输出：0
```

稍微剪下枝

(i&1)==0判断是不是偶数

~~~java
class Solution {
    public int countPrimes(int n) {
        if (n == 1) return 0;
        int count = 0;
        for (int i = 2; i < n; i++)
            if (isPrime(i))
                count++;
        return count;
    }

    public boolean isPrime(int x) {
        if (x == 2)
            return true;
        if ((x & 1) == 0)
            return false;
        for (int i = 3; i * i <= x; i += 2) {
            if (x % i == 0)
                return false;
        }
        return true;
    }
}
~~~

击败12%

### Q[13罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

难度简单1090

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

 

**示例 1:**

```
输入: "III"
输出: 3
```

**示例 2:**

```
输入: "IV"
输出: 4
```

**示例 3:**

```
输入: "IX"
输出: 9
```

**示例 4:**

```
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

**示例 5:**

```
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

 

**提示：**

- 题目所给测试用例皆符合罗马数字书写规则，不会出现跨位等情况。
- IC 和 IM 这样的例子并不符合题目要求，49 应该写作 XLIX，999 应该写作 CMXCIX 。
- 关于罗马数字的详尽书写规则，可以参考 [罗马数字 - Mathematics ](https://b2b.partcommunity.com/community/knowledge/zh_CN/detail/10753/罗马数字#knowledge_article)。

当当前字母比右边小，则减

其他情况则加

~~~java
class Solution {
    public int romanToInt(String s) {
        HashMap<Character,Integer> m = new HashMap<>();
        m.put('I',1);
        m.put('V',5);
        m.put('X',10);
        m.put('L',50);
        m.put('C',100);
        m.put('D',500);
        m.put('M',1000);
        int ans = 0;
        for(int i = 0; i < s.length(); i++){
            if(i+1<s.length()){
                if(m.get(s.charAt(i))<m.get(s.charAt(i+1)))
                    ans-=m.get(s.charAt(i));
                else
                    ans+=m.get(s.charAt(i));
            }
            else{
                ans+=m.get(s.charAt(i));
            }
        }
        return ans;
    }
}
~~~

击败40%

### Q[191位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)

难度 简单

编写一个函数，输入是一个无符号整数，返回其二进制表达式中数字位数为 ‘1’ 的个数（也被称为[汉明重量](https://baike.baidu.com/item/汉明重量)）。

 

**示例 1：**

```
输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
```

**示例 2：**

```
输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
```

**示例 3：**

```
输入：11111111111111111111111111111101
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
```

 

**提示：**

- 请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
- 在 Java 中，编译器使用[二进制补码](https://baike.baidu.com/item/二进制补码/5295284)记法来表示有符号整数。因此，在上面的 **示例 3** 中，输入表示有符号整数 `-3`。

 

**进阶**:
如果多次调用这个函数，你将如何优化你的算法？

![image.png](https://pic.leetcode-cn.com/abfd6109e7482d70d20cb8fc1d632f90eacf1b5e89dfecb2e523da1bcb562f66-image.png)

~~~java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int sum = 0;
        while(n!=0){
            sum++;
            n = n&(n-1);
        }
        return sum;
    }
}
~~~

击败98%

### Q[190颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)

难度 简单

颠倒给定的 32 位无符号整数的二进制位。

 

**示例 1：**

```
输入: 00000010100101000001111010011100
输出: 00111001011110000010100101000000
解释: 输入的二进制串 00000010100101000001111010011100 表示无符号整数 43261596，
     因此返回 964176192，其二进制表示形式为 00111001011110000010100101000000。
```

**示例 2：**

```
输入：11111111111111111111111111111101
输出：10111111111111111111111111111111
解释：输入的二进制串 11111111111111111111111111111101 表示无符号整数 4294967293，
     因此返回 3221225471 其二进制表示形式为 10111111111111111111111111111111 。
```

 

**提示：**

- 请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
- 在 Java 中，编译器使用[二进制补码](https://baike.baidu.com/item/二进制补码/5295284)记法来表示有符号整数。因此，在上面的 **示例 2** 中，输入表示有符号整数 `-3`，输出表示有符号整数 `-1073741825`。

 

**进阶**:
如果多次调用这个函数，你将如何优化你的算法？

一位位加上去

~~~java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int ans = 0;
        for(int i = 0; i < 32; i++){
            ans+=((n&1)<<(31-i));
            n>>=1;
        }
        return ans;
    }
}
~~~

击败100%，没想到效率还挺高

### Q[268丢失的数字](https://leetcode-cn.com/problems/missing-number/)

难度 简单

给定一个包含 `[0, n]` 中 `n` 个数的数组 `nums` ，找出 `[0, n]` 这个范围内没有出现在数组中的那个数。

 

**进阶：**

- 你能否实现线性时间复杂度、仅使用额外常数空间的算法解决此问题?

 

**示例 1：**

```
输入：nums = [3,0,1]
输出：2
解释：n = 3，因为有 3 个数字，所以所有的数字都在范围 [0,3] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 2：**

```
输入：nums = [0,1]
输出：2
解释：n = 2，因为有 2 个数字，所以所有的数字都在范围 [0,2] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 3：**

```
输入：nums = [9,6,4,2,3,5,7,0,1]
输出：8
解释：n = 9，因为有 9 个数字，所以所有的数字都在范围 [0,9] 内。8 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 4：**

```
输入：nums = [0]
输出：1
解释：n = 1，因为有 1 个数字，所以所有的数字都在范围 [0,1] 内。1 是丢失的数字，因为它没有出现在 nums 中。
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 104`
- `0 <= nums[i] <= n`
- `nums` 中的所有数字都 **独一无二**

先用等差数列求得和，然后用和去减每一个数

~~~java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int sum = (n+1)*n/2;
        for(int i:nums)
            sum-=i;
        return sum;
    }
}
~~~

击败100%

### Q[334递增的三元子序列](https://leetcode-cn.com/problems/increasing-triplet-subsequence/)

难度 中等

给定一个未排序的数组，判断这个数组中是否存在长度为 3 的递增子序列。

数学表达式如下:

> 如果存在这样的 *i, j, k,* 且满足 0 ≤ *i* < *j* < *k* ≤ *n*-1，
> 使得 *arr[i]* < *arr[j]* < *arr[k]* ，返回 true ; 否则返回 false 。

**说明:** 要求算法的时间复杂度为 O(*n*)，空间复杂度为 O(*1*) 。

**示例 1:**

```
输入: [1,2,3,4,5]
输出: true
```

**示例 2:**

```
输入: [5,4,3,2,1]
输出: false
```

存两个数min1 min2，记录两个最小值，当当前数大于min2，则说明存在

~~~java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int min1 = Integer.MAX_VALUE,min2 = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++){
            if(min1>=nums[i])//这个min1此时存的并不是对应的序列，而是最小值
                min1 = nums[i];
            else if(min2>=nums[i])//只要nums[i]>min2 就说明存在这样的子序列
                min2 = nums[i];
            else
                return true;
        }
        return false;
    }
}
~~~

击败100%

### Q[328奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

难度 中等

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

**示例 1:**

```
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
```

**示例 2:**

```
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
```

**说明:**

- 应当保持奇数节点和偶数节点的相对顺序。
- 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。

链表题没什么好说的，就是最后怎么处理，需要想想

~~~java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;
        if (head.next == null) return head;
        ListNode odd = head, even = head.next, temp = head.next;
        while (temp != null) {
            odd.next = temp.next;
            if(odd.next == null) {
                odd.next = even;
                odd = null;
            }
            else
                odd = odd.next;
            temp.next = odd == null ? null : odd.next;
            temp = odd == null ? null : odd.next;
        }
        if (odd != null)
            odd.next = even;
        return head;
    }
}
~~~

击败100%



### Q[103二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

难度 中等

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```

用两个栈

~~~java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        if (root == null) return new ArrayList<>();
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> l;
        Stack<TreeNode> sleft = new Stack<>();
        Stack<TreeNode> sright = new Stack<>();
        sleft.push(root);
        while (!sleft.empty() || !sright.empty()) {
            l = new ArrayList<>();
            while (!sleft.empty()) {
                TreeNode t1 = sleft.pop();
                l.add(t1.val);
                if (t1.left != null)
                    sright.push(t1.left);
                if (t1.right != null)
                    sright.push(t1.right);
            }
            if (l.size() != 0) {
                ans.add(l);
                l = new ArrayList<>();
            }
            while (!sright.empty()) {
                TreeNode t1 = sright.pop();
                l.add(t1.val);
                if (t1.right != null)
                    sleft.push(t1.right);
                if (t1.left != null)
                    sleft.push(t1.left);
            }
            if (l.size() != 0) {
                ans.add(l);
                l = new ArrayList<>();
            }
        }
        return ans;
    }
}
~~~

击败98%

### Q[230二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

难度 中等

给定一个二叉搜索树，编写一个函数 `kthSmallest` 来查找其中第 **k** 个最小的元素。

**说明：**
你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。

**示例 1:**

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 1
```

**示例 2:**

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 3
```

**进阶：**
如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第 k 小的值，你将如何优化 `kthSmallest` 函数？

简单的中序遍历，记录结点个数

~~~java
class Solution {
    int ans;

    public int kthSmallest(TreeNode root, int k) {
        inorder(root,k,0);
        return ans;
    }

    public int inorder(TreeNode root, int k, int num) {
        if(root == null)
            return 0;
        if (root.left == null && root.right == null) {
            if (k == num + 1)
                ans = root.val;
            return 1;
        }
        int left = 0;
        if(root.left!=null)
            left = inorder(root.left,k,num);
        if(left+1+num == k){
            ans = root.val;
        }
        int right = 0;
        if(root.right!=null){
            right = inorder(root.right,k,num+left+1);
        }
        return left+1+right;
    }
}
~~~

击败100%

### Q[1024视频拼接](https://leetcode-cn.com/problems/video-stitching/)

难度 中等

你将会获得一系列视频片段，这些片段来自于一项持续时长为 `T` 秒的体育赛事。这些片段可能有所重叠，也可能长度不一。

视频片段 `clips[i]` 都用区间进行表示：开始于 `clips[i][0]` 并于 `clips[i][1]` 结束。我们甚至可以对这些片段自由地再剪辑，例如片段 `[0, 7]` 可以剪切成 `[0, 1] + [1, 3] + [3, 7]` 三部分。

我们需要将这些片段进行再剪辑，并将剪辑后的内容拼接成覆盖整个运动过程的片段（`[0, T]`）。返回所需片段的最小数目，如果无法完成该任务，则返回 `-1` 。

 

**示例 1：**

```
输入：clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], T = 10
输出：3
解释：
我们选中 [0,2], [8,10], [1,9] 这三个片段。
然后，按下面的方案重制比赛片段：
将 [1,9] 再剪辑为 [1,2] + [2,8] + [8,9] 。
现在我们手上有 [0,2] + [2,8] + [8,10]，而这些涵盖了整场比赛 [0, 10]。
```

**示例 2：**

```
输入：clips = [[0,1],[1,2]], T = 5
输出：-1
解释：
我们无法只用 [0,1] 和 [1,2] 覆盖 [0,5] 的整个过程。
```

**示例 3：**

```
输入：clips = [[0,1],[6,8],[0,2],[5,6],[0,4],[0,3],[6,7],[1,3],[4,7],[1,4],[2,5],[2,6],[3,4],[4,5],[5,7],[6,9]], T = 9
输出：3
解释： 
我们选取片段 [0,4], [4,7] 和 [6,9] 。
```

**示例 4：**

```
输入：clips = [[0,4],[2,8]], T = 5
输出：2
解释：
注意，你可能录制超过比赛结束时间的视频。
```

 

**提示：**

- `1 <= clips.length <= 100`
- `0 <= clips[i][0] <= clips[i][1] <= 100`
- `0 <= T <= 100`

dp问题 记录最少次数，从第一个位置开始枚举，每个位置遍历数组

~~~java

class Solution {
    public int videoStitching(int[][] clips, int T) {
        int[] dp = new int[T + 1];
        Arrays.fill(dp, Integer.MAX_VALUE - 1);
        dp[0] = 0;
        for (int i = 1; i <= T; ++i) {
            for (int[] clip : clips) {
                if (clip[0] < i && i <= clip[1]) {
                    dp[i] = Math.min(dp[clip[0]] + 1, dp[i]);
                }
            }
        }
        return dp[T] == Integer.MAX_VALUE - 1 ? -1 : dp[T];
    }
}
~~~

击败22%

### Q[162寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

难度 中等

峰值元素是指其值大于左右相邻值的元素。

给定一个输入数组 `nums`，其中 `nums[i] ≠ nums[i+1]`，找到峰值元素并返回其索引。

数组可能包含多个峰值，在这种情况下，返回任何一个峰值所在位置即可。

你可以假设 `nums[-1] = nums[n] = -∞`。

**示例 1:**

```
输入: nums = [1,2,3,1]
输出: 2
解释: 3 是峰值元素，你的函数应该返回其索引 2。
```

**示例 2:**

```
输入: nums = [1,2,1,3,5,6,4]
输出: 1 或 5 
解释: 你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。
```

**说明:**

你的解法应该是 *O*(*logN*) 时间复杂度的。

指针所处的位置，不是在上升，就是在下降。而不管是处于上升段，还是下降段。我们都可以找到一个峰值



标答使用递归写的分治法，非常漂亮

~~~java
class Solution {
    public int findPeakElement(int[] nums) {
        return merge(nums, 0, nums.length - 1);
    }

    public int merge(int[] nums, int l, int r) {
        if (l == r)
            return l;
        int mid = l + (r - l) / 2;
        if (nums[mid] > nums[mid + 1])
            return merge(nums, l, mid);
        return merge(nums, mid + 1, r);
    }
}
~~~

击败100%

### Q[845数组中的最长山脉](https://leetcode-cn.com/problems/longest-mountain-in-array/)

难度 中等

我们把数组 A 中符合下列属性的任意连续子数组 B 称为 “*山脉”*：

- `B.length >= 3`
- 存在 `0 < i < B.length - 1` 使得 `B[0] < B[1] < ... B[i-1] < B[i] > B[i+1] > ... > B[B.length - 1]`

（注意：B 可以是 A 的任意子数组，包括整个数组 A。）

给出一个整数数组 `A`，返回最长 *“山脉”* 的长度。

如果不含有 “*山脉”* 则返回 `0`。

 

**示例 1：**

```
输入：[2,1,4,7,3,2,5]
输出：5
解释：最长的 “山脉” 是 [1,4,7,3,2]，长度为 5。
```

**示例 2：**

```
输入：[2,2,2]
输出：0
解释：不含 “山脉”。
```

 

**提示：**

1. `0 <= A.length <= 10000`
2. `0 <= A[i] <= 10000`



利用滑窗，双指针，分为两种情况，上坡，下坡。我们需要三种状态。

- 若一开始为下坡/平，则为-1状态，此时需经过上坡，再经过下坡
- 若为上坡，则为0状态，需找到下坡。遇到平，则跳转至-1状态
- 若为下坡，则为1状态，此时开始计算。如果遇到平，转为-1状态。遇到上坡转为0状态。

~~~java
class Solution {
    public int longestMountain(int[] A) {
        if (A.length < 3) return 0;
        int left = 0;
        int right = 1;
        int max = 0;
        int flag = A[left] >= A[right] ? -1 : 0;
        while (right < A.length) {
            if (flag == -1 && A[right - 1] >= A[right]) {
                left = right;
                right++;
            } else if (flag == -1) {
                flag = 0;
                right++;
            } else if (flag == 0 && A[right - 1] < A[right]) {
                right++;
            } else if (flag == 0 && A[right - 1] != A[right]) {
                max = Math.max(max, right - left + 1);
                flag = 1;
                right++;
            } else if (flag == 0 && A[right - 1] == A[right]) {
                left = right;
                right++;
                flag = -1;
            } else if (flag == 1 && A[right - 1] > A[right]) {
                max = Math.max(max, right - left + 1);
                right++;
            } else if (flag == 1 && A[right - 1] == A[right]) {
                left = right;
                right++;
                flag = -1;
            } else {
                left = right - 1;
                right++;
                flag = 0;
            }
        }
        return max;
    }
}
~~~

击败69%，六发

### Q[5546按键持续时间最长的键](https://leetcode-cn.com/problems/slowest-key/)

难度 简单

LeetCode 设计了一款新式键盘，正在测试其可用性。测试人员将会点击一系列键（总计 `n` 个），每次一个。

给你一个长度为 `n` 的字符串 `keysPressed` ，其中 `keysPressed[i]` 表示测试序列中第 `i` 个被按下的键。`releaseTimes` 是一个升序排列的列表，其中 `releaseTimes[i]` 表示松开第 `i` 个键的时间。字符串和数组的 **下标都从 0 开始** 。第 `0` 个键在时间为 `0` 时被按下，接下来每个键都 **恰好** 在前一个键松开时被按下。

测试人员想要找出按键 **持续时间最长** 的键。第 `i` 次按键的持续时间为 `releaseTimes[i] - releaseTimes[i - 1]` ，第 `0` 次按键的持续时间为 `releaseTimes[0]` 。

注意，测试期间，同一个键可以在不同时刻被多次按下，而每次的持续时间都可能不同。

请返回按键 **持续时间最长** 的键，如果有多个这样的键，则返回 **按字母顺序排列最大** 的那个键。

 

**示例 1：**

```
输入：releaseTimes = [9,29,49,50], keysPressed = "cbcd"
输出："c"
解释：按键顺序和持续时间如下：
按下 'c' ，持续时间 9（时间 0 按下，时间 9 松开）
按下 'b' ，持续时间 29 - 9 = 20（松开上一个键的时间 9 按下，时间 29 松开）
按下 'c' ，持续时间 49 - 29 = 20（松开上一个键的时间 29 按下，时间 49 松开）
按下 'd' ，持续时间 50 - 49 = 1（松开上一个键的时间 49 按下，时间 50 松开）
按键持续时间最长的键是 'b' 和 'c'（第二次按下时），持续时间都是 20
'c' 按字母顺序排列比 'b' 大，所以答案是 'c'
```

**示例 2：**

```
输入：releaseTimes = [12,23,36,46,62], keysPressed = "spuda"
输出："a"
解释：按键顺序和持续时间如下：
按下 's' ，持续时间 12
按下 'p' ，持续时间 23 - 12 = 11
按下 'u' ，持续时间 36 - 23 = 13
按下 'd' ，持续时间 46 - 36 = 10
按下 'a' ，持续时间 62 - 46 = 16
按键持续时间最长的键是 'a' ，持续时间 16
```

 

**提示：**

- `releaseTimes.length == n`
- `keysPressed.length == n`
- `2 <= n <= 1000`
- `0 <= releaseTimes[i] <= 109`
- `releaseTimes[i] < releaseTimes[i+1]`
- `keysPressed` 仅由小写英文字母组成

此题为第212场周赛第一题。第一次做周赛题。四题写出了两题。第三题Q5548，本以为为动态规划，周赛时，以为自己哪里细节搞错了，其实思路是错了。最终为782名

这道题题干很长，其实很简单。保存最大值，与其对应字母，一次遍历，每次计算持续时间，若大于，则更新。若等于，则比较字母

~~~java
class Solution {
    public char slowestKey(int[] releaseTimes, String keysPressed) {
        int max = releaseTimes[0];
        char ch = keysPressed.charAt(0);
        if(keysPressed.length()==1)
            return ch;
        for(int i = 1; i < keysPressed.length(); i++){
            if(releaseTimes[i]-releaseTimes[i-1]==max){
                if(keysPressed.charAt(i)>ch){
                    ch = keysPressed.charAt(i);
                }
            }else if(releaseTimes[i]-releaseTimes[i-1]>max){
                max = releaseTimes[i]-releaseTimes[i-1];
                ch = keysPressed.charAt(i);
            }
        }
        return ch;
    }
}
~~~

### Q[5547等差子数组](https://leetcode-cn.com/problems/arithmetic-subarrays/)

难度 中等

如果一个数列由至少两个元素组成，且每两个连续元素之间的差值都相同，那么这个序列就是 **等差数列** 。更正式地，数列 `s` 是等差数列，只需要满足：对于每个有效的 `i` ， `s[i+1] - s[i] == s[1] - s[0]` 都成立。

例如，下面这些都是 **等差数列** ：

```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

下面的数列 **不是等差数列** ：

```
1, 1, 2, 5, 7
```

给你一个由 `n` 个整数组成的数组 `nums`，和两个由 `m` 个整数组成的数组 `l` 和 `r`，后两个数组表示 `m` 组范围查询，其中第 `i` 个查询对应范围 `[l[i], r[i]]` 。所有数组的下标都是 **从 0 开始** 的。

返回 `boolean` 元素构成的答案列表 `answer` 。如果子数组 `nums[l[i]], nums[l[i]+1], ... , nums[r[i]]` 可以 **重新排列** 形成 **等差数列** ，`answer[i]` 的值就是 `true`；否则`answer[i]` 的值就是 `false` 。

 

**示例 1：**

```
输入：nums = [4,6,5,9,3,7], l = [0,0,2], r = [2,3,5]
输出：[true,false,true]
解释：
第 0 个查询，对应子数组 [4,6,5] 。可以重新排列为等差数列 [6,5,4] 。
第 1 个查询，对应子数组 [4,6,5,9] 。无法重新排列形成等差数列。
第 2 个查询，对应子数组 [5,9,3,7] 。可以重新排列为等差数列 [3,5,7,9] 。
```

**示例 2：**

```
输入：nums = [-12,-9,-3,-12,-6,15,20,-25,-20,-15,-10], l = [0,1,6,4,8,7], r = [4,4,9,7,9,10]
输出：[false,true,false,false,true,true]
```

 

**提示：**

- `n == nums.length`
- `m == l.length`
- `m == r.length`
- `2 <= n <= 500`
- `1 <= m <= 500`
- `0 <= l[i] < r[i] < n`
- `-105 <= nums[i] <= 105`

想不出其他办法，遂暴力法。每次排个序。竟然也过了

~~~java
class Solution {
    public List<Boolean> checkArithmeticSubarrays(int[] nums, int[] l, int[] r) {
        List<Boolean> ans = new ArrayList<>();
        for(int i = 0; i < l.length; i++){
            int[] x = Arrays.copyOfRange(nums,l[i],r[i]+1);
            Arrays.sort(x);
            int cha = x[1]-x[0];
            boolean flag = true;
            for(int j = 2; j < x.length; j++){
                if(x[j]-x[j-1]!=cha){
                    ans.add(false);
                    flag =false;
                    break;
                }
            }
            if(flag)
                ans.add(true);
        }
        return ans;
    }
}
~~~

### Q[5548最小体力消耗路径](https://leetcode-cn.com/problems/path-with-minimum-effort/)

难度 中等

你准备参加一场远足活动。给你一个二维 `rows x columns` 的地图 `heights` ，其中 `heights[row][col]` 表示格子 `(row, col)` 的高度。一开始你在最左上角的格子 `(0, 0)` ，且你希望去最右下角的格子 `(rows-1, columns-1)` （注意下标从 **0** 开始编号）。你每次可以往 **上**，**下**，**左**，**右** 四个方向之一移动，你想要找到耗费 **体力** 最小的一条路径。

一条路径耗费的 **体力值** 是路径上相邻格子之间 **高度差绝对值** 的 **最大值** 决定的。

请你返回从左上角走到右下角的最小 **体力消耗值** 。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex1.png)

```
输入：heights = [[1,2,2],[3,8,2],[5,3,5]]
输出：2
解释：路径 [1,3,5,3,5] 连续格子的差值绝对值最大为 2 。
这条路径比路径 [1,2,2,2,5] 更优，因为另一条路劲差值最大值为 3 。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex2.png)

```
输入：heights = [[1,2,3],[3,8,4],[5,3,5]]
输出：1
解释：路径 [1,2,3,4,5] 的相邻格子差值绝对值最大为 1 ，比路径 [1,3,5,3,5] 更优。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex3.png)

```
输入：heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
输出：0
解释：上图所示路径不需要消耗任何体力。
```

 

**提示：**

- `rows == heights.length`
- `columns == heights[i].length`
- `1 <= rows, columns <= 100`
- `1 <= heights[i][j] <= 106`

周赛的时候，只想着斜线dp一次就好了。第三个样例一直过不了。

下午看到题解，只要一直dp，就好了

~~~java
class Solution {
    public int minimumEffortPath(int[][] heights) {
        int rowlength = heights.length;
        int collength = heights[0].length;
        int[][] dp = new int[heights.length][heights[0].length];
        for (int i = 0; i < heights.length; i++)
            for (int j = 0; j < heights[0].length; j++)
                dp[i][j] = Integer.MAX_VALUE;
        dp[0][0] = 0;
        boolean flag = true;
        while (flag) {
            flag = false;
            for (int x = 0; x < rowlength; x++) {
                for (int y = 0; y < collength; y++) {
                    int pre = dp[x][y];
                    if (x - 1 >= 0)
                        dp[x][y] = Math.min(dp[x][y], Math.max(Math.abs(heights[x][y] - heights[x-1][y]), dp[x-1][y]));
                    if(x+1<rowlength)
                        dp[x][y] = Math.min(dp[x][y], Math.max(Math.abs(heights[x][y] - heights[x+1][y]), dp[x+1][y]));
                    if (y - 1 >= 0)
                        dp[x][y] = Math.min(dp[x][y], Math.max(Math.abs(heights[x][y] - heights[x][y - 1]), dp[x][y - 1]));
                    if(y+1<collength)
                        dp[x][y] = Math.min(dp[x][y], Math.max(Math.abs(heights[x][y] - heights[x][y + 1]), dp[x][y + 1]));
                    flag = dp[x][y] != pre || flag;
                }
            }
        }
        return dp[rowlength - 1][collength - 1];
    }
}
~~~

估计没啥人提交，击败双百

### Q[297二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

难度 困难

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

**示例:** 

```
你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```

**提示:** 这与 LeetCode 目前使用的方式一致，详情请参阅 [LeetCode 序列化二叉树的格式](https://leetcode-cn.com/faq/#binary-tree)。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。

**说明:** 不要使用类的成员 / 全局 / 静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。

我自己写了一个，超时过不了，仿照标答写

~~~java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        String s = "";
        return ser(root,s);
    }
    public String ser(TreeNode root, String s){
        if(root == null){
            return s+"null,";
        }
        s+=(""+root.val+",");
        s=ser(root.left,s);
        s=ser(root.right,s);
        return s;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        ArrayList<String> l = new ArrayList<String>(Arrays.asList(data.split(",")));
        return de(l);
    }
    public TreeNode de(ArrayList<String> l){
        if(l.size() == 0){
            return null;
        }
        if(l.get(0).compareTo("null")==0){
            l.remove(0);
            return null;
        }
        TreeNode root = new TreeNode(Integer.valueOf(l.get(0)));
        l.remove(0);
        root.left = de(l);
        root.right = de(l);
        return root;
    }
}
~~~

击败5%

### Q[380常数时间插入、删除和获取随机元素](https://leetcode-cn.com/problems/insert-delete-getrandom-o1/)

难度中等214

设计一个支持在*平均* 时间复杂度 **O(1)** 下，执行以下操作的数据结构。

1. `insert(val)`：当元素 val 不存在时，向集合中插入该项。
2. `remove(val)`：元素 val 存在时，从集合中移除该项。
3. `getRandom`：随机返回现有集合中的一项。每个元素应该有**相同的概率**被返回。

**示例 :**

```
// 初始化一个空的集合。
RandomizedSet randomSet = new RandomizedSet();

// 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomSet.insert(1);

// 返回 false ，表示集合中不存在 2 。
randomSet.remove(2);

// 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomSet.insert(2);

// getRandom 应随机返回 1 或 2 。
randomSet.getRandom();

// 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomSet.remove(1);

// 2 已在集合中，所以返回 false 。
randomSet.insert(2);

// 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
randomSet.getRandom();
```

利用set完成

~~~java
class RandomizedSet {
    Set<Integer> set;

    /** Initialize your data structure here. */
    public RandomizedSet() {
        set = new HashSet<>();
    }

    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        return set.add(val);
    }

    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        return set.remove(val);
    }

    /** Get a random element from the set. */
    public int getRandom() {
        Random r = new Random();
        int pos = r.nextInt(set.size());
        Integer[] b = new Integer[set.size()];
        set.toArray(b);
        return b[pos];
    }
}
~~~

击败8%

### Q[171Excel表列序号](https://leetcode-cn.com/problems/excel-sheet-column-number/)

难度 简单

给定一个Excel表格中的列名称，返回其相应的列序号。

例如，

```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

**示例 1:**

```
输入: "A"
输出: 1
```

**示例 2:**

```
输入: "AB"
输出: 28
```

**示例 3:**

```
输入: "ZY"
输出: 701
```

**致谢：**
特别感谢 [@ts](http://leetcode.com/discuss/user/ts) 添加此问题并创建所有测试用例。

这题只要读懂就会了，可以看成26进制

~~~java
class Solution {
    public int titleToNumber(String s) {
        int sum = 0;
        for(int i = 0; i < s.length(); i++){
            sum = sum*26;
            sum+= (s.charAt(i)-'A'+1);
        }
        return sum;
    }
}
~~~

击败100%

### Q[166分数到小数](https://leetcode-cn.com/problems/fraction-to-recurring-decimal/)

难度 中等

给定两个整数，分别表示分数的分子 `numerator` 和分母 `denominator`，以 **字符串形式返回小数** 。

如果小数部分为循环小数，则将循环的部分括在括号内。

如果存在多个答案，只需返回 **任意一个** 。

对于所有给定的输入，**保证** 答案字符串的长度小于 `104` 。

 

**示例 1：**

```
输入：numerator = 1, denominator = 2
输出："0.5"
```

**示例 2：**

```
输入：numerator = 2, denominator = 1
输出："2"
```

**示例 3：**

```
输入：numerator = 2, denominator = 3
输出："0.(6)"
```

**示例 4：**

```
输入：numerator = 4, denominator = 333
输出："0.(012)"
```

**示例 5：**

```
输入：numerator = 1, denominator = 5
输出："0.2"
```

 

**提示：**

- `-231 <= numerator, denominator <= 231 - 1`
- `denominator != 0`

又当了抄题家。一位位计算，每次计算出一个余数，检查余数是否出现过。若有相同余数，则出现循环（循环在两个重复值之间）。

因为Math.abs(int)被卡了int极限值

~~~java
class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        HashMap<Long, Integer> map = new HashMap<>();
        StringBuilder s = new StringBuilder();
        long num = Math.abs((long)numerator);
        long den = Math.abs((long) denominator);
        if ((numerator < 0 && denominator > 0) || (numerator > 0 && denominator < 0))
            s.append("-");
        s.append(num / den);
        if (num % den == 0) {
            return s.toString();
        }
        s.append('.');
        num %= den;
        while (num != 0) {
            if (map.containsKey(num)) {
                int p = map.get(num);
                s.insert(p, '(');
                s.append(')');
                break;
            }
            map.put(num, s.length());
            num *= 10;
            s.append(num/den);
            num %= den;
        }
        return s.toString();
    }
}
~~~

击败100%

### Q[150逆波兰表达式求值](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)

难度 中等

根据[ 逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437)，求表达式的值。

有效的运算符包括 `+`, `-`, `*`, `/` 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

 

**说明：**

- 整数除法只保留整数部分。
- 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

 

**示例 1：**

```
输入: ["2", "1", "+", "3", "*"]
输出: 9
解释: 该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```

**示例 2：**

```
输入: ["4", "13", "5", "/", "+"]
输出: 6
解释: 该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
```

**示例 3：**

```
输入: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
输出: 22
解释: 
该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

 

**逆波兰表达式：**

逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

- 平常使用的算式则是一种中缀表达式，如 `( 1 + 2 ) * ( 3 + 4 )` 。
- 该算式的逆波兰表达式写法为 `( ( 1 2 + ) ( 3 4 + ) * )` 。

逆波兰表达式主要有以下两个优点：

- 去掉括号后表达式无歧义，上式即便写成 `1 2 + 3 4 + * `也可以依据次序计算出正确结果。
- 适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中。

没什么难度

~~~java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        for(String s: tokens){
            if(s.length()==1&&s.charAt(0) == '+'){
                int x = stack.pop();
                int y = stack.pop();
                stack.push(x+y);
            }
            else if(s.length()==1&&s.charAt(0) == '-'){
                int x = stack.pop();
                int y = stack.pop();
                stack.push(y-x);
            }else if(s.charAt(0) == '*'){
                int x = stack.pop();
                int y = stack.pop();
                stack.push(y*x);
            }else if(s.charAt(0) == '/'){
                int x = stack.pop();
                int y = stack.pop();
                stack.push(y/x);
            }else {
                stack.push(Integer.valueOf(s));
            }
        }
        return stack.pop();
    }
}
~~~

击败62%

### Q[454四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)

难度 中等

给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 `(i, j, k, l)` ，使得 `A[i] + B[j] + C[k] + D[l] = 0`。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -228 到 228 - 1 之间，最终结果不会超过 231 - 1 。

**例如:**

```
输入:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

输出:
2

解释:
两个元组如下:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

先用两个数建个map，然后再用另两个数取值

~~~java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        HashMap<Integer,Integer> map1 = new HashMap<>();
        HashMap<Integer,Integer> map2 = new HashMap<>();
        int count = 0;
        for(int i = 0; i < A.length; i++){
            for(int j = 0; j < B.length; j++){
                map1.put(A[i]+B[j],map1.getOrDefault(A[i]+B[j],0)+1);
            }
        }
        for(int i = 0; i < C.length; i++){
            for(int j = 0; j < D.length; j++){
                count+=map1.getOrDefault(-C[i]-D[j],0);    
            }
        }return count;
    }
}
~~~

击败95%

### Q[289生命游戏](https://leetcode-cn.com/problems/game-of-life/)

难度 中等

根据 [百度百科](https://baike.baidu.com/item/生命游戏/2926434?fr=aladdin) ，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。

给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态：1 即为活细胞（live），或 0 即为死细胞（dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

1. 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
2. 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
3. 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
4. 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

根据当前状态，写一个函数来计算面板上所有细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

 

**示例：**

```
输入： 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
输出：
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

 

**进阶：**

- 你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。
- 本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？

如果想节省内存，就得搞两个中间态，我没搞

~~~java
class Solution {
    public void gameOfLife(int[][] board) {
        int[][] b = new int[board.length][board[0].length];
        for (int i = 0; i < board.length; i++)
            for (int j = 0; j < board[0].length; j++)
                b[i][j] = board[i][j];
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                int c = count(b, i, j);
                if (b[i][j] == 1) {
                    if (c < 2 || c > 3)
                        board[i][j] = 0;
                } else {
                    if (c == 3)
                        board[i][j] = 1;
                }
            }
        }
    }

    public int count(int[][] board, int x, int y) {
        int c = 0;
        int n1 = board.length;
        int n2 = board[0].length;
        if (x - 1 >= 0 && y - 1 >= 0 && board[x - 1][y - 1] == 1)
            c++;
        if (x - 1 >= 0 && board[x - 1][y] == 1)
            c++;
        if (y - 1 >= 0 && board[x][y - 1] == 1)
            c++;
        if (x + 1 < n1 && y + 1 < n2 && board[x + 1][y + 1] == 1)
            c++;
        if (x + 1 < n1 && board[x + 1][y] == 1)
            c++;
        if (y + 1 < n2 && board[x][y + 1] == 1)
            c++;
        if (x + 1 < n1 && y - 1 >= 0 && board[x + 1][y - 1] == 1)
            c++;
        if (x - 1 >= 0 && y + 1 < n2 && board[x - 1][y + 1] == 1)
            c++;
        return c;
    }
}
~~~

100% 91%，也没多花多少内存

### Q[41缺失的第一个正数](https://leetcode-cn.com/problems/first-missing-positive/)

难度 困难

给你一个未排序的整数数组，请你找出其中没有出现的最小的正整数。

 

**示例 1:**

```
输入: [1,2,0]
输出: 3
```

**示例 2:**

```
输入: [3,4,-1,1]
输出: 2
```

**示例 3:**

```
输入: [7,8,9,11,12]
输出: 1
```

 

**提示：**

你的算法的时间复杂度应为O(*n*)，并且只能使用常数级别的额外空间。

看了一个题解，写的是原地哈希法。

把数组中值为i的存到i-1的位置。

第四行的while很关键。如果写if就会把换过来的漏掉

~~~java
class Solution {
    public int firstMissingPositive(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            while(nums[i]-1<nums.length&&nums[i]-1>=0&& nums[nums[i] - 1] != nums[i]) {
                int x = nums[i];
                int temp = nums[x-1];
                nums[x-1] = x;
                nums[i] = temp;
            }
        }

        for(int i = 0; i < nums.length;i++){
            if(nums[i] != i+1)
                return i+1;
        }
        return nums.length+1;
    }
}
~~~

击败100%

### Q[剑指Offer03数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

难度简单203

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**示例 1：**

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

 

**限制：**

```
2 <= n <= 100000
```

上一题修改修改

~~~java
class Solution {
    public int findRepeatNumber(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            while(nums[i]<nums.length&&nums[i]>=0&& nums[nums[i]] != nums[i]) {
                int x = nums[i];
                int temp = nums[x];
                nums[x] = x;
                nums[i] = temp;
            }
        }
        for(int i = 0; i < nums.length; i++)
            if(nums[i]!=i)
                return nums[i];
        return -1;
    }
}
~~~

击败68%，空间96%

### Q[1207独一无二的出现次数](https://leetcode-cn.com/problems/unique-number-of-occurrences/)

难度 简单

给你一个整数数组 `arr`，请你帮忙统计数组中每个数的出现次数。

如果每个数的出现次数都是独一无二的，就返回 `true`；否则返回 `false`。

 

**示例 1：**

```
输入：arr = [1,2,2,1,1,3]
输出：true
解释：在该数组中，1 出现了 3 次，2 出现了 2 次，3 只出现了 1 次。没有两个数的出现次数相同。
```

**示例 2：**

```
输入：arr = [1,2]
输出：false
```

**示例 3：**

```
输入：arr = [-3,0,1,-3,1,1,1,-3,10,0]
输出：true
```

 

**提示：**

- `1 <= arr.length <= 1000`
- `-1000 <= arr[i] <= 1000`

一次map，一次set

~~~cpp
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        map<int, int> m;
        for (auto i:arr) {
            if(m.find(i) == m.end())
                m.insert(pair<int,int>(i,1));
            else
                m[i] = m[i]+1;
        }
        set<int> s;
        for(auto i:m) {
            if(s.find(i.second)!=s.end())
                return false;
            s.insert(i.second);
        }
        return true;
    }
};
~~~

击败47%

### Q[128最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

难度 困难

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

 

**进阶：**

- 你可以设计并实现时间复杂度为 `O(n)` 的解决方案吗？

 

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

 

**提示：**

- `0 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

一个set，然后一直去查下一个元素在不在

然后超时了。

应该检查前一个在不在。

~~~cpp
class Solution
{
public:
    int longestConsecutive(vector<int> &nums)
    {
        int max = 0;
        unordered_map<int, int> m;
        for (int i : nums)
        {
            m.insert(pair<int, int>(i, 1));
        }
        for (auto i : m)
        {
            long t = i.first;
            if (m.find(t - 1) == m.end())
                while (m.find(t) != m.end())
                {
                    t++;
                    if (t == 2147483648)
                        break;
                }
            max = max > (t - i.first) ? max : (t - i.first);
        }
        return max;
    }
};
~~~

击败51%

### Q[227基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

难度 中等

实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，`+`， `-` ，`*`，`/` 四种运算符和空格 ` `。 整数除法仅保留整数部分。

**示例 1:**

```
输入: "3+2*2"
输出: 7
```

**示例 2:**

```
输入: " 3/2 "
输出: 1
```

**示例 3:**

```
输入: " 3+5 / 2 "
输出: 5
```

**说明：**

- 你可以假设所给定的表达式都是有效的。
- 请**不要**使用内置的库函数 `eval`。

用栈

~~~cpp
class Solution
{
public:
    int calculate(string s)
    {
        stack<int> s1;
        stack<char> s2;
        int temp = 0;
        for (int i = 0; i < s.length(); i++)
        {
            if (s[i] == ' ')
                continue;
            else if (s[i] >= '0' && s[i] <= '9')
            {
                temp = 10 * temp + (s[i] - '0');
            }
            else
            {
                s1.push(temp);
                temp = 0;
                if (s[i] == '+' || s[i] == '-' || s[i] == '*' || s[i] == '/')
                {
                    while (s2.size()!=0&&!((s2.top()=='+'||s2.top()=='-')&&(s[i]=='*'||s[i]=='/')))
                    {
                        r(s1, s2);
                    }
                    s2.push(s[i]);
                }
            }
        }
        s1.push(temp);
        while (s2.size() != 0)
        {
            r(s1, s2);
        }
        return s1.top();
    }
    void r(stack<int> &s1, stack<char> &s2)
    {
        char ch = s2.top();
        s2.pop();
        int x1 = s1.top();
        s1.pop();
        int x2 = s1.top();
        s1.pop();
        if (ch == '+')
            s1.push(x2 + x1);
        else if (ch == '-')
            s1.push(x2 - x1);
        else if (ch == '*')
            s1.push(x2 * x1);
        else if (ch == '/')
            s1.push(x2 / x1);
    }
};
~~~

击败28%

### Q[129求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

难度 中等

给定一个二叉树，它的每个结点都存放一个 `0-9` 的数字，每条从根到叶子节点的路径都代表一个数字。

例如，从根到叶子节点路径 `1->2->3` 代表数字 `123`。

计算从根到叶子节点生成的所有数字之和。

**说明:** 叶子节点是指没有子节点的节点。

**示例 1:**

```
输入: [1,2,3]
    1
   / \
  2   3
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.
```

**示例 2:**

```
输入: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
输出: 1026
解释:
从根到叶子节点路径 4->9->5 代表数字 495.
从根到叶子节点路径 4->9->1 代表数字 491.
从根到叶子节点路径 4->0 代表数字 40.
因此，数字总和 = 495 + 491 + 40 = 1026.
```

没啥好说的

~~~cpp
class Solution
{
public:
    int sumNumbers(TreeNode *root)
    {
        if(root == nullptr)
            return 0;
        return val(root, 0);
    }
    int val(TreeNode *root, int x)
    {
        if (root->left == NULL && root->right == NULL)
            return x * 10 + root->val;
        x *= 10;
        x += root->val;
        int left = 0;
        int right = 0;
        if (root->right != NULL)
            right = val(root->right, x);
        if (root->left != NULL)
            left = val(root->left, x);
        return left + right;
    }
};
~~~

击败35%

### Q[239滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

难度 困难

给定一个数组 *nums*，有一个大小为 *k* 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 *k* 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

 

**进阶：**

你能在线性时间复杂度内解决此题吗？

 

**示例:**

```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

 

**提示：**

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `1 <= k <= nums.length`

窗口，左窗 右窗，窗口大小固定。每次从右加一个点

用双端队列。建立一个递减数组，如果当前元素大于队尾，弹出队尾。只要比当前小的数都用不上了。因为队尾的下标比当前元素还小

每次遍历，看队首是什么元素

~~~java
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> q;
        q.push_back(0);
        vector<int> v;
        for(int i = 1; i < nums.size()&&i < k;++i){
            while(q.size()>0&&nums[q[q.size()-1]]<nums[i]){
                q.pop_back();
            }
            if(q.size()==0||nums[q[q.size()-1]]>=nums[i]){
                q.push_back(i);
            }
        }
        v.push_back(nums[q[0]]);
        for(int i = 1; i+k-1<nums.size();++i){
            while(q.size()>0&&nums[q[q.size()-1]]<nums[i+k-1]){
                q.pop_back();
            }
            if(q.size()==0||nums[q[q.size()-1]]>=nums[i+k-1]){
                q.push_back(i+k-1);
            }
            if(q[0]<i){
                q.pop_front();
            }
            v.push_back(nums[q[0]]);
        }
        return v;
    }
};
~~~

击败5%

### Q[76最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

难度 困难

给你一个字符串 S、一个字符串 T 。请你设计一种算法，可以在 O(n) 的时间复杂度内，从字符串 S 里面找出：包含 T 所有字符的最小子串。

 

**示例：**

```
输入：S = "ADOBECODEBANC", T = "ABC"
输出："BANC"
```

 

**提示：**

- 如果 S 中不存这样的子串，则返回空字符串 `""`。
- 如果 S 中存在这样的子串，我们保证它是唯一的答案。

建立滑窗，没啥好方法。left right，left每次向右移一个，然后找right最小值

用两个map来存

~~~cpp
class Solution
{
public:
    unordered_map<char,int> m1,m2;
    string minWindow(string s, string t)
    {
        for (auto &c : t)
        {
            m1[c]++;
        }
        int left = 0;
        int right = 0;
        int minLength = INT_MAX;
        int mleft = 0;
        int mright = 0;
        m2[s[0]]++;
        while(left+t.size()-1<s.size()){
            while(!check()&&right<s.size()){
                right++;
                m2[s[right]]++;
            }
            if(right<s.size()&&(right-left+1)<minLength){
                minLength = right-left+1;
                mleft = left;
                mright = right;
            }
            m2[s[left]]--;
            left++;
        }
        return minLength == INT_MAX?"":s.substr(mleft,minLength);
    }
    bool check(){
        for(auto &c:m1){
            if(m2[c.first]<c.second)
                return false; 
        }
        return true;
    }
};
~~~

击败8%

### Q[463岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

难度 简单

给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。

网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

 

**示例 :**

```
输入:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

输出: 16

解释: 它的周长是下面图片中的 16 个黄色的边：
```

题目已经降维了，明确说只有一个岛。只要遍历一遍就好了

~~~cpp
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int ans = 0;
        for(int i = 0; i < grid.size(); ++i) {
            for(int j = 0; j < grid[i].size(); ++j){
                if(grid[i][j] == 1){
                    int temp = 4;
                    if(i-1>=0&&grid[i-1][j] == 1)
                        temp--;
                    if(j-1>=0&&grid[i][j-1] == 1)
                        --temp;
                    if(i+1<grid.size()&&grid[i+1][j] == 1)
                        --temp;
                    if(j+1<grid[i].size()&&grid[i][j+1]==1)
                        --temp;
                    ans += temp;
                }
            }
        }
        return ans;
    }
};
~~~

击败41%

### Q[127单词接龙](https://leetcode-cn.com/problems/word-ladder/)

难度 中等

给定两个单词（*beginWord* 和 *endWord*）和一个字典，找到从 *beginWord* 到 *endWord* 的最短转换序列的长度。转换需遵循如下规则：

1. 每次转换只能改变一个字母。
2. 转换过程中的中间单词必须是字典中的单词。

**说明:**

- 如果不存在这样的转换序列，返回 0。
- 所有单词具有相同的长度。
- 所有单词只由小写字母组成。
- 字典中不存在重复的单词。
- 你可以假设 *beginWord* 和 *endWord* 是非空的，且二者不相同。

**示例 1:**

```
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。
```

**示例 2:**

```
输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。
```

刚开始写了个暴力法，应该用bfs

~~~cpp
class Solution
{
public:
    int ladderLength(string beginWord, string endWord, vector<string> &wordList)
    {
        queue<string> word;
        queue<int> l;
        word.push(beginWord);
        l.push(1);
        vector<bool> flag(wordList.size(), false);
        while (word.size() > 0)
        {
            string p = word.front();
            word.pop();
            int length = l.front();
            l.pop();
            for (int i = 0; i < wordList.size(); ++i)
            {
                if (!flag[i])
                {
                    int count = 0;
                    bool f = true;
                    for (int j = 0; j < p.size(); ++j)
                    {
                        if (p[j] != wordList[i][j])
                        {
                            if (count == 1)
                            {
                                f = false;
                                break;
                            }
                            count = 1;
                        }
                    }
                    if (count == 1 && f == true)
                    {
                        if (wordList[i].compare(endWord) == 0)
                            return length + 1;
                        word.push(wordList[i]);
                        l.push(length + 1);
                        flag[i] = true;
                    }
                }
            }
        }
        return 0;
    }
};
~~~

击败9%

### Q[130被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

难度 中等

给定一个二维的矩阵，包含 `'X'` 和 `'O'`（**字母 O**）。

找到所有被 `'X'` 围绕的区域，并将这些区域里所有的 `'O'` 用 `'X'` 填充。

**示例:**

```
X X X X
X O O X
X X O X
X O X X
```

运行你的函数后，矩阵变为：

```
X X X X
X X X X
X X X X
X O X X
```

**解释:**

被围绕的区间不会存在于边界上，换句话说，任何边界上的 `'O'` 都不会被填充为 `'X'`。 任何不在边界上，或不与边界上的 `'O'` 相连的 `'O'` 最终都会被填充为 `'X'`。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

刚开始想着之间dfs，遇到边界返回真。但是这样很难传true给其他叶子

于是从四周开始dfs。只要是四周dfs到的，均保持原样

~~~cpp
class Solution
{
public:
    void solve(vector<vector<char>> &board)
    {
        if (board.size() == 0)
            return;
        vector<vector<bool>> flag(board.size(), vector<bool>(board[0].size(), false));
        for (int i = 0; i < board.size(); i++)
        {
            if (!flag[i][0] && board[i][0] == 'O')
            {
                flag[i][0] = true;
                dfs(flag, board, i, 0);
            }
            if (!flag[i][board[0].size() - 1] && board[i][board[0].size() - 1] == 'O')
            {
                flag[i][board[0].size() - 1] = true;
                dfs(flag, board, i, board[0].size() - 1);
            }
        }
        for (int j = 0; j < board[0].size(); ++j)
        {
            if (!flag[0][j] && board[0][j] == 'O')
            {
                flag[0][j] = true;
                dfs(flag, board, 0, j);
            }
            if (!flag[flag.size() - 1][j] && board[flag.size() - 1][j] == 'O')
            {
                flag[flag.size() - 1][j] = true;
                dfs(flag, board, flag.size() - 1, j);
            }
        }
        for (int i = 0; i < board.size(); i++)
        {
            for (int j = 0; j < board[i].size(); ++j)
            {
                if (flag[i][j])
                    board[i][j] = 'O';
                else
                    board[i][j] = 'X';
            }
        }
    }
    void dfs(vector<vector<bool>> &v, vector<vector<char>> &board, int x, int y)
    {
        if (x - 1 >= 0 && !v[x - 1][y] && board[x - 1][y] == 'O')
        {
            v[x - 1][y] = true;
            dfs(v, board, x - 1, y);
        }
        if (y - 1 >= 0 && !v[x][y - 1] && board[x][y - 1] == 'O')
        {
            v[x][y - 1] = true;
            dfs(v, board, x, y - 1);
        }
        if (x + 1 < board.size() && !v[x + 1][y] && board[x + 1][y] == 'O')
        {
            v[x + 1][y] = true;
            dfs(v, board, x + 1, y);
        }
        if (y + 1 < board[0].size() && !v[x][y + 1] && board[x][y + 1] == 'O')
        {
            v[x][y + 1] = true;
            dfs(v, board, x, y + 1);
        }
    }
};
~~~

击败47%

### Q[381O(1) 时间插入、删除和获取随机元素 - 允许重复](https://leetcode-cn.com/problems/insert-delete-getrandom-o1-duplicates-allowed/)

难度 困难

设计一个支持在*平均* 时间复杂度 **O(1)** 下**，** 执行以下操作的数据结构。

**注意: 允许出现重复元素。**

1. `insert(val)`：向集合中插入元素 val。
2. `remove(val)`：当 val 存在时，从集合中移除一个 val。
3. `getRandom`：从现有集合中随机获取一个元素。每个元素被返回的概率应该与其在集合中的数量呈线性相关。

**示例:**

```
// 初始化一个空的集合。
RandomizedCollection collection = new RandomizedCollection();

// 向集合中插入 1 。返回 true 表示集合不包含 1 。
collection.insert(1);

// 向集合中插入另一个 1 。返回 false 表示集合包含 1 。集合现在包含 [1,1] 。
collection.insert(1);

// 向集合中插入 2 ，返回 true 。集合现在包含 [1,1,2] 。
collection.insert(2);

// getRandom 应当有 2/3 的概率返回 1 ，1/3 的概率返回 2 。
collection.getRandom();

// 从集合中删除 1 ，返回 true 。集合现在包含 [1,2] 。
collection.remove(1);

// getRandom 应有相同概率返回 1 和 2 。
collection.getRandom();
```

这题真的挺困难。

建立一个vector< int> map< int, set< int>>用来存vector坐标，删除元素就把元素与最后一个交换。

要注意删除元素可能就是最后一个，此时不能在往集合里添加（删除里面的那个if困扰我很久）

~~~cpp
class RandomizedCollection
{
public:
    unordered_map<int, unordered_set<int>> m;
    vector<int> v;
    /** Initialize your data structure here. */
    RandomizedCollection()
    {
    }

    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    bool insert(int val)
    {
        v.push_back(val);
        m[val].insert(v.size() - 1);
        return m[val].size() == 1;
    }

    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    bool remove(int val)
    {
        if (m.find(val) == m.end())
            return false;
        int pos = *(m[val].begin());
        m[val].erase(pos);
        m[v[v.size() - 1]].erase(v.size() - 1);
        if (pos != v.size() - 1)
            m[v[v.size() - 1]].insert(pos);
        v[pos] = v[v.size() - 1];
        if (m[val].size() == 0)
        {
            m.erase(val);
        }
        v.pop_back();
        return true;
    }

    /** Get a random element from the collection. */
    int getRandom()
    {
        if (v.size() == 0)
            return -1;
        return v[rand() % v.size()];
    }
};
~~~

击败96%

### Q[834树中距离之和](https://leetcode-cn.com/problems/sum-of-distances-in-tree/)

难度 困难

给定一个无向、连通的树。树中有 `N` 个标记为 `0...N-1` 的节点以及 `N-1` 条边 。

第 `i` 条边连接节点 `edges[i][0]` 和 `edges[i][1]` 。

返回一个表示节点 `i` 与其他所有节点距离之和的列表 `ans`。

**示例 1:**

```
输入: N = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
输出: [8,12,6,10,10,10]
解释: 
如下为给定的树的示意图：
  0
 / \
1   2
   /|\
  3 4 5

我们可以计算出 dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5) 
也就是 1 + 1 + 2 + 2 + 2 = 8。 因此，answer[0] = 8，以此类推。
```

**说明:** `1 <= N <= 10000`

首先用dfs，算出root的值。之后每个结点，利用变化结点来计算。

以上面的树为例，树是固定的。计算出0为 1+1+2+2+2。当我们换到1时，1+2+3+3+3,非1的子孙叶距离全部+1。再看2时，1+1+1+1+2,如果叶子为2的子孙叶，则距离都-1，非子孙则都+1。

dp[ root] = dp [ parent] - 子孙个数 + 非子孙个数。

我们在第一次dfs即可算出每个结点子孙树。两次dfs即可解决这个问题

~~~cpp
class Solution
{
public:
    unordered_map<int, unordered_set<int>> tree;
    shared_ptr<vector<int>> s;

    vector<int> sumOfDistancesInTree(int N, vector<vector<int>> &edges)
    {
        s = make_shared<vector<int>>(N);
        for (auto edge : edges)
        {
            tree[edge[0]].insert(edge[1]);
            tree[edge[1]].insert(edge[0]);
        }
        map<int, int> m;
        m.insert(pair<int, int>(0, 1));
        int x = dfs(0, 0, m);
        vector<int> ans(N);
        ans[0] = x;
        for (auto it = tree[0].begin(); it != tree[0].end(); ++it)
        {
            if (ans[*it] == 0)
                d(*it, N, ans[0], ans);
        }
        return ans;
    }
    int dfs(int root, int length, map<int, int> &m)
    {
        int sum = 0;
        int leave = 0;
        for (auto it = tree[root].begin(); it != tree[root].end(); ++it)
        {
            if (m[*it]++ == 0)
            {
                sum += dfs(*it, length + 1, m);
                leave += ((*s)[*it] + 1);
            }
        }
        sum += length;
        (*s)[root] = leave;
        return sum;
    }
    void d(int root, int N, int sum, vector<int> &v)
    {
        int x = (*s)[root];
        int y = N - x - 2;
        v[root] = sum - x + y;
        int leave = 0;
        for (auto it = tree[root].begin(); it != tree[root].end(); ++it)
        {
            if (v[*it] == 0)
            {
                d(*it, N, v[root], v);
                leave += ((*s)[*it] + 1);
            }
        }
        (*s)[root] = leave;
    }
};
~~~

击败5%

### Q[124二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

难度 困难

给定一个**非空**二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。

 

**示例 1：**

```
输入：[1,2,3]

       1
      / \
     2   3

输出：6
```

**示例 2：**

```
输入：[-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

输出：42
```

这题不难，不需要看题解。只要在每个结点处理就好了。

一方面是求max，一方面是求返回值。max可以计算两路会合。而返回值只计算一路

~~~cpp
class Solution
{
public:
    int max;
    int maxPathSum(TreeNode *root)
    {
        if (root == NULL)
            return INT_MIN;
        max = root->val;
        dfs(root);
        return max;
    }
    int dfs(TreeNode *root)
    {
        if (root == NULL)
            return INT_MIN;
        int left = INT_MIN;
        int right = INT_MIN;
        int r = root->val;
        if (root->left != NULL)
        {
            left = dfs(root->left);
            max = left > max ? left : max;
            max = left+root->val>max?left+root->val:max;
            r = root->val+left>r?root->val+left:r;
        }
        if (root->right != NULL)
        {
            right = dfs(root->right);
            max = right > max ? right : max;
            max = right + root->val>max?right+root->val:max;
            r = root->val+right>r?root->val+right:r;
        }
        if (root->left != NULL && root->right != NULL)
            max = left + right + root->val > max ? left + right + root->val : max;
        return r;
    }
};
~~~

击败97%

### Q[547朋友圈](https://leetcode-cn.com/problems/friend-circles/)

难度 中等

班上有 **N** 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 A 是 B 的朋友，B 是 C 的朋友，那么我们可以认为 A 也是 C 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 **N \* N** 的矩阵 **M**，表示班级中学生之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j 个学生**互为**朋友关系，否则为不知道。你必须输出所有学生中的已知的朋友圈总数。

 

**示例 1：**

```
输入：
[[1,1,0],
 [1,1,0],
 [0,0,1]]
输出：2 
解释：已知学生 0 和学生 1 互为朋友，他们在一个朋友圈。
第2个学生自己在一个朋友圈。所以返回 2 。
```

**示例 2：**

```
输入：
[[1,1,0],
 [1,1,1],
 [0,1,1]]
输出：1
解释：已知学生 0 和学生 1 互为朋友，学生 1 和学生 2 互为朋友，所以学生 0 和学生 2 也是朋友，所以他们三个在一个朋友圈，返回 1 。
```

 

**提示：**

- `1 <= N <= 200`
- `M[i][i] == 1`
- `M[i][j] == M[j][i]`

并查集

~~~cpp
class Solution {
public:
    int count;
    int findCircleNum(vector<vector<int>>& M) {
        count = M.size(); 
        vector<int> v(M.size());
        for(int i = 0; i < M.size(); ++i){
            v[i] = i;
        }
        for(int i = 0; i < M.size(); ++i){
            for(int j = 0; j < M[i].size(); ++j){
                if(M[i][j] == 1){
                    un(i,j,v);
                }
            }
        }
        return count;
    }
    void un(int x,int y,vector<int> &v){
        int x1 = find(x,v);
        int y1 = find(y,v);
        if(x1 == y1){
            return;
        }
        v[x1] = v[y1];
        count--;
    }
    int find(int x,vector<int> &v){
        while(v[x] != x){
            v[x] = v[v[x]];
            x = v[x];
        }
        return x;
    }
};
~~~

击败73%

### Q[1619删除某些元素后的数组均值](https://leetcode-cn.com/problems/mean-of-array-after-removing-some-elements/)

难度简单3

给你一个整数数组 `arr` ，请你删除最小 `5%` 的数字和最大 `5%` 的数字后，剩余数字的平均值。

与 **标准答案** 误差在 `10-5` 的结果都被视为正确结果。

 

**示例 1：**

```
输入：arr = [1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,3]
输出：2.00000
解释：删除数组中最大和最小的元素后，所有元素都等于 2，所以平均值为 2 。
```

**示例 2：**

```
输入：arr = [6,2,7,5,1,2,0,3,10,2,5,0,5,5,0,8,7,6,8,0]
输出：4.00000
```

**示例 3：**

```
输入：arr = [6,0,7,0,7,5,7,8,3,4,0,7,8,1,6,8,1,1,2,4,8,1,9,5,4,3,8,5,10,8,6,6,1,0,6,10,8,2,3,4]
输出：4.77778
```

**示例 4：**

```
输入：arr = [9,7,8,7,7,8,4,4,6,8,8,7,6,8,8,9,2,6,0,0,1,10,8,6,3,3,5,1,10,9,0,7,10,0,10,4,1,10,6,9,3,6,0,0,2,7,0,6,7,2,9,7,7,3,0,1,6,1,10,3]
输出：5.27778
```

**示例 5：**

```
输入：arr = [4,8,4,10,0,7,1,3,7,8,8,3,4,1,6,2,1,1,8,0,9,8,0,3,9,10,3,10,1,10,7,3,2,1,4,9,10,7,6,4,0,8,5,1,2,1,6,2,5,0,7,10,9,10,3,7,10,5,8,5,7,6,7,6,10,9,5,10,5,5,7,2,10,7,7,8,2,0,1,1]
输出：5.29167
```

 

**提示：**

- `20 <= arr.length <= 1000`
- `arr.length` 是 `20` 的 **倍数** 
- `0 <= arr[i] <= 105`

~~~cpp
class Solution {
public:
    double trimMean(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        double sum = 0;
        double left = arr.size()*0.05,right =arr.size()*0.95;
        for(int i = left; i < right;++i){
            sum += arr[i];
        }
        return sum/(right-left);
    }
};
~~~

击败63%

### Q[5539按照频率将数组升序排序](https://leetcode-cn.com/problems/sort-array-by-increasing-frequency/)

难度 简单

给你一个整数数组 `nums` ，请你将数组按照每个值的频率 **升序** 排序。如果有多个值的频率相同，请你按照数值本身将它们 **降序** 排序。 

请你返回排序后的数组。

 

**示例 1：**

```
输入：nums = [1,1,2,2,2,3]
输出：[3,1,1,2,2,2]
解释：'3' 频率为 1，'1' 频率为 2，'2' 频率为 3 。
```

**示例 2：**

```
输入：nums = [2,3,1,3,2]
输出：[1,3,3,2,2]
解释：'2' 和 '3' 频率都为 2 ，所以它们之间按照数值本身降序排序。
```

**示例 3：**

```
输入：nums = [-1,1,-6,4,5,-6,1,4,1]
输出：[5,-1,4,4,-6,-6,1,1,1]
```

 

**提示：**

- `1 <= nums.length <= 100`
- `-100 <= nums[i] <= 100`

此题为第38场双周赛。这个双周赛写了三个/四个。排名430/2004。第一题罚时15min。第四题超时过不了

这题感觉其实挺难的...

先用cpp，没看清题意，还要按大小排序。

然而java我不会map遍历，临时学了一下

先用map记录次数，然后建立一个结构体数组，按照频率、大小排序

~~~java
class Solution {

    public int[] frequencySort(int[] nums) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i:nums)
            map.put(i,map.getOrDefault(i,0)+1);
        Iterator<Map.Entry<Integer, Integer>> iterator = map.entrySet().iterator();
        Stru[] s = new Stru[map.size()];
        int pos = 0;
        while(iterator.hasNext()){
            Map.Entry<Integer, Integer> entry = iterator.next();
            s[pos] = new Stru();
            s[pos].x = entry.getKey();
            s[pos].y = entry.getValue();
            pos++;
        }
        Arrays.sort(s, new Comparator<Stru>() {
            @Override
            public int compare(Stru o1, Stru o2) {
                if(o1.y-o2.y == 0){
                    return o2.x - o1.x;
                }
                return o1.y - o2.y;
            }
        });
        int j = 0;
        for(int i = 0; i < nums.length; i++){
            if(s[j].y!=0) {
                nums[i] = s[j].x;
                s[j].y--;
            }
            if(s[j].y==0)
                j++;
        }
        return nums;
    }
}
class Stru{
    public int x;
    public int y;
}
~~~

### Q[5540两点之间不包含任何点的最宽垂直面积](https://leetcode-cn.com/problems/widest-vertical-area-between-two-points-containing-no-points/)

难度 中等

给你 `n` 个二维平面上的点 `points` ，其中 `points[i] = [xi, yi]` ，请你返回两点之间内部不包含任何点的 **最宽垂直面积** 的宽度。

**垂直面积** 的定义是固定宽度，而 y 轴上无限延伸的一块区域（也就是高度为无穷大）。 **最宽垂直面积** 为宽度最大的一个垂直面积。

请注意，垂直区域 **边上** 的点 **不在** 区域内。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/31/points3.png)

```
输入：points = [[8,7],[9,9],[7,4],[9,7]]
输出：1
解释：红色区域和蓝色区域都是最优区域。
```

**示例 2：**

```
输入：points = [[3,1],[9,0],[1,0],[1,4],[5,3],[8,8]]
输出：3
```

 

**提示：**

- `n == points.length`
- `2 <= n <= 105`
- `points[i].length == 2`
- `0 <= xi, yi <= 109`

这题都不想说啥了，这题放第一个还差不多。所谓宽垂直面积，就是宽度

~~~java
class Solution {
    public int maxWidthOfVerticalArea(int[][] points) {
        Arrays.sort(points, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0]-o2[0];
            }
        });
        int max = 0;
        for(int i = 0; i +1 <points.length; i++){
            int temp = points[i+1][0] - points[i][0];
            max = max>temp?max:temp;
        }
        return max;
    }
}
~~~

### Q[5541统计只差一个字符的子串数目](https://leetcode-cn.com/problems/count-substrings-that-differ-by-one-character/)

难度 中等

给你两个字符串 `s` 和 `t` ，请你找出 `s` 中的非空子串的数目，这些子串满足替换 **一个不同字符** 以后，是 `t` 串的子串。换言之，请你找到 `s` 和 `t` 串中 **恰好** 只有一个字符不同的子字符串对的数目。

比方说， `"**compute**r"` 和 `"**computa**tion"` 加粗部分只有一个字符不同： `'e'`/`'a'` ，所以这一对子字符串会给答案加 1 。

请你返回满足上述条件的不同子字符串对数目。

一个 **子字符串** 是一个字符串中连续的字符。

 

**示例 1：**

```
输入：s = "aba", t = "baba"
输出：6
解释：以下为只相差 1 个字符的 s 和 t 串的子字符串对：
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
加粗部分分别表示 s 和 t 串选出来的子字符串。
```

**示例 2：**

```
输入：s = "ab", t = "bb"
输出：3
解释：以下为只相差 1 个字符的 s 和 t 串的子字符串对：
("ab", "bb")
("ab", "bb")
("ab", "bb")
加粗部分分别表示 s 和 t 串选出来的子字符串。
```

**示例 3：**

```
输入：s = "a", t = "a"
输出：0
```

**示例 4：**

```
输入：s = "abe", t = "bbc"
输出：10
```

 

**提示：**

- `1 <= s.length, t.length <= 100`
- `s` 和 `t` 都只包含小写英文字母。

这题就穷举，n3竟然过了。感觉中等题就是可以暴力，复杂题一遍需要好思路

~~~java
class Solution {
    public int countSubstrings(String s, String t) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = i; j < s.length(); j++) {
                String s1 = s.substring(i, j+1);
                for (int i1 = 0; i1 +s1.length()-1< t.length(); i1++) {
                    String s2 = t.substring(i1, t.length());
                    int flag = 0;
                    for (int i3 = 0; i3 < s1.length(); i3++){
                        if(s1.charAt(i3)!=s2.charAt(i3)){
                            flag++;
                            if(flag>1)
                                break;
                        }
                    }
                    if(flag == 1)
                        count++;
                }
            }
        }
        return count;
    }
}
~~~

### Q[140单词拆分 II](https://leetcode-cn.com/problems/word-break-ii/)

难度 困难

给定一个**非空**字符串 *s* 和一个包含**非空**单词列表的字典 *wordDict*，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

**说明：**

- 分隔时可以重复使用字典中的单词。
- 你可以假设字典中没有重复的单词。

**示例 1：**

```
输入:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
输出:
[
  "cats and dog",
  "cat sand dog"
]
```

**示例 2：**

```
输入:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
输出:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
解释: 注意你可以重复使用字典中的单词。
```

**示例 3：**

```
输入:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
输出:
[]
```

试了很多方法都没优化到点上

都会挂在这个例子上

~~~
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
["a","aa","aaa","aaaa","aaaaa","aaaaaa","aaaaaaa","aaaaaaaa","aaaaaaaaa","aaaaaaaaaa"]
~~~

**dfs的时候 判断一下，从当前pos能否划分。把这个记录下来。**

~~~cpp
class Solution
{
public:
    unordered_map<int, unordered_set<string>> m;
    unordered_set<int> f;//关键在这个记录，记录失败的起始点
    vector<string> wordBreak(string s, vector<string> &wordDict)
    {

        for (auto st : wordDict)
        {
            for (int i = 0; i <= s.size(); ++i)
            {
                int j = 0;
                while (j < st.size() && s[i + j] == st[j])
                {
                    j++;
                }
                if (j == st.size())
                    m[i].insert(st);
            }
        }
        vector<string> ans;
        dfs(ans, 0, s, "");
        return ans;
    }
    bool dfs(vector<string> &ans, int pos, string s, string temp)
    {
        if (f.find(pos)!=f.end()||pos > s.size())
        {
            return false;
        }
        if (pos == s.size())
        {
            ans.push_back(temp);
            return true;
        }
        auto it = m[pos].begin();
        bool flag = false;
        while (it != m[pos].end())
        {
            if (pos != 0)
                flag|=dfs(ans, pos + it->size(), s, temp + " " + *it);
            else
                flag|=dfs(ans, pos + it->size(), s, temp + *it);
            ++it;
        }
        if(!flag)
            f.insert(pos);
        return flag;
    }
};
~~~

击败91%

### Q [5554能否连接形成数组](https://leetcode-cn.com/problems/check-array-formation-through-concatenation/)

难度 简单

给你一个整数数组 `arr` ，数组中的每个整数 **互不相同** 。另有一个由整数数组构成的数组 `pieces`，其中的整数也 **互不相同** 。请你以 **任意顺序** 连接 `pieces` 中的数组以形成 `arr` 。但是，**不允许** 对每个数组 `pieces[i]` 中的整数重新排序。

如果可以连接 `pieces` 中的数组形成 `arr` ，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：arr = [85], pieces = [[85]]
输出：true
```

**示例 2：**

```
输入：arr = [15,88], pieces = [[88],[15]]
输出：true
解释：依次连接 [15] 和 [88]
```

**示例 3：**

```
输入：arr = [49,18,16], pieces = [[16,18,49]]
输出：false
解释：即便数字相符，也不能重新排列 pieces[0]
```

**示例 4：**

```
输入：arr = [91,4,64,78], pieces = [[78],[4,64],[91]]
输出：true
解释：依次连接 [91]、[4,64] 和 [78]
```

**示例 5：**

```
输入：arr = [1,3,5,7], pieces = [[2,4,6,8]]
输出：false
```

 

**提示：**

- `1 <= pieces.length <= arr.length <= 100`
- `sum(pieces[i].length) == arr.length`
- `1 <= pieces[i].length <= arr.length`
- `1 <= arr[i], pieces[i][j] <= 100`
- `arr` 中的整数 **互不相同**
- `pieces` 中的整数 **互不相同**（也就是说，如果将 `pieces` 扁平化成一维数组，数组中的所有整数互不相同）

此题为213场周赛。这场还是写了三个题，第四个困难题还是超时。371/3826

第一题不难，降维了。明确说了互相不同。只要找到一个相同，就开始向后遍历

~~~java
class Solution {
    public boolean canFormArray(int[] arr, int[][] pieces) {
        boolean[] b = new boolean[arr.length];
        for(int[] a:pieces){
            int i = 0;
            for(i = 0; i < arr.length; i ++){
                if(arr[i] == a[0])
                    break;
            }
            int j = 0;
            while(j<a.length){
                if(i<arr.length&&a[j] == arr[i])
                    b[i] = true;
                else
                    return false;
                i++;
                j++;
            }
        }
        for(boolean r: b) {
            if (!r)
                return false;
        }
        return true;
    }
}
~~~

### Q[5555统计字典序元音字符串的数目](https://leetcode-cn.com/problems/count-sorted-vowel-strings/)

难度 中等

给你一个整数 `n`，请返回长度为 `n` 、仅由元音 (`a`, `e`, `i`, `o`, `u`) 组成且按 **字典序排列** 的字符串数量。

字符串 `s` 按 **字典序排列** 需要满足：对于所有有效的 `i`，`s[i]` 在字母表中的位置总是与 `s[i+1]` 相同或在 `s[i+1]` 之前。

 

**示例 1：**

```
输入：n = 1
输出：5
解释：仅由元音组成的 5 个字典序字符串为 ["a","e","i","o","u"]
```

**示例 2：**

```
输入：n = 2
输出：15
解释：仅由元音组成的 15 个字典序字符串为
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"]
注意，"ea" 不是符合题意的字符串，因为 'e' 在字母表中的位置比 'a' 靠后
```

**示例 3：**

```
输入：n = 33
输出：66045
```

 

**提示：**

- `1 <= n <= 50` 

a1 e2 i3 o4 u5 

这题组合数学。有点像双周赛最后一题。dfs，记录此时最大的值就是5个数，从当前最大值开始枚举

~~~cpp
class Solution {
    int count;
    public int countVowelStrings(int n) {
        count = 0;
        dfs(n,0,1);
        return count;
    }
    public void dfs(int n,int pos,int max){
        if(pos == n)
        {
            count++;
            return;
        }
        for(int i = max; i <= 5; i++){
            dfs(n,pos+1,i);
        }
    }
}
~~~

### Q[5556可以到达的最远建筑](https://leetcode-cn.com/problems/furthest-building-you-can-reach/)

难度 中等

给你一个整数数组 `heights` ，表示建筑物的高度。另有一些砖块 `bricks` 和梯子 `ladders` 。

你从建筑物 `0` 开始旅程，不断向后面的建筑物移动，期间可能会用到砖块或梯子。

当从建筑物 `i` 移动到建筑物 `i+1`（下标 **从 0 开始** ）时：

- 如果当前建筑物的高度 **大于或等于** 下一建筑物的高度，则不需要梯子或砖块
- 如果当前建筑的高度 **小于** 下一个建筑的高度，您可以使用 **一架梯子** 或 **`(h[i+1] - h[i])` 个砖块**

如果以最佳方式使用给定的梯子和砖块，返回你可以到达的最远建筑物的下标（下标 **从 0 开始** ）。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/31/q4.gif)

```
输入：heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
输出：4
解释：从建筑物 0 出发，你可以按此方案完成旅程：
- 不使用砖块或梯子到达建筑物 1 ，因为 4 >= 2
- 使用 5 个砖块到达建筑物 2 。你必须使用砖块或梯子，因为 2 < 7
- 不使用砖块或梯子到达建筑物 3 ，因为 7 >= 6
- 使用唯一的梯子到达建筑物 4 。你必须使用砖块或梯子，因为 6 < 9
无法越过建筑物 4 ，因为没有更多砖块或梯子。
```

**示例 2：**

```
输入：heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
输出：7
```

**示例 3：**

```
输入：heights = [14,3,19,3], bricks = 17, ladders = 0
输出：3
```

 

**提示：**

- `1 <= heights.length <= 105`
- `1 <= heights[i] <= 106`
- `0 <= bricks <= 109`
- `0 <= ladders <= heights.length`

这题看着还挺有意思的

刚开始没看到梯子，之间就写了。然后样例1过不了。

然后就懂了，贪心算法嘛。先不管梯子，建立一个数组维护 最耗砖头。

每次砖头不够用，就用一个梯子，砖头加上 最耗砖头的那个

~~~cpp
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        int i = 0;
        int[] p = new int[ladders];

        for (i = 1; i < heights.length; i++) {
            if (heights[i - 1] >= heights[i])
                continue;
            if (ladders > 0 && p[ladders - 1] < heights[i] - heights[i - 1]) {
                for (int t = 1; t < ladders; t++) {
                    p[t - 1] = p[t];
                }
                p[ladders - 1] = heights[i] - heights[i - 1];
            }
            
                bricks -= (heights[i] - heights[i - 1]);
            if(bricks>=0)
                continue;
            else if (ladders > 0) {
                bricks += p[ladders - 1];
                ladders--;
            }
            if(bricks<0)
                break;
        }
        return i - 1;
    }
}
~~~

### Q[349两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

难度 简单

给定两个数组，编写一个函数来计算它们的交集。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
```

 

**说明：**

- 输出结果中的每个元素一定是唯一的。
- 我们可以不考虑输出结果的顺序。

两个set求交集

~~~cpp
class Solution {
public:
    unordered_set<int> s1;
    unordered_set<int> s2;
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        for(int i:nums1)
            s1.insert(i);
        
        for(int i:nums2){
            s2.insert(i);
        }
        vector<int> ans;
        for(int i: s1){
            if(s2.find(i)!=s2.end()){
                ans.push_back(i);
            }
        }
        return ans;
    }
};
~~~

击败96%

### Q[207课程表](https://leetcode-cn.com/problems/course-schedule/)

难度 中等

你这个学期必须选修 `numCourse` 门课程，记为 `0` 到 `numCourse-1` 。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：`[0,1]`

给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？

 

**示例 1:**

```
输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
```

**示例 2:**

```
输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
```

 

**提示：**

1. 输入的先决条件是由 **边缘列表** 表示的图形，而不是 邻接矩阵 。详情请参见[图的表示法](http://blog.csdn.net/woaidapaopao/article/details/51732947)。
2. 你可以假定输入的先决条件中没有重复的边。
3. `1 <= numCourses <= 10^5`

dfs，剪一剪枝，记忆化搜索

~~~cpp
class Solution
{
public:
    unordered_map<int, unordered_set<int>> m;
    bool canFinish(int numCourses, vector<vector<int>> &prerequisites)
    {
        vector<bool> find(numCourses);
        vector<bool> save(numCourses);
        for (auto v : prerequisites)
        {
            m[v[0]].insert(v[1]);
        }
        for (int i = 0; i < numCourses; ++i)
        {
            if (m[i].size() == 0)
                save[i] = true;
            else if (!dfs(numCourses, prerequisites, save, i, find))
                return false;
        }
        return true;
    }
    bool dfs(int numCourses, vector<vector<int>> &prerequisites, vector<bool> &save, int i, vector<bool> &find)
    {
        if (save[i] == true)
            return true;
        bool flag = true;
        if (find[i] == true)
            return false;
        find[i] = true;
        for (auto v : m[i])
        {
            if (find[v] == true)
            {
                if (save[v] == false)
                {
                    flag = false;
                    break;
                }
                else
                    flag = true;
            }
            flag &= dfs(numCourses, prerequisites, save, v, find);
            if(flag == false)
                break;
        }
        save[i] = flag;
        return flag;
    }
};
~~~

击败18%

### Q[210课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)

难度中等294

现在你总共有 *n* 门课需要选，记为 `0` 到 `n-1`。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: `[0,1]`

给定课程总量以及它们的先决条件，返回你为了学完所有课程所安排的学习顺序。

可能会有多个正确的顺序，你只要返回一种就可以了。如果不可能完成所有课程，返回一个空数组。

**示例 1:**

```
输入: 2, [[1,0]] 
输出: [0,1]
解释: 总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。
```

**示例 2:**

```
输入: 4, [[1,0],[2,0],[3,1],[3,2]]
输出: [0,1,2,3] or [0,2,1,3]
解释: 总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
     因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。
```

**说明:**

1. 输入的先决条件是由**边缘列表**表示的图形，而不是邻接矩阵。详情请参见[图的表示法](http://blog.csdn.net/woaidapaopao/article/details/51732947)。
2. 你可以假定输入的先决条件中没有重复的边。

**提示:**

1. 这个问题相当于查找一个循环是否存在于有向图中。如果存在循环，则不存在拓扑排序，因此不可能选取所有课程进行学习。
2. [通过 DFS 进行拓扑排序](https://www.coursera.org/specializations/algorithms) - 一个关于Coursera的精彩视频教程（21分钟），介绍拓扑排序的基本概念。
3. 拓扑排序也可以通过 [BFS](https://baike.baidu.com/item/宽度优先搜索/5224802?fr=aladdin&fromid=2148012&fromtitle=广度优先搜索) 完成。

上一题稍作修改。save数组还可以记录是否存入vector

~~~cpp
class Solution
{
public:
    unordered_map<int, unordered_set<int>> m;
    vector<int> classes;
    vector<int> findOrder(int numCourses, vector<vector<int>> &prerequisites)
    {
        vector<bool> find(numCourses);
        vector<bool> save(numCourses);
        for (auto v : prerequisites)
        {
            m[v[0]].insert(v[1]);
        }
        for (int i = 0; i < numCourses; ++i)
        {
            if (m[i].size() == 0)
            {
                if (save[i] == false)
                    classes.push_back(i);
                save[i] = true;
            }
            else if (!dfs(numCourses, prerequisites, save, i, find))
                return {};
        }
        return classes;
    }
    bool dfs(int numCourses, vector<vector<int>> &prerequisites, vector<bool> &save, int i, vector<bool> &find)
    {
        if (save[i] == true)
            return true;
        bool flag = true;
        if (find[i] == true)
            return false;
        find[i] = true;
        for (auto v : m[i])
        {
            if (find[v] == true)
            {
                if (save[v] == false)
                {
                    flag = false;
                    break;
                }
                else
                    flag = true;
            }
            flag &= dfs(numCourses, prerequisites, save, v, find);
            if (flag == false)
                break;
        }
        save[i] = flag;
        if (flag == true)
            classes.push_back(i);
        return flag;
    }
};
~~~

击败13%

### Q[329矩阵中的最长递增路径](https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/)

难度 困难

给定一个整数矩阵，找出最长递增路径的长度。

对于每个单元格，你可以往上，下，左，右四个方向移动。 你不能在对角线方向上移动或移动到边界外（即不允许环绕）。

**示例 1:**

```
输入: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
输出: 4 
解释: 最长递增路径为 [1, 2, 6, 9]。
```

**示例 2:**

```
输入: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
输出: 4 
解释: 最长递增路径是 [3, 4, 5, 6]。注意不允许在对角线方向上移动。
```

dfs，一下子就想到要把每个点最长路记录下来。但不知道为啥又觉得，这样子不对。想不出方法证明、也想不出方法驳倒

这个最长递增路径，一定是单向的。大概因为这样所以可以这样做

~~~cpp
class Solution
{
public:
    int longestIncreasingPath(vector<vector<int>> &matrix)
    {
        if (matrix.size() == 0)
            return 0;
        vector<vector<int>> v(matrix.size(), vector<int>(matrix[0].size()));
        vector<vector<bool>> path(matrix.size(), vector<bool>(matrix[0].size()));
        int max = 0;
        for (int i = 0; i < matrix.size(); i++)
        {
            for (int j = 0; j < matrix[i].size(); j++)
            {
                int t = dfs(matrix, path, i, j, v);
                max = max < t ? t : max;
            }
        }
        return max;
    }
    int dfs(vector<vector<int>> &matrix, vector<vector<bool>> &path, int x, int y, vector<vector<int>> &v)
    {
        int d[4] = {0};
        if (v[x][y] != 0)
            return v[x][y];
        path[x][y] = true;

        if (x - 1 >= 0 && !path[x - 1][y] && matrix[x - 1][y] < matrix[x][y])
        {
            d[0] = dfs(matrix, path, x - 1, y, v);
        }
        if (y - 1 >= 0 && !path[x][y - 1] && matrix[x][y - 1] < matrix[x][y])
        {

            d[1] = dfs(matrix, path, x, y - 1, v);
        }
        if (x + 1 < matrix.size() && !path[x + 1][y] && matrix[x + 1][y] < matrix[x][y])
        {

            d[2] = dfs(matrix, path, x + 1, y, v);
        }
        if (y + 1 < matrix[0].size() && !path[x][y + 1] && matrix[x][y + 1] < matrix[x][y])
        {
            d[3] = dfs(matrix, path, x, y + 1, v);
        }
        sort(begin(d), end(d));
        path[x][y] = false;
        v[x][y] = d[3] + 1;
        return d[3] + 1;
    }
};
~~~

击败13%

### Q[941有效的山脉数组](https://leetcode-cn.com/problems/valid-mountain-array/)

难度 简单

给定一个整数数组 `A`，如果它是有效的山脉数组就返回 `true`，否则返回 `false`。

让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：

- `A.length >= 3`

- 在 

  ```
  0 < i < A.length - 1
  ```

   条件下，存在 

  ```
  i
  ```

   使得：

  - `A[0] < A[1] < ... A[i-1] < A[i]`
  - `A[i] > A[i+1] > ... > A[A.length - 1]`

 

![img](https://assets.leetcode.com/uploads/2019/10/20/hint_valid_mountain_array.png)

 

**示例 1：**

```
输入：[2,1]
输出：false
```

**示例 2：**

```
输入：[3,5,5]
输出：false
```

**示例 3：**

```
输入：[0,3,2,1]
输出：true
```

 

**提示：**

1. `0 <= A.length <= 10000`
2. `0 <= A[i] <= 10000 `

~~~cpp
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        if(A.size()<3) 
            return false;
        if(A[0]>=A[1])
            return false;
        int i = 0;
        bool flag = false;
        while (i+1<A.size()&&A[i]<A[i+1]) {
            i++;
        }
        while (i+1<A.size()&&A[i]>A[i+1]) {
            i++;
            flag = true;
        }
        if (flag&&i==A.size()-1)
            return true;
        return false;
    }
};
~~~

击败18%

### Q[剑指Offer51数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

难度 困难

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

 

**示例 1:**

```
输入: [7,5,6,4]
输出: 5
```

 

**限制：**

```
0 <= 数组长度 <= 50000
```

题目越短，事情就越不简单

用merge sort

后移一位。

L = [8, 12, 16, 22, 100]   R = [9, 26, 55, 64, 91]  M = [8]
        |                       |
      lPtr                     rPtr
观察就可以知道，当左边大于右边，即可构成逆序对

count += (mid - i + 1);

被merge sort 卡了好几次。第一次是merge没写对 范围从0 开始。第二次是sort用的数组应该重复利用，而不是每次重新开辟

~~~cpp
class Solution
{
public:
    int count;
    int reversePairs(vector<int> &nums)
    {
        count = 0;
        vector<int> aux(nums.size());
        sor(nums, 0, nums.size() - 1,aux);
        return count;
    }
    void sor(vector<int> &nums, int start, int end,vector<int> &aux)
    {
        if (start >= end)
            return;

        int mid = start + (end - start) / 2;
        sor(nums, start, mid,aux);
        sor(nums, mid + 1, end,aux);
        merge(nums, start, mid, end,aux);
    }
    void merge(vector<int> &nums, int start, int mid, int end,vector<int> &aux)
    {

        for (int i = start; i <= end; i++)
        {
            aux[i] = nums[i];
        }
        int i = start;
        int j = mid + 1;
        int pos = start;
        int prej = j;
        while (pos <= end)
        {
            if (i > mid)
            {
                nums[pos++] = aux[j++];
            }
            else if (j > end)
            {
                nums[pos++] = aux[i++];
            }
            else if (aux[i] > aux[j])
            {
                nums[pos++] = aux[j++];
                count += (mid - i + 1);
            }
            else
            {
                nums[pos++] = aux[i++];
            }
        }
    }
};
~~~

击败41%

### Q[315计算右侧小于当前元素的个数](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/)

难度困难

给定一个整数数组 *nums*，按要求返回一个新数组 *counts*。数组 *counts* 有该性质： `counts[i]` 的值是 `nums[i]` 右侧小于 `nums[i]` 的元素的数量。

 

**示例：**

```
输入：nums = [5,2,6,1]
输出：[2,1,1,0] 
解释：
5 的右侧有 2 个更小的元素 (2 和 1)
2 的右侧仅有 1 个更小的元素 (1)
6 的右侧有 1 个更小的元素 (1)
1 的右侧有 0 个更小的元素
```

 

**提示：**

- `0 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

上一题代码魔改，建立一个index数组，存原来的位置。

用一个ans数组存结果。把上题的count稍作修改

（魔改得乱七八糟

~~~cpp
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        vector<int> aux(nums.size());
        vector<int> index(nums.size());
        vector<int> tempindex(nums.size());
        vector<int> ans(nums.size());
        for(int i = 0; i < nums.size(); i++) {
            index[i] = i;
        }
        sor(nums, 0, nums.size() - 1,aux,index,tempindex,ans);
        return ans;
    }
    void sor(vector<int> &nums, int start, int end,vector<int> &aux,vector<int> &index,vector<int> &tempindex,vector<int> &ans) 
    {
        if (start >= end)
            return;

        int mid = start + (end - start) / 2;
        sor(nums, start, mid,aux,index,tempindex,ans);
        sor(nums, mid + 1, end,aux,index,tempindex,ans);
        merge(nums, start, mid, end,aux,index,tempindex,ans);
    }
    void merge(vector<int> &nums, int start, int mid, int end,vector<int> &aux,vector<int> &index,vector<int> &tempindex,vector<int> &ans)
    {

        for (int i = start; i <= end; i++)
        {
            aux[i] = nums[i];
            tempindex[i] = index[i];
        }
        int i = start;
        int j = mid + 1;
        int pos = start;
        int prej = j;
        while (pos <= end)
        {
            if (i > mid)
            {
                index[pos] = tempindex[j];
                nums[pos++] = aux[j++];
            
            }
            else if (j > end)
            {
                ans[tempindex[i]] += (j-mid-1);
                index[pos] = tempindex[i];
                nums[pos++] = aux[i++];
            }
            else if (aux[i] > aux[j])
            {
                index[pos] = tempindex[j];
                nums[pos++] = aux[j++];
            }
            else
            {
                ans[tempindex[i]] += (j-mid-1);
                index[pos] = tempindex[i];
                nums[pos++] = aux[i++];
            }
        }
    }
};
~~~

击败72%

### Q[57插入区间](https://leetcode-cn.com/problems/insert-interval/)

难度困难226

给出一个*无重叠的 ，*按照区间起始端点排序的区间列表。

在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

 

**示例 1：**

```
输入：intervals = [[1,3],[6,9]], newInterval = [2,5]
输出：[[1,5],[6,9]]
```

**示例 2：**

```
输入：intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出：[[1,2],[3,10],[12,16]]
解释：这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。
```

 

**注意：**输入类型已在 2019 年 4 月 15 日更改。请重置为默认代码定义以获取新的方法签名。

我想得太复杂了。

其实只有三种情况，第一种，铁定在区间左边。第二种，铁定在区间右边。第三种，在之间（只需要放缩left与max

~~~cpp
class Solution
{
public:
    vector<vector<int>> insert(vector<vector<int>> &intervals, vector<int> &newInterval)
    {
        int left = newInterval[0];
        int right = newInterval[1];
        vector<vector<int>> v;
        bool placed = true;
        for (int i = 0; i < intervals.size(); ++i)
        {
            if (intervals[i][1] < left)
            {
                v.push_back(intervals[i]);
            }
            else if (intervals[i][0] > right)
            {
                if (placed)
                {
                    v.push_back({left, right});
                    placed = false;
                }
                v.push_back(intervals[i]);
            }
            else
            {
                left = min(left, intervals[i][0]);
                right = max(right, intervals[i][1]);
            }
        }
        if (placed)
        {
            v.push_back({left, right});
            placed = false;
        }
        return v;
    }
};
~~~

击败50%

### Q[131分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

难度中等413

给定一个字符串 *s*，将 *s* 分割成一些子串，使每个子串都是回文串。

返回 *s* 所有可能的分割方案。

**示例:**

```
输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]
```

回溯法，在每个点有两种choice，分割开，不分割

~~~cpp
class Solution
{
public:
    vector<vector<string>> ans;
    vector<vector<string>> partition(string s)
    {
        vector<string> v;
        backward(v,s,0);
        return ans;
    }
    void backward(vector<string> &v, string s, int pos)
    {
        if(pos >= s.size())
            return;
        int i = 0;
        int j = pos;
        bool flag = true;
        while (i <= j)
        {
            if (s[i] != s[j])
            {
                flag = false;
                break;
            }
            i++;
            j--;
        }
        if (flag)
        {
            if (pos + 1 == s.size())
            {
                v.push_back(s);
                ans.push_back(vector<string>(v));
                v.pop_back();
                return;
            }
            //cout<<s.substr(0,pos+1)<<" ";
            v.push_back(s.substr(0, pos + 1));
            //cout<<s.substr(pos+1)<<" ";
            backward(v, s.substr(pos+1), 0);
            v.pop_back();
        }
        backward(v, s, pos + 1);
    }
};
~~~

击败30%

### Q[212单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)

难度困难275

给定一个二维网格 **board** 和一个字典中的单词列表 **words**，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

**示例:**

```
输入: 
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

输出: ["eat","oath"]
```

**说明:**
你可以假设所有输入都由小写字母 `a-z` 组成。

**提示:**

- 你需要优化回溯算法以通过更大数据量的测试。你能否早点停止回溯？
- 如果当前单词不存在于所有单词的前缀中，则可以立即停止回溯。什么样的数据结构可以有效地执行这样的操作？散列表是否可行？为什么？ 前缀树如何？如果你想学习如何实现一个基本的前缀树，请先查看这个问题： [实现Trie（前缀树）](https://leetcode-cn.com/problems/implement-trie-prefix-tree/description/)。

这题根据之前写的前缀树来完成。Q208

前缀树 里面有一个大小26 前缀树数组，若下一个为b，则把下一个结点存在数组1号位

先建树，然后搜索

~~~java
class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        Trie tree = new Trie();
        for (String s : words) {
            tree.insert(s);
        }
        List<String> ans = new ArrayList<>();
        boolean[][] path = new boolean[board.length][board[0].length];
        for(int i = 0; i < board.length; i++){
            for(int j = 0; j <  board[0].length; j++){
               if( tree.next[board[i][j] - 'a']!=null){
                   path[i][j] = true;
                   tree.next[board[i][j] - 'a'].startsWith(board,path,i,j,""+(char)(board[i][j]),ans);
                   path[i][j] = false;
               }
            }
        }
        return ans;
    }



}

class Trie {
    public Trie[] next;
    boolean isEnd;

    public Trie() {
        next = new Trie[26];
    }

    public void insert(String word) {
        Trie temp = this;
        for (int i = 0; i < word.length(); i++) {
            if (temp.next[word.charAt(i) - 'a'] == null) {
                temp.next[word.charAt(i) - 'a'] = new Trie();
            }
            temp = temp.next[word.charAt(i) - 'a'];
        }
        temp.isEnd = true;
    }


    public boolean startsWith(char[][] boards, boolean[][] path, int x, int y,String s,List<String> ans) {
        Trie temp = this;
        if (isEnd) {
            ans.add(s);
            isEnd = false;
        }
        if (x - 1 >= 0 && !path[x - 1][y] && temp.next[boards[x - 1][y]-'a'] != null) {
            path[x - 1][y] = true;
            temp.next[boards[x - 1][y]-'a'].startsWith(boards, path, x - 1, y,s+(char)(boards[x - 1][y]),ans);
            path[x - 1][y] = false;
        }
        if (y - 1 >= 0 && !path[x][y - 1] && temp.next[boards[x][y - 1]-'a'] != null) {
            path[x][y - 1] = true;
            temp.next[boards[x][y - 1]-'a'].startsWith(boards, path, x, y - 1,s+(char)(boards[x ][y-1]),ans);
            path[x][y - 1] = false;
        }
        if (x + 1 < boards.length && !path[x + 1][y] && temp.next[boards[x + 1][y]-'a'] != null) {
            path[x + 1][y] = true;
            temp.next[boards[x + 1][y]-'a'].startsWith(boards, path, x + 1, y,s+(char)(boards[x+1][y]),ans);
            path[x + 1][y] = false;
        }
        if (y + 1 < boards[0].length && !path[x][y + 1] && temp.next[boards[x][y + 1]-'a'] != null) {
            path[x][y + 1] = true;
            temp.next[boards[x][y + 1]-'a'].startsWith(boards, path, x, y + 1,s+(char)(boards[x][y+1]),ans);
            path[x][y + 1] = false;
        }
        return false;
    }
}
~~~

击败44%

### Q[1356根据数字二进制下 1 的数目排序](https://leetcode-cn.com/problems/sort-integers-by-the-number-of-1-bits/)

难度 简单

给你一个整数数组 `arr` 。请你将数组中的元素按照其二进制表示中数字 **1** 的数目升序排序。

如果存在多个数字二进制中 **1** 的数目相同，则必须将它们按照数值大小升序排列。

请你返回排序后的数组。

 

**示例 1：**

```
输入：arr = [0,1,2,3,4,5,6,7,8]
输出：[0,1,2,4,8,3,5,6,7]
解释：[0] 是唯一一个有 0 个 1 的数。
[1,2,4,8] 都有 1 个 1 。
[3,5,6] 有 2 个 1 。
[7] 有 3 个 1 。
按照 1 的个数排序得到的结果数组为 [0,1,2,4,8,3,5,6,7]
```

**示例 2：**

```
输入：arr = [1024,512,256,128,64,32,16,8,4,2,1]
输出：[1,2,4,8,16,32,64,128,256,512,1024]
解释：数组中所有整数二进制下都只有 1 个 1 ，所以你需要按照数值大小将它们排序。
```

**示例 3：**

```
输入：arr = [10000,10000]
输出：[10000,10000]
```

**示例 4：**

```
输入：arr = [2,3,5,7,11,13,17,19]
输出：[2,3,5,17,7,11,13,19]
```

**示例 5：**

```
输入：arr = [10,100,1000,10000]
输出：[10,100,10000,1000]
```

 

**提示：**

- `1 <= arr.length <= 500`
- `0 <= arr[i] <= 10^4`

*b**i**t*[*i*]=*b**i**t*[*i*>>1]+(*i*&1) 用这个建表

cpp的sort第三个参数好像与java略不同，java返回int，这个应该返回bool

~~~cpp
class Solution {
public:
    vector<int> sortByBits(vector<int>& arr) {
        int bit[10001];
        for(int i =1;i<=10000;i++){
            bit[i] = bit[i>>1] +(i&1);
        }
        sort(arr.begin(),arr.end(),[&](const int&a,const int&b){
            if(bit[a]<bit[b])
                return true;
            if(bit[a]>bit[b])
                return false;
            if(a<b)
                return true;
            return false;
        });
        return arr;
    }
};
~~~

击败90%

### Q[301删除无效的括号](https://leetcode-cn.com/problems/remove-invalid-parentheses/)

难度 困难

删除最小数量的无效括号，使得输入的字符串有效，返回所有可能的结果。

**说明:** 输入可能包含了除 `(` 和 `)` 以外的字符。

**示例 1:**

```
输入: "()())()"
输出: ["()()()", "(())()"]
```

**示例 2:**

```
输入: "(a)())()"
输出: ["(a)()()", "(a())()"]
```

**示例 3:**

```
输入: ")("
输出: [""]
```

bfs，每次删掉一个。刚开始用的是queue，发现会重复，改用set

~~~cpp
class Solution
{
public:
    vector<string> removeInvalidParentheses(string s)
    {
        set<string> q1;
        set<string> q2;
        q1.insert(s);
        vector<string> ans;
        bool flag = false;
        while (true)
        {
            if (flag)
                return ans;
            auto it = q1.begin();
            while (it!=q1.end())
            {
                string s1 = *it;
                if (isRight(s1))
                {
                    flag = true;
                    ans.push_back(s1);
                }
                else
                {
                    for (int i = 0; i < s1.size(); i++)
                    {
                        //cout<<s1.substr(0,i)+s1.substr(i+1);
                        q2.insert(s1.substr(0, i) + s1.substr(i + 1));
                    }
                }
                it = q1.erase(it);
            }
            if (flag)
                return ans;
            it = q2.begin();
            while (it!=q2.end())
            {
                string s1 = *it;
                if (isRight(s1))
                {
                    flag = true;
                    ans.push_back(s1);
                }
                else
                {
                    for (int i = 0; i < s1.size(); i++)
                    {
                        q1.insert(s1.substr(0, i + 1) + s1.substr(i + 1));
                    }
                }
                it = q2.erase(it);
            }
        }
    }
    bool isRight(string s)
    {
        int i = 0;
        stack<int> s1;
        while (i < s.size())
        {
            if (s[i] == '(')
                s1.push(1);
            else if (s[i] == ')')
            {
                if (s1.size() > 0)
                    s1.pop();
                else
                {
                    return false;
                }
            }
            i++;
        }
        if (s1.size() != 0)
            return false;
        return true;
    }
};
~~~

击败5%

### Q[327区间和的个数](https://leetcode-cn.com/problems/count-of-range-sum/)

难度困难194

给定一个整数数组 `nums`，返回区间和在 `[lower, upper]` 之间的个数，包含 `lower` 和 `upper`。
区间和 `S(i, j)` 表示在 `nums` 中，位置从 `i` 到 `j` 的元素之和，包含 `i` 和 `j` (`i` ≤ `j`)。

**说明:**
最直观的算法复杂度是 *O*(*n*2) ，请在此基础上优化你的算法。

**示例:**

```
输入: nums = [-2,5,-1], lower = -2, upper = 2,
输出: 3 
解释: 3个区间分别是: [0,0], [2,2], [0,2]，它们表示的和分别为: -2, -1, 2。
```

利用mergesort，有点像之前那个查找逆序对

求出前缀和。mergesort 合并前先看对

~~~cpp
class Solution
{
public:
    int countRangeSum(vector<int> &nums, int lower, int upper)
    {
        if(nums.size() == 0)
            return 0;
        vector<long> presum(nums.size()+1);
        vector<long> aux(nums.size()+1);
        presum[0] = 0;
        for(int i = 1; i<presum.size();i++){
            presum[i] = presum[i-1]+nums[i-1];
        }
        return merges(presum,0,presum.size()-1,lower,upper,aux);
    }
    int merges(vector<long> &presum,int left,int right,int lower,int upper,vector<long>& aux){
        if(left>=right)
            return 0;
        int mid = (left+right)/2;
        int count = merges(presum,left,mid,lower,upper,aux);
        count +=merges(presum,mid+1,right,lower,upper,aux);
        int i = left;
        int l = mid+1;
        int r = mid+1;
        while(i<=mid){
            while(l<=right && presum[l] - presum[i]<lower){
                l++;
            }
            while(r<=right && presum[r] - presum[i]<=upper){
                r++;
            }
            count += (r-l);
            i++;
        }
        for(int i = left;i <= right;i++){
            aux[i] = presum[i];
        } 
        i = left;
        int j = mid+1;
        int pos = left;
        while(i<=mid||j<=right){
            if(i>mid){
                presum[pos++] = aux[j++];
            }
            else if(j>right){
                presum[pos++] = aux[i++];
            }
            else if(aux[i]<aux[j])
                presum[pos++] = aux[i++];
            else
                presum[pos++] = aux[j++];
        }
        return count;
    }
};
~~~



击败96%

### Q[5561获取生成数组中的最大值](https://leetcode-cn.com/problems/get-maximum-in-generated-array/)

难度简单1

给你一个整数 `n` 。按下述规则生成一个长度为 `n + 1` 的数组 `nums` ：

- `nums[0] = 0`
- `nums[1] = 1`
- 当 `2 <= 2 * i <= n` 时，`nums[2 * i] = nums[i]`
- 当 `2 <= 2 * i + 1 <= n` 时，`nums[2 * i + 1] = nums[i] + nums[i + 1]`

返回生成数组 `nums` 中的 **最大** 值。

 

**示例 1：**

```
输入：n = 7
输出：3
解释：根据规则：
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
因此，nums = [0,1,1,2,1,3,2,3]，最大值 3
```

**示例 2：**

```
输入：n = 2
输出：1
解释：根据规则，nums[0]、nums[1] 和 nums[2] 之中的最大值是 1
```

**示例 3：**

```
输入：n = 3
输出：2
解释：根据规则，nums[0]、nums[1]、nums[2] 和 nums[3] 之中的最大值是 2
```

 

**提示：**

- `0 <= n <= 100`

第214场周赛，写了两，607 / 3597

依照规则建表

~~~java
class Solution {
    public int getMaximumGenerated(int n) {
        if(n == 1) return 1;
        if(n == 0) return 0;
        int[] x= new int[n+1];
        x[0] = 0;
        x[1]=1;
        int max = 1;
        for(int i = 2; i <= n; i++){
            if(i%2==0)
                x[i] = x[i/2];
            else
                x[i] = x[i/2]+x[i/2+1];
            max = Math.max(x[i],max);
        }
        return max;
    }
}
~~~

### Q[5562字符频次唯一的最小删除次数](https://leetcode-cn.com/problems/minimum-deletions-to-make-character-frequencies-unique/)

难度中等1

如果字符串 `s` 中 **不存在** 两个不同字符 **频次** 相同的情况，就称 `s` 是 **优质字符串** 。

给你一个字符串 `s`，返回使 `s` 成为 **优质字符串** 需要删除的 **最小** 字符数。

字符串中字符的 **频次** 是该字符在字符串中的出现次数。例如，在字符串 `"aab"` 中，`'a'` 的频次是 `2`，而 `'b'` 的频次是 `1` 。

 

**示例 1：**

```
输入：s = "aab"
输出：0
解释：s 已经是优质字符串。
```

**示例 2：**

```
输入：s = "aaabbbcc"
输出：2
解释：可以删除两个 'b' , 得到优质字符串 "aaabcc" 。
另一种方式是删除一个 'b' 和一个 'c' ，得到优质字符串 "aaabbc" 。
```

**示例 3：**

```
输入：s = "ceabaacb"
输出：2
解释：可以删除两个 'c' 得到优质字符串 "eabaab" 。
注意，只需要关注结果字符串中仍然存在的字符。（即，频次为 0 的字符会忽略不计。）
```

 

**提示：**

- `1 <= s.length <= 105`
- `s` 仅含小写英文字母

这题难度也不大。先统计次数，然后把次数塞进set里，只要出现过就，减少一次再检测

~~~cpp
class Solution {
public:
    int minDeletions(string s) {
        if(s.size() == 0)
            return 0;
        int m[26] = {0};
        for(auto c:s){
            m[c-'a']++;
        }
        int min = INT_MIN;
        int count = 0;
        unordered_set<int> s1;
        for(int i = 0; i<26;i++){
            if(m[i]==0)
                continue;
            if(s1.find(m[i])!=s1.end()){
                m[i]--;
                count++;
                while(m[i]!=0&&s1.find(m[i])!=s1.end()){
                    m[i]--;
                    count++;
                }
                s1.insert(m[i]);
            }
            else{
                s1.insert(m[i]);
            }
        }
        return count;
    }
};
~~~

### Q[973最接近原点的 K 个点](https://leetcode-cn.com/problems/k-closest-points-to-origin/)

难度 中等

我们有一个由平面上的点组成的列表 `points`。需要从中找出 `K` 个距离原点 `(0, 0)` 最近的点。

（这里，平面上两点之间的距离是欧几里德距离。）

你可以按任何顺序返回答案。除了点坐标的顺序之外，答案确保是唯一的。

 

**示例 1：**

```
输入：points = [[1,3],[-2,2]], K = 1
输出：[[-2,2]]
解释： 
(1, 3) 和原点之间的距离为 sqrt(10)，
(-2, 2) 和原点之间的距离为 sqrt(8)，
由于 sqrt(8) < sqrt(10)，(-2, 2) 离原点更近。
我们只需要距离原点最近的 K = 1 个点，所以答案就是 [[-2,2]]。
```

**示例 2：**

```
输入：points = [[3,3],[5,-1],[-2,4]], K = 2
输出：[[3,3],[-2,4]]
（答案 [[-2,4],[3,3]] 也会被接受。）
```

 

**提示：**

1. `1 <= K <= points.length <= 10000`
2. `-10000 < points[i][0] < 10000`
3. `-10000 < points[i][1] < 10000`

排序就好了。cpp的排序 函数应该写成返回bool，而java只要返回相减值就好了

~~~cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        sort(points.begin(),points.end(),[](vector<int> &a,vector<int> &b){
            int x = a[0]*a[0]+a[1]*a[1];
            int  y = b[0]*b[0]+b[1]*b[1];
            if(x>y) return false;
            return true;
        });
        int i = 0;
        vector<vector<int>> ans;
        while(i<K){
            ans.push_back({points[i][0],points[i][1]});
            i++;
        }
        return ans;
    }
};
~~~

击败65%

### Q[557反转字符串中的单词 III](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)

难度 简单

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

 

**示例：**

```
输入："Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"
```

 

***\**\*\*\*提示：\*\*\*\*\****

- 在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。

用reverse

~~~cpp
class Solution {
public:
    string reverseWords(string s) {
        int pre = 0;
        int i = 0 ;
        for(i = 0; i < s.size(); ++i){
            if(s[i]!=' '){
                
            }
            if(s[i]== ' '){
                reverse(s.begin()+pre,s.begin()+i);
                pre = i+1;
            }
        }
        reverse(s.begin()+pre,s.begin()+i);
        return s;
    }
};
~~~

击败40%

### Q[231 2的幂](https://leetcode-cn.com/problems/power-of-two/)

难度 简单

给定一个整数，编写一个函数来判断它是否是 2 的幂次方。

**示例 1:**

```
输入: 1
输出: true
解释: 20 = 1
```

**示例 2:**

```
输入: 16
输出: true
解释: 24 = 16
```

**示例 3:**

```
输入: 218
输出: false
```

移位操作

~~~cpp
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if(n == 0) return false;
        while(n!=0){
            if(n&1 == 1)
                break;
            n>>=1;
        }
        n>>=1;
        if(n == 0)  return true;
        return false;
    }
};
~~~

击败42%

### Q[59螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

难度 中等

给定一个正整数 *n*，生成一个包含 1 到 *n*2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

**示例:**

```
输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

四个方向转转，大一就会写，这次还调试了一会

~~~cpp
class Solution
{
public:
    vector<vector<int>> generateMatrix(int n)
    {
        vector<vector<int>> ans(n, vector<int>(n));
        int dir = 0;
        int x = 0, y = 0;
        int count = 1;
        vector<vector<bool>> flag(n, vector<bool>(n));
        while (count<=n*n)
        {
            if (dir == 0)
            {
                if (x >= 0 && y >= 0 && x < n && y < n && !flag[x][y])
                {
                    flag[x][y] = true;
                    ans[x][y] = count++;
                    ++y;
                }
                else
                {
                    --y;
                    ++x;
                    dir = 1;
                }
            }
            else if (dir == 1)
            {
                if (x >= 0 && y >= 0 && x < n && y < n && !flag[x][y])
                {
                    flag[x][y] = true;
                    ans[x][y] = count++;
                    ++x;
                }
                else
                {
                    --x;
                    --y;
                    dir = 2;
                }
            }
            else if (dir == 2)
            {
                if (x >= 0 && y >= 0 && x < n && y < n && !flag[x][y])
                {
                    flag[x][y] = true;
                    ans[x][y] = count++;
                    --y;
                }
                else
                {
                    ++y;
                    --x;
                    dir = 3;
                }
            }
            else if (dir == 3)
            {
                if (x >= 0 && y >= 0 && x < n && y < n && !flag[x][y])
                {
                    flag[x][y] = true;
                    ans[x][y] = count++;
                    --x;
                }
                else
                {
                    ++x;
                    ++y;
                    dir = 0;
                }
            }
        }
        return ans;
    }
};
~~~

击败38%

### Q[235二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

难度 简单

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

例如，给定如下二叉搜索树: root = [6,2,8,0,4,7,9,null,null,3,5]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)

 

**示例 1:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

**示例 2:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

 

**说明:**

- 所有节点的值都是唯一的。
- p、q 为不同节点且均存在于给定的二叉搜索树中。

刚开始没看到是二叉搜索树

（自认为写的有点巧妙

如果分叉两边返回值都为真，说明现在是答案

一边为真，接着返回

~~~cpp
class Solution
{
public:
    TreeNode *lowestCommonAncestor(TreeNode *root, TreeNode *p, TreeNode *q)
    {
        if (root->val == p->val || root->val == q->val)
        {
            return root;
        }
        TreeNode *t1 = nullptr, *t2 = nullptr;
        if (root->left != nullptr)
            t1 = lowestCommonAncestor(root->left, p, q);
        if (root->right != nullptr)
            t2 = lowestCommonAncestor(root->right, p, q);
        if (t1 != nullptr && t2 != nullptr)
            return root;
        if (t1 != nullptr)
            return t1;
        return t2;
    }
};
~~~

击败96%

### Q[292Nim 游戏](https://leetcode-cn.com/problems/nim-game/)

难度简单390

你和你的朋友，两个人一起玩 [Nim 游戏](https://baike.baidu.com/item/Nim游戏/6737105)：

- 桌子上有一堆石头。
- 你们轮流进行自己的回合，你作为先手。
- 每一回合，轮到的人拿掉 1 - 3 块石头。
- 拿掉最后一块石头的人就是获胜者。

假设你们每一步都是最优解。请编写一个函数，来判断你是否可以在给定石头数量为 `n` 的情况下赢得游戏。如果可以赢，返回 `true`；否则，返回 `false` 。

 

**示例 1：**

```
输入：n = 4
输出：false 
解释：如果堆中有 4 块石头，那么你永远不会赢得比赛；
     因为无论你拿走 1 块、2 块 还是 3 块石头，最后一块石头总是会被你的朋友拿走。
```

**示例 2：**

```
输入：n = 1
输出：true
```

**示例 3：**

```
输入：n = 2
输出：true
```

 

**提示：**

- `1 <= n <= 231 - 1`

~~~cpp
class Solution {
public:
    bool canWinNim(int n) {
        if(n%4 == 0)
            return false;
        return true;
    }
};
~~~

### Q[514自由之路](https://leetcode-cn.com/problems/freedom-trail/)

难度 困难

视频游戏“辐射4”中，任务“通向自由”要求玩家到达名为“Freedom Trail Ring”的金属表盘，并使用表盘拼写特定关键词才能开门。

给定一个字符串 **ring**，表示刻在外环上的编码；给定另一个字符串 **key**，表示需要拼写的关键词。您需要算出能够拼写关键词中所有字符的**最少**步数。

最初，**ring** 的第一个字符与12:00方向对齐。您需要顺时针或逆时针旋转 ring 以使 **key** 的一个字符在 12:00 方向对齐，然后按下中心按钮，以此逐个拼写完 **key** 中的所有字符。

旋转 **ring** 拼出 key 字符 **key[i]** 的阶段中：

1. 您可以将 **ring** 顺时针或逆时针旋转**一个位置**，计为1步。旋转的最终目的是将字符串 **ring** 的一个字符与 12:00 方向对齐，并且这个字符必须等于字符 **key[i] 。**
2. 如果字符 **key[i]** 已经对齐到12:00方向，您需要按下中心按钮进行拼写，这也将算作 **1 步**。按完之后，您可以开始拼写 **key** 的下一个字符（下一阶段）, 直至完成所有拼写。

**示例：**

 

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/ring.jpg)

```
输入: ring = "godding", key = "gd"
输出: 4
解释:
 对于 key 的第一个字符 'g'，已经在正确的位置, 我们只需要1步来拼写这个字符。 
 对于 key 的第二个字符 'd'，我们需要逆时针旋转 ring "godding" 2步使它变成 "ddinggo"。
 当然, 我们还需要1步进行拼写。
 因此最终的输出是 4。
```

**提示：**

1. **ring** 和 **key** 的字符串长度取值范围均为 1 至 100；
2. 两个字符串中都只有小写字符，并且均可能存在重复字符；
3. 字符串 **key** 一定可以由字符串 **ring** 旋转拼出。

暴力法，先建立ring的set，从key遍历，每次从key i-1转移到i。转移方程不好写。

~~~cpp
class Solution
{
public:
    int findRotateSteps(string ring, string key)
    {
        vector<vector<int>>dp(key.size(),vector<int>(ring.size()));
        
        int n = ring.size();
        int mi = INT_MAX;
        unordered_set<int> charset[26];
        for (int i = 0; i < ring.size(); ++i)
        {
            charset[ring[i] - 'a'].insert(i);
        }
        auto it = charset[key[0] - 'a'].begin();
        while (it != charset[key[0] - 'a'].end())
        {
            dp[0][*it] = min(*it,abs(n-*it))+1;
            it++;
        }
        for (int i = 1; i < key.size(); i++)
        {
            auto it1 = charset[key[i]-'a'].begin();
            while(it1!=charset[key[i]-'a'].end()){
                dp[i][*it1] = INT_MAX;
                auto it2 = charset[key[i-1]-'a'].begin();
                while(it2!=charset[key[i-1]-'a'].end()){
                    cout<<*it2<<endl;
                    dp[i][*it1] = min(dp[i-1][*it2]+min(abs(*it1-*it2),abs(n-abs(*it1-*it2)))+1,dp[i][*it1]);
                    cout<<dp[i][*it1]<<endl;
                    if(i == key.size()-1){
                        mi = min(mi,dp[i][*it1]);
                    }
                    it2++;
                }
                it1++;
            }
        }
        return mi;
    }
};
~~~

击败9%

### Q[922按奇偶排序数组 II](https://leetcode-cn.com/problems/sort-array-by-parity-ii/)

难度简单157

给定一个非负整数数组 `A`， A 中一半整数是奇数，一半整数是偶数。

对数组进行排序，以便当 `A[i]` 为奇数时，`i` 也是奇数；当 `A[i]` 为偶数时， `i` 也是偶数。

你可以返回任何满足上述条件的数组作为答案。

 

**示例：**

```
输入：[4,2,5,7]
输出：[4,5,2,7]
解释：[4,7,2,5]，[2,5,4,7]，[2,7,4,5] 也会被接受。
```

 

**提示：**

1. `2 <= A.length <= 20000`
2. `A.length % 2 == 0`
3. `0 <= A[i] <= 1000`

 

~~~cpp
class Solution
{
public:
    vector<int> sortArrayByParityII(vector<int> &A)
    {
        if (A.size() == 1)
            return {A[0]};
        if (A.size() == 0)
            return {};
        int i = 0, j = 1;
        while (i < A.size() && j < A.size())
        {
            while (i < A.size() && A[i] % 2 == 0)
                i += 2;
            while (j < A.size() && A[j] % 2 != 0)
                j += 2;
            if (i < A.size() && j < A.size())
            {
                int temp = A[i];
                A[i] = A[j];
                A[j] = temp;
            }
        }
        return A;
    }
};
~~~

击败78%

### Q[89格雷编码](https://leetcode-cn.com/problems/gray-code/)

难度中等241

格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。

给定一个代表编码总位数的非负整数 *n*，打印其格雷编码序列。即使有多个不同答案，你也只需要返回其中一种。

格雷编码序列必须以 0 开头。

 

**示例 1:**

```
输入: 2
输出: [0,1,3,2]
解释:
00 - 0
01 - 1
11 - 3
10 - 2

对于给定的 n，其格雷编码序列并不唯一。
例如，[0,2,3,1] 也是一个有效的格雷编码序列。

00 - 0
10 - 2
11 - 3
01 - 1
```

**示例 2:**

```
输入: 0
输出: [0]
解释: 我们定义格雷编码序列必须以 0 开头。
     给定编码总位数为 n 的格雷编码序列，其长度为 2n。当 n = 0 时，长度为 20 = 1。
     因此，当 n = 0 时，其格雷编码序列为 [0]。
```

根据格雷码定义出发。先是按照前一个编码，从右往左开始，一个个试，看集合中有没有这个元素。

~~~cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        if(n==0) return {0};
        unordered_set<int> s1;
        vector<int> v;
        v.push_back(0);
        s1.insert(0);
        int i = 1;
        int l = 1<<n;
        while(i<l){
            int x = v[v.size()-1];
            int t = 1; 
            while(s1.find(x^t)!=s1.end()){
                t<<=1;
            }
            v.push_back(x^t);
            s1.insert(x^t);
            i++;
        }
        return v;
    }
};
~~~

击败43%

### Q[1122数组的相对排序](https://leetcode-cn.com/problems/relative-sort-array/)

难度 简单

给你两个数组，`arr1` 和 `arr2`，

- `arr2` 中的元素各不相同
- `arr2` 中的每个元素都出现在 `arr1` 中

对 `arr1` 中的元素进行排序，使 `arr1` 中项的相对顺序和 `arr2` 中的相对顺序相同。未在 `arr2` 中出现过的元素需要按照升序放在 `arr1` 的末尾。

 

**示例：**

```
输入：arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
输出：[2,2,2,1,4,3,3,9,6,7,19]
```

 

**提示：**

- `arr1.length, arr2.length <= 1000`
- `0 <= arr1[i], arr2[i] <= 1000`
- `arr2` 中的元素 `arr2[i]` 各不相同
- `arr2` 中的每个元素 `arr2[i]` 都出现在 `arr1` 中

一开始我以为有序图是按照插入顺序。其实是按照key的大小

~~~cpp
class Solution
{
public:
    vector<int> relativeSortArray(vector<int> &arr1, vector<int> &arr2)
    {
        vector<int> sq;
        unordered_map<int, int> m;
        vector<int> v;
        for (int i : arr2)
        {
            m[i] = 1;
            sq.push_back(i);
        }
        for (int i : arr1)
        {
            if (m[i] >= 1)
            {
                m[i]++;
            }
            else
            {
                v.push_back(i);
            }
        }
        vector<int> ans;
        for(int i = 0; i <sq.size();++i){
            for(int j = m[sq[i]]; j>1; --j){
                ans.push_back(sq[i]);
            }
        }
        sort(v.begin(),v.end());
        for(int i:v){
            ans.push_back(i);
        }
        return ans;
    }
};
~~~

击败88%

### Q[341扁平化嵌套列表迭代器](https://leetcode-cn.com/problems/flatten-nested-list-iterator/)

难度中等161

给你一个嵌套的整型列表。请你设计一个迭代器，使其能够遍历这个整型列表中的所有整数。

列表中的每一项或者为一个整数，或者是另一个列表。其中列表的元素也可能是整数或是其他列表。

 

**示例 1:**

```
输入: [[1,1],2,[1,1]]
输出: [1,1,2,1,1]
解释: 通过重复调用 next 直到 hasNext 返回 false，next 返回的元素的顺序应该是: [1,1,2,1,1]。
```

**示例 2:**

```
输入: [1,[4,[6]]]
输出: [1,4,6]
解释: 通过重复调用 next 直到 hasNext 返回 false，next 返回的元素的顺序应该是: [1,4,6]。
```

dfs搞定

~~~cpp
class NestedIterator {
public:
    vector<int> n;
    int i;
    NestedIterator(vector<NestedInteger> &nestedList):i(0) {
        dfs(nestedList);
    }

    void dfs(vector<NestedInteger> &nestedList){
        for(NestedInteger e:nestedList){
            if(e.isInteger()==true){
                n.push_back(e.getInteger());
            }
            else{
                dfs(e.getList());
            }
        }
    }
    
    int next() {
        return n[i++];
    }
    
    bool hasNext() {
        if(i<n.size())
            return true;
        return false;
    }
};
~~~

击败72%

### Q[402移掉K位数字](https://leetcode-cn.com/problems/remove-k-digits/)

难度 中等

给定一个以字符串表示的非负整数 *num*，移除这个数中的 *k* 位数字，使得剩下的数字最小。

**注意:**

- *num* 的长度小于 10002 且 ≥ *k。*
- *num* 不会包含任何前导零。

**示例 1 :**

```
输入: num = "1432219", k = 3
输出: "1219"
解释: 移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219。
```

**示例 2 :**

```
输入: num = "10200", k = 1
输出: "200"
解释: 移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。
```

示例 **3 :**

```
输入: num = "10", k = 2
输出: "0"
解释: 从原数字移除所有的数字，剩余为空就是0。
```

原本使用回溯法，但是因为数字连longlong都存不下。看到这题数字全用string存就应该感觉到不对劲

建立一个单调队列。中间因为 k没用完，不应返回“” ，以为可以跳过后面的字符 没有判断完整个字符串 种种原因

33个样例，交了7发

~~~cpp
class Solution
{
public:
    string removeKdigits(string num, int k)
    {
        if(k == num.size())
            return "0";
        vector<char> v;
        int pos = 0;
        v.push_back(num[0]);
        for (int i = 1; i < num.size(); ++i)
        {
            if (k == 0)
            {
                v.push_back(num[i]);
                pos++;
            }
            else
            {
                
                if (num[i] > v[pos])
                {
                    v.push_back(num[i]);
                    pos++;
                }
                else
                {
                    while (pos>=0&&num[i] < v[pos] && k > 0)
                    {
                        v.pop_back();
                        pos--;
                        k--;
                    }
                    v.push_back(num[i]);
                    pos++;
                }
            }
        }
        string ans;
        int flag = 0;
        for (int i = 0; i < v.size()-k; ++i)
        {
            char ch = v[i];

            if (ch != '0')
                flag = 1;
            else if (flag == 0)
                continue;
            ans += ch;
        }
        return ans==""?"0":ans;
    }
};
~~~

击败94%

### Q[5550拆炸弹](https://leetcode-cn.com/problems/defuse-the-bomb/)

难度 简单

你有一个炸弹需要拆除，时间紧迫！你的情报员会给你一个长度为 `n` 的 **循环** 数组 `code` 以及一个密钥 `k` 。

为了获得正确的密码，你需要替换掉每一个数字。所有数字会 **同时** 被替换。

- 如果 `k > 0` ，将第 `i` 个数字用 **接下来** `k` 个数字之和替换。
- 如果 `k < 0` ，将第 `i` 个数字用 **之前** `k` 个数字之和替换。
- 如果 `k == 0` ，将第 `i` 个数字用 `0` 替换。

由于 `code` 是循环的， `code[n-1]` 下一个元素是 `code[0]` ，且 `code[0]` 前一个元素是 `code[n-1]` 。

给你 **循环** 数组 `code` 和整数密钥 `k` ，请你返回解密后的结果来拆除炸弹！

 

**示例 1：**

```
输入：code = [5,7,1,4], k = 3
输出：[12,10,16,13]
解释：每个数字都被接下来 3 个数字之和替换。解密后的密码为 [7+1+4, 1+4+5, 4+5+7, 5+7+1]。注意到数组是循环连接的。
```

**示例 2：**

```
输入：code = [1,2,3,4], k = 0
输出：[0,0,0,0]
解释：当 k 为 0 时，所有数字都被 0 替换。
```

**示例 3：**

```
输入：code = [2,4,9,3], k = -2
输出：[12,5,6,13]
解释：解密后的密码为 [3+9, 2+3, 4+2, 9+4] 。注意到数组是循环连接的。如果 k 是负数，那么和为 之前 的数字。
```

 

**提示：**

- `n == code.length`
- `1 <= n <= 100`
- `1 <= code[i] <= 100`
- `-(n - 1) <= k <= n - 1`

第39场周赛，写了两题，排名264/2069。这题不难，只要暴力去算后/前几个，注意到了头 变化一下

~~~java
class Solution {
    public int[] decrypt(int[] code, int k) {
        int[] ans = new int[code.length];
        if(k==0){
            for(int i = 0; i < code.length; i++)
                code[i] = 0;
            return code;
        }
        if(k>0){
            for(int i = 0; i < code.length;i++){
                int temp = 0;
                int pi = i;
                for(int j = 1; j<= k; j++){
                    if(pi+j == code.length){
                        pi = -j;
                    }
                    temp+=code[pi+j];
                }
                ans[i] = temp;
            }
        }
        if(k<0){
            for(int i = 0; i < code.length;i++){
                int temp = 0;
                int pi = i;
                for(int j = 1; j<= -k; j++){
                    if(pi-j == -1){
                        pi = code.length-1+j;
                    }
                    temp+=code[pi-j];
                }
                ans[i] = temp;
            }
        }
        return ans;
    }
}
~~~

### Q[5551使字符串平衡的最少删除次数](https://leetcode-cn.com/problems/minimum-deletions-to-make-string-balanced/)

难度 中等

给你一个字符串 `s` ，它仅包含字符 `'a'` 和 `'b'` 。

你可以删除 `s` 中任意数目的字符，使得 `s` **平衡** 。我们称 `s` **平衡的** 当不存在下标对 `(i,j)` 满足 `i < j` 且 `s[i] = 'b'` 同时 `s[j]= 'a'` 。

请你返回使 `s` **平衡** 的 **最少** 删除次数。

 

**示例 1：**

```
输入：s = "aababbab"
输出：2
解释：你可以选择以下任意一种方案：
下标从 0 开始，删除第 2 和第 6 个字符（"aababbab" -> "aaabbb"），
下标从 0 开始，删除第 3 和第 6 个字符（"aababbab" -> "aabbbb"）。
```

**示例 2：**

```
输入：s = "bbaaaaabb"
输出：2
解释：唯一的最优解是删除最前面两个字符。
```

 

**提示：**

- `1 <= s.length <= 105`
- `s[i]` 要么是 `'a'` 要么是 `'b'` 。

dp问题，只有两种状态，i之前全为a，或者i前一段已经为b，只能从a转移到b

~~~java
class Solution {
    public int minimumDeletions(String s) {
        int[][] dp = new int[s.length()][2];
        if (s.charAt(0) == 'a') {
            dp[0][0] = 0;
            dp[0][1] = 1;
        } else {
            dp[0][0] = 1;
            dp[0][1] = 0;
        }
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) == 'a') {
                dp[i][0] = dp[i - 1][0];
                dp[i][1] = dp[i - 1][1] + 1;
            } else {
                dp[i][0] = dp[i - 1][0] + 1;
                dp[i][1] = Math.min(dp[i - 1][1], dp[i - 1][0]);
            }
        }
        return Math.min(dp[s.length() - 1][0], dp[s.length() - 1][1]);
    }
}
~~~

### Q[5601设计有序流](https://leetcode-cn.com/problems/design-an-ordered-stream/)

难度简单1

有 `n` 个 `(id, value)` 对，其中 `id` 是 `1` 到 `n` 之间的一个整数，`value` 是一个字符串。不存在 `id` 相同的两个 `(id, value)` 对。

设计一个流，以 **任意** 顺序获取 `n` 个 `(id, value)` 对，并在多次调用时 **按 `id` 递增的顺序** 返回一些值。

实现 `OrderedStream` 类：

- `OrderedStream(int n)` 构造一个能接收 `n` 个值的流，并将当前指针 `ptr` 设为 `1` 。

- ```
  String[] insert(int id, String value)
  ```

   

  向流中存储新的

   

  ```
  (id, value)
  ```

   

  对。存储后：

  - 如果流存储有 `id = ptr` 的 `(id, value)` 对，则找出从 `id = ptr` 开始的 **最长 id 连续递增序列** ，并 **按顺序** 返回与这些 id 关联的值的列表。然后，将 `ptr` 更新为最后那个 `id + 1` 。
  - 否则，返回一个空列表。

 

**示例：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/15/q1.gif)**

```
输入
["OrderedStream", "insert", "insert", "insert", "insert", "insert"]
[[5], [3, "ccccc"], [1, "aaaaa"], [2, "bbbbb"], [5, "eeeee"], [4, "ddddd"]]
输出
[null, [], ["aaaaa"], ["bbbbb", "ccccc"], [], ["ddddd", "eeeee"]]

解释
OrderedStream os= new OrderedStream(5);
os.insert(3, "ccccc"); // 插入 (3, "ccccc")，返回 []
os.insert(1, "aaaaa"); // 插入 (1, "aaaaa")，返回 ["aaaaa"]
os.insert(2, "bbbbb"); // 插入 (2, "bbbbb")，返回 ["bbbbb", "ccccc"]
os.insert(5, "eeeee"); // 插入 (5, "eeeee")，返回 []
os.insert(4, "ddddd"); // 插入 (4, "ddddd")，返回 ["ddddd", "eeeee"]
```

 

**提示：**

- `1 <= n <= 1000`
- `1 <= id <= n`
- `value.length == 5`
- `value` 仅由小写字母组成
- 每次调用 `insert` 都会使用一个唯一的 `id`
- 恰好调用 `n` 次 `insert`

第215场周赛，1127 / 4428，写了两题。

题目写的很复杂。用一个vector存下来。有个ptr存位置

~~~cpp
class OrderedStream {
public:
    vector<string> v;
    int ptr;
    OrderedStream(int n):v(n),ptr(0){

    }
    
    vector<string> insert(int id, string value) {
        v[id-1] = value;
        vector<string> ans;
        while(ptr<v.size()&&v[ptr]!=""){
            ans.push_back(v[ptr]);
            ptr++;
        }
        return ans;
    }
};
~~~

### Q[5603确定两个字符串是否接近](https://leetcode-cn.com/problems/determine-if-two-strings-are-close/)

难度中等3

如果可以使用以下操作从一个字符串得到另一个字符串，则认为两个字符串 **接近** ：

- 操作 1：交换任意两个

   

  现有

   

  字符。

  - 例如，`a**b**cd**e** -> a**e**cd**b**`

- 操作 2：将一个

   

  现有

   

  字符的每次出现转换为另一个

   

  现有

   

  字符，并对另一个字符执行相同的操作。

  - 例如，`**aa**c**abb** -> **bb**c**baa**`（所有 `a` 转化为 `b` ，而所有的 `b` 转换为 `a` ）

你可以根据需要对任意一个字符串多次使用这两种操作。

给你两个字符串，`word1` 和 `word2` 。如果 `word1` 和 `word2` **接近** ，就返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：word1 = "abc", word2 = "bca"
输出：true
解释：2 次操作从 word1 获得 word2 。
执行操作 1："abc" -> "acb"
执行操作 1："acb" -> "bca"
```

**示例 2：**

```
输入：word1 = "a", word2 = "aa"
输出：false
解释：不管执行多少次操作，都无法从 word1 得到 word2 ，反之亦然。
```

**示例 3：**

```
输入：word1 = "cabbba", word2 = "abbccc"
输出：true
解释：3 次操作从 word1 获得 word2 。
执行操作 1："cabbba" -> "caabbb"
执行操作 2："caabbb" -> "baaccc"
执行操作 2："baaccc" -> "abbccc"
```

**示例 4：**

```
输入：word1 = "cabbba", word2 = "aabbss"
输出：false
解释：不管执行多少次操作，都无法从 word1 得到 word2 ，反之亦然。
```

 

**提示：**

- `1 <= word1.length, word2.length <= 105`
- `word1` 和 `word2` 仅包含小写英文字母

这题目也写得很复杂。我一度以为写不出，直接看下一题。下一题更写不出来。

其实只要统计次数就好了。第一个操作决定了，位置压根不重要。第二个操作又告诉你了，只要有相同次数就好了

由于忽略 第二个操作需要 该字母至少有一次。浪费了一发

~~~java
class Solution {
    public boolean closeStrings(String word1, String word2) {
        if(word1.length()!=word2.length())
            return false;
        int[][] count =new  int[2][26];
        for(int i = 0; i < word1.length(); ++i){
            count[0][word1.charAt(i)-'a']++;
            count[1][word2.charAt(i)-'a']++;
        }
        for(int i = 0; i < 26; i++){
            if(count[0][i]!=0&&count[1][i]==0)
                return false;
            if(count[0][i]==0&&count[1][i]!=0)
                return false;
        }
        Arrays.sort(count[0]);
        Arrays.sort(count[1]);
        for(int i = 0; i < 26; i++){
            if(count[0][i]!=count[1][i])
                return false;
        }
        return true;
    }
}
~~~



### Q[378有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

难度中等473

给定一个 *`n x n`* 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 `k` 小的元素。
请注意，它是排序后的第 `k` 小元素，而不是第 `k` 个不同的元素。

 

**示例：**

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

返回 13。
```

 

**提示：**
你可以假设 k 的值永远是有效的，`1 ≤ k ≤ n2 `。

用一个数组存下每行的位置，起始为0

由分析可以知道 每行当前位置都是 可能成为当前最小的

用一个优先队列排序

~~~java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int[] re = new int[matrix.length];
        PriorityQueue<Integer> p = new PriorityQueue<Integer>((x, y) -> {
            return matrix[x][re[x]] - matrix[y][re[y]];
        });
        for (int i = 0; i < matrix.length; i++)
            p.add(i);
        int x = 1;
        while (x < k) {
            int y = p.poll();
            re[y]++;
            if (re[y] < matrix[y].length)
                p.add(y);
            x++;
        }
        int t = p.poll();
        return matrix[t][re[t]];
    }
}
~~~

cpp的lambda没写好，用了java来写

击败25%

### Q[1030距离顺序排列矩阵单元格](https://leetcode-cn.com/problems/matrix-cells-in-distance-order/)

难度 简单

给出 `R` 行 `C` 列的矩阵，其中的单元格的整数坐标为 `(r, c)`，满足 `0 <= r < R` 且 `0 <= c < C`。

另外，我们在该矩阵中给出了一个坐标为 `(r0, c0)` 的单元格。

返回矩阵中的所有单元格的坐标，并按到 `(r0, c0)` 的距离从最小到最大的顺序排，其中，两单元格`(r1, c1)` 和 `(r2, c2)` 之间的距离是曼哈顿距离，`|r1 - r2| + |c1 - c2|`。（你可以按任何满足此条件的顺序返回答案。）

 

**示例 1：**

```
输入：R = 1, C = 2, r0 = 0, c0 = 0
输出：[[0,0],[0,1]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1]
```

**示例 2：**

```
输入：R = 2, C = 2, r0 = 0, c0 = 1
输出：[[0,1],[0,0],[1,1],[1,0]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1,1,2]
[[0,1],[1,1],[0,0],[1,0]] 也会被视作正确答案。
```

**示例 3：**

```
输入：R = 2, C = 3, r0 = 1, c0 = 2
输出：[[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1,1,2,2,3]
其他满足题目要求的答案也会被视为正确，例如 [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]]。
```

 

**提示：**

1. `1 <= R <= 100`
2. `1 <= C <= 100`
3. `0 <= r0 < R`
4. `0 <= c0 < C`

类似bfs

~~~cpp
class Solution {
public:
    vector<vector<int>> allCellsDistOrder(int R, int C, int r0, int c0) {
        vector<vector<int>> ans;
        set<pair<int,int>> s;
        ans.push_back({r0,c0});
        s.insert(pair<int,int>(r0,c0));
        int pos = 0;
        while(pos<ans.size()){
            int x = ans[pos][0];
            int y = ans[pos][1];
            if(x-1>=0&&s.find(pair<int,int>(x-1,y))==s.end()){
                ans.push_back({x-1,y});
                s.insert(pair<int,int>(x-1,y));
            }
            if(y-1>=0&&s.find(pair<int,int>(x,y-1))==s.end()){
                ans.push_back({x,y-1});
                s.insert(pair<int,int>(x,y-1));
            }
            if(x+1<R&&s.find(pair<int,int>(x+1,y))==s.end()){
                ans.push_back({x+1,y});
                s.insert(pair<int,int>(x+1,y));
            }
            if(y+1<C&&s.find(pair<int,int>(x,y+1))==s.end()){
                ans.push_back({x,y+1});
                s.insert(pair<int,int>(x,y+1));
            }
            pos++;
        }
        return ans;
    }
};
~~~

击败17%

### Q[134加油站](https://leetcode-cn.com/problems/gas-station/)

难度中等451

在一条环路上有 *N* 个加油站，其中第 *i* 个加油站有汽油 `gas[i]` 升。

你有一辆油箱容量无限的的汽车，从第 *i* 个加油站开往第 *i+1* 个加油站需要消耗汽油 `cost[i]` 升。你从其中的一个加油站出发，开始时油箱为空。

如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。

**说明:** 

- 如果题目有解，该答案即为唯一答案。
- 输入数组均为非空数组，且长度相同。
- 输入数组中的元素均为非负数。

**示例 1:**

```
输入: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

输出: 3

解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
```

**示例 2:**

```
输入: 
gas  = [2,3,4]
cost = [3,4,3]

输出: -1

解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。
```

原本是暴力n2，看了题解说 从x出发到y走不动了。下一次直接从y开始出发。

因为 x 到 （x，y）中任意一点时，车油>0，而你从其中任意一点出发，车油为0。所以可以直接从y出发

~~~cpp
class Solution
{
public:
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost)
    {
        for (int i = 0; i < gas.size(); ++i)
        {
            int g = 0;
            int pi = i;
            bool flag = true;
            for (int j = 0; j < gas.size(); ++j)
            {
                
                if (pi + j >= gas.size())
                    pi = -j;
                g += gas[pi + j];
                g -= cost[pi + j];
                if (g < 0)
                {
                    flag = false;
                    if(pi+j>i)
                        i = pi+j;
                    break;
                }
            }
            if(flag)
                return i;
        }
        return -1;
    }
};
~~~

击败62%

### Q[147对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

难度 中等

对链表进行插入排序。

![img](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)
插入排序的动画演示如上。从第一个元素开始，该链表可以被认为已经部分排序（用黑色表示）。
每次迭代时，从输入数据中移除一个元素（用红色表示），并原地将其插入到已排好序的链表中。

 

**插入排序算法：**

1. 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
2. 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
3. 重复直到所有输入数据插入完为止。

 

**示例 1：**

```
输入: 4->2->1->3
输出: 1->2->3->4
```

**示例 2：**

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

~~~cpp
class Solution
{
public:
    ListNode *insertionSortList(ListNode *head)
    {
        if (head == nullptr)
            return head;
        ListNode first(0);
        first.next = head;
        ListNode* last = head;
        ListNode* p = head->next;
        last->next = nullptr;
        ListNode* temp = &first; 
        while (p != nullptr)
        {
            temp = &first;
            while(temp!=last &&  temp->next->val < p->val){
                temp = temp->next;
            }
            if(temp == last){
                temp->next = p;
                p = p->next;
                temp->next->next = nullptr;
                last = temp->next;
            }
            else{
                ListNode *r = p->next;
                p->next = temp->next;
                temp->next = p;
                p = r;
            }
        }
        return first.next;
    }
};
~~~

击败8%

### Q[5605检查两个字符串数组是否相等](https://leetcode-cn.com/problems/check-if-two-string-arrays-are-equivalent/)

难度 简单

给你两个字符串数组 `word1` 和 `word2` 。如果两个数组表示的字符串相同，返回 `true` ；否则，返回 `false` *。*

**数组表示的字符串** 是由数组中的所有元素 **按顺序** 连接形成的字符串。

 

**示例 1：**

```
输入：word1 = ["ab", "c"], word2 = ["a", "bc"]
输出：true
解释：
word1 表示的字符串为 "ab" + "c" -> "abc"
word2 表示的字符串为 "a" + "bc" -> "abc"
两个字符串相同，返回 true
```

**示例 2：**

```
输入：word1 = ["a", "cb"], word2 = ["ab", "c"]
输出：false
```

**示例 3：**

```
输入：word1  = ["abc", "d", "defg"], word2 = ["abcddefg"]
输出：true
```

 

**提示：**

- `1 <= word1.length, word2.length <= 103`
- `1 <= word1[i].length, word2[i].length <= 103`
- `1 <= sum(word1[i].length), sum(word2[i].length) <= 103`
- `word1[i]` 和 `word2[i]` 由小写字母组成

第216场周赛。这两天没什么时间写代码，生疏了。写了两题 1824/3826

这题没什么好说的

~~~cpp
class Solution {
public:
    bool arrayStringsAreEqual(vector<string>& word1, vector<string>& word2) {
        //vector<bool> flag(word1.size());
        string s1;
        for(auto s: word1)
            s1+=s;
        string s2;
        for(auto s:word2)
            s2 +=s;
        if(s1.compare(s2)==0)
            return true;
        return false;
    }
};
~~~

### Q[5606具Q有给定数值的最小字符串](https://leetcode-cn.com/problems/smallest-string-with-a-given-numeric-value/)

难度中等2

**小写字符** 的 **数值** 是它在字母表中的位置（从 `1` 开始），因此 `a` 的数值为 `1` ，`b` 的数值为 `2` ，`c` 的数值为 `3` ，以此类推。

字符串由若干小写字符组成，**字符串的数值** 为各字符的数值之和。例如，字符串 `"abe"` 的数值等于 `1 + 2 + 5 = 8` 。

给你两个整数 `n` 和 `k` 。返回 **长度** 等于 `n` 且 **数值** 等于 `k` 的 **字典序最小** 的字符串。

注意，如果字符串 `x` 在字典排序中位于 `y` 之前，就认为 `x` 字典序比 `y` 小，有以下两种情况：

- `x` 是 `y` 的一个前缀；
- 如果 `i` 是 `x[i] != y[i]` 的第一个位置，且 `x[i]` 在字母表中的位置比 `y[i]` 靠前。

 

**示例 1：**

```
输入：n = 3, k = 27
输出："aay"
解释：字符串的数值为 1 + 1 + 25 = 27，它是数值满足要求且长度等于 3 字典序最小的字符串。
```

**示例 2：**

```
输入：n = 5, k = 73
输出："aaszz"
```

 

**提示：**

- `1 <= n <= 105`
- `n <= k <= 26 * n`

这题刚开始理解错了，以为前面都是a，最后塞z，中间有一个字母。

应该从左往右，尽量塞靠前的字母

~~~cpp
class Solution {
public:
    string getSmallestString(int n, int k) {
        vector<char> v(n);
        for(int i = 0; i < n; i++){
            for(char ch = 'a';ch <= 'z'; ch++){
                if((k-(ch-'a'+1))<=(n-i-1)*26){
                    k = k-(ch-'a'+1);
                    v[i] = ch;
                    break;
                }
            }
        }
        string ans("");
        for(auto c : v){
            ans+=c;
        }
        return ans;
    }
};
~~~

### Q[5607生成平衡数组的方案数](https://leetcode-cn.com/problems/ways-to-make-a-fair-array/)

难度中等2

给你一个整数数组 `nums` 。你需要选择 **恰好** 一个下标（下标从 **0** 开始）并删除对应的元素。请注意剩下元素的下标可能会因为删除操作而发生改变。

比方说，如果 `nums = [6,1,7,4,1]` ，那么：

- 选择删除下标 `1` ，剩下的数组为 `nums = [6,7,4,1]` 。
- 选择删除下标 `2` ，剩下的数组为 `nums = [6,1,4,1]` 。
- 选择删除下标 `4` ，剩下的数组为 `nums = [6,1,7,4]` 。

如果一个数组满足奇数下标元素的和与偶数下标元素的和相等，该数组就是一个 **平衡数组** 。

请你返回删除操作后，剩下的数组 `nums` 是 **平衡数组** 的 **方案数** 。

 

**示例 1：**

```
输入：nums = [2,1,6,4]
输出：1
解释：
删除下标 0 ：[1,6,4] -> 偶数元素下标为：1 + 4 = 5 。奇数元素下标为：6 。不平衡。
删除下标 1 ：[2,6,4] -> 偶数元素下标为：2 + 4 = 6 。奇数元素下标为：6 。平衡。
删除下标 2 ：[2,1,4] -> 偶数元素下标为：2 + 4 = 6 。奇数元素下标为：1 。不平衡。
删除下标 3 ：[2,1,6] -> 偶数元素下标为：2 + 6 = 8 。奇数元素下标为：1 。不平衡。
只有一种让剩余数组成为平衡数组的方案。
```

**示例 2：**

```
输入：nums = [1,1,1]
输出：3
解释：你可以删除任意元素，剩余数组都是平衡数组。
```

**示例 3：**

```
输入：nums = [1,2,3]
输出：0
解释：不管删除哪个元素，剩下数组都不是平衡数组。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

这个题周赛的时候想出了思路，奈何代码能力有点菜。

这个判断方程真不太好写

计算 奇数偶数 前缀和后缀和。删除某个数字即是把后面 奇数变成偶数，偶数变成奇数

~~~cpp
class Solution
{
public:
    int waysToMakeFair(vector<int> &nums)
    {
        if(nums.size() == 1)   
            return 1;
        int count = 0;
        int n = nums.size();
        vector<int> presum1(n);
        vector<int> presum2(n);
        vector<int> hsum1(n);
        vector<int> hsum2(n);

        for (int i = 2; i < nums.size(); i += 2)
        {
            presum1[i] = nums[i - 2] + presum1[i - 2];
        }
        for (int i = 3; i < nums.size(); i += 2)
        {
            presum2[i] = nums[i - 2] + presum2[i - 2];
        }
        for (int i = nums.size() - 3; i >= 0; i -= 2)
        {
            if (i % 2 == 0)
            {
                hsum1[i] = hsum1[i + 2] + nums[i + 2];
            }
            else
            {
                hsum2[i] = hsum2[i + 2] + nums[i + 2];
            }
        }
        for (int i = nums.size() - 4; i >= 0; i -= 2)
        {
            if (i % 2 == 0)
            {
                hsum1[i] = hsum1[i + 2] + nums[i + 2];
            }
            else
            {
                hsum2[i] = hsum2[i + 2] + nums[i + 2];
            }
        }
        for(int i = 0; i < nums.size(); ++i){
            if(i+1<nums.size()){
                if(presum1[i] + presum2[i]+hsum1[i+1]+hsum2[i+1] +nums[i+1]== presum1[i+1] + presum2[i+1]+hsum1[i]+hsum2[i])
                    count++;
            }
            else{
                if(presum1[i] + presum2[i]== presum1[i-1] + presum2[i-1]+nums[i-1])
                    count++;
            }
        }
        return count;
    }
};
~~~

击败100%，当前提交比较少。

### Q[395至少有K个重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/)

难度 中等

找到给定字符串（由小写字符组成）中的最长子串 ***T\*** ， 要求 ***T\*** 中的每一字符出现次数都不少于 *k* 。输出 ***T\*** 的长度。

**示例 1:**

```
输入:
s = "aaabb", k = 3

输出:
3

最长子串为 "aaa" ，其中 'a' 重复了 3 次。
```

**示例 2:**

```
输入:
s = "ababbc", k = 2

输出:
5

最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。
```

先找出不符合字母的位置，然后按他们的位置拆开字符串，递归检查

~~~cpp
class Solution
{
public:
    int longestSubstring(string s, int k)
    {
        if (s.size() == 0)
            return 0;
        vector<int> f(26);
        set<int> e[26];
        vector<int> point(s.size());
        for (int i = 0; i < s.size(); ++i)
        {
            f[s[i] - 'a']++;
            e[s[i] - 'a'].insert(i);
        }
        bool flag = true;
        int m = 0;
        for (int i = 0; i < 26; ++i)
        {
            if (f[i] != 0 && f[i] < k)
            {
                flag = false;
                int pre = 0;
                auto it = e[i].begin();
                while (it != e[i].end())
                {
                    point[*it] = 1;
                    it++;
                }
            }
        }
        if (flag)
            return s.size();
        int i = 0;
        while (i < s.size() && point[i] != 0)
        {
            i++;
        }
        if(i>=s.size())
            return 0;
        for (int j = i+1; j < s.size(); ++j)
        {
            if (point[j] == 1)
            {
                m = max(m, longestSubstring(s.substr(i, j - i), k));
                i = j;
            }
        }
        if (point[s.size() - 1] != 1)
            m = max(m, longestSubstring(s.substr(i, s.size() - 1 - i + 1), k));
        return m;
    }
};
~~~

击败18%

### Q[222完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

难度中等301

给出一个**完全二叉树**，求出该树的节点个数。

**说明：**

[完全二叉树](https://baike.baidu.com/item/完全二叉树/7773232?fr=aladdin)的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

**示例:**

```
输入: 
    1
   / \
  2   3
 / \  /
4  5 6

输出: 6
```

刚开始不管完全，直接计算

~~~cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == nullptr) return 0;
        int count = 1;
        if(root->left != nullptr)
            count += countNodes(root->left);
        if(root->right != nullptr)
            count+=countNodes(root->right);
        return count;
    }
};
~~~

击败98%

按照官方题解，写二分查找。利用位操作来计算每个叶子的路径

~~~cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == nullptr)
            return 0;
        if(root->left == nullptr)
            return 1;
        int h = 0;
        TreeNode *t = root;
        while(t!=nullptr){
            t = t->left;
            h++;
        }
        int left = 1<<(h-1);
        int right = 1<<(h);
        int mid = (left+right)/2;
        while(left<right){
            if(exist(root,mid,h)){
                left = mid+1;
            }
            else{
                right = mid;
            }
            mid = (left+right)/2;
            cout<<left<<" ";
        }
        return left-1;
    }
    bool exist(TreeNode* root,int n,int h){
        if(n == 1){
            if(root!=nullptr){
                return true;
            }
            return false;
        }
        if(root == nullptr)
            return false;
        int dir = n&(1<<(h-2));
        n = n|(1<<(h-2));
        n = n&((1<<(h-1))-1);
        if(dir == 0){
            return exist(root->left,n,h-1);
        }
        else{
            return exist(root->right,n,h-1);
        }
    }
};
~~~

二分真是太难写了。刚开始right写的是 right = 1<<(h) -1;一直通过不了

结果才击败37% ...

### Q[1370上升下降字符串](https://leetcode-cn.com/problems/increasing-decreasing-string/)

难度简单33

给你一个字符串 `s` ，请你根据下面的算法重新构造字符串：

1. 从 `s` 中选出 **最小** 的字符，将它 **接在** 结果字符串的后面。
2. 从 `s` 剩余字符中选出 **最小** 的字符，且该字符比上一个添加的字符大，将它 **接在** 结果字符串后面。
3. 重复步骤 2 ，直到你没法从 `s` 中选择字符。
4. 从 `s` 中选出 **最大** 的字符，将它 **接在** 结果字符串的后面。
5. 从 `s` 剩余字符中选出 **最大** 的字符，且该字符比上一个添加的字符小，将它 **接在** 结果字符串后面。
6. 重复步骤 5 ，直到你没法从 `s` 中选择字符。
7. 重复步骤 1 到 6 ，直到 `s` 中所有字符都已经被选过。

在任何一步中，如果最小或者最大字符不止一个 ，你可以选择其中任意一个，并将其添加到结果字符串。

请你返回将 `s` 中字符重新排序后的 **结果字符串** 。

 

**示例 1：**

```
输入：s = "aaaabbbbcccc"
输出："abccbaabccba"
解释：第一轮的步骤 1，2，3 后，结果字符串为 result = "abc"
第一轮的步骤 4，5，6 后，结果字符串为 result = "abccba"
第一轮结束，现在 s = "aabbcc" ，我们再次回到步骤 1
第二轮的步骤 1，2，3 后，结果字符串为 result = "abccbaabc"
第二轮的步骤 4，5，6 后，结果字符串为 result = "abccbaabccba"
```

**示例 2：**

```
输入：s = "rat"
输出："art"
解释：单词 "rat" 在上述算法重排序以后变成 "art"
```

**示例 3：**

```
输入：s = "leetcode"
输出："cdelotee"
```

**示例 4：**

```
输入：s = "ggggggg"
输出："ggggggg"
```

**示例 5：**

```
输入：s = "spo"
输出："ops"
```

 

**提示：**

- `1 <= s.length <= 500`
- `s` 只包含小写英文字母。

这题描述有问题。比如说aaabbb。我取出一个a，那么剩余字符是aabbb，此时最小字符应该是a。而按照样例来看应该是b。

按照样例来看这题就很简单了。无非就是字典序升字典序降，字典序升字典序降

~~~cpp
class Solution
{
public:
    string sortString(string s)
    {
        vector<int> c(26);
        string ans;
        for (auto ch : s)
            c[ch - 'a']++;
        bool flag = true;
        while (flag)
        {
            flag = false;
            for (int i = 0; i < 26; i++)
            {
                if (c[i] > 0)
                {
                    ans += (char)('a' + i);
                    c[i]--;
                    flag = true;
                }
            }
            for (int i = 25; i >= 0; i--)
            {
                if (c[i] > 0)
                {
                    ans += (char)('a' + i);
                    c[i]--;
                    flag = true;
                }
            }
        }
        return ans;
    }
};
~~~

击败81%

### Q[164最大间距](https://leetcode-cn.com/problems/maximum-gap/)

难度困难224

给定一个无序的数组，找出数组在排序之后，相邻元素之间最大的差值。

如果数组元素个数小于 2，则返回 0。

**示例 1:**

```
输入: [3,6,9,1]
输出: 3
解释: 排序后的数组是 [1,3,6,9], 其中相邻元素 (3,6) 和 (6,9) 之间都存在最大差值 3。
```

**示例 2:**

```
输入: [10]
输出: 0
解释: 数组元素个数小于 2，因此返回 0。
```

**说明:**

- 你可以假设数组中所有元素都是非负整数，且数值在 32 位有符号整数范围内。
- 请尝试在线性时间复杂度和空间复杂度的条件下解决此问题。

用set，其实不符合题目要求

~~~cpp
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        set<int> s;
        for(auto a : nums){
            s.insert(a);
        }
        auto it = s.begin();
        int pre = *it;
        int m = 0;
        while(it!=s.end()){
            m = max(m,abs(pre-*it));
            pre = *it;
            it++;
        }
        return m;
    }
};
~~~

击败8% 5%

桶排序

两个数的间距不会比(Max - Min) / (nums.size() - 1)大，建立大小为(Max - Min) / (nums.size() - 1)的桶，每个桶只存最大值和最小值

~~~cpp
class Solution
{
public:
    int maximumGap(vector<int> &nums)
    {
        if(nums.size()==0||nums.size()==1)
            return 0;
        int Min = INT_MAX, Max = INT_MIN;
        for (auto i : nums)
        {
            Min = min(i, Min);
            Max = max(i, Max);
        }
        if(Max == Min)
            return 0;
        int n = (Max - Min) / (nums.size() - 1);
        if(n<1)
            n = 1;
        int length = (Max - Min) / n + 1;
        vector<pair<int, int>> b(length, pair<int, int>(INT_MAX, INT_MIN));

        for (auto i : nums)
        {
            b[(i - Min) / n].first = min(b[(i - Min) / n].first, i);
            b[(i - Min) / n].second = max(b[(i - Min) / n].second, i);
        }
        int prex = b[0].second;
        int m = 0;
        for (int i = 1; i < length; ++i)
        {
            if (b[i].first != INT_MAX)
            {
                m = max(b[i].first-prex, m);
            }
            if (b[i].second != INT_MIN)
            {
                prex = b[i].second;
            }
        }
        return m;
    }
};
~~~

击败79% 15%

我用set是24ms，直接排序16ms，桶排12ms

### Q[179最大数](https://leetcode-cn.com/problems/largest-number/)

难度中等430

给定一组非负整数 `nums`，重新排列它们每个数字的顺序（每个数字不可拆分）使之组成一个最大的整数。

**注意：**输出结果可能非常大，所以你需要返回一个字符串而不是整数。

 

**示例 1：**

```
输入：nums = [10,2]
输出："210"
```

**示例 2：**

```
输入：nums = [3,30,34,5,9]
输出："9534330"
```

**示例 3：**

```
输入：nums = [1]
输出："1"
```

**示例 4：**

```
输入：nums = [10]
输出："10"
```

 

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 109`

刚开始以为直接按字典序排序就好了，写的时候就想到了点问题

看题解其实就是按照 两个字符串a+b>b+a这样判断

但是sort函数却一直有问题，查阅了一些资料发现，sort函数要求相等时返回false

~~~cpp
class Solution
{
public:
    string largestNumber(vector<int> &nums)
    {
        vector<string> s1;
        for (auto i : nums)
        {
                s1.push_back(to_string(i));
        }
        sort(s1.begin(), s1.end(), [](string a, string b) {
            if (a + b > b + a)
                return true;
            return false;
        });
        string ans;
        for (auto i : s1)
        {
            if (ans.size()>0||i != "0")
                ans += i;
        }
        if (ans.size() == 0)
            ans = "0";
        return ans;
    }
};
~~~

击败65%

### Q[493翻转对](https://leetcode-cn.com/problems/reverse-pairs/)

难度困难162

给定一个数组 `nums` ，如果 `i < j` 且 `nums[i] > 2*nums[j]` 我们就将 `(i, j)` 称作一个***重要翻转对\***。

你需要返回给定数组中的重要翻转对的数量。

**示例 1:**

```
输入: [1,3,2,3,1]
输出: 2
```

**示例 2:**

```
输入: [2,4,3,5,1]
输出: 3
```

**注意:**

1. 给定数组的长度不会超过`50000`。
2. 输入数组中的所有数字都在32位整数的表示范围内。

一看到就猜到是分治法。合并数组前统计下次数

~~~cpp
class Solution {
public:
    int count = 0;
    int reversePairs(vector<int>& nums) {
        vector<int> aux(nums.size());
        merge(nums,aux,0,nums.size()-1);
        for(auto i:nums){
            cout<<i<<" ";
        }
        return count;
    }
    void merge(vector<int>& nums,vector<int>& aux,int left,int right) {
        if(left>=right) 
            return;
        int mid = (left+right)/2;
        merge(nums,aux,left,mid);
        merge(nums,aux,mid+1,right);
        for(int i = left; i <= right; ++i){
            aux[i] = nums[i];
        }
        int x1 = left;
        int x2 = mid+1;

        while(x1<=mid && x2<=right){
            if(((long)nums[x2]*2)<(long)nums[x1]){
                count+=(mid-x1+1);
                x2++;
            }else{
                x1++;
            }
        }
        x1 = left;
        x2 =mid+1;

        for(int i = left; i<=right; ++i){
            if(x1>mid){
                nums[i] = aux[x2++];
            }
            else if(x2>right){
                nums[i] = aux[x1++];
            }
            else if(aux[x1]>aux[x2]){
                nums[i] = aux[x2++];
            }
            else{
                nums[i] = aux[x1++];
            }
        }
    }
};
~~~

击败53%

### Q[976三角形的最大周长](https://leetcode-cn.com/problems/largest-perimeter-triangle/)

难度简单92

给定由一些正数（代表长度）组成的数组 `A`，返回由其中三个长度组成的、**面积不为零**的三角形的最大周长。

如果不能形成任何面积不为零的三角形，返回 `0`。

 



**示例 1：**

```
输入：[2,1,2]
输出：5
```

**示例 2：**

```
输入：[1,2,1]
输出：0
```

**示例 3：**

```
输入：[3,2,3,4]
输出：10
```

**示例 4：**

```
输入：[3,6,2,3]
输出：8
```

 

**提示：**

1. `3 <= A.length <= 10000`
2. `1 <= A[i] <= 10^6`

排个序nlogn，从A.size()-1开始遍历O（n），检测i i-1 i-2能否构成三角形（贪心算法）

~~~~cpp
class Solution {
public:
    int largestPerimeter(vector<int>& A) {
        if(A.size()<3) return 0;
        sort(A.begin(),A.end());
        for(int i = A.size()-1; i >= 2; --i){
            if(A[i-1] + A[i-2] > A[i]){
                return A[i]+A[i-1] + A[i-2];
            }
        }
        return 0;
    }
};
~~~~

击败73%

### Q[767重构字符串](https://leetcode-cn.com/problems/reorganize-string/)

难度中等161

给定一个字符串`S`，检查是否能重新排布其中的字母，使得两相邻的字符不同。

若可行，输出任意可行的结果。若不可行，返回空字符串。

**示例 1:**

```
输入: S = "aab"
输出: "aba"
```

**示例 2:**

```
输入: S = "aaab"
输出: ""
```

**注意:**

- `S` 只包含小写字母并且长度在`[1, 500]`区间内。

一开始就想到思路了。先统计次数，然后优先队列，每次取最大的两个。

但是不知道什么时候返回“”

> 当 n 是偶数时，有 n/2n/2 个偶数下标和 n/2n/2 个奇数下标，因此每个字母的出现次数都不能超过 n/2n/2 次，否则出现次数最多的字母一定会出现相邻。

> 当 n是奇数时，由于共有 (n+1)/2(n+1)/2 个偶数下标，因此每个字母的出现次数都不能超过 (n+1)/2(n+1)/2 次，否则出现次数最多的字母一定会出现相邻。

写优先队列遇到了点小问题

~~~cpp
auto cmp = [](pair<int,char>a,pair<int,char>b)->bool{
            if(a.first < b.first)
                return true;
            return false;
        };
priority_queue<pair<int,char>,vector<pair<int,char>>,decltype(cmp)> queue(cmp);
~~~

~~~cpp
class Solution {
public:
    string reorganizeString(string S) {
        vector<int> v(26);
        for(int i = 0; i < S.length(); ++i)
            v[S[i]-'a']++;
        if(S.size()%2==0){
            for(int i = 0; i < 26; ++i){
                if(v[i]>S.length()/2)
                    return "";
            }
        }
        if(S.size()%2==1){
            for(int i = 0; i < 26; ++i){
                if(v[i]>(S.length()+1)/2)
                    return "";
            }
        }
        priority_queue<pair<int,char>> queue;
        for(int i = 0; i < 26;++i){
            if(v[i]!=0){
                queue.push(pair<int,char>(v[i],'a' + i));
            }
        }
        string ans;
        while(queue.size()>1){
            pair<int,char> a = queue.top();
            queue.pop();
            pair<int,char> b = queue.top();
            queue.pop();
            ans += a.second;
            ans+=b.second;
            a.first--;
            b.first--;
            if(a.first>0){
                queue.push(a);
            }
            if(b.first>0){
                queue.push(b);
            }
        }
        if(queue.size()>0){
            if(queue.top().first>1)
                return "";
            ans += queue.top().second;
        }
        return ans;
    }
};
~~~

击败26%

### Q[321拼接最大数](https://leetcode-cn.com/problems/create-maximum-number/)

难度困难187

给定长度分别为 `m` 和 `n` 的两个数组，其元素由 `0-9` 构成，表示两个自然数各位上的数字。现在从这两个数组中选出 `k (k <= m + n)` 个数字拼接成一个新的数，要求从同一个数组中取出的数字保持其在原数组中的相对顺序。

求满足该条件的最大数。结果返回一个表示该最大数的长度为 `k` 的数组。

**说明:** 请尽可能地优化你算法的时间和空间复杂度。

**示例 1:**

```
输入:
nums1 = [3, 4, 6, 5]
nums2 = [9, 1, 2, 5, 8, 3]
k = 5
输出:
[9, 8, 6, 5, 3]
```

**示例 2:**

```
输入:
nums1 = [6, 7]
nums2 = [6, 0, 4]
k = 5
输出:
[6, 7, 6, 0, 4]
```

**示例 3:**

```
输入:
nums1 = [3, 9]
nums2 = [8, 9]
k = 3
输出:
[9, 8, 9]
```

看到这题我就想到了贪心算法，每次都尽量找最大的，尽可能往后压缩

写出来后，就只能过一个样例。

如果nums1 和 nums2 都有相同的最大的时候，这个算法就挂了。不知道应该去选择哪一个。我就想，相等时再写个循环判断下一个。但是这样子还是有问题，如果下个还是相等，而且代码会很冗余。

看到官方题解只有一个单调栈，我觉得贪心击败没戏了。

又按标签找到了一个c++ 贪心。

用一个set存下所有相等的点，每次循环一遍set

~~~cpp
class Solution
{
public:
    vector<int> maxNumber(vector<int> &nums1, vector<int> &nums2, int k)
    {
        vector<int> ans(k);
        int n1 = nums1.size();
        int n2 = nums2.size();

        set<pair<int, int>> s;
        s.insert(pair<int, int>(0, 0));
        for (int i = 0; i < k; i++)
        {
            set<pair<int, int>> temp;
            for (auto &p : s)
            {
                int prem1 = p.first;
                int prem2 = p.second;
                int l1 = n1 - prem1;
                int l2 = n2 - prem2;

                int m1 = prem1;
                int m2 = prem2;

                for (int j = prem1; m1 < n1 && j < n1 && n1 - j + l2 >= k - i; ++j)
                {
                    if (nums1[j] > nums1[m1])
                        m1 = j;
                }
                if (m1 < n1 && nums1[m1] >= ans[i])
                {
                    if (nums1[m1] > ans[i])
                        temp.clear();
                    ans[i] = nums1[m1];
                    temp.insert(pair<int, int>(m1+1, prem2));
                }
                for (int j = prem2; m2 < n2 && j < n2 && n2 - j + l1 >= k - i; ++j)
                {
                    if (nums2[j] > nums2[m2])
                        m2 = j;
                }
                if (m2 < n2 && nums2[m2] >= ans[i])
                {
                    if (nums2[m2] > ans[i])
                        temp.clear();
                    ans[i] = nums2[m2];
                    temp.insert(pair<int, int>(prem1, m2+1));
                }
            }
            s.swap(temp);
        }
        return ans;
    }
};
~~~

击败99%，感觉是第一次写困难这么爽。找最大值可以用stl的算法。

### Q[659分割数组为连续子序列](https://leetcode-cn.com/problems/split-array-into-consecutive-subsequences/)

难度中等148

给你一个按升序排序的整数数组 `num`（可能包含重复数字），请你将它们分割成一个或多个子序列，其中每个子序列都由连续整数组成且长度至少为 3 。

如果可以完成上述分割，则返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入: [1,2,3,3,4,5]
输出: True
解释:
你可以分割出这样两个连续子序列 : 
1, 2, 3
3, 4, 5
```

 

**示例 2：**

```
输入: [1,2,3,3,4,4,5,5]
输出: True
解释:
你可以分割出这样两个连续子序列 : 
1, 2, 3, 4, 5
3, 4, 5
```

 

**示例 3：**

```
输入: [1,2,3,4,4,5]
输出: False
```

 

**提示：**

1. 输入的数组长度范围为 [1, 10000]

刚开始往dp想了，因为贪心很难搞，当前数到底应该算前面还是算后面

其实我漏了个条件，这题是连续数

看了题解，hashmap + prioityqueue，每个数都尽量往前塞，而且是往前一个的最短序列里塞。

~~~cpp
class Solution {
public:
    bool isPossible(vector<int>& nums) {
        unordered_map<int,priority_queue<int,vector<int>,greater<int>>> m;
        for(int i = 0; i < nums.size(); ++i) {
            if(m[nums[i]-1].size()!=0){
                int x = m[nums[i]-1].top(); 
                m[nums[i]-1].pop();
                m[nums[i]].push(x+1);
            }
            else{
                m[nums[i]].push(1);
            }
        }
        auto it = m.begin();
        while(it != m.end()){
            if(it->second.size()>0){
               if(it->second.top()<3)
                    return false; 
            }
            it++;
        }
        return true;
    }
};
~~~

击败5%

### Q[1550存在连续三个奇数的数组](https://leetcode-cn.com/problems/three-consecutive-odds/)

难度简单5

给你一个整数数组 `arr`，请你判断数组中是否存在连续三个元素都是奇数的情况：如果存在，请返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：arr = [2,6,4,1]
输出：false
解释：不存在连续三个元素都是奇数的情况。
```

**示例 2：**

```
输入：arr = [1,2,34,3,4,5,7,23,12]
输出：true
解释：存在连续三个元素都是奇数的情况，即 [5,7,23] 。
```

 

**提示：**

- `1 <= arr.length <= 1000`
- `1 <= arr[i] <= 1000`

~~~cpp
class Solution {
public:
    bool threeConsecutiveOdds(vector<int>& arr) {
        int count = 0;
        for(int i:arr){
            if(i%2==0){
                count = 0;
            }else{
                count++;
                if(count == 3)
                    return true;
            }
        }
        return false;
    }
};
~~~

击败72%

### Q[1451重新排列句子中的单词](https://leetcode-cn.com/problems/rearrange-words-in-a-sentence/)

难度中等13

「句子」是一个用空格分隔单词的字符串。给你一个满足下述格式的句子 `text` :

- 句子的首字母大写
- `text` 中的每个单词都用单个空格分隔。

请你重新排列 `text` 中的单词，使所有单词按其长度的升序排列。如果两个单词的长度相同，则保留其在原句子中的相对顺序。

请同样按上述格式返回新的句子。

 

**示例 1：**

```
输入：text = "Leetcode is cool"
输出："Is cool leetcode"
解释：句子中共有 3 个单词，长度为 8 的 "Leetcode" ，长度为 2 的 "is" 以及长度为 4 的 "cool" 。
输出需要按单词的长度升序排列，新句子中的第一个单词首字母需要大写。
```

**示例 2：**

```
输入：text = "Keep calm and code on"
输出："On and keep calm code"
解释：输出的排序情况如下：
"On" 2 个字母。
"and" 3 个字母。
"keep" 4 个字母，因为存在长度相同的其他单词，所以它们之间需要保留在原句子中的相对顺序。
"calm" 4 个字母。
"code" 4 个字母。
```

**示例 3：**

```
输入：text = "To be or not to be"
输出："To be or to be not"
```

 

**提示：**

- `text` 以大写字母开头，然后包含若干小写字母以及单词间的单个空格。
- `1 <= text.length <= 10^5`

熟悉stl就可以秒杀

~~~cpp
class Solution
{
public:
    string arrangeWords(string text)
    {
        text[0] = tolower(text[0]);
        vector<string> array;
        int prepos = 0;
        for (int i = 0; i < text.length(); i++)
        {
            if (text[i] == ' ')
            {
                array.push_back(text.substr(prepos, i - prepos));
                prepos = i + 1;
            }
        }
        array.push_back(text.substr(prepos));
        auto fun = [](string s1, string s2) {
            if (s1.length() < s2.length())
                return true;
            return false;
        };
        stable_sort(array.begin(), array.end(), fun);
        string ans;
        for (int i = 0; i < array.size(); i++)
        {
            if (i == 0)
            {
                ans = ans + (char)toupper(array[i][0]) + array[i].substr(1);
            }
            else
                ans += array[i];
            ans += ' ';
        }
        return ans.substr(0, ans.length()-1);
    }
};
~~~

击败67%

### Q[735行星碰撞](https://leetcode-cn.com/problems/asteroid-collision/)

难度中等117

给定一个整数数组 `asteroids`，表示在同一行的行星。

对于数组中的每一个元素，其绝对值表示行星的大小，正负表示行星的移动方向（正表示向右移动，负表示向左移动）。每一颗行星以相同的速度移动。

找出碰撞后剩下的所有行星。碰撞规则：两个行星相互碰撞，较小的行星会爆炸。如果两颗行星大小相同，则两颗行星都会爆炸。两颗移动方向相同的行星，永远不会发生碰撞。

**示例 1:**

```
输入: 
asteroids = [5, 10, -5]
输出: [5, 10]
解释: 
10 和 -5 碰撞后只剩下 10。 5 和 10 永远不会发生碰撞。
```

**示例 2:**

```
输入: 
asteroids = [8, -8]
输出: []
解释: 
8 和 -8 碰撞后，两者都发生爆炸。
```

**示例 3:**

```
输入: 
asteroids = [10, 2, -5]
输出: [10]
解释: 
2 和 -5 发生碰撞后剩下 -5。10 和 -5 发生碰撞后剩下 10。
```

**示例 4:**

```
输入: 
asteroids = [-2, -1, 1, 2]
输出: [-2, -1, 1, 2]
解释: 
-2 和 -1 向左移动，而 1 和 2 向右移动。
由于移动方向相同的行星不会发生碰撞，所以最终没有行星发生碰撞。
```

**说明:**

- 数组 `asteroids` 的长度不超过 `10000`。
- 每一颗行星的大小都是非零整数，范围是 `[-1000, 1000]` 。

他这个碰撞类似一个栈结构，先进后出。

最后汇总答案时，需要先进先出

所以用deque

~~~cpp
class Solution
{
public:
    vector<int> asteroidCollision(vector<int> &asteroids)
    {
        deque<int> q;
        vector<int> ans;
        for (int i : asteroids)
        {
            if (i > 0)
            {
                q.push_back(i);
            }
            else if (i < 0)
            {
                bool flag = true;
                while (q.size() > 0)
                {
                    if (abs(i) < abs(q.back()))
                    {
                        break;
                    }
                    else if (abs(i) == abs(q.back()))
                    {
                        q.pop_back();
                        flag = false;
                        break;
                    }
                    else
                    {
                        q.pop_back();
                    }
                }
                if (flag && q.empty())
                    ans.push_back(i);
            }
        }
        for (auto i : q)
        {
            ans.push_back(i);
        }
        return ans;
    }
};
~~~

击败8%

### Q[861翻转矩阵后的得分](https://leetcode-cn.com/problems/score-after-flipping-matrix/)

难度中等94

有一个二维矩阵 `A` 其中每个元素的值为 `0` 或 `1` 。

移动是指选择任一行或列，并转换该行或列中的每一个值：将所有 `0` 都更改为 `1`，将所有 `1` 都更改为 `0`。

在做出任意次数的移动后，将该矩阵的每一行都按照二进制数来解释，矩阵的得分就是这些数字的总和。

返回尽可能高的分数。

 



**示例：**

```
输入：[[0,0,1,1],[1,0,1,0],[1,1,0,0]]
输出：39
解释：
转换为 [[1,1,1,1],[1,0,0,1],[1,1,1,1]]
0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39
```

 

**提示：**

1. `1 <= A.length <= 20`
2. `1 <= A[0].length <= 20`
3. `A[i][j]` 是 `0` 或 `1`

一开始的思路其实很接近了。我没注意到列也可以反转。如果只是行可以反转，那么只要看最高位就好了。而列反转则需看每一列0的个数。

先进行行反转，再进行列反转

~~~cpp
class Solution {
public:
    int matrixScore(vector<vector<int>>& A) {
        int sum = 0;
        vector<bool> flag(A.size());
        for(int i = 0; i < A.size(); i++) {
            flag[i] = (A[i][0] == 0);
        }
        sum += ((A.size())*(1<<(A[0].size()-1)));
        for(int i = 1; i < A[0].size(); i++) {
            int count = 0;
            for(int j = 0; j < A.size(); j++) {
                if(A[j][i] == 0 && flag[j] == false){
                    count++;
                }
                else if(A[j][i] == 1 && flag[j] == true){
                    count++;
                }
            }
            if((double)count<(double)(A.size())/2){
                sum+=((A.size()-count)*(1<<(A[0].size()-i-1)));
            }
            else{
                sum+=((count)*(1<<(A[0].size()-i-1)));
            }
        }
        return sum;
    }
};
~~~

击败83%

### Q[842将数组拆分成斐波那契序列](https://leetcode-cn.com/problems/split-array-into-fibonacci-sequence/)

难度中等89

给定一个数字字符串 `S`，比如 `S = "123456579"`，我们可以将它分成斐波那契式的序列 `[123, 456, 579]`。

形式上，斐波那契式序列是一个非负整数列表 `F`，且满足：

- `0 <= F[i] <= 2^31 - 1`，（也就是说，每个整数都符合 32 位有符号整数类型）；
- `F.length >= 3`；
- 对于所有的`0 <= i < F.length - 2`，都有 `F[i] + F[i+1] = F[i+2]` 成立。

另外，请注意，将字符串拆分成小块时，每个块的数字一定不要以零开头，除非这个块是数字 0 本身。

返回从 `S` 拆分出来的任意一组斐波那契式的序列块，如果不能拆分则返回 `[]`。

 

**示例 1：**

```
输入："123456579"
输出：[123,456,579]
```

**示例 2：**

```
输入: "11235813"
输出: [1,1,2,3,5,8,13]
```

**示例 3：**

```
输入: "112358130"
输出: []
解释: 这项任务无法完成。
```

**示例 4：**

```
输入："0123"
输出：[]
解释：每个块的数字不能以零开头，因此 "01"，"2"，"3" 不是有效答案。
```

**示例 5：**

```
输入: "1101111"
输出: [110, 1, 111]
解释: 输出 [11,0,11,11] 也同样被接受。
```

 

**提示：**

1. `1 <= S.length <= 200`
2. 字符串 `S` 中只含有数字。

今天这个中等写的特别艰难。暴力回溯法，穷举每个点可不可以。

刚开始函数里最前面的判断没写好，出现了很奇怪的结果，然后一直搞不懂哪里错了。

被int的长度卡了两发。我用istringstream他竟然不会检测int有没有越界

~~~cpp
class Solution
{
public:
    vector<int> splitIntoFibonacci(string S)
    {
        vector<int> result;
        backward(result, S, 1, 0);
        if (result.size() < 3)
            return {};
        return result;
    }
    bool backward(vector<int> &result, string S, int pos, int prepos)
    {
        if (pos > S.size())
            return result.size() >= 3 && prepos == S.size();
        if (pos - prepos > 1 && S[prepos] == '0')
            return false;
        istringstream is(S.substr(prepos, pos - prepos));
        long y;
        int x;
        is >> y;
        x = (int)y;
        if(x != y)
            return false;
        if (result.size() < 2)
        {
            result.push_back(x);
            if (backward(result, S, pos + 1, pos))
                return true;
            result.pop_back();
        }
        else
        {
            int n = result.size();
            long comp = (long)x - (long)result[n - 1] - (long)result[n - 2];
            if (comp == 0)
            {
                result.push_back(x);
                if (backward(result, S, pos + 1, pos))
                    return true;
                result.pop_back();
            }
            else if (comp > 0)
            {
                return false;
            }
        }
        return backward(result, S, pos + 1, prepos);
    }
};
~~~

击败17

### Q[860柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/)

难度简单142

在柠檬水摊上，每一杯柠檬水的售价为 `5` 美元。

顾客排队购买你的产品，（按账单 `bills` 支付的顺序）一次购买一杯。

每位顾客只买一杯柠檬水，然后向你付 `5` 美元、`10` 美元或 `20` 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 `5` 美元。

注意，一开始你手头没有任何零钱。

如果你能给每位顾客正确找零，返回 `true` ，否则返回 `false` 。

**示例 1：**

```
输入：[5,5,5,10,20]
输出：true
解释：
前 3 位顾客那里，我们按顺序收取 3 张 5 美元的钞票。
第 4 位顾客那里，我们收取一张 10 美元的钞票，并返还 5 美元。
第 5 位顾客那里，我们找还一张 10 美元的钞票和一张 5 美元的钞票。
由于所有客户都得到了正确的找零，所以我们输出 true。
```

**示例 2：**

```
输入：[5,5,10]
输出：true
```

**示例 3：**

```
输入：[10,10]
输出：false
```

**示例 4：**

```
输入：[5,5,10,10,20]
输出：false
解释：
前 2 位顾客那里，我们按顺序收取 2 张 5 美元的钞票。
对于接下来的 2 位顾客，我们收取一张 10 美元的钞票，然后返还 5 美元。
对于最后一位顾客，我们无法退回 15 美元，因为我们现在只有两张 10 美元的钞票。
由于不是每位顾客都得到了正确的找零，所以答案是 false。
```

 

**提示：**

- `0 <= bills.length <= 10000`
- `bills[i]` 不是 `5` 就是 `10` 或是 `20` 

第一发搞错了，20块只找了两张五

~~~java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int[] x = new int[3];
        for(int i:bills){
            if(i == 20){
                if(x[1] >0){
                    if(x[0]>0){
                        x[1]-=1;
                        x[0]-=1;
                    }
                    else
                        return false;
                }
                else{
                    if(x[0]>2){
                        x[0]-=3;
                    }else{
                        return false;
                    }
                }
            }
            else if(i == 10){
                if(x[0]>0){
                    x[0]--;
                    x[1]++;
                }else{
                    return false;
                }
            }else{
                x[0]++;
            }

        }
        return true;
    }
}
~~~

击败99%

### Q[649Dota2 参议院](https://leetcode-cn.com/problems/dota2-senate/)

难度中等105

Dota2 的世界里有两个阵营：`Radiant`(天辉)和 `Dire`(夜魇)

Dota2 参议院由来自两派的参议员组成。现在参议院希望对一个 Dota2 游戏里的改变作出决定。他们以一个基于轮为过程的投票进行。在每一轮中，每一位参议员都可以行使两项权利中的`**一**`项：

1. `禁止一名参议员的权利`：

   参议员可以让另一位参议员在这一轮和随后的几轮中丧失**所有的权利**。

2. `宣布胜利`：

​     如果参议员发现有权利投票的参议员都是**同一个阵营的**，他可以宣布胜利并决定在游戏中的有关变化。

 

给定一个字符串代表每个参议员的阵营。字母 “R” 和 “D” 分别代表了 `Radiant`（天辉）和 `Dire`（夜魇）。然后，如果有 `n` 个参议员，给定字符串的大小将是 `n`。

以轮为基础的过程从给定顺序的第一个参议员开始到最后一个参议员结束。这一过程将持续到投票结束。所有失去权利的参议员将在过程中被跳过。

假设每一位参议员都足够聪明，会为自己的政党做出最好的策略，你需要预测哪一方最终会宣布胜利并在 Dota2 游戏中决定改变。输出应该是 `Radiant` 或 `Dire`。

 

**示例 1：**

```
输入："RD"
输出："Radiant"
解释：第一个参议员来自 Radiant 阵营并且他可以使用第一项权利让第二个参议员失去权力，因此第二个参议员将被跳过因为他没有任何权利。然后在第二轮的时候，第一个参议员可以宣布胜利，因为他是唯一一个有投票权的人
```

**示例 2：**

```
输入："RDD"
输出："Dire"
解释：
第一轮中,第一个来自 Radiant 阵营的参议员可以使用第一项权利禁止第二个参议员的权利
第二个来自 Dire 阵营的参议员会被跳过因为他的权利被禁止
第三个来自 Dire 阵营的参议员可以使用他的第一项权利禁止第一个参议员的权利
因此在第二轮只剩下第三个参议员拥有投票的权利,于是他可以宣布胜利
```

 

**提示：**

- 给定字符串的长度在 `[1, 10,000]` 之间.

官方题解写的很巧妙，用一个队列。如果r[ 0 ] <d [ 0 ]，则把r [ 0 ] + n扔到r队尾。

~~~cpp
class Solution {
public:
    string predictPartyVictory(string senate) {
        int n = senate.size();
        deque<int> r;
        deque<int> d;
        for(int i = 0; i < n; i++) {
            if(senate[i] == 'R')
                r.push_back(i);
            else
                d.push_back(i);
        }
        while(r.size()>0&&d.size()>0){
            if(r[0]<d[0]){
                r.push_back(r[0]+n);
            }
            else{
                d.push_back(d[0]+n);
            }
            d.pop_front();
            r.pop_front();
        }
        if(r.size()>0)
            return "Radiant";
        return "Dire";
    }
};
~~~

击败29%

### Q[376摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

难度中等292

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为**摆动序列。**第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。

例如， `[1,7,4,9,2,5]` 是一个摆动序列，因为差值 `(6,-3,5,-7,3)` 是正负交替出现的。相反, `[1,4,7,2,5]` 和 `[1,7,4,5,5]` 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。

**示例 1:**

```
输入: [1,7,4,9,2,5]
输出: 6 
解释: 整个序列均为摆动序列。
```

**示例 2:**

```
输入: [1,17,5,10,13,15,10,5,16,8]
输出: 7
解释: 这个序列包含几个长度为 7 摆动序列，其中一个可为[1,17,10,13,10,16,8]。
```

**示例 3:**

```
输入: [1,2,3,4,5,6,7,8,9]
输出: 2
```

**进阶:**
你能否用 O(*n*) 时间复杂度完成此题?

看了官方题解的dp方程，并不是很理解（画几个例子大概就懂了。

~~~cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size()<1)
            return 0;
        vector<vector<int>> dp(nums.size(),vector<int>(2));
        dp[0][0] = 1;
        dp[0][1] = 1;
        for(int i = 1; i < nums.size(); i++) {
            if(nums[i]>nums[i-1]){
                dp[i][0] = max(dp[i-1][0],dp[i-1][1]+1);
                dp[i][1] = dp[i-1][1];
            }
            else if(nums[i]==nums[i-1]){
                dp[i][0] = dp[i-1][0];
                dp[i][1] = dp[i-1][1];
            }
            else {
                dp[i][0] = dp[i-1][0];
                dp[i][1] = max(dp[i-1][0]+1,dp[i-1][1]);
            }
        }
        int n = nums.size();
        return max(dp[n-1][0],dp[n-1][1]);
    }
};
~~~

击败14%

### Q[738单调递增的数字](https://leetcode-cn.com/problems/monotone-increasing-digits/)

难度中等90

给定一个非负整数 `N`，找出小于或等于 `N` 的最大的整数，同时这个整数需要满足其各个位数上的数字是单调递增。

（当且仅当每个相邻位数上的数字 `x` 和 `y` 满足 `x <= y` 时，我们称这个整数是单调递增的。）

**示例 1:**

```
输入: N = 10
输出: 9
```

**示例 2:**

```
输入: N = 1234
输出: 1234
```

**示例 3:**

```
输入: N = 332
输出: 299
```

**说明:** `N` 是在 `[0, 10^9]` 范围内的一个整数。

自己试了半天，写的乱七八糟

抄题：思路：从后往前遍历，如果前面的值大于后面的值就把当前位数减一然后把后面的值变成9，以此类推

~~~cpp
class Solution
{
public:
    int monotoneIncreasingDigits(int N)
    {
        shared_ptr<stack<char>> s1 = make_shared<stack<char>>(stack<char>());
        string num = to_string(N);
        int n = num.length();
        s1->push(num[n-1]);
        for (int i = n-2; i >= 0; i--) {
            if(num[i]>s1->top()){
                int count = s1->size();
                s1 = make_shared<stack<char>>(stack<char>());
                while(count>0){
                    s1->push('9');
                    count--;
                }
                s1->push(num[i]-1);
            }
            else{
                s1->push(num[i]);
            }
        }        
        int ans = 0;
        while(s1->size()>0){
            ans = ans*10 + (s1->top()-'0');
            s1->pop();
        }
        return ans;
    }
};
~~~

击败42%

### Q[290单词规律](https://leetcode-cn.com/problems/word-pattern/)

难度简单228

给定一种规律 `pattern` 和一个字符串 `str` ，判断 `str` 是否遵循相同的规律。

这里的 **遵循** 指完全匹配，例如， `pattern` 里的每个字母和字符串 `str` 中的每个非空单词之间存在着双向连接的对应规律。

**示例1:**

```
输入: pattern = "abba", str = "dog cat cat dog"
输出: true
```

**示例 2:**

```
输入:pattern = "abba", str = "dog cat cat fish"
输出: false
```

**示例 3:**

```
输入: pattern = "aaaa", str = "dog cat cat dog"
输出: false
```

**示例 4:**

```
输入: pattern = "abba", str = "dog dog dog dog"
输出: false
```

**说明:**
你可以假设 `pattern` 只包含小写字母， `str` 包含了由单个空格分隔的小写字母。  

题目有点难看懂。就是pattern里相同字母对应s的单词必须相同。另外不同单词对应s的单词必须不同。搞两个map

~~~cpp
class Solution
{
public:
    bool wordPattern(string pattern, string s)
    {
        vector<string> words;
        int prepos = 0;
        for (int i = 0; i < s.length(); ++i)
        {
            if (s[i] == ' ')
            {
                words.push_back(s.substr(prepos, i - prepos));
                prepos = i + 1;
            }
        }
        words.push_back(s.substr(prepos, s.length()));
        if (words.size() != pattern.length())
        {
            return false;
        }
        unordered_map<char, string> m1;
        unordered_map<string, char> m2;
        int j = 0;
        for (auto i : pattern)
        {
            if (m1.find(i) == m1.end())
            {
                if (m2.find(words[j]) == m2.end()||m2[words[j]] == i)
                {
                    m2[words[j]] = i;
                    m1[i] = words[j];
                }
                else
                {
                    return false;
                }
            }
            else
            {
                if (m1[i] != words[j])
                {
                    return false;
                }
            }
            j++;
        }
        return true;
    }
};
~~~

击败100%

### Q[714买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

难度中等315

给定一个整数数组 `prices`，其中第 `i` 个元素代表了第 `i` 天的股票价格 ；非负整数 `fee` 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

**注意：**这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

**示例 1:**

```
输入: prices = [1, 3, 2, 8, 4, 9], fee = 2
输出: 8
解释: 能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

**注意:**

- `0 < prices.length <= 50000`.
- `0 < prices[i] < 50000`.
- `0 <= fee < 50000`.

我还记得没有手续费的dp怎么写。加上手续费也没啥区别

~~~cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        vector<vector<int>>dp(prices.size(),vector<int>(2));
        dp[0][0] = 0;
        dp[0][1] = -prices[0]-fee;
        for(int i = 1; i < prices.size(); i++) {
            dp[i][0] = max(dp[i-1][0],dp[i-1][1]+prices[i]);
            dp[i][1] = max(dp[i-1][1],dp[i-1][0]-prices[i]-fee);
        }
        return max(dp[prices.size()-1][0],dp[prices.size()-1][1]);
    }
};
~~~

击败21%

### Q[389找不同](https://leetcode-cn.com/problems/find-the-difference/)

难度简单180

给定两个字符串 ***s*** 和 ***t***，它们只包含小写字母。

字符串 ***t\*** 由字符串 ***s\*** 随机重排，然后在随机位置添加一个字母。

请找出在 ***t*** 中被添加的字母。

 

**示例 1：**

```
输入：s = "abcd", t = "abcde"
输出："e"
解释：'e' 是那个被添加的字母。
```

**示例 2：**

```
输入：s = "", t = "y"
输出："y"
```

**示例 3：**

```
输入：s = "a", t = "aa"
输出："a"
```

**示例 4：**

```
输入：s = "ae", t = "aea"
输出："a"
```

 

**提示：**

- `0 <= s.length <= 1000`
- `t.length == s.length + 1`
- `s` 和 `t` 只包含小写字母

~~~cpp
class Solution {
public:
    char findTheDifference(string s, string t) {
        unordered_map<char,int> m;
        for(auto ch:s){
            m[ch]++;
        }
        for(auto ch:t){
            if(m[ch]<=0)
                return ch;
            m[ch]--;
        }
        return -1;
    }
};
~~~

击败35%

### Q[746使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)

难度简单425

数组的每个索引作为一个阶梯，第 `i`个阶梯对应着一个非负数的体力花费值 `cost[i]`(索引从0开始)。

每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

**示例 1:**

```
输入: cost = [10, 15, 20]
输出: 15
解释: 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。
```

 **示例 2:**

```
输入: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出: 6
解释: 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6。
```

**注意：**

1. `cost` 的长度将会在 `[2, 1000]`。
2. 每一个 `cost[i]` 将会是一个Integer类型，范围为 `[0, 999]`。

~~~cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        if(cost.size()<2)
            return 0;
        vector<int>dp(cost.size());
        int n = cost.size();
        for(int i = 2; i < n; i++){
            dp[i] = min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return min(dp[n-1]+cost[n-1],dp[n-2]+cost[n-2]);
    }
};
~~~

击败62%

### Q[135分发糖果](https://leetcode-cn.com/problems/candy/)

难度困难321

老师想给孩子们分发糖果，有 *N* 个孩子站成了一条直线，老师会根据每个孩子的表现，预先给他们评分。

你需要按照以下要求，帮助老师给这些孩子分发糖果：

- 每个孩子至少分配到 1 个糖果。
- 相邻的孩子中，评分高的孩子必须获得更多的糖果。

那么这样下来，老师至少需要准备多少颗糖果呢？

**示例 1:**

```
输入: [1,0,2]
输出: 5
解释: 你可以分别给这三个孩子分发 2、1、2 颗糖果。
```

**示例 2:**

```
输入: [1,2,2]
输出: 4
解释: 你可以分别给这三个孩子分发 1、2、1 颗糖果。
     第三个孩子只得到 1 颗糖果，这已满足上述两个条件。
```

从左到右遍历一遍，再从右向左遍历一遍，取最大值

~~~cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> c(ratings.size());
        if(ratings.size()<1)
            return 0;
        c[0] = 1;
        for(int i = 1; i < ratings.size(); ++i) {
            if(ratings[i]> ratings[i-1])
                c[i] = c[i-1] + 1;
            else 
                c[i] = 1;
        }
        for(int i = ratings.size()-2; i>=0 ; --i) {
            if(ratings[i]> ratings[i+1])
                c[i] = max(c[i+1] + 1,c[i]);
        }
        int amount = 0;
        for(auto i:c){
            amount += i;
        }
        return amount;
    }
};
~~~

击败82%

### Q[455分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

难度简单235

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 `i`，都有一个胃口值 `g[i]`，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 `j`，都有一个尺寸 `s[j]` 。如果 `s[j] >= g[i]`，我们可以将这个饼干 `j` 分配给孩子 `i` ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

**示例 1:**

```
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
```

**示例 2:**

```
输入: g = [1,2], s = [1,2,3]
输出: 2
解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.
```

 

**提示：**

- `1 <= g.length <= 3 * 104`
- `0 <= s.length <= 3 * 104`
- `1 <= g[i], s[j] <= 231 - 1`

排个序，简单遍历

~~~cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int i = 0, j = 0;
        while(i<g.size() && j<s.size()) {
            if(g[i]<=s[j]) {
                i++;
                j++;
            }
            else{
                j++;
            }
        }
        return i;
    }
};
~~~

击败97%