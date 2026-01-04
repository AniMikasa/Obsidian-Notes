be careful about subset (子集) and proper-subset (真子集)

Empty set ∅ is unique

A − (B ∪ C) = (A − B) ∩ (A − C)
A − (B ∩ C) = (A − B) ∪ (A − C)

Generalized Union (广义并) and Generalized Intersection (广义交).

For technical reason, we define ∪ ∅ = ∅, but ∩ ∅ is undefined

power set (幂集)

the power set is always non-empty

2 A always has “more” elements than that of A.

2 A ∈ 2 B ⇒ A ∈ B
A ⊆ B ⇔ 2 A ⊆ 2 B
2 A ∩ 2 B = 2A∩B
2 A ∪ 2 B ⊆ 2 A∪B

Let A be a family of sets. Then A is said to be a transitive set if any element of
an element in A is still an element in A, i.e.,(∀x)(∀y)((y ∈ A ∧ x ∈ y) → x ∈ A)

if x ∈ A, then x ⊆ A.

A is a transitive set, if and only if, 2 A is a transitive set.

If A is a transitive set, then ∪ A is a transitive set.

If any element in A is a transitive set, then ∪ A is a transitive set.

⟨x, y⟩ := {{x}, {x, y}}
Let A be a set and $x, y ∈ A$. Then we have $⟨x, y⟩ ∈ 2^{2^A}$ .

For example, we have $⟨a, b, c, d⟩ = ⟨⟨a, b, c⟩, d⟩ = ⟨⟨⟨a, b⟩, c⟩, d⟩$
