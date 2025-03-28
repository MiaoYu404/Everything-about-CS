## 1. Formal Languages

### Definition
- A **formal language** is a set of strings over a finite set of symbols called an **alphabet** (denoted $\Sigma$).

| Name         | 中文  | Definition                                                                      |
| ------------ | --- | ------------------------------------------------------------------------------- |
| **Alphabet** | 字母集 | A finite set of symbols (e.g., $\Sigma = \{0, 1\}$).                            |
| **String**   | 字符串 | A finite sequence of symbols from the alphabet (e.g., "$01$", "$110$").         |
| **Language** | 语言  | A set of strings over $\Sigma$ (e.g., all strings with an even number of $0$s). |
### Example
- Alphabet: $\Sigma = \{0, 1\}$
- Language: $L = \{\epsilon, 00, 11, 0000, 0011, ...\}$ (strings with an even number of $0$s)
  - Here, $\epsilon$ denotes the empty string.

---

## 2. Regular Languages 正则语言

### Definition
- A **regular language 正则语言** is a formal language that can be recognized by a **finite automaton 有穷自动机** (FA) or described by a **regular expression 正则表达式** (RE).
- Recognized 识别 by: Deterministic Finite Automata (DFAs), Nondeterministic Finite Automata (NFAs), or ε-NFAs.
- Examples:
  - L = {w ∈ {0, 1}* | w ends in 01}
  - L = {w ∈ {0, 1}* | w has at least one 0}

### Finite Automata 有穷自动机
#### Deterministic Finite Automata (DFA)
- **Components 形式化 DFA**: 
  - Q: Finite set of states
  - Σ: Input alphabet
  - δ: Transition function (δ: Q × Σ → Q)
  - q₀: Start state (q₀ ∈ Q)
  - F: Set of final states (F ⊆ Q)
- **Behavior**: For each state and input symbol, there is exactly one next state.
- **Language**: A string w is accepted if δ(q₀, w) ∈ F.

**Example DFA** (Strings ending in 01 over Σ = {0, 1}):
- Q = {q₀, q₁, q₂}
- δ:
  - δ(q₀, 0) = q₁, δ(q₀, 1) = q₀
  - δ(q₁, 0) = q₁, δ(q₁, 1) = q₂
  - δ(q₂, 0) = q₁, δ(q₂, 1) = q₀
- q₀ = start state, F = {q₂}
- Accepts: "01", "001", "101", etc.

**Proofs of Set Equivalence** we need to prove that $S\subseteq T$ and $T\subseteq S$.

**Example of a Regular Language 正则语言**
L3 = { w | w in {0,1}* and w, viewed as a binary integer is divisible by 23}
The DFA:
- 23 states, named 0, 1,…,22.
- Correspond to the 23 remainders of an integer divided by 23.
- Start and only final state is 0.
If string w represents integer i, then assume δ(0, w) = i%23. Then w0 represents integer 2i, so we want δ(i%23, 0) = (2i)%23.
Similarly: w1 represents 2i+1, so we want δ(i%23, 1) = (2i+1)%23.
- Example: δ(15,0) = 30%23 = 7; δ(11,1) = 23%23 = 0.

#### Nondeterministic Finite Automata (NFA)
- **Components**: Same as DFA, but δ: Q × Σ → 2^Q (set of possible states).
- **Behavior**: Multiple possible next states for a given state and input; accepts if *any* path leads to a final state.
- **Example NFA** (Strings ending in 01):
  - Q = {q₀, q₁, q₂}
  - δ(q₀, 0) = {q₀}, δ(q₀, 1) = {q₀, q₁}
  - δ(q₁, 0) = ∅, δ(q₁, 1) = {q₂}
  - δ(q₂, 0) = ∅, δ(q₂, 1) = ∅
  - q₀ = start, F = {q₂}

#### ε-NFA
- Allows transitions on ε (empty string) without consuming input.
- **Closure of States**: CL(q) = states reachable from q via ε-transitions.
- Converted to DFA via subset construction, including ε-closures.

#### Equivalence
- DFAs, NFAs, and ε-NFAs all recognize the same class of languages: regular languages.
- **Subset Construction**: Converts NFA to DFA by treating sets of NFA states as DFA states.

### Regular Expressions (REs)
- **Definition**:
  - Basis: a (symbol), ε (empty string), ∅ (empty set)
  - Induction: E₁ + E₂ (union), E₁E₂ (concatenation), E* (Kleene star)
- **Examples**:
  - (0+1)*1: All strings ending in 1
  - 0*10*: Strings with exactly one 1
- **Conversion**:
  - RE to ε-NFA: Construct via union, concatenation, and closure rules.
  - DFA to RE: Use k-path induction to build REs for paths between states.

### Pumping Lemma for Regular Languages
- **Statement**: For every regular language L, there exists an integer n such that for every string w ∈ L with |w| ≥ n, w can be written as w = xyz where:
  1. |xy| ≤ n
  2. |y| > 0
  3. For all i ≥ 0, xyⁱz ∈ L
- **Use**: Proves a language is not regular (e.g., {0ⁿ1ⁿ | n ≥ 0}).

**Example**: Show {0ⁿ1ⁿ | n ≥ 0} is not regular:
- Assume it is regular with pumping length n.
- Take w = 0ⁿ1ⁿ (|w| = 2n ≥ n).
- By the lemma, w = xyz, |xy| ≤ n, |y| > 0.
- Since |xy| ≤ n, xy is within the 0s, so y = 0ᵏ (k > 0).
- Pump with i = 2: xy²z = 0ⁿ⁺ᵏ1ⁿ, which has more 0s than 1s, not in L. Contradiction.

