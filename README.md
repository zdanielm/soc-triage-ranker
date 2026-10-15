# SOC Triage Ranking

Rank security incidents by likelihood of needing analyst attention, then distill the model's logic into simple, auditable correlation rules.

Built on the [Microsoft GUIDE dataset](https://www.kaggle.com/datasets/Microsoft/microsoft-security-incident-prediction) (real analyst triage labels from Microsoft Defender).

> [!WARNING]
> **Status:** work in progress.

---

## Why this project

As Red Teaming and attacking advanced with the growing use of artificial intelligence, SOC analysts and Blue Teamers face far more alerts than they can process.

This project aims to ask and then answer two questions:

1. **Triage:** Can a model learn from past analyst decisions to rank incidents so that the true positives surface first?
2. **Explainability to rules:** Can we use XAI tools and surrogate models to extract the model's logic into plain rules usable without ML, and how much performance do those distilled rules retain?

> [!IMPORTANT]
> The model **ranks**, but never suppresses. Analysts still keep an eye on everything, but in priority order.

## Dataset

- **Source:** Microsoft GUIDE (Grand Unified Incident Dataset), Kaggle
- **Unit of prediction:** the incident (alerts and evidence aggregated)
- **Target:** `IncidentGrade` (TruePositive / BenignPositive / FalsePositive)

> [!NOTE]
> The data is anonymized, so features come from detector, category, MITRE technique, entity mix, and timing rather than raw log content.

## Planned Approach

| Phase | What happens |
|---|---|
| 1. EDA | Class balance, missingness, alerts per incident and per org |
| 2. Splits and baselines | Time-based split, incident-grouped; majority class, detector-rate heuristic, logistic regression |
| 3. Features | Incident-level counts, MITRE/category encodings, time-aware historical TP rates, temporal and rarity features |
| 4. Modeling | LightGBM, Optuna tuning, probability calibration |
| 5. SOC-style evaluation | Macro-F1, precision/recall, recall at a fixed review budget, workload curves |
| 6. Explainability to rules | SHAP, surrogate decision tree, hand-written correlation rules vs. full model |

### Leakage controls

- Split by time, and keep all rows of an incident in the same fold
- Target and historical-rate encodings are fit on training data only
- Optional hold-out of entire organizations to test generalization to unseen customers

## Results

> [!IMPORTANT]
> This will be updated later

## Repo structure

> [!NOTE]
> This will be updated later

## Tech stack

Python, Polars, scikit-learn, LightGBM, SHAP, Optuna, imodels, matplotlib/seaborn.

## Limitations

- Anonymized data limits feature richness and means rules cannot map directly to raw log fields or Sigma.
- Labels reflect one vendor's analysts and detection stack; results may not transfer to other environments.
- Offline evaluation does not capture analyst behavior changes once a ranking is deployed.
- Concept drift: attacker behavior changes, so a production system would need monitoring and retraining.

***Still it's a great project to complete...***

## Roadmap

- [ ] Phase 1: EDA
- [ ] Phase 2: Splits and baselines
- [ ] Phase 3: Feature engineering
- [ ] Phase 4: Modeling and calibration
- [ ] Phase 5: SOC-style evaluation
- [ ] Phase 6: SHAP to rules
- [ ] Phase 7: Model card and writeup

## Acknowledgments

Dataset: Microsoft Security Incident Prediction (GUIDE). See [the Kaggle page](https://www.kaggle.com/datasets/Microsoft/microsoft-security-incident-prediction) for the license and citation requirements.

## License

MIT, see `License.md`
