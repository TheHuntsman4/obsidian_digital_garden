---
{"dg-publish":true,"permalink":"/garden/paper-reviews/lo-ra-efficient-low-rank-adaptation-of-large-models/"}
---

23.04.25

**Paper Link: "https://arxiv.org/pdf/2402.12354"**

- **Methodology used:** This paper uses a **theoretical approach by studying the infinite-width limit of LoRA finetuning dynamics**. Based on this analysis, they propose a new method called **LoRA+ which involves setting different learning rates for the A and B modules in the LoRA framework**. The methodology is **model-agnostic** and applicable to general neural networks. They then conduct **extensive empirical validation** of their theoretical findings using various language models and downstream tasks. They also experimentally determine a **default ratio for the learning rates of the A and B modules**.
    
- **New things introduced/ Novelty:** The primary novelty is the **introduction of LoRA+**, a modification to the standard LoRA method that uses **distinct learning rates for the A and B matrices**. The paper provides a **theoretical justification** for this approach based on the infinite-width limit analysis. The identification of a **generally beneficial default ratio (λ = ηB/ηA)** for these learning rates is also a novel practical contribution.
    
- **Key take aways and results:** The key takeaway is that the **standard LoRA setup is suboptimal for feature learning**. **LoRA+ improves feature learning** in low-rank adaptation in the infinite-width limit, leading to better performance. The **empirical results across different language models and tasks validate the effectiveness of LoRA+**. The identified **default learning rate ratio (λ)** can generally improve performance without extensive hyperparameter tuning.
    
- **Comparison with State of the Art (SOTA) and how better it is and under what circumstances:** The paper builds upon **LoRA (Low-Rank Adaptation)**, which is itself a state-of-the-art parameter-efficient finetuning method for large language models. LoRA+ improves upon standard LoRA by addressing its suboptimal learning dynamics, leading to **enhanced performance on downstream tasks** as demonstrated by the empirical results on benchmarks like MNLI and QQP. The improvements are observed across different language models, suggesting a general benefit.
    
- **Drawbacks that are discussed in the paper:** The paper **demonstrates that the original LoRA method has limitations** in its learning dynamics, as standard LoRA is shown to be suboptimal.
    
- **Improvements that can be made:** The paper introduces **LoRA+ as an improvement**. Further research could explore the **optimal learning rate ratios for different model architectures and task types**. Investigating the theoretical properties of LoRA and LoRA+ beyond the infinite-width limit could also yield further insights and potential improvements.
 
Notes on fine tuning:
- earlier, just training the the final layer and freezing all the other layers was used. This only lead to the output being the final output embeddings and it cant learn on internal model embeddings -> Adapters -> inference time is slower and is not efficient