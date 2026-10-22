# Receipt: Units n/a; P-File voltageCalib per electrode, unit not documented

**Claim**: no released source states a unit for the P-File field `raw` or for `voltageCalib`, so channels.tsv units are
n/a; voltageCalib is Ant 0.02195717, Cen 0.02259168, Pos 0.02262028 in every non-empty P-File cell.

**PR**: nemarDatasets/nm000436#3; receipt on top of f390815 (PR head 208a65ef + the edit that drops the ADC-count reading).

## Source
- NEMAR nm000436 `origin/main` dc800ce55e614bbac223e105f40dcfc2834cf5df: all 84 `sub-*/ieeg/*_ieeg.eeg` and the 9
  `sourcedata/P-Files/P-CH1052-NN.mat` (fetched from the NEMAR annex); sha256 of each = its git-annex key (84/84, 9/9).
- Upstream: Dryad doi:10.5061/dryad.cjsxksngp. `sourcedata/README_sourcedata.txt`: the P-Files are identical to Dryad
  except the 116-byte MAT text header (HDF5 content unchanged), SHA-256 verified against Dryad.
- Unit statements, all checked at `origin/main`:
  - Dryad README (`sourcedata/README.md`), P-File variable table: `raw` | Raw neural signal timeseries | `-`;
    `voltageCalib` | Voltage calibration factor | `-`.
  - Authors' code, `sourcedata/PNSxET_Root_Folder_.zip` (sha256 c410475623cc8d35b9830babeb920ca7de1f157a939f84c80a1e3259838740b6
    = annex key): `Analysis Scripts/Figures_Paper.m` plots `stim(recIdx, chanIdx).raw(:, trialIdx)` unscaled
    (lines 996-997) under `ylabel(['Channel ' num2str(chanIdx) ' (uV)'])` (line 1021); `FHC/APMReadData.m` line 96
    `vcal = repmat(1.41/32767.0,1,nchan); % default voltage calibration` and line 617 reads the file's value as float32,
    with no unit in either.
  - Article (Luu et al. 2025, doi:10.1016/j.brs.2025.06.004): full text not reachable (publisher and medRxiv 403).
- No source found that states the unit of voltageCalib or how `raw` relates to the acquired ADC samples.

## Method
- h5py 3.16.0 on the MATLAB v7.3 P-Files: `voltageCalib` from every non-empty `base`/`stim` cell; the `raw` trace of each
  run from its `scans.tsv` `source_cell` (trial 1 or 2 of two-trial cells).
- BrainVision `.eeg` read as little-endian float32 (MULTIPLEXED, 3 channels) with `numpy.fromfile`, no scaling, compared
  bitwise with float32 of `raw`.
- On float64 `raw`: mean, standard deviation, fraction of samples that are integers, fraction within 1e-6 of an integer
  multiple of voltageCalib (the ADC-grid check of the converter note kept in the README).
- Measured note only (not stated in the metadata): clipping plateaus, from 100-wide histogram bins beyond +/-25000; a side
  has a plateau if its modal bin holds >= 100 samples and >= 2x the first bin.

## Result
| channel | distinct values | value (8 decimals) | cells | P-Files |
|---|---:|---|---:|---:|
| Ant | 1 | 0.02195717 | 76 | 9 |
| Cen | 1 | 0.02259168 | 76 | 9 |
| Pos | 1 | 0.02262028 | 76 | 9 |

- 252 traces, all 2,880,000 samples; BrainVision = float32(`raw`) for 252/252; max |BrainVision - raw| 0.00195, max
  relative 6.0e-8 (README: relative <= 6e-8, absolute <= 0.002).
- |mean| 4.9e-8 to 7.2e-4; std 472 to 7285; max |mean| / std 6.5e-7 (README amplitude bullet).
- Integer samples: 0 in every trace; samples on the voltageCalib grid: at most 4.5e-6 of a trace. The values are on
  neither grid, as the converter note says, so they are not described as ADC counts.

Clipping plateaus (5 of 252 traces; the PR no longer states them):

| file | cell | channel | + plateau (n) | - plateau (n) | span |
|---|---|---|---:|---:|---:|
| sub-07_task-taps_acq-off_run-01 | base(1,1) trial 1 | Ant | 32150 (298) | -33150 (347) | 65300 |
| sub-07_task-taps_acq-on_run-02 | stim(1,1) trial 1 | Ant | 32050 (245) | -32950 (366) | 65000 |
| sub-07_task-taps_acq-off_run-03 | base(2,1) trial 1 | Ant | 32450 (382) | -33150 (390) | 65600 |
| sub-07_task-taps_acq-on_run-04 | stim(2,1) trial 1 | Ant | 32250 (288) | -33150 (401) | 65400 |
| sub-07_task-taps_acq-on_run-05 | stim(3,1) trial 1 | Ant | 32650 (241) | -32550 (288) | 65200 |

## Coverage
84 of 84 BrainVision files (252 of 252 traces); 9 of 9 P-Files (all 228 non-empty voltageCalib cells).

## Reproduce
```
uv run --no-project --python 3.12 --with h5py --with pandas python \
  code/receipts/2026-10-22_units-na-voltagecalib.py <nm000436 root> <nm000436 git clone> <dir with the 9 P-Files>
```
Slurm job 13449815, 2026-10-22 (h5py 3.16.0, numpy 2.5.4, pandas 3.1.0); output above verbatim.
