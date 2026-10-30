# Receipt: BrainVision headers say n/a, committed as plain git text files

**Claim**: the 84 `.vhdr` headers, annexed with unit µV at `origin/main`, are plain git text files at the PR head whose
channel units are n/a, as in channels.tsv. Only the unit fields and the converter comment line differ from the annexed
files; no `.eeg` or `.vmrk` file changes.

**PR**: nemarDatasets/nm000436#3, header commit `3086f96d0d4aff4e70a4e9353091e7e45a37d8be` (the receipt commit adds
only this receipt's two files).

## Source
- NEMAR nm000436 `origin/main` `dc800ce55e614bbac223e105f40dcfc2834cf5df`: the 84 annexed `*_ieeg.vhdr`, fetched from the
  NEMAR annex (public S3 prefix `nemar/nm000436/objects/`); the sha256 of each equals its git-annex key (column 2).
- PR head: the 84 headers (git blobs) and the channels.tsv and `_ieeg.json` of the 84 runs, read with `git show`.
- Mechanism as in nemarDatasets/nm000509 `432a7a42`: the annex pointer of the `.vhdr` is replaced by the header text, a
  plain git blob (mode 100644); no `.gitattributes` change. Here the files were added with
  `git -c annex.largefiles=nothing add`.

## Method
- Header text: the annexed file with the two edits that `code/b3d_convert_vim.py` makes at this head: the unit field of
  every `Ch<n>=` record `µV` -> `n/a`, and in the comment `field 'raw' (float64 µV)` -> `field 'raw' (float64, unit not
  documented)`.
- `2026-10-30_vhdr-plain-git.py <clone> <PR head> <dir with the origin/main headers>` (Python 3.12.15, pandas 3.1.0),
  per header: annexed at `origin/main` with sha256 = key; plain git blob with mode 100644 at the head; changed lines
  (difflib, classed as unit field, comment, SamplingInterval or other); header units vs channels.tsv units per channel
  (same names in the same order); header rate (1e6 / SamplingInterval) vs `_ieeg.json` SamplingFrequency and
  channels.tsv sampling_frequency; `.eeg` (annex pointer) and `.vmrk` (git text) blobs at the head equal to those at
  `origin/main`.

## Result (script output)

| recording | origin/main: annexed, sha256 = key | head | lines changed | channels | units header / channels.tsv | units agree | rate header / ieeg.json / channels.tsv (Hz) | rate agrees | .eeg and .vmrk as on origin/main |
|---|---|---|---|---:|---|---:|---|---|---|
| sub-02_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-off_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-off_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-off_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-off_run-09 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-on_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-on_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-on_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-on_run-08 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-on_run-10 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-02_task-taps_acq-on_run-11 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-off_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-off_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-off_run-08 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-off_run-11 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-09 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-10 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-12 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-13 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-03_task-taps_acq-on_run-14 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-off_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-off_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-off_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-on_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-on_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-on_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-on_run-08 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-06_task-taps_acq-on_run-09 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-07_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-07_task-taps_acq-off_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-07_task-taps_acq-off_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-07_task-taps_acq-on_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-07_task-taps_acq-on_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-07_task-taps_acq-on_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-07_task-taps_acq-on_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-off_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-off_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-off_run-08 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-on_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-on_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-on_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-on_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-08_task-taps_acq-on_run-09 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-09_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-09_task-taps_acq-off_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-09_task-taps_acq-off_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-09_task-taps_acq-on_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-09_task-taps_acq-on_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-09_task-taps_acq-on_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-09_task-taps_acq-on_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-off_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-off_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-off_run-08 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-on_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-on_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-on_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-on_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-10_task-taps_acq-on_run-09 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-off_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-off_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-off_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-on_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-on_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-on_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-on_run-08 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-on_run-09 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-11_task-taps_acq-on_run-10 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-12_task-taps_acq-off_run-01 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-12_task-taps_acq-off_run-02 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-12_task-taps_acq-off_run-04 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-12_task-taps_acq-off_run-07 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-12_task-taps_acq-on_run-03 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-12_task-taps_acq-on_run-05 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-12_task-taps_acq-on_run-06 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |
| sub-12_task-taps_acq-on_run-08 | True True | plain git | comment 1, unit 3 | 3 | n/a / n/a | 3/3 | 48000 / 48000 / 48000 | True | True |

Totals: {'headers': 84, 'plain': 84, 'key_ok': 84, 'names_ok': 84, 'rate_ok': 84, 'data_same': 84, 'channels': 252, 'units_ok': 252, 'changed': 84, 'comment': 84, 'unit': 252}

Files changed by the PR (origin/main...head): 174; by suffix: {'CHANGES': 1, '.md': 2, '.py': 2, '.json': 1, '.tsv': 84, '.vhdr': 84} (at the header commit; no `.eeg` or `.vmrk` among them.)

All 84 headers are plain git text at the head with n/a in all 252 channel records, matching the 252 channels.tsv rows;
the only changed lines are the 252 unit fields and the 84 comment lines; the rates are unchanged (48000 Hz, as in
`_ieeg.json` and channels.tsv); the 84 `.eeg` (same annex keys) and 84 `.vmrk` are unchanged from `origin/main`.

## Coverage
84 of 84 headers (252 of 252 channel records), 84 of 84 channels.tsv and `_ieeg.json`, 84 of 84 `.eeg` and `.vmrk`.

## Reproduce
```
uv run --no-project --python 3.12 --with pandas python code/receipts/2026-10-30_vhdr-plain-git.py \
  <nm000436 git clone> 3086f96d0d4aff4e70a4e9353091e7e45a37d8be <dir with the 84 origin/main headers>
```
Run on 2026-10-30 (header text only, no signal data read); the table is verbatim from its output.
