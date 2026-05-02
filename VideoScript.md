# CS4168 Data Mining — Video Demo Script
**Target length: ~6 minutes | 4 speakers**

---

## [INTRO — Mark | ~30 sec]

> "Hi, we're Group [X] for CS4168 Data Mining. In this video we'll walk through our analysis of the Spotify Tracks dataset — roughly 2,000 tracks across five genres: pop, indie-pop, synth-pop, R&B, and hip-hop. Our notebook covers four tasks: exploratory data analysis, clustering, classification, and regression, all aimed at understanding and predicting track popularity."

---

## [PART 1 — EDA — Mark | ~1 min]

> "I handled the exploratory data analysis. The most striking finding was the popularity distribution — it's bimodal, with a large spike at zero and a second peak around 50. This spike at zero comes from obscure or unlisted tracks, and it made us flag popularity as a noisy, hard-to-predict target going into modelling.
>
> Looking at correlations, no single audio feature has a strong linear relationship with popularity — the maximum absolute correlation is around 0.10, between loudness and popularity. This told us upfront that a linear model would likely struggle.
>
> The one feature that clearly separates genres is speechiness — hip-hop and R&B tracks cluster at high values, while pop and indie-pop sit near zero. This informed our clustering work and our feature engineering decisions."

---

## [PART 2 — CLUSTERING — Kevin | ~1 min 30 sec]

> "I built the clustering pipelines. We used k-Means and DBSCAN on all audio features after dropping track_genre to avoid leakage.
>
> For k-Means, we tested k from 2 to 10 using the elbow method and silhouette scores. The elbow lands at k=3, which also gives the highest silhouette score of 0.168. We compared it against k=5 — since there are five genres, k=5 was a natural hypothesis — but it scored lower on silhouette (0.158) and the extra clusters didn't map cleanly onto genres. So k=3 was selected.
>
> The three clusters are interpretable: Cluster 0 captures rap and vocal tracks with high speechiness — predominantly hip-hop and R&B. Cluster 1 groups high-energy electronic and pop tracks. Cluster 2 picks up acoustic, lower-energy tracks — indie-pop and softer R&B. They don't perfectly align with genre labels because genres like pop and synth-pop overlap heavily in audio space, but the structure is meaningful.
>
> For DBSCAN, we used a k-NN distance plot to select eps=3.5. The result was degenerate — DBSCAN merged 1,931 of the 1,960 tracks into one giant cluster with a tiny satellite of 21 points. The audio feature space is so continuously dense that DBSCAN can't find separate high-density islands. k-Means k=3 was confirmed as our final solution."

---

## [PART 3 — CLASSIFICATION — Raid | ~1 min]

> "I worked on classification. The target was a binary popularity variable — tracks above the median popularity of 45 are labelled high, below or equal are labelled low. The split is nearly 50/50, so class balance wasn't an issue.
>
> We compared two classifiers: Logistic Regression and Random Forest, both inside full sklearn pipelines with standard scaling and one-hot encoding. We used 5-fold stratified cross-validation with F1 as the selection metric.
>
> Logistic Regression achieved an F1 of 0.63 on the test set — in line with what the weak correlations in EDA predicted. Random Forest with grid-search tuning reached an F1 of 0.745 and accuracy of 75.3%, with a ROC-AUC of 0.826. Random Forest is our final classifier — it captures the non-linear interactions between features that a linear model simply can't."

---

## [PART 4 — REGRESSION — Jason | ~1 min]

> "I handled regression — predicting the raw popularity score rather than the binary label. We kept the original popularity column and compared Ridge Regression against a Random Forest Regressor.
>
> Ridge achieved an RMSE of 28.75 and an R² of just 0.02 — it explains almost none of the variance. This is consistent with what EDA told us: popularity has no strong linear signal.
>
> Random Forest did considerably better: RMSE of 24.4 and R² of 0.294. Still not a strong model — it explains about 29% of the variance — but a significant improvement over Ridge.
>
> Comparing tasks: classification hit 75% accuracy while regression achieved R²=0.29. Predicting exact popularity is substantially harder than predicting high vs low. The bimodal distribution and the zero-popularity spike create a lot of noise that a regression model has to absorb, whereas a binary classifier only needs to get the broad direction right."

---

## [MODELLING DECISIONS — Any speaker | ~1 min]

### Decision that IMPROVED performance:

> "Switching from Logistic Regression to Random Forest for classification was the single biggest performance gain. F1 went from 0.63 to 0.745 — a jump of over 11 points. The EDA had already indicated non-linear structure: popularity doesn't correlate strongly with any individual feature, but combinations of features can still be predictive. Random Forest exploits those interactions; Logistic Regression can't."

### Decision that REDUCED performance:

> "Using Ridge Regression for the regression task was a clear step down. Ridge assumes a linear relationship between features and popularity, but as EDA showed, the maximum correlation is around 0.10. The result was an R² of 0.02 — essentially no explanatory power. Random Forest on the same task scored R²=0.294. Ridge's failure here is a direct consequence of imposing linearity on a fundamentally non-linear problem."

---

## [OUTRO — Any speaker | ~20 sec]

> "To summarise: audio features alone provide a weak but non-zero signal for popularity. Non-linear models outperform linear ones across both classification and regression. Clustering reveals interpretable audio profiles but can't fully recover genre boundaries. Thanks for watching."

---

*Estimated total: ~5 min 30 sec — leaves buffer for screen transitions.*
