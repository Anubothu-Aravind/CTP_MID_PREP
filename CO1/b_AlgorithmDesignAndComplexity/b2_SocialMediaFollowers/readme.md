# b.2 Social Media Followers: Username Existence Verification

---

## 1. Problem Statement

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

## 3. Step-by-Step Example

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

## 4. Algorithm

### Linear Search Algorithm

1. **Input**: List of follower usernames $L$ of size $n$, target query string $T$.
2. **Procedure**:
   1. Start at the first element (index $0$).
   2. Compare the current element $L[i]$ with the target key $T$.
   3. If they match, return `(FOUND, comparisons)` (where `comparisons` $= i + 1$) and exit.
   4. If they do not match, move to the next element (increment index $i$ and comparison count).
   5. Repeat steps 2–4 until a match is found or the end of the list is reached.
   6. If the end of the list is reached without a match, return `(NOT FOUND, comparisons)` (where `comparisons` $= n$).
3. **Output**: Result status (`FOUND` / `NOT FOUND`) and total element comparisons performed.

---

### Hash-based Search Algorithm (Separate Chaining)

1. **Input**: Separate Chaining Hash Table $H$ of capacity $m$, target query string $T$, hash function $h(k) = \sum_{c \in k} \text{ASCII}(c)$.
2. **Procedure**:
   1. Compute the ASCII summation hash value: $\text{hash-code}(T) = \sum_{c \in T} \text{ASCII}(c)$.
   2. Map the hash value to a bucket index using modulo arithmetic: $idx = \text{hash-code}(T) \bmod m$.
   3. Direct jump to access the bucket chain $chain = H[idx]$ in the hash table.
   4. Start at the first node of the bucket chain.
   5. Compare the current node's value with the target key $T$.
   6. If they match, return `(FOUND, probes)` (where `probes` is the current node position) and exit.
   7. If they do not match, move to the next node in the chain (increment probe count).
   8. Repeat steps 5–7 until a match is found or the end of the chain is reached.
   9. If the end of the chain is reached without a match, return `(NOT FOUND, probes)` (meaning the target is not present in the table).
3. **Output**: Result status (`FOUND` / `NOT FOUND`) and total chain nodes probed.

---

## 5. Implementation

```python
class HashTableChaining:
    def __init__(self, capacity=10):
        self.capacity = capacity
        self.buckets = [[] for _ in range(capacity)]

    def _hash(self, key):
        return sum(ord(c) for c in key) % self.capacity

    def insert(self, key):
        idx = self._hash(key)
        self.buckets[idx].insert(0, key)

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
```

---

## 6. Input and Output

### Input

- **Dataset ($n = 10$)**: `['alex_99', 'beatrice_dev', 'charlie_k', 'david_pro', 'elena_m', 'frank_z', 'grace_hopper', 'hannah_b', 'ian_ml', 'julia_code']`
- **Target Query**: `'charlie'` (Not Present)

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
```
