# LLM TRAINING CHEAT SHEET

## Optimised for ModernBERT / BigBird / BERT on small GPUs

### 1. Model Size & Memory

Memory ≈ Parameters × 2 bytes (BF16) × 3 (Weights + gradients + optimizer)

| Model             | Params | Training Memory | Notes                |
|-------------------|--------|------------------|----------------------|
| BERT base         | 110M   | 3–4 GB           | Easy                 |
| BigBird base      | 127M   | 4–5 GB           | Sparse attention     |
| ModernBERT base   | 150M   | 4–6 GB           | Fast, long context   |
| ModernBERT large  | 400M   | 12–16 GB         | Too big for 4 GB     |
| LLaMA 7B          | 7B     | 40–60 GB         | Multi GPU only       |


GPU (4 GB) -> ModernBERT base, BigBird base, BERT base, or LoRA.
GPU (8 GB) → ModernBERT‑base, BigBird‑base, BERT‑base, ModernBERT‑large (if available), or LoRA.

BF16 = 16bit floating point (bits for exponent is 8, and 7 for mantissa. So stable as can handle bigger numbers, but at less precision). FP16 it's 5 (and 10 bits for mantissa so better precision). Both use the same memory.

### 2. Batch Size

Batch size × Sequence length × Model size ≈ GPU memory.

Recommended (4 GB GPU, or, 8 GB GPU):
| Model            | Seq Len (4 GB) | Batch (4 GB) | Seq Len (8 GB)   | Batch (8 GB) |
|------------------|----------------|--------------|-------------------|--------------|
| BERT base        | 512            | 8–16         | 1024–2048         | 16–32        |
| BigBird base     | 2048           | 2–4          | 4096              | 4–8          |
| ModernBERT base  | 4096–8192      | 1–2          | 8192–16384        | 2–4          |
| SHAP             | 512            | 8            | 1024              | 16           |


Gradient accumulation: effective_batch = batch_size × grad_accumulation_steps Example: batch=2, accum=8 → effective batch=16.


### 3. Sequence Length

Sequence length is the main driver of memory usage.

| Seq Len | Memory Multiplier |
|---------|-------------------|
| 512     | 1×                |
| 1024    | 2×                |
| 2048    | 4×                |
| 4096    | 8×                |
| 8192    | 16×               |

Training: 2048 Inference: 4096–8192 SHAP: 512


### 4. Learning Rate

For classification:

| Model               | LR    | Reason (3 words)        | How it works (1 sentence)                                      |
|---------------------|-------|--------------------------|------------------------------------------------------------------|
| BERT base           | 2e-5  | Stable general baseline  | Uses a moderate LR that avoids overshooting during fine‑tuning.  |
| BigBird             | 2e-5  | Efficient long context   | Matches BERT’s LR but keeps sparse attention training stable.    |
| ModernBERT base     | 1e-5  | Larger, needs caution    | Lower LR prevents instability due to deeper, wider architecture. |
| LoRA adapters       | 2e-4  | Fast lightweight tuning  | Higher LR trains small adapter layers quickly without harming base weights. |
| Classification head | 3e-4  | Simple small layer       | Small linear head can safely learn fast with a higher LR.        |




| Model              | LR    |
|--------------------|-------|
| BERT base          | 2e-5  |
| BigBird            | 2e-5  |
| ModernBERT base    | 1e-5  |
| LoRA adapters      | 2e-4  |
| Classification head| 3e-4  |


5. Stability Settings

| Setting                     | Value/Action                 | Reason                   | How it works                                                  |
|-----------------------------|------------------------------|--------------------------|---------------------------------------------------------------|
| bf16                        | True                         | Stable low precision     | Uses 16‑bit floats with large range to prevent training NaNs. |
| Gradient checkpointing      | True                         | Saves GPU memory         | Recomputes intermediate activations on the fly to reduce VRAM. |
| Warmup steps                | 300                          | Smooth start-up          | Gradually increases learning rate to avoid early instability. |
| Max grad norm               | 1.0                          | Prevents gradient spikes | Clips gradients so updates never exceed a safe magnitude.     |
| Optimizer                   | AdamW (Torch)                | Modern stable default    | Applies decoupled weight decay for more stable convergence.   |
| Early stopping              | Enabled                      | Avoids overfitting       | Stops training when validation stops improving.               |
| Freeze lower layers         | Yes (small data)             | Preserve base knowledge  | Locks foundational layers so only higher layers update.       |
| Train top + LoRA            | Yes (small data)             | Efficient adaptation     | Adds small trainable adapters to top layers for fast tuning.  |



