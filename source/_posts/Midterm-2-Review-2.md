---
title: Midterm 2 Review(2)
date: 2019-07-24 18:57:45
tags: CS61C
---

# Factors that Decide the Cache Performance

## HIT Time

### Contributing Factors

- Cache Size
- Associativity



## Miss Rate

### Contributing Factors

- Memory access behaviour of the program

- Block size / Cache size / Cache Type ( associativity )

  ![image-20190724192712638](/Users/hubohan/Library/Application Support/typora-user-images/image-20190724192712638.png)

## Miss Penalty

Contributing Factors:

- How big is your memory hierarchy
- Small block size — Lower MP



# MultiLevel Cache

L1 Cache has the lowest hit time, and L2 / L3 focus on low miss rate 

L1 does not care about high hit rate

`AMAT = L1 HR + L1 MR * L1 MP`

`L1 MP = L2 HR + L2 MR * L2 MP`

` L2 Local Miss Rate = L2 Miss / L1 Miss`

` Global Miss Rate = Products of all (higher) Local Miss Rate `

For example, global L2 miss rate = MRLocal L1 * MRLocalL2

![image-20190724200304673](/Users/hubohan/Library/Application Support/typora-user-images/image-20190724200304673.png)

![image-20190724200322133](/Users/hubohan/Library/Application Support/typora-user-images/image-20190724200322133.png)



- TO BE Done: Example 1 and 2

  