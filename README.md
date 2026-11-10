# ECG-Heartbeat-Categorization-REAL-Dataset

Main results

The test set has 800 normal and 200 abnormal beats, so chance is a PR-AUC of 0.20 and an ROC-AUC of 0.50.

Features	| Detector	ROC-AUC	| PR-AUC	| Recall	| F1
Conventional	| Isolation Forest	| 0.732	| 0.417	| 0.285	| 0.385
Topological	| Isolation Forest	| 0.775	| 0.450	| 0.330	| 0.416
Combined	| Isolation Forest	| 0.783	| 0.470	| 0.345	| 0.444
Conventional	| One-Class SVM	| 0.685 |	0.549	| 0.400	| 0.523
Topological	| One-Class SVM	| 0.734	| 0.463	| 0.255	| 0.363
Combined | One-Class SVM	| 0.729	| 0.490	| 0.305	| 0.419

What this says:

Topology helps the overall ranking. ROC-AUC rises for both detectors when topological features are added (0.732 to 0.775 to 0.783 for Isolation Forest, and 0.685 to 0.734 for One-Class SVM).
It doesn’t consistently help at the top. For Isolation Forest, combined features win on every metric. For One-Class SVM, conventional features have the best PR-AUC, recall and F1.
Overall detection is weak. Even the best model catches only about 35–40% of abnormal beats at about 5% false alarms. That’s modest, and the abnormal class pools four different beat types.
The curves explain the mixed result
Conventional features with One-Class SVM catch some blatant anomalies very precisely: precision about 0.8–0.9 up to roughly a third of the anomalies.
Topological and combined features are better across the wider range. Their ROC curves sit above the conventional curve, and past about 50% recall their precision is higher.
So the two feature types look complementary. Conventional features find the obvious anomalies and topology helps with the harder ones. That’s my reading of the curves, not a tested claim.
What the topology shows
Longer loops in abnormal beats. In the boxplots, abnormal beats have higher H1 total persistence (median about 1.0 against 0.6) and higher maximum persistence (about 0.45 against 0.22).
Lower H1 entropy in abnormal beats (about 1.9 against 2.3), meaning their loop structure is dominated by fewer, larger loops.
Similar H1 loop counts for both classes, so the difference is in loop size, not number.
The Betti curve peaks later for abnormal beats (scale about 0.7 against 0.2–0.4), but the ±1 SD bands overlap heavily. This matches the modest scores.
Which topological representation helps
Persistence statistics and entropy with One-Class SVM was best, with PR-AUC 0.554 and F1 0.490, level with the best conventional result.
Persistence images and landscapes were middling (PR-AUC about 0.41–0.43).
Betti curves with One-Class SVM were weakest (PR-AUC 0.321, recall 0.13).
All 113 topological features together did worse than the 17 statistics alone for One-Class SVM (0.463 against 0.554). High-dimensional features may dilute the signal. That’s a hypothesis I haven’t tested.
Caveats to state
One random sample. With 200 abnormal beats, each AUC has a standard error of roughly ±0.02. Differences of 0.03–0.05 are not firmly established, so the next step is repeating the run over many seeds or bootstrapping.
Picking the best row from the ablation table uses the test set. Present the persistence statistics result as exploratory, not as a final choice.
No patient-level split, because the file has no patient IDs. Beats from one patient can appear in both train and test, so results may look better than they would on new patients.
Pooled abnormal types. The notebook doesn’t show which of the four abnormal types topology helps with.
Draft finding for your notebook

On MIT-BIH beats (2,800 beats sampled, test set 20% abnormal), topological features improved the overall ranking of anomalies for both detectors (ROC-AUC 0.685–0.732 for conventional features against 0.729–0.783 for topological or combined), but did not consistently improve precision-recall performance. Combining both feature types was best for Isolation Forest (PR-AUC 0.470 against 0.417), while conventional features were best for One-Class SVM (PR-AUC 0.549 against 0.463 for topology). Abnormal beats showed longer-lived H1 loops and lower H1 persistence entropy than normal beats. Overall detection performance was modest (recall 0.26–0.40 at about 5% false alarms), and the differences between feature sets are small relative to sampling variability. Persistence statistics and entropy were the most useful topological representation. 

Limitations: single random sample, no patient-level split, pooled abnormal classes.
