# Daily Design Journal - Day 1

---

## Section 1 — Specific Bug

No compiler or runtime errors occurred today because no implementation has done today. Today I focused on designing the DynamicArray and preparing the design proposal.

---

## Section 2 — Failed Attempt

Initially, I considered increasing the array capacity by one element whenever it became full. After analyzing the time complexity, I realized this would require frequent memory reallocations and copying of elements, making append operations inefficient (O(n) for many insertions). Therefore, I rejected this approach and tried 1.5x and 1.8x as they were giving less unused space but in this the no. of resizes is very high, so i tried 2x resize factor and in this the no of resize is very low but unused space is more. 

---

## Section 3 — Memory Diagram

**Submitted separately as a hand-drawn diagram.**

The diagram includes:
- DynamicArray object
- `data` pointer
- `size`
- `capacity`
- Heap memory showing contiguous elements
- Resizing process (old block → new larger block)

---

## Section 4 — Code Reference

Commit Hash:
**N/A**

Filename:
**N/A**

Relevant Line Numbers:
**N/A**

Reason:
Implementation has not started. No source code has been written yet.

---

## Section 5 — Learning Reflection

Today I understood why DynamicArray stores elements in contiguous memory instead of using a linked list. Although resizing requires copying elements, contiguous storage provides constant-time indexing and much better cache performance.

I also learned why capacity is usually doubled during resizing. Earlier I thought increasing capacity by one would save memory, but after analyzing the number of reallocations, I realized that doubling greatly reduces resizing frequency and gives amortized O(1) insertion performance. While 1.5x and 1.8x can also be used as less memory space is ideal but here the no of resize will increase.

Another important concept I learned is the Rule of Three. Since the class manages dynamic memory, using the default copy behavior would create shallow copies, leading to dangling pointers and double deletion. Implementing a destructor, copy constructor, and copy assignment operator ensures that every object manages its own memory safely.
