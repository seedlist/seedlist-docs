English | [中文](incentive_cn.md)
## Summary
> 1. The seed token follows the cycle halving deflation model;

> 2. The token has no practical use, and it will not be given any practical way to use it now and in the future;

> 3. The user's willingness to use the seedlist is the user's trust in the seedlist. We need a way to pay tribute to the user. This is one of the reasons for the existence of the seed token: as a commemorative feedback to the user's trust;

> 4. In addition, based on the seedlist architecture model, any encryption developer can develop and register his own encryption process to the seedlist contract for every user in the community to use. The contributions of these crypto developers need to be respected,
     > Even if it is courtesy, this is another reason for the existence of seed tokens;

## Rules
#### A. About users
> 1. 21 million tokens (with a precision of 18 digits) are given to community users as a cycle; (why 21 million is used as the base number, maybe because I am too poisoned by Bitcoin)

> 2. Based on step (1), a total of 71 cycles can be calculated;

> 3. In the first cycle, each time the user saves, (2100/2\*\*0) seed tokens will be issued to the user’s current wallet address; when the second cycle is entered, ( 2100/2\*\*1) seed tokens to the user’s current wallet address; when entering the third cycle, (2100/2\*\*2) seed tokens will be issued to the user’s current wallet address;
     Decrease in subsequent cycles...

#### B. About the developer
> 1. Select 6% as the basic share base;

> 2. Every time a user uses an encrypted contract (denoted as E.C), the system will calculate the amount of incentive tokens given to the encrypted contract according to the following algorithm:
     step one: Count the current percentage of E.C usage in the past 100 block heights and record it as N%; 
     step two: Incentive token amount = stored user incentive amount \* 6% \* N%;

Therefore, the total number of tokens issued in a complete cycle is at most 22.26 million; the total number of tokens is at most 22.26 million x 71 cycles (total 15.8046); the total number of tokens issued to community users is fixed at: 1.491 billion, at least accounting for 94.34% of the total; up to 5.66% issued to developers;    