# Causal Testing Artifact Setup

## Getting the virtual machine

The Causal Testing virtual machine file is located here: [CausalTesting_Artifact.ova](https://drive.google.com/open?id=1KHwFB2S9-pdzdgChGxqG1AnQNqa9glPj)

Download this file to your computer. Make sure you make note of where you put the file, as you will need it in the next step.

## Loading the virtual machine

Before you can run the virtual machine, you will need to download and install [VirtualBox 6](https://www.virtualbox.org).

Once you've installed VirtualBox, you're reading to load and run the virtual machine.
To load the virtual machine, you will need to import the **CausalTesting_Artifact.ova** file into VirtualBox. You do so as follows:

1. Open VirtualBox.
2. Go to **File > Import Appliance...**
3. Find and select the downloaded virtual machine file (CausalTesting_Artifact.ova). Click **"Continue"**.
4. Leave all the settings as they are and clikc **"Import"**.

Once the virtual machine is imported, it will appear in your VirtualBox Manager as **CausalTesting_Artifact**. You can now start the virtual machine by clicking the green **"Start"** arrow at the top of the VirtualBox Manager (see screenshot below).

<img src="https://drive.google.com/uc?id=1VjrcBPwrz4LO8cwlspb0TmEUcqwMoCp-" alt="VirtualBox Manager"/>

Once the virtual machine loads, Eclipse will open -- this process may take a few minutes. Once Eclipse opens, there will be 7 files opens labeled *Defect 0- Defect 6*. At the bottom of the window, the Tasks View is open with a list of TODOs. Each TODO is attached to the failing test for each defect. For example, **TODO: Test 00 (Training)** is associated with *Defect_0_Training*.


## Running Holmes

To see Holmes output for each defect:

1. Double-click a TODO in the Tasks View to get to the failing test.
    * *Defect 0* runs the test generation portion of Holmes (which does not include execution traces, as this was not automated in the user study version of Holmes).

    * *Defects 1-6* show the pre-processed output that participants saw during the user study.

2. Highlight the method that takes the input being tested. Each TODO is followed by comments that specify what we asked participants to do and the method to highlight.

3. Right-click the highlighted method and click **"Run Holmes"** in the pop-up menu.


If you are running Holmes on *Defect 0*, the editor will automatically go to the top of the file as Holmes generates and executes tests. This process will take a minute or two; eventually the Holmes View will open with results of the execution.

If you are running Holmes on *Defects 1-6*, after a few seconds, the Holmes View will open with the pre-processed results used in our user study. For each of these defects, the output includes the original failing test, generated passing tests, and generated faiing tests. You can access the execution trace for each test by clicking the "See Execution Trace" button under the test of interest.
