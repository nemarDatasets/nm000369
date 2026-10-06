# Sensorimotor alpha and beta ECoG during movement imagery (Stolk et al. 2019) - participants S4, S5, S6

Electrocorticography (subdural grids/strips, University Medical Center Utrecht) recorded while epilepsy patients imagined
grasping a cylinder with the left or right hand (movement imagery task, 60 trials per session). The paper analysed 9 of 11
implanted participants; the authors' OSF record (https://osf.io/z4hfm/) states: "Due to privacy issues, we are not allowed
to share the data of all the patients reported in our paper. The data of the patients that have explicitly granted
permission to share their data is shared here." Those are participants S4, S5 and S6, released as trial segments.

Paper: Stolk A, Brinkman L, Vansteensel MJ, Aarnoutse E, Leijten FSS, Dijkerman CH, Knight RT, de Lange FP, Toni I (2019).
Electrocorticographic dissociation of alpha and beta rhythmic activity in the human sensorimotor system. eLife 8:e48065.
https://doi.org/10.7554/eLife.48065 . License of the OSF data record: CC-By Attribution 4.0 International.

## Recording (paper)
128-channel Micromed system (22 bits), analog band-pass 0.15-134.4 Hz, sampled at 512 Hz. Ad-Tech subdural grids and strips,
10 mm spacing, 2.3 mm exposed diameter. Epochs with overt movements or distracting events were excluded by the authors
(6 +/- 2 % of trials). Ethics: Medical Ethical Committee of UMC Utrecht, reference 12-075.

## Content and conversion
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
