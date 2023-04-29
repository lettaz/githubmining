# Documentation of data collection / processing

The following sections describe the different steps which were necessary to gather the necessary data for the research of bdd glue code
The date were the code was executed is:


```python
import datetime
print(datetime.datetime.now())
```

    2020-11-30 01:29:45.600423
    

# Import used libraries


```python
# Library to query
from github import Github
# Library to better query and display dataframes
import pandas as pd
# used to get environment variable
import os
# define the github api query object
g = Github(os.environ['GITHUBTOKEN'])
# can be used to make direct http requests 
import requests
# used to serialize and save objects in python
import pickle
```

# Find repositories that contain Gherkin language

Unfortunately the Github API only allows to find repositories (repos) by their main programming language.
Therefore a generic query that returns repositories that can be iterated over is used


## Get Repository list


```python
repos = g.search_repositories(query='stars:>500', sort='stars', order='desc')
repos.totalCount
```




    1000



The pygithub library only returns a totalCount of 1000.

If we do the same query on the Github Homepage the results are a little different

![./repo_count.png](./repo_count.png)



after iterating over this a new request should be made.
The chosen approach ist ot get the stars count of the last repo and limit the next search to repos with stars between `500-<stars_of_last_repo>`


```python
repos = g.search_repositories(query='stars:500..41689', sort='stars', order='desc')
```

## Check if they contain gherkin

To check if a repo contains Gherkin files we check the list of languages returned by the github api
The following code example does work for a small number of requests. But since Github API has a rate limit, the used script is a little more complicated (check `find_gherkin_repos.py`)


```python
contains_gherkin_files = {}
for repo in repos[:5]:
    contains_gherkin_files[repo.full_name] = 'Gherkin' in repo.get_languages()

pd.DataFrame.from_dict(contains_gherkin_files, orient='index', columns=['contains_gherkin'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>contains_gherkin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>jekyll/jekyll</th>
      <td>True</td>
    </tr>
    <tr>
      <th>Hack-with-Github/Awesome-Hacking</th>
      <td>False</td>
    </tr>
    <tr>
      <th>necolas/normalize.css</th>
      <td>False</td>
    </tr>
    <tr>
      <th>scutan90/DeepLearning-500-questions</th>
      <td>False</td>
    </tr>
    <tr>
      <th>google/material-design-icons</th>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



# Gather characteristics of the Repositories




The following generic characteristics are gathered:

- `num_stars`: Number of github stars
- `num_watchers`: Number of watchers
- `num_commits`: Number of commits
- `comments`: Number of comments
- `languages`: Programming languages (sorted by loc)
- `date_of_last_commit`: Date of the last commit

Special characteristics interesting for BDD glue code study:

- **`commits_with_given`: Commits that contain keyword 'Given'**
- **`file_cucumber`: Gherkin Feature files that are used with Cucumber**
- **`file_given`: Files that contain keyword 'Given'**
- **`files_feature`: Files that have the extension '.feature'**

For more details about the characteristics and how they collected can be seen in the scripts:

- [fetch_repo_stats.py](fetch_data/fetch_repo_stats.py)

- [repo_stats.py](fetch_data/repo_stats.py)


## Get Feature Files

The following code shows how the files that have the file extension `.feature` are obtained for the example repository `influxdata/influxdb`.


```python
def get_feature_files(repo):
    files = g.search_code(query=f'repo:{repo.full_name} extension:feature')
    return [ file.path for file in files ]

