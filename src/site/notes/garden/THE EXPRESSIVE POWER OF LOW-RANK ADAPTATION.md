---
{"dg-publish":true,"permalink":"/garden/the-expressive-power-of-low-rank-adaptation/"}
---

23.04.25
### Methodology Used

The paper theoretically analyzes the expressive power of Low-Rank Adaptation (LoRA) by investigating its ability to adapt a frozen pre-trained model (f0​) to accurately represent a target model (f​). The core methodology involves:

1. **Linear Model Analysis:** Starting with a simplified scenario where both the frozen and target models are linear. This serves as a warm-up to understand the minimal achievable approximation error and the smallest LoRA-rank required for exact representation under non-singularity assumptions. The proof for linear models involves decomposing the difference between the adapted and target models and relating it to the best low-rank approximation of the error matrix.
    
2. **Fully Connected Neural Network (FNN) Analysis:** Extending the analysis to ReLU FNNs. This involves addressing the non-linearity introduced by ReLU activation functions and biases. The techniques used include:
    
    - **Linearization:** Eliminating non-linearity in initial layers by using sufficiently large biases to ensure ReLUs are always activated.
        
    - **Weight Matrix Alignment:** Using the results from the linear case to align the weight matrices and bias vectors of the adapted model with the target model.
        
    - **Model Partition:** For multi-layer FNNs, partitioning the layers of the adapted model to approximate corresponding layers or groups of layers in the target model. Both uniform and general partitions are considered.
        
3. **Transformer Network (TFN) Analysis:** Extending the analysis to TFNs, which introduces non-linearities from softmax and ReLU. The approach involves segmenting sequences of transformer blocks based on these non-linearities and matching intermediate outputs. The analysis focuses on adding LoRA adapters primarily to self-attention layers.
    
4. **Empirical Validation:** Conducting experiments on both synthetic (linear models, FNNs, TFNs) and real datasets (GLUE benchmark) to validate the theoretical findings regarding the relationship between approximation error, LoRA-rank, model depth, and the distance between frozen and target models. The performance of the theoretically constructed LoRA adapters is compared with those optimized via gradient descent.
    

The theoretical proofs often rely on constructing specific LoRA adapters that achieve the desired approximation or exact representation under certain non-singularity assumptions about the weight matrices.

### New Things Introduced / Novelty

The paper presents the **first theoretical analysis of the expressive power of Low-Rank Adaptation (LoRA)**. Key novel contributions include:

- Providing theoretical bounds on the approximation error of LoRA for FNNs and TFNs as a function of the LoRA-rank.
    
- Identifying the minimum LoRA-rank required for an adapted model to exactly match a target model for FNNs and TFNs under specified conditions.
    
- Analyzing the impact of model architecture (depth, width) and the "distance" between the frozen and target models on the required LoRA-rank.
    
- Introducing techniques like linearization and model partition to extend linear model analysis to non-linear networks (FNNs and TFNs) in the context of LoRA's expressive power.
    
- The theoretical finding that even a randomly initialized FNN can be adapted to a target FNN using LoRA with a nearly optimal number of parameters (up to a constant factor), providing the first theoretical insight into LoRA's practical success.
    
- A theoretical comparison showing that LoRA can outperform tuning final layers, especially when the initial layers of the frozen model are not well-aligned with the target task.
    

### Key Takeaways and Results

- **Expressive Power:** LoRA is theoretically capable of adapting a frozen model to accurately represent a target model. For FNNs, exact representation is possible if the LoRA-rank is sufficiently high, related to the width and the ratio of frozen model depth to target model depth. For TFNs, exact representation is possible with a LoRA-rank related to the embedding size.
    
- **Rank and Error:** The approximation error decreases as the LoRA-rank increases. The paper quantifies this relationship with upper bounds.
    
- **Model Depth and Rank:** For FNNs, when the frozen model is significantly deeper than the target model, a smaller LoRA-rank per layer is sufficient for adaptation.
    
- **Model Proximity:** A frozen model that is "closer" to the target model (in terms of weight matrices) requires a lower LoRA-rank to achieve a desired level of performance.
    
- **Attention Layers in TFNs:** For TFNs, adapting primarily the attention weight matrices with LoRA can be sufficient for effective adaptation, aligning with empirical observations.
    
