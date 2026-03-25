# Contrastive Pretraining Teaches Format Generation, Not Behavioral Knowledge

Code and data for the paper "Contrastive Pretraining Teaches Format Generation, Not Behavioral Knowledge" (Sanchez, 2026).

**Paper:** [arXiv link forthcoming]

## Key Finding

Injecting contrastive behavioral pairs into 5% of training blocks (0.11% of tokens) breaks the behavioral emergence wall: a 7M model exceeds vanilla 34M on bias and sycophancy at 5x fewer parameters. But logit-level analysis reveals the mechanism is format generation enablement, not knowledge creation---every vanilla model from 3M to 64M already achieves 41.0% accuracy when constrained to answer tokens.

## Results Summary

| Finding | Result |
|---------|--------|
| 7M contrastive vs 34M vanilla sycophancy | 0.513 vs 0.300 |
| Optimal injection rate | 5% (non-monotonic) |
| Logit-level accuracy (all vanilla models) | 41.0% (123/300) |
| Deconcentration score separation | Direct > 1.0, Null < 0.16 |
| Cross-transfer onset | d = 96 (readout threshold) |
| Perplexity cost at 7M (5%) | -1.7% (improves LM quality) |
| Perplexity cost at 64M (5%) | +1.0% (negligible) |

## Repository Structure

```
paper/          LaTeX source and bibliography
figures/        Paper figures (PDF)
src/            Experiment code
  configs.py              Model architecture configs (3M--64M)
  train_model.py          GPT-2 pretraining on OpenWebText
  train_contrastive.py    Contrastive variant training (3%, 5%, 10%)
  train_geometric.py      Geometric regularizer (negative control)
  scale_audit.py          Behavioral evaluation + subspace geometry
```

## Reproducing

### 1. Train vanilla and contrastive models
```bash
python src/train_model.py --config 7m
python src/train_contrastive.py --config 7m --rate 0.05 --behaviors bias,sycophancy
```

### 2. Run behavioral audit
```bash
python src/scale_audit.py --model checkpoints/7m_contr_05 --probes data/bbq_300.json
```

## Citation

```bibtex
@article{sanchez2026contrastive,
  title={Contrastive Pretraining Teaches Format Generation, Not Behavioral Knowledge},
  author={Sanchez, Bryan},
  year={2026}
}
```

## License

MIT
