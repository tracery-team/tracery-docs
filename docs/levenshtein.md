# Levenshtein Distance

The Levenshtein distance is a metric for measuring the difference between two sequences. It is named after the Soviet mathematician Vladimir Levenshtein, who considered this distance in 1965. The Levenshtein distance between two strings is defined as the minimum number of single-character edits (insertions, deletions, or substitutions) required to change one string into the other.

## Implementation in TypeScript

The following TypeScript function calculates the Levenshtein distance between two strings, `s1` and `s2`:

```typescript
export function levenshtein(s1: string, s2: string): number {
  if (s1 === s2) {
    return 0;
  }

  const s1_len = s1.length;
  const s2_len = s2.length;
  if (s1_len === 0) {
    return s2_len;
  }
  if (s2_len === 0) {
    return s1_len;
  }

  let v0 = new Array(s1_len + 1);
  let v1 = new Array(s1_len + 1);

  for (let i = 0; i < s1_len + 1; i++) {
    v0[i] = i;
  }

  for (let j = 1; j <= s2_len; j++) {
    v1[0] = j;
    const char_s2 = s2[j - 1];

    for (let i = 0; i < s1_len; i++) {
      const char_s1 = s1[i];
      const cost = char_s1 === char_s2 ? 0 : 1;
      const m_min = Math.min(v0[i + 1] + 1, v1[i] + 1, v0[i] + cost);
      v1[i + 1] = m_min;
    }

    [v0, v1] = [v1, v0];
  }

  return v0[s1_len];
}
```

### Explanation

#### **Base Cases**:

- If the two strings are identical, the distance is `0`.
- If one of the strings is empty, the distance is the length of the other string.

#### **Initialization**:

- Two arrays, `v0` and `v1`, are initialized to store the distances.

#### **Distance Calculation**:

- The outer loop iterates over each character of `s2`.
- The inner loop iterates over each character of `s1`.
- The cost is `0` if the characters are the same, otherwise `1`.
- The minimum cost is calculated considering insertion, deletion, and substitution.

#### **Result**:

- The final distance is stored in `v0[s1_len]`.

## Applying Levenshtein Distance in Search

The `applySearch` function uses the Levenshtein distance to filter and sort elements based on their similarity to a target string.

```typescript
export const applySearch = <T>(
  elements: T[],
  distanceFn: (element: T) => number,
  maxDistance: number = 20
) => {
  const filteredElements = elements
    .map((element) => {
      return Object.freeze({
        element,
        distance: distanceFn(element),
      });
    })
    .filter(({ distance }) => distance <= maxDistance);
  filteredElements.sort(({ distance: distanceA }, { distance: distanceB }) => {
    const weight = distanceA - distanceB;
    return weight;
  });
  return filteredElements.map(({ element }) => element);
};
```

### Explanation

#### **Mapping Elements**:

- Each element is mapped to an object containing the element and its distance to the target string.

#### **Filtering**:

- Elements with a distance greater than `maxDistance` are filtered out.

#### **Sorting**:

- The remaining elements are sorted by their distance in ascending order.

#### **Result**:

- The sorted elements are returned.

This function is useful for implementing search functionalities where results are ranked by their similarity to the search query.
