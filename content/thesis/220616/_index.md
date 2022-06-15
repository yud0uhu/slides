+++
title = "輪講"
outputs = ["Reveal"]
[reveal_hugo]
theme = "sky"
slide_number = true
[markup.highlight]
codeFences = false
+++

## 輪講

#### 2022-06-16

---

## Key bindings

- RTOS（Real Time Operating System）
  - An OS for building a system that can execute processes within the required time (= real-time system).
  - The key point is "satisfying time constraint conditions.

---

## BackGround

<!-- 大型OSは1950年代に登場しました．
しかし，1980年代の半ばまで，マイコンOSは以下の理由から定着しませんでした．
概念，構造，機能，インターフェース
そして，組み込みプログラミングをする人たちの中に，OSを理解している人が少なかったためです． -->

Large OSs appeared in the 1950s.  
However, until the mid-1980s, microcomputer OSs did not take root for the following reasons

- Concepts, structures, functions, and interfaces.
- The concept, structure, functionality, and interface of the OS, and because few people doing embedded programming understood the OS.

---

<!-- しかし現在では，事情が異なります
商用の32bitの複合マイコンが普及していて，誰もが手軽に入手できます．
そしてこれらは(And these are)，これらは低コストで高性能なデバイスであり、豊富なオンチップメモリと周辺機能を内蔵しています． -->

Today, however, the situation is different.

- Commercial 32-bit composite microcontrollers are widely available and easily accessible to everyone.
  - low-cost
  - high-performance devices
  - rich on-chip memory and built-in peripheral functions.

---

<!-- しかし，そもそも，「できる」こと=「やるべき」ことなのか？
・なぜRTOSを採用するのか？
・RTOSが適合する設計とはどのようなものなのか？
このような判断をするために，私たちは基本的な組み込みシステムのソフトウェアにおける実用的な設計手法の基礎を学ぶ必要があります．
 -->

But, in the first place, **does "can" = "should"?**  
**Why should RTOS be adopted?**  
**What kind of design is RTOS compatible with?**  
To make such decisions, it is necessary to learn the basics of practical design techniques in basic embedded system software.

---

## To produce high quality software

<!-- 高品質なソフトウェアの条件とは何でしょう？
本書では，以下のように定義されています． -->

From the start, what are the requirements for high-quality software?
In this document, it is defined as follows.

```md
1.'functional' correctness.
2.'temporal' correctness.
3.Its behaviour should be predictable.
4.Its behaviour should be consistent.
5.low complexity.
6.static analysis.
7.dynamic analysis.
8.Run-time performance should be predictable.
9.Memory requirements should be predictable.
10.The code can, if needed, be shown to conform to relevant standards.
```

---

What should software developers consider when applying these principles to real-time systems such as the following?

---

For example, consider a system that controls temperature by varying the flow rate of a liquid.

---

In such a system, the software should perform data acquisition, signal linearization, scaling, control calculations, and actuator driving.  
**However, the SIL standard prohibits interrupt processing.**

---

Therefore, to handle time in this system, the following description is used.

```c
DelayUntilTime = xx millseconds;
```

---

## Basic features of real-time operating systems

What are the basic functions of a real-time OS?
A timer times out. How can software know what happens in real time in the computer world, such as when a power switch is pressed?
There are two ways. `Polling` or `hardware interrupt`.

---

Interrupts can be thought of as engines that execute specific software code.
Interrupts can be used to execute multiple tasks "simultaneously.
However, "interrupts" are a very complex concept for software developers to deal with.

<!-- The central function of an operating system is to remove this burden from the code author; the OS removes the complexity of the computer from the programmer, allowing the programmer to focus on his or her primary task. -->

---

Hardware and OS software considerations include.

- Task structuring of programs.
- Task implementations as logically separate units (task abstraction).
- Parallelism (concurrency) of operations.
- Use of system resources at predetermined times.
- Use of system resources at random times.
- Task implementation with minimal hardware knowledge.

---

Commercial operating systems are extremely unstable in mainframe environments. The number, complexity, and scale of tasks to be processed at any one time is unknown.

---

In embedded applications, tasks are well defined. Therefore, the processor must be able to handle the total computer load on a fairly specific timescale.
Therefore, the operating system must be efficient, and reliability of operation is of paramount importance.

---

In this example, there are three main tasks: the JPT control, the flight recorder interface, and the pilot interface.
Executing each on a separate processor is called multiprocessor.
However, this is expensive, complex, and technically overkill.

<img src="images/figure_1.png">

---

Each can be run on a separate processor, i.e., multiprocessing. In such a small system, this would be expensive, complex, and technologically excessive. Therefore, only one processor is used.

<!-- Note A single-processor multitasking design is called a "multitasking" system and is distinguished from a multiprocessor. -->

---

Now there are three independent but interdependent tasks.
The solution to this problem is a "multitasking system" for the OS.
The task scheduling is to decide "when" and "for what" the task is to be executed. This is "task scheduling.

---

Tasks must monitor the use of resources shared between them and perform "mutual exclusion" to prevent damage or corruption of those resources.
Tasks must be able to "talk" to each other, thus requiring communication facilities "synchronization and data transfer."

<!-- For example, implement several independent control channels on a single-board digital controller. These tasks can proceed without the need to communicate with each other. -->

---

## Finally

An OS is defined as "a set of software products that jointly control system resources on a computer and the processes that use those resources.
Embedded OSs are smaller and simpler than mainframe OSs.  
Finally, see the figure below.

<img src="images/figure_2.png">
