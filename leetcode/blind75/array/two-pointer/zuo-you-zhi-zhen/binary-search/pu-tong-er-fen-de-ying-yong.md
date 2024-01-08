# 普通二分的应用

### 检测解的应用

[875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)\


Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.

&#x20;

**Example 1:**

<pre><code><strong>Input: piles = [3,6,7,11], h = 8
</strong><strong>Output: 4
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: piles = [30,11,23,4,20], h = 5
</strong><strong>Output: 30
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: piles = [30,11,23,4,20], h = 6
</strong><strong>Output: 23
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= piles.length <= 104`
* `piles.length <= h <= 109`
* `1 <= piles[i] <= 109`

search speed, need to know how many hours it should eat in given speed and then compare hour and hour.

```
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        // time t needed to eat all bananas in k speed, comparing to the real time h
        // if t < h, can try a slower speed
        // if t > h, can try a faster speed
    
        // speed
        int left = 1; int right = 0;
        for (int pile : piles) {
            right = Math.max(right, pile);
        }

        while (left + 1 < right) {
            int mid = left + (right - left)/2;
            //System.out.println(left + " " + right + " " + mid);
            int time = canEatAll(piles, h, mid);
            if (time > h) {
                left = mid;
            } else {
                // if using speed mid and eat all in h hours, can try eat slower (pick a smaller speed)
                right = mid;
            }
        }
        
        if (canEatAll(piles, h, left) <= h) {
            return left;
        }
        return right;
    }

    // time needed to eat all by given speed, if speed < h, can eat all
    private int canEatAll(int[] arr, int h, int speed) {
        int time = 0;
        for (int i = 0; i < arr.length; i++) {
            // can only eat one piles in 1 hour, 1和小时内这一堆没吃完也不能去吃下一堆
            time += Math.ceil((double)arr[i] / speed);
        }
        return time;
    }
}
```



### 计数二分
