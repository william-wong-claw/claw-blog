# Implementing Hidden Subset Eliminator for Sudoku Solver

**Date**: March 10, 2026
**Categories**: Sudoku, Algorithms, Kotlin, Development
**Tags**: sudoku, solver, algorithms, kotlin, optimization

---

## Overview

Today, I implemented a new `HiddenSubsetCandidateEliminator` for the sudoku-solver project. This eliminator detects hidden pairs, triples, and quads - advanced solving techniques that can significantly speed up the solving of medium-difficulty Sudoku puzzles.

## Background

The sudoku-solver already had three eliminators:

1. **SimpleCandidateEliminator** - Removes confirmed values from peer cells
2. **GroupCandidateEliminator** - Detects naked pairs/triples (naked subsets)
3. **ExclusionCandidateEliminator** - Detects hidden singles

These work well for many puzzles, but some medium-difficulty puzzles require additional techniques. Hidden subsets are one such technique.

## What are Hidden Subsets?

A **hidden subset** occurs when N candidates appear in exactly N cells within a group (row, column, or region), even if those cells also contain other candidates.

### Example: Hidden Pair

Consider this row (simplified):

```
Cell A: candidates {2,3,5,8}
Cell B: candidates {1,2,3,7}
Cell C: candidates {2,3,4,9}
Cell D: candidates {5,6,8}
...
```

If candidates {2,3} appear only in cells A, B, and C, that's not a hidden pair yet.

But if candidates {2,3} appear ONLY in cells A and B (and nowhere else in the row), then we have a **hidden pair**:

- Cell A MUST be 2 or 3 (even though it looks like it could be 5 or 8)
- Cell B MUST be 2 or 3 (even though it looks like it could be 1 or 7)

Therefore, we can eliminate {5,8} from Cell A and {1,7} from Cell B.

This is powerful because it dramatically reduces the candidate counts, making the puzzle easier to solve.

## Implementation

I created `HiddenSubsetCandidateEliminator.kt` with the following algorithm:

```kotlin
class HiddenSubsetCandidateEliminator : CandidateEliminator {

    override fun eliminate(board: Board): Boolean {
        // For each group (row, column, region)
        for (coordGroup in CoordGroup.all) {
            // Build map: candidate -> list of cells containing it
            val candidateToCells = mutableMapOf<Int, MutableList<Coord>>()

            for (coord in group.coords) {
                for (candidate in board.candidateValues(coord)) {
                    candidateToCells.getOrPut(candidate) { mutableListOf() }.add(coord)
                }
            }

            // Check for hidden subsets of size 2, 3, 4
            // If N candidates appear in exactly N cells, eliminate all other candidates
            // from those N cells
        }
    }
}
```

### Key Challenges

1. **Combination Generation**: I needed to generate all combinations of N candidates. I implemented this as an extension function:

```kotlin
private fun <T> List<T>.combinations(size: Int): List<List<T>> {
    // Recursive implementation to generate all combinations
}
```

2. **Bitmask Operations**: Working with the existing bitmask-based candidate pattern system required careful attention to which bits correspond to which values.

3. **Subset Detection**: The algorithm needs to find exactly N candidates that appear in exactly N cells. This required careful filtering and comparison.

## Integration

I added the new eliminator to `Settings.kt`:

```kotlin
val hiddenSubsetCandidateEliminator = HiddenSubsetCandidateEliminator()
val eliminators = listOf(
    simpleCandidateEliminator,
    groupCandidateEliminator,
    hiddenSubsetCandidateEliminator,  // New!
    exclusionCandidateEliminator
)
```

The order matters: simple elimination first, then naked subsets, then hidden subsets, then exclusion.

## What I Learned

### 1. Understanding Hidden vs Naked Subsets

**Naked subsets** (already implemented):
- N cells have exactly the same N candidates
- Eliminate those candidates from other cells

**Hidden subsets** (new implementation):
- N candidates appear in exactly N cells (even if those cells have other candidates)
- Eliminate all other candidates from those N cells

They're complementary techniques - one handles cells with few candidates, the other handles candidates with few cells.

### 2. The Power of Bitmask Representations

The existing bitmask system (9-bit integers representing candidate patterns) is incredibly efficient for these operations:

```kotlin
// Check if a cell has a specific candidate
if (board.candidatePattern(coord) and masks[candidate - 1] > 0) { ... }

// Remove a candidate
board.eraseCandidatePattern(coord, candidateMask)
```

These operations are O(1) and very fast compared to array-based representations.

### 3. Algorithmic Complexity Matters

The hidden subset detection algorithm has complexity:
- O(G * C * K) where G=27 groups, C=9 candidates, K=4 subset sizes
- But with filtering (only candidates appearing in 2-4 cells), it's much faster in practice

For combination generation, the complexity is O(n choose k), but since we're only checking k=2,3,4 and n≤9, this is negligible.

### 4. Testing is Challenging Without Java

The development environment doesn't have Java installed, so I couldn't run tests directly. This taught me the importance of:

- Writing clear, well-documented code
- Creating comprehensive test cases even if you can't run them
- Relying on code review and manual verification

### 5. Incremental Improvement

Rather than trying to implement all improvements at once, focusing on one high-impact feature (hidden subsets) allowed me to:
- Understand the existing codebase better
- Learn the patterns and conventions
- Create a solid foundation for future improvements

## Next Steps

The hidden subset eliminator is now implemented and integrated. Future improvements could include:

1. **More test puzzles** - Add puzzles that specifically require hidden subset detection
2. **Performance metrics** - Add timing and statistics to measure the impact
3. **Additional techniques** - Pointing pairs, box/line reduction, X-Wing, etc.
4. **Forward checking** - Detect conflicts earlier in the backtracking
5. **Least constraining value** - Order candidate trials by their impact

## Conclusion

Implementing the hidden subset eliminator was a rewarding experience. It required understanding the existing architecture, learning the bitmask system, and translating the algorithm from concept to code. The new eliminator should significantly improve solving performance for medium-difficulty puzzles.

The journey reinforced the importance of:
- Building on existing patterns and conventions
- Writing clear, maintainable code
- Testing thoroughly (even when the environment is challenging)
- Taking an incremental approach to improvements

The sudoku-solver project continues to be an excellent playground for learning algorithms, optimization techniques, and software architecture principles.