example_repo = g.get_repo('influxdata/influxdb')
feature_files = get_feature_files(example_repo)
pd.DataFrame(feature_files, columns=[['feature_files']])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>feature_files</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>e2e/features/homePage/homePage.feature</td>
    </tr>
    <tr>
      <th>1</th>
      <td>e2e/features/influx/influx.feature</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e2e/features/loadData/clientlib.feature</td>
    </tr>
    <tr>
      <th>3</th>
      <td>e2e/features/loadData/loadData.feature</td>
    </tr>
    <tr>
      <th>4</th>
      <td>e2e/features/onboarding/onboarding.feature</td>
    </tr>
    <tr>
      <th>5</th>
      <td>e2e/features/settings/labels.feature</td>
    </tr>
    <tr>
      <th>6</th>
      <td>e2e/features/settings/settings.feature</td>
    </tr>
    <tr>
      <th>7</th>
      <td>e2e/features/settings/variables.feature</td>
    </tr>
    <tr>
      <th>8</th>
      <td>e2e/features/signin/signin.feature</td>
    </tr>
    <tr>
      <th>9</th>
      <td>e2e/features/monitoring/history.feature</td>
    </tr>
    <tr>
      <th>10</th>
      <td>e2e/features/dashboards/dashboard.feature</td>
    </tr>
    <tr>
      <th>11</th>
      <td>e2e/features/dashboards/noteCell.feature</td>
    </tr>
    <tr>
      <th>12</th>
      <td>e2e/features/loadData/tokens.feature</td>
    </tr>
    <tr>
      <th>13</th>
      <td>e2e/cloud/features/signin/01_signinPerf.feature</td>
    </tr>
    <tr>
      <th>14</th>
      <td>e2e/cloud/features/signin/02_signinCloud.feature</td>
    </tr>
    <tr>
      <th>15</th>
      <td>e2e/features/dashboards/dashboards.feature</td>
    </tr>
    <tr>
      <th>16</th>
      <td>e2e/features/settings/templates.feature</td>
    </tr>
    <tr>
      <th>17</th>
      <td>e2e/features/dashboards/variables.feature</td>
    </tr>
    <tr>
      <th>18</th>
      <td>e2e/features/dataExplorer/dataExplorer.feature</td>
    </tr>
    <tr>
      <th>19</th>
      <td>e2e/features/loadData/buckets.feature</td>
    </tr>
    <tr>
      <th>20</th>
      <td>e2e/features/monitoring/alerts.feature</td>
    </tr>
    <tr>
      <th>21</th>
      <td>e2e/features/loadData/scrapers.feature</td>
    </tr>
    <tr>
      <th>22</th>
      <td>e2e/features/loadData/telegrafs.feature</td>
    </tr>
    <tr>
      <th>23</th>
      <td>e2e/features/dashboards/cellEdit.feature</td>
    </tr>
  </tbody>
</table>
</div>



<https://github.com/influxdata/influxdb/blob/master/e2e/features/homePage/homePage.feature>


