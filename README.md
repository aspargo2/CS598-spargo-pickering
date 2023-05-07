# CS598-spargo-pickering
CS598 final project

There are 2 repositories necessary to reproduce the findings of MIMIC-Extract.
1. https://github.com/MIT-LCP/mimic-code/tree/main/mimic-iii : This is the repo with scripts to set up a postgresql database with all the tables and entries from mimic-iii.
2. https://github.com/MLforHealth/MIMIC_Extract : This is the repo with scripts to convert the mimic-iii data to timeseries data, and python notebooks to test MIMIC-Extract against other data processing pipelines.

Both repositories have set up scripts that are outdated, and so we've updated the scripts and uploaded them to this repository. In order to set up MIMIC-Extract, you will need to download those repositories, and replace their files with the ones in our repository. These are the following steps that we followed in order to run the full MIMIC-Extract Pipeline.
1. Setting up MIMIC-III psql relational database, as per instructions in https://github.com/MIT-LCP/mimic-code/tree/main/mimic-iii
    a. Download MIMIC-III data, and update setup_user_env.sh with the paths to the data files.
    b. run `sh setup_user_env.sh` to create a user in the database
    c. run `make mimic-build` to populate psql with data

2. Running the MIMIC-Extract pipeline : the output will be a folder of timeseries data in the format of .h5 and .csv files. These files are necessary to run the mechanical ventilation models.
    a. run `psql -d mimic -f postgres-functions.sql`
    b. run `sh postgres_make_concepts.sh`
    c. run `bash postgres_make_extended_concepts.sh`
    d. run `psql -d mimic -f niv-durations.sql`
    e. run `bash build_curated_from_psql.sh`

3. Running the MIMIC-Extract Notebooks: There is only one working jupyter notebook - Mechanical Ventilation. All other notebooks have outdated pickle files, and were not reproducible from scratch.
    a. move our `Baselines for Intervention Prediction - Mechanical Ventilation.ipynb` into the MIMIC-Extract/notebooks folder
    b. run all cells. This will run all the models using the timeseries data created from MIMIC-Extract, and print values for AUC, Macro_AUC, Accuracy, F1 Macro, and AUPRC Macro
