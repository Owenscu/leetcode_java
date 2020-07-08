@[toc]
# 数组
## 1299. 将每个元素替换为右侧最大元素
[题目](https://leetcode-cn.com/problems/replace-elements-with-greatest-element-on-right-side/)
思路：
逆序解决，而且不能用多余的空间，
arr[i] = max(arr[i+1],res[i+1])
~~~java
import java.util.Map;
import java.util.Scanner;

public class Solution {

    public int[] replaceElements(int[] arr){
        int cur_max = arr[arr.length-1];
        arr[arr.length-1] = -1;

        for(int i = arr.length-2;i >=0;i--){
            int tmp = arr[i];
            arr[i] = Math.max(cur_max,arr[i+1]);
            cur_max = tmp;
        }
        return arr;
    }

    public static void main(String[] args){
        Solution test = new Solution();

        Scanner sc = new Scanner(System.in);
        String[] nums = null;
        nums = sc.nextLine().split(" ");
        int[] arr = new int[nums.length];
        for(int i = 0;i < nums.length;i++){
            arr[i] = Integer.valueOf(nums[i]);
        }

        int[] res = test.replaceElements(arr);
        for(int i:res){
            System.out.print(i+" ");
        }
    }
}

~~~

## 42.接雨水
[题目](https://leetcode-cn.com/problems/trapping-rain-water/)
思路：
记录该位置左边的最大值和右边的最大值，然后该位置接水量就是Math.min(right[i],left[i]) - height[i]
~~~java
import java.util.Scanner;

public class Solution {
    public int trap(int[] height){

        if (height.length < 3){
            return 0;
        }

        int[] right = new int[height.length];
        int[] left = new int[height.length];

        left[0] =  height[0];
        right[height.length - 1] = height[height.length - 1];

        for(int i = 1;i<height.length;i++){
            left[i] = Math.max(height[i],left[i-1]);
        }

        for(int i = height.length - 2 ;i >= 0;i--){
            right[i] = Math.max(height[i],right[i+1]);
        }

        int res = 0;
        for(int i = 0;i < height.length;i++){
            res += Math.min(right[i],left[i]) - height[i];
        }

        return res;
    }

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String[] nums = null;
        nums = sc.nextLine().split(" ");
        int[] height = new int[nums.length];
        for(int i=0;i<nums.length;i++){
            height[i] = Integer.valueOf(nums[i]);
        }

        Solution test = new Solution();
        System.out.println(test.trap(height));
    }
}

~~~

# 双指针
## 三数之和
[题目](https://leetcode-cn.com/problems/3sum/)
思路：
排序后，用双指针
注意：
1、排除重复答案的
~~~java
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }
~~~
2、判断tmp==0后不能直接left++ right--
~~~java
                     while(left < right && nums[left] == nums[left+1]){
                        left += 1;
                    }

                    while(left < right && nums[right] == nums[right - 1]){
                        right -= 1;
                    }
~~~

~~~java
import java.lang.reflect.Array;
import java.util.*;

public class Solution {
    public List<List<Integer>> threeSum(int[] nums){
        List<List<Integer>> res = new ArrayList<List<Integer>>();

        if(nums.length < 3){
            return res;
        }

        Arrays.sort(nums);

        for(int i = 0;i<nums.length-2;i++){

            // 处理 答案重复
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;
            while(left < right){
                int tmp = nums[i] + nums[left] + nums[right];
                if (tmp == 0){
                    List<Integer> cur = new ArrayList<>();
                    cur.add(nums[i]);
                    cur.add(nums[left]);
                    cur.add(nums[right]);
                    res.add(cur);

                    while(left < right && nums[left] == nums[left+1]){
                        left += 1;
                    }

                    while(left < right && nums[right] == nums[right - 1]){
                        right -= 1;
                    }

                    left += 1;
                    right -=1;
                }

                else if(tmp > 0){
                    right -= 1;
                }
                else{
                    left += 1;
                }
            }
        }
        return res;
    }

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String[] arr = null;
        arr = sc.nextLine().split(" ");
        int[] nums = new int[arr.length];
        for(int i = 0;i < arr.length;i++){
            nums[i] = Integer.valueOf(arr[i]);
        }

        Solution test = new Solution();
        System.out.println(test.threeSum(nums));
    }
}

~~~
# 链表
## 206.反转链表
~~~java
public class Solution {
    public ListNode reverseList(ListNode head){
        ListNode pre = null;
        ListNode cur = head;
        while(head != null){
            cur = head;
            head = head.next;    //注意把提前这一行放这里
            cur.next = pre;
            pre = cur;
        }
        return cur;
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next =  new ListNode(5);

        Solution test = new Solution();
        ListNode res = test.reverseList(head);
        while(res != null){
            System.out.print(res.val + " ");
            res = res.next;
        }
    }
}
~~~
## 反转链表2
[题目](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
思路
第一步：找到待反转节点的前一个节点。记录为beforInverNode，并记录开始反转的节点startInverNode
第二步：反转m到n这部分。
第三步：将反转的起点的next指向反转的后面一部分。
第四部：将startInverNode的next指向后面不需要反转的部分
注意反转完后链表的状态：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200706001603452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L293ZW5meQ==,size_16,color_FFFFFF,t_70)
所以需要:
~~~java
        beforeInverseNode.next = pre;
        startInverseNode.next = node;
