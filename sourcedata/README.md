## Real-time VIM thalamus recordings during peripheral nerve stimulation treatment for essential tremor: DBS intraoperative dataset

This codebase contains processed microelectrode recordings (MER) from human subjects with essential tremor, along with analysis scripts used to generate the findings published in [Luu et al., 2025](https://doi.org/10.1016/j.brs.2025.06.004).

## Human Subject Study

**Study identifier:** CH1052\
**Number of subjects:** 9 (numbered 02, 03, 06-12)\
**Condition:** Essential tremor\
**Intervention:** Transcutaneous afferent patterned stimulation (TAPS)\
**Recording context:** Intra-operative microelectrode recordings during deep brain stimulation (DBS) surgery

## Getting Started

```
Repository/
├── E-Files/
│   ├── E-CH1052-02.mat
│   ├── E-CH1052-03.mat
│   ├── E-CH1052-06.mat
│   ├── E-CH1052-07.mat
│   ├── E-CH1052-08.mat
│   ├── E-CH1052-09.mat
│   ├── E-CH1052-10.mat
│   ├── E-CH1052-11.mat
│   ├── E-CH1052-12.mat
│   └── stimVbase.mat
├── P-Files/
│   ├── P-CH1052-02.mat
│   ├── P-CH1052-03.mat
│   ├── P-CH1052-06.mat
│   ├── P-CH1052-07.mat
│   ├── P-CH1052-08.mat
│   ├── P-CH1052-09.mat
│   ├── P-CH1052-10.mat
│   ├── P-CH1052-11.mat
│   └── P-CH1052-12.mat
└── PNSxET Root Folder/
    ├── Analysis Scripts/
    │   ├── AssistingFunctions/
    │   │   └── [General utility functions for data processing]
    │   ├── FHC/
    │   │   └── [Functions for handling FHC microelectrode recording system data]
    │   ├── MotionRemoval/
    │   │   └── [Algorithms for removing motion artifacts from neural recordings]
    │   ├── fCWT/
    │   │   └── [Fast cont wavelet transform for time-frequency analysis]
    │   ├── povilaskarvelis-DataViz-3.2.4.0/
    │   │   └── [Data visualization utilities]
    │   ├── Figures_Paper.m
    │   ├── Figures_Spikes.m
    │   └── MER_compare_tetras.m
    └── Data/
        ├── E_spk-Files/
        │   └── stimVbaseSPK.mat
        └── T-Files/
            └── TETRAS Data Log.csv
```

### File Organization Setup

Files are organized into separate zips to reduce individual file size. Before running the scripts, unzip `P-Files` and `E-Files` folders into the `Data` folder within the `PNSxET Root Folder` directory.

### Running Analysis Scripts

* **ROOTpath configuration:** All scripts require setting the `ROOTpath` variable to the path of the 'PNSxET Root Folder' directory before execution
* **Analysis rationale:** The rationale for each analysis is explained in the Methods and Supplementary Methods sections of the manuscript
* **Supporting functions:** All subdirectories of the 'Analysis Scripts' folder contain supporting functions called by the main scripts

### Data Processing Pipeline

```
Raw MER → P-Files → E-Files → Analysis Outputs
         (Signal    (Spectral   (Figures &
          Processing) Analysis)   Statistics)
```

## File Naming Conventions

### P- and E-Files (Neural Data in MATLAB structure .mat)

**Convention:** `Prefix-CH1052-##.mat`

* **P-**: Prefix indicating "Processed" neural data after signal processing and artifact removal
* **E-**: Prefix indicating "Extracted" neural data after binning and quantifying power spectral density estimates
* **CH1052**: IRB study identifier code
* **##**: Subject number (02, 03, 06, 07, 08, 09, 10, 11, 12)
  * Note: Subject numbers 01, 04, and 05 were recruited for the same IRB but for a different project

## Neural Recording Data Files

### P-Files Exemplar: P-CH1052-02.mat

This file contains processed neural data from Subject 02 who received transcutaneous afferent patterned stimulation (TAPS) treatment on the right hand.

**File location for processing:** `PNSxET Root Folder/Data/P-Files/P-CH1052-02.mat`

**Top-level structure:** Two main conditions separating recordings by stimulation state:

* **`base`**: Baseline recordings without TAPS stimulation
* **`stim`**: Recordings during TAPS stimulation

**Data organization:** Each condition (`base` and `stim`) contains a matrix of structures organized as:

* **Rows**: Recording depths (varies by subject, typically 4-5 depths)
* **Columns**: Three recording electrode positions
  * Column 1: Anterior microrecording electrode
  * Column 2: Center microrecording electrode
  * Column 3: Posterior microrecording electrode

**Variables in each recording depth × channel structure (26 fields):**

| Variable       | Description                                       | Units               | Data Type                     |
| -------------- | ------------------------------------------------- | ------------------- | ----------------------------- |
| `raw`          | Raw neural signal timeseries                      | -                   | double array (2880000×1)      |
| `trialTimes`   | Start and end timestamps for trial                | -                   | double (1×2)                  |
| `voltageCalib` | Voltage calibration factor                        | -                   | double scalar                 |
| `timeCalib`    | Time calibration factor                           | -                   | double scalar                 |
| `fs`           | Sampling frequency                                | Hz                  | double                        |
| `recDepth`     | Recording depth identifier (z value in chanCoord) | -                   | char array (1×3)              |
| `chan`         | Channel number                                    | 1, 2, or 3          | double                        |
| `chanCoord`    | Recording electrode coordinates (x, y, z)         | mm                  | double (1×3)                  |
| `implantCoord` | DBS implant coordinates (x, y, z)                 | mm                  | double (1×3)                  |
| `therapyCoord` | Therapeutic contact coordinates (x, y, z)         | mm                  | double (1×3)                  |
| `implantSide`  | Laterality of DBS implant                         | Left or Right       | char array                    |
| `stimFreq`     | TAPS stimulation frequency                        | Hz                  | double, NaN if not applicable |
| `stimMiliAmp`  | TAPS stimulation amplitude                        | mA                  | double                        |
| `lfp`          | Local field potential signal                      | -                   | double array (2880000×1)      |
| `spk`          | Spike-sorted neural activity                      | -                   | double array (2880000×1)      |
| `rawLPCheby`   | Raw signal low-pass filtered (Chebyshev)          | -                   | double array (2880000×1)      |
| `rawBPCheby`   | Raw signal band-pass filtered (Chebyshev)         | -                   | double array (2880000×1)      |
| `spkSTD`       | Spike detection threshold                         | standard deviations | double                        |
| `spkOnset`     | Spike onset times                                 | -                   | cell array (1×1)              |
| `times`        | Time vector for waveforms                         | -                   | cell array (1×1)              |
| `waveforms`    | Spike waveform shapes                             | -                   | cell array (1×1)              |
| `pxx`          | Power spectral density                            | -                   | double array (32769×1)        |
| `f`            | Frequency vector for PSD                          | Hz                  | double array (32769×1)        |
| `blankingMask` | Mask for blanking motion artifact periods         | -                   | logical array (2880000×1)     |
| `raw_blanked`  | Raw signal with artifacts blanked                 | -                   | double array (2880000×1)      |
| `spk_blanked`  | Spike signal with artifacts blanked               | -                   | double array (2880000×1)      |

**Note:** P-Files contain the core processed data, while E-Files extend this with additional binned spectral analysis (`pxx_binned`, `f_binned`, `spkCount`).

### E-Files Exemplar: E-CH1052-02.mat

This file contains spectral analysis data derived from P-CH1052-02.mat for Subject 02. It adds binned power spectral density (`pxx_binned`), binned frequency vector (`f_binned`), and spike counts per bin (`spkCount`) to the P-Files while holding the same organization structure.

**File location for processing:** `PNSxET Root Folder/Data/E-Files/E-CH1052-02.mat`

**Variables in each recording depth × channel structure (29 fields):**

| Variable     | Description                   | Units | Data Type                |
| ------------ | ----------------------------- | ----- | ------------------------ |
| `raw`        | Raw neural signal timeseries  | -     | double array (2880000×1) |
| ...          | ...                           | ...   | ...                      |
| `pxx_binned` | Binned power spectral density | -     | double array (32769×60)  |
| `f_binned`   | Binned frequency vector       | Hz    | double array (32769×60)  |
| `spkCount`   | Spike counts per bin          | -     | double array (1×60)      |

## Tabular Data Descriptions

### TETRAS Data Log.csv

Raw scores from TETRAS (The Essential Tremor Rating Assessment Scale) testing for 9 subjects. Contains pre- and post-stimulation tremor assessments for both treated and untreated limbs.

**File location for processing:** `PNSxET Root Folder/Data/T-Files/TETRAS Data Log.csv`

**Variables:**

| Variable            | Description                   | Units/Scale            | Data Type   |
| ------------------- | ----------------------------- | ---------------------- | ----------- |
| `subject`           | Subject identifier            | IRB code (CH1052-##)   | Categorical |
| `treated_hand`      | Laterality of TAPS treatment  | "right" or "left"      | Categorical |
| `stim`              | Stimulation timing            | "pre" or "post"        | Categorical |
| `measured_hand`     | Which hand was assessed       | "right" or "left"      | Categorical |
| `head_tremor`       | Head tremor severity          | 0-4                    | Ordinal     |
| `face_tremor`       | Facial tremor severity        | 0-4                    | Ordinal     |
| `voice_tremor`      | Voice tremor severity         | 0-4                    | Ordinal     |
| `forward_postural`  | Forward postural tremor       | 0-4                    | Ordinal     |
| `lateral_postural`  | Lateral postural tremor       | 0-4                    | Ordinal     |
| `kinetic_tremor`    | Kinetic (action) tremor       | 0-4                    | Ordinal     |
| `spiral_drawing`    | Spiral drawing task tremor    | 0-4                    | Ordinal     |
| `dot_approximation` | Dot approximation task tremor | 0-4                    | Ordinal     |
| `handwriting`       | Handwriting task tremor       | 0-4; NaN for left hand | Ordinal     |
| `lower_limb`        | Lower limb tremor severity    | 0-4                    | Ordinal     |
| `standing_tremor`   | Standing tremor severity      | 0-4                    | Ordinal     |

**Notes:**

* All tremor scores use the TETRAS ordinal scale: 0 (no tremor) to 4 (severe tremor)
* NaN values in `handwriting` column indicate testing was not performed for dominant left hand
* Each subject has 4 rows: pre/post stimulation × treated/untreated hand

### stimVbase.mat

Processed MI_LFP (Modulation Index for Local Field Potentials) table containing summary measures of neural modulation across all subjects, bands, and recording locations.

**File location for processing:** `PNSxET Root Folder/Data/E-Files/stimVbase.mat`

**Variables:**

| Variable           | Description                                                             | Units/Scale                             | Data Type          |
| ------------------ | ----------------------------------------------------------------------- | --------------------------------------- | ------------------ |
| `subject`          | Subject identifier                                                      | IRB naming (CH1052-##)                  | String/Categorical |
| `fIdx`             | Subject index                                                           | Ordinal numbering (1-9)                 | Integer            |
| `treated_hand`     | Laterality of TAPS treatment                                            | "right" or "left"                       | String/Categorical |
| `treatedAvgDiff`   | Average improvement in treated limb                                     | TETRAS score difference                 | Numeric            |
| `untreatedAvgDiff` | Average improvement in untreated limb                                   | TETRAS score difference                 | Numeric            |
| `tremorReduction`  | Fractional tremor reduction                                             | Dimensionless                           | Numeric            |
| `recIdx`           | Recording location index                                                | Ordinal                                 | Integer            |
| `chanIdx`          | Channel index                                                           | Ordinal                                 | Integer            |
| `trialIdx`         | Trial/attempt index                                                     | Ordinal                                 | Integer            |
| `depth`            | Depth of recording contact from implant target (z-axis)                 | mm                                      | Numeric            |
| `dist2implant`     | Distance of recording contact to final DBS implant location (Euclidean) | mm                                      | Numeric            |
| `band_f`           | Frequency band for LFP analysis                                         | Hz                                      | Numeric array      |
| `band`             | Frequency band name                                                     | "theta", "alpha", "beta", "gamma", etc. | String/Categorical |
| `MI_banded`        | Modulation index for the band                                           | Dimensionless                           | Numeric            |

**Data organization:** Each row represents a unique combination of subject, recording location, channel, trial, and frequency band. Used for analyzing LFP modulation patterns across different brain regions and frequencies.

### stimVbaseSPK.mat

Processed MI_Spikes table containing summary measures of spiking activity modulation, extending stimVbase.mat with spike-specific analysis.

**File location for processing:** `PNSxET Root Folder/Data/E_spk-Files/stimVbaseSPK.mat`

**Variables:** All variables from `stimVbase.mat` plus,

| Variable | Description            | Units/Scale   | Type    |
| -------- | ---------------------- | ------------- | ------- |
| `spkMI`  | Spike modulation index | Dimensionless | Numeric |

## Analysis Scripts

### Main Analysis Scripts

1. **MER_compare_tetras.m**
   * **Purpose:** TETRAS tremor changes analysis
   * **Outputs:** Figures 2A-E; Supplementary Figures S1-3
   * **Data sources:** TETRAS Data Log.csv, stimVbase.mat
2. **Figures_Paper.m**
   * **Purpose:** LFP modulation analysis and visualization
   * **Outputs:** Figures 3A-I; Supplementary Figures S4-S8
   * **Data sources:** E-Files, stimVbase.mat
3. **Figures_Spikes.m**
   * **Purpose:** Spiking activity modulation analysis
   * **Outputs:** Figures 4A-F
   * **Data sources:** stimVbaseSPK.mat

## Contact and References

For questions about this dataset or analysis code, please refer to:

**Luu et al., 2025.** "Wearable peripheral nerve stimulator reduces essential tremor symptoms through targeted brain modulation" (DOI: [https://doi.org/10.1016/j.brs.2025.06.004](https://doi.org/10.1016/j.brs.2025.06.004))

**Repository maintained by:** Cuong P. Luu ([cuong.luu@wisc.edu](mailto:cuong.luu@wisc.edu))


## Human subjects data

All protected health information (PHI) has been purged from the metadata. Informed consent was obtained from all subjects to publish de-identified data in accordance with approved Institutional Review Board protocol.