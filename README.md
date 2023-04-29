# An Exploratory Study on the Characteristics of Gherkin Specifications in Open-Source Projects

This is a step by step guide to recreate the dataset and experiments that we performed for this paper.


# Setting up the Anaconda Environment

1) `conda env create --file environment.yml`

2) `conda activate plot`

3) `jupyter notebook`

## Fetch files that use Gherkin

Inside the folder *software-composition-seminar*,

1) Execute the code in the file **Documentation.md** to fetch all the repos using Gherkin and having more than 500 stars.

2) Execute the code in the file **Analytics_on_repo.md** to extract all the meta level statistics on the spec files identified in step 1.

## Statistics on spec files

Execute the file **Parsing_feat_files.md** to perform statistics on the meta stats extracted from the spec files in the previous step.

The *repo_stats.json* is a json file that contains all the meta stats for each feature file in each repository.

## Visualisation

The visualisation of the results are provided in the .svg files.
