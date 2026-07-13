# DataLoca Dataset Preparation Checklist

*For packaging a terrestrial acoustic localization dataset to the DataLoca standard (v0.5).*

Work top to bottom: assemble the required components first, add optional ones as applicable.
---

## 1. Required top-level structure

- [ ] Top-level folder named for the project (`/project_name/`)
- [ ] `readme.md` present at top level
- [ ] `localized_events.csv` present at top level
- [ ] `/localization_metadata/` subfolder with
    - [ ] `point_table.csv`
    - [ ] `audio_file_table.csv`
- [ ] `/audio/` subfolder containing every audio file referenced in the metadata
- [ ] `/scripts/` subfolder with
    - [ ] Scripts used to create data
    - [ ]`environment.yml`


## 2. Event table — `localized_events.csv`
- [ ] Column names and order match files in [templates/](https://github.com/LeonardoViotti/DataLoca/tree/main/templates)
- [ ] Every localized event has exactly one row
- [ ] `event_id` — unique across the *entire* dataset, fixed length, only alphanumerics and underscores
- [ ] `label` — set for every event (matches a `class` value in `classes.csv` if that file is included)
- [ ] `start_timestamp` — ISO format, including UTC offset (e.g. `2022-02-07T20:00:06-05:00`)
- [ ] `duration` — seconds (event length, or fixed clip length if no per-event bounding box)
- [ ] `x`, `y` — estimated event position in **meters**, in the CRS declared in `readme.md`
- [ ] `z` — included only if you have altitude; reference datum described in `readme.md`
- [ ] `file_ids` — list of the files that participated in the localization; each value exists in `audio_file_table.csv`
- [ ] `file_start_time_offsets` — one offset (seconds from file start to clip start) per entry in `file_ids`, same order
- [ ] Optional per-file lists (`tdoas`, `distance_residuals`, `classifier_scores`) — if present, each has one value per `file_id`, in the same order as `file_ids`
- [ ] `round` — included if the same array was collected across multiple rounds

## 3. Microphone location table — `localization_metadata/point_table.csv`
- [ ] Column names and order match files in [templates/](https://github.com/LeonardoViotti/DataLoca/tree/main/templates)
- [ ] One row per deployment point
- [ ] `point_id` — unique; matches the `point_id` values used in `audio_file_table.csv`
- [ ] `x`, `y` — microphone position in **meters** (projected CRS such as UTM, or relative offsets from a reference point), same CRS as the event table
- [ ] `z` — if included, datum described in `readme.md`
- [ ] `array_id` — **required if the dataset contains more than one array**; alphanumeric, spaces/underscores only`
- [ ] `round` — included if recorder positions differed across rounds

## 4. Audio file table — `localization_metadata/audio_file_table.csv`
- [ ] Column names and order match files in [templates/](https://github.com/LeonardoViotti/DataLoca/tree/main/templates)
- [ ] One row per audio file in `/audio/`
- [ ] `file_id` — matches the values used in the `file_ids` column of `localized_events.csv`
- [ ] `relative_path` — resolves to the actual file, relative to the dataset top level (e.g. `audio/aru001/20250101_060000.WAV`)
- [ ] `point_id` — matches a `point_id` in `point_table.csv`
- [ ] `start_timestamp` — recording start time in ISO format
- [ ] Optional keys (`round`, `recorder_id`, `card_id`) added where relevant

## 5. Audio subfolder — `/audio/`

- [ ] Contains every file referenced by `relative_path` in `audio_file_table.csv`
- [ ] Files are synchronized (true nominal sample rate, precise expected start time)
- [ ] Internal folder structure (flat, nested by recorder, nested by round, etc.) matches what the `relative_path` column says — the layout itself is flexible, the paths just have to be truthful

## 6. ReadMe — `readme.md`

- [ ] Describes the study purpose and design
- [ ] States the **coordinate reference system** used for all `x`/`y` (and `z`) columns
- [ ] Explains the datum/reference point for any `z` altitude values
- [ ] Documents the localization procedure, species/sound detection, and audio synchronization method
- [ ] Describes the meaning of `label` values **if `classes.csv` is not included**
- [ ] Describes the `accuracy` metric and how position accuracy was measured or approximated (if accuracy columns are used)
- [ ] Follows the repository `readme.md` template structure

## 7. Scripts — `/scripts/`

- [ ] Includes all scripts/notebooks (`.py`, `.r`, `.rmd`, `.ipynb`) used to produce detections and localizations
- [ ] If there is **no** manual labeling: the scripts can regenerate `localized_events.csv` from the audio
- [ ] If there **is** a hand-labeling/manual-review step: the review workflow is included (review notebook, include/exclude table, etc.)
- [ ] Frozen environment file present (`environment.yml` with pinned versions; e.g. `conda env export -f environment.yml`)

## 8. Optional components — include if applicable

- [ ] `classes.csv` — lists and describes every `label` value used; `class` column values align with `label` in `localized_events.csv`
- [ ] `/observed_events/playbacks.csv` — for playback experiments; positions in the dataset CRS, `label` in `classes.csv`, `start_timestamp` per event (not per file)
- [ ] `/observed_events/observations.csv` — for in-person field observations with known positions; optional `direction` in degrees CW from N
- [ ] Other observation-type files (spot mapping, point counts, transects, focal follows) added under `/observed_events/` as needed


## 09. Formatting conventions

- [ ] **List columns are JSON array strings**, not bare comma lists — e.g. `["R006.WAV","R002.WAV"]`
- [ ] All timestamps are ISO format with an explicit UTC offset
- [ ] All coordinates are in meters and share one CRS, declared in `readme.md`
- [ ] All ID fields (`event_id`, `playback_id`, `observed_event_id`, `array_id`) use only alphanumerics and underscores, with fixed length where the spec requires it
- [ ] Any recorder/round/card keys are used consistently across every table they appear in

## 10. Before release

- [ ] Package validated end-to-end: pick a few events at random and confirm each traces from `localized_events.csv` → `audio_file_table.csv` → the actual file and its `point_id` in `point_table.csv`
- [ ] Standard version noted (this checklist targets **v0.5**)
- [ ] (If registering) dataset added to the DataLoca index of compatible datasets

---