6. LoRA / QLoRA
Use LoRA when:
| Item                | Value/Action                          | Reason (3 words)        | How it works (1 sentence)                                      |
|---------------------|----------------------------------------|--------------------------|------------------------------------------------------------------|
| LoRA usage          | Dataset < 10k                          | Small data efficiency    | Trains tiny adapter layers instead of full model weights.        |
| LoRA usage          | GPU < 8 GB                             | Fits small GPUs          | Reduces trainable parameters so VRAM stays low.                  |
| LoRA usage          | Fast, stable training                  | Quick safe tuning        | Avoids full fine‑tuning instability by modifying only key layers.|
| LoRA r              | 8                                      | Balanced rank size       | Controls adapter bottleneck dimension for efficient updates.     |
| LoRA alpha          | 16                                     | Stable scaling factor    | Scales LoRA updates to maintain training stability.              |
| LoRA dropout        | 0.05                                   | Prevents overfitting     | Adds small dropout inside adapters to improve generalisation.    |
| LoRA target modules | query, value                           | Key attention paths      | Modifies attention projections where task‑specific signals live. |
| QLoRA               | 4‑bit quantization                     | Extreme memory savings   | Stores weights in 4‑bit and uses double quantization for stability. |
| QLoRA               | Fits 7B on 8–12 GB                     | Enables large models     | Compresses model so fine‑tuning fits consumer GPUs.              |
| QLoRA               | Not needed for ModernBERT              | Already lightweight      | ModernBERT base is small enough to train without quantization.   |

### 7. Speed Optimisation
Enable:
| Setting/Action            | Value/Action            | Reason                   | How it works                                                     |
|---------------------------|-------------------------|--------------------------|------------------------------------------------------------------|
| bf16                      | Enable                  | Faster stable compute    | Uses tensor cores efficiently with safe 16‑bit precision.        |
| Gradient checkpointing    | Enable                  | Saves GPU memory         | Recomputes activations instead of storing them.                  |
| Dataloader workers        | 4                       | Faster loading           | Loads batches in parallel using multiple CPU workers.            |
| Optimizer                 | adamw_torch             | Stable modern default    | Uses decoupled weight decay for smoother convergence.            |
| Reduce sequence length    | Lower tokens            | Biggest speed gain       | Cuts quadratic attention cost, reducing compute massively.       |
| Reduce logging frequency  | Fewer logs              | Less overhead            | Minimises CPU/GPU sync points during training.                   |
| Reduce epochs             | Shorter training        | Avoids wasted compute    | Stops early once validation stabilises.                          |
| Increase accumulation     | Higher accumulation     | Simulates big batches    | Accumulates gradients across steps to mimic larger batch sizes.  |

### 8. Evaluation
Use:
| Metric/Tool        | Purpose                  | Reason                    | How it works                                                     |
|--------------------|---------------------------|--------------------------|------------------------------------------------------------------|
| Accuracy           | Overall correctness       | Simple global metric     | Measures proportion of correct predictions.                      |
| Macro F1           | Balanced class score      | Handles imbalance        | Computes F1 per class then averages equally.                     |
| Confusion matrix   | Error breakdown           | Shows misclass patterns  | Displays counts of predicted vs actual labels.                   |
| SHAP               | Feature importance        | Explains predictions     | Computes token‑level contribution scores for each prediction.    |
| Attention heatmaps | Visual attention          | Interpret model focus    | Shows which tokens receive highest attention weights.            |

Batch Size (Explanation)
| Batch Type     | Effect                      | Reason                   | How it works (1 sentence)                                        |
|----------------|-----------------------------|--------------------------|------------------------------------------------------------------|
| Large batch    | Faster, stable gradients    | Smooth gradient updates  | Processes many sequences at once, reducing noise in updates.     |
| Small batch    | Fits long sequences         | Lower memory use         | Uses fewer sequences per step, allowing longer token lengths.    |
| Grad accumulation | Simulates large batch    | Works on small GPUs      | Accumulates gradients over multiple steps before updating.       |

Attention Mechanisms (Explanation)
| Model/Type          | Pattern                         | Reason (3 words)        | How it works (1 sentence)                                      |
|---------------------|----------------------------------|--------------------------|------------------------------------------------------------------|
| BERT (standard)     | Full attention matrix            | Simple dense attention   | Computes all token‑to‑token interactions (O(n²)).                |
| BigBird             | Sparse blocks + global tokens    | Efficient long context   | Uses block sparse patterns to reduce cost to O(n).               |
| ModernBERT          | Hybrid local + global            | Fast long context        | Combines sliding windows + global tokens without sparse blocks.  |

One Page Summary of parameters for Model: ModernBERT
- base Batch: 2
- Accumulation: 8
- Effective batch: 16 
- Seq length: 2048 (train), 4096–8192 (infer), 512 (SHAP)
- LR: 1e 5
- Stability: bf16, warmup 300, grad clip 1.0 
- Speed: reduce seq length, increase accumulation

