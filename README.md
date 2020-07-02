[toc]
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
