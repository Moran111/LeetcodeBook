# Question 1

```
Question:
Given an array of size n with unique positive integers and a positive integer K,
check if there exists a combination of elements in the array satisfying below constraints.
a. The sum of all such elements is K
b. None of those elements are adjacent in the original array

Ex:
Input:
arr = {1, 9, 8, 3, 6, 7, 5, 11, 12, 4}
K = 14

Output:
[3, 7, 4]
```

## Find all possible subsets with constraints&#x20;

Ask:&#x20;

1. what is no combination that can satisfy all conditions? what should we return?&#x20;
2. should we return exist one, are we going to return one result even though there are multiple possible answers?&#x20;

```
private static List<Integer> findSubset(List<Integer> arr, int k) {
        List<Integer> res = new ArrayList<>();
        dfs(arr, 0, k, res);
        return res;
    }

    // are I find a valid res that sum up to k and no adjacent elements
    private static boolean dfs(List<Integer> arr, int index, int target, List<Integer> res) {
        if (index == arr.size() || target < 0) {
            return false;
        }
        if (target == 0) {
            return true;
        }

        // choose one element from arr
        for (int i = index; i < arr.size(); i++) {
            int currNum = arr.get(i);
            // do not add adjacent element
            res.add(currNum);
            boolean isExist = dfs(arr, i+2, target - currNum, res);
            if (isExist) {
                return true;
            }
            res.remove(res.size()-1);
        }
        return false;
    }
```

