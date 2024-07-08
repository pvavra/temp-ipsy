# BIDS 

If you are following the project workflow, by now you should have a project set up on the cluster and you might be in the process of organizing your **raw data**. The next step is to convert your data into BIDS format.

[**BIDS**](https://bids.neuroimaging.io/get_started.html) is an acronym that stands for **Brain Imaging Data Structure**. It is a standardized format to organize and describe neuroscientific data (see the schema below).</b>  
In a nutshell, BIDS provides a framework for standardizing file names, folder structures and metadata for different kinds of data such as neuroimaging, behavioral, physiological etc.  
We recommend you to go through the [BIDS specification page](https://bids-specification.readthedocs.io/en/stable/) and [BIDS Starter Kit pages](https://bids-standard.github.io/bids-starter-kit/) to understand more in depth the concepts and the phylosophy behind BIDS.

```bash
.
├── CHANGES
├── dataset_description.json
├── participants.tsv
├── README
└── sub-01
    ├── anat
    │   ├── sub-01_T2starw.nii.gz
    │   └── sub-01_T1W.nii.gz
    ├── func
    │   ├── sub-01_task-mybrillianttask_bold.nii.gz
    │   └── sub-01_task-mybrillianttask_events.tsv
    └── task-mybrillianttask_bold.json

```

## Why BIDS?

- It provides a logical and intelligible structure for data of different kinds.
- It makes your data clear and reusable by other researchers, thus increasing the chance of collaborations and the chance of being cited.
- It helps you to organize your data without coming up by yourself with an efficient and flexible structure.
- It has been adopted by all the major tools for neuroscientific analysis.
- It improves science and also your science. 
  
## How to BIDS 

In the following tabs, we explain how to convert your **unprocessed raw data** into BIDS for different modalities. We focus only on specific tools that are either widely used or provide some advantages for the users.

Before you start converting your data we strongly recommend to go through the [BIDS specification page](https://bids-specification.readthedocs.io/en/stable/) relative to your modalities of interest. Make sure you read the [common principles](https://bids-specification.readthedocs.io/en/stable/common-principles.html) to grasp the jargon used for BIDS.

=== "fMRI"

    ### Using heudiconv to convert fMRI data to BIDS

    [Heudiconv](https://heudiconv.readthedocs.io/en/latest/) is a python library which helps you to convert f/MRI data to BIDS with little effort. You do not need to be very proficient in python to use it, although some basics would help you. You can either follow this brief tutorial or use the [tutorials](https://heudiconv.readthedocs.io/en/latest/tutorials.html) provided by the heudiconv developers and users.

    **Step 1**
    
    In order to convert correctly your data, heudiconv leverages on so called heuristics.</b>     
    Heuristics are rules that you provide to the software to specify which files you would like to convert.
    
    You must specify such rules within a python script that is then passed to heudiconv. You do not need to create this script from scratch, you can generate a template by running the following line on the terminal:

    ```bash
    $ heudiconv --files your_dicom_file -o bids_output_dir -f convertall -s number_of_the_subject -c none
    ```

    The previous command does **not** convert any of your data, but it creates, within the output directory, a hidden folder, `.heudiconv`, containing a template of the script `heuristic.py` and a `dicominfo.tsv` which lists all the information about the dicom files necessary for specifing the heuristics. 
    
    
    !!! note "Check your dicoms"
        It is a good practice to extract your dicom files and check by yourself the file names to assess that they match with the names stored by heudiconv in the `dicominfo.tsv`.


    !!! Danger "Delete .heudiconv" 
        Before running the actual BIDS convertion, make sure to delete the **.heudiconv** directory, otherwise the conversion will not produce a correct BIDS dataset.

    **Step 2**
    
    The following example represents a `heuristic.py` script taylored for a specific dataset. You can also use this template and adapt it to your own dataset.


    ```python

    from __future__ import annotations

    import logging
    from typing import Optional

    from heudiconv.utils import SeqInfo

    lgr = logging.getLogger("heudiconv")


    def create_key(
        template: Optional[str],
        outtype: tuple[str, ...] = ("nii.gz",),
        annotation_classes: None = None,
    ) -> tuple[str, tuple[str, ...], None]:
        """
        This function allows you to create
        the keys necessary to extract the files
        that you want to convert.
        """

        if template is None or not template:
            raise ValueError("Template must be a valid format string")
        return (template, outtype, annotation_classes)


    def infotodict(
        seqinfo: list[SeqInfo],
    ) -> dict[tuple[str, tuple[str, ...], None], list[str]]:
        """Heuristic evaluator for determining which runs belong where

        allowed template fields - follow python string module:

        item: index within category
        subject: participant id
        seqitem: run number during scanning
        subindex: sub index within group
        """

        # Create keys: This section is specific to each project
        # you need to create your own keys and the info dictionary

        t1w = create_key("sub-{subject}/anat/sub-{subject}_acq-mprage_T1w")
        task = create_key("sub-{subject}/func/sub-{subject}_task-foodchoice_run-{item:01d}_bold")
        fmap_mag = create_key("sub-{subject}/fmap/sub-{subject}_acq-grefieldmapping_magnitude")
        fmap_phasediff = create_key("sub-{subject}/fmap/sub-{subject}_acq-grefieldmapping_phasediff")
        epi = create_key("sub-{subject}/fmap/sub-{subject}_dir-AP-run-{item:01d}_epi")


        # the dictionary info must contain your keys
        info: dict[tuple[str, tuple[str, ...], None], list[str]] = { 
                t1w: [],
                task: [],
                fmap: [],
                epi: []
                }

        for s in seqinfo:
            """
            The namedtuple `s` contains the following fields:

            * total_files_till_now
            * example_dcm_file
            * series_id
            * dcm_dir_name
            * unspecified2
            * unspecified3
            * dim1
            * dim2
            * dim3
            * dim4
            * TR
            * TE
            * protocol_name
            * is_motion_corrected
            * is_derived
            * patient_id
            * study_description
            * referring_physician_name
            * series_description
            * image_type
            """

            # The following conditional statements constitute
            # the core of the heuristics. 
            # Modify them according to the dicom file names and 
            # other features (like data size) that 
            # can help select the correct files

        if "t1_mpr" in s.protocol_name:
            info[t1w].append(s.series_id)
        elif 'fMRI_SMS2_2.2iso_66sl_TR2_run' in s.protocol_name and s.dim4 == 200:
            info[task].append(s.series_id)
        elif 'field_mapping' in s.protocol_name and s.dim3 == 132:
            info[fmap_mag].append(s.series_id)
        elif 'field_mapping' in s.protocol_name and s.dim3 == 66:
            info[fmap_phasediff].append(s.series_id)
        elif 'EPI' in s.protocol_name:
            info[epi].append(s.series_id)

            
        return info
    ```
    
    The essential parts of the script that you need to modify are the **keys** and the actual **heuristics**, keep in mind that you have to modify keys and heuristics according to your dataset files:

    - **Keys:** In the `dicominfo.tsv` (in `.heudiconv`) file you find the exact naming of the dicom files, you need to go through the list of files and decide which of them are important for your analysis.</b>  
    Once you have made your decision, you can create the proper keys by following the naming conventions explained in the [BIDS tutorial](https://bids-standard.github.io/bids-starter-kit/folders_and_files/files.html). Keys create the BIDS compliant file names and structure.
    
        - Start with your anatomical images, which all go into the `anat` folder:

            ```python
            t1w = create_key("sub-{subject}/anat/sub-{subject}_acq-mprage_T1w")
            ```

            - The `sub-{subject}/` folder where `{subject}` is a placeholder that heudiconv fills in with the correct subject ID.
            - The folder `anat/` is dedicated to all the anatomical images.
            - In `acq-<label>` you provide the type of acquisition e.g. `mprage` (this is not mandatory).
            - The `Tw1` suffix is the standard name for T1 weighted images. Keep in mind that you might have have different anatomical images.  

        - Set up the functional images, which go into the `func` folder.

            ```python
            task = create_key("sub-{subject}/func/sub-{subject}_task-foodchoice_run-{item:01d}_bold")
            ```

            - As for the anatomical image you need to add the `sub-{subject}/` folder.
            - The `func` folder is dedicated to the functional images.
            - The `task-<taskname>` indicates the name of the task.
            - `_run-` is just a place holder for the run number that heudiconv fills in automatically.
            - The `bold` suffix indicates the type of functional file we are dealing with.

        - Set up the field maps images, which go into the `fmap` folder. The previous anatomical and functional images
          are rather standard for fMRI experiments, the field maps can be very different and sometimes absent.

            ```python
            fmap_mag = create_key("sub-{subject}/fmap/sub-{subject}_acq-grefieldmapping_magnitude")
            fmap_phasediff = create_key("sub-{subject}/fmap/sub-{subject}_acq-grefieldmapping_phasediff")
            epi = create_key("sub-{subject}/fmap/sub-{subject}_dir-AP-run-{item:01d}_epi")
            ```
            
            - As for the other images you set up the subject folder `sub-{subject}`.
            - The `fmap` folder is dedicated to the fieldmap related images.
            - In this particular case for `fmap_mag` and `fmap_phasediff` we specified the `_acq-<label>_` which changes for different cases.
            - The suffixes `magnitude` and `phasediff` and `epi` are mandatory for these fieldmap images, please refer to the [official page](https://bids-specification.readthedocs.io/en/stable/modality-specific-files/magnetic-resonance-imaging-data.html#fieldmap-data).
            - The `_dir-AP_` indicates the Phase-Encoding direction in the `EPI` image, refer to the [official page](https://bids-specification.readthedocs.io/en/stable/appendices/entities.html#dir) for further information.

    - **Heuristics:** Now we need to create a few basic rules to tell heudiconv which files to extract.
    
        In the following example we simply match the name of a `dicom` we would like to include in the BIDS conversion with the file names contained in `s.protocol_name`, if there is a match we add the file in the `info` dictionary.

        ```python
        if "t1_mpr" in s.protocol_name: # name matching
            info[t1w].append(s.series_id) # append to the dictionary
        ```
        Sometimes a file name matching is not enough to extract a the correct file, so we can combine more rules like the following cases, in which we match the file name and the file size.

        ```python
        elif 'fMRI_SMS2_2.2iso_66sl_TR2_run' in s.protocol_name and s.dim4 == 200:
            info[task].append(s.series_id)
        ```

        ```python
        elif 'field_mapping' in s.protocol_name and s.dim3 == 132:
            info[fmap_mag].append(s.series_id)
        elif 'field_mapping' in s.protocol_name and s.dim3 == 66:
            info[fmap_phasediff].append(s.series_id)
        ```

        As you might guess you need to create rules on a case by case basis.


    **Step 3**

    Now you just need to run heudiconv for the actual conversion with the following line, please for further information about heudiconv specs visit the software page or type `heudiconv --help` in the terminal:
    

    ```bash
    $ heudiconv --files <file_containing_dicoms> -o <directory_for_bids/> -f <heuristic.py> -s <subject_number> -c dcm2niix -b --overwrite
    ```

    The following tree represents the result of the BIDS conversion. 

    ```
        .
        ├── CHANGES
        ├── dataset_description.json
        ├── participants.json
        ├── participants.tsv
        ├── README
        ├── scans.json
        ├── sub-01
        │   ├── anat
        │   │   ├── sub-01_acq-mprage_T1w.json
        │   │   └── sub-01_acq-mprage_T1w.nii.gz
        │   ├── fmap
        │   │   ├── sub-01_acq-grefieldmapping_magnitude1.json
        │   │   ├── sub-01_acq-grefieldmapping_magnitude1.nii.gz
        │   │   ├── sub-01_acq-grefieldmapping_magnitude2.json
        │   │   ├── sub-01_acq-grefieldmapping_magnitude2.nii.gz
        │   │   ├── sub-01_acq-grefieldmapping_phasediff.json
        │   │   ├── sub-01_acq-grefieldmapping_phasediff.nii.gz
        │   │   ├── sub-01_dir-AP_run-1_epi.json
        │   │   └── sub-01_dir-AP_run-1_epi.nii.gz
        │   ├── func
        │   │   ├── sub-01_task-foodchoice_run-1_bold.json
        │   │   ├── sub-01_task-foodchoice_run-1_bold.nii.gz
        │   │   ├── sub-01_task-foodchoice_run-1_events.tsv
        │   │   ├── sub-01_task-foodchoice_run-2_bold.json
        │   │   ├── sub-01_task-foodchoice_run-2_bold.nii.gz
        │   │   ├── sub-01_task-foodchoice_run-2_events.tsv
        │   │   ├── sub-01_task-foodchoice_run-3_bold.json
        │   │   ├── sub-01_task-foodchoice_run-3_bold.nii.gz
        │   │   ├── sub-01_task-foodchoice_run-3_events.tsv
        │   │   ├── sub-01_task-foodchoice_run-4_bold.json
        │   │   ├── sub-01_task-foodchoice_run-4_bold.nii.gz
        │   │   ├── sub-01_task-foodchoice_run-4_events.tsv
        │   │   ├── sub-01_task-foodchoice_run-5_bold.json
        │   │   ├── sub-01_task-foodchoice_run-5_bold.nii.gz
        │   │   └── sub-01_task-foodchoice_run-5_events.tsv
        │   └── sub-01_scans.tsv
        └── task-foodchoice_bold.json

    ```

    Note that heudiconv creates the whole BIDS structure, the sidecars as `.json` files, and the events files as `.tsv`.</b>   `events.tsv` are just tabular files containing the header: `onset	duration	trial_type	response_time	stim_file`. Your task is to complete these files according to the bids conventions.

=== "EEG/MEG"

    ### EEG BIDS recommended formats

    Given the variety of EEG data formats, BIDS recommends two main formats (we also strongly suggest to use these two formats):

    - European format: `.edf`
    - BrainVision format: `.vhdr, .vmrk, .eeg`

    Other BIDS accepted formats, although not recommended, are: Biosemi `.bdf` and EEGLAB: `.fdt .set`

    ### EEG conversion with Fieldtrip (Matlab)



    ### EEG conversion with MNE (python)

=== "Behavioral/Physiological"

    ### Behavioral data

    BIDS for behavioral data follows conventions similar to those used for the `events.tsv` files with neural recordings. 
    We recommend to read carefully the [description](https://bids-specification.readthedocs.io/en/stable/modality-specific-files/behavioral-experiments.html#behavioral-experiments-with-no-neural-recordings) provided in the BIDS website.

    !!! note "Behavioral data general rules"

          - **What kind of data:** Any behavioral measures (with no concurrent neural recordings).
          - **File types:** Tabular data as `.tsv` with the following header `trial response response_time stim_file` (further entries can be added please refer to the BIDS page above). Metadata as `.json` files.  
          - **Where:** Data must go under the `<beh>/` folder.
          - **How:** Data must have the following format:</b>  
                          `<matches>_beh.tsv`</b>  
                          `<matches>_beh.json`</b>  
                      where `<matches>` can be `sub-012_task-mytaskname`. In case you have multiple sessions and runs your file might be:`sub-012_ses-1_task-mytaskname_run-1-beh.tsv` and  the relative metadata `sub-012_ses-1_task-mytaskname_run-1-beh.json`.
    
    
    !!! note "Save your behavioral data already in BIDS" 
        It is convenient to save your raw behavioral files as BIDS compliant. 
    
    Given the large variety of structures and formats that researchers use for their behavioral experiments, we do not provide an example.

    ### Physiological data

    We recommend to read the [dedicated page](https://bids-specification.readthedocs.io/en/stable/modality-specific-files/physiological-and-other-continuous-recordings.html) in the BIDS website. 

    !!! note "Physio data general rules"

        - **What kind of data:** Cardiac, respiratory and other continuous recordings
        - **File types:** Continuous recordings as compressed files `.tsv.gz` (no header). Metadata as `.json` files.  
        - **Where:** Data can go under different `<datatype>/` folders, such as `func`, `anat`, `dwi`, `meg`, `eeg`, `ieeg`, or `beh`
        - **How:** Data must have the following format:</b>  
                        `<matches>[_recording-<label>]_physio.tsv.gz`
                        `<matches>[_recording-<label>]_physio.json`
                    where `<matches>` can be `sub-012_task-mytaskname` and `_recoding-<label>` can be used to distinguish between two or more type of recordings e.g. `recording-breathing` and `recording-eyetracking`. In case you have multiple sessions and runs your file might be: `sub-012_ses-1_task-mytaskname_run-1-breathing_physio.tsv.gz` and the relative metadata `sub-012_ses-1_task-mytaskname_run-1-breathing_physio.json`. 
    
=== "Eye-tracking"
    Coming soon...

## BIDS beyond rawdata

TODO: Your data are not limited to your raw data, but include also so called `derivatives`, meaning the results of data processing.
