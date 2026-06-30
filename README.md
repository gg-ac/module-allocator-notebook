This repo contains python code for allocating students to modules based on their preferences and choice constraints.

## Getting Started

### Running on Binder.org
The simplest way to run the app is via the binder.org web service.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/gg-ac/module-allocator-notebook/HEAD?urlpath=%2Fdoc%2Ftree%2Fapp.ipynb)


### Running Locally (on your computer)

The code requires several dependencies which can be installed in a Conda environment. After installing Conda, use the following commands to create the environment and install the dependencies.
```
conda env create -f environment.yml
conda activate module-allocator-notebook
```

## Using the App
To use the app, you will need to run ```app.ipynb``` locally or through Binder. You will not need to edit any code, but you will need to run each piece of the code as follows.

### Step 1
Run the whole notebook by pressing "run".
This will create a user interface section under the heading "Step 1 - Data Loading".
You will then upload the required CSV data files. Press each upload button, selecting the file, then when all files have been selected, press "Load Data".

### Step 2
Run the section of the notebook labelled "Step 2 - Constraints".
This will generate a new user interface section where you can input the constraints for the allocation process.

- ```Required Credits Per Student```: The number of credits to be allocated to each student
- ```Module Group Credit Requirements```: The minimum and maximum number of credits to be allocated to all students from each of the module groups
- ```Semester Credit Requirements```: The minimum and maximum number of credits to be allocated to students in each semester
- ```Worst Module Preference to Allow```: A number representing the maximum (worst) preference rating that a module can have and still be allocated to the student who gave it that rating. This allows you to ensure that no student is allocated very low preferences.
- ```Stop After (N Modules Per Student)```: The number of modules to allocate to each student.
- ```Optimisation Iterations```: The number of times to run the random allocation before picking the run which had the best average student satisfaction rating.
- ```Random Seed```: Change this to change the randomisation. Keep it the same to randomise in the same way every time the algorithm is run. You shouldn't need to change this.
- ```Check Constraints```: When ticked, don't allow finished assignments of students to modules which don't satisfy the constraints above. When unticked, do allow finished assignments of students to modules which don't satisfy the constraints above.

### Step 3
Run the code under the heading "Step 3 - Allocation".
This will create a button labelled "Run Allocation". Press it to run the allocation.
You can optionally show some of the results by pressing the "Show Results" button.

### Step 4
Run the code under the heading "Step 4 - Results".
This will create a download button that you can press to download the resulting allocation data. The download is a "zip" file containing several CSV files. There is one file for each module, listing the students allocated to that module; and four allocation metadata files summarising properties of the allocation process.
Make sure you check the "constraints_summary.csv" file to ensure all the constraints were properly satisfied by the allocation.


## Data Format
The allocator requires several sets of data to be uploaded as CSV files. CSV files are like simplified versions of Excel documents, and can be exported via "save as" in Excel.

Each of the files contains structured data about modules, students, and other requirements.

**Sample data files are available in the "sample_data" directory.**

#### module_metadata.csv
- ```module_id```: The unique ID code of the module. Must match the IDs used in other data files.
- ```module_name```: A human-readable name for the module
- ```module_group```: The unique ID of the module group (e.g. "Health/Social", "Biological", "Cognitive").
- ```semester```: A number defining the semester in which the module runs
- ```credits```: A number of course credits for the module. All modules can be set to 1 if credits do not vary between modules.
- ```capacity```: The maximum number of student places available on this module 
- ```available_spaces```: The number of student places not yet allocated on this module
- ```required_modules```: A comma-separated list of other module ID codes for modules which must be taken by any student taking this module (i.e. co-requisites). (e.g. "PSYC3502,PSYC3548", without the quotes)
- ```mutually_excluded_modules```: A comma-separated list of other modules ID codes for modules which cannot be taken by a student already taking this module.
- ```programme_bsc```: A true/false value indicating whether this module can be taken by students on the BSc programme
- ```programme_psyched```: A true/false value indicating whether this module can be taken by students on the PychEd programme
- ```programme_ppst```: A true/false value indicating whether this module can be taken by students on the PPST programme

Note: More or fewer programmes can be added by inserting or removing columns with heading format "programme_X".

#### student_module_preferences.csv
- ```student_name```: The full name of the student
- ```student_id```: The unique ID of the student
- ```programme```: The ID of the degree programme to which the student belongs. Must exactly match the suffix of a programme column from "module_metadata.csv". (e.g. "ppst", "bsc", "psyched")
- An additional column for each module, labelled with the module ID, and containing the student's preference rating for that module. Rating "0" means unrated, "1" means most preferred, "2" is second most preferred, etc.

#### student_module_group_preferences.csv
- ```student_name```: The full name of the student
- ```student_id```: The unique ID of the student
- ```Biological```: The number of modules the student would prefer to be allocated in the "Biological" category
- ```Cognitive```: The number of modules the student would prefer to be allocated in the "Cognitive" category
- ```Health/Social```: The number of modules the student would prefer to be allocated in the "Health/Social" category

#### (Optional) module_assignment_summary.csv
This file is similar in structure to ```student_module_preferences.csv```, and is generated by the app. It can be uploaded under the "Prior Allocs" section to allow the previously allocated modules and remaining capacities to be accounted for in a new round of allocation.
