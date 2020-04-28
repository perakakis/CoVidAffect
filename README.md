# [CoVidAffect](https://covidaffect.info)
Real-time, geolocalized data of mood variations following the COVID-19 crisis

# How to cite
Perakakis, P. (2019). HEPLAB: a Matlab graphical interface for the preprocessing of the heartbeat-evoked potential (Version v1.0.0). Zenodo. http://doi.org/10.5281/zenodo.2649943

# Background
The COVID-19 outbreak and the ensuing confinement measures are expected to bear a significant psychological impact on the affected populations. Here, we publish a dataset from CoVidAffect, a citizen science project that was launched to provide direct, geolocalized data, of individual changes in subjective feeling and physical arousal following the COVID-19 crisis. These publicly available data are continuously updated and visual summaries are displayed on the project website. The data can be further analyzed to identify affected geographical regions, quantify emotional responses to specific measures and policies, and to understand the effect of context variables, such as living space, employment status, and practice of physical exercise, on emotional regulation and psychological resilience. Our goal is to offer a resource that will help to anticipate the needs for psychosocial support and facilitate evidence-based policy making.

# Preprint
You can read more details about the data acquisition method in our preprint:

# Data Records
The data are stored in three distinct CSV files located at the 'datasets' directory.

The file participants.csv stores data from the initial participation questionnaire and includes the following variables:

**id** – Unique identifier for each participant. This variable is used to merge the three databases.
registered_date – Date of registration in the database in the form: DD/MM/YYYY HR:MM:SS.

**sex** – Gender of each participant. Possible values are: feminine, masculine and other.

**age** – Age of each participant ranging from 16 to 100+ years with all intermediate values possible.

**country** – The participant’s country of residence.

**postcode** – The participant’s postal code that is used for geolocation.

**family** – Numeric value that codes the number of people in the residence. Possible values are: 1 (“I live alone”), 2, 3, 4, 5, and 6+.

**family_ages** – Age of each one of the residents included in the variable family.

**family_relation** – The relationship between the participant and each one of the residents included in the variable family. Possible comma separated values are: parent, partner, child, sibling, grandparent, grandchild, other, any (=no family relation).
type_living – Housing type. Possible values are: study, apartment, house, residence, chalet, other.
living_rooms – When type_living is set to apartment, house, or chalet, this numeric variable codes the number of rooms (1, 2, or 3+).
open_spaces – Access to open spaces. Possible comma separated values are: balcony, garden, yard, other, no.
work_previous – Employment status before the crisis. Possible values are: employee, self (self-employed), unemployed, student, retired, other.
work_actual – Current employment status before the crisis. Possible values are: same, new, telework, erte (temporary employment suspension), fired, increased, reduced, other.
income – Net monthly income. Coded in 10 possible ranges (in Euros): 0-500, 500-1000, 1000-1500, 1500-2000, 2000-2500, 2500-3000, 3000-5000, 5000-7000, 7000-9000, 9000+.
negative_economy – Binary flag to code the participant's perception on whether the crisis has negatively affected their economic situation (0=’no’, 1=’yes’).
covid – Codes presence of COVID-19 symptoms. Possible values are: no, not_reported (symptomatic but not officially diagnosed), reported (officially diagnosed with COVID-19).
covid_family – Codes presence of COVID-19 symptoms in other residents. Possible values are: no, not_reported (symptomatic but not officially diagnosed), reported (officially diagnosed with COVID-19).
activity – Codes the number of hours dedicated to physical exercise before the crisis. Possible values are: 0-2, 2-4, 4-6, 6-8, 8+.
valence – Valence rating before the crisis in a scale from -50 (negative) to +50 (positive).





## Standalone installation
If you wish to use HEPLAB as a standalone toolbox to detect cardiac events and export them for further processing with a different analysis software, you simply have to add manually HEPLAB's main folder and subfolders to Matlab's path.

Note that if you are using HEPLAB as a standalone toolbox, to start the GUI you simply have to type: `heplab` to Matlab's command window. The program will then prompt a dialogue box that will let you select a *.mat* file. This file needs to contain two variables. An `ecg`variable with the raw ECG signal and an `srate` variable with the sampling rate. When you are finished with the event detection and artifact correction, instead of using the [SAVE EVENTS](#save-events) button, you will have to export the event information using the [Save HEP structure](#save-hep-structure) menu option.

# GUI
To open HEPLAB's GUI first you need to import your *continuous* data into EEGLAB using one of the available options depending on your data format. At the moment, HEPLAB only works with continous data so make sure you take this into account in your analysis workflow and perform cardiac event detection *before* segmenting your data into epochs.

Once your data is loaded, click on the **Tools** tab at EEGLAB's main window. You will see **HEPLAB** as one of the available tools. Clicking on **HEPLAB** first lets you select your ECG channel and then opens the main GUI:
![Main GUI](Documentation/Figures/GUI.png)

