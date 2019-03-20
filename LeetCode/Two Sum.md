## 两数之和
给定一个整数数组 `nums` 和一个目标值 `target`, 请你在数组中找出和为目标值的那 **两个**整数, 并返回他们的数组下标.
你可以假设每种输入只会对应一个答案. 但是, 你不能重复利用这个数组中相同的元素.

**示例:**
```js
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

### javascript 解法

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
// 第一种
let sums = function (nums, target) {
    const mMap = {};
    for(let i = 0; i < nums.length; i++) {
	let t = target - nums[i];
	if(mMap[t] && mMap[t] != i) {
	    return res = [t, nums[i]];
	} else {
	    mMap[nums[i]] = i;
	}
    }
}

// 第二种
var twoSum = function(nums, target) {
    var map1 =new Map();
    for(let i=0;i<nums.length;i++){
        var complement = target - nums[i];
        if (map1.has(complement) && map1.get(complement)!=i){
            return [map1.get(complement),i]
        }
        map1.set(nums[i],i);
    } 
};
```


### 题解方案

方法一: 暴力法
暴力法很简单。遍历每个元素 xx，并查找是否存在一个值与 target - xtarget−x 相等的目标元素。

|序号|名称|描述
|:-:|:-:|:-:
|1|暴力法|暴力法很简单。遍历每个元素 xx，并查找是否存在一个值与 target - xtarget−x 相等的目标元素
|2|两遍哈希表|

```java
// java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[j] == target - nums[i]) {
                return new int[] { i, j };
            }
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
