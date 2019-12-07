---
title: Crosscutting Issues
date: 2019-08-24 22:57:13
tags: Architecture
---



## RISC Instruction Sets and Efficiency of Pipelining

- Suppose we need to add two values in memory and store them back.

  In some sophisticated ISA this will only take 1 instruction.

  In RISC, it will take 2 loads, an add, and a store.

  **These instructions cannot be scheduled sequentially without intervening stalls**

- Since in RISC instructions, the individual operations are seperate and can be individually scheduled by compiler or hardwares.



## Dynamically Scheduled Pipelines

- Simple pipelines fetch an instruction and issue it when there's no data dependency between an instruction already in the pipeline and the instruction to be issued that cannot be hidden by bypassing or forwarding.
- Forwarding increase the latency of the circuit when there's not a hazard
- Compiler can schedule the instructions to avoid the hazard ( called compiler or static scheduling )
- All the techniques discussed so far use in-order issue. 
  - Check the dependency in ID
  - If there's a hazard, the pipeline will stall **even if there are later instructions that are independent**.
- out-of-order execution: seperate ID into 2 stages
	- Issue
	- Read Operands

#### Dynamic Scheduling with a Scoreboard

- All instruction pass through the issue stage in order

- The can be **stalled** or **bypass each other** in the **read operands** stage

- Scoreboarding: allowing instructiosn to execute out of order when there are sufficient resources and no data dependences.

- The possible WAR hazards

```
  DIV.D F0, F2, F4
  ADD.D F10, F0, F8
  SUB.D F8, F8, F14
```

  - Antidependence between **ADD.D and SUB.D** 
  - SUB.D execute before ADD.D, then the source of ADD.D is destroyed.
  - Likewise, WAW hazards will occur if the dest of SUB.D is F10

- The goal of scoreboard is to maintain an execution rate of 1 IPC by execute an instruction as early as possible
- **If one instruction is stalled, the other can be issued when there's no dependence**

------

The detection and resolution of hazard are centralized in the scoreboard

- Every instruction goes into scoreboard, where the data dependence is recorded. 
- The scoreboard will decide when the instruction can read operand and issue. 
- The scoreboard will monitor the exection of every instructions.
- The scoreboard can decide whether an instruction can write to destination.

------

##### 4 Steps in Execution

These 4 following steps will replace ID, EX and WB

1. Issue

   **a functional unit for the instruction is free and no other active instruction has the same destination**

   When it is stalled, the instruction buffer will be filled.

2. Read Operands

   a source operand is available if no earlier issued active instruction is going to write it

   This step, combined with issue step, completes the function of **ID step** in simple MIPS pipeline.

3. Execution

4. Write Back

   check the WAR hazard 	

   - There is an instruction that has not read its operands that precedes the completing instruction( the instruction to be commit ) **AND**

   - one of the operands is the same register as the result of the completing instruction

   - **It might be hard to distinguish the WAR and RAW**

------

- Scoreboard does not take advantage of forwarding because the operands are read only when operands are available
- The penalty is not as large as we initially think
- There's a limited number of source operand bus and result bus of register file so the scoreboard must guarantee the instruction in step 2 and 4 does not exceed the number of buses available.



#### Scoreboard Data Structure

- Code Snippet

  ```asm
  L.D		F6, 34(R2)
  L.D		F2, 45(R3)
  MUL.D	F0, F2, F4
  SUB.D	F8, F6, F2
  DIV.D F10, F0, F6
  ADD.D F6, F8, F2
  ```

  

#### 3 Parts of the Scoreboard

1. _Instruction Status_ : which step the **instruction is in**
2. _Functional unit status_ : the state of **functional unit**
   - Busy
   - Op
   - Fi - Destination
   - Fj, Fk - Source
   - Qj, Qk - Functional Units producing source register Fj, Fk
   - Rj, Rk - Flags indicating when Fj, Fk are ready and not yet read
3. _Register result status_ : which functional unit will write each **register**.

