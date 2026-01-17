# MLLM-Graph-Understanding

Multimodal Large Language Models (MLLMs) are capable of extracting and synthesizing information from various modalities of input (e.g. textual, visual, auditory). Extracting visual information from images of line and bar graphs is particularly challenging due to the high-level of compression, overlapping visuals, and various scales. We benchmarked seven SoTA VLMs and MLLMs on structured and unstructured graphical extraction tasks across various types of graphs, and fine-tuned IBM: Granite Vision 3.3 2B model (due to lightweight and computational constraints). Results showed that basic graph extraction tasks such as extrema extraction were easier for models than more fine-trained and ambagious tasks. While pretrained MLLMs often misread domains and axis ranges (e.g., Granite 3.3-2B reaches only 0.29 domain F1), lightweight QLoRA fine-tuning on our annotated graphs raises its domain F1 to 0.84 and drives numeric sMAPE down to small, stable values. Finally, graph type did not significant impact the models' abilities to understand graphs although more visually-dense graphs were more challenging to analyze.

## Summary of Results
| Model              | Domain F1 | Title Lev. Dist | Title Lev. Sim | Max sMAPE | Min sMAPE | Lower Bound Range sMAPE | Upper Bounded Range sMAPE |
|--------------------|-----------|-----------------|----------------|-----------|-----------|--------------------------|---------------------------|
| Granite 3.3 2B     | **0.29**      | 2.32            | 0.96           | 114.59    | 10.57     | 10.95                    | 114.69                    |
| Gemma 3 4B         | 0.26      | 0.54            | **1.00**           | 0.04      | **0.07**      | 0.22                     | 0.05                      |
| Gemma 3 12B        | 0.09      | 0.55            | 0.99           | 0.07      | 0.15      | 0.09                     | 0.05                      |
| Mistral 3.1 24B    | 0.25      | 0.38            | 1.00           | 0.05      | 0.09      | 0.15                     | 0.05                      |
| Mistral 3.2 24B    | 0.08      | 0.46            | 1.00           | 0.04      | 0.09      | 0.21                     | 0.11                      |
| Qwen2.5 72B        | 0.09      |** 0.31**            | 1.00           | **0.03**      | 0.08      | 0.09                     | 0.06                      |
| Gemma 3 27B        | 0.07      | 0.40            | 1.00           | 0.07      | 0.20      | **0.02**                     | **0.03**                      |

Symmetric Mean Absolute Percentage Error (sMAPE) 
sMAPE = (1 / N) · Σ(i = 1 → N) |yᵢ − ŷᵢ| / Rᵢ

