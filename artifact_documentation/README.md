# Causal Testing Artifact (ICSE 2020)

This is the artifact for the paper *"Causal Testing: Understanding
Defects' Root Causes"*. This artifact facilitates the use of Holmes for debugging defects and re-use of the implementation and/or artifacts in other research studies.

## What is included in the artifact?

Our artifact includes the following:

1. Holmes, our prototype Causal Testing implementation.

2. A virtual machine that facilitates the use of Holmes, our Causal Testing prototype.

2. The supplemental data collection materials used when evaluating Holmes.

## Where can I obtain the artifact components?

All the above listed artifacts, with the exception of the virtual machine file, are located in this repository.

## Setting up the virtual machine
*Copied from INSTALL.md*

1. Download [VirtualBox](https://www.virtualbox.org).
2. Download virtual machine file [CausalTesting_Artifact.ova](https://drive.google.com/open?id=1KHwFB2S9-pdzdgChGxqG1AnQNqa9glPj).
<br> **Please note this is a large file (5BG) and may some time to download.**
3. Open VirtualBox.
4. Go to **File > Import Appliance...**
5. Find and select the downloaded virtual machine file (CausalTesting_Artifact.ova). Click **"Continue"**.
6. Leave all the settings as they are and click **"Import"**.

Once the virtual machine is imported, it will appear in your VirtualBox Manager as **CausalTesting_Artifact**. 

You can now start the virtual machine by clicking the green **"Start"** arrow at the top of the VirtualBox Manager (see screenshot below).

<img src="https://drive.google.com/uc?id=1VjrcBPwrz4LO8cwlspb0TmEUcqwMoCp-" alt="VirtualBox Manager"/>

### Resizing the virtual machine screen

If the virtual machine loads and you find it is not an appropriate size, you can find the size that works for you by doing the following:

1. In your VirtualBox VM menu, go to **View > Virtual Screen 1 > ...**. You will see different scaling options; select the one that best suites your screen.

<img src="https://drive.google.com/uc?id=1VMiFnd8EXqXiPjR7LAvyUMd94dYkNdQc" alt="Resize menu" width="550" height="600"/>

2. Your menu may look different if you are running a different operating system. However, if given percentages to re-scale they will have the same effect.

### Waking up the virtual machine

If you leave the virtual machine and return to a black screen or screen saver, press any key on your keyboard to wake up the virtual machine.

## Loading and navigating Eclipse

Once the virtual machine loads, Eclipse will open. In Eclipse, there will be 8 projects loaded on the left side in the Project Explorer. Seven of the projects are from the [Defects4J](https://github.com/rjust/defects4j) defect benchmark; these projects are labeled *Defect_0_Training*, *Defect_1*, *Defect_2*, *Defect_3*, *Defect_4*, *Defect_5*, and *Defect_6*. The eighth project is *Holmes*. 

<img src="https://drive.google.com/uc?id=1zKXVhjMjzfgrrh2xXY_A7MlLfAkigjRO" alt="Eclipse"/>

At the bottom of the window, the Tasks View is open with a list of TODOs. Each TODO maps to a failing test in its respective project that exposes a defect in that project's source code. 
For example **TODO: Test 00 (Training)** maps to a failing test in the project *Defect_0_Training* that exposes a defect.

You may get a "Low Disk Space" warning -- you can click "Ignore". Running Holmes doesn't require a significant amount of memory.

## Holmes runtime

The runtime for Holmes varies depending on a number of factors, such as input value, input difference threshold, and project size. The current version of Holmes works on tests for single parameter methods that take either a primitive type or String. To replicate Holmes' runtime when generating tests, do the following:

1. Double-click the TODO labeled **TODO: Test 00 (Training)**. Within a few seconds, the file *BooleanUtilsTest.java* opens at the ```test_toBoolean_String()``` method. Inside the ```test_toBoolean_String()``` method is the failing test ```assertEquals(false, BooleanUtils.toBoolean("tru");```.

<img src="https://drive.google.com/uc?id=1fpS9WQLitBs_fk07tuBtkhkkrLHGAh0x" alt="Eclipse TODOs"/>

2. Double-click the method call ```toBoolean``` so that it is highlighted, as show in the screenshot below.

<img src="https://drive.google.com/uc?id=1Bs8DV4B1rsqqr8PDWPZbiQL45HFkcqjv" alt="Highlighting target method"/>

3. Right-click the highlighted method and click **"Run Holmes"** in the pop-up menu (shown below). The editor will automatically go to the top of the file and some dialog windows may pop up as Holmes generates and executes tests. This process will take a minute or two; to reduce the chances of Holmes or the virtual machine hanging, we selected a defect for which Holmes is quickly able to find similar passing tests.

<img src="https://drive.google.com/uc?id=1T5IOWdJvIkt6nte0zcKnrlbdPxQzraol" alt="Run Holmes command"/>

Eventually, a view labeled "Holmes View" will open at the bottom of the screen with the results of the execution (as shown below). 

<img src="https://drive.google.com/uc?id=1ZP2c1zftIuyvyyuEPOBj1pTb9mujvi8N" alt="Test 00 Output"/>

Now let's see how we can use Causal Testing to debug.

## Debugging with Holmes

Holmes is a prototype implementation of our novel testing technique, Causal Testing. Causal Testing conducts causal experiments, which involves perturbing test inputs to find passing executions that are similar to the failing execution, to help developers understand why a given test is failing and how to fix it.

To see how you can use Causal Testing to debug a failing test, we first need to produce the output:

1. Double-click the TODO labled **TODO: Test 01**. This will open the file *StringEscapeUtilsTest.java* at the ```testEscapeJavaWithSlash()``` test method. Inside the test method is the following failing test:

```
String input = "String with a slash (/) in it";
final String expected = input;
final String actual = StringEscapeUtils.escapeJava(input);

assertEquals(expected, actual);
```

2. For this test, the method call being tested is ```escapeJava(input)```. Therefore, to invoke Holmes we want to double-click to highlight ```escapeJava```, as shown below.

<img src="https://drive.google.com/uc?id=13BS4_6TgrzqRICaPM9LBRoFonLq8RWzt" alt="Test 01"/>

3. Right click the highlighted method and select **Run Holmes** from the pop-up menu. The output will appear at the bottom of the screen in the "Holmes View"

<img src="https://drive.google.com/uc?id=1SsHbQei8HjLDAuWMpthbxDubzzJiJhwW" alt="Run Holmes Test 02" width="700" height="500"/>


Now that we have the Causal Testing results, we can begin to debug the defect.

First, we can see that Holmes has provided three similar passing tests and three similar failing tests. Just from looking at the inputs to the tests that pass and the tests that fail we can see that, like the original failing test, all the additional failing tests include the ```/``` character while the tests that pass do not.

Second, we can see that each generated test has a button under it labeled "See Execution Trace". Clicking this button opens a minimized trace of the execution; clicking the button again hides the trace.

<img src="https://drive.google.com/uc?id=1JbXlMtDWcoBIlNipa4h9kiPbyAV55pKn" alt="Test 02 Output"/>

