# AI Scientific Writing Assistant with Claim Verification

## Project Identity

| | |
|---|---|
| **Project Name** | AI Scientific Writing Assistant with Claim Verification |
| **Primary Domain** | Generative AI / Scientific NLP / Information Retrieval |
| **Primary Problem** | Connecting scientific claims in academic writing with reliable supporting and contradictory evidence |
| **Primary Users** | Researchers, Master's students, PhD students, academics, and scientific writers |
| **Primary Outcome** | Faster and more rigorous evidence-backed scientific writing |
| **Core Principle** | The AI assists scientific judgment; it does not replace it |

---

## 1. Project Overview

The project is an AI-powered scientific writing assistant designed to help researchers, students, academics, and scientists write more rigorous and well-supported academic documents.

The primary purpose of the system is to assist users in identifying scientific claims within their writing and determining whether those claims are adequately supported by existing scientific literature.

Rather than functioning as a conventional chatbot or a simple academic search engine, the system should operate as an intelligent research companion that works directly with the user's scientific writing.

As a researcher writes a manuscript, thesis, dissertation, research proposal, or scientific report, the system should be able to recognize statements that represent scientific claims, identify the evidence required to support those claims, search relevant scientific literature, retrieve potentially supporting or contradicting publications, and present the evidence to the user in an understandable and actionable manner.

The ultimate objective is to reduce the amount of manual effort required to find and verify scientific references while improving the quality, traceability, and reliability of citations in academic writing.

## 2. Core Problem

Scientific writing requires authors to support important claims with appropriate evidence.

For example, a researcher may write:

> "Auditory attention decoding performance is most prominent between 150 and 200 ms time lags."

The researcher then needs to determine:

- Whether this is actually supported by existing research
- Which publications report similar findings
- Whether the cited studies directly support the statement
- Whether there are studies that report different findings
- How strong the available evidence is
- Whether the statement should be modified
- Which references should be cited

Currently, this process is largely manual. Researchers typically have to:

1. Identify claims themselves.
2. Search multiple scientific databases.
3. Open and read numerous papers.
4. Determine whether individual papers actually support their statement.
5. Compare contradictory findings.
6. Manually create citations.
7. Manually maintain their reference list.

This process becomes particularly difficult when writing a thesis or dissertation containing hundreds or thousands of scientific statements.

The project aims to make this process substantially more intelligent and efficient.

## 3. Vision

The long-term vision is to create an intelligent scientific writing environment where evidence discovery and claim verification happen naturally as the researcher writes.

The system should feel less like:

> "Ask an AI a question."

and more like:

> "Write normally, while an AI research assistant continuously checks whether your scientific statements are properly supported."

The system should assist rather than replace the researcher. The researcher remains responsible for interpreting the literature and making the final scientific decision.

## 4. Target Users

The primary users are:

- **Researchers** — Scientists writing research papers, technical reports, and publications.
- **Master's and PhD Students** — Students writing theses, dissertations, literature reviews, and research proposals.
- **Academic Supervisors** — Supervisors who want to review whether important claims in student work are adequately supported.
- **Scientific Writers** — Professionals producing technical and scientific documents.
- **Research Organizations** — Organizations that need systematic evidence discovery and citation support.

## 5. Primary Goal

The primary goal is:

> **To automatically identify scientific claims in academic writing and help the author discover, evaluate, and cite relevant scientific evidence supporting or contradicting those claims.**

The system should ultimately answer the question:

> "Is this scientific statement adequately supported by the existing literature, and what evidence should I examine?"

## 6. Core User Experience

A researcher should be able to write something such as:

> "Deep learning methods have substantially improved auditory attention decoding performance compared with traditional signal-processing approaches."

The system should recognize that this is a potentially verifiable scientific claim. It should then provide the researcher with relevant scientific evidence.

For example, the system could present:

**Claim**
> "Deep learning methods have substantially improved auditory attention decoding performance compared with traditional signal-processing approaches."

**Evidence Found**
Several potentially relevant publications.

For each publication, the researcher should be able to see:

- Title
- Authors
- Publication year
- Journal/conference
- DOI or other identifier
- Abstract
- Relevant evidence
- Relationship to the claim
- Strength of support
- Potential limitations

The researcher should then be able to investigate the source and decide whether it genuinely supports the statement.

## 7. Claim Identification

The system should distinguish between ordinary text and statements that make scientifically meaningful claims.

**Ordinary statement**
> "In this chapter, we discuss auditory attention decoding."

This does not necessarily require evidence.

**Scientific claim**
> "Auditory attention decoding performance improves significantly when neural responses are analyzed using deep learning methods."

This represents a claim that could require supporting evidence.

The system should therefore identify potentially evidence-requiring statements without unnecessarily flagging every sentence.

Claims may include:

- Quantitative claims
- Causal claims
- Comparative claims
- General scientific statements
- Statements about established findings
- Statements about relationships between variables
- Statements describing experimental findings
- Statements describing the effectiveness of a method
- Statements about physiological or biological mechanisms

## 8. Scientific Evidence Discovery

For each identified claim, the system should help locate relevant scientific publications.

The desired literature coverage includes scientific sources such as:

- Peer-reviewed journal articles
- Conference papers
- Preprints
- Academic books or chapters where appropriate
- Systematic reviews
- Meta-analyses
- Technical publications

The system should prioritize sources that are genuinely relevant to the claim rather than merely matching keywords.

For example, a paper containing the words "auditory attention" should not automatically be considered evidence for a claim about a specific temporal latency. The relevance of the actual scientific finding is more important than superficial keyword similarity.

## 9. Evidence Classification

A major goal of the project is to distinguish between different relationships between a claim and a scientific publication. A retrieved paper could be:

| Category | Description |
|---|---|
| **Strongly Supporting** | The paper provides direct evidence consistent with the claim. |
| **Partially Supporting** | The paper supports part of the claim but does not establish the entire statement. |
| **Weakly Supporting** | The paper is related to the claim but provides limited or indirect evidence. |
| **Contradicting** | The paper reports findings inconsistent with the claim. |
| **Related but Insufficient** | The paper discusses a closely related topic but does not provide adequate evidence for the specific claim. |
| **Not Relevant** | The publication does not meaningfully support the statement. |

This distinction is one of the most important aspects of the project.

## 10. Evidence Strength

The system should communicate the strength of available evidence.

For example:

**Claim:**
> "Method X significantly improves prediction accuracy."

The system might indicate that:

- 8 publications support the claim.
- 2 publications report mixed results.
- 1 publication reports contradictory findings.

The system should therefore avoid presenting scientific evidence as simply "true" or "false." Instead, it should communicate the available evidence and its limitations.

The goal is to support scientific judgment rather than replace it.

## 11. Contradictory Evidence

An especially important feature is identifying disagreement in the scientific literature.

For example, if a researcher writes:

> "Method X consistently outperforms Method Y."

The system should not simply find papers supporting this statement. It should also attempt to identify relevant studies reporting:

- Similar results
- Different results
- No significant difference
- Results dependent on experimental conditions
- Limitations or exceptions

The system should alert the researcher when the literature does not provide a clear consensus.

This feature is critical because a system that only finds supporting evidence could encourage confirmation bias.

## 12. Citation Recommendation

The system should recommend publications that may be appropriate citations for individual claims.

For each claim, the researcher should be able to see a ranked set of candidate references. The user should be able to select an appropriate source and add it to their document.

The system should support standard academic citation information, including:

- Authors
- Title
- Year
- Journal/conference
- DOI
- URL where applicable
- Bibliographic information
- BibTeX representation

The objective is to reduce repetitive reference-management work.

## 13. Evidence Highlighting

One of the most important features is the ability to show the researcher why a publication was recommended.

Instead of merely displaying:

> "This paper is relevant."

the system should identify the relevant evidence from the publication.

For example:

- **User's claim:** "Auditory attention decoding performance is strongest between 150 and 200 ms."
- **Retrieved paper:** Smith et al., 2024
- **Relevant evidence:** The system identifies the portion of the publication discussing the relevant temporal response and presents it to the researcher.

This creates a traceable connection:

```
Claim → Publication → Evidence
```

The researcher should be able to inspect the source rather than blindly trusting an AI-generated conclusion.

## 14. Claim-Level Verification

The system should allow a researcher to evaluate an entire document rather than only individual sentences.

For example, after analyzing a thesis chapter, the system could produce a report such as:

| Metric | Count |
|---|---:|
| Claims analyzed | 127 |
| Claims with strong supporting evidence | 72 |
| Claims with partial evidence | 24 |
| Claims with weak evidence | 18 |
| Claims with contradictory evidence | 7 |
| Claims with no relevant evidence found | 6 |

This would give the researcher a high-level overview of the evidential quality of their writing.

## 15. Document-Level Analysis

The system should eventually be able to analyze:

- Individual sentences
- Paragraphs
- Sections
- Entire manuscripts
- Thesis chapters
- Full theses

It should identify which claims already have citations and which potentially require additional evidence.

It should also help identify inconsistencies. For example, a researcher may make one statement in Chapter 2 and a contradictory statement in Chapter 5. The system could flag this as something worth reviewing.

## 16. Scientific Writing Assistance

Although the primary purpose is evidence discovery and verification, the system can provide additional writing assistance related specifically to scientific claims.

For example, if a statement is too strong relative to the available evidence, the system could suggest that the researcher consider more cautious wording.

- **Original:** "Deep learning always provides better performance than traditional methods."
- **Potential concern:** The available literature does not support such a broad statement.
- **Suggested direction:** The system could indicate that the researcher may want to qualify the statement.

The researcher should remain in control of the final wording.

