# Dataset_title  
Creators: [fill in] Include email for corresponding author  

Affiliations: [fill in]  

Version number [v1] (update any time the contents change)  

DOI:  

## General characteristics  
audio format: 10 second clips centered on 3 second localized events  
dimensions localized: 2  
number of localization arrays: 13  
array geometry: 5x10 square grid with 33m spacing  
sounds localized: Ovenbird songs  
number of audio files: 4025  
size: 0.375 GB  

audio_format common approaches:  
audio files from a consistent, pre-determined time periods (e.g. 10 minutes at 9am on June 1) are extracted from all recorders. The localized events occur within these audio files.  
the included audio clips have been extracted from each localized event for each recorder (or a subset)  


## Study description

Study purpose: describe the purpose of the study, original intended use of the data if known, any species intended to be targeted  


Data types collected: [List the types of data collected, e.g.: audio recordings, point counts, focal follows, individual bird positions, spot mapping, playback data]  

Notes:  
Anything else about the study site, for instance, if a particular species or behavior was observed that is relevant to the analysis  
Describe anything noteworthy or unusual that should be considered during analysis of this data.   

## Files  
This section describes the contents of each file (or file type when multiple files of the same type are included). Default descriptions can be copied if applicable. Add descriptions for any additional file types, e.g. if you include a vegetation survey or a spotmapping survey. We provide examples here:  

localized_events.csv: table with a row describing each acoustically localized sound   
- Columns:
	- event_id: unique within the dataset  
	- label: species or other sound type name (e.g. “playback”) of the localized event  
		- Matches a value in classes.csv class column, or a class described in readme if classes.csv is not included  
	- Start_timestamp: onset time of the event in ISO format 2025-05-20T10:00:00.000-05:00  
	- duration: event length in seconds  
	- position: localized position coordinate in meters: (utm x, utm y ,elevation, utm zone)  
	- file_ids: list of file_id  [for audio clips that participated in the localization] OR [within [100m] of the event]  
		- Matches the `file_id` column of audio_file_table.csv   
	- file_start_time_offsets: time in seconds from start of each audio file to the the start of the clip   

classes.csv: table of species names and alpha codes.  
- Columns:  
	- class: e.g. alpha code using IBP 2025 taxonomy [link]
		- these values are used in `label` column of localized_events.csv  
	- species: English common name using IBP taxonomy  
	- description: notes on classification, e.g. “did not annotate drumming, only vocalizations”  

/script/sync.js: script using Open Acoustic Devices Audiomoth-Utils lib to post-process gps-enabled Audiomoth recordings

/script/detect.py: python script using HawkEars to detect species in audio

/script/localize.ipynb: python notebook using OpenSoundscape to perform acoustic localization on the outputs of /script/detect.py

/script/plot.rmd: R markdown script to visualize the localized songs

/script/python_environment.yml: conda enviornment file; to re-create Python environment used here, run `conda create -f python_environment.yml`

/localization_metadata/audio_file_table.csv: lists all audio files by audio_file_id, rel_path the relative path from the top-level directory of this dataset to the audio clip, and the point_id indexing into point_table for the position of the microphone where this audio file was recorded.

/localization_metadata/point_table.csv: lists the positions of each microphone

/audio/: each subfolder in /audio/ contains audio clips from one microphone, with an associated position listed in /localization_metadata/point_table.csv. Audio files in sub-folders are [10 second audio clips for each localized event that includes the microphone, centered on the 3-second window where the event occurred] OR [10 minutes of audio starting at 9am on 2025-01-01, 2025-01-08, and 2025-01-15].

## Sites

List and describe field site(s) where data were collected:   
Site name, State/Province, Country  
Ecosystem description (e.g. ecosystem type, vegetation attributes, animal community type)  


## Hardware  

Recorder source: [company, e.g. “Wildlife Acoustics,” “Open Acoustic Devices,” or custom]  
Recorder model: [make, model, and version of hardware and any customizations, e.g. “AudioMoth 1.2.0 with GPS receiver”]  
Firmware version: [e.g. AudioMoth GPS Firmare 1.0; AudioMoth Firmware 12.1]  

## Recording properties  

[These properties should describe the contents of the final dataset included in the archive; e.g. if you included only  a subset of recordings taken from a longer deployment, describe the subset]  

Date range of data: [YYYY-MM-DD] to [YYYY-MM-DD]   

Recording schedule description:  
Times of recording: [Describe times of day recorded (specific times, or relative to sunrise/sunset)]  
Sleep-wake schedule: [Recorder on-off schedule  within times of day recorded, if applicable, e.g. “10 min/20 min off”]  
Sample rate: [XXXX] Hz  
Other relevant settings: [e.g. gain]  

Data aggregation notes:  
If applicable, include details of how data was moved from  individual recorders onto computers and organized, what data if any was removed, what data was missing or corrected, and any manipulations.  

## Recorder positioning

Placement:  
Spatial pattern or geometry: [describe general shape of microphone deployment and any intentional 3D structure, e.g. grid of hexagons, tetrahedral microphones, etc.]  
Range of spacing between adjacent mics: [e.g. 30m between mics in grid, 30m between central point of hexagon and corner of hexagon]  
Dimensions of array: [describe the spatial extent of the array, e.g. 250 m x 250 m]  

