---
title: MidTerm 2 Review (1)
date: 2019-07-23 22:57:13
tags: CS61C Exams
---



## Cache Introduction

### Principle of Locality

- Example: Library
- Code, Stack, Array: Data are organized tightly together

#### Temporal Locality (locality in time)

If a memory location is referenced then it will tend to be referenced again soon

#### Spatial Locality (locality in space)

If a memory location is referenced, the locations with nearby addresses will tend to be referenced soon

## Cache Management

- TIO => TAG / INDEX / OFFSET
- Valid Bit
- Cache Params: **Block Size(K)** and **Cache Size(C)**
- Index may be block number, or group number(in associative ways)

### Block Replacement Policy

#### Random Replacement

#### LRU

Least Recently Used

### Cache Implementation

- Actual Data Block
- Tag Bits
- Valid Bit
- LRU Bits: 00 - Used Last 11 - Ready to remove

### Consistency Between Cache and Mem

#### Write Hit

- Write Through:
  - Write data to both mem and cache ( slower ) 
  - Include a write buffer that will update the mem parallel

- Write Back:
  - if the block is in cache, update it only in cache, and mark it 
  - use a dirty bit to record whether it is modified

#### Read Miss

- fectch data and stall the pipeline, then recover 

#### Write Miss

- Always have to update the block in the memory
- _Update the cache or not?_

- Write Allocate
  - Bring a block into cache after a read miss
  - Always pair with **Write Back**
  - E.g. Accessing the same memory address many times -> cache it
- No Write Allocate 
  - Always pair with **Write Through**
  - E.g. Random writes ( don't care about the caching )



## Cache Mapping

### Direct-Mapped 

- Each memory block is mapped excatly to one slot in the cache 
- the Hash function is **deterministic**
- Just need to check `one slot`
- Faster!
- **But access bahaviour might leave some slot blank**
  - Byte X, Byte X + Cache Size, Byte X + n * Cache Size will be mapped into the same block
  - Will cause conflicts

- `TIO` Address format
  - `T` - Tag 
  -  `I`: Index - determined by `number of blocks`
  -  `O`: offset - determined by `block size`

- **the WORST CASE of DM**
  - consider that the cache size(C) = 16
  - access 0 -> 16 -> 0 -> 32 -> n*16
  - Only use one slot and cause every access MISS

### Set Associative

- Divide cache into sets, each of which consists of N blocks 

- N is the **associativity**

- the Index refers to the group num

- $$ I = log_2 (C/KN)$$

  ![image-20190723221429306](/Users/hubohan/Library/Application%20Support/typora-user-images/image-20190723221429306.png)

- **the WORST CASE of SA**

  - with LRU: repeat pattern that at least N+1 map into the same set
  - E.g N = 2, use the pattern 0, 8, 16
  - **Question: which block will be mapped into the same group? **

## Cache Performance

- Miss Rate and Miss Penalty
- Average Memeory Access Time: AMAT 
- **AMAT  = HIT Time + Miss Rate * Miss Penalty**
- Increase in hit rate ( cache size ) will increase the hit time, which will overcome the improvement for hit rate, decrease the performance 



### Source of Cache Misses: 3Cs

| Item       | Explain                                                      | Solution                | Effect                                       |
| ---------- | ------------------------------------------------------------ | ----------------------- | -------------------------------------------- |
| Compulsory | the first time to reference must cause miss                  | Increase the block size | Increase MP; too large block **increase MR** |
| Capacity   | can't hold all blocks in a set                               | Increase the cache size | Increase the HT                              |
| Conflict   | different address mapped into the same location ( Lack of associativity ) | Increase associativity  | Increase the HT                              |



