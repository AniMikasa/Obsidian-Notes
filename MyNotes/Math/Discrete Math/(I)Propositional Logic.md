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

WFF:
Well-formed formulas are defined inductively as follows:
1. Each propositional variable is a WFF.
2. If α is a formula, then ¬α is a WFF.
3. If α and β are WFFs, and • is any binary logical operator, then (α • β) is a WFF, where • could be (not limited to) the usual operators ¬, ∧, ∨, → or ↔.
Some Simplification Rules for WFFs：
-  Outside (·) can always be omitted, e.g., (P → Q) can be written as P → Q.
- (¬P) can be simplified as ¬P, e.g., (¬(Q∧(¬P))) can be written as ¬(Q → ¬P).
- We can always omit (·) by understanding the precedence (优先级) of the operators. Commonly, we assume precedence in following order ¬, ∧, ∨,→,↔ and for the same operator from left to right. 

The truth value of formula α is determined by the assignments (指派) of $P_1, . . . , P_n$.Specifically, a truth assignment is a vector $I = (v_1, . . . , v_n)$, where each $v_i$ is either 0 or 1.

Given a formula α and an assignment $I = (v_1, . . . , v_n)$, we write $I |= α$ if it is true under assignment $I$.

A compound proposition formula is said to be
	• a tautology (重言式) if it is true under any assignment;
	• satisfiable (可满足的) if it is true under some assignment;
	• a contradiction (矛盾式) if it is false under any assignment.

**Method**:
draw truth table (真值表), to show how the truth value of the compound proposition changes
when the truth values of the atomic propositions in it change.

**Properties**：


**Examples:**
Boolean satisfiability problem (SAT) is one of the most fundamentally important problems.This problem can be solve by testing all $2^n$ possible assignments. However, this is very inefficient (but avoidable because it has been proved to be NP-hard).