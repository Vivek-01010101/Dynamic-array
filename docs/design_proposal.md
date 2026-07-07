# Design Proposal for Dynamic Array

## Dynamic Array -
The DynamicArray stores its elements in a contiguous block of heap memory that is managed manually. Memory is allocated whenever the capacity needs to increase and is released when the DynamicArray object is destroyed. During resizing, a new memory block is created and all existing elements are copied into it before the old memory is deallocated.

Appending elements provides amortized O(1) time complexity because the array grows by doubling its capacity. Since resizing occurs only when the capacity reaches powers of two, inserting n elements requires n insertion operations and (n − 1) copy operations during all resizes, resulting in a total of 2n − 1 operations. Dividing this by n gives an average of about 2 operations per insertion, which is considered O(1) amortized time complexity.

## Section 1 – Public API
### Template

The class uses templates so it can store any data type (`int`, `char`, `float`, `double`, etc.). Without templates, separate classes would be needed for each data type.

 **T** represents the data type of the element stored in the dynamic array. It is decided when the object is created. For example, if we create DynamicArray\<int>, then T becomes int; if we create DynamicArray\<char>, then T becomes char. This allows the same class to store different types of data.
The following functions can be used by the user to interact with the `DynamicArray`.

### i) Constructor
**Purpose:** Initializes the dynamic array by setting the initial values of `data`, `size`, and `capacity`.

- **Parameters:** None
- **Return Type:** None

---

### ii) `append(T value)`
**Purpose:** Adds a new element at the end of the dynamic array.

- **Input Parameter:** Value to be inserted
- **Return Type:** `void`

---

### iii) `pop()`
**Purpose:** Removes and returns the last element of the array.

- **Input Parameter:** None
- **Return Type:** `T`

**Possible Error:** If the array is empty, no operation is performed.

---

### iv) `pop(size_t index)`
**Purpose:** Removes and returns the element at the given index.

- **Input Parameter:** Index
- **Return Type:** `T`

**Possible Error:** If the index is invalid, an out-of-range error is generated.

---

### v) `remove(T value)`
**Purpose:** Removes the first occurrence of the given value.

- **Input Parameter:** Value
- **Return Type:** `void`

**Possible Error:** If the value does not exist, a `value_not_exist` error is generated.

---

### vi) `insert(size_t index, T value)`
**Purpose:** Inserts a new element at the specified index.

- **Input Parameters:** Index, Value
- **Return Type:** `void`

---

### vii) `size()`
**Purpose:** Returns the current number of elements stored in the array.

- **Return Type:** `size_t`

---

### viii) `capacity()`
**Purpose:** Returns the total allocated capacity of the array.

- **Return Type:** `size_t`

---

### ix) `resize()`
**Purpose:** Increases the capacity when `size == capacity`.

- **Return Type:** `void`

**Note:** Internal helper function.

---

### x) `clear()` 
**Purpose:** Removes all elements from the array.

---


### xi) Destructor
**Purpose:** Releases dynamically allocated memory to prevent memory leaks.

---

# Section 2 – Internal Representation

## Memory Diagram of Dyanmic Array

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/affb7ba2-4204-4733-9769-3856c45139ba" />


## Template

The class uses templates so it can store any data type (`int`, `char`, `float`, `double`, etc.). Without templates, separate classes would be needed for each data type.

## Internal Structure

```cpp
template<typename T>
class DynamicArray
{
private:
    T* data;
    size_t size;
    size_t capacity;
};
```

The data members are private to provide encapsulation. Users interact with the array only through public member functions.

## Data Members

- **data** – Pointer to the first element of the dynamically allocated contiguous array.
- **size** – Current number of elements stored.
- **capacity** – Total allocated storage.

## Rule of Three

The class follows the Rule of Three:

1. Destructor
2. Copy Constructor
3. Copy Assignment Operator

A shallow copy is not used because it can cause dangling pointers and double deletion. Therefore, a deep copy is used so that each object owns its own memory.

---

<!-- # Section 3 – Complexity Estimates

| Operation | Time Complexity | Reason |
|-----------|-----------------|--------|
| `append()` | Best: **O(1)**, Amortized: **O(1)**, Worst: **O(n)** | If capacity>size then O(1), otherwise resizing is done by calling resize() . |
| `pop()` | **O(1)** | Removes the last element directly. |
| `pop(index)` | **O(n)** | Removes element by index and Remaining elements are shifted. |
| `remove(value)` | **O(n)** | Array is traversed to find the element and shifting are required. |
| `insert(index, value)` | **O(n)** | Elements are shifted to make space for the element to insert. |
| `size()` | **O(1)** | Increment size by one as element is inserted. |
| `capacity()` | **O(1)** | Returns stored variable. |
| `resize()` | **O(n)** | Allocates new memory and copies all elements. | -->
# Section 3 – Complexity Estimates

