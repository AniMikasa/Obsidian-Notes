## Definitions
In the context of predicate logic, we usually call a subject an Object/Individual (个体词语). For example, “Alice” or “Bob” are object constant (个体常元) and we usually use “x, y, z” to represent unspecified object variables(个体变元).

we call the set of all objects under consideration the Domain of Discourse (论域) and usually denote it by D.

Given a set of objects, essentially we want to determine whether their have some properties/relations. This is actually the role of Predicate (谓词), which is a mapping
$$
P : D_n→ \{0, 1\}
$$

Formally, in the context of predicate logic, a function is a mapping that maps a groups of objects to a new object
$$f : D_n → D$$

To quantify (量化) the object, we have two different Quantifiers (量词):
- Universal Quantifier (全称量词): “∀” read as “for all”, meaning that
	$(∀x)(P(x))$ is true, if and only if, $P(x)$ is true for any $x ∈ D$
- Existential Quantifier (存在量词): “∃” read as “there exists”, meaning that
	$(∃x)(P(x))$ is true, if and only if, $P(x)$ is true for some $x ∈ D$

Free Variables (自由变元), Bound Variables (约束变元), Scope (辖域)

A quantifier can only bound free variables in its scope.

An assignment for a predicate logic formula contains the followings
1. specific domain of discourse D
2. for each object variables, assign a specific object in D
3. for each predicate variables, assign a specific predicate in {P | P : Dn→ {0, 1}}.
4. for each propositional variables, assign a specific truth value in {0, 1}.
5. for each function variables, assign a specific function in {f | f : Dn→D}.

Similar to the case of propositional logic, we say a predicate logic formula is
	• universally valid (普遍有效的) if it is true under any assignment;
	• satisfiable (可满足的) if it is true under some assignment;
	• a unsatisfiable (不可满足的) if it is false under any assignment.

Let α and β be two predicate formulas. We say α and β are equivalent, denoted by α = β or α ⇔ β if the truth values of α and β are the same under any assignments.According to the definition, we also have α = β iff α ↔ β is universally valid.

A Skolem normal form is a PNF without existential quantifiers, i.e., α =

(∀x1)(∀x2)· · ·(∀xn)M (x1, x2, · · · , xn).
## Methods
Formalize Natural Sentences using Predicate Logic Formula:
"All rational numbers are real numbers" can be written as: (∀x)(Rational(x)→Real(x))
“Some real number is a rational number.” can be written as: (∃x)(Real(x) ∧ Rational(x))
“Any number has a unique predecessor, except ‘0’.” can be written as: (∀x) (¬Eq(x, 0)→(∃y) (Eq(y, pre(x)) ∧ (∀z) (Eq(z, pre(x))→Eq(y, z))))

Inference in Predicate Logic:
we summarize some Basic Inference Rules.
	$(∀x)P(x) ∨ (∀x)Q(x) ⇒ (∀x)(P(x) ∨ Q(x))$
	$(∃x)(P(x) ∧ Q(x)) ⇒ (∃x)P(x) ∧ (∃x)Q(x)$
	$(∀x)(P(x)→Q(x)) ⇒ (∀x)P(x)→(∀x)Q(x)$
	$(∀x)(P(x)→Q(x)) ⇒ (∃x)P(x)→(∃x)Q(x)$
	$(∀x)(P(x) ↔ Q(x)) ⇒ (∀x)P(x) ↔ (∀x)Q(x)$
	$(∀x)(P(x) ↔ Q(x)) ⇒ (∃x)P(x) ↔ (∃x)Q(x)$
	$(∀x)(P(x)→Q(x)) ∧ (∀x)(Q(x)→R(x)) ⇒ (∀x)(P(x)→R(x))$
	$(∀x)(P(x)→Q(x)) ∧ P(a) ⇒ Q(a))$
	$(∀x)(∀y)P(x, y) ⇒ (∃x)(∀y)P(x, y)$
	$(∃x)(∀y)P(x, y) ⇒ (∀y)(∃x)P(x, y)$

for any predicate formula α, we may not find a Skolem normal form that is equivalent to α. However, we can always transfer it to a Skolem normal form α ′ such that α is satisfiable iff α′ is satisfiable.

