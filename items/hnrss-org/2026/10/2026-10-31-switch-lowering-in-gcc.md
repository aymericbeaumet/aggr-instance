---
title: Switch Lowering in GCC
link: https://xoranth.net/gcc-switch/
source: hnrss-org
published: 2026-10-31T08:02:26Z
updated: 2026-10-31T08:02:26Z
first_seen: 2026-10-31T15:15:45.849013716Z
authors:
- signa11
content: extracted
html: 2026-10-31-switch-lowering-in-gcc.html
---

* * *

**2024-01-14**

### Introduction

We have seen in a [previous post](https://xoranth.net/verb-parse/) that switch statements are more complex than they seem. The compiler is able to implement them as a decision tree (even though maybe it shouldn't). Is that everything it can do?

*In this post, I'll focus on `gcc`. All optimizations, except the bitset lowering, translate to clang. I'll be using the `-O3` optimization flag unless indicated otherwise.*

*I've also slightly edited `gcc` code and removed certain assertions for brevity.*

### Examples

Let us start with a simple example. A function that takes two integers `x` and `val`. Depending on the value of `x`, it calls a corresponding function (`f0`, `f1`, and so forth) with `val` as an argument and returns the result. Were we to implement this manually, we'd would use an array of function pointers, where the input serves as the index into the array.

```c
int f0(int val);
int f1(int val);
int f2(int val);
int f3(int val);
int f4(int val);

int compact(int x, int val) {
    switch (x) {
        case 0:
            return f0(val);
        case 1:
            return f1(val);
        case 2:
            return f2(val);
        case 3:
            return f3(val);
        case 4:
            return f4(val);
        default:
            return 0;
    }
    return 0;
}
```

```asm
compact:
	mov	eax, edi
	mov	edi, esi
	cmp	eax, 4
	ja	.L2
	jmp	[QWORD PTR .L4[0+rax*8]]
.L4:
	.quad	.L8
	.quad	.L7
	.quad	.L6
	.quad	.L5
	.quad	.L3
.L5:
	jmp	f3
.L3:
	jmp	f4
.L8:
	jmp	f0
.L7:
	jmp	f1
.L6:
	jmp	f2
.L2:
	xor	eax, eax
	ret
```

The compiler agrees with this approach, and transforms the switch statement into a jump table.

Moreover, it is able to identify when the values are offset by a constant:

```c
int f0(int val);
int f1(int val);
int f2(int val);
int f3(int val);
int f4(int val);

int compact(int x, int val) {
    switch (x) {
        case 97:
            return f0(val);
        case 98:
            return f1(val);
        case 99:
            return f2(val);
        case 100:
            return f3(val);
        case 101:
            return f4(val);
        default:
            return 0;
    }
    return 0;
}
```

```asm
compact:
	lea	eax, [rdi-97]
	cmp	eax, 4
	ja	.L2
	mov	edi, esi
	jmp	[QWORD PTR .L4[0+rax*8]]
.L4:
	.quad	.L8
	.quad	.L7
	.quad	.L6
	.quad	.L5
	.quad	.L3
.L5:
	jmp	f3
.L3:
	jmp	f4
.L8:
	jmp	f0
.L7:
	jmp	f1
.L6:
	jmp	f2
.L2:
	xor	eax, eax
	ret
```

Our intuition suggests that the more sparse the values, the less advantageous the jump table conversion is.

Through experimentation, we find that `gcc` considers the threshold to be around eight jump table entries per `case` label.

In other words, the following function compiles to a jump table:

```c
int f0(int val);
int f1(int val);
int f2(int val);
int f3(int val);
int f4(int val);

int compact_7(int x, int val) {
    switch (x) {
        case 0:
            return f0(val);
        case 7:
            return f1(val);
        case 14:
            return f2(val);
        case 21:
            return f3(val);
        case 34:
            return f4(val);
        default:
            return 0;
    }
    return 0;
}
```

```asm
compact_7:
	mov	eax, edi
	mov	edi, esi
	cmp	eax, 34
	ja	.L2
	jmp	[QWORD PTR .L4[0+rax*8]]
.L4:
	.quad	.L8
	.quad	.L2
	; ...
	.quad	.L2
	.quad	.L7
	.quad	.L2
	; ...
	.quad	.L2
	.quad	.L6
	.quad	.L2
	; ...
	.quad	.L2
	.quad	.L5
	.quad	.L2
	; ...
	.quad	.L2
	.quad	.L3
.L2:
	xor	eax, eax
	ret
.L8:
	jmp	f0
.L7:
	jmp	f1
.L6:
	jmp	f2
.L5:
	jmp	f3
.L3:
	jmp	f4
```

But this other function is expanded as a tree of comparisons:

```c
int f0(int val);
int f1(int val);
int f2(int val);
int f3(int val);
int f4(int val);

int sparse_8(int x, int val) {
    switch (x) {
        case 0:
            return f0(val);
        case 8:
            return f1(val);
        case 16:
            return f2(val);
        case 24:
            return f3(val);
        case 40:
            return f4(val);
        default:
            return 0;
    }
    return 0;
}
```

```asm
sparse_8:
	mov	eax, edi
	mov	edi, esi
	cmp	eax, 16
	je	.L2
	jle	.L10
	cmp	eax, 24
	je	.L7
	cmp	eax, 40
	jne	.L1
	jmp	f4
.L10:
	test	eax, eax
	je	.L4
	cmp	eax, 8
	jne	.L1
	jmp	f1
.L1:
	xor	eax, eax
	ret
.L4:
	jmp	f0
.L7:
	jmp	f3
.L2:
	jmp	f2
```

Here's the comparison tree visualized for clarity:

A couple of observations:

1. We've only tried equally spaced values so far. Can the compiler identify transformations that are profitable on a subset of all cases?
2. So far each `case` led to a different result. What if we had different cases with the same result?
3. The switch in `sparse_8` could be simplified by dividing `x` by eight before the jump. Interestingly, `gcc` *doesn't* find this simplification.

Let's dig into 1. and 2.

#### Outliers

Altering our first example by including an outlier value shows that the compiler is able to recognize dense subsets of cases.

```c
int f0(int val);
int f1(int val);
int f2(int val);
int f3(int val);
int f4(int val);
int f5(int val);

int outlier(int x, int val) {
    switch (x) {
        case 1:
            return f0(val);
        case 2:
            return f1(val);
        case 3:
            return f2(val);
        case 4:
            return f3(val);
        case 5:
            return f4(val);
        case 100:
            return f5(val);
        default:
            return 0;
    }
    return 0;
}
```

```asm
outlier:
	mov	eax, edi
	mov	edi, esi
	cmp	eax, 5
	jg	.L2
	test	eax, eax
	jle	.L1
	cmp	eax, 5
	ja	.L4
	jmp	[QWORD PTR .L6[0+rax*8]]
.L6:
	.quad	.L4
	.quad	.L4
	.quad	.L9
	.quad	.L8
	.quad	.L7
	.quad	.L5
.L2:
	cmp	eax, 100
	jne	.L1
	jmp	f5
.L7:
	jmp	f3
.L5:
	jmp	f4
.L9:
	jmp	f1
.L8:
	jmp	f2
.L4:
	jmp	f0
.L1:
	xor	eax, eax
	ret
```

The compiler emits a branch for case 100, but emits a jump table for case 1 through 5.

#### Contiguous

In our examples so far, each case statement lead to a different code block. Let's test what happens when multiple `case` labels invoke the same function.

```c
int f1(int val);
int f2(int val);
int f3(int val);
int f4(int val);
int f5(int val);

int blocks_2(int x, int val) {
    switch (x) {
        case 0:  case 1:  case 2:  case 3:
            return f1(val);
        case 21:  ...  case 24:
            return f2(val);
        case 42:  ...  case 45:
            return f3(val);
        case 63:  ...  case 66:
            return f4(val);
        case 84:  ...  case 87:
            return f5(val);
        default:
            return 0;
    }
    return 0;
}
```

```asm
blocks_2:
	mov	eax, edi
	mov	edi, esi
	cmp	eax, 45
	jg	.L2
	cmp	eax, 41
	jg	.L3
	cmp	eax, 3
	jg	.L4
	test	eax, eax
	js	.L1
	jmp	f1
.L2:
	cmp	eax, 66
	jg	.L8
	cmp	eax, 62
	jle	.L1
	jmp	f4
.L4:
	sub	eax, 21
	cmp	eax, 3
	ja	.L1
	jmp	f2
.L8:
	sub	eax, 84
	cmp	eax, 3
	ja	.L1
	jmp	f5
.L3:
	jmp	f3
.L1:
	xor	eax, eax
	ret
```

We have 20 case labels, with values between 0 and 87. The compiler could lower this `switch` to a jump table with 88 entries. Our hypothesis for the heuristic is that a jump table conversion is profitable when its size is smaller than `8 * num_case_labels`. Since `88 < 20 * 8`, the conversion should be profitable in this case, however `gcc` opted to lower the switch to a sequence of comparisons.

To better understand why, let's examine a specific block of `case` labels.

```c
int blocks_2(int x, int val) {
    switch (x) {
        // ...
        case 42:  case 43:  case 44:  
        case 45:
            return f3(val);
        // ...
    }
    return 0;
}
```

```asm
blocks_2:
	mov	eax, edi ; Move `val` to $eax
	mov	edi, esi
	cmp	eax, 45
	; If val <= 45, do _not_ jump
	jg	.L2
	cmp	eax, 41
	; If val >= 42, jump to L3
	jg	.L3
	; ...
.L2:
; ...
.L3:
        ; We reach L3 only if 42 <= val <= 45
        jmp	f3        ; Call f3(val)
; ...
```

The compiler realizes that the cases are contiguous and can be implemented with two comparisons instead of four. If the input of the jump table heuristic is the number of *comparisons* instead of *case labels*, things work out:

We have a total of 10 comparisons. Since `10 * 8 < 88`, our revised heuristic suggests that a jump table transformation is unprofitable.

#### Non-contiguous

Let us try a non-contiguous case.

```c
int f1(int val);

int bittest(int x, int val) {
    switch (x) {
        case '0':  case '2':  case '4':  
        case '5':  case '7':  case '9':  
        case 'A':  case 'C':  case 'E':
            return f1(x);
        default:
            return 0;
    }
    return 0;
}
```

```asm
bittest:
	lea	eax, [rdi-48]
	cmp	eax, 21
	ja	.L1
	mov	edx, 2753205  ; ???
	bt	rdx, rax
	jc	.L7
.L1:
	xor	eax, eax
	ret
.L7:
	jmp	f1
```

The compiler emits non-intuitive machine code. Why is there no `cmp` nor jump tables? Where does the constant `2753205` come from?

An hint comes from the instruction `bt`. According to the [Intel docs](https://www.felixcloutier.com/x86/bt), `bt`:

> Selects the bit in a bit string (specified with the first operand, called the bit base) at the bit-position designated by the bit offset (specified by the second operand) and stores the value of the bit in the CF flag.

If we examine the binary representation of `2753205`, we find that it is in fact a bitset. The bits that are set to one correspond to the ASCII values of the case labels, each value being reduced by 48.

### Implementation

We've encountered several distinct lowering strategies employed by gcc. For a more in-depth understanding, we can delve into the compiler's source code.

The compiler pass that responsible for lowering `switch` statements is the (appropriately named) `pass_lower_switch`, which in turn calls `analyze_switch_statement`.

Since `gcc` source can be daunting to follow, I'll provide a step-by-step commented version below. Following this, I will break down each section to explain its functionality.

```cpp
bool switch_decision_tree::analyze_switch_statement () 
{
    unsigned l = gimple_switch_num_labels (m_switch);
    auto_vec<cluster *> clusters;
    // ...

    compute_cases_per_edge ();
```

Some preamble code. `m_switch` points to a gimple object. Gimple is the internal representation used by `gcc`. Through this, we extract the overall count of cases and the individual counts of cases for each outgoing edge.

We also allocate a vector that will record the lowering strategy chosen for each case label.

```cpp
    for (unsigned i = 1; i < l; i++)
    {
        tree elt = gimple_switch_label (m_switch, i);
	// ...
        tree low = CASE_LOW (elt);
        tree high = CASE_HIGH (elt);

        profile_probability p
            = case_edge->probability.apply_scale (1, (intptr_t) (case_edge->aux));
        clusters.quick_push (new simple_cluster (low, high, elt, case_edge->dest, p));
        // ...
    }
```

More setup code. We notice that gimple has a concept of high and low value for each label. That is, contiguous `case` labels have already joined together at this stage.

For each case label, we allocate a `simple_cluster`. This cluster serves as a record, capturing the range of values associated with the label, the relevant output edge, and the likelihood of this cluster being chosen. This probability comes from profile-guided optimization. By default it is uniform among case labels.

```cpp
    // ...
    vec<cluster *> output = bit_test_cluster::find_bit_tests (clusters);
```

We transform the vector of clusters we have created. The name `bit_test_cluster` hints to the bitset lowering strategy we saw before.

```cpp
    /* Find jump table clusters.  */
    vec<cluster *> output2;
    auto_vec<cluster *> tmp;
    // ...
    
    for (unsigned i = 0; i < output.length (); i++) {
        cluster *c = output[i];
        if (c->get_type () != SIMPLE_CASE) {
            if (!tmp.is_empty ()) {
                // ... See below for what's here
                tmp.truncate (0);
            }
            output2.safe_push (c); 
        }
        else
            tmp.safe_push (c);
    }
```

This section allocates a vector for the transformed cases and a scratch vector. Any case that wasn't altered during the bitset transformation process is gathered in this scratch vector. These cases are then processed and cleared whenever we come across an optimized cluster.

Essentially, in this section we are looking for sequences of case labels that couldn't be converted into bitset clusters.

```cpp
                vec<cluster *> n = jump_table_cluster::find_jump_tables (tmp);
                output2.safe_splice (n);
                n.release ();
```

We attempt to convert these consecutive labels into a jump table and add the outcome to the output.

```cpp
    /* We still can have a temporary vector to test.  */
    if (!tmp.is_empty ()) {
        vec<cluster *> n = jump_table_cluster::find_jump_tables (tmp);
        output2.safe_splice (n);
        n.release ();
    }
```

This is a coda for the loop above.

```cpp
    // ...

    bool expanded = try_switch_expansion (output2);
    release_clusters(output2);
    return expanded;
}
```

Whatever is left goes through another pass, which turns the switch statement into a decision tree. The bitsets and jump table are leaves of the decision tree.

#### Bitsets

Let's dig further into `bit_test_cluster::find_bit_tests`. The high level structure of the code is as follow.

Initially, we aim to find the optimal method to bundle all clusters into bitsets. Optimality, in this context, refers to minimizing the number of bitsets used. We do so using dynamic programming.

Subsequently, we iterate over the bitsets and apply a profitability heuristic. If the bitsets meet this heuristic, we emit a `bit_test_cluster`; otherwise, we keep the original `simple_cluster`s.

```cpp
vec<cluster *> bit_test_cluster::find_bit_tests (vec<cluster *> &clusters)
{
    // ...

    unsigned l = clusters.length ();
    auto_vec<min_cluster_item> min;
    // ...

    min.quick_push (min_cluster_item (0, 0, 0));
    for (unsigned i = 1; i <= l; i++)
    {
        /* Set minimal # of clusters with i-th item to infinite.  */
        min.quick_push (min_cluster_item (INT_MAX, INT_MAX, INT_MAX));

        for (unsigned j = 0; j < i; j++)
        {
            if (min[j].m_count + 1 < min[i].m_count
                    && can_be_handled (clusters, j, i - 1))
                min[i] = min_cluster_item (min[j].m_count + 1, j, INT_MAX);
        }

        gcc_checking_assert (min[i].m_count != INT_MAX);
    }
```

For the dynamic programming, we'll use a vector of `min_cluster_item` structures. Each `min_cluster_item` has three components:

- The first records how many bitsets do we need to represent the first *i* case labels, where `i` is the index in the vector.
- The second records the **start** of the last bitset.
- The third is unused.

The dynamic programming is as follows. We want to find the minimum number of bitsets to represent the first *i* case labels.

The base case is straightforward, a single case label can be represented by a single bitset.

Suppose we have already have found the optimal number of bitsets for the first *j* case label, for all . We define to be the optimal number of splits for the first `i` elements. This means we already computed , and are now seeking .

If the range of case labels can be represented as a bitset cluster, it must be true that

Conversely, assume we have the optimal assignment for the first *i* labels. If the last bitset starts at label , this assignment is also optimal when restricted to . Were this not the case, we could improve the assignment on as well.

Therefore, it must be that:

These conditions are exactly what the `if` statement in the code above checks for.

The `can_be_handled` function controls whether a range of `case` labels can be turned into a bitset test. It tests two criteria:

- The *combined* range of all the case labels must be smaller than the width of a register. On contemporary systems, the combined range should be less than 64.
- The case labels must have at most 3 *unique* outgoing edges.

If there is more than one edge, we emit a cascade of bit tests.

```cpp
    vec<cluster *> output;
    // ...
    
    /* Find and build the clusters.  */
    for (unsigned end = l;;)
    {
        int start = min[end].m_start;

        if (is_beneficial (clusters, start, end - 1))
        {
            bool entire = start == 0 && end == clusters.length ();
            output.safe_push (new bit_test_cluster (clusters, start, end - 1, entire));
        }
        else
            for (int i = end - 1; i >= start; i--)
                output.safe_push (clusters[i]);

        end = start;

        if (start <= 0)
            break;
    }

    output.reverse ();
    return output;
}
```

Now we convert only bitsets that satisfy the `is_beneficial` test. In the code `count` represents the number of case labels handled by the cluster, while `uniq` denotes the number of distinct outgoing edges.

```cpp
bool bit_test_cluster::is_beneficial (unsigned count, unsigned uniq)
{
    return (((uniq == 1 && count >= 3)
              || (uniq == 2 && count >= 5)
              || (uniq == 3 && count >= 6)));
}
```

#### Jump tables

The logic for jump table conversion mirrors the one used for bit test conversion. It leverages dynamic programming to identify "clusters" that can be profitably converted to a jump table.

Let's delve into the code.

```cpp
vec<cluster *> jump_table_cluster::find_jump_tables (vec<cluster *> &clusters)
{
    if (!is_enabled ())
        return clusters.copy ();

    unsigned l = clusters.length ();
    auto_vec<min_cluster_item> min;
    min.reserve (l + 1);
```

We start by preallocating a vector of `min_cluster_item`. We'll use this vector to keep track of the best jump table assignments for consecutive `case` clusters.

```cpp
    min.quick_push (min_cluster_item (0, 0, 0));
```

We also push the base case for dynamic programming. This corresponds to using a single jump table for the whole `switch`.

Let's focus now on the main loop.

```cpp
    unsigned HOST_WIDE_INT max_ratio = /* Used in the main heuristic. See next snippet. */;
                
    for (unsigned i = 1; i <= l; i++)
    {
        // NOTE(xoranth): Base case
        min.quick_push (min_cluster_item (INT_MAX, INT_MAX, INT_MAX));
        
        /* Pre-calculate number of comparisons for the clusters.  */
        HOST_WIDE_INT comparison_count = 0;
        for (unsigned k = 0; k <= i - 1; k++)
        {
            simple_cluster *sc = static_cast<simple_cluster *> (clusters[k]);
            // NOTE(xoranth): get_comparison_count is 2 for contiguous case 
            // labels, and 1 otherwise
            comparison_count += sc->get_comparison_count ();
        }

        for (unsigned j = 0; j < i; j++)
        {
            unsigned HOST_WIDE_INT s = min[j].m_non_jt_cases;
            if (i - j < case_values_threshold ())
                s += i - j;
            
            /* Prefer clusters with smaller number of numbers covered.  */
            if (/* Check if new split is better. See next snippet */) 
            {
                // NOTE(xoranth): Update best score.
                min[i] = min_cluster_item (min[j].m_count + 1, j,
					   s /* `s` is the number of unhandled cases */);
            }

            simple_cluster *sc = static_cast<simple_cluster *> (clusters[j]);
            comparison_count -= sc->get_comparison_count ();
        }
    }
```

The dynamic programming loop mirrors the one we used for bitsets. We want to find the minimum number of jump tables to represent the first i case labels.

We iterate over splits at the `j`-th case label, for `j < i`. During each iteration we track the best scoring split. The special case `j == 0` represents a single jump table for the entire `switch` statement.

Inside this inner loop we also keep track of:

1. How many distinct jump tables we would need to lower the switch.
2. The **start** of the *last* jump table of the decomposition.
3. How many cases *cannot* be handled by a jump table. In the code, this is the accumulator `s`.

Point 1. and 2. are analogous to the bitset lowering pass, while 3. is new and specific to jump tables.

Namely, there is some overhead to dispatching to a jump table, due to the indirect jump and the need to check bounds. Therefore `gcc` requires a minimum amount of `case` labels to generate a jump table.

If a split would generate a jump table that is too small, the size of the jump table is added to an accumulator `s`, which is used as a tie-breaker between different split candidates.

To compute the scoring, we need the comparison count between `j`-th and the `i`-th case label. We compute it summing up the number of comparisons for all labels, and decreasing the count as we progress in the inner loop.

We also need the number of unhandled `case` labels. We determine it comparing the size of the jump table to the result of the `case_values_threshold` function. This function is architecture dependent and takes into account the presence of switch-case specific instructions, but on modern x64 and Arm64 machines, it defaults to 5.

```cpp
unsigned int default_case_values_threshold (void) {
    return (targetm.have_casesi () ? 4 : 5);
}
```

Let's now focus on the main heuristic, starting with how `max_ratio` is computed. I've reproduced below the main loop, omitting lines unrelated to the main heuristic.

We've seen in the examples section that gcc uses the ratio between jump table entries and comparisons as a metric for the lowering the heuristic. The code below shows that distinct ratios are used when optimizing for speed (flags `-O[123|fast]`) or size (flag `-Os`). Specifically, a ratio of 8 is used when optimizing for speed, while a ratio of 2 is used when optimizing for size.

```cpp
    unsigned HOST_WIDE_INT max_ratio
        = (optimize_insn_for_size_p ()
                ? param_jump_table_max_growth_ratio_for_size    // This is `2`
                : param_jump_table_max_growth_ratio_for_speed); // This is `8`

    for (unsigned i = 1; i <= l; i++)
    {
        /* Set minimal # of clusters with i-th item to infinite.  */
        min.quick_push (min_cluster_item (INT_MAX, INT_MAX, INT_MAX));

        // NOTE(xoranth): Compute comparison count, see previous snippet

        for (unsigned j = 0; j < i; j++)
        {
            // NOTE(xoranth): Compute unhandled cases, see previous snippet
            
            /* Prefer clusters with smaller number of numbers covered.  */
            if ((min[j].m_count + 1 < min[i].m_count)
               || (min[j].m_count + 1 == min[i].m_count && s < min[i].m_non_jt_cases)
               && can_be_handled (clusters, j, i - 1, max_ratio, comparison_count)) 
            {
                min[i] = min_cluster_item (min[j].m_count + 1, j,
					   s /* `s` is the number of unhandled cases */ );
            }

            // NOTE(xoranth): Update comparison count, see previous snippet
        }
    }
```

Now onto the main loop. Given two splits, we prefer the one that results in fewer jump tables being emitted. Or, if they would result in the same number of jump tables, we prefer the one that handles the most `case` labels. We also ignore splits that would result in unprofitable jump tables, where profitability is checked by the `can_be_handled` function.

The `can_be_handled` condition, checks for overflow and compares the ratio of case labels to jump table entries:

```cpp
bool jump_table_cluster::can_be_handled (const vec<cluster *> &clusters,
                                         unsigned start, unsigned end,
                                         unsigned HOST_WIDE_INT max_ratio,
                                         unsigned HOST_WIDE_INT comparison_count)
{
    /* If the switch is relatively small such that the cost of one
       indirect jump on the target are higher than the cost of a
       decision tree, go with the decision tree.

       [...]

       For algorithm correctness, jump table for a single case must return
       true.  We bail out in is_beneficial if it's called just for
       a single case.  */
    if (start == end)
        return true;

    unsigned HOST_WIDE_INT range = get_range (clusters[start]->get_low (), clusters[end]->get_high ());
    /* Check for overflow.  */
    // ...

    return lhs <= max_ratio * comparison_count;
}
```

The coda of the function also mirrors the one for bitset clusters. It iterates over all jump tables in reverse order, and prunes the ones that fail the `is_beneficial` check.

```cpp
    vec<cluster *> output;
    output.create (4);

    /* Find and build the clusters.  */
    for (unsigned int end = l;;)
    {
        int start = min[end].m_start;

        /* Do not allow clusters with small number of cases.  */
        if (is_beneficial (clusters, start, end - 1))
            output.safe_push (new jump_table_cluster (clusters, start, end - 1));
        else
            for (int i = end - 1; i >= start; i--)
                output.safe_push (clusters[i]);
        end = start;

        if (start <= 0) break;
    }

    output.reverse ();
    return output;
}
```

The `is_beneficial` check for jump tables aims to prune small jump tables. It compares the number of case labels to `case_values_threshold`.

```cpp
bool jump_table_cluster::is_beneficial (const vec<cluster *> &, unsigned start, unsigned end) {
    if (start == end) return false;
    return end - start + 1 >= case_values_threshold ();
}
```

#### Binary tree

If `gcc` cannot emit a bitset cluster or a jump table, it falls back to a binary tree of comparison.

In emitting the tree, it takes into account the probability of each case label cluster. The probability can be obtained via profile-guided optimization, or with the `__builtin_expect_with_probability` builtin.

The code is pretty straightforward.

```cpp
void switch_decision_tree::balance_case_nodes (case_tree_node **head, case_tree_node *parent) 
{
    case_tree_node *np = *head;
    if (np) {
        int i = 0;
        case_tree_node **npp, *left;
        profile_probability prob = profile_probability::never ();
```

Sum probability of all case labels.

```cpp
        /* Count the number of entries on branch.  Also count the ranges.  */
        while (np) {
            i++;
            prob += np->m_c->m_prob;
            np = np->m_right;
        }
```

Split the case label list such that both halves have equal probability.

```cpp
        if (i > 2) {
            npp = head;
            left = *npp;
	    // NOTE(xoranth): this divides prob by 2.
            profile_probability pivot_prob = prob.apply_scale (1, 2);

            while (1) {
                /* Skip nodes while their probability does not reach that amount.  */
                prob -= (*npp)->m_c->m_prob;
                if ((prob.initialized_p () && prob < pivot_prob) || ! (*npp)->m_right)
                    break;
                npp = &(*npp)->m_right;
            }

            np = *npp;
            *npp = 0;
            *head = np;
            np->m_parent = parent;
            np->m_left = left == np ? NULL : left;
```

Recurse in both halves to build the binary tree.

```cpp
            balance_case_nodes (&np->m_left, np);
            balance_case_nodes (&np->m_right, np);
            np->m_c->m_subtree_prob = np->m_c->m_prob;
            if (np->m_left)
                np->m_c->m_subtree_prob += np->m_left->m_c->m_subtree_prob;
            if (np->m_right)
                np->m_c->m_subtree_prob += np->m_right->m_c->m_subtree_prob;
        } else {
	    // [...] Base case
        }
    }
}
```

### Conclusion

We've seen the heuristics and lowering strategies `gcc` employs when compiling switch statements, including:

- Compressing multiple `case` labels into a bitset.
- Transforming the `switch` statement into a jump table.
- Transforming the `switch` statement into a binary decision tree.

For anyone interested in delving deeper, I'd suggest examining the `gcc` [source code](https://github.com/gcc-mirror/gcc/blob/master/gcc/tree-switch-conversion.cc), and reading [this paper](https://www.researchgate.net/publication/245584786_A_Superoptimizer_Analysis_of_Multiway_Branch_Code_Generation).

*Finally, I want to thank B.X.V., C.P., and V.P. for their feedback when writing this post*