Coordinate Reference System:  
Specify `x` , `y`, and `z` coordinates in one of two ways:  
1. Global CRS: A standard projected or geographic coordinate reference system (e.g., UTM Zone 33N, WGS84 lat/lon) with EPSG code or Datum and Projection  
OR  
2. Local coordinates: Coordinates relative to a user-defined origin point, with the origin location given in a global CRS  
Deployment:  
Describe any particular decisions relevant to the deployment, for instance:  
General info/size of what the recorders were deployed on (E.g. top of 3m poles, on trees between 10-20cm DBH)  
Particular protective housing or hardware used to affix recorders to trees/poles  
Special deployment instructions, e.g. if devices were deployed facing north,   
If recorders were deployed sequentially at the same points multiple times, or batteries/SD cards in the recorders were refreshed, explain the sequence here.   

Position measurement:
Method of measurement: [describe method and the make/model of any specialized hardware used e.g.  RTK GPS, Hemisphere S631 GNSS with base station and rover; measured height difference between each pair of ARUs using meter stick or laser range finder]  
Measurement postprocessing: [describe steps you took to postprocess locations, e.g. postprocessed position estimates to correct for position of RTK rover]  
General accuracy (lat/long): [write a range of accuracy, e.g. “standard deviation of position measurement ranged between 0.10 m - 0.40 m]  
General accuracy (elevation): [write a range of accuracy]  
Other notes: [e.g. “Position of the base station was near the north of the array; rover did not always connect correctly at the south side of the array so these recorders’ positions may have been measured less accurately”  

## Synchronization  
Files in archived are presumed to be synchronized (i.e. they start at precisely the time listed in the metadata and their sample rate precisely matches the audio file sample rate).  

Synchronization type: [cable, acoustic, GPS, wifi network]  

Synchronization frequency: [describe how often the synchronization occurred, how frequently timestamps taken, e.g. “GPS timestamp taken at the beginning and end of every 10min long recording”]  

Synchronization method:  
Methods used: [describe any post-processing required to synchronize clips. Are there blank spaces included in files to account for buffer overflows?]  
Recording start/end time trimming: Synchronized recordings taken simultaneously [WERE/WERE NOT] trimmed to the same start/end time.  
OPTIONAL: [Scripts/resources: point to any particular scripts used to sync the data]  

## Sound detection  

This section describes the contents of all files in the /scripts/ subdirectory. It should be possible to reproduce the localized_events.csv table using the scripts and resources provided.   

Audio preprocessing:  
Describe any resampling, noise reduction processes and algorithms used, or other pre-processing applied to the audio  
Are recordings in the dataset denoised? [Do the recordings in this dataset come with denoising already applied?]  
Scripts: [list filenames of relevant scripts included in this archive]  

Describe the list of classes (sound types) that were localized. For instance, we analyzed songs but not calls of [list of 5 bird species]; or, 4 Blue Jay call types described by Someone et al 2024 [1]  
If the classes are drawn from a standard convention, e.g. “IBP alpha codes v 2024,” mention that convention here  
Detection strategy: [e.g. convolutional neural network, hand-labeling]  
Detector name and version: [e.g. BirdNET v1.0]  
Link to detector information: [any pertinent links to software or publications for this detector]  
Post-processing detector outputs:   
Describe binarization/thresholding of classifier scores: [method used to generate 0-1 detection from continuous detector outputs, if applicable, e.g., thresholds used on a per-species basis]  
Describe any manual review process used, e.g. confirmed species ID  
Scripts/resources: [list filenames of relevant scripts or resources (e.g. table of per-species thresholds) included in this archive]  

## Localization  
Tools/packages used for localization  
Localization algorithm: [e.g. SoundFinder, correlation-sum, custom tool]  
Time delay calculation algorithm: [e.g. GCC audio cross-correlation]  
References: [any pertinent links to software or publications of this algorithm]  
Error rejection parameters: [any automated approaches to error rejection, e.g. minimum cross-correlation, minimum distance between recorders]  
Other customizations to the localization approach  
Scripts: [list filenames of relevant scripts included in this archive]  

### Manual review  
Describe any manual review performed during or after acoustic localization, and what proportion of the data underwent manual review. For example, checked alignment of cross-correlation, visually inspected heatmap of estimated position, or confirmed species ID  
Scripts/resources: [list filenames of relevant scripts or resource documents, e.g. tables of hand-labeled data, included in this archive]  

## Observational data  
Optional, describe point counts, focal follows, spot mapping, etc  

## Acknowledgements  
List people and roles, eg, a list of field technicians who assisted in deployments  
Describe data ownership and any restrictions on the use of the data  

## License  
Include a name and link to a standard or custom license for this dataset. E.g., CC0 “no rights reserved” https://creativecommons.org/public-domain/cc0/   

## Work Cited and Links   
References to any manuscripts or documents describing the study, results, or protocols related to this document  

Link to any associated studies, protocols, reference documents, taxonomies,  or resources, with short descriptions  