Obtain Skolem Normal Form:
1. Convert α to PNF: $(Q_ix_1)· · ·(Q_nx_n)M (x_1, · · · , x_n)$.
2. For each “$(∃x_i)$”, remove the quantifier, and replace variable xi in $M(x_1, . . . , x_n)$ by a function of those variables bound by universal quantifiers in front of $(∃x_i)$.For example, for PNF $α = (∀x)(∃y)P(x, y)$, its Skolem normal form is $α ′ =(∀x)P(x, f(x))$; for PNF $(∃x)(∀y)P(x, y)$, its Skolem normal form is $(∀y)P(a, y)$; And for $(∃x)(∀y)(∀z)(∃u)(∀v)(∃w)P(x, y, z, u, v, w)$, is $(∀y)(∀)z(∀v)P(a, y, z, f(y, z), v, g(y, z, v))$

Resolution Method for Predicate Logic:
Step 1: Write the Skolem Normal Form for α∧¬β, e.g., G∗ = (∀x1). . .(∀xn)M(·).
Step 2: Write matrix M in CNF, e.g., M = C1 ∧ C2 ∧ . . . ∧ Cn.
	– note that in clause set S = {C1, C2, . . . , Cn}, each variable is bound by ∀.
Step 3: Resolve for S as follows:
	– a substitution (置换) $σ$ = {x/a} is an operator that replaces each symbol x in a formula by a new symbol a.
		For $L=P(x) → Q(x, y)$ and $σ$={x/a}, we have $Lσ=P(a) → Q(a, y)$.
	– for $C_1 = L_1 ∨ C_1^′$ and $C_2 = ¬L_2 ∨ C_2^′$ . If exists a substitution $σ$, such that $L_1σ = L_2σ$, then $C_1$and $C_2$ are resolved by $C_1^′σ ∨ C_2^′σ$.
		For $C_1 = P(x) ∨ Q(x)$ and $C_2 = ¬P(a) ∨ R(y)$, by taking σ = {x/a},we have $P(x)σ = P(a)$and P(a)σ = P(a). This gives us a new clause $Q(x)σ ∨ R(y)σ = Q(a) ∨ R(y)$.
Step 4: Repeat until we find a contradiction.




## Properties and Theorems
Change of Quantifiers:
1. (∀x)α(x) = ¬(∃x)¬α(x) or ¬(∀x)α(x) = (∃x)¬α(x);
2. (∃x)α(x) = ¬(∀x)¬α(x) or ¬(∃x)α(x) = (∀x)¬α(x).

Distributive Laws for Predicate Formulas:
1. $(∀x)(α(x) ∨ β) = (∀x)α(x) ∨ β$
2. $(∃x)(α(x) ∨ β) = (∃x)α(x) ∨ β$
3. $(∀x)(α(x) ∧ β) = (∀x)α(x) ∧ β$
4. $(∃x)(α(x) ∧ β) = (∃x)α(x) ∧ β$
Note that, quantifier is changed in 5 and 6 because negate prefix
5. $(∀x)(α(x)→β) = (∃x)α(x)→β$
6. $(∃x)(α(x)→β) = (∀x)α(x)→β$
7. $(∀x)(β →α(x)) = β →(∀x)α(x)$
8. $(∃x)(β →α(x)) = β →(∃x)α(x)$
9. $(∀x)(α(x) ∧ β(x)) = (∀x)α(x) ∧ (∀x)β(x)$
10. $(∃x)(α(x) ∨ β(x)) = (∃x)α(x) ∨ (∃x)β(x)$
11. $(∃x)(α(x)→β(x)) = (∀x)α(x)→(∃x)β(x)$


Variable Rename:
1. $(∀x)α(x) = (∀y)α(y) and (∃x)α(x) = (∃y)α(y)$
2. $(∀x)(∀y)(α(x) ∧ β(y)) = (∀x)(α(x) ∧ β(x))$
3. $(∃x)(∃y)(α(x) ∨ β(y)) = (∃x)(α(x) ∨ β(x))$


$(∃x)(∀y)P(x, y) ⇒ (∀y)(∃x)P(x, y)$

## Examples