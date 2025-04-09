CAV 2025 Paper #83 Reviews and Comments
===========================================================================
Paper #83 D-Hammer: Efficient Equational Reasoning for Labelled Dirac
Notation


Review #83A
===========================================================================
* Updated: Mar 18, 2025

Overall merit
-------------
4. Accept

Reviewer expertise
------------------
3. Knowledgeable

Paper summary
-------------
Summary:
The paper introduces D-Hammer, a tool that enables equivalence checking of Dirac notation, including, for the first time, labeled Dirac notation. Their approach offers a complete normalization algorithm that avoids checking permutations of axioms that cannot be handled by simple term rewriting. This claim is supported by experimental evidence. A tool implementation is applied to examples of both plain and labeled notation and the results compare favorably with DiracDec (Xu et al., POPL’25).

From my perspective, the most interesting difference with DiacDec lies in how the D-Hammer language handles AC symbols. The main challenge, as already noted in Xu et al.'s paper, is that not all axioms in Dirac notation can be normalized through term rewriting alone. Both approaches address this by separating axioms into two sets: those that can be normalized by term rewriting (R) and those that cannot (E), with the latter primarily containing AC symbols.

In Xu et al.'s work, normalization is applied to R on both sides of the equality, after which equivalence is checked within E. This paper, on the other hand, takes a seemingly better approach. Instead of introducing auxiliary symbols with variable arities to handle E, the authors design a language that inherently supports AC symbols with indefinite arities. This allows them to apply specialized algorithms to normalize E directly, effectively normalizing both R and E together.

Comments for authors
--------------------
Strengths of the paper:
- The paper identifies the limitations of existing equivalence-checking tools designed for plain Dirac notation and presents a well-defined solution. This problem is effectively motivated in the introduction. Furthermore, since the most relevant expressions and equalities in quantum mechanics are often formulated in labeled Dirac notation, developing automated tools capable of handling this notation represents an essential advancement for the research community.
- The complete normalization approach.
- Another key strength of the proposed tool is its ability to overcome other practical limitations of DiracDec. Specifically, one of the biggest strengths of D-Hammer lies in its relationship with Mathematica. Since DiracDec is implemented within Mathematica, its behavior and typing rules are inherently constrained by Mathematica’s lack of a strong type system. In contrast, D-Hammer is programmed in C++, and is designed to benefit from a more structured term representation (e.g., AC operators with indefinite arity), a more expressive type system (e.g., dependent types), and a more efficient rewriting engine. While D-Hammer still interacts with Mathematica for reasoning about functions not natively supported by its rewriting rules, these interactions are carefully managed and do not negatively impact overall efficiency. This advantage is reflected in the experimental results, where D-Hammer consistently outperforms DiracDec in equivalence-checking tasks.

Weak points:
- It remains unclear whether the better results stem from a fundamental improvement in the algorithm (the complete normalization) or simply from a more efficient implementation—since the tool more often avoids using Mathematica as a backend. 
- Regarding the experimental evaluation, the results strongly suggest that D-Hammer outperforms DiracDec. However, it would have been more compelling to see a comparison using all 1031 lemmas from (Xu et al. POPL’25) to better assess how D-Hammer performs in a broader range of cases. For the evaluation of D-Hammer’s performance on labeled notation, solving the LDN-10 instance is impressive, but only 18 examples seem to fall short of asserting the tool’s performance.


Questions:
- I could not sufficiently check some of the key claims. For instance, Theorem 1 is stated without formal proof, whereas an equivalent theorem in Xu et al. (POPL’25) is given nearly a full page of justification in Section 7.2. This omission might be understandable if the authors use the same algorithm. However, since they claim their approach differs, it seems crucial that the paper at least discusses confluence and termination in more detail (maybe because they use special algorithms for set E, is it unnecessary?). Please clarify.
- A similar issue arises with Theorem 2, which asserts the soundness and completeness of the normalization procedure. Proving completeness is generally challenging; in fact, Xu et al. (POPL’25) explicitly state in Section 7.2 that they were unable to prove completeness and instead conjectured it. Furthermore, establishing completeness typically requires demonstrating confluence and termination, yet the paper does not address these concepts. Additionally, can the authors explain how they ensure that none of the expressions in Appendix D introduce circular dependencies?
- Did the authors consider using SMT solvers with non-linear arithmetic and trigonometric expressions instead of Mathematica?


Opinion:
I really enjoyed reading the paper. It is well-written and well-motivated, and the numerical results seem quite strong. I genuinely believe the work is valuable and could be even more useful than the plain Dirac notation case, particularly for applications in quantum communication and many-body physics. 
What would also be interesting is extending these methods to second quantization rules, where the algebra becomes even more tedious and complex. A development in that direction would represent a fundamental advancement.
As for the fit to CAV, I believe that the community automated reasoning methods can have an impact in the field of physics. For instance, the community of tensor networks is enormous and applies techniques very similar to those we use in decision diagrams. ZX calculus and model counting are also rapidly gaining popularity for these applications. We have but touched the surface of the possibilities here. Therefore, the paper fits at CAV in my opinion.

Post-rebuttal:
The rebuttal convinces me and resolves my concerns.

