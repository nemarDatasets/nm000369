# Sensorimotor alpha and beta ECoG during movement imagery (Stolk et al. 2019) - participants S4, S5, S6

## Overview
Electrocorticography (subdural grids/strips, University Medical Center Utrecht) recorded while epilepsy patients imagined
grasping a cylinder with the left or right hand (movement imagery task, 60 trials per session). The paper analysed 9 of 11
implanted participants; the authors' OSF record (https://osf.io/z4hfm/) states: "Due to privacy issues, we are not allowed
to share the data of all the patients reported in our paper. The data of the patients that have explicitly granted
permission to share their data is shared here." Those are participants S4, S5 and S6, released as trial segments.

Paper: Stolk A, Brinkman L, Vansteensel MJ, Aarnoutse E, Leijten FSS, Dijkerman CH, Knight RT, de Lange FP, Toni I (2019).
Electrocorticographic dissociation of alpha and beta rhythmic activity in the human sensorimotor system. eLife 8:e48065.
https://doi.org/10.7554/eLife.48065 . License of the OSF data record: CC-By Attribution 4.0 International.

## Participants / cohort (paper, Methods 'Participants')
- Paper cohort: 11 epilepsy patients (7 males, 14-45 years) implanted subdurally with grid and strip arrays at the University
  Medical Center Utrecht (The Netherlands) to localise the seizure onset zone for surgical resection; arrays on the left
  hemisphere in 8 cases, on the right in 3; 81.3 +/- 11.2 electrodes (mean +/- SEM). All had normal hearing and vision.
  Two participants were excluded for not adhering to the task instructions (9 analysed behaviourally); two had no
  upper-limb sensorimotor coverage (7 analysed neurally). No seizures occurred during task administration.
- This release: S4 (right hemisphere, 112 electrodes), S5 (left, 120), S6 (left, 64); hemisphere and counts from the released
  electrode positions/atlas labels. In the paper, S4 and S5 contributed alpha/beta local-maxima electrodes; S6 had limited
  sensorimotor coverage and only its four stimulation-positive electrodes were used, for temporal dynamics only.
- Per-participant age, sex and handedness are not published (n/a in participants.tsv); recording dates/years are not stated.

## Task (paper, Methods 'Movement imagery task')
Participants lay semi-recumbent in their hospital bed and performed up to three sessions (mean 2 +/- 0.2) of 60 trials (10 min).
A black-white cylinder (17.5 x 3.5 cm), tilted in 1 of 15 orientations (24 deg apart, pseudo-random), was shown for 2-5 s
(adjusted per participant); participants imagined grasping its middle third with the left or right hand (hand alternating every
ten trials, visual cue). A response screen (black and white squares, order pseudo-random) followed, and participants reported
whether the thumb ended on the black or white part by pressing the left or right button with the left or right thumb on a
button box held with both hands; a fixation cross (3-4 s) followed. A control task (same visual input, judge which side of the
cylinder is larger) was completed by 8 of 9 participants; it is not identified in this release.

## Acquisition (paper, Methods 'ECoG acquisition and analysis')
128-channel Micromed system (Treviso, Italy; 22 bits), analog band-pass 0.15-134.4 Hz, sampled at 512 Hz. Ad-Tech subdural grids
and strips (Racine, USA), 10 mm spacing, 2.3 mm exposed diameter. Epochs with overt movements or distracting events were excluded
by the authors (6 +/- 2 % of trials). Electrode localisation: post-implantation CT fused with the pre-operative T1 MRI (Philips 3T
Achieva; CT Philips Tomoscan SR7000), projection to FreeSurfer surfaces (Stolk et al. 2018, https://doi.org/10.1038/s41596-018-0009-6).
Ethics: Medical Ethical Committee of UMC Utrecht, reference 12-075. The paper's own analysis then filtered (1-200 Hz), removed line
noise and re-referenced to the common average; that processing is NOT applied to the shared segments (see below).

## Content, preprocessing and conversion (known caveats)
- sub-S4: 112 ECoG channels, 173 epochs of 2305 samples (-1.5 s to 3.000 s around the trial time zero), 512 Hz
- sub-S5: 120 ECoG channels, 49 epochs of 2049 samples (-1.5 s to 2.500 s around the trial time zero), 512 Hz
- sub-S6: 64 ECoG channels, 180 epochs of 2561 samples (-1.5 s to 3.500 s around the trial time zero), 512 Hz
- The source contains only trial segments (FieldTrip `data.trial`), not continuous recordings. Each participant's segments are
  written back-to-back into one BrainVision file (`RecordingType`: `epoched`; `events.tsv` marks every epoch, its time-zero
  sample and the 9 undocumented `trialinfo` columns verbatim). Do not treat the file as continuous: epoch boundaries are
  discontinuities.
- No filtering, re-referencing or resampling was applied. Values are stored losslessly (round trip exact, values in
  microvolts): sub-S4 BrainVision INT_16 x 0.09765625; sub-S5 BrainVision INT_16 x 0.09765625; sub-S6 no exact integer coding exists (off-grid values, likely resampled by the authors), so the float64 samples are stored losslessly in NWB (acquisition 'ECoG'). **Units:** the .mat files do not state the signal unit
  (FieldTrip `elec.chanunit` 'V' is the toolbox default for sensor definitions). The values are labelled microvolts because
  the recording system is Micromed and the quantum (0.09765625) is the same as in Micromed data with declared microvolt
  units; this is an inference, not a source statement.
- Electrodes: positions from the FieldTrip `elec` structure (mm, coordinate system not named by the source), plus the authors'
  electrode table (11 mm atlas look-ups, discard/epileptic/out-of-brain flags) as extra columns. Channels flagged by the authors
  are `status=bad` in channels.tsv (no channel removed).
- Reference: not reported for the shared segments (n/a).
- `sourcedata/osf-z4hfm/`: the original OSF files, byte-identical: `S*_raw_segmented.mat`, `S*_electable_11mm.xlsx` and
  FreeSurfer pial surfaces `S*_lh.pial`/`S*_rh.pial` (cortical meshes, no facial information).
- Per-participant age/sex are not released (paper: 11 participants, 7 males, 14-45 years): n/a in participants.tsv.

## How to load
```python
import mne_bids
bp = mne_bids.BIDSPath(root=".", subject="S4", task="motorimagery", datatype="ieeg")
raw = mne_bids.read_raw_bids(bp)  # sub-S4/S5 BrainVision; sub-S6 is NWB (read with pynwb)
```
Remember that the files are epoched (`RecordingType` = `epoched`): use `events.tsv` (`sample`, `time_zero_sample`) to cut epochs.

## Citation
Stolk A et al. (2019) eLife 8:e48065, https://doi.org/10.7554/eLife.48065 ; data: https://osf.io/z4hfm/ .

## Provenance
OSF project z4hfm ("Data accompanying Stolk et al. 2019 ...", created 2020-04-21); paper full text PMC6785220. Metadata enriched
2026-10-07 from the paper (participants, task, acquisition, acknowledgements) and from the released electrode tables.