- **Comparison with Final Layer Tuning:** LoRA can be theoretically superior to tuning only the final layers, particularly for randomly initialized models, highlighting the benefit of distributing adaptation across layers.
    
- **Importance of Bias Tuning (FNNs):** Tuning biases alongside weight matrices in FNNs can be beneficial for LoRA's performance, especially at lower ranks, as it helps manage non-linearities.
    
- **Optimization Challenges:** While the theory shows the _existence_ of effective LoRA adapters, empirical results suggest that current gradient-based optimization methods may not always find the theoretically optimal adapters, especially for FNNs and TFNs at lower ranks.
    

### Comparison with State of the Art (SOTA) and How Better It Is

The paper compares LoRA primarily with:

- **Full Fine-tuning:** LoRA is a parameter-efficient alternative. The paper's theoretical contribution is not directly comparing performance metrics (like accuracy) with full fine-tuning but rather the _expressive power_ – showing that LoRA _can_ represent the same functions as the target model, suggesting it has the _potential_ to match full fine-tuning performance, which is supported by existing empirical evidence (cited in the introduction).
    
- **Tuning Final Layers:** The paper theoretically shows that LoRA can be strictly more expressive than tuning only the final layers, especially when the initial layers of the frozen model are not well-suited for the target task (e.g., randomly initialized). This provides a theoretical basis for empirical observations that LoRA often outperforms tuning final layers. The paper quantifies this by showing that LoRA requires fewer parameters to achieve exact approximation in certain FNN scenarios compared to the number of parameters updated in final layer tuning that fails to achieve the same.
    
- **Other Parameter-Efficient Methods (Briefly Mentioned):** The paper situates LoRA among other methods like BitFit, Prefix-tuning, Prompt-tuning, and Neural Reprogramming in the related works section. It notes that unlike these methods, there was previously little theoretical understanding of LoRA's expressive power. The paper's novelty lies in providing this theoretical foundation for LoRA, which is currently the dominant parameter-efficient fine-tuning method.
    

The paper is better than previous theoretical work by being the first to specifically analyze LoRA's expressive power for widely used architectures like FNNs and TFNs, going beyond simpler linear models or focusing on other adaptation techniques.

### Drawbacks That Are Discussed in the Paper

- **Optimality of Construction:** The paper notes that their specific theoretical construction of LoRA adapters for FNNs and TFNs, while proving existence, may not be empirically optimal compared to gradient-based methods, particularly in the low-rank regime. This suggests a gap between theoretical existence and practical optimization.
    
- **Analytical Complexity of TFNs:** The theoretical analysis for TFNs is more complex than for FNNs. As a result, the paper only identifies conditions for _exact_ approximation in TFNs and does not quantify the approximation error when the LoRA-rank is below the required threshold, unlike the FNN analysis.
    
- **Simplified TFN Model:** For analytical tractability, the TFN analysis omits skip connections and layer normalization, which are common components in modern Transformer architectures.
    
- **Focus on Expressive Power:** The study focuses _solely_ on the expressive power (what functions a LoRA-adapted model _can_ represent) and explicitly excludes other important aspects like optimization dynamics and generalization performance.
    

### Improvements That Can Be Made

Based on the drawbacks and future work discussed in the paper:

- **Improved LoRA Adapter Construction:** Develop more parameter-efficient theoretical constructions of LoRA adapters for FNNs and TFNs that might lead to tighter bounds on approximation error and potentially align better with empirical optimization results.
    
- **Quantifying TFN Approximation Error:** Extend the theoretical analysis for TFNs to quantify the approximation error when the LoRA-rank is lower than the threshold required for exact representation.
    
- **Analysis on More Complex TFNs:** Incorporate components like skip connections and layer normalization into the theoretical analysis of TFNs to make the results more applicable to state-of-the-art Transformer architectures.
    
- **Analysis of Optimization:** Investigate the optimization landscape and dynamics of training LoRA adapters using gradient-based methods to understand how efficiently the theoretically optimal adapters can be found in practice.
    
- **Analysis of Generalization:** Theoretically study the generalization performance of LoRA to unseen data, which is crucial for understanding its practical success beyond just expressive power.
    
- **Varying Model Dimensions:** While briefly discussed as an extension, a more detailed theoretical analysis for cases where the frozen and target models have different dimensions could provide further insights relevant to practical scenarios where pre-trained models are often larger.