```python
# either the manual gathered stuff
with open('./data/manual_feature_file_list.pickle', 'rb') as file:
    repos= pickle.load(file)
df = pd.DataFrame(repos)
df[['full_name','num_stars','num_commits','languages']].head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>full_name</th>
      <th>num_stars</th>
      <th>num_commits</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>torvalds/linux</td>
      <td>101487</td>
      <td>968187</td>
      <td>{'C': 853161843, 'C++': 11419111, 'Assembly': ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>iluwatar/java-design-patterns</td>
      <td>62226</td>
      <td>2978</td>
      <td>{'Java': 3390250, 'HTML': 20964, 'CSS': 11102,...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>jekyll/jekyll</td>
      <td>41693</td>
      <td>11200</td>
      <td>{'Ruby': 696738, 'Gherkin': 224137, 'JavaScrip...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>eugenp/tutorials</td>
      <td>23873</td>
      <td>21061</td>
      <td>{'Java': 18787946, 'JavaScript': 2946071, 'HTM...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>hashicorp/consul</td>
      <td>20701</td>
      <td>13255</td>
      <td>{'Go': 10117251, 'JavaScript': 1082533, 'SCSS'...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>github/hub</td>
      <td>20563</td>
      <td>3306</td>
      <td>{'Go': 358904, 'Gherkin': 289265, 'Shell': 392...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>influxdata/influxdb</td>
      <td>20017</td>
      <td>34431</td>
      <td>{'Go': 14021584, 'TypeScript': 3730682, 'JavaS...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>elastic/kibana</td>
      <td>15187</td>
      <td>38439</td>
      <td>{'TypeScript': 76447863, 'JavaScript': 1268035...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>diaspora/diaspora</td>
      <td>12570</td>
      <td>20279</td>
      <td>{'Ruby': 2291824, 'JavaScript': 754337, 'Haml'...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>nextcloud/server</td>
      <td>12369</td>
      <td>56380</td>
      <td>{'PHP': 19363568, 'JavaScript': 10473779, 'Vue...</td>
    </tr>
  </tbody>
</table>
</div>




```python
from IPython.display import display
for repo in repos:
    temp_df = pd.DataFrame(data=repo['feature_files'], columns=[repo['full_name']])
    display(temp_df)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>torvalds/linux</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tools/build/Makefile.feature</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>iluwatar/java-design-patterns</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>naked-objects/integtests/src/test/java/domaina...</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>jekyll/jekyll</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>features/collections_dir.feature</td>
    </tr>
    <tr>
      <th>1</th>
      <td>features/data.feature</td>
    </tr>
    <tr>
      <th>2</th>
      <td>features/drafts.feature</td>
    </tr>
    <tr>
      <th>3</th>
      <td>features/frontmatter_defaults.feature</td>
    </tr>
    <tr>
      <th>4</th>
      <td>features/include_tag.feature</td>
    </tr>
    <tr>
      <th>5</th>
      <td>features/incremental_rebuild.feature</td>
    </tr>
    <tr>
      <th>6</th>
      <td>features/layout_data.feature</td>
    </tr>
    <tr>
      <th>7</th>
      <td>features/site_data.feature</td>
    </tr>
    <tr>
      <th>8</th>
      <td>features/highlighting.feature</td>
    </tr>
    <tr>
      <th>9</th>
      <td>features/create_sites.feature</td>
    </tr>
    <tr>
      <th>10</th>
      <td>features/embed_filters.feature</td>
    </tr>
    <tr>
      <th>11</th>
      <td>features/link_tag.feature</td>
    </tr>
    <tr>
      <th>12</th>
      <td>features/include_relative_tag.feature</td>
    </tr>
    <tr>
      <th>13</th>
      <td>features/post_excerpts.feature</td>
    </tr>
    <tr>
      <th>14</th>
      <td>features/collections.feature</td>
    </tr>
    <tr>
      <th>15</th>
      <td>features/theme_configuration.feature</td>
    </tr>
    <tr>
      <th>16</th>
      <td>features/post_data.feature</td>
    </tr>
    <tr>
      <th>17</th>
      <td>features/cache.feature</td>
    </tr>
    <tr>
      <th>18</th>
      <td>features/theme.feature</td>
    </tr>
    <tr>
      <th>19</th>
      <td>features/theme_gem.feature</td>
    </tr>
    <tr>
      <th>20</th>
      <td>features/markdown.feature</td>
    </tr>
    <tr>
      <th>21</th>
      <td>features/pagination.feature</td>
    </tr>
    <tr>
      <th>22</th>
      <td>features/plugins.feature</td>
    </tr>
    <tr>
      <th>23</th>
      <td>features/post_url_tag.feature</td>
    </tr>
    <tr>
      <th>24</th>
      <td>features/site_configuration.feature</td>
    </tr>
    <tr>
      <th>25</th>
      <td>features/permalinks.feature</td>
    </tr>
    <tr>
      <th>26</th>
      <td>features/rendering.feature</td>
    </tr>
    <tr>
      <th>27</th>
      <td>features/hooks.feature</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>eugenp/tutorials</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>spring-cucumber/src/test/resources/baelung.fea...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>spring-cucumber/src/test/resources/version.fea...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>testing-modules/testing-libraries/src/test/res...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>testing-modules/rest-testing/src/test/resource...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>testing-modules/testing-libraries/src/test/res...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>testing-modules/testing-libraries/src/test/res...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>testing-modules/testing-libraries/src/test/res...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>testing-modules/testing-libraries/src/test/res...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>testing-modules/testing-libraries/src/test/res...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>testing-modules/testing-libraries/src/test/res...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>testing-modules/rest-testing/src/test/resource...</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>hashicorp/consul</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ui/packages/consul-ui/tests/acceptance/compone...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ui/packages/consul-ui/tests/acceptance/compone...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ui/packages/consul-ui/tests/acceptance/compone...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ui/packages/consul-ui/tests/acceptance/compone...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ui/packages/consul-ui/tests/acceptance/compone...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>103</th>
      <td>ui/packages/consul-ui/tests/acceptance/dc/serv...</td>
    </tr>
    <tr>
      <th>104</th>
      <td>ui/packages/consul-ui/tests/acceptance/dc/serv...</td>
    </tr>
    <tr>
      <th>105</th>
      <td>ui/packages/consul-ui/tests/acceptance/dc/serv...</td>
    </tr>
    <tr>
      <th>106</th>
      <td>ui/packages/consul-ui/tests/acceptance/page-na...</td>
    </tr>
    <tr>
      <th>107</th>
      <td>ui/packages/consul-ui/tests/acceptance/dc/serv...</td>
    </tr>
  </tbody>