## GUI options
### Slider
Once your ECG channel is loaded, you can use the *slider* to scroll through the signal. The **Location** box always indicates the starting time point (in seconds) of the data segment displayed in the plot.

### Window size
By default the ECG signal is displayed in 10-sec windows. You can change the size of the window to display larger —or smaller— portions of the signal by adjusting the **Win size** parameter. Use the two arrows to navigate through the signal jumping in **Win size** increments.

### Scale
The **Scale** option can be used to multiply the entire signal by a user-defined parameter. This can be useful to invert the signal (type `-1` in the **Scale** box) or to adjust its amplitude to facilitate visual inspection and artifact correction. By default HEPLAB plots the signal's normalized amplitude in the [0,1] range, obtained by:

`ecg = (ecg-min(ecg))/(max(ecg)-min(ecg));`

Note that if you accidentally multiply by `0`, the ECG signal will be erased and you will have to reload it from the EEG structure or a HEP variable (see [Menu options](#menu-options) bellow).

### Filter
The **Filter** option applies a 2nd order Butterworth filter to the ECG signal. You can select the low and high-cutoff frecuency (in *Hz*) in the corresponding boxes. Depending on the presence of low and/or fast frecuencies in the original ECG signal, filtering can significantly improve the efficiency of the peak detection algorithms and it is therefore highly recommended before their application.

Note that filtering permanently modifies the data. To recover the original pre-filtered signal you will need to reload it from EEGLAB, or from a *.mat* file (see [Menu options](#menu-options) bellow).  

### Preview IBIs
The **PREVIEW IBIs** button opens a new Matlab figure displaying the interbeat intervals (IBIs) resulting from the marked cardiac events. This is very helpful for detecting artifacts that may have escaped a first visual inspection. See the [Test case](#test-case) bellow for an example.

### Save events
The **SAVE EVENTS** button is used after cardiac-event detection and artifact correction to add the resulting events in EEGLAB's event structure.  

## Menu options
### Edit
#### Load ECG from eeglab
This option can be used to reload the original ECG signal from EEGLAB. Note that this will permanently erase any previously detected cardiac events. To avoid loing your work, either add the events in the EEG structure using the [SAVE EVENTS](#save-events) button or save the entire HEP structure in a *.mat* file using the [Save HEP structure](#save-hep-structure) menu option.

#### Load ECG from .mat file
This option can be used to load an ECG signal contained as a variable in a *.mat* file. This file must include an `ecg`variable with the raw ECG signal and an `srate` variable with the sampling rate.

#### Clear events
Use this option to remove all cardiac event marks. Note that applying any peak detection algorithm also removes previously detected events.

#### Save HEP structure
This option lets you select a filename and save the entire HEP structure, including the raw ECG signal and the cardiac event information.

#### Load HEP structure
If you have previously saved a file containing a HEP structure, you can use this option to select the file and load all HEP parameters, inlcuding the ECG signal and the cardiac events.

### Tools
#### Detect R waves
- **ecglab fast** and **ecglab slow** are two very similar peak detection algorithms included in the *ECGLAB*, developed by the group of João Luiz Azevedo de Carvalho at the University of Brasilia, DF, Brasil. Details about the algorithm are given in [Carvalho et al., 2002](Documentation/Articles/icsp2002_ecglab.pdf).
- **Pan-Tompkin** implements the [Pan & Tompkins, 1985](Documentation/Articles/PanTompkins.pdf) algorith using the function `BioSigKit.PanTompkins()` that is distributed with the [BioSigKit toolbox](http://joss.theoj.org/papers/10.21105/joss.00671), developed by 
[Hooman Sedghamiz](https://github.com/hooman650).

#### Detect T waves
- An implementation of the **Multilevel Teager Energy Operator** described in [Sedghamiz & Santonocito, 2015](http://ieeexplore.ieee.org/document/7391510/). The algorithm used in HEPLAB is a modified version of the function `BioSigKit.MTEO_qrstAlg()` that is distributed with the [BioSigKit toolbox](http://joss.theoj.org/papers/10.21105/joss.00671), developed by [Hooman Sedghamiz](https://github.com/hooman650).

# Test case
We will demonstrate a test case using the sample data in *EEG1.set*, included in the HEPLAB package. 

After loading *ECG1.set* click on **Tools>HEPLAB**. In the dialogue box select the last channel that contains the ECG signal:

![chnselect](Documentation/Figures/chnselect.png)

The selected ECG signal is displayed at the main GUI:

![rawecg](Documentation/Figures/rawecg.png)

To remove the low and high frecuencies from the particularly signal we apply a filter with 3Hz low, and 30Hz high cutoff frecuency:

![filtecg](Documentation/Figures/filtecg.png)

Next, we detect the R wave peaks by applying the *ecglab fast* algorithm:

![filtecg](Documentation/Figures/Rdetect.png)

For this particular signal the fast algorithm works perfect, but there are cases where the other two available algorithms may produce better results. You can easily go through the different algorithms to decide the one that is more suitable for each particular signal.

The detected R wave peaks are marked by red circles. When the *Win size* is 10 seconds or smaller, next to each red mark you can read the index and the value of each interbeat interval. In this example, the second red mark designates the first interval, which is the difference between the second and the first peak, with a value of 1079 ms: *1079[1]*.

We can increase *Win size* and use the slider or the arrows to rapidly scroll through larger segments of the data.

To demonstate artifact correction, we deliberately remove the R wave peak of the 80<sup>th</sup> interval:

![arti1](Documentation/Figures/artifact1.png)

We can now use the **PREVIEW IBIs** button to inspect the interval series resulting form the R wave peak detection process:

![arti1](Documentation/Figures/artifact2.png)

In this matlab figure we can use the cross tool to mark the problematic interval and identify its index. Here it is the 79<sup>th</sup> interval since for the representation of the IBI series the first interval is always removed.

We can now go back to the main GUI and manually add a mark on the 80<sup>th</sup> interval by clicking on top of it. A new inspection of the IBI series indicates that there are no more artifacts in the peak detection process:

![arti1](Documentation/Figures/artifact3.png)

Please note that the inspection of the IBI series is an additional measure that should not substitute the careful visual inspection of the entire ECG signal.

A similar process can be used for the detection of the T wave:

![arti1](Documentation/Figures/Tdetect.png)

Once the peak detection process is concluded, we can use the **SAVE EVENTS** button to add the detected cardiac events to the EEG structure and save the result as a new dataset. A simple visual inspection of the channel data in EEGLAB shows that the cardiac events have been imported successfully:

![arti1](Documentation/Figures/eegevents.png)

It is now fairly easy to use common EEGLAB functions for epoching, baseline removal, artifact rejection and averaging, in order to obtain the Heartbeat Evoked Potential.

# Data structure
HEPLAB saves the structure variables *HEP* and *hepgui* in the Matlab workspace. These two structures contain all necessary variables for the execution of the program. The structure *hepgui* contains all gui objects. The *HEP* variable contains the ECG signal, the sample rate and other useful parameters.

When you execute the *heplab.m* script from the command window, the program looks for a HEP variable in the workspace and if it does not exist it opens a dialogue box to let you select a *.mat* file containing the *ecg* and *srate* variables. If a HEP variable is found in the workspace *heplab.m* will open the GUI using the data in contained within this variable.

The peak detection functions are normally called from the GUI but advanced users can use them directly from the command window or from within their scripts. Please see the [Codemap](#codemap) to identify the functions and the *help* in each one of them for usage instructions. Note that their use without visual inspection and artifact correction is strongly discouraged!

# Codemap

```
|--- heplab.m                           <-------- Main module, GUI and Command line
|--- eegplugin_heplab.m                 <-------- Load as EEGLAB plugin
|--- Functions                          <-------- List of scripts and functions
    |--- heplab_about.m                 <-------- Contact and license information
    |--- heplab_calculate_IBIs.m        <-------- Calculates IBIs, called by heplab_ecgplot.m
    |--- heplab_chansel.m               <-------- GUI to select ECG channel, called by pop_heplab.m
    |--- heplab_ecg_filt.m              <-------- Applies a Butterworth filter, called by heplab.m
    |--- heplab_ecgplot.m               <-------- Plots the ECG signal, called by heplab.m
    |--- heplab_edit_events.m           <-------- Manualy edit cardiac events, called by heplab.m
    |--- heplab_fastdetect.m            <-------- ECGLAB fast peak detection, called by heplab.m
    |--- heplab_load_ECG.m              <-------- Load ECG from .mat file, called by heplab.m
    |--- heplab_load_HEP.m              <-------- Load HEP data from .mat file, called by heplab.m
    |--- heplab_pan_tompkin.m           <-------- Pan-Tompking peak detection, called by heplab.m
    |--- heplab_preview_IBIs.m          <-------- Plot IBIs, called by heplab.m
    |--- heplab_save_events.m           <-------- Add events to EEG structure, called by heplab.m
    |--- heplab_slowdetect.m            <-------- ECGLAB slow peak detection, called by heplab.m
    |--- heplab_T_detect_MTEO.m         <-------- T wave detection, called by heplab.m
    |--- heplab_pop_heplab.m            <-------- Initiate HEP variables
|--- SampleData                         <-------- Sample data folder
    |--- EEG1.set                       <-------- EEGLAB set file used in the test case
|--- Documentation                      <-------- Manual and relevant articles
|--- LICENCE                            <-------- GPL License
```

