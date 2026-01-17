# MLLM-Graph-Understanding

Multimodal Large Language Models (MLLMs) are capable of extracting and synthesizing information from various modalities of input (e.g. textual, visual, auditory). Extracting visual information from images of line and bar graphs is particularly challenging due to the high-level of compression, overlapping visuals, and various scales. We benchmarked seven SoTA VLMs and MLLMs on structured and unstructured graphical extraction tasks across various types of graphs, and fine-tuned IBM: Granite Vision 3.3 2B model. Results showed that basic graph extraction tasks such as extrema extraction were easier for models than more fine-trained and ambagious tasks. While pretrained MLLMs often misread domains and axis ranges (e.g., Granite 3.3-2B reaches only 0.29 domain F1), lightweight QLoRA fine-tuning on our annotated graphs raises its domain F1 to 0.84 and drives numeric sMAPE down to small, stable values. Finally, graph type did not significant impact the models' abilities to understand graphs although more visually-dense graphs were more challenging to analyze.

## Summary of Results


\begin{table}[H]
\centering
\small
\setlength{\tabcolsep}{5pt}
\begin{tabular}{lcccccccc}
\toprule
\textbf{Model} &
\makecell[c]{Domain\\F1} &
\makecell[c]{Title\\Lev.\ Dist} &
\makecell[c]{Title\\Lev.\ Sim} &
\makecell[c]{Max\\sMAPE} &
\makecell[c]{Min\\sMAPE} &
\makecell[c]{Lower Bound\\Range sMAPE} &
\makecell[c]{Upper Bounded\\Ranger sMAPE} \\
\midrule
Granite 3.3 2B & \cellcolor{green!20}0.29 & \cellcolor{red!20}2.32 & \cellcolor{red!20}0.96 & \cellcolor{red!20}114.59 & \cellcolor{red!20}10.57 & \cellcolor{red!20}10.95 & \cellcolor{red!20}114.69 \\
Gemma 3 4B   & 0.26 & 0.54 & \cellcolor{green!20}1.00 & 0.04 & \cellcolor{green!20}0.07 & 0.22 & 0.05 \\
Gemma 3 12B  & 0.09 & 0.55 & 0.99 & 0.07 & 0.15 & 0.09 & 0.05 \\

Mistral 3.1 24b   & 0.25 & 0.38 & 1.00 & 0.05 & 0.09 & 0.15 & 0.05 \\
Mistral 3.2 24b   & 0.08 & 0.46 & 1.00 & 0.04 & 0.09 & 0.21 & 0.11 \\
Qwen2.5 72b       & 0.09 & \cellcolor{green!20}0.31 & 1.00 & \cellcolor{green!20}0.03 & 0.08 & 0.09 & 0.06 \\
Gemma 3 27B  & \cellcolor{red!20}0.07 & 0.40 & 1.00 & 0.07 & 0.20 & \cellcolor{green!20}0.02 & \cellcolor{green!20}0.03 \\

\bottomrule
\end{tabular}
\caption{Benchmark results on graph images.}
\label{tab:mllm-graph-bench}
\end{table}