</table>
<p>108 rows × 1 columns</p>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>github/hub</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>features/alias.feature</td>
    </tr>
    <tr>
      <th>1</th>
      <td>features/cherry_pick.feature</td>
    </tr>
    <tr>
      <th>2</th>
      <td>features/delete.feature</td>
    </tr>
    <tr>
      <th>3</th>
      <td>features/fish_completion.feature</td>
    </tr>
    <tr>
      <th>4</th>
      <td>features/init.feature</td>
    </tr>
    <tr>
      <th>5</th>
      <td>features/merge.feature</td>
    </tr>
    <tr>
      <th>6</th>
      <td>features/push.feature</td>
    </tr>
    <tr>
      <th>7</th>
      <td>features/zsh_completion.feature</td>
    </tr>
    <tr>
      <th>8</th>
      <td>features/ci_status.feature</td>
    </tr>
    <tr>
      <th>9</th>
      <td>features/pr-checkout.feature</td>
    </tr>
    <tr>
      <th>10</th>
      <td>features/git_compatibility.feature</td>
    </tr>
    <tr>
      <th>11</th>
      <td>features/pr-list.feature</td>
    </tr>
    <tr>
      <th>12</th>
      <td>features/help.feature</td>
    </tr>
    <tr>
      <th>13</th>
      <td>features/checkout.feature</td>
    </tr>
    <tr>
      <th>14</th>
      <td>features/bash_completion.feature</td>
    </tr>
    <tr>
      <th>15</th>
      <td>features/release.feature</td>
    </tr>
    <tr>
      <th>16</th>
      <td>features/compare.feature</td>
    </tr>
    <tr>
      <th>17</th>
      <td>features/create.feature</td>
    </tr>
    <tr>
      <th>18</th>
      <td>features/am.feature</td>
    </tr>
    <tr>
      <th>19</th>
      <td>features/apply.feature</td>
    </tr>
    <tr>
      <th>20</th>
      <td>features/browse.feature</td>
    </tr>
    <tr>
      <th>21</th>
      <td>features/clone.feature</td>
    </tr>
    <tr>
      <th>22</th>
      <td>features/fetch.feature</td>
    </tr>
    <tr>
      <th>23</th>
      <td>features/fork.feature</td>
    </tr>
    <tr>
      <th>24</th>
      <td>features/remote_add.feature</td>
    </tr>
    <tr>
      <th>25</th>
      <td>features/submodule_add.feature</td>
    </tr>
    <tr>
      <th>26</th>
      <td>features/sync.feature</td>
    </tr>
    <tr>
      <th>27</th>
      <td>features/issue-transfer.feature</td>
    </tr>
    <tr>
      <th>28</th>
      <td>features/api.feature</td>
    </tr>
    <tr>
      <th>29</th>
      <td>features/authentication.feature</td>
    </tr>
    <tr>
      <th>30</th>
      <td>features/gist.feature</td>
    </tr>
    <tr>
      <th>31</th>
      <td>features/pull_request.feature</td>
    </tr>
    <tr>
      <th>32</th>
      <td>features/pr-show.feature</td>
    </tr>
    <tr>
      <th>33</th>
      <td>features/issue.feature</td>
    </tr>
    <tr>
      <th>34</th>
      <td>features/pr-merge.feature</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>influxdata/influxdb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>e2e/features/homePage/homePage.feature</td>
    </tr>
    <tr>
      <th>1</th>
      <td>e2e/features/influx/influx.feature</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e2e/features/loadData/clientlib.feature</td>
    </tr>
    <tr>
      <th>3</th>
      <td>e2e/features/loadData/loadData.feature</td>
    </tr>
    <tr>
      <th>4</th>
      <td>e2e/features/onboarding/onboarding.feature</td>
    </tr>
    <tr>
      <th>5</th>
      <td>e2e/features/settings/labels.feature</td>
    </tr>
    <tr>
      <th>6</th>
      <td>e2e/features/settings/settings.feature</td>
    </tr>
    <tr>
      <th>7</th>
      <td>e2e/features/settings/variables.feature</td>
    </tr>
    <tr>
      <th>8</th>
      <td>e2e/features/signin/signin.feature</td>
    </tr>
    <tr>
      <th>9</th>
      <td>e2e/features/monitoring/history.feature</td>
    </tr>
    <tr>
      <th>10</th>
      <td>e2e/features/dashboards/dashboard.feature</td>
    </tr>
    <tr>
      <th>11</th>
      <td>e2e/features/dashboards/noteCell.feature</td>
    </tr>
    <tr>
      <th>12</th>
      <td>e2e/features/loadData/tokens.feature</td>
    </tr>
    <tr>
      <th>13</th>
      <td>e2e/cloud/features/signin/01_signinPerf.feature</td>
    </tr>
    <tr>
      <th>14</th>
      <td>e2e/cloud/features/signin/02_signinCloud.feature</td>
    </tr>
    <tr>
      <th>15</th>
      <td>e2e/features/dashboards/dashboards.feature</td>
    </tr>
    <tr>
      <th>16</th>
      <td>e2e/features/settings/templates.feature</td>
    </tr>
    <tr>
      <th>17</th>
      <td>e2e/features/dashboards/variables.feature</td>
    </tr>
    <tr>
      <th>18</th>
      <td>e2e/features/dataExplorer/dataExplorer.feature</td>
    </tr>
    <tr>
      <th>19</th>
      <td>e2e/features/loadData/buckets.feature</td>
    </tr>
    <tr>
      <th>20</th>
      <td>e2e/features/monitoring/alerts.feature</td>
    </tr>
    <tr>
      <th>21</th>
      <td>e2e/features/loadData/scrapers.feature</td>
    </tr>
    <tr>
      <th>22</th>
      <td>e2e/features/loadData/telegrafs.feature</td>
    </tr>
    <tr>
      <th>23</th>
      <td>e2e/features/dashboards/cellEdit.feature</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>elastic/kibana</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>x-pack/plugins/apm/e2e/cypress/integration/csm...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>x-pack/plugins/apm/e2e/cypress/integration/apm...</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>diaspora/diaspora</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>features/desktop/activity_stream.feature</td>
    </tr>
    <tr>
      <th>1</th>
      <td>features/desktop/aspect_navigation.feature</td>
    </tr>
    <tr>
      <th>2</th>
      <td>features/desktop/blocks_user.feature</td>
    </tr>
    <tr>
      <th>3</th>
      <td>features/desktop/change_settings.feature</td>
    </tr>
    <tr>
      <th>4</th>
      <td>features/desktop/closes_account.feature</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>65</th>
      <td>features/desktop/registrations.feature</td>
    </tr>
    <tr>
      <th>66</th>
      <td>features/mobile/registrations.feature</td>
    </tr>
    <tr>
      <th>67</th>
      <td>features/desktop/post_with_a_poll.feature</td>
    </tr>
    <tr>
      <th>68</th>
      <td>features/mobile/drawer.feature</td>
    </tr>
    <tr>
      <th>69</th>
      <td>features/desktop/diaspora_links_resolve.feature</td>
    </tr>
  </tbody>
