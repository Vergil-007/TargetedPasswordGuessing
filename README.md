\# TargetedPasswordResearch



A safe, synthetic-data research implementation inspired by the experimental structure of the 2026 PassGLM paper:



> W. Ou et al., "Improving targeted password guessing attacks by using personally identifiable information and old password," Cybersecurity, vol. 9, Art. no. 9, 2026.



\## Safety Boundary



This project generates and uses entirely synthetic research records.



The repository does \*\*not\*\* contain leaked credential dumps, real passwords, real personally identifiable information (PII), or real user records.



Do not replace the generated synthetic data with real credential or PII datasets.



\## Project Overview



This project adapts the experimental concept of PassGLM into a smaller, controlled research environment.



The original paper uses GLM-4-9B with LoRA and a real `hotel\_mix` dataset. This implementation instead uses a lightweight open model and synthetic data so that the complete experimental pipeline can run on a consumer GPU with 6 GB VRAM.



The project investigates how different information conditions affect model performance:



\- PII-only

\- Old Password-only

\- Combined PII + Old Password



\## Experimental Conditions



\### PII Mode



Synthetic PII information is provided as input to predict a synthetic target password.



\### Old Password Mode



A synthetic old password is provided as input to predict a synthetic target password.



\### Combined Mode



Synthetic PII information and a synthetic old password are provided together to predict a synthetic target password.



\## Model



\- Base Model: Qwen2.5-0.5B-Instruct

\- Fine-Tuning: LoRA

\- Quantization: 4-bit NF4

\- Sequence Length: 192

\- Training Epochs: 1

\- Per-Device Batch Size: 1

\- Gradient Accumulation: 16

\- LoRA Rank: 8

\- Learning Rate: 2 × 10⁻⁴



\## Dataset



The project uses a fully synthetic dataset.



\- Total records: 10,000

\- Training records: 8,000

\- Testing records: 2,000

\- Split: 80/20



The synthetic dataset is designed for controlled experimentation and does not represent real users or real leaked credentials.



\## Evaluation Metrics



The models are evaluated using:



\- Top-1 Accuracy

\- Top-5 Accuracy

\- Top-10 Accuracy

\- Top-50 Accuracy

\- Top-100 Accuracy

\- Mean Reciprocal Rank (MRR)



\## Final Results



| Input Mode | Top-1 | Top-5 | Top-10 | Top-50 | Top-100 | MRR |

|---|---:|---:|---:|---:|---:|---:|

| PII | 11.5% | 37.0% | 48.0% | 62.5% | 71.0% | 0.3326 |

| Old Password | 15.5% | 45.0% | 53.0% | 71.5% | 78.0% | 0.3689 |

| Combined | 23.5% | 61.0% | 72.5% | 83.0% | 86.5% | 0.4739 |



The Combined configuration produced the highest measured values across the evaluated Top-K metrics and MRR in this synthetic experiment.



\## Hardware \& Software



Designed and tested around:



\- NVIDIA GTX 1660 Ti — 6 GB VRAM

\- 8 GB RAM

\- Python 3.12

\- Windows 11

\- CUDA-capable PyTorch

\- 4-bit model loading

\- LoRA / QLoRA-style parameter-efficient fine-tuning



\## Project Workflow



```text

Synthetic Dataset

&#x20;       ↓

Data Preprocessing

&#x20;       ↓

Input Mode Selection

&#x20;  ↙       ↓       ↘

&#x20;PII      Old    Combined

&#x20;  ↘       ↓       ↙

Prompt Construction

&#x20;       ↓

Qwen2.5-0.5B

&#x20;       ↓

4-bit NF4 + LoRA

&#x20;       ↓

Candidate Generation

&#x20;       ↓

Top-K Ranking

&#x20;       ↓

Top-K Accuracy + MRR

&#x20;       ↓

Model Comparison

