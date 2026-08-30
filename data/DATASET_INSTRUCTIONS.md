# Dataset instructions

The three datasets used in the paper are all publicly available. This
repository does not re-host them; download each from its original source and
either place under `./data/{name}/` or export a `{NAME}_ROOT` environment
variable.

---

## PhysioMio (48 stroke patients)

Headline dataset. 64-channel HD-sEMG at 2 kHz, bilateral, longitudinal.

- **Publication:** Ilg et al., *PhysioMio: bilateral and longitudinal HD-sEMG
  dataset of 16 hand gestures from 48 stroke patients*, Scientific Data 13:19
  (2026).
- **DOI:** [10.1038/s41597-026-06557-0](https://doi.org/10.1038/s41597-026-06557-0)
- **Env var:** `PHYSIOMIO_ROOT`
- **Default path:** `./data/physiomio/`
- **License:** CC BY 4.0

The per-patient 4-channel selection this paper uses is stored in
`data/physiomio_channel_picks.csv` (included). Every accuracy in the paper
is computed on these four channels, never on the full 64-channel grid.

---

## GrabMyo (43 healthy subjects, 1.14M windows, pretraining source)

The healthy-population EMG corpus whose transfer we test.

- **Publication:** Pradhan, He & Jiang, *Multi-day dataset of forearm and wrist
  electromyogram for hand gesture recognition and biometrics*, Scientific Data
  9:733 (2022).
- **PhysioNet record:** [Gesture Recognition and Biometrics ElectroMyogram
  (GRABMyo), v1.0.1](https://physionet.org/content/grabmyo/1.0.1/)
- **DOI:** [10.13026/701k-gs64](https://doi.org/10.13026/701k-gs64)
- **Env var:** `GRABMYO_ROOT`
- **Default path:** `./data/grabmyo/`
- **License:** CC BY 4.0

---

## Lucchetti (10 stroke patients, external replication)

Independent cohort used for the pre-registered replication (Appendix E).

- **Publication:** Lucchetti et al., *A Kinematic and EMG Dataset for Upper
  Limb and Hand Movement Analysis in Post-Stroke and Healthy Subjects*,
  Scientific Data 12:1904 (2025).
- **DOI:** [10.1038/s41597-025-06174-3](https://doi.org/10.1038/s41597-025-06174-3)
- **Env var:** `LUCCHETTI_ROOT`
- **Default path:** `./data/lucchetti/`
- **License:** CC BY 4.0

---

## Storage footprint

Approximate download sizes (compressed):

| Dataset | ~size |
|---|---|
| PhysioMio | ~4 GB |
| GrabMyo | ~1 GB |
| Lucchetti | ~200 MB |

Total: ~5 GB. Store on any SSD; reproduction runs are compute-bound rather
than I/O-bound.
