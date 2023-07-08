# Code and data organization

Clone the repository to a "root" folder, such as
MYROOT_FOLDER\ml_projects\skin_cancer_classification

Download the dataset from
https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000
and unzip the file into a folder called data_ham1000.

The JPEG (.jpg) files are stored in folders HAM10000_images_part_1 and
HAM10000_images_part_2. Copy all contents from HAM10000_images_part_2 to
HAM10000_images_part_1, such that all JPG files are into a unique folder.

Move data_ham1000 to MYROOT_FOLDER, such that the JPG images
are at ../../data_ham1000/HAM10000_images_part_1 with respect to your
scripts.

You should have:
MYROOT_FOLDER/ml_projects/skin_cancer_classification/organize_data.py (and other scripts)

MYROOT_FOLDER/data_ham1000/HAM10000_images_part_1

MYROOT_FOLDER/data_ham1000/HAM10000_metadata.csv

Execute organize_data.py to create:

MYROOT_FOLDER/data_ham1000/binary_HAM10000_metadata.csv

MYROOT_FOLDER/data_ham1000/train.csv

MYROOT_FOLDER/data_ham1000/test.csv

MYROOT_FOLDER/data_ham1000/validation.csv

the three distinct CSV files, with train, test and validation sets.

# Executing the code to train and test models

You can either use a neural network completely designed by yourself, or you can use transfer learning. In the latter case, you will adopt a "backend model" that performs feature extraction and add the top layers to perform classification based on these features.

## Using Optuna

Execute model_selection_no_backend.py to find hyperparameters.

## Using Optuna with the outputs of a backend neural network

First execute save_backend_output.py to create Pickle files with the outputs of the backend neural network. And then start working with these features. This will save substantial time.

Now execute model_selection_backend_outputs.py to find hyperparameters.

## After choosing your model and hyperparameters

One can train a single neural network, without running Optuna, using the script
single_model_train_test.py.

# Running Optuna dashboard and Tensorboard

## Part I - How the Optuna dashboard works when running the model_selection_backend_outputs.py code:

1) to save the information of each "study" in a database, notice the optuna.create_study method at line 356 of the model_selection_backend_outputs.py script. An input argument of this method is
storage="sqlite:///../../outputs/optuna_db.sqlite3", # Specify the storage URL here.
that is, you must look for the optuna_db.sqlite3 file in the indicated folder. This file contains the data.

2) To install the Optuna dashboard, one should look at https://github.com/optuna/optuna-dashboard
In short is to use:
``pip install optuna-dashboard``

3) Then, to run the Optuna dashboard, go to the command line (cmd on Windows), use cd until you reach the folder where the optuna_db.sqlite3 file is located, and then use:
``optuna-dashboard sqlite:///optuna_db.sqlite3``
after that you have to open the browser and paste the suggested address, for example http://127.0.0.1:8080/

## Part II - How the Tensorboard works

1) To find the directory where Tensorboard is saving the files, look for the callback creation. For example, in model_selection_backend_outputs.py code: it's around line 238:
tensorboard = TensorBoard
and look for the argument log_dir= '.\logoptuna'.
In this case, the folder is called .\logoptuna'. In model_selection_no_backend.py, the folder is:
log_dir= '../outputs/tensorboard_logs'

2) After installing Tensorboard, to run it, go to the command line (cmd on Windows) and run
``tensorboard --logdir mylogdir``
where mylogdir is the folder, such as tensorboard_logs (inform the full path if it is not in the folder that houses your log folder (mylogdir).
After that, you have to open the browser and paste the address suggested by Tensorboard.

# Python template

Python template containing formatter, linter and other automatic verifications. First you need to install the Conda environment with required packages using `conda env create -f env.yml` (if you want to change the name of the environment from env to something else such as your_env_name, edit the entry "name" in the env.yml file). After successfully creating the environment, activate it using the `conda activate` command, such `conda activate your_env_name` (after this, all your commands will be executed into the python environment). Finally, execute the command 
`pre-commit install` to activate the pre-commit, so every time you make a commit it will verify your code and assure that you complied with all project standards.

## Using this template

When creating a new repository on GitHub, be sure to select in the template field `python_template` from LASSE organization. So, all the files in this template will be moved to your new project.

Use the --no-verify option to skip git commit hooks, e.g. ``git commit -m "commit message" --no-verify``. When the --no-verify option is used, the pre-commit and commit-msg hooks are bypassed.

## VS Code integration

File `.vscode/settings.json` contains the default workspace configurations to automatically activate the formatter, linter, type check and sort imports in VS Code. Most of them promote file verification when saving the document. *Remember to select the correct python environment into the VS Code to enable it to use the packages installed into the environment*.

## GitHub Actions

Into `.github/` folder, there are specifications to GitHub actions to verify if the pushed commits are compliant with the project standards. You may need to activate GitHub actions in your repo to enable this verification.
