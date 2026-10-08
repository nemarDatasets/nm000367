[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.nm000367-blue)](https://doi.org/10.82901/nemar.nm000367)

Cognitive boundaries and theta phase precession: human microwire LFP (Zheng et al. 2024, DANDI 000940)
======================================================================================================

Overview
--------
Microwire local field potentials (LFP) recorded from Behnke-Fried hybrid depth electrodes in patients with
drug-resistant epilepsy undergoing invasive seizure monitoring at Toronto Western Hospital and Cedars-Sinai
Medical Center, while they performed the cognitive-boundary memory paradigm: encoding of 90 silent movie
clips with no boundary (NB), soft boundaries (SB, cuts within the same movie) or a hard boundary (HB, cut
to a different movie), followed by a scene-recognition test (old/new with confidence) and a
time-discrimination test (which of two frames came first, with confidence).
Recording sites: amygdala, hippocampus and parahippocampal gyrus, and in some participants dorsal anterior
cingulate cortex, pre-supplementary motor area and ventromedial prefrontal cortex.

This dataset is an iEEG-BIDS representation of the LFP released by the authors in NWB format on DANDI:

  Zheng J, Yebra M, Schjetnan A, Patel K, Katz C, Kyzar M, Mosher C, Kalia S, Chung J, Reed C, Valiante T,
  Mamelak A, Rutishauser U (2024). Data for: Hippocampal Theta Phase Precession Supports Memory Formation
  and Retrieval of Naturalistic Experience in Humans. DANDI archive. https://dandiarchive.org/dandiset/000940
  (license CC-BY-4.0). At conversion time (2026-10-06) the Dandiset had no published version; the 16 NWB
  assets were downloaded from the draft and verified against the DANDI SHA-256 digests listed in
  sourcedata/sourcedata_provenance.json.

  Article: Zheng J et al. Theta phase precession supports memory formation and retrieval of naturalistic
  experience in humans. Nature Human Behaviour 8, 2423-2436 (2024). https://doi.org/10.1038/s41562-024-01983-9

Please cite both. Task design: Zheng J et al. Neurons detect cognitive boundaries to structure episodic
memories in humans. Nature Neuroscience 25, 358-368 (2022). https://doi.org/10.1038/s41593-022-01020-w

Relation to other releases: the article states that 19 of its 22 participants come from the 2022 study,
"for whom we previously published single-neuron but not field potential data". Those single-neuron data
are on DANDI as Dandiset 000207 (spike times only, no continuous signal). This release (16 participants)
is the first public release of the field potentials.
Same participants in other releases: the NWB file identifiers carry the lab patient code (e.g. P62CS, TWH101;
column lab_patient_code of participants.tsv). Where the same code appears in another public release with the same
age and sex, participants.tsv names that subject (column same_participant_in): 9 of the 16 participants were also
recorded in the Sternberg working-memory task of DANDI 000673 (Daume et al. 2024; NEMAR nm000368), and sub-8 (P62CS) also took part
in the movie-watching study DANDI 000623 (NEMAR nm000357, sub-CS62). These are different tasks and recordings,
not duplicates. One code (TWH116 here, P116TWH in DANDI 000673) has a different age and sex in the two releases
and is not linked.

Ethics
------
From the article: "This study complies with all relevant ethical regulations. The study protocol was
approved by the Research Ethics Board at Toronto Western Hospital (approval number: 15-5052; approval date:
14 May 2021) and the Institutional Review Board at Cedars-Sinai Medical Center (approval number: Study572;
approval date: 9 June 2020)." Patients "volunteered for this study and provided their informed consent".
This deposit redistributes the publicly released data under its CC-BY-4.0 license.

Contents
--------
16 participants, 16 recordings (one per participant), 388 microwire LFP channels (11-41 per
recording), 200 Hz, 2694-3134 s per recording, 12.99 h in total; 7,200 trial rows and 27,358 TTL markers.

  sub-<label>                  DANDI subject label (sub-2 ... sub-17; one recording per participant).
  ieeg/*_ieeg.vhdr/.vmrk/.eeg  BrainVision, IEEE float32, microvolts, resolution 1.0.
  ieeg/*_channels.tsv          one row per microwire, in the column order of the source series.
  ieeg/*_electrodes.tsv        source coordinates of each microwire (one location per bundle).
  ieeg/*_events.tsv            all trials of the three task parts and all TTL markers (see Events).
  sub-*/sub-*_scans.tsv        recording year, source file, SHA-256 and float32 rounding error.
  sourcedata/dandi-000940/     byte-identical copies of the 16 DANDI NWB files (+ dandiset.yaml);
                               sourcedata/sourcedata_provenance.json lists size, SHA-256, DANDI asset id.

Signal: what was converted and how
----------------------------------
Source: acquisition/LFPs (ElectricalSeries) of each NWB file: float64 values with unit "volts" and
conversion 1e-06 (i.e. the stored numbers are microvolts), offset 0, regular 200 Hz clock starting at
0 s. The NWB description reads "These are LFP recordings that have been downsampled to 200 Hz". The values
were written to BrainVision as float32 microvolts; this is the only change (float32 rounding, at most
0.00049 µV per file, reported per file in scans.tsv). No filtering, resampling, re-referencing, cropping
or channel removal was done by this conversion.

Processing already applied by the authors: broadband signals were recorded at 32 kHz (0.1-8000 Hz, Neuralynx
ATLAS) and downsampled by the authors to the released 200 Hz series; the anti-aliasing filter of that step
is not documented. The article describes, for its own analysis, removal of spike waveforms by linear
interpolation over 3 ms around each detected spike and downsampling to 250 Hz; the released series is
200 Hz and the NWB does not say whether spike interpolation was applied. The source electrodes table
column "filtering" (300-3000 Hz) describes the spike-detection band, not the LFP.
Channel type: BIDS has no microwire channel type; SEEG (depth electrode) is used and each channel is
described as a microwire in channels.tsv. Only the microwires that are present in the source LFP series
are included (11-41 per participant).

Events
------
onset = NWB time - LFP starting_time (0 s in all files; the LFP covers the experiment from the start TTL).
  encoding              one row per clip (intervals/encoding_table): Clip_name, stimCategory, boundary1/2/3_time,
                        fixcross_time, ExperimentID.
  scene_recognition     one row per test frame (intervals/recognition_table): frameName, stimuli_type
                        (target = 1, foil = 0), old_new (response old = 2, new = 1), confidence, accuracy, RT,
                        resp_value, source_response_time (NWB response_time), boundary_type, trial_num, fixcross_time.
  time_discrimination   one row per test pair (intervals/timediscrimination_table): frameName, leftright,
                        resp_key, resp_value, accuracy, confidence, RT, source_response_time, boundary_type, trial_num.
  ttl                   every TTL marker (acquisition/events) with its experiment id (70 encoding,
                        71 recognition, 72 time discrimination).
All source columns are kept with their NWB names (names that BIDS reserves for events columns, here
response_time, get the prefix source_) and the NWB column descriptions are copied into events.json. Times inside source columns (fixcross_time, boundary*_time, source_response_time) are absolute NWB
times; they equal recording time because the LFP starts at 0 s.
Note on stimCategory: the NWB column description says "1=no boundary (NB), 2=soft boundary (SB), 3=hard
boundary (HB)", but the stored values are 0, 1 and 2; the authors' analysis code
(cogboundary-phasepre-release-NWB, B01_raster_psth_encoding_SAligned_BSep_NWB.m) maps 0 = NB, 1 = SB,
2 = HB. The boundary_type columns of the test tables use 1/2/3 as described.
The movie clips are not distributed (copyright); the authors' README gives a download link
(https://github.com/rutishauserlab/cogboundary-phasepre-release-NWB). The NWB OpticalSeries
stimulus/presentation/ExternalVideos is a 200 x 50 x 50 x 3 placeholder ("Please contact authors for clips").

Coordinates
-----------
electrodes.tsv gives the x, y, z of the NWB electrodes table in mm. The article states that electrode
locations were obtained by co-registering post-operative CT with pre-operative MRI (Freesurfer) and that the
MRI was aligned to the CIT168 template in MNI152 coordinates; the coordinate space label is therefore
"Other" with that description (coordsystem.json). All microwires of one bundle share one coordinate.

Participants
------------
Cohort (Zheng et al. 2024, Methods and Supplementary Table 1): 22 patients with refractory (drug-resistant)
epilepsy (13 female; mean age 39 +/- 16 years) implanted with Behnke-Fried hybrid depth electrodes for seizure
monitoring at Toronto Western Hospital and Cedars-Sinai Medical Center; 19 of them were already in Zheng et al.
2022. This release contains 16 of the 22 (one recording each, recorded in 2018 according to the NWB files; see
Known caveats). Not in DANDI 000940: paper participants 1 (P60CS), 18 (TWH129), 19 (TWH138), 20 (P70CS),
21 (P71CS) and 22 (P76CS); P60CS, TWH129 (P129TWH), P70CS, P71CS and P76CS have Sternberg-task LFP in DANDI 000673
(NEMAR nm000368, sub-4, sub-33, sub-12, sub-13, sub-16; same code, age and sex).

participants.tsv columns:
  age, sex, species, recording_institution, dandi_subject_id   NWB general/subject and general/institution.
  lab_patient_code, same_participant_in                         NWB file identifier; links to other releases.
  paper_participant_id, paper_n_neurons_inside_mtl/_outside_mtl Zheng et al. 2024 Supplementary Table 1. Patient
                                                                ID, participant ID, age and gender in that table
                                                                agree with lab_patient_code, dandi_subject_id, age
                                                                and sex for all 16 participants (mapping proof).
  diagnosis, implant_type                                       cohort-level facts from the article Methods.
  seizure_onset_zone                                            Daume et al. 2024 (Nature) Supplementary Table S5,
                                                                for the 9 participants linked to DANDI 000673;
                                                                n/a for the other 7 (Zheng et al. 2022/2024 do
                                                                not report it).
  recording_year                                                year of NWB session_start_time (as scans.tsv).
  n_lfp_channels, lfp_regions, lfp_hemispheres                  derived from channels.tsv of this release.
Handedness, epilepsy duration/onset age, etiology and medication are not reported by the sources (n/a).
Recording year (scans.tsv) is the year of the NWB session_start_time, which the authors set to 1 January of
the recording year to avoid disclosure of protected health information.

Not converted (available unchanged in the NWB files under sourcedata/ and on DANDI)
------------------------------------------------------------------------------------
Spike-sorted single units (units table: spike times, electrodes, boundary_cell_flag, event_cell_flag,
phase_precession_cell_flag) and the stimulus placeholder.

Conversion checks
-----------------
- Every BrainVision file was read back with MNE-Python and compared with the source NWB: channel names and
  order equal the NWB electrode region, sampling rate and sample count equal, every sample equals the
  float32 representation of the source value (largest absolute difference to the float64 source
  0.00049 µV, largest relative difference 6e-8), no non-finite values.
- Every interval row and TTL marker was recomputed from the NWB (onset = time - starting_time); all match
  events.tsv within 1 µs (rounding to 6 decimals); no event lies outside the recording.
- The NWB copies in sourcedata/ match the DANDI SHA-256 digests.
- bids-validator 3.0.2: 0 errors; warnings only for recommended fields the source does not document.

Conversion code: b2dandi_rutishauser_bids.py (iEEG-NEMAR campaign, batch 2), using h5py and pybv.

Known caveats
-------------
- Recording year: all 16 NWB files state 2018-01-01, whereas DANDI 000673 dates the Sternberg sessions of 7 of
  the 9 shared patients to 2019 (P61CS, P62CS, P64CS, TWH109, TWH110, TWH113) or 2020 (P65CS). In
  participants.tsv recording_year is n/a for those 7; for the others the 2018 value is kept as released (unverified).
- Numbering: Zheng et al. 2022 (Supplementary Table 2) numbers TWH113 as 8 and P62CS as 9; Zheng et al. 2024 and
  this release use 8 = P62CS and 9 = TWH113. Use lab_patient_code to match the 2022 single-neuron release
  (DANDI 000207).
- stimCategory values 0/1/2 versus the NWB description 1/2/3 (see Events); the LFP anti-aliasing filter and
  whether spike interpolation was applied are not documented (see Signal); the movie clips are not distributed.
- One code (TWH116 here, P116TWH in DANDI 000673) has a different age and sex in the two releases and is not
  linked.

How to load
-----------
  from mne_bids import BIDSPath, read_raw_bids
  bp = BIDSPath(root="nm000367", subject="2", task="cogboundary", datatype="ieeg")
  raw = read_raw_bids(bp)            # 200 Hz microwire LFP in microvolts (MNE stores volts)
  events = raw.annotations           # trials and TTL markers from events.tsv

Citation
--------
Zheng J, Yebra M, Schjetnan AGP, Patel K, Katz CN, Kyzar M, Mosher CP, Kalia SK, Chung JM, Reed CM, Valiante TA,
Mamelak AN, Kreiman G, Rutishauser U. Theta phase precession supports memory formation and retrieval of
naturalistic experience in humans. Nature Human Behaviour 8, 2423-2436 (2024). doi:10.1038/s41562-024-01983-9
and the data: DANDI:000940 (https://dandiarchive.org/dandiset/000940).

Provenance of the 2026-10-07 metadata enrichment
------------------------------------------------
Zheng et al. 2024 Supplementary Information (Supplementary Table 1); Zheng et al. 2022 Nat Neurosci
Supplementary Information (Supplementary Table 2); Daume et al. 2024 Nature Supplementary Information
(Supplementary Table S5, doi:10.1038/s41586-024-07309-z); the release itself (channels.tsv, scans.tsv).
