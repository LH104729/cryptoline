# Algebra packed-limb experiment

The experimental `-algebra-pack-limbs` option detects expressions of the form

```
a0 + a1 * 2^w + a2 * 2^(2*w) + ...
```

and introduces a fresh Singular ring variable `C` for each selected packed
expression.

For each pack, occurrences of the low limb `a0` in the original generators and
target are printed as

```
C - (a1 * 2^w + a2 * 2^(2*w) + ...)
```

before Singular parses the polynomial. This can cause complete packed sums to
normalize to `C` without matching and replacing the complete sum directly.

The low limb remains in the Singular ring. The ideal also retains the original,
non-rewritten linking equation

```
C - (a0 + a1 * 2^w + a2 * 2^(2*w) + ...) = 0
```

so the relation between `C` and the original limb variables is explicit. The
linking equation is printed with the raw expression printer; its `a0` occurrence
is not rewritten.

If multiple detected packs share the same low limb, only the first detected pack
is selected for substitution, avoiding conflicting definitions for that limb.

When at least one pack is selected, the Singular ring is ordered as

```
(low_limb_0, ..., low_limb_k,
 C_0, ..., C_k,
 remaining_variables),
(lp(number_of_low_limbs), dp(number_of_other_variables))
```

This makes each low limb, rather than its compact `C`, the leading variable in
its raw linking equation. The intended reduction direction is therefore

```
a0 -> C - (a1 * 2^w + a2 * 2^(2*w) + ...)
```

and not the reverse expansion of `C` into all limbs. If no pack is selected,
the configured CryptoLine monomial order is used unchanged.