Minor comments
Small examples might help when introducing the syntax. For instance, how is the set product used for index sets?
p5 weak dependent —> weakly dependent
p9: alwasy
p17: quantum circuit —> quantum circuits
p36: step that discard  (what is the difference between 1 and 2 here?)
p37: Besids,
For future work, perhaps: The text primarily focuses on labeled Dirac notation due to its relevance in many-body physics. The second quantization formalism is also of significant interest. Since working with second quantization rules can be highly complex and prone to errors, especially due to the anti-commutation relations of fermionic operators, it would be interesting to explore whether your tool could be extended to incorporate second quantization. Could D-Hammer be extended to support this formalism?



Review #83B
===========================================================================

Overall merit
-------------
4. Accept

Reviewer expertise
------------------
3. Knowledgeable

Paper summary
-------------
The paper introduces D-Hammer, a tool for automated equational proof for labelled Dirac notation. It uses a dependently typed language to formalize labelled Dirac notation and defines an equational theory which is proved to be sound wrt a denotational semantics. The authors also propose a normalization procedure to prove equivalence of expressions. The tool is implemented in C++ and shown to have good performance.

Comments for authors
--------------------
The paper is well motivated. Since labelled Dirac notation is widely used to represent many-body quantum systems, equational reasoning of it is undoubtedly important. So far the reasoning has been done manually. The paper rigorously defines the syntax and semantics of a language for labelled Dirac notation as well as a tool for automated equational reasoning. D-Hammer is the counterpart of DiracDec for plain Dirac notation, but is more efficient as it is implemented in C++ while the latter is implemented in Mathematica. The basic idea of D-Hammer is similar to that of DiracDec, but there are notable differences because of the use of dependent types and the new implementation. 

Theorem 2 states the soundness and completeness of normalization. As to the equational theory, only soundness is stated (Theorem 1). It would be interesting to discuss the completeness as well.

There are a lot of axioms and rewriting rules used by D-Hammer. How many of them are valid for plain Dirac notation (already used by DiracDec), and how many are newly added in this work?

Minor comments:
-	P.3, “as r,q consistent” -> “as r and 1 are consistent”
-	P.6, “compares where” -> “compares whether”
-	In Section 3.1, the syntax for types and terms are abstract. It would be better to give some concrete examples.



Review #83C
===========================================================================

Overall merit
-------------
2. Weak reject

Reviewer expertise
------------------
2. Some familiarity

Paper summary
-------------
This paper introduces D-Hammer, a tool for automating equational proofs in labelled Dirac notation, a formalism widely used in quantum computing and quantum programming. D-Hammer features a higher-order, dependently-typed language, an efficient normalization algorithm, and an optimized C++ implementation. The tool is evaluated on representative examples from both plain and labelled Dirac notation, demonstrating significant performance improvements over DiracDec (Xu et al., POPL’25) in the plain Dirac setting.

Comments for authors
--------------------
### Pros

+ The first tool to support automated equational proofs for labelled Dirac notation.

+ Achieves significantly faster equivalence verification compared to DiracDec.

### Cons

- The positioning of this work within the broader literature on equivalence reasoning in quantum computing is unclear. Please see my detailed questions below.

- It is not evident why existing equality saturation techniques cannot be applied to Dirac notation, provided that equivalence-preserving rewriting rules are well-defined.

### Detailed Comments

Thank you for the submission! While I am not an expert in quantum computing, I found this paper easy to follow, and the descriptions of the syntax and semantics of labelled Dirac notation are quite clear. However, I have two main technical concerns regarding this work, which I outline below:

- **Equivalence Checking in Quantum Computing**:  
  The central focus of this paper is equational reasoning for labelled Dirac notation, which seems closely related to a broader class of problems—*equivalence checking in quantum computing*. A well-established approach in this area is **ZX-calculus**, where any quantum circuit can be represented as a **ZX-diagram**. In this formalism, quantum gates such as **S, T, H, and CNOT** are represented as nodes in a graph, and a set of rewriting rules enables transformations and simplifications of these diagrams. Two circuits are considered equivalent if their ZX-diagrams can be reduced to the same (isomorphic) form.

- The approach proposed in this paper appears to share strong similarities with ZX-calculus. While this work does not use a graph-based representation, it explicitly expands tensor product computations. Additionally, the equivalence-preserving rewriting rules introduced in **Section 3.4** seem analogous to the rewriting rules used in ZX-diagrams. This raises the question: **What is the relationship between the techniques presented in this paper and ZX-calculus?** 

- Given that ZX-calculus is also widely used for verifying quantum circuit equivalence, it would be valuable to compare D-Hammer against ZX-calculus-based methods. A direct comparison with ZX-calculus would make the evaluation more compelling. If the authors could clarify this connection and possibly include a comparison, I would reconsider my overall score. 

  A relevant reference on ZX-calculus:  
  van de Wetering, John. *"ZX-calculus for the working quantum computer scientist."* arXiv preprint arXiv:2012.13966 (2020).

