# SearchAnswer

## The Judges Give Problem

Description

The difficulty of the questions is divided into three grades: "simple questions", "medium questions" and "difficult questions". The judges have come up with $$EE simple questions, MM medium questions and HH difficult questions. Then the judges give EMEM "Simple And Medium" questions and MHMH "Medium And Difficult" questions. The "Simple And Medium" is a type of problem that can be classified as eithor a "simple question or a "medium question". The "Medium And Difficult" is a type of problem that can be classified as either a "medium question" or a "difficult question". The judges decreed that a contest must consist of three questions: one for "easy", one for "medium" and one for "difficult". Each question can only appear in one match at most. Now we want to know how many contests can the judges organize?$$

$$0≤E,EM,M,MH,H≤10180≤E,EM,M,MH,H≤1018$$

Example

**Example 1:**

```
Input:
2
2
1
2
2
Output:
3

```

```
public class Solution {
    /**
     * @param E: the number of easy problems
     * @param EM: the number of "easy and medium" problems
     * @param M: the number of medium problems
     * @param MH: the number of "medium and hard" problems
     * @param H: the number of hard problems
     * @return: nothing
     */
    public long theNumberOfContests(long E, long EM, long M, long MH, long H) {
        // write your code here.
        long totalQ = E + EM + M + MH + H;
        long posibleContests = totalQ / 3;
        long start  = 0;
        while (start + 1 < posibleContests) {
            long mid = (start + posibleContests) / 2;
            if (helper(E, EM, M, MH, H, mid)) {
                start = mid;
            } else {
                posibleContests = mid;
            }
        }
        if (helper(E, EM, M, MH, H, posibleContests)) {
            return posibleContests;
        } else if (helper(E, EM, M, MH, H, start)) {
            return start;
        }
        return 0;
    }

    private boolean helper(long E, long EM, long M, long MH, long H, long contests) {
        if (E < contests) {
            EM -= contests - E;
            if (EM < 0) return false;
        }
        if (H < contests) {
            MH -= contests - H;
            if (MH < 0) return false;
        }
        return (EM + M + MH) >= contests;
    }
}
```

