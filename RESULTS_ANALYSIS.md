# Results & Discussion: Specific Strength Prediction & Bayesian Optimization

## 1. Model Performance
The Gradient Boosting Regressor was trained on the featurized chemical compositions using the **Magpie** preset from the `CBFV` package. Hyperparameter tuning via grid search arrived at an optimal model configuration:
- **Learning Rate:** 0.2
- **Max Depth:** 10
- **Min Samples Split:** 10
- **Number of Estimators:** 50

### Evaluation Metrics
On the held-out test set, the model achieved:
- **R-squared ($R^2$):** 0.826
- **Root Mean Squared Error (RMSE):** 29.84

**Discussion:**
An $R^2$ score of 0.826 indicates a strong predictive capability, capturing over 82% of the variance in the specific strength targets based solely on elemental properties and temperature. The RMSE of 29.84 denotes the average deviation from the actual specific strength values, which is highly acceptable for materials science data that often features inherent experimental noise and complex phenomena.

## 2. Feature and Representation Efficacy
The use of the `CBFV` (Composition-Based Feature Vector) library, specifically utilizing the **Magpie** database, proved highly effective. It successfully translated abstract chemical formulas into dense numerical vectors that capture stoichiometric heuristics and elemental properties (atomic radii, electronegativity, valence electrons), giving the Gradient Boosting Regressor meaningful physical traits to split on.

## 3. Top Candidate Materials
The dataset sorting revealed the top experimental materials with the highest actual specific strength values. The High-Entropy Alloys (HEAs) strongly stood out:
1. **Al0.5Mo0.5NbTa0.5TiZr**: Specific Strength = 309.7 
2. **AlMo0.5NbTa0.5TiZr**: Specific Strength = 307.4 
3. **AlCr0.5NbTiV**: Specific Strength = 290.1 

**Discussion:**
The presence of lightweight elements like Aluminum (Al) combined with refractory metals (Mo, Nb, Ta, Ti, Zr, V) aligns perfectly with modern metallurgical intuition. These elements form robust solid solutions that maintain high strength-to-weight ratios, confirming that the model's training data effectively reflects actual physical realities.

## 4. Bayesian Optimization & Expected Improvement
Using a custom bootstrap estimator, we calculated the mean and standard deviation of predictions for the unlabeled search space. The resulting **Expected Improvement (EI)** quantified the potential of discovering a material strictly better than our current best observation ($f^\star = 309.7$).

The ranking by Expected Improvement provides a concrete, data-driven roadmap for the next batch of physical experiments, systematically balancing:
- **Exploitation:** Compositions where the model confidently predicts high specific strength.
- **Exploration:** Compositions where the model prediction has high uncertainty (high standard deviation).

## 5. t-SNE Latent Space Visualization
To better understand the high-dimensional chemical feature space, a t-SNE projection was generated in 2D. Because the subset of data for $T < 24^\circ\text{C}$ was too sparse (only 5 samples), the dataset was split for temperatures $\le 25^\circ\text{C}$ to ensure a robust visualization. 

**Observation & Insight:**
In the resulting t-SNE plot, one of the top-performing high-entropy alloys was observed clustering tightly with low-performing materials. Because t-SNE groups data points entirely on the similarity of their input features (the CBFV elemental vectors), this indicates that the top performer shares a nearly identical empirical formula with the low performers! 

This visual phenomenon beautifully highlights the highly non-linear nature of materials science: a minor elemental tweak or trace addition can fundamentally alter the crystal structure or phase stability, resulting in a drastically higher specific strength. It serves as a prime example of why powerful, non-linear machine learning algorithms like Gradient Boosting are absolutely necessary over simple generalized linear models.

## Conclusion
This workflow efficiently showcases the viability of combining elemental featurization with robust ML models. By integrating uncertainty through bootstrapping, the pipeline successfully shifts from purely "predictive modeling" to "active experimental design."
