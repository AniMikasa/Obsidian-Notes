## Set Theory
be careful about subset (子集) and proper-subset (真子集)

Empty set ∅ is unique

$A − (B ∪ C) = (A − B) ∩ (A − C)$
$A − (B ∩ C) = (A − B) ∪ (A − C)$

Generalized Union (广义并) and Generalized Intersection (广义交).

For technical reason, we define ∪ ∅ = ∅, but ∩ ∅ is undefined

power set (幂集)

the power set is always non-empty

$2^A$ always has “more” elements than that of A.

$2^A ∈ 2^B ⇒ A ∈ B$
$A ⊆ B ⇔ 2^A ⊆ 2^B$
$2^A ∩ 2^B = 2^{A∩B}$
$2^A ∪ 2^B ⊆ 2^{A∪B}$

Let A be a family of sets. Then A is said to be a transitive set if any element of
an element in A is still an element in A, i.e.,(∀x)(∀y)((y ∈ A ∧ x ∈ y) → x ∈ A)

if x ∈ A, then x ⊆ A.

A is a transitive set, if and only if, 2 A is a transitive set.

If A is a transitive set, then ∪ A is a transitive set.

If any element in A is a transitive set, then ∪ A is a transitive set.

⟨x, y⟩ := {{x}, {x, y}}
Let A be a set and $x, y ∈ A$. Then we have $⟨x, y⟩ ∈ 2^{2^A}$ .

For example, we have $⟨a, b, c, d⟩ = ⟨⟨a, b, c⟩, d⟩ = ⟨⟨⟨a, b⟩, c⟩, d⟩$

Basic Properties of Set Operations:
1. A × ∅ = ∅ × A = ∅
2. If A = B, and A, B are non-empty, then A × B = B × A
3. (A × B) × C = A × (B × C)
4. A × (B ∪ C) = (A × B) ∪ (A × C)
5. A × (B ∩ C) = (A × B) ∩ (A × C)
6. (B ∪ C) × A = (B × A) ∪ (C × A)
7. (B ∩ C) × A = (B × A) ∩ (C × A)

For any two sets A, B and non-empty set C, we claim that the followings are equivalent:
1. A ⊆ B
2. A × C ⊆ B × C
3. C × A ⊆ C × B

Let A, B, C, D be non-empty sets. We have A × B ⊆ C × D iff A ⊆ C and B ⊆ D


we cannot just describe a set of things satisfying a predicate as a set without any restriction.The key idea for resolving this issue is that sets are constructed from axioms, which leads to the Axiomatic Set Theory(公理集合论).

Axiom of Extensionality (外延公理) specifying when two sets are the same.
Axiom of Empty Set (空集存在公理) in order to generate the first set in axiomatic set theory, the empty set.
Suppose that we have two sets. We can put they together in a bigger set based on the Axiom of Pairing (配对公理).
This is done by the Axiom of Union (并集公理) which allows us to unpack a set of sets and create a flatter set.
According to Axiom Schema of Separation (分离公理模式),there is no “universal set” in axiomatic set theory.Using this axiom schema, we can define set intersection and set complement by A∩B ={a ∈ A : a ∈ B} and A − B = {a ∈ A : a ∈/ B}.
Now, given a set, we want to always be able to build a larger set (we will understand the need later when we discuss the difference between countable and uncountable set).This is done by the Axiom of Power Set (幂集公理)
Therefore, we have to introduce the Axiom of Infinity (无穷公理) that guarantees the existence of at least one infinite set, namely a set containing the natural numbers.
Axiom of Regularity (正则公理) This axiom together with the axiom of pairing imply that no set is an element of itself, and that no infinite sequence $A_1, A_2, . . .$ such that $A_{i+1} ∈ A_i$ .

## Relation