### Closure Properties
- Regular languages are closed under:
  - **Union**: L₁ ∪ L₂ (RE: R₁ + R₂)
  - **Concatenation**: L₁L₂ (RE: R₁R₂)
  - **Kleene Star**: L* (RE: R*)
  - **Intersection**: L₁ ∩ L₂ (via product DFA)
  - **Complement**: Σ* - L (swap final/non-final states in DFA)
  - **Difference**, **Reversal**, **Homomorphism**, **Inverse Homomorphism**

### Decision Properties
- Algorithms exist to decide:
  - **Membership**: Is w ∈ L? (Simulate DFA on w)
  - **Emptiness**: Is L = ∅? (Check if any final state is reachable)
  - **Infiniteness**: Is L infinite? (Check for cycles to final states)
  - **Equivalence**: Is L₁ = L₂? (Product DFA with final states for disagreement)
  - **Containment**: Is L₁ ⊆ L₂? (Product DFA for L₁ - L₂ emptiness)

---

## 3. Context-Free Languages (CFLs)

### Definition
- A **context-free language** is a formal language generated by a **context-free grammar** (CFG).
- More powerful than regular languages; can describe nested or balanced structures.
- Example: {0ⁿ1ⁿ | n ≥ 0} (not regular, but context-free).

### Context-Free Grammars (CFG)
- **Components**:
  - Terminals: Alphabet symbols (e.g., {0, 1})
  - Nonterminals: Variables (e.g., S, A, B)
  - Start Symbol: A designated nonterminal (e.g., S)
  - Productions: Rules of the form A → α (α is a string of terminals and/or nonterminals)
- **Example CFG** for {0ⁿ1ⁿ | n ≥ 0}:
  - S → ε | 0S1
  - Derivation: S ⇒ 0S1 ⇒ 00S11 ⇒ 000111

#### Derivations
- **General**: Replace any nonterminal using a production (e.g., S ⇒ 0S1 ⇒ 00S11).
- **Leftmost**: Always replace the leftmost nonterminal (e.g., S ⇒ₗₘ 0S1 ⇒ₗₘ 00S11 ⇒ₗₘ 000111).
- **Rightmost**: Always replace the rightmost nonterminal (e.g., S ⇒ᵣₘ 0S1 ⇒ᵣₘ 0ε1 ⇒ᵣₘ 01).

#### Parse Trees
- Graphical representation of a derivation.
- Root: Start symbol
- Internal nodes: Nonterminals
- Leaves: Terminals (yield = derived string)
- **Example**: For S → 0S1 | ε, parse tree for "0011" has yield 0011.

### Pumping Lemma for CFLs
- **Statement**: For every CFL L, there exists an integer n such that for every z ∈ L with |z| ≥ n, z = uvwxy where:
  1. |vwx| ≤ n
  2. |vx| > 0
  3. For all i ≥ 0, uvⁱwxⁱy ∈ L
- **Use**: Proves a language is not context-free (e.g., {0ⁿ1ⁿ2ⁿ | n ≥ 0}).

**Example**: Show {0ⁿ1ⁿ2ⁿ | n ≥ 0} is not context-free:
- Assume it is context-free with pumping length n.
- Take z = 0ⁿ1ⁿ2ⁿ (|z| = 3n ≥ n).
- By the lemma, z = uvwxy, |vwx| ≤ n, |vx| > 0.
- Since |vwx| ≤ n, vwx cannot span all three segments (0s, 1s, 2s).
- If vwx is in 0s and 1s, pump i = 0: uwy has fewer 0s or 1s than 2s, not in L. Contradiction.

### Normal Forms
- **Chomsky Normal Form (CNF)**:
  - Productions: A → BC or A → a (B, C are variables, a is a terminal).
  - Steps: Eliminate ε-productions, unit productions, useless symbols; convert to CNF.
- **Example**: Convert S → 0S1 | ε to CNF:
  - Cleaned: S → 0S1, S → A (A → 0, B → 1 added), then S → AC, C → SB.

### Closure Properties
- CFLs are closed under:
  - **Union**: S → S₁ | S₂
  - **Concatenation**: S → S₁S₂
  - **Kleene Star**: S → SS | ε
- Not closed under:
  - **Intersection**: {0ⁿ1ⁿ | n ≥ 0} ∩ {1ⁿ0ⁿ | n ≥ 0} = {0ⁿ1ⁿ0ⁿ | n ≥ 0} (not CFL)
  - **Complement**

### Decision Properties
- Decidable:
  - **Membership**: Using parsing algorithms (e.g., CYK for CNF).
  - **Emptiness**: Check if start symbol derives a terminal string.
- Undecidable:
  - **Equivalence**: No general algorithm exists.

---

## 4. Relationship Between Regular and Context-Free Languages
- **Inclusion**: Every regular language is context-free (CFGs can simulate FAs).
- **Expressive Power**: CFLs can describe languages with nested structures (e.g., {0ⁿ1ⁿ | n ≥ 0}) that regular languages cannot due to finite memory limitations.

---

## 5. Applications
- **Regular Languages**: Lexical analysis in compilers (tokenizing).
- **Context-Free Languages**: Syntax analysis in compilers (parsing).

---

## Practice Exercises
1. **DFA Design**: Create a DFA for strings over {0, 1} with an even number of 0s.
2. **CFG Design**: Write a CFG for balanced parentheses (e.g., (), (()), (()())).
3. **Pumping Lemma**: Prove {0ⁿ1ⁿ | n ≥ 0} is not regular using the pumping lemma.
4. **RE to NFA**: Convert (0+1)*0 to an ε-NFA.

---

This review note consolidates the essential topics from your COMP4011 slides, providing a complete and structured resource for your mid-term preparation. Update your GitHub repository with these notes and practice examples to reinforce your understanding!