# b.2 Social Media Followers: Username Existence Verification

## 1. Question
In a social media platform scenario, checking whether a particular username exists among $n$ usernames requires selecting an efficient search strategy. 
**Problem**: Compare **Linear Search** and **Hash-based Searching** for this scenario in terms of expected complexity.

---

## 2. Algorithm Identification

This problem falls under the **Searching & Hashing** paradigm. Two approaches are considered: **Linear Search** and **Hash-based Search**.

| Attribute | Linear Search | Hash-based Search |
|---|---|---|
| **Definition** | Checks each username sequentially until the target is found or the list ends. | Converts a username into a bucket index using a hash function for direct lookup. |
| **Expected Complexity** | **O(n)** | **O(1)** |
| **Useful for** | Small datasets, unsorted lists, infrequent searches, and memory-constrained systems. | Large datasets and high-frequency real-time lookups. |
| **Chosen** | Standard Sequential Search | Separate Chaining |

### Types / Variants

| Linear Search Variants | Hashing Variants |
|---|---|
| **Standard Sequential Search** [Chosen] | **Separate Chaining** [Chosen] |
| Checks elements one by one without modifying the input. | Stores collided keys in a linked-list chain at the same bucket. |
| **Sentinel Linear Search** [Not Chosen] | **Open Addressing** [Not Chosen] |
| Requires adding a temporary sentinel to the array. | Probes neighbouring slots after a collision; can cause clustering. |
| **Self-Organizing Search** [Not Chosen] | **Perfect Hashing** [Not Chosen] |
| Reorders elements after searches, adding write overhead. | Requires a static, completely known set of keys. |

---

## 3. Core Concepts

### Searching Methods

| Attribute | Linear Search | Hash-based Search |
|---|---|---|
| **Mechanism** | Compares the target with each element sequentially. | Computes a hash $\rightarrow$ maps it to a bucket $\rightarrow$ checks that bucket. |
| **Average Case Work** | Average successful search: approximately **n/2 comparisons**. | Expected work: approximately **1 + $\alpha$/2 probes** ($\alpha$ = load factor). |
| **Expected Time** | **O(n)** expected time. | **O(1)** expected time when the load factor is reasonable. |

### Hashing Components

| Component | Meaning | Example |
|---|---|---|
| **Search Key (k or T)** | Username being stored or searched. | `david_pro` |
| **Hash Function h(k)** | Converts the key into a numerical hash value. | Sum of ASCII values |
| **Hash Value** | Integer produced by the hash function. | `david_pro` $\rightarrow$ `952` |
| **Table Capacity (m)** | Number of buckets in the table. | `m = 5` |
| **Bucket Index (i)** | Position where the key is stored. | `i = h(k) mod m` |
| **Collision** | Two different keys map to the same bucket. | Multiple keys $\rightarrow$ Bucket 0 |
| **Separate Chaining** | Stores collided keys in a linked-list chain. | `Bucket 0 -> key -> key -> key` |

### Collision Example

If `"Aravind007"` and `"Beatrice99"` both map to Bucket 3:

$$\text{Bucket 3} \longrightarrow [\text{"Aravind007"}] \longrightarrow [\text{"Beatrice99"}] \longrightarrow \text{NULL}$$

To search for `"Beatrice99"`:

$$\text{Hash key} \longrightarrow \text{Bucket 3} \longrightarrow \text{Compare Node 1 (Mismatch)} \longrightarrow \text{Compare Node 2 (Match)}$$

### Case-Insensitive Usernames

If usernames are treated as case-insensitive:

* `"Aravind007"` $\rightarrow$ `"aravind007"`
* `"ARAVIND007"` $\rightarrow$ `"aravind007"`

Both inputs therefore represent the same normalized username and can be detected as duplicates.

---

## 4. Design Choices

| Parameter | Options | Chosen | Reason |
|---|---|---|---|
| **Table Capacity (m)** | `m=n`, `m>n`, `m=2^k` | **m = 5** | Easy hand calculations and intentionally demonstrates collisions. |
| **Hash Function** | ASCII Sum, Polynomial Rolling Hash, Cryptographic/Python Hash | **ASCII Sum** | Simple, deterministic, and easy to calculate by hand. |
| **Collision Resolution** | Separate Chaining, Open Addressing | **Separate Chaining** | Simple insertion, independent chains, and clear collision handling. |

### Effect of Alternatives

| Alternative | Effect |
|---|---|
| **m = 11** | Lower load factor (`5/11 ≈ 0.45`) and fewer collisions. |
| **m = 2** | High load factor (`2.5`), producing long chains and potentially approaching **O(n)** search. |
| **Polynomial Hash** | Better distribution; anagrams such as `"cat"` and `"act"` receive different hash values. |
| **Cryptographic Hash** | Good distribution but unnecessarily complex for hand calculations. |
| **Open Addressing** | Can cause clustering, needs deletion handling such as tombstones, and becomes inefficient at high load factors. |

---

## 5. Step-by-Step Example

### Given Data