</table>
<p>70 rows × 1 columns</p>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nextcloud/server</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tests/acceptance/features/apps.feature</td>
    </tr>
    <tr>
      <th>1</th>
      <td>build/integration/ldap_features/ldap-openldap....</td>
    </tr>
    <tr>
      <th>2</th>
      <td>build/integration/ldap_features/openldap-numer...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>build/integration/capabilities_features/capabi...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>build/integration/features/auth.feature</td>
    </tr>
    <tr>
      <th>5</th>
      <td>build/integration/features/caldav.feature</td>
    </tr>
    <tr>
      <th>6</th>
      <td>build/integration/features/checksums.feature</td>
    </tr>
    <tr>
      <th>7</th>
      <td>build/integration/features/download.feature</td>
    </tr>
    <tr>
      <th>8</th>
      <td>build/integration/features/maintenance-mode.fe...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>build/integration/features/ocs-v1.feature</td>
    </tr>
    <tr>
      <th>10</th>
      <td>build/integration/features/ratelimiting.feature</td>
    </tr>
    <tr>
      <th>11</th>
      <td>build/integration/filesdrop_features/filesdrop...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>build/integration/ldap_features/ldap-ocs.feature</td>
    </tr>
    <tr>
      <th>13</th>
      <td>build/integration/remoteapi_features/remote.fe...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>build/integration/setup_features/setup.feature</td>
    </tr>
    <tr>
      <th>15</th>
      <td>build/integration/federation_features/federate...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>tests/acceptance/features/access-levels.feature</td>
    </tr>
    <tr>
      <th>17</th>
      <td>build/integration/features/trashbin.feature</td>
    </tr>
    <tr>
      <th>18</th>
      <td>tests/acceptance/features/app-theming.feature</td>
    </tr>
    <tr>
      <th>19</th>
      <td>build/integration/features/comments-search.fea...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>build/integration/features/comments.feature</td>
    </tr>
    <tr>
      <th>21</th>
      <td>build/integration/features/dav-v2.feature</td>
    </tr>
    <tr>
      <th>22</th>
      <td>build/integration/features/favorites.feature</td>
    </tr>
    <tr>
      <th>23</th>
      <td>build/integration/features/tags.feature</td>
    </tr>
    <tr>
      <th>24</th>
      <td>build/integration/sharing_features/sharing-v1-...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>build/integration/sharees_features/sharees_pro...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>build/integration/ldap_features/openldap-uid-u...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>tests/acceptance/features/app-comments.feature</td>
    </tr>
    <tr>
      <th>28</th>
      <td>tests/acceptance/features/app-files-tags.feature</td>
    </tr>
    <tr>
      <th>29</th>
      <td>build/integration/sharing_features/sharing-v1-...</td>
    </tr>
    <tr>
      <th>30</th>
      <td>build/integration/sharees_features/sharees.fea...</td>
    </tr>
    <tr>
      <th>31</th>
      <td>build/integration/features/external-storage.fe...</td>
    </tr>
    <tr>
      <th>32</th>
      <td>build/integration/features/webdav-related.feature</td>
    </tr>
    <tr>
      <th>33</th>
      <td>build/integration/sharing_features/sharing-v1-...</td>
    </tr>
    <tr>
      <th>34</th>
      <td>tests/acceptance/features/login.feature</td>
    </tr>
    <tr>
      <th>35</th>
      <td>tests/acceptance/features/users.feature</td>
    </tr>
    <tr>
      <th>36</th>
      <td>build/integration/features/provisioning-v2.fea...</td>
    </tr>
    <tr>
      <th>37</th>
      <td>build/integration/features/provisioning-v1.fea...</td>
    </tr>
    <tr>
      <th>38</th>
      <td>build/integration/features/carddav.feature</td>
    </tr>
    <tr>
      <th>39</th>
      <td>tests/acceptance/features/header.feature</td>
    </tr>
    <tr>
      <th>40</th>
      <td>build/integration/sharing_features/sharing-v1....</td>
    </tr>
    <tr>
      <th>41</th>
      <td>tests/acceptance/features/app-files.feature</td>
    </tr>
    <tr>
      <th>42</th>
      <td>tests/acceptance/features/app-files-sharing-li...</td>
    </tr>
    <tr>
      <th>43</th>
      <td>tests/acceptance/features/app-files-sharing.fe...</td>
    </tr>
    <tr>
      <th>44</th>
      <td>build/integration/features/transfer-ownership....</td>
    </tr>
  </tbody>
</table>
</div>



```python

```