## 17. Important Principle: Evidence, Not Hallucination

A central objective of the project is to minimize unsupported AI-generated information.

The system should never treat an AI-generated explanation as scientific evidence. Scientific evidence must ultimately trace back to identifiable literature. Every recommendation should therefore have a traceable source.

The researcher should be able to move from:

```
Claim
  → Recommended publication
    → Relevant evidence
      → Original publication
```

This traceability is fundamental to the project.

## 18. Research Transparency

The system should communicate uncertainty.

It should be possible for the system to say:

> "No sufficiently strong supporting evidence was identified."

rather than generating a plausible-looking citation.

Similarly, it should distinguish:

> "No evidence was found"

from:

> "Evidence exists showing the claim is false."

These are scientifically very different conclusions.

## 19. Example End-to-End Scenario

A PhD student is writing a thesis on auditory attention decoding. They write:

> "Temporal response function models generally achieve their strongest decoding performance around 150–200 ms after stimulus onset."

The system recognizes the statement as a scientific claim. It then identifies relevant literature. The researcher sees:

| Candidate | Paper | Finding | Assessment |
|---|---|---|---|
| 1 | Paper A — 2022 | Evidence appears highly consistent with the claim. | **Support: Strong** |
| 2 | Paper B — 2023 | Reports a similar temporal range but under different experimental conditions. | **Support: Partial** |
| 3 | Paper C — 2024 | Reports peak performance outside the proposed range. | **Relationship: Contradictory / context-dependent** |

The system informs the researcher that the literature is not necessarily uniform.

The researcher can inspect the papers, decide how the claim should be written, and add appropriate citations.

This is the intended experience.

## 20. Long-Term Vision

The long-term goal is to evolve the system from a citation recommendation tool into a broader scientific evidence intelligence platform.

Potential future capabilities could include:

- Literature review assistance
- Research gap identification
- Evidence mapping
- Contradiction detection
- Citation network analysis
- Research trend analysis
- Automatic literature review organization
- Hypothesis-to-evidence exploration
- Research question refinement
- Scientific figure and table interpretation
- Evidence-based manuscript review

These are future possibilities rather than requirements for the initial version.

## 21. Minimum Viable Product

The initial version should focus on one clearly defined workflow:

```
Scientific sentence → Claim identification → Relevant literature → Evidence → Support assessment → Citation
```

The MVP should be capable of:

1. Accepting scientific text from a researcher.
2. Identifying potentially meaningful scientific claims.
3. Finding relevant scientific publications.
4. Presenting the most relevant publications.
5. Showing evidence from those publications.
6. Indicating how strongly each publication supports or contradicts the claim.
7. Providing bibliographic information.
8. Allowing the researcher to select a citation.
9. Making the relationship between the claim and evidence transparent.

The MVP does not need to solve every aspect of scientific writing.

## 22. Success Criteria

The project should ultimately be evaluated according to measurable outcomes.

| Criterion | Evaluation Question |
|---|---|
| **Claim Detection** | Can the system correctly identify statements that require scientific evidence? |
| **Literature Retrieval** | Can it retrieve genuinely relevant scientific publications? |
| **Evidence Retrieval** | Can it identify the parts of publications that actually relate to the claim? |
| **Claim-Evidence Matching** | Can it distinguish supporting, contradictory, partial, and irrelevant evidence? |
| **Citation Recommendation** | Are the recommended citations appropriate? |
| **False Evidence Prevention** | Does the system avoid presenting unrelated publications as supporting evidence? |
| **Usability** | Can a researcher use the system without disrupting their normal writing workflow? |
| **Time Savings** | Does the system reduce the time required to find and verify references? |

## 23. What Makes This Project Different

This project should **NOT** be positioned as:

> "A chatbot that answers questions about research papers."

It should be positioned as:

> **An evidence-grounded scientific writing assistant that connects claims in academic writing to supporting and contradictory scientific literature.**

The central object is not the conversation. The central object is the relationship:

```
Scientific Claim ↔ Scientific Evidence
```

That distinction is important.

## 24. Intended Final Product

The final product should feel like an intelligent research-writing environment.

A researcher writes normally. The system works alongside them and provides unobtrusive evidence assistance.

The researcher can:

- See which claims may need citations.
- Discover relevant papers.
- Inspect supporting evidence.
- Identify contradictory literature.
- Assess evidence strength.
- Insert citations.
- Generate references.
- Review the evidential quality of an entire document.

The researcher always makes the final scientific decision.

## 25. Overall Project Goal

The ultimate goal is to build a reliable AI research assistant that helps transform scientific writing from a largely manual workflow:

```
Write → Search → Read → Verify → Cite
```

into an assisted workflow:

```
Write → Identify Claim → Discover Evidence → Evaluate Evidence → Cite
```

The project should demonstrate that AI can be used not merely to generate academic text, but to improve the evidence, traceability, and rigor behind scientific writing.