- **Technical Novelty and Equality Saturation**:  
  Another concern is the level of technical novelty in this work. The techniques presented here seem to apply equality saturation to a new domain—labelled Dirac notation. Given this, I wonder whether existing equality saturation techniques (e.g., egg [1]) could be directly utilized for reasoning in labelled Dirac notation. If not, it would be insightful for the authors to highlight the domain-specific challenges that prevent a direct application of egg or similar techniques.

  [1] Willsey, Max, Chandrakana Nandi, Yisu Remy Wang, Oliver Flatt, Zachary Tatlock, and Pavel Panchekha. *"Egg: Fast and extensible equality saturation."* Proceedings of the ACM on Programming Languages 5, no. POPL (2021): 1-29.

- **Figure 2 – Evaluation Clarifications**:  
  When comparing D-Hammer to DiracDec, the paper notes that D-Hammer omits certain rarely used features in CoqQ benchmarks, resulting in fewer verified cases. However, I am concerned that these omitted features might be the primary sources of performance overhead in DiracDec. If so, the performance improvement of D-Hammer may be due in part to excluding these computationally expensive cases rather than an inherent efficiency advantage. Could the authors provide more clarification on this point?



Response by Yingte Xu <yingte.xu@mpi-sp.org> (Author) (670 words)
---------------------------------------------------------------------------
We sincerely thank all reviewers for their comments, questions and innovative suggestions.

## Benchmark Evaluation and Clarification (A, C)
We will revise the paper to clarify the comparison with DiracDec. In particular, we will emphasize that:
- the enhanced efficiency primarily results from algorithmic improvements rather than the shift to a C++ implementation. This is supported by the fact that efficiency gains are more significant for larger examples, as can be observed in Figure 2.
- the omission of the projection rules has a limited influence on the performance evaluation. Only 4 examples involving projections are omitted, and the remaining cases are not affected by projection rules, so removing the projection feature does not influence too much.

Our paper presents a representative selection of labelled Dirac notation examples that can be handled by our tool. We will add further examples (~50) from [14,20,21] and possible applications in physics that can be handled by our tool—and they will be distributed with the tool.

## Comparison with ZX Calculus (C)
Labelled Dirac Notation (LDN) and ZX calculus have different scopes: the ZX calculus mainly focuses on quantum circuits, whereas LDN aims to provide general support for quantum physics. This difference in scope is reflected in the underlying languages: for instance,  many works in ZX calculus consider concrete sets of quantum gates, whereas LDN emphasizes the use of big operators. At a technical level, both the ZX calculus and LDN share the use of rewriting techniques. However, it is difficult to compare ZX and LDN: from a theoretical perspective, they pose different challenges. From a practical perspective, they target different examples, as observed in (Xu et al, POPL 2025). We will expand the related work section to include a longer comparison with ZX examples

## Comparison with Equality Saturation (C)
Our algorithm is based on term rewriting, and does not use equality saturation. We believe that equality saturation could be used to prove Dirac notation equations, but are not convinced that it would yield better results. Equality saturation has the advantage when it is hard to rewrite terms to a normal form. This may occur when we consider further extensions of labelled Dirac notation.

## Proof of Theorem 1 and 2 (A)
We provide the detailed proof of Theorem 1 and 2 in the **attached file** (rebuttal.pdf).

For Theorem 1 and soundness proof in Theorem 2, it suffices to prove that each rule is sound with respect to the axiomatic definition. The proof is routine.
The completeness proof for normalization in Theorem 2 is a relative completeness w.r.t. to the completeness of plain Dirac notation, and thus it is weaker and also easier to prove the confluence and termination of the whole system. The proof involves checking that step (1) - (3) can succeed rewriting every labelled Dirac formula to equation (4).

## Use SMT Solvers for Scalars (A)
Deciding the equivalence of scalars is not our main focus so we didn’t spend time exploring other tools. Also, SMT solvers have limited support for functions like sqrt(x) and exp(x) compared to Mathematica. But it is possible to try out with SMT solvers in the future.

## Rewriting Rules and Termination (A, B)
D-Hammer shares a common foundation for plain Dirac notation with DiracDec. There are 172 rules in DiracDec in total. We merged analogous rules by typing, removed 13 rules related to projections, and obtained 147 rules in D-Hammer. Part of the new procedure in D-Hammer are expressed as 8 rewriting rules. But it’s hard to quantify the difference as numbers of rewriting rules, because we use abstract rules and other algorithms.
The abstract and heterogeneous procedure also pose difficulty in proving confluence and termination. But we conjecture that termination still holds following the result from DiracDec, and no circular rewritings will happen.

## Applications in Physics (A)
We agree that exploring applications in physics is promising. Indeed, D-Hammer already possesses the capability to represent certain expressions in second quantization formalism. For instance, the creation operator in the occupation number representation is expressible as Sum[i, sqrt(i+1) |i+1><i|], enabling simplifications consistent with theoretical expectations.



Comment @A1 by Shepherd
---------------------------------------------------------------------------
 We thank the authors for their clear rebuttal, which proposes several improvements. The reviewers would like to see this materialize in the final version as follows: 
* An extended comparison with ZX calculus
* An empirical comparison with ZX calculus, for instance, for the circuit equivalence type queries found in Table 3.