~~~
~~~java
import java.util.List;

public class Solution {
    public ListNode reverseBetween(ListNode head,int m,int n){
        ListNode res = new ListNode(0);
        res.next = head;
        ListNode node = res;
        for(int i = 1;i < m;i++){
            node = node.next;
        }

        ListNode beforeInverseNode = node;
        node = node.next;
        ListNode startNode = node;
        ListNode pre = null;
        ListNode cur = null;
        for(int i = m;i <=n;i++){
            cur = node;
            node = node.next;
            cur.next = pre;
            pre = cur;
        }

        beforeInverseNode.next = pre;
        startNode.next = node;
        return res.next;
    }

    public static void main(String[] args) {

        int m = 2, n = 6;
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next =  new ListNode(3);
        head.next.next.next =  new ListNode(4);
        head.next.next.next.next =  new ListNode(5);
        head.next.next.next.next.next =  new ListNode(6);
        head.next.next.next.next.next.next =  new ListNode(7);

        Solution test = new Solution();
        ListNode res = test.reverseBetween(head,m,n);
        while(res != null){
            System.out.print(res.val + " ");
            res = res.next;
        }
    }
}
~~~
## k个一组翻转链表
[题目](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode res = new ListNode(0);
        res.next = head;
        ListNode lastNode = res;
        while(head != null){
            ListNode startNode = head;
            int count = 0;
            ListNode headd = null;
            while(head != null && count <k){
                count ++;
                if(count == k){
                    headd = head;
                }
                head = head.next;
            }
            if(headd != null){
                headd.next = null;
            }

            if(count == k){

                ListNode endNode = reverse(startNode);
                lastNode.next = endNode;
                lastNode = startNode;
            }
            else{
                lastNode.next = startNode;
            }
        }

        return res.next;

    }

    public ListNode reverse(ListNode head){
        ListNode pre = null;
        ListNode cur = head;

        while (head != null){

            head = head.next;
            cur.next = pre;
            pre = cur;
            cur = head;
        }
        return pre;
    }
}
~~~
## 21.合并两个有序链表
[题目](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
递归
~~~java
public class Solution {
    public ListNode mergeTwoLists(ListNode l1,ListNode l2){
        if(l1 == null){
            return l2;
        }

        else if(l2 == null){
            return l1;
        }

        else if(l1.val < l2.val){
            l1.next = mergeTwoLists(l1.next,l2);
            return l1;
        }

        else{
            l2.next = mergeTwoLists(l1,l2.next);
            return l2;
        }

    }

    public static void main(String[] args) {
        ListNode l1 = new ListNode(1);
        l1.next = new ListNode(2);
        l1.next.next =  new ListNode(4);

        ListNode l2 = new ListNode(1);
        l2.next = new ListNode(3);
        l2.next.next =  new ListNode(4);

        Solution test = new Solution();
        ListNode res = test.mergeTwoLists(l1,l2);
        while(res != null){
            System.out.print(res.val + " ");
            res = res.next;
        }
    }
}
~~~
## 23.合并k个排序链表
[题目](https://leetcode-cn.com/problems/merge-k-sorted-lists/)
思路：
分治，两两合并
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704213652393.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L293ZW5meQ==,size_16,color_FFFFFF,t_70)
~~~java
public class Solution {
    public ListNode mergeKlists(ListNode[] lists){
        int interval = 1;
        int len = lists.length;
        while(len > interval){
            for(int i = 0;i < len - interval; i += interval * 2){
                lists[i] = merge2lists(lists[i], lists[i + interval]);
            }
            interval *= 2;
        }
        return len > 0 ? lists[0]:null;
    }

    public ListNode merge2lists(ListNode l1,ListNode l2){

        if(l1 == null){
            return l2;
        }

        else if(l2 == null){
            return l1;
        }

        else if(l1.val < l2.val){
            l1.next = merge2lists(l1.next,l2);
            return l1;
        }

        else{
            l2.next = merge2lists(l1,l2.next);
            return l2;
        }
    }

    public static void main(String[] args) {
//        ListNode l1 = new ListNode(1);
//        l1.next = new ListNode(4);
//        l1.next.next =  new ListNode(5);
//
//        ListNode l2 = new ListNode(1);
//        l2.next = new ListNode(3);
//        l2.next.next =  new ListNode(4);
//
//        ListNode l3 = new ListNode(2);
//        l3.next = new ListNode(6);

//        ListNode[] lists = {l1,l2,l3};
        ListNode[] lists = {};

        Solution test = new Solution();
        ListNode res = test.mergeKlists(lists);
        while(res != null){
            System.out.print(res.val + " ");
            res = res.next;
        }
    }
}
~~~