| Item | Value |
|---|---|
| Number of usernames | **n = 5** |
| Usernames | `["alex_99", "beatrice_dev", "charlie_k", "david_pro", "elena_m"]` |
| Target | **`david_pro`** |
| Hash table capacity | **m = 5** |
| Collision method | **Separate Chaining** |

### Hash Function

**Hash Code Formula:**
$$\text{hash-code}(k) = \sum_{c \in k} \text{ASCII}(c)$$


**Bucket Index Formula:**
$$i = \text{hash-code}(k) \bmod m$$

### Hash Table Construction

| Username | ASCII Sum | Index Formula | Bucket |
|---|---:|---:|---|
| `alex_99` | 635 | `635 mod 5 = 0` | **Bucket 0** |
| `beatrice_dev` | 1245 | `1245 mod 5 = 0` | **Bucket 0** |
| `charlie_k` | 930 | `930 mod 5 = 0` | **Bucket 0** |
| `david_pro` | 952 | `952 mod 5 = 2` | **Bucket 2** |
| `elena_m` | 721 | `721 mod 5 = 1` | **Bucket 1** |

### Final Hash Table Structure

| Bucket Index | Chain Contents |
|---|---|
| **Bucket 0** | `alex_99 -> beatrice_dev -> charlie_k -> NULL` |
| **Bucket 1** | `elena_m -> NULL` |
| **Bucket 2** | `david_pro -> NULL` |
| **Bucket 3** | Empty |
| **Bucket 4** | Empty |

### Trace Execution (Side-by-Side Comparison)

| Step | Linear Search | Hash-based Search (Separate Chaining) |
|---|---|---|
| **Target Query** | `david_pro` | `david_pro` |
| **Step 1** | `alex_99 != david_pro` (Mismatch) | Calculate ASCII hash $\rightarrow$ **952** |
| **Step 2** | `beatrice_dev != david_pro` (Mismatch) | Compute bucket index: `952 mod 5 = 2` |
| **Step 3** | `charlie_k != david_pro` (Mismatch) | Jump directly to **Bucket 2** |
| **Step 4** | `david_pro == david_pro` (Match Found) | `david_pro == david_pro` (Match Found) |
| **Result** | **Match found at 4th comparison** | **Match found at 1st bucket probe** |
| **Complexity** | **O(n)** | **O(1) expected** |

---

## 6. Algorithm Procedures

### Linear Search Algorithm
1. **Input**: List of usernames $L$ of size $n$, target query string $T$.
2. **Procedure**:
   - Loop index $i$ from $0$ to $n - 1$:
     - If $L[i] == T$, return `(True, i + 1)`.
3. **Output**: `(False, n)` if target is not present.

### Hash-based Search Algorithm (Separate Chaining)
1. **Input**: Separate Chaining Hash Table $H$ of capacity $m$, target query string $T$.
2. **Procedure**:
   - Compute hash bucket index $idx = h(T) \bmod m$.
   - Retrieve bucket list $chain = H[idx]$.
   - Traverse $chain$: if node value equals $T$, return `(True, probes)`.
3. **Output**: Return `(False, probes)` if target key is not present in bucket chain.

---

## 7. Python Code Implementation

```python
import time

class HashTableChaining:
    def __init__(self, capacity=10):
        self.capacity = capacity
        self.buckets = [[] for _ in range(capacity)]

    def _hash(self, key):
        return hash(key) % self.capacity

    def insert(self, key):
        idx = self._hash(key)
        self.buckets[idx].append(key)

    def search(self, key):
        idx = self._hash(key)
        bucket = self.buckets[idx]
        probes = 0
        for item in bucket:
            probes += 1
            if item == key:
                return True, probes
        return False, max(1, probes)


def linear_search(usernames_list, target):
    comparisons = 0
    for username in usernames_list:
        comparisons += 1
        if username == target:
            return True, comparisons
    return False, comparisons


def hash_search_chaining(hash_table, target):
    return hash_table.search(target)


if __name__ == "__main__":
    followers_list = [
        "alex_99", "beatrice_dev", "charlie_k", "david_pro", "elena_m",
        "frank_z", "grace_hopper", "hannah_b", "ian_ml", "julia_code"
    ]
    
    chaining_table = HashTableChaining(capacity=len(followers_list))
    for username in followers_list:
        chaining_table.insert(username)
    
    target = "charlie"
    
    found_ls, comp_ls = linear_search(followers_list, target)
    found_hs, comp_hs = hash_search_chaining(chaining_table, target)
    
    res_ls = "FOUND" if found_ls else "NOT FOUND"
    res_hs = "FOUND" if found_hs else "NOT FOUND"
    probe_str_ls = f"{comp_ls} comparisons"
    probe_str_hs = f"{comp_hs} chain probe" if comp_hs == 1 else f"{comp_hs} chain probes"
    
    print("=" * 60)
    print("        SOCIAL MEDIA FOLLOWERS - SEARCH COMPARISON")
    print("=" * 60)
    print()
    print(f"Total Followers (n): {len(followers_list)}")
    print(f"Target Username: '{target}'")
    print()
    print("-" * 60)
    print("ALGORITHM              RESULT          OPERATIONS")
    print("-" * 60)
    print(f"Linear Search          {res_ls:<15} {probe_str_ls}")
    print(f"Separate Chaining Hash  {res_hs:<15} {probe_str_hs}")
    print("-" * 60)
    print()
    print("Conclusion:")
    print("Linear Search  → O(n)")
    print("Hash Search    → O(1) expected")
    print("=" * 60)

    n = 500_000
    capacity = int(n / 0.75)
    large_list = [f"user_{i}" for i in range(n)]
    
    large_chaining_table = HashTableChaining(capacity=capacity)
    for u in large_list:
        large_chaining_table.insert(u)
        
    search_target = "user_49999"
    
    start_t = time.perf_counter()
    found_l, comp_l = linear_search(large_list, search_target)
    time_l = (time.perf_counter() - start_t) * 1000
    
    start_t = time.perf_counter()
    found_h, comp_h = hash_search_chaining(large_chaining_table, search_target)
    time_h = (time.perf_counter() - start_t) * 1000
    
    res_str_bench = "FOUND" if found_l else "NOT FOUND"
    probe_bench_ls = f"{comp_l:,} comparisons"
    probe_bench_hs = f"{comp_h} chain probe" if comp_h == 1 else f"{comp_h} chain probes"
    
    print()
    print("=" * 60)
    print("              SCALABILITY BENCHMARK")
    print("=" * 60)
    print()
    print(f"Dataset Size (n): {n:,} usernames")
    print(f"Target Username:  '{search_target}'")
    print()
    print("-" * 60)
    print("ALGORITHM                    TIME           OPERATIONS")
    print("-" * 60)
    print(f"Linear Search                {time_l:.4f} ms      {probe_bench_ls}")
    print(f"Separate Chaining Hash        {time_h:.4f} ms      {probe_bench_hs}")
    print("-" * 60)
    print()
    print(f"Result: Target username {res_str_bench}")
    print()
    print("Complexity:")
    print("Linear Search          → O(n)")
    print("Hash Search            → O(1) expected")
    print("=" * 60)
```

