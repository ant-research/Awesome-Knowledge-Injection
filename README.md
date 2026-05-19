# Awesome Knowledge Injection

<p align="center">
  <strong>English</strong> | <a href="./README.zh-CN.md">简体中文</a>
</p>

<p align="center">
  <a href="https://awesome.re"><img src="https://awesome.re/badge.svg" alt="Awesome"></a>
  <img src="https://img.shields.io/badge/Papers-50%2B-0969da" alt="papers">
  <img src="https://img.shields.io/badge/GitHub%20Projects-7%2B-2da44e" alt="github projects">
  <img src="https://img.shields.io/badge/Benchmarks-8%2B-f57c00" alt="benchmarks">
  <img src="https://img.shields.io/badge/Industry%20Systems-8%2B-8250df" alt="industry systems">
  <img src="https://img.shields.io/badge/Updated-2026--05--19-red" alt="updated">
</p>

<p align="center">
  <img src="./poster_en.png" alt="Awesome Knowledge Injection overview" width="100%">
</p>

> A curated map centered on VLM / MLLM knowledge injection with broad-capability retention, plus related inspirations such as knowledge editing and public industry routes.

<p align="center"><strong>Agent-assisted, human-curated, periodically refreshed.</strong></p>

<a id="task-introduction"></a>

## 🧩 Task Introduction

This awesome targets the following business / research problem: after a VLM / MLLM already has strong general capability, how can we inject new domain knowledge, factual knowledge, task knowledge, or multimodal knowledge while preserving the model's original broad capability as much as possible?

Typical scenarios include:

- The model needs to absorb evolving knowledge such as news events, enterprise knowledge bases, vertical-domain knowledge, and multimodal facts.
- The model should keep its original visual understanding, QA, OCR, reasoning, instruction-following, and safety behavior after knowledge updates.
- Updating the model should not require full retraining every time; practical routes include SFT, distillation, PEFT, LoRA, replay, parameter regularization, and knowledge editing.
- Evaluation should cover not only new-knowledge correctness, but also broad capability retention, old-knowledge retention, cross-modal consistency, OOD generalization, and side effects.

Repository scope:

- The mainline focus is `VLM / MLLM knowledge injection + broad capability retention`.
- `LLM` papers are included as upstream inspirations only when their method can plausibly transfer to multimodal knowledge injection.
- Knowledge editing is included as a related inspiration branch because it is usually closer to small-scope / localized knowledge injection.
- GitHub and Hugging Face resources are included only when they have independent value; official paper repos stay attached to paper rows.

Current coverage:

| Dimension | Coverage | Notes |
| --- | --- | --- |
| Main method tracks | 4 | `Distill`, `Replay`, `Parameter Regularization`, and `LoRA Isolation` |
| Core papers | 50+ | includes verified entries and Agent-indexed candidates; candidates are explicitly marked in tables |
| Related inspirations | 2 groups | `LLM upstream methods` and `knowledge editing` |
| GitHub projects | 7+ | only integration-oriented repos and infrastructure that do not duplicate paper repos |
| Benchmarks / datasets | 8+ | public resources for knowledge injection, continual learning, and editing evaluation |

Last updated: `2026-05-19`

Maintenance note: this repository uses Agent-assisted updates. A lightweight update Agent periodically revisits arXiv, GitHub, company pages, and public benchmark pages to refresh links, stars, and candidate additions. All automated updates first go into a review-only workspace and only become canonical after human approval. See [MAINTAINER_AGENT.md](./MAINTAINER_AGENT.md).

🚧 Coming soon: we are building a companion `Project` with implementation code across different base models and baseline methods.

---

<a id="table-of-contents"></a>

## 🧭 Table of Contents

