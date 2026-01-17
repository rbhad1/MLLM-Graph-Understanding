# MLLM-Graph-Understanding

Multimodal Large Language Models (MLLMs) are capable of extracting and synthesizing information from various modalities of input (e.g. textual, visual, auditory). Extracting visual information from images of line and bar graphs is particularly challenging due to the high-level of compression, overlapping visuals, and various scales. We benchmarked seven SoTA VLMs and MLLMs on structured and unstructured graphical extraction tasks across various types of graphs, and fine-tuned IBM: Granite Vision 3.3 2B model (due to lightweight and computational constraints). Results showed that basic graph extraction tasks such as extrema extraction were easier for models than more fine-trained and ambagious tasks. While pretrained MLLMs often misread domains and axis ranges (e.g., Granite 3.3-2B reaches only 0.29 domain F1), lightweight QLoRA fine-tuning on our annotated graphs raises its domain F1 to 0.84 and drives numeric sMAPE down to small, stable values. Finally, graph type did not significant impact the models' abilities to understand graphs although more visually-dense graphs were more challenging to analyze.

### [FULL PAPER](https://drive.google.com/file/d/19kKn1KiEmAr3S23-mYtwPaNNaUW8ZZy4/view?usp=sharing)


## Summary of Results
### Benchmarking Results
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
sMAPE = (1 / N) * sum_{i=1 to N} ( |y_i - y_hat_i| / R_i )


### Fine-tuned Results: IBM Granite Vision 3.3–2B 
| Metric                  | Baseline (0 epochs) | Fine-tuned (3 epochs) | Fine-tuned (5 epochs) | Fine-tuned (8 epochs) |
|-------------------------|---------------------|------------------------|------------------------|------------------------|
| Domain F1               | 0.290               | 0.836                  | 0.834                  | 0.869                  |
| Title Lev. Distance     | 2.316               | 1.431                  | 0.569                  | 0.409                  |
| Title Lev. Similarity   | 0.961               | 0.980                  | 0.995                  | 0.996                  |
| Max sMAPE               | 114.595             | 0.011                  | 0.016                  | 0.017                  |
| Min sMAPE               | 10.568              | 0.029                  | 0.026                  | 0.018                  |
| Range Lower sMAPE       | 10.948              | 0.006                  | 0.004                  | 0.003                  |
| Range Upper sMAPE       | 114.688             | 0.093                  | 0.081                  | 0.008                  |


Compared to the pretrained Granite Vision 3.3 baseline, the fine-tuning drastically improves domain F1 from 0.29 to 0.84 and reduces the numeric sMAPE values from catastrophic outliers (e.g., 114.59 for the maximum) to small, stable errors (0.011 for the maximum and 0.006 for the lower range bound). The fine-tuned model also halves the average Levenshtein distance (2.32 to 1.43) while slightly increasing title similarity (0.96 to 0.98), indicating more consistent title extraction. Also, fine-tuned Granite now outperforms all other pretrained baselines on both domain and numeric metrics (as seen in Table~\ref{tab:mllm-graph-bench}), although it still occasionally fails on graphs with ambiguous domains or cluttered axes, as discussed in our qualitative analysis below.

We found that the most improvement in performance across all categories was with three epochs, after which there were minute improvements. After five epochs, the domain F1-Score decreased slightly but increased after eight epochs. For all other metrics, three to five to eight epochs consistently improved performance. 

## Conclusion

We investigated how well multimodal LLMs can extract specific information from graph images, focusing on extrema, y-axis ranges, titles, and high-level domains. Our benchmarking results show that pretrained MLLMs reliably recover titles and simple extrema but struggle with domain classification and precise axis range extraction, especially for smaller models such as IBM Granite 3.3--2B. Fine-tuning Granite 3.3--2B with a lightweight QLoRA setup and a strict JSON-style prompting scheme on our annotated graph dataset raised domain F1 from 0.29 to 0.84 and decreased numeric sMAPE to small, consistent values across maximum, minimum, and range bounds, allowing this relatively small open-weight model to match or surpass much larger zero-shot baselines. At the same time, both large pretrained models and our fine-tuned model still struggle on ambiguous domains and graphs with non-zero baselines or cluttered tick labels, showing that chart reading remains a more difficult task than generic image captioning.

Our study has several limitations, such as a small and domain-imbalanced dataset, limited compute resources available (single T4/A100 GPU in Colab), and dataset size. This restricted the number of runs, epochs, and hyperparameter combinations we could explore. Future work could involve benchmarking and fine-tuning on a larger and more balanced dataset, and exploring a more diverse and noisier real-world set of charts (e.g., stacked bars, heatmaps, multi-axis plots, scanned documents). We can also explore performance of downstream tasks, such as interpreting graphs in the context of longer documents or generating correct written summaries of charts.