---

## 8. Input and Output

### Input
- **Small Dataset ($n = 10$)**: `['alex_99', 'beatrice_dev', 'charlie_k', 'david_pro', 'elena_m', 'frank_z', 'grace_hopper', 'hannah_b', 'ian_ml', 'julia_code']`
- **Target Query**: `'charlie'` (Not Present)
- **Large Dataset ($n = 500,000$, $m = 666,666$, $\alpha \approx 0.75$) Target**: `'user_49999'` (Present)

### Execution Output
```text
============================================================
        SOCIAL MEDIA FOLLOWERS - SEARCH COMPARISON
============================================================

Total Followers (n): 10
Target Username: 'charlie'

------------------------------------------------------------
ALGORITHM              RESULT          OPERATIONS
------------------------------------------------------------
Linear Search          NOT FOUND       10 comparisons
Separate Chaining Hash  NOT FOUND       1 chain probe
------------------------------------------------------------

Conclusion:
Linear Search  → O(n)
Hash Search    → O(1) expected
============================================================

============================================================
              SCALABILITY BENCHMARK
============================================================

Dataset Size (n): 500,000 usernames
Target Username:  'user_49999'

------------------------------------------------------------
ALGORITHM                    TIME           OPERATIONS
------------------------------------------------------------
Linear Search                1.0385 ms      50,000 comparisons
Separate Chaining Hash        0.0051 ms      2 chain probes
------------------------------------------------------------

Result: Target username FOUND

Complexity:
Linear Search          → O(n)
Hash Search            → O(1) expected
============================================================
```

---

## 9. Time & Space Complexity Analysis

### Complexity Comparison Matrix

| Complexity Attribute | Linear Search | Separate Chaining Hash Searching |
| :--- | :--- | :--- |
| **Data Structure** | Array / Python `list` | Hash Table with Linked Lists (`HashTableChaining`) |
| **Best-case Time Complexity** | $O(1)$ (target is at index 0) | $O(1)$ |
| **Average/Expected Time Complexity** | **$O(n)$** | **$O(1)$** (Expected when load factor $\alpha \approx 1$) |
| **Worst-case Time Complexity** | $O(n)$ (target at end or missing) | $O(n)$ (all keys hash to 1 bucket chain) |
| **Auxiliary Space Complexity** | $O(1)$ extra space | $O(n)$ extra space (stores bucket lists & node pointers) |
| **Pre-processing Overhead** | $O(0)$ (No setup needed) | $O(n)$ (Initial hash table construction) |
| **Best Suited For** | Small datasets ($n < 50$) or single queries | Large datasets ($n \ge 1,000$) with frequent lookups |

---

### Solution Summary for Scenario
For checking username existence among $n$ social media followers:
- **Linear Search** scans elements sequentially, yielding **$O(n)$ expected time complexity**. It is inefficient for social media scale.
- **Separate Chaining Hash Searching** maps usernames directly to hash buckets and resolves collisions using independent bucket linked lists, yielding **$O(1)$ expected time complexity**. Despite requiring $O(n)$ auxiliary memory for bucket lists, it is the optimal choice for real-time username validation.
