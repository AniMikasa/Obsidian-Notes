**Definitions**:
A proposition is a declarative sentence/assertion that is either true or false(but not both).
Those propositions that cannot be further decomposed are called atomic propositions (原子命题)

Those propositions composed by multiple atomic propositions and logical operators, such
as “if snow is black, then 1 + 1 = 3”, are called compound propositions (复合命题).
Those undetermined (atomic) propositions P, Q, R, . . . are called(atomic) propositional variables (命题变元).

By putting propositional variables together using logical operators, we obtain a compound proposition formula (复合命题公式) such as P ∧ Q → R, ¬P ∨ Q.

Let P be a proposition. Then ¬P is a new compound proposition such that it is true iff P is false, where “iff” standards for “if and only if”.

Let P and Q be two propositions. Then
– P ∧ Q is conjunction such that it is true iff both P and Q are true;
– P ∨ Q is disjunction such that it is false iff both P and Q are false.

exclusive(异或) denoted by P ⊕ Q. Specifically, P ⊕ Q is true iff the truth values of P and Q are different.

Let P and Q be two propositions. Then P → Q is a new compound proposition such that it is false iff P is true and Q is false.

Similarly, we can also define the bi-conditional statement (双条件) P ↔ Q such that it is true iff the truth values of P and Q are the same.

WFF: Well-formed formulas are defined inductively as follows:
1. Each propositional variable is a WFF.
2. If α is a formula, then ¬α is a WFF.
3. If α and β are WFFs, and • is any binary logical operator, then (α • β) is a WFF, where • could be (not limited to) the usual operators ¬, ∧, ∨, → or ↔.



**Method**:
draw truth table (真值表), to show how the truth value of the compound proposition changes
when the truth values of the atomic propositions in it change.

**Property**：