| Operation | Best Case | Average Case | Worst Case | Reason |
|-----------|:---------:|:------------:|:----------:|--------|
| `append(T value)` | **O(1)** | **O(1)** Amortized | **O(n)** | If there is free capacity, the element is inserted directly. When the array is full, `resize()` is called and all existing elements are copied to a new memory block, giving a worst-case time of **O(n)**. Since resizing happens only occasionally, the amortized complexity is **O(1)**. |
| `pop()` | **O(1)** | **O(1)** | **O(1)** | Removes the last element directly without shifting any elements. |
| `pop(size_t index)` | **O(1)** | **O(n)** | **O(n)** | If the last element is removed, no shifting is required (**O(1)**). Otherwise, all elements after the index are shifted one position to the left. |
| `remove(T value)` | **O(1)** | **O(n)** | **O(n)** | Best case occurs when the value is found at the last position or the array is empty. On average and in the worst case, the array must be searched and elements may need to be shifted. |
| `insert(size_t index, T value)` | **O(1)** Amortized | **O(n)** | **O(n)** | Inserting at the end without resizing takes **O(1)**. Inserting at other positions requires shifting elements, and resizing (if needed) increases the cost to **O(n)**. |
| `size()` | **O(1)** | **O(1)** | **O(1)** | Returns the stored `size` variable directly. |
| `capacity()` | **O(1)** | **O(1)** | **O(1)** | Returns the stored `capacity` variable directly. |
| `resize()` | **O(n)** | **O(n)** | **O(n)** | Allocates a new memory block and copies all existing elements to the new array before deleting the old memory. |


**Capacity Growth**

The capacity is doubled (`capacity *= 2`) whenever the array becomes full. This reduces the number of resizing operations and provides amortized **O(1)** append performance.

here we are doubling array bcoz if we double the size of array then less resizing need to perform but at large array size, memory block remain unused, we can also use **(1.5x)** or **(1.8x)** but here the space will less unused but reisizing will increase. In case of **(3x or 4x)** resizing will decrease but the unused space will increase more.

---

# Section 4 – Design Decisions

### Decision 1 – Contiguous Memory
**Chosen because**
- Here the continguous memory will be used because it provide random access and updation of element in O(1) Time complexity using simpler pointer arithmetic.
- Consecutive elements are stored next to each other in memory. When the CPU loads one element into the cache, nearby elements are also loaded automatically. This reduces cache misses and improves the overall performance of operations such as traversal and iteration.


**Rejected Alternative:** Linked List

**Reason**
- In a linked list, elements are not stored in contiguous memory. To access an element at a particular index, the list must be traversed from the beginning, resulting in O(n) time complexity.
- Since nodes are scattered across different memory locations, the CPU cache cannot efficiently preload nearby elements. This leads to more cache misses and slower execution compared to contiguous memory.
- Every node in a linked list stores one or more pointers in addition to the actual data. These extra pointers increase the memory usage, making linked lists less memory-efficient than dynamic arrays.

---

### Decision 2 – Capacity Growth
**Chosen:** `capacity *= 2`

**Reason**
- `capacity *= 2` is used here as less number of resizing is recquired but there is more vacant memory. As it provide Amortized O(1) append TC.
- Amortized O(1) append.

**Rejected:** `capacity += 1`, `capacity *= 1.5`,`capacity *= 4`

**Reason**
- Here `capacity += 1`,`capacity *= 1.5`,`capacity *= 4` and more greater other is rejected because in less than 2, the no of resize increases and unused space decrease but in greater than 2 no of resizes decrease but unused space become very large
- Poor overall performance.

---

### Decision 3 – Template-Based Design

Templates allow one implementation to work with every data type without code duplication. The class uses templates so it can store any data type (int, char, float, double, etc.). Without templates, separate classes would be needed for each data type.

It is decided when the object is created. For example, if we create DynamicArray\<int>, then T becomes int; if we create DynamicArray\<char>, then T becomes char. This allows the same class to store different types of data.

---

### Decision 4 – Deep Copy


Deep copy is used so that each DynamicArray object has its own separate memory. During copying, a new memory block is allocated and all elements are copied into it, making both objects independent.

A shallow copy was rejected because it copies only the pointer, causing both objects to share the same memory and when the destructor of previous object is called, it free the memory of the prev object but as the new object also stores the same address, it now become a dangling pointer and when destrcutor of new object try to free memory at the already free memory location, it will do double deletion. Therefore, deep copy ensures safe memory management and prevents these problems.

---