- [🧩 Task Introduction](#task-introduction)
- [📊 Benchmark Summary](#benchmark-summary)
- [🔬 Research Directions](#research-directions)
  - [Direction 1: Training Signals and Data Retention (Distill / Replay)](#direction-distill-replay)
    - [Distill](#distill)
    - [Direct MLLM / VLM Multimodal Distillation](#distill-multimodal)
    - [Online Distill: Loss Design, Token Weighting, and Dense Credit](#distill-online-loss)
    - [Self Distill: Self-Teacher, Privileged Context, and No-External-Teacher Retention](#distill-self)
    - [Online Distill + RL / RLVR](#distill-rl)
    - [Mechanism / Survey / Diagnosis](#distill-mechanism)
    - [Replay](#replay)
  - [Direction 2: Constrained Parameter-Space Updates (Parameter Regularization)](#direction-parameter-regularization)
  - [Direction 3: Modular Isolation and Local Updates (LoRA Isolation / Knowledge Editing)](#direction-lora-isolation)
  - [Related Inspiration: Knowledge Editing](#knowledge-editing)
- [🌐 Ecosystem](#ecosystem)
  - [GitHub](#ecosystem-github)
  - [Hugging Face](#ecosystem-huggingface)
- [📮 Contact Us](#contact-us)

<a id="benchmark-summary"></a>

## 📊 Benchmark Summary

Benchmarks should answer two questions at the same time: `whether new knowledge was successfully injected` and `whether broad original capabilities were preserved`. Therefore, evaluation should not only measure new-knowledge QA accuracy, but also retention, cross-modal consistency, OOD phrasing, continual-update stability, and localized-edit side effects.

| Evaluation dimension | Main question | Representative entries |
| --- | --- | --- |
| New-knowledge injection | Can the model correctly answer or use new facts, domains, or tasks? | KORE-74K, EVOKE / MMEVOKE |
| Broad capability retention | Do original VQA, OCR, visual recognition, reasoning, and instruction-following abilities degrade? | MLLM-CL, UCIT, VTCTrain, VLMEvalKit |
| Continual learning / forgetting | Does catastrophic forgetting or answer-format drift emerge after repeated updates? | CoIN-ASD, MLLM-DCL, MTIL |
| Knowledge-editing side effects | Does a local factual update affect unrelated facts or cross-modal consistency? | MMKE-Bench, MC-MKE, ComprehendEdit |
| OOD / evolving knowledge | Does the model generalize under paraphrases, cross-event questions, and cross-domain settings? | EVOKE / MMEVOKE, ImageWikiQA |

### Supporting Benchmarks and Evaluation Papers

| Time | Paper | Approach | Model type | Experimental task | Primary datasets / benchmarks | GitHub | Stars | Venue | Year |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| 2025-05 | [When Large Multimodal Models Confront Evolving Knowledge: Challenges and Explorations](https://arxiv.org/abs/2505.24449) | defines evolving multimodal knowledge through EVOKE / MMEVOKE | VLM / MLLM | multimodal knowledge updating, evolving-knowledge evaluation | EVOKE / MMEVOKE | [EVOKE-LMM/EVOKE](https://github.com/EVOKE-LMM/EVOKE) | 113 | arXiv / ICLR 2026 | 2025 |
| 2025-02 | [MMKE-Bench](https://arxiv.org/abs/2502.19870) | benchmark for diverse visual knowledge editing | VLM / MLLM | multimodal knowledge editing evaluation | MMKE-Bench | - | - | ICLR 2025 | 2025 |
| 2024-12 | [ComprehendEdit](https://arxiv.org/abs/2412.12821) | more complete multimodal editing data and evaluation framework | VLM / MLLM | comprehensive multimodal editing evaluation | ComprehendEdit | - | - | arXiv | 2024 |
| 2024-06 | [MC-MKE](https://arxiv.org/abs/2406.13219) | evaluation centered on cross-modal consistency after editing | VLM / MLLM | cross-modal consistency evaluation | MC-MKE | - | - | Findings of ACL 2025 | 2024 |

### Benchmarks and Datasets

This section keeps only data / benchmark / code entry points. Paper-level interpretation lives in the research-direction tables below.

| Name | Type | Links | Notes |
| --- | --- | --- | --- |
| EVOKE / MMEVOKE | benchmark + code + dataset | [code](https://github.com/EVOKE-LMM/EVOKE) / [dataset](https://huggingface.co/datasets/kailinjiang/MMEVOKE) | Main benchmark entry for evolving multimodal knowledge. |
| KORE-74K | dataset + code | [code](https://github.com/KORE-LMM/KORE) / [dataset](https://huggingface.co/datasets/kailinjiang/KORE-74K) | Data entry for retention-aware multimodal injection. |
| MLLM-CL | benchmark + dataset | [dataset](https://huggingface.co/datasets/MLLM-CL/MLLM-CL) | Core entry for domain-vs-ability continual-learning evaluation. |
| UCIT | dataset | [dataset](https://huggingface.co/datasets/MLLM-CL/UCIT) | Continual instruction-tuning dataset. |
| VTCTrain | dataset | [dataset](https://huggingface.co/datasets/MLLM-CL/VTCTrain) | Continual training / instruction-style task data. |
| DCL_10Percent_with_RAG | dataset | [dataset](https://huggingface.co/datasets/MLLM-CL/DCL_10Percent_with_RAG) | Connects domain continual learning with RAG-style support. |
| MMKE-Bench | benchmark data | [dataset](https://huggingface.co/datasets/kailinjiang/MMKE-Bench-dataset) | Multimodal knowledge-editing benchmark data. |
| MC-MKE | benchmark data | [dataset](https://huggingface.co/datasets/reroze/MC-MKE) | Useful when cross-modal consistency matters. |
| KnowEdit | dataset | [dataset](https://huggingface.co/datasets/zjunlp/KnowEdit) | Adjacent knowledge-editing data entry. |

<a id="research-directions"></a>

## 🔬 Research Directions

The mainline of this README focuses on one target problem: `VLM / MLLM knowledge injection while preserving broad general capability`. Under a continual-learning lens, methods can be organized into four categories:

- `Distill`
- `Replay`
- `Parameter Regularization`
- `LoRA Isolation`

At the moment, the strongest direct public results for `VLM / MLLM knowledge injection + retention` are mostly concentrated in `Parameter Regularization` and `LoRA Isolation`. `Distill` and `Replay` remain important mainline directions; within `Distill`, we now have a meaningful set of direct `VLM / MLLM` papers, but many of the most mature training recipes and analyses still come from the `LLM` setting.

<a id="direction-overview"></a>

### Direction Overview

| Category | Core idea | Strengths | Weaknesses | Current public status |
| --- | --- | --- | --- | --- |
| `Distill` | use a teacher / old model distribution to constrain updates | does not always require storing a large old dataset; directly preserves behavior | sensitive to teacher quality and training cost; can distill wrong distributions too | direct VLM / MLLM papers now exist, but the knowledge-injection-with-retention setting is still less mature than parameter-regularization lines |
| `Replay` | mix old samples, replay buffers, or data mixtures during training | simple and often strong in practice; easy to reason about | needs stored or synthesized old data; privacy, storage, and training cost are higher | current public evidence is still more recipe- and diagnosis-like |
| `Parameter Regularization` | constrain important parameters, update directions, or low-rank spaces | lightweight, PEFT-friendly, easy to plug into existing training stacks | may under-inject if regularization is too strong; hyperparameter-sensitive | currently the strongest and densest direct public mainline |
| `LoRA Isolation` | isolate knowledge via separate LoRAs, experts, or routing paths | strong interference control; modular | higher parameter / deployment complexity; routing adds design overhead | few public representatives so far, but the direction is clear |

Except for `Distill`, tables below are sorted chronologically, newest first. `Distill` is further split by distillation signal source, loss design, and model type. `Online distill` and `self distill` can overlap, so each paper is filed by its dominant contribution while cross-tags are kept in the position field. `GitHub / Stars` records only public official repositories; newly added rows use snapshots verifiable around `2026-05-19`, while older rows keep their original snapshots.

<a id="direction-distill-replay"></a>

### Direction 1: Training Signals and Data Retention (Distill / Replay)

<a id="distill"></a>

#### Distill

This remains a mainline category for the target problem. Based on the `2026-05-18` Agent discovery run, the four-way split proposed by the user is mostly valid, but this README uses five buckets: `direct MLLM / VLM multimodal distillation`, `Online Distill: loss and token weighting`, `Self Distill: self-teacher / privileged context`, `Online Distill + RL / RLVR`, and `Mechanism / Survey / Diagnosis`. The reason is that `online distill` describes training on the student's on-policy trajectories, while `self distill` describes whether the teacher comes from the same model; the two axes can overlap.

| Subcategory | Filing criterion | Common training signals / losses | Relevance to knowledge injection and retention |
| --- | --- | --- | --- |
| `Direct MLLM / VLM multimodal distillation` | the experimental target is directly VLM / MLLM / Video-LLM | logit distillation, feature distillation, visual-token distillation, cross-modal token interaction, structured video feedback | closest to the repository's main problem; highest priority |
| `Online Distill: loss and token weighting` | the student learns on its own generated trajectories with dense teacher / verifier / rubric supervision | KL / reverse-KL, sampled-token NLL, top-K support matching, token-importance weighting, teacher-uncertainty weighting, control variates | directly informs how to constrain output distribution drift during knowledge injection |
| `Self Distill: self-teacher / privileged context` | teacher and student share the same base model; the teacher receives reference answers, feedback, harnesses, or other privileged context | privileged-context KL, logit steering, reward-regularized KL, reflection-enhanced feedback, teacher-exposure scheduling | useful when no strong external teacher is available, but can reinforce the model's own errors |
| `Online Distill + RL / RLVR` | combines sparse reward / GRPO / RLVR with dense distillation signals | advantage / reward weighted distillation, sample routing, post-RL compaction, dense credit assignment | highly relevant for the target setting: sparse reward defines the new target, distillation stabilizes behavior |
| `Mechanism / Survey / Diagnosis` | main contribution explains when OPD / OPSD works or fails | failure modes, teacher-prefix drift, tokenizer mismatch, teachability collapse, capability-loss accounting | helps decide whether LLM recipes should be transferred to MLLMs |

<a id="distill-multimodal"></a>

##### Direct MLLM / VLM Multimodal Distillation

| Time | Position | Paper | Approach / Loss design | Model type | Task / datasets | Code / Stars | Venue / status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05 | Video-LLM self-distillation + RL signal | [VISD: Enhancing Video Reasoning via Structured Self-Distillation](https://arxiv.org/abs/2605.06094) | video-aware judge generates structured feedback; sparse rewards are combined with token-level self-distillation | VLM / Video-LLM | video reasoning, spatio-temporal grounding; diverse video reasoning benchmarks | - | arXiv / recorded |
| 2026-05 | VLM cascaded KD | [LLaVA-CKD: Bottom-Up Cascaded Knowledge Distillation for Vision-Language Models](https://arxiv.org/abs/2605.10641) | bottom-up cascaded KD for staged VLM capability compression | VLM | VQA and vision-language understanding | - | arXiv / indexed candidate |
| 2026-04 | multimodal black-box OPD | [Beyond SFT-to-RL: Pre-alignment via Black-Box On-Policy Distillation for Multimodal RL](https://arxiv.org/abs/2604.28123) | inserts response-level black-box OPD between SFT and RLVR; MoE discriminators separate perception and reasoning drift | VLM / MLLM | multimodal RLVR, visual grounding, reasoning retention; Qwen3-VL 4B / 8B, 1.26M public demos + 113K Gemini 3 Flash demonstrations | [XIAO4579/PRISM](https://github.com/XIAO4579/PRISM) / 70 | arXiv / recorded |
| 2026-03 | uncertainty-aware MLLM KD | [Uncertainty-Aware Knowledge Distillation for Multimodal Large Language Models (Beta-KD)](https://arxiv.org/abs/2603.21426) | beta-distribution weighting balances data supervision and teacher supervision according to teacher uncertainty | VLM / MLLM | MLLM compression and transfer; GQA / ScienceQA-IMG / TextVQA / POPE / MME-P / MMBench-dev | [Jingchensun/beta-kd](https://github.com/Jingchensun/beta-kd) / 3 | CVPR 2026 / recorded |
| 2026-02 | multimodal token-interaction KD | [Beyond Next-Token Alignment: Distilling Multimodal Large Language Models via Token Interactions](https://arxiv.org/abs/2602.09483) | explicitly models cross-modal token interactions rather than only next-token alignment | VLM / MLLM | MLLM compression and cross-modal retention | - | arXiv / indexed candidate |
| 2025-12 | VLM long-window KD | [Towards Long-window Anchoring in Vision-Language Model Distillation](https://arxiv.org/abs/2512.21576) | long-window anchoring addresses limited student context windows under VLM distillation | VLM | long-context vision-language understanding | - | AAAI 2026 / indexed candidate |
| 2025-12 | masked teacher + reinforced student | [Masking Teacher and Reinforcing Student for Distilling Vision-Language Models](https://arxiv.org/abs/2512.22238) | masks teacher signals and reinforces the student for compact VLM distillation | VLM | lightweight VLM deployment | - | arXiv / indexed candidate |
| 2025-11 | unbalanced visual-token KD | [EM-KD: Distilling Efficient Multimodal Large Language Model with Unbalanced Vision Tokens](https://arxiv.org/abs/2511.21106) | KD for efficient MLLMs under unbalanced visual tokens | VLM / MLLM | MLLM compression, visual-information retention | - | AAAI 2026 / indexed candidate |
| 2025-11 | CLIP / VQA KD diagnosis | [When Better Teachers Don't Make Better Students](https://arxiv.org/abs/2511.17886) | analyzes why stronger teachers do not always produce stronger multimodal students | VLM / CLIP | VQA, CLIP distillation | - | arXiv / indexed candidate |
| 2025-03 | multi-prompt self-distillation | [Enhancing Multi-hop Reasoning in Vision-Language Models via Self-Distillation with Multi-Prompt Ensembling](https://arxiv.org/abs/2503.01754) | multi-prompt ensembling plus self-distillation compresses stronger reasoning traces back into the student | VLM / MLLM | multi-hop visual reasoning, VQA; 5 VQA benchmarks | - | arXiv / recorded |
| 2024-09 | VLM continual dual-teacher KD | [Adapt without Forgetting](https://www.ecva.net/papers/eccv_2024/papers_ECCV/html/7052_ECCV_2024_paper.php) | dual-teacher proximity distillation with graph modeling for new-task adaptation and zero-shot retention | VLM | continual learning, zero-shot retention; MTIL + CIFAR100 / TinyImageNet | [myz-ah/AwoForget](https://github.com/myz-ah/AwoForget) / - | ECCV 2024 / recorded |
| 2024-09 | selective dual-teacher KD | [Select and Distill](https://www.ecva.net/papers/eccv_2024/papers_ECCV/html/7626_ECCV_2024_paper.php) | selective dual-teacher transfer separates what to preserve from what to adapt | VLM | FGVCAircraft / DTD / EuroSAT / Flowers102 / Food101 / OxfordPets / StanfordCars / UCF101 / ImageNet | [chu0802/SnD](https://github.com/chu0802/SnD) / 16 | ECCV 2024 / recorded |
| 2024-07 | MLLM KD factor analysis | [LLAVADI](https://arxiv.org/abs/2407.19409) | combines feature distillation, logit distillation, and teacher-generated data | VLM / MLLM | CC-595K / LLaVA-665K + GQA / ScienceQA-IMG / TextVQA / POPE / MME-P / MMBench-dev | - | arXiv / recorded |
| 2023-12 | visual program distillation | [Visual Program Distillation](https://openaccess.thecvf.com/content/CVPR2024/html/Hao_Visual_Program_Distillation_OFDistilling_Tools_and_Programmatic_Reasoning_into_Vision-Language_CVPR_2024_paper.html) | distills LLM-generated programs and tool-use reasoning into a VLM | VLM / MLLM | MMBench / OK-VQA / A-OKVQA / TallyQA / POPE / Hateful Memes | - | CVPR 2024 / recorded |
| 2023-10 | lifelong multi-domain VQA | [Multi-Domain Lifelong Visual Question Answering via Self-Critical Distillation](https://dl.acm.org/doi/10.1145/3581783.3612274) | replay-free self-critical distillation over logits and intermediate representations | VLM | CLEVR / GQA / VizWiz / AQUA / VQA-Abstract | - | ACM MM 2023 / recorded |

<a id="distill-online-loss"></a>

##### Online Distill: Loss Design, Token Weighting, and Dense Credit

| Time | Position | Paper | Approach / Loss design | Model type | Task / datasets | Code / Stars | Venue / status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05 | neighboring token-weighting inspiration | [InfoSFT](https://arxiv.org/abs/2605.14967) | information-aware token weighting makes SFT learn more and forget less; not strict OPD, but useful for loss-weighting design | LLM | SFT / forgetting mitigation | - | arXiv / indexed candidate |
| 2026-05 | token-level self-uncertainty weighting | [Respecting Self-Uncertainty in On-Policy Self-Distillation](https://arxiv.org/abs/2605.13255) | weights teacher token signals by self-uncertainty instead of using uniform token supervision | LLM | reasoning post-training | - | arXiv / indexed candidate |
| 2026-05 | prefix / suffix teachability diagnosis | [Prefix Teach, Suffix Fade](https://arxiv.org/abs/2605.13643) | shows suffix-token teaching signal can collapse in strong-to-weak OPD | LLM | structured output / reasoning | - | arXiv / indexed candidate |
| 2026-05 | multi-rollout peer supervision | [Multi-Rollout On-Policy Distillation](https://arxiv.org/abs/2605.12652) | uses peer successes and failures across rollouts for finer token supervision | LLM | verifier-based reasoning | - | arXiv / indexed candidate |
| 2026-05 | token-routed self-OPD | [TRACE](https://arxiv.org/abs/2605.10194) | avoids all-token KL waste by routing supervision to important tokens | LLM | RLVR / reasoning alignment | - | arXiv / indexed candidate |
| 2026-05 | best-of-N teacher rollout | [On-Policy Distillation with Best-of-N Teacher Rollout Selection](https://arxiv.org/abs/2605.09725) | selects better teacher rollouts before distillation to reduce noisy teacher signals | LLM | reasoning post-training | - | arXiv / indexed candidate |
| 2026-05 | rock-token analysis | [Cornerstones or Stumbling Blocks?](https://arxiv.org/abs/2605.09253) | analyzes how a small set of critical tokens drives or harms OPD learning | LLM | reasoning / token-level credit | - | arXiv / indexed candidate |
| 2026-05 | rubric-based OPD | [Rubric-based On-policy Distillation](https://arxiv.org/abs/2605.07396) | uses structured semantic rubrics instead of white-box teacher logits | LLM | alignment / reasoning | - | arXiv / indexed candidate |
| 2026-05 | control-variate KL | [KL for a KL](https://arxiv.org/abs/2605.07865) | uses a control-variate baseline to reduce single-sample OPD gradient variance | LLM | reasoning post-training | - | arXiv / indexed candidate |
| 2026-05 | step-wise OPD | [SOD](https://arxiv.org/abs/2605.07725) | decomposes sparse trajectory supervision into step-wise distillation for small agent models | LLM / Agent | tool-integrated reasoning | - | arXiv / indexed candidate |
| 2026-05 | cross-tokenizer OPD | [SimCT](https://arxiv.org/abs/2605.07711) | recovers supervision lost when teacher and student tokenizers differ | LLM | cross-tokenizer distillation | - | arXiv / indexed candidate |
| 2026-05 | long-horizon pruning | [Prune-OPD](https://arxiv.org/abs/2605.07804) | prunes low-value or drifted distillation signals in long-horizon reasoning to improve OPD reliability | LLM | long-horizon reasoning | - | arXiv / indexed candidate |
| 2026-05 | multi-agent debate teacher | [MAD-OPD](https://arxiv.org/abs/2605.01347) | uses multi-agent debate to produce stronger token-level teacher supervision and break the single-teacher ceiling | LLM / Agent | agentic reasoning | - | arXiv / indexed candidate |
| 2026-04 | dual-path adaptive weighting | [SCOPE](https://arxiv.org/abs/2604.10688) | signal-calibrated OPD enhancement with dual-path adaptive weighting | LLM | reasoning alignment | - | arXiv / indexed candidate |
| 2026-04 | temporal curriculum | [TCOD](https://arxiv.org/abs/2604.24005) | adds temporal curriculum to multi-turn agent OPD to reduce long-horizon error accumulation | LLM / Agent | multi-turn autonomous agents | - | arXiv / indexed candidate |
| 2026-04 | token importance | [TIP: Token Importance in On-Policy Distillation](https://arxiv.org/abs/2604.14084) | directly studies which tokens carry useful OPD learning signal | LLM | online KD / reasoning | - | arXiv / indexed candidate |
| 2026-03 | frontier-of-competence sampling | [PACED](https://arxiv.org/abs/2603.11178) | focuses distillation / self-distillation on problems near the student's competence frontier | LLM | LLM distillation efficiency | - | arXiv / indexed candidate |
| 2026-03 | OPD failure fixes | [Revisiting On-Policy Distillation](https://arxiv.org/abs/2603.25562) | identifies long-rollout, teacher-prefix drift, and tokenizer-mismatch failures; proposes top-K local support matching | LLM | reasoning / agent multi-task training | - | arXiv / recorded |
| 2026-03 | hindsight + entropy weighting | [HEAL](https://arxiv.org/abs/2603.10359) | weights trajectory quality using hindsight information and teacher entropy | LLM | online distillation / reasoning | - | arXiv / recorded |
| 2026-02 | context distillation | [On-Policy Context Distillation for Language Models](https://arxiv.org/abs/2602.12275) | internalizes in-context knowledge into parameters by connecting context distillation with on-policy distillation | LLM | context distillation / knowledge internalization | - | arXiv / indexed candidate |
| 2023-06 | generative online distillation | [GKD](https://arxiv.org/abs/2306.13649) / [MiniLLM](https://arxiv.org/abs/2306.08543) | student-generated trajectory distillation; MiniLLM uses reverse-KL for generative students | LLM | instruction following, text generation; MT-Bench / AlpacaEval / instruction data | - | arXiv / recorded |

<a id="distill-self"></a>

##### Self Distill: Self-Teacher, Privileged Context, and No-External-Teacher Retention

| Time | Position | Paper | Approach / Loss design | Model type | Task / datasets | Code / Stars | Venue / status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05 | unified self-distillation | [UniSD](https://arxiv.org/abs/2605.06597) | studies self-generated trajectory reliability, representation alignment, and training stability in one framework | LLM | LLM adaptation / reasoning | - | arXiv / indexed candidate |
| 2026-05 | preference-based self-distillation | [Preference-Based Self-Distillation](https://arxiv.org/abs/2605.05040) | adds reward regularization beyond KL matching to stabilize self-distillation | LLM | reasoning post-training | - | arXiv / indexed candidate |
| 2026-05 | outcome-guided logit steering | [OGLS-SD](https://arxiv.org/abs/2605.12400) | uses outcome-guided logit steering to correct over-alignment in OPSD | LLM | LLM reasoning | - | arXiv / indexed candidate |
| 2026-05 | teacher-exposure scheduling | [Adaptive Teacher Exposure](https://arxiv.org/abs/2605.11458) | tunes privileged-teacher exposure to reduce teacher dependence | LLM | LLM reasoning | - | arXiv / indexed candidate |
| 2026-05 | input-specific credit | [From Generic Correlation to Input-Specific Credit](https://arxiv.org/abs/2605.11613) | moves from generic correlation to input-specific credit assignment | LLM | post-training / reasoning | - | arXiv / indexed candidate |
| 2026-05 | reflection-enhanced self-distillation | [Learning with Rare Success but Rich Feedback](https://arxiv.org/abs/2605.12741) | uses environmental feedback and reflection to strengthen self-distillation when successes are rare but feedback is rich | LLM | interactive / feedback learning | - | arXiv / indexed candidate |
| 2026-05 | harness self-distillation | [Training with Harnesses](https://arxiv.org/abs/2605.08741) | distills inference-time harness capabilities back into the base model | LLM | complex reasoning | - | arXiv / indexed candidate |
| 2026-05 | multilingual safety self-distill | [Multilingual Safety Alignment via Self-Distillation](https://arxiv.org/abs/2605.02971) | transfers high-resource-language safety ability to low-resource languages through self-distillation | LLM | multilingual safety alignment | - | arXiv / indexed candidate |
| 2026-04 | performance-recovery self-distillation | [Self-Distillation as a Performance Recovery Mechanism for LLMs](https://arxiv.org/abs/2604.15794) | uses SDFT to recover after SFT / quantization / pruning and analyzes manifold alignment with CKA | LLM | performance recovery / forgetting mitigation | - | arXiv / recorded |
| 2026-01 | OPSD | [Self-Distilled Reasoner](https://arxiv.org/abs/2601.18734) | cold-starts CoT and iteratively distills from the same model under privileged context | LLM | math reasoning; AIME24 / AIME25 / HMMT25 | [siyan-zhao/OPSD](https://github.com/siyan-zhao/OPSD) / 107 | arXiv / recorded |
| 2026-01 | continual-learning self-distillation | [Self-Distillation Enables Continual Learning](https://arxiv.org/abs/2601.19897) | teacher uses demonstrations / privileged context while student sees normal inputs; KL preserves old behavior while learning new tasks | LLM | continual learning, domain adaptation; sequential task streams | - | arXiv / recorded |

<a id="distill-rl"></a>

##### Online Distill + RL / RLVR: Sparse Rewards with Dense Distillation

| Time | Position | Paper | Approach / Loss design | Model type | Task / datasets | Code / Stars | Venue / status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05 | language feedback + variational PD | [Learning from Language Feedback via Variational Policy Distillation](https://arxiv.org/abs/2605.15113) | turns language feedback into variational policy distillation to reduce RLVR exploration bottlenecks | LLM | verifiable reasoning | - | arXiv / indexed candidate |
| 2026-05 | agentic RL + OPSD | [Self-Distilled Agentic Reinforcement Learning](https://arxiv.org/abs/2605.15155) | uses OPSD to provide dense trajectory supervision for long-horizon agent RL | LLM / Agent | long-horizon agent interaction | - | arXiv / indexed candidate |
| 2026-05 | agent advantage reweighting | [GEAR](https://arxiv.org/abs/2605.11853) | converts outcome-level reward into finer supervision with granularity-adaptive advantage reweighting | LLM / Agent | agent post-training | - | arXiv / indexed candidate |
| 2026-05 | self-distilled RLVR exploration | [Rebellious Student](https://arxiv.org/abs/2605.10781) | reverses selected teacher signals in self-distilled RLVR to encourage exploration | LLM | reasoning RLVR | - | arXiv / indexed candidate |
| 2026-05 | on-policy optimization + distillation | [Combining On-Policy Optimization and Distillation for Long-Context Reasoning](https://arxiv.org/abs/2605.12227) | combines on-policy optimization and distillation for stable long-context reasoning | LLM | long-context reasoning | - | arXiv / indexed candidate |
| 2026-05 | sparse-to-dense reward principle | [Beyond GRPO and On-Policy Distillation](https://arxiv.org/abs/2605.12483) | compares how sparse GRPO rewards and dense OPD rewards should allocate checked examples | LLM | language-model post-training | - | arXiv / indexed candidate |
| 2026-05 | reward-weighted OPD | [Reward-Weighted On-Policy Distillation](https://arxiv.org/abs/2605.13501) | uses verifier signals to drive reward-weighted OPD | LLM | NL-to-SVA generation | - | arXiv / indexed candidate |
| 2026-05 | post-RL compaction | [OPSD Compresses What RLVR Teaches](https://arxiv.org/abs/2605.06188) | uses OPSD as a post-RL compaction stage, while noting weaker gains in long-thinking settings | LLM | reasoning models / RLVR compaction | - | arXiv / indexed candidate |
| 2026-05 | anti-self-distillation | [Anti-Self-Distillation for Reasoning RL via Pointwise Mutual Information](https://arxiv.org/abs/2605.11609) | uses PMI to identify and reverse self-distillation signals that may lock in bad reasoning behavior | LLM | reasoning RL | - | arXiv / indexed candidate |
| 2026-04 | co-evolving policy distillation | [Co-Evolving Policy Distillation](https://arxiv.org/abs/2604.27083) | introduces OPD during each expert's RLVR training instead of after expert training is complete | LLM | multi-expert capability consolidation | - | arXiv / indexed candidate |
| 2026-04 | RLVR + self-distillation | [Self-Distilled RLVR](https://arxiv.org/abs/2604.03128) | adds self-distillation to RLVR to reduce rollout variance and stabilize reasoning gains | LLM | math reasoning, RLVR | - | arXiv / recorded |
| 2026-04 | unified GRPO + self-distill | [SRPO](https://arxiv.org/abs/2604.02288) | unifies GRPO and self-distillation policy optimization through sample routing | LLM | verifiable reasoning; 5 reasoning benchmarks | - | arXiv / recorded |

<a id="distill-mechanism"></a>

##### Mechanism / Survey / Diagnosis

| Time | Position | Paper | Main finding | Model type | Task / datasets | Code / Stars | Venue / status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05 | OPD applicability analysis | [Unmasking On-Policy Distillation](https://arxiv.org/abs/2605.10889) | analyzes when OPD helps and when it hurts training | LLM | reasoning post-training | - | arXiv / indexed candidate |
| 2026-05 | OPD / OPSD mechanism map | [The Many Faces of On-Policy Distillation](https://arxiv.org/abs/2605.11182) | summarizes OPD / OPSD mechanisms, pitfalls, and fixes | LLM | reasoning / post-training | - | arXiv / indexed candidate |
| 2026-05 | OPD extrapolation risk | [The Extrapolation Cliff in On-Policy Distillation](https://arxiv.org/abs/2605.08737) | shows excessive reward extrapolation can break near-deterministic structured-output contracts | LLM | structured output | - | arXiv / indexed candidate |
| 2026-05 | unified OPD recipe | [Uni-OPD](https://arxiv.org/abs/2605.03677) | offers a dual-perspective OPD recipe and discusses reliability conditions | LLM | expert capability consolidation | - | arXiv / indexed candidate |
| 2026-04 | loss-accounting position paper | [Knowledge Distillation Must Account for What It Loses](https://arxiv.org/abs/2604.25110) | argues distillation evaluation should track teacher capabilities lost by the student, not only task scores | LLM | distillation evaluation | - | arXiv / indexed candidate |
| 2026-04 | OPD survey | [A Survey of On-Policy Distillation for Large Language Models](https://arxiv.org/abs/2604.00626) | organizes OPD definitions, variants, use cases, and risks | LLM | survey | - | arXiv / recorded |
| 2026-04 | OPD mechanism analysis | [Rethinking On-Policy Distillation of Large Language Models](https://arxiv.org/abs/2604.13016) | explains OPD from phenomenology, mechanism, and training recipe perspectives | LLM | OPD training / evaluation | - | arXiv / recorded |

<a id="replay"></a>

#### Replay

This is also a mainline category, but public results currently lean more toward `data mixing / replay-like recipes` than toward a dense cluster of canonical methods.

| Time | Position | Paper | Approach | Model type | Experimental task | Primary datasets / benchmarks | GitHub | Stars | Venue | Year |
| --- | --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| 2026-03 | mainline-related recipe insight | [Fine-tuning MLLMs Without Forgetting Is Easier Than You Think](https://arxiv.org/abs/2603.14493) | data mixing + replay-like recipe + diagnostic forgetting analysis | VLM / MLLM | multimodal continual fine-tuning, forgetting analysis, practical recipe design | ImageNet-VQA / ImageWikiQA / LLaVA-665K / MLLM-CL | - | - | arXiv | 2026 |

<a id="direction-parameter-regularization"></a>

### Direction 2: Constrained Parameter-Space Updates (Parameter Regularization)

This is currently one of the strongest mainline categories. The shared pattern is to optimize knowledge injection and capability retention together through update constraints, parameter importance, or constrained low-rank adaptation.

| Time | Position | Paper | Approach | Model type | Experimental task | Primary datasets / benchmarks | GitHub | Stars | Venue | Year |
| --- | --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| 2026-02 | mainline method | [Model-Dowser: Data-Free Importance Probing to Mitigate Catastrophic Forgetting in Multimodal Large Language Models](https://arxiv.org/abs/2602.04509) | data-free importance probing that protects high-importance parameters based on weight magnitude, input activations, and output sensitivity | VLM / MLLM | MLLM fine-tuning, catastrophic-forgetting mitigation | LLaVA / NVILA adaptation and broad-capability retention evaluations | [model-dowser/model-dowser.github.io](https://github.com/model-dowser/model-dowser.github.io) | 0 | arXiv | 2026 |
| 2026-02 | mainline method | [Spectral Imbalance Causes Forgetting in Low-Rank Continual Adaptation](https://arxiv.org/abs/2602.00722) | analyzes singular-value spectral imbalance in low-rank updates and balances update directions via constrained Stiefel-manifold optimization | VLM / MLLM | PEFT continual learning, backward / forward forgetting mitigation | UCIT / MLLM-DCL / MM-MergeBench | [haodotgu/EBLoRA](https://github.com/haodotgu/EBLoRA) | 3 | arXiv | 2026 |
| 2026-01 | mainline method | [KeepLoRA](https://openreview.net/forum?id=T3Vc5fkTzV) | residual gradient adaptation + low-rank update control | VLM / MLLM | continual learning, multi-task / VQA-style retention | MTIL / MLLM-DCL / UCIT | [MaolinLuo/KeepLoRA](https://github.com/MaolinLuo/KeepLoRA) | 51 | ICLR 2026 | 2026 |
| 2025-10 | mainline method | [KORE](https://arxiv.org/abs/2510.19316) | retention-aware augmentations + constrained updates for joint injection and preservation | VLM / MLLM | multimodal knowledge injection, capability retention | KORE-74K | [KORE-LMM/KORE](https://github.com/KORE-LMM/KORE) | 141 | arXiv | 2025 |
| 2025-05 | mainline method | [SEFE: Superficial and Essential Forgetting Eliminator for Multimodal Continual Instruction Tuning](https://arxiv.org/abs/2505.02486) | separates superficial forgetting from essential forgetting and combines ASD + RegLoRA to handle answer-style drift and true forgetting together | VLM / MLLM | multimodal continual instruction tuning, capability retention | CoIN-ASD / ScienceQA / TextVQA / ImageNet / GQA / VizWiz / Grounding / VQAv2 / OCRVQA | [jinpeng0528/SEFE](https://github.com/jinpeng0528/SEFE) | 11 | ICML 2025 | 2025 |
| 2025-03 | LLM upstream inspiration | [LoRA-Null](https://openreview.net/forum?id=1R9JgqUVBp) | null-space initialization + conservative low-rank updates; a useful reference line for preservation-oriented PEFT | LLM | retention during fine-tuning, forgetting mitigation | TriviaQA / NQ-Open / WebQuestions + general capability benchmarks | [HungerPWAY/LoRA-Null](https://github.com/HungerPWAY/LoRA-Null) | 9 | arXiv / OpenReview | 2025 |

<a id="direction-lora-isolation"></a>

### Direction 3: Modular Isolation and Local Updates (LoRA Isolation / Knowledge Editing)

This line reduces interference by isolating knowledge into separate LoRAs, experts, or routing paths.

| Time | Position | Paper | Approach | Model type | Experimental task | Primary datasets / benchmarks | GitHub | Stars | Venue | Year |
| --- | --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| 2026-03 | mainline method / MoE routing isolation | [On Token's Dilemma: Dynamic MoE with Drift-Aware Token Assignment for Continual Learning of Large Vision Language Models](https://arxiv.org/abs/2603.27481) | dynamically expands MoE and uses token-level assignment guidance to suppress routing drift | VLM / LVLM | multimodal continual instruction tuning, old-task token routing retention | multi-task LVLM continual-learning evaluation | [zhaoc5/DyMoE](https://github.com/zhaoc5/DyMoE) | 3 | arXiv | 2026 |
| 2026-02 | mainline method / MoE isolation | [Continual-NExT: A Unified Comprehension And Generation Continual Learning Framework](https://arxiv.org/abs/2602.18055) | MAGE mixes General LoRA and Expert LoRA to support continual comprehension and generation in dual-to-dual MLLMs | VLM / MLLM | unified continual learning for comprehension + generation, cross-modal knowledge transfer | Continual-NExT framework / benchmark | [ECNU-SII/Continual-NExT](https://github.com/ECNU-SII/Continual-NExT) | 232 | arXiv | 2026 |
| 2026-02 | mainline method / MoE isolation | [SAME: Stabilized Mixture-of-Experts for Multimodal Continual Instruction Tuning](https://arxiv.org/abs/2602.01990) | decomposes routing dynamics into orthogonal subspaces and constrains expert updates with curvature-aware scaling from historical input covariance | VLM / MLLM | multimodal continual instruction tuning, router-drift / expert-drift mitigation | CoIN MCIT / ScienceQA / TextVQA / ImageNet / GQA / VizWiz / Ref-REC / VQAv2 / OCR-VQA | - | - | arXiv | 2026 |
| 2026 | mainline method / LoRA structure expansion | [LoRA in LoRA: Towards Lifelong Model Editing via Integrating LoRA Dynamics in Evolving Subspace](https://ojs.aaai.org/index.php/AAAI/article/view/39082) | integrates LoRA dynamics inside an evolving subspace for lifelong editing and capability retention | VLM / MLLM | continual visual instruction tuning, model editing, capability retention | CVIT Benchmark: ScienceQA / TextVQA / Flickr30k / ImageNet / GQA / VQAv2 | - | - | AAAI 2026 | 2026 |
| 2025-06 | mainline method | [MLLM-CL](https://arxiv.org/abs/2506.05453) | MR-LoRA routing + expert decomposition across domain and ability tracks | VLM / MLLM | domain continual learning, ability continual learning | MLLM-CL / UCIT / VTCTrain / DCL | [bjzhb666/MLLM-CL](https://github.com/bjzhb666/MLLM-CL) | 64 | arXiv | 2025 |

Related engineering entry:
`[Ghy0501/MCITlib](https://github.com/Ghy0501/MCITlib)` is more of a toolkit / benchmark organization repo than a single-paper method, but it is worth tracking from the engineering side of this category.

<a id="knowledge-editing"></a>

#### Related Inspiration: Knowledge Editing

Knowledge editing is not the main problem setup of this repository. It is usually closer to `small-scope / local knowledge injection`, such as editing one fact or a small set of facts, rather than preserving broad capability under larger-scale multimodal knowledge injection. Still, it is very relevant for localized updates and side-effect control.

| Time | Paper | Approach | Model type | Experimental task | Primary datasets / benchmarks | GitHub | Stars | Venue | Year |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| 2025-07 | [AdaEdit: Advancing Continuous Knowledge Editing For Large Language Models](https://aclanthology.org/2025.acl-long.208/) | stable update mechanism for continuous editing | LLM | continuous knowledge editing, long-horizon stability evaluation | CounterFact / zsRE / continuous edit sequences | - | - | ACL 2025 | 2025 |
| 2024 | [EasyEdit: An Easy-to-use Knowledge Editing Framework for LLMs](https://github.com/zjunlp/EasyEdit) | unified experimental framework and method integration | LLM | reproducible knowledge-editing evaluation | CounterFact / zsRE / KnowEdit and related datasets | [zjunlp/EasyEdit](https://github.com/zjunlp/EasyEdit) | 2769 | ACL 2024 | 2024 |
| 2023-10 | [Can We Edit Multimodal Large Language Models?](https://aclanthology.org/2023.emnlp-main.856/) | extends knowledge editing into multimodal models | VLM / MLLM | multimodal factual editing, image-text knowledge updates | MMEdit / multimodal editing benchmarks | - | - | EMNLP 2023 | 2023 |
| 2022-10 | [MEMIT](https://arxiv.org/abs/2210.07229) | many-fact batch editing | LLM | batch factual updating, local knowledge modification | CounterFact / zsRE | [kmeng01/memit](https://github.com/kmeng01/memit) | 544 | ICLR 2023 | 2022 |
| 2022-02 | [ROME](https://arxiv.org/abs/2202.05262) | rank-one parameter update editing | LLM | single-fact editing, knowledge localization | CounterFact / zsRE | [kmeng01/rome](https://github.com/kmeng01/rome) | 743 | NeurIPS 2022 | 2022 |
| 2021-10 | [MEND: Fast Model Editing at Scale](https://openreview.net/pdf?id=0DcZxeWfOPt) | low-rank gradient editor network | LLM | fast local knowledge editing | zsRE / CounterFact | [eric-mitchell/mend](https://github.com/eric-mitchell/mend) | 259 | ICLR 2022 | 2021 |

<a id="ecosystem"></a>

## 🌐 Ecosystem

<a id="ecosystem-github"></a>

### GitHub

This section avoids repeating official repos that are already attached to papers in the research directions. Repos such as `KORE / SEFE / MLLM-CL / EVOKE` stay attached to paper rows above. This section keeps only integration-oriented repositories, training infrastructure, and evaluation tooling.

GitHub stars checked on `2026-04-17`.

<a id="ecosystem-github-aggregation"></a>

#### Aggregation / Integration Projects

| Project | Stars | Link | Category | Why it matters |
| --- | ---: | --- | --- | --- |
| Awesome Continual Learning of Vision-Language Models | 179 | [YuyangSunshine/Awesome-Continual-learning-of-Vision-Language-Models](https://github.com/YuyangSunshine/Awesome-Continual-learning-of-Vision-Language-Models) | neighboring awesome list | A valuable external index for VLM continual learning; new papers from it should be read, classified, and merged into the research directions instead of copied verbatim. |
| MCITlib | 78 | [Ghy0501/MCITlib](https://github.com/Ghy0501/MCITlib) | multimodal continual instruction tuning | Plays both a library and benchmark role. |

<a id="ecosystem-github-infrastructure"></a>

#### Training / Adaptation / Evaluation Infrastructure

| Project | Stars | Link | Category | Notes |
| --- | ---: | --- | --- | --- |
| PEFT | 20949 | [huggingface/peft](https://github.com/huggingface/peft) | PEFT infrastructure | Core infrastructure for LoRA, adapters, and low-rank updates. |
| LLaMA-Factory | 70198 | [hiyouga/LLaMA-Factory](https://github.com/hiyouga/LlamaFactory) | unified training framework | One of the most widely reused training stacks, now including broad VLM / MLLM support. |
| ms-swift | 13758 | [modelscope/ms-swift](https://github.com/modelscope/ms-swift) | training / post-training framework | Common in distillation, SFT, GRPO, and MLLM training pipelines. |
| VLMEvalKit | 4049 | [open-compass/VLMEvalKit](https://github.com/open-compass/VLMEvalKit) | evaluation framework | Very useful for retention, OOD, and broad-capability evaluation. |
| Avalanche | 2049 | [ContinualAI/avalanche](https://github.com/ContinualAI/avalanche) | continual learning toolkit | A general-purpose continual learning infrastructure repo. |

<a id="ecosystem-huggingface"></a>

### Hugging Face

Hugging Face currently serves mainly as a dataset, benchmark, and paper-related entry layer for this topic. This repository does not maintain a generic base-model download leaderboard.

| Resource | Type | Link | Notes |
| --- | --- | --- | --- |
| MMEVOKE | dataset / benchmark | [kailinjiang/MMEVOKE](https://huggingface.co/datasets/kailinjiang/MMEVOKE) | Entry point for evolving multimodal knowledge evaluation. |
| KORE-74K | dataset | [kailinjiang/KORE-74K](https://huggingface.co/datasets/kailinjiang/KORE-74K) | Dataset entry for retention-aware knowledge injection. |
| MLLM-CL | dataset / benchmark | [MLLM-CL/MLLM-CL](https://huggingface.co/datasets/MLLM-CL/MLLM-CL) | Entry point for domain-vs-ability continual-learning evaluation. |
| UCIT | dataset | [MLLM-CL/UCIT](https://huggingface.co/datasets/MLLM-CL/UCIT) | Continual instruction-tuning data entry. |
| MMKE-Bench | dataset / benchmark | [kailinjiang/MMKE-Bench-dataset](https://huggingface.co/datasets/kailinjiang/MMKE-Bench-dataset) | Multimodal knowledge-editing evaluation data. |
| MC-MKE | dataset / benchmark | [reroze/MC-MKE](https://huggingface.co/datasets/reroze/MC-MKE) | Cross-modal consistency editing benchmark data. |

<a id="contact-us"></a>

## 📮 Contact Us

If you have questions, suggestions, or would like to discuss potential collaboration, feel free to reach out:

- Yan Hong: `ruoning.hy@antgroup.com`
- Wei Li: `livie.lw@antgroup.com`
- Jun Lan: `yelan.lj@antgroup.com`
