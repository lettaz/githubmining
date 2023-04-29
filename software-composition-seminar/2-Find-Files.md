```python
import pickle
import pandas as pd
from github import Github
import os
g = Github('f9b312d6ab0fa60f7a52bfb89a24991df82acc5c')
```


```python
def save_to_file(data, name='repos'):
    with open(f'./data/{name}.pickle', 'wb') as file:
        pickle.dump(data, file)
```


```python
# either the manual gathered stuff
with open('./data/manual_feature_file_list.pickle', 'rb') as file:
    repo_stats = {
        'repos': pickle.load(file)
    }
df = pd.DataFrame(repo_stats['repos'])
df.head(10)
```




    [{'Go': 14021584,
      'TypeScript': 3730682,
      'JavaScript': 679270,
      'SCSS': 216574,
      'Gherkin': 189456,
      'Python': 42767,
      'Shell': 41532,
      'Makefile': 16739,
      'Dockerfile': 1798,
      'HTML': 804}]




```python
# or the complete dataset
with open('./data/repo_stats.pickle', 'rb') as file:
    repo_stats = pickle.load(file)
df = pd.DataFrame(repo_stats['repos'])
df.head(10)
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
      <th>name</th>
      <th>full_name</th>
      <th>url</th>
      <th>fork</th>
      <th>num_forks</th>
      <th>num_contributors</th>
      <th>num_commits</th>
      <th>num_stars</th>
      <th>num_watchers</th>
      <th>commit_activities</th>
      <th>issues_closed</th>
      <th>issues_all</th>
      <th>pull_requests_closed</th>
      <th>pull_requests_all</th>
      <th>comments</th>
      <th>languages</th>
      <th>date_of_last_commit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>linux</td>
      <td>torvalds/linux</td>
      <td>https://github.com/torvalds/linux</td>
      <td>False</td>
      <td>34526</td>
      <td>[403 {"message": "The history or contributor l...</td>
      <td>968187</td>
      <td>101487</td>
      <td>7580</td>
      <td>[{'total': 1964, 'week': 1575763200, 'days': [...</td>
      <td>440</td>
      <td>761</td>
      <td>440</td>
      <td>761</td>
      <td>1172</td>
      <td>{'C': 853161843, 'C++': 11419111, 'Assembly': ...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>java-design-patterns</td>
      <td>iluwatar/java-design-patterns</td>
      <td>https://github.com/iluwatar/java-design-patterns</td>
      <td>False</td>
      <td>19820</td>
      <td>212</td>
      <td>2978</td>
      <td>62226</td>
      <td>3993</td>
      <td>[{'total': 4, 'week': 1575763200, 'days': [0, ...</td>
      <td>1322</td>
      <td>1583</td>
      <td>905</td>
      <td>938</td>
      <td>39</td>
      <td>{'Java': 3390250, 'HTML': 20964, 'CSS': 11102,...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>jekyll</td>
      <td>jekyll/jekyll</td>
      <td>https://github.com/jekyll/jekyll</td>
      <td>False</td>
      <td>9115</td>
      <td>402</td>
      <td>11200</td>
      <td>41693</td>
      <td>1473</td>
      <td>[{'total': 5, 'week': 1575763200, 'days': [0, ...</td>
      <td>8185</td>
      <td>8334</td>
      <td>3944</td>
      <td>4014</td>
      <td>556</td>
      <td>{'Ruby': 696738, 'Gherkin': 224137, 'JavaScrip...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tutorials</td>
      <td>eugenp/tutorials</td>
      <td>https://github.com/eugenp/tutorials</td>
      <td>False</td>
      <td>38199</td>
      <td>323</td>
      <td>21061</td>
      <td>23873</td>
      <td>1540</td>
      <td>[{'total': 81, 'week': 1575763200, 'days': [17...</td>
      <td>10260</td>
      <td>10291</td>
      <td>9834</td>
      <td>9855</td>
      <td>26</td>
      <td>{'Java': 18787946, 'JavaScript': 2946071, 'HTM...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>consul</td>
      <td>hashicorp/consul</td>
      <td>https://github.com/hashicorp/consul</td>
      <td>False</td>
      <td>3544</td>
      <td>379</td>
      <td>13255</td>
      <td>20701</td>
      <td>983</td>
      <td>[{'total': 35, 'week': 1575763200, 'days': [0,...</td>
      <td>8415</td>
      <td>9211</td>
      <td>5041</td>
      <td>5146</td>
      <td>195</td>
      <td>{'Go': 10117251, 'JavaScript': 1082533, 'SCSS'...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>5</th>
      <td>hub</td>
      <td>github/hub</td>
      <td>https://github.com/github/hub</td>
      <td>False</td>
      <td>2141</td>
      <td>218</td>
      <td>3306</td>
      <td>20563</td>
      <td>477</td>
      <td>[{'total': 2, 'week': 1575763200, 'days': [0, ...</td>
      <td>2325</td>
      <td>2567</td>
      <td>739</td>
      <td>781</td>
      <td>178</td>
      <td>{'Go': 358904, 'Gherkin': 289265, 'Shell': 392...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>6</th>
      <td>influxdb</td>
      <td>influxdata/influxdb</td>
      <td>https://github.com/influxdata/influxdb</td>
      <td>False</td>
      <td>2825</td>
      <td>323</td>
      <td>34431</td>
      <td>20017</td>
      <td>755</td>
      <td>[{'total': 49, 'week': 1575763200, 'days': [0,...</td>
      <td>18772</td>
      <td>20021</td>
      <td>9096</td>
      <td>9218</td>
      <td>351</td>
      <td>{'Go': 14021584, 'TypeScript': 3730682, 'JavaS...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>7</th>
      <td>kibana</td>
      <td>elastic/kibana</td>
      <td>https://github.com/elastic/kibana</td>
      <td>False</td>
      <td>6167</td>
      <td>353</td>
      <td>38439</td>
      <td>15187</td>
      <td>847</td>
      <td>[{'total': 168, 'week': 1575763200, 'days': [0...</td>
      <td>76758</td>
      <td>84422</td>
      <td>54235</td>
      <td>54703</td>
      <td>366</td>
      <td>{'TypeScript': 76447863, 'JavaScript': 1268035...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>8</th>
      <td>diaspora</td>
      <td>diaspora/diaspora</td>
      <td>https://github.com/diaspora/diaspora</td>
      <td>False</td>
      <td>2915</td>
      <td>281</td>
      <td>20279</td>
      <td>12570</td>
      <td>502</td>
      <td>[{'total': 0, 'week': 1575763200, 'days': [0, ...</td>
      <td>7702</td>
      <td>8151</td>
      <td>3491</td>
      <td>3521</td>
      <td>1727</td>
      <td>{'Ruby': 2291824, 'JavaScript': 754337, 'Haml'...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>9</th>
      <td>server</td>
      <td>nextcloud/server</td>
      <td>https://github.com/nextcloud/server</td>
      <td>False</td>
      <td>2291</td>
      <td>378</td>
      <td>56380</td>
      <td>12369</td>
      <td>509</td>
      <td>[{'total': 57, 'week': 1575763200, 'days': [10...</td>
      <td>21473</td>
      <td>24118</td>
      <td>12535</td>
      <td>12705</td>
      <td>140</td>
      <td>{'PHP': 19363568, 'JavaScript': 10473779, 'Vue...</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>




```python
# feature_files
from fetch_data.repo_stats import get_feature_files

for repo_data in repo_stats['repos'][:10]:
    repo = g.get_repo(repo_data['full_name'])
    repo_data["feature_files"] = get_feature_files(repo, g)
    
repo_stats['repos'][:10]
```




    [{'name': 'linux',
      'full_name': 'torvalds/linux',
      'url': 'https://github.com/torvalds/linux',
      'fork': False,
      'num_forks': 34526,
      'num_contributors': [github.GithubException.GithubException(403,
                                              {'message': 'The history or contributor list is too large to list contributors for this repository via the API.',
                                               'documentation_url': 'https://docs.github.com/rest/reference/repos#list-repository-contributors'}),
       github.GithubException.GithubException(403,
                                              {'message': 'The history or contributor list is too large to list contributors for this repository via the API.',
                                               'documentation_url': 'https://docs.github.com/rest/reference/repos#list-repository-contributors'})],
      'num_commits': 968187,
      'num_stars': 101487,
      'num_watchers': 7580,
      'commit_activities': [{'total': 1964,
        'week': 1575763200,
        'days': [36, 369, 510, 346, 275, 327, 101]},
       {'total': 1690,
        'week': 1576368000,
        'days': [78, 312, 415, 377, 253, 182, 73]},
       {'total': 653, 'week': 1576972800, 'days': [90, 158, 153, 50, 79, 76, 47]},
       {'total': 768, 'week': 1577577600, 'days': [46, 102, 56, 54, 191, 230, 89]},
       {'total': 1572,
        'week': 1578182400,
        'days': [131, 293, 233, 298, 271, 258, 88]},
       {'total': 1639,
        'week': 1578787200,
        'days': [75, 302, 301, 456, 258, 202, 45]},
       {'total': 1395,
        'week': 1579392000,
        'days': [75, 260, 251, 255, 257, 231, 66]},
       {'total': 1065,
        'week': 1579996800,
        'days': [71, 186, 151, 180, 260, 168, 49]},
       {'total': 1111,
        'week': 1580601600,
        'days': [64, 245, 175, 167, 239, 175, 46]},
       {'total': 1543,
        'week': 1581206400,
        'days': [68, 277, 311, 307, 211, 270, 99]},
       {'total': 1539,
        'week': 1581811200,
        'days': [98, 241, 316, 295, 239, 271, 79]},
       {'total': 1777,
        'week': 1582416000,
        'days': [97, 339, 311, 332, 326, 296, 76]},
       {'total': 1722,
        'week': 1583020800,
        'days': [63, 403, 231, 330, 346, 270, 79]},
       {'total': 1791,
        'week': 1583625600,
        'days': [88, 281, 352, 331, 355, 316, 68]},
       {'total': 1553,
        'week': 1584230400,
        'days': [59, 278, 254, 222, 295, 312, 133]},
       {'total': 1867,
        'week': 1584835200,
        'days': [94, 292, 337, 366, 375, 303, 100]},
       {'total': 1436,
        'week': 1585440000,
        'days': [98, 210, 198, 367, 247, 244, 72]},
       {'total': 1280,
        'week': 1586044800,
        'days': [105, 342, 181, 209, 211, 172, 60]},
       {'total': 1476,
        'week': 1586649600,
        'days': [67, 191, 268, 357, 230, 245, 118]},
       {'total': 1830,
        'week': 1587254400,
        'days': [116, 352, 303, 320, 231, 338, 170]},
       {'total': 1984,
        'week': 1587859200,
        'days': [109, 334, 349, 370, 425, 330, 67]},
       {'total': 1979,
        'week': 1588464000,
        'days': [116, 330, 316, 308, 447, 274, 188]},
       {'total': 1835,
        'week': 1589068800,
        'days': [108, 315, 325, 322, 304, 370, 91]},
       {'total': 1695,
        'week': 1589673600,
        'days': [64, 238, 432, 281, 303, 269, 108]},
       {'total': 1833,
        'week': 1590278400,
        'days': [77, 224, 284, 361, 437, 345, 105]},
       {'total': 1544,
        'week': 1590883200,
        'days': [81, 344, 181, 402, 294, 198, 44]},
       {'total': 1232,
        'week': 1591488000,
        'days': [110, 234, 199, 183, 219, 217, 70]},
       {'total': 1536,
        'week': 1592092800,
        'days': [63, 358, 272, 261, 252, 186, 144]},
       {'total': 1568,
        'week': 1592697600,
        'days': [111, 282, 341, 276, 264, 226, 68]},
       {'total': 1989,
        'week': 1593302400,
        'days': [108, 452, 281, 397, 353, 347, 51]},
       {'total': 1727,
        'week': 1593907200,
        'days': [89, 334, 301, 321, 329, 272, 81]},
       {'total': 1930,
        'week': 1594512000,
        'days': [64, 409, 365, 290, 327, 343, 132]},
       {'total': 2179,
        'week': 1595116800,
        'days': [246, 362, 365, 308, 456, 306, 136]},
       {'total': 1385,
        'week': 1595721600,
        'days': [80, 295, 291, 201, 284, 187, 47]},
       {'total': 1233,
        'week': 1596326400,
        'days': [72, 181, 220, 143, 397, 167, 53]},
       {'total': 1146,
        'week': 1596931200,
        'days': [71, 161, 305, 144, 173, 239, 53]},
       {'total': 1571,
        'week': 1597536000,
        'days': [31, 341, 310, 291, 231, 279, 88]},
       {'total': 1488,
        'week': 1598140800,
        'days': [97, 213, 300, 310, 268, 180, 120]},
       {'total': 1659,
        'week': 1598745600,
        'days': [102, 217, 331, 278, 375, 279, 77]},
       {'total': 1667,
        'week': 1599350400,
        'days': [53, 357, 280, 238, 373, 296, 70]},
       {'total': 1747,
        'week': 1599955200,
        'days': [73, 328, 273, 327, 303, 340, 103]},
       {'total': 1545,
        'week': 1600560000,
        'days': [109, 334, 209, 225, 258, 284, 126]},
       {'total': 1316,
        'week': 1601164800,
        'days': [108, 261, 323, 236, 160, 169, 59]},
       {'total': 792,
        'week': 1601769600,
        'days': [71, 177, 103, 137, 178, 86, 40]},
       {'total': 846, 'week': 1602374400, 'days': [33, 102, 269, 87, 247, 65, 43]},
       {'total': 352, 'week': 1602979200, 'days': [20, 72, 57, 52, 72, 69, 10]},
       {'total': 448, 'week': 1603584000, 'days': [18, 101, 105, 57, 65, 84, 18]},
       {'total': 294, 'week': 1604188800, 'days': [24, 58, 43, 44, 71, 38, 16]},
       {'total': 273, 'week': 1604793600, 'days': [22, 39, 44, 39, 34, 80, 15]},
       {'total': 193, 'week': 1605398400, 'days': [20, 41, 38, 22, 28, 33, 11]},
       {'total': 93, 'week': 1606003200, 'days': [4, 32, 25, 13, 10, 9, 0]},
       {'total': 1, 'week': 1606608000, 'days': [1, 0, 0, 0, 0, 0, 0]}],
      'issues_closed': 440,
      'issues_all': 761,
      'pull_requests_closed': 440,
      'pull_requests_all': 761,
      'comments': 1172,
      'languages': {'C': 853161843,
       'C++': 11419111,
       'Assembly': 9441609,
       'Shell': 2464807,
       'Objective-C': 2299634,
       'Makefile': 2148715,
       'Perl': 1140174,
       'Python': 1135234,
       'Roff': 172913,
       'SmPL': 158894,
       'Yacc': 120698,
       'Lex': 63198,
       'Awk': 45920,
       'UnrealScript': 17732,
       'Gherkin': 8378,
       'M4': 3325,
       'Clojure': 1462,
       'XS': 1239,
       'Raku': 1176,
       'sed': 379},
      'date_of_last_commit': None,
      'feature_files': ['tools/build/Makefile.feature']},
     {'name': 'java-design-patterns',
      'full_name': 'iluwatar/java-design-patterns',
      'url': 'https://github.com/iluwatar/java-design-patterns',
      'fork': False,
      'num_forks': 19820,
      'num_contributors': 212,
      'num_commits': 2978,
      'num_stars': 62226,
      'num_watchers': 3993,
      'commit_activities': [{'total': 4,
        'week': 1575763200,
        'days': [0, 1, 0, 0, 0, 3, 0]},
       {'total': 3, 'week': 1576368000, 'days': [1, 0, 0, 1, 0, 1, 0]},
       {'total': 1, 'week': 1576972800, 'days': [1, 0, 0, 0, 0, 0, 0]},
       {'total': 3, 'week': 1577577600, 'days': [1, 0, 0, 0, 0, 0, 2]},
       {'total': 10, 'week': 1578182400, 'days': [1, 0, 0, 3, 1, 0, 5]},
       {'total': 3, 'week': 1578787200, 'days': [1, 0, 0, 0, 1, 0, 1]},
       {'total': 1, 'week': 1579392000, 'days': [0, 1, 0, 0, 0, 0, 0]},
       {'total': 3, 'week': 1579996800, 'days': [0, 0, 0, 0, 0, 0, 3]},
       {'total': 2, 'week': 1580601600, 'days': [0, 0, 0, 1, 0, 0, 1]},
       {'total': 0, 'week': 1581206400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1581811200, 'days': [1, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1582416000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 8, 'week': 1583020800, 'days': [0, 0, 0, 0, 0, 0, 8]},
       {'total': 1, 'week': 1583625600, 'days': [0, 0, 0, 0, 1, 0, 0]},
       {'total': 0, 'week': 1584230400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 9, 'week': 1584835200, 'days': [0, 1, 0, 4, 2, 1, 1]},
       {'total': 0, 'week': 1585440000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1586044800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 12, 'week': 1586649600, 'days': [12, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1587254400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 2, 'week': 1587859200, 'days': [1, 0, 0, 0, 0, 1, 0]},
       {'total': 0, 'week': 1588464000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1589068800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1589673600, 'days': [0, 1, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1590278400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 2, 'week': 1590883200, 'days': [2, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1591488000, 'days': [0, 0, 0, 1, 0, 0, 0]},
       {'total': 14, 'week': 1592092800, 'days': [14, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1592697600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 2, 'week': 1593302400, 'days': [0, 0, 0, 0, 0, 0, 2]},
       {'total': 7, 'week': 1593907200, 'days': [0, 1, 6, 0, 0, 0, 0]},
       {'total': 8, 'week': 1594512000, 'days': [0, 0, 0, 2, 1, 0, 5]},
       {'total': 26, 'week': 1595116800, 'days': [11, 4, 2, 3, 3, 1, 2]},
       {'total': 157, 'week': 1595721600, 'days': [12, 15, 33, 80, 3, 2, 12]},
       {'total': 30, 'week': 1596326400, 'days': [3, 10, 8, 1, 0, 1, 7]},
       {'total': 45, 'week': 1596931200, 'days': [14, 17, 9, 0, 0, 0, 5]},
       {'total': 30, 'week': 1597536000, 'days': [3, 8, 5, 3, 2, 5, 4]},
       {'total': 41, 'week': 1598140800, 'days': [6, 1, 5, 2, 0, 2, 25]},
       {'total': 26, 'week': 1598745600, 'days': [4, 1, 7, 4, 6, 3, 1]},
       {'total': 18, 'week': 1599350400, 'days': [11, 0, 0, 0, 3, 1, 3]},
       {'total': 15, 'week': 1599955200, 'days': [14, 0, 0, 1, 0, 0, 0]},
       {'total': 3, 'week': 1600560000, 'days': [0, 0, 2, 0, 0, 0, 1]},
       {'total': 5, 'week': 1601164800, 'days': [1, 0, 0, 1, 3, 0, 0]},
       {'total': 9, 'week': 1601769600, 'days': [7, 0, 1, 0, 0, 0, 1]},
       {'total': 6, 'week': 1602374400, 'days': [2, 0, 2, 0, 0, 1, 1]},
       {'total': 0, 'week': 1602979200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1603584000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 6, 'week': 1604188800, 'days': [0, 0, 0, 0, 0, 0, 6]},
       {'total': 2, 'week': 1604793600, 'days': [2, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1605398400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 10, 'week': 1606003200, 'days': [1, 4, 1, 0, 0, 0, 4]},
       {'total': 0, 'week': 1606608000, 'days': [0, 0, 0, 0, 0, 0, 0]}],
      'issues_closed': 1322,
      'issues_all': 1583,
      'pull_requests_closed': 905,
      'pull_requests_all': 938,
      'comments': 39,
      'languages': {'Java': 3390250,
       'HTML': 20964,
       'CSS': 11102,
       'JavaScript': 1190,
       'Gherkin': 1078},
      'date_of_last_commit': None,
      'feature_files': ['naked-objects/integtests/src/test/java/domainapp/integtests/specs/modules/simple/SimpleObjectSpec_listAllAndCreate.feature']},
     {'name': 'jekyll',
      'full_name': 'jekyll/jekyll',
      'url': 'https://github.com/jekyll/jekyll',
      'fork': False,
      'num_forks': 9115,
      'num_contributors': 402,
      'num_commits': 11200,
      'num_stars': 41693,
      'num_watchers': 1473,
      'commit_activities': [{'total': 5,
        'week': 1575763200,
        'days': [0, 0, 5, 0, 0, 0, 0]},
       {'total': 2, 'week': 1576368000, 'days': [0, 0, 2, 0, 0, 0, 0]},
       {'total': 6, 'week': 1576972800, 'days': [2, 0, 0, 4, 0, 0, 0]},
       {'total': 4, 'week': 1577577600, 'days': [0, 2, 2, 0, 0, 0, 0]},
       {'total': 0, 'week': 1578182400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 11, 'week': 1578787200, 'days': [0, 0, 0, 8, 1, 2, 0]},
       {'total': 3, 'week': 1579392000, 'days': [1, 2, 0, 0, 0, 0, 0]},
       {'total': 7, 'week': 1579996800, 'days': [0, 0, 0, 2, 0, 4, 1]},
       {'total': 13, 'week': 1580601600, 'days': [1, 2, 2, 2, 5, 1, 0]},
       {'total': 5, 'week': 1581206400, 'days': [0, 0, 0, 2, 2, 1, 0]},
       {'total': 15, 'week': 1581811200, 'days': [0, 2, 0, 5, 6, 2, 0]},
       {'total': 16, 'week': 1582416000, 'days': [0, 3, 1, 6, 4, 2, 0]},
       {'total': 10, 'week': 1583020800, 'days': [0, 2, 0, 2, 0, 2, 4]},
       {'total': 5, 'week': 1583625600, 'days': [0, 0, 3, 0, 2, 0, 0]},
       {'total': 10, 'week': 1584230400, 'days': [2, 4, 0, 0, 2, 2, 0]},
       {'total': 6, 'week': 1584835200, 'days': [2, 0, 0, 0, 4, 0, 0]},
       {'total': 20, 'week': 1585440000, 'days': [3, 5, 0, 5, 1, 3, 3]},
       {'total': 8, 'week': 1586044800, 'days': [5, 0, 0, 0, 2, 1, 0]},
       {'total': 26, 'week': 1586649600, 'days': [2, 17, 1, 3, 1, 1, 1]},
       {'total': 7, 'week': 1587254400, 'days': [0, 0, 0, 0, 1, 2, 4]},
       {'total': 17, 'week': 1587859200, 'days': [1, 8, 0, 0, 7, 1, 0]},
       {'total': 12, 'week': 1588464000, 'days': [0, 3, 0, 1, 3, 0, 5]},
       {'total': 19, 'week': 1589068800, 'days': [11, 1, 0, 0, 7, 0, 0]},
       {'total': 23, 'week': 1589673600, 'days': [1, 2, 2, 5, 8, 5, 0]},
       {'total': 8, 'week': 1590278400, 'days': [0, 4, 0, 3, 1, 0, 0]},
       {'total': 13, 'week': 1590883200, 'days': [0, 4, 0, 1, 6, 2, 0]},
       {'total': 13, 'week': 1591488000, 'days': [0, 4, 2, 0, 0, 3, 4]},
       {'total': 3, 'week': 1592092800, 'days': [0, 0, 0, 2, 0, 0, 1]},
       {'total': 12, 'week': 1592697600, 'days': [1, 3, 0, 8, 0, 0, 0]},
       {'total': 6, 'week': 1593302400, 'days': [0, 0, 0, 4, 0, 2, 0]},
       {'total': 14, 'week': 1593907200, 'days': [2, 2, 0, 3, 3, 4, 0]},
       {'total': 6, 'week': 1594512000, 'days': [1, 0, 0, 0, 3, 2, 0]},
       {'total': 2, 'week': 1595116800, 'days': [0, 0, 0, 0, 2, 0, 0]},
       {'total': 4, 'week': 1595721600, 'days': [0, 0, 0, 4, 0, 0, 0]},
       {'total': 4, 'week': 1596326400, 'days': [0, 1, 0, 3, 0, 0, 0]},
       {'total': 2, 'week': 1596931200, 'days': [0, 0, 0, 2, 0, 0, 0]},
       {'total': 7, 'week': 1597536000, 'days': [3, 2, 0, 2, 0, 0, 0]},
       {'total': 7, 'week': 1598140800, 'days': [2, 2, 1, 0, 2, 0, 0]},
       {'total': 12, 'week': 1598745600, 'days': [0, 0, 0, 0, 0, 10, 2]},
       {'total': 14, 'week': 1599350400, 'days': [2, 4, 2, 4, 0, 2, 0]},
       {'total': 13, 'week': 1599955200, 'days': [0, 4, 0, 3, 3, 3, 0]},
       {'total': 16, 'week': 1600560000, 'days': [0, 0, 1, 2, 4, 6, 3]},
       {'total': 14, 'week': 1601164800, 'days': [0, 4, 0, 5, 5, 0, 0]},
       {'total': 9, 'week': 1601769600, 'days': [1, 0, 0, 3, 4, 0, 1]},
       {'total': 7, 'week': 1602374400, 'days': [4, 1, 0, 0, 0, 2, 0]},
       {'total': 1, 'week': 1602979200, 'days': [0, 0, 0, 1, 0, 0, 0]},
       {'total': 7, 'week': 1603584000, 'days': [0, 2, 0, 0, 5, 0, 0]},
       {'total': 16, 'week': 1604188800, 'days': [0, 4, 2, 2, 6, 2, 0]},
       {'total': 25, 'week': 1604793600, 'days': [2, 2, 0, 18, 3, 0, 0]},
       {'total': 4, 'week': 1605398400, 'days': [0, 0, 0, 4, 0, 0, 0]},
       {'total': 6, 'week': 1606003200, 'days': [0, 3, 0, 0, 2, 1, 0]},
       {'total': 2, 'week': 1606608000, 'days': [2, 0, 0, 0, 0, 0, 0]}],
      'issues_closed': 8185,
      'issues_all': 8334,
      'pull_requests_closed': 3944,
      'pull_requests_all': 4014,
      'comments': 556,
      'languages': {'Ruby': 696738,
       'Gherkin': 224137,
       'JavaScript': 38178,
       'HTML': 11772,
       'Shell': 7900,
       'Dockerfile': 2279,
       'CSS': 1019,
       'SCSS': 606,
       'CoffeeScript': 234,
       'PHP': 90},
      'date_of_last_commit': None,
      'feature_files': ['features/post_excerpts.feature',
       'features/create_sites.feature',
       'features/rendering.feature',
       'features/hooks.feature',
       'features/site_configuration.feature',
       'features/permalinks.feature',
       'features/include_tag.feature',
       'features/theme_gem.feature',
       'features/post_url_tag.feature',
       'features/pagination.feature',
       'features/plugins.feature',
       'features/site_data.feature',
       'features/highlighting.feature',
       'features/post_data.feature',
       'features/include_relative_tag.feature',
       'features/collections.feature',
       'features/layout_data.feature',
       'features/collections_dir.feature',
       'features/markdown.feature',
       'features/theme.feature',
       'features/drafts.feature',
       'features/frontmatter_defaults.feature',
       'features/data.feature',
       'features/link_tag.feature',
       'features/embed_filters.feature',
       'features/incremental_rebuild.feature',
       'features/cache.feature',
       'features/theme_configuration.feature']},
     {'name': 'tutorials',
      'full_name': 'eugenp/tutorials',
      'url': 'https://github.com/eugenp/tutorials',
      'fork': False,
      'num_forks': 38199,
      'num_contributors': 323,
      'num_commits': 21061,
      'num_stars': 23873,
      'num_watchers': 1540,
      'commit_activities': [{'total': 81,
        'week': 1575763200,
        'days': [17, 4, 8, 6, 5, 32, 9]},
       {'total': 82, 'week': 1576368000, 'days': [12, 10, 10, 7, 33, 7, 3]},
       {'total': 50, 'week': 1576972800, 'days': [7, 6, 10, 0, 3, 15, 9]},
       {'total': 54, 'week': 1577577600, 'days': [4, 1, 2, 16, 8, 12, 11]},
       {'total': 57, 'week': 1578182400, 'days': [6, 4, 9, 9, 13, 9, 7]},
       {'total': 46, 'week': 1578787200, 'days': [9, 8, 3, 7, 8, 4, 7]},
       {'total': 95, 'week': 1579392000, 'days': [6, 6, 11, 19, 33, 10, 10]},
       {'total': 97, 'week': 1579996800, 'days': [15, 23, 22, 7, 9, 11, 10]},
       {'total': 60, 'week': 1580601600, 'days': [2, 1, 6, 25, 8, 2, 16]},
       {'total': 132, 'week': 1581206400, 'days': [17, 57, 16, 14, 5, 15, 8]},
       {'total': 75, 'week': 1581811200, 'days': [12, 10, 6, 19, 1, 5, 22]},
       {'total': 46, 'week': 1582416000, 'days': [4, 7, 7, 4, 3, 11, 10]},
       {'total': 67, 'week': 1583020800, 'days': [7, 25, 6, 0, 6, 4, 19]},
       {'total': 86, 'week': 1583625600, 'days': [11, 3, 10, 34, 10, 9, 9]},
       {'total': 195, 'week': 1584230400, 'days': [27, 8, 19, 68, 42, 12, 19]},
       {'total': 100, 'week': 1584835200, 'days': [21, 26, 13, 12, 10, 3, 15]},
       {'total': 75, 'week': 1585440000, 'days': [8, 11, 14, 10, 12, 1, 19]},
       {'total': 104, 'week': 1586044800, 'days': [7, 3, 31, 8, 9, 41, 5]},
       {'total': 75, 'week': 1586649600, 'days': [12, 10, 7, 6, 3, 13, 24]},
       {'total': 95, 'week': 1587254400, 'days': [8, 10, 28, 9, 5, 13, 22]},
       {'total': 67, 'week': 1587859200, 'days': [12, 14, 8, 14, 8, 6, 5]},
       {'total': 89, 'week': 1588464000, 'days': [14, 6, 39, 15, 3, 6, 6]},
       {'total': 87, 'week': 1589068800, 'days': [7, 5, 37, 12, 3, 6, 17]},
       {'total': 63, 'week': 1589673600, 'days': [18, 8, 12, 13, 4, 4, 4]},
       {'total': 59, 'week': 1590278400, 'days': [10, 9, 7, 12, 6, 5, 10]},
       {'total': 62, 'week': 1590883200, 'days': [6, 10, 23, 11, 8, 4, 0]},
       {'total': 122, 'week': 1591488000, 'days': [16, 11, 38, 20, 9, 20, 8]},
       {'total': 93, 'week': 1592092800, 'days': [7, 12, 1, 5, 39, 12, 17]},
       {'total': 66, 'week': 1592697600, 'days': [13, 8, 13, 13, 5, 6, 8]},
       {'total': 71, 'week': 1593302400, 'days': [27, 10, 6, 5, 9, 8, 6]},
       {'total': 52, 'week': 1593907200, 'days': [3, 12, 9, 5, 10, 7, 6]},
       {'total': 74, 'week': 1594512000, 'days': [14, 8, 9, 14, 5, 20, 4]},
       {'total': 133, 'week': 1595116800, 'days': [15, 5, 64, 7, 31, 4, 7]},
       {'total': 56, 'week': 1595721600, 'days': [9, 3, 14, 2, 20, 5, 3]},
       {'total': 45, 'week': 1596326400, 'days': [10, 4, 2, 5, 5, 4, 15]},
       {'total': 76, 'week': 1596931200, 'days': [4, 1, 19, 10, 18, 12, 12]},
       {'total': 105, 'week': 1597536000, 'days': [14, 17, 12, 11, 43, 2, 6]},
       {'total': 40, 'week': 1598140800, 'days': [4, 10, 2, 6, 4, 5, 9]},
       {'total': 44, 'week': 1598745600, 'days': [9, 11, 11, 5, 4, 0, 4]},
       {'total': 61, 'week': 1599350400, 'days': [13, 8, 8, 7, 16, 3, 6]},
       {'total': 71, 'week': 1599955200, 'days': [6, 9, 8, 31, 10, 1, 6]},
       {'total': 44, 'week': 1600560000, 'days': [4, 7, 7, 8, 12, 4, 2]},
       {'total': 56, 'week': 1601164800, 'days': [5, 11, 5, 4, 1, 20, 10]},
       {'total': 46, 'week': 1601769600, 'days': [2, 2, 3, 15, 8, 11, 5]},
       {'total': 33, 'week': 1602374400, 'days': [6, 8, 2, 4, 5, 4, 4]},
       {'total': 31, 'week': 1602979200, 'days': [1, 0, 7, 10, 6, 1, 6]},
       {'total': 61, 'week': 1603584000, 'days': [3, 4, 6, 41, 4, 2, 1]},
       {'total': 34, 'week': 1604188800, 'days': [1, 2, 10, 3, 8, 4, 6]},
       {'total': 25, 'week': 1604793600, 'days': [2, 4, 1, 10, 1, 4, 3]},
       {'total': 36, 'week': 1605398400, 'days': [2, 6, 2, 13, 1, 11, 1]},
       {'total': 22, 'week': 1606003200, 'days': [3, 2, 7, 4, 1, 1, 4]},
       {'total': 0, 'week': 1606608000, 'days': [0, 0, 0, 0, 0, 0, 0]}],
      'issues_closed': 10260,
      'issues_all': 10291,
      'pull_requests_closed': 9834,
      'pull_requests_all': 9855,
      'comments': 26,
      'languages': {'Java': 18787946,
       'JavaScript': 2946071,
       'HTML': 804979,
       'CSS': 804190,
       'TypeScript': 771176,
       'Groovy': 154977,
       'AspectJ': 70251,
       'Shell': 45606,
       'FreeMarker': 41353,
       'RAML': 38085,
       'ANTLR': 31940,
       'SCSS': 30909,
       'Kotlin': 16638,
       'Scala': 15319,
       'Python': 14778,
       'Gherkin': 10005,
       'Dockerfile': 8900,
       'HCL': 5984,
       'Batchfile': 4770,
       'C++': 3241,
       'Clojure': 1892,
       'PLpgSQL': 1888,
       'Solidity': 1542,
       'Starlark': 1245,
       'Smarty': 1142,
       'XSLT': 819,
       'Rich Text Format': 810,
       'Thrift': 609,
       'CLIPS': 383,
       'Pug': 297,
       'R': 48,
       'Handlebars': 12},
      'date_of_last_commit': None,
      'feature_files': ['testing-modules/rest-testing/src/test/resources/Feature/cucumber.feature',
       'testing-modules/testing-libraries/src/test/resources/features/calculator.feature',
       'testing-modules/testing-libraries/src/test/resources/features/shopping.feature',
       'testing-modules/testing-libraries/src/test/resources/features/book-store-without-background.feature',
       'testing-modules/testing-libraries/src/test/resources/features/book-store-with-hooks.feature',
       'testing-modules/testing-libraries/src/test/resources/features/calculator-scenario-outline.feature',
       'testing-modules/testing-libraries/src/test/resources/features/book-store.feature',
       'testing-modules/testing-libraries/src/test/resources/features/book-store-with-background.feature',
       'testing-modules/rest-testing/src/test/resources/karate/user.feature',
       'spring-cucumber/src/test/resources/baelung.feature',
       'spring-cucumber/src/test/resources/version.feature']},
     {'name': 'consul',
      'full_name': 'hashicorp/consul',
      'url': 'https://github.com/hashicorp/consul',
      'fork': False,
      'num_forks': 3544,
      'num_contributors': 379,
      'num_commits': 13255,
      'num_stars': 20701,
      'num_watchers': 983,
      'commit_activities': [{'total': 35,
        'week': 1575763200,
        'days': [0, 5, 20, 2, 3, 5, 0]},
       {'total': 37, 'week': 1576368000, 'days': [0, 5, 9, 13, 2, 7, 1]},
       {'total': 0, 'week': 1576972800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1577577600, 'days': [0, 0, 0, 0, 0, 1, 0]},
       {'total': 13, 'week': 1578182400, 'days': [0, 3, 3, 0, 5, 2, 0]},
       {'total': 20, 'week': 1578787200, 'days': [0, 2, 5, 3, 3, 7, 0]},
       {'total': 44, 'week': 1579392000, 'days': [0, 4, 12, 9, 7, 12, 0]},
       {'total': 49, 'week': 1579996800, 'days': [0, 6, 11, 8, 6, 17, 1]},
       {'total': 43, 'week': 1580601600, 'days': [0, 19, 4, 6, 6, 8, 0]},
       {'total': 73, 'week': 1581206400, 'days': [0, 30, 23, 9, 7, 3, 1]},
       {'total': 40, 'week': 1581811200, 'days': [0, 0, 20, 11, 4, 5, 0]},
       {'total': 2, 'week': 1582416000, 'days': [1, 0, 0, 0, 1, 0, 0]},
       {'total': 21, 'week': 1583020800, 'days': [0, 3, 4, 4, 8, 2, 0]},
       {'total': 30, 'week': 1583625600, 'days': [0, 11, 11, 4, 4, 0, 0]},
       {'total': 23, 'week': 1584230400, 'days': [0, 8, 8, 4, 2, 0, 1]},
       {'total': 34, 'week': 1584835200, 'days': [0, 7, 5, 5, 10, 7, 0]},
       {'total': 55, 'week': 1585440000, 'days': [0, 8, 14, 11, 16, 6, 0]},
       {'total': 41, 'week': 1586044800, 'days': [1, 3, 11, 7, 14, 5, 0]},
       {'total': 70, 'week': 1586649600, 'days': [0, 24, 19, 7, 11, 9, 0]},
       {'total': 40, 'week': 1587254400, 'days': [1, 5, 5, 6, 11, 12, 0]},
       {'total': 63, 'week': 1587859200, 'days': [0, 12, 11, 7, 8, 24, 1]},
       {'total': 75, 'week': 1588464000, 'days': [0, 12, 11, 30, 7, 15, 0]},
       {'total': 106, 'week': 1589068800, 'days': [0, 21, 16, 44, 24, 1, 0]},
       {'total': 41, 'week': 1589673600, 'days': [0, 4, 11, 12, 14, 0, 0]},
       {'total': 24, 'week': 1590278400, 'days': [0, 0, 4, 9, 5, 6, 0]},
       {'total': 48, 'week': 1590883200, 'days': [0, 2, 8, 15, 5, 17, 1]},
       {'total': 49, 'week': 1591488000, 'days': [0, 9, 14, 7, 6, 13, 0]},
       {'total': 62, 'week': 1592092800, 'days': [0, 10, 15, 12, 17, 8, 0]},
       {'total': 43, 'week': 1592697600, 'days': [1, 7, 12, 16, 3, 4, 0]},
       {'total': 34, 'week': 1593302400, 'days': [0, 8, 5, 8, 11, 2, 0]},
       {'total': 55, 'week': 1593907200, 'days': [0, 14, 10, 9, 13, 9, 0]},
       {'total': 32, 'week': 1594512000, 'days': [0, 6, 6, 8, 5, 7, 0]},
       {'total': 47, 'week': 1595116800, 'days': [0, 11, 11, 9, 11, 3, 2]},
       {'total': 48, 'week': 1595721600, 'days': [0, 6, 7, 18, 13, 4, 0]},
       {'total': 27, 'week': 1596326400, 'days': [0, 0, 0, 6, 6, 15, 0]},
       {'total': 50, 'week': 1596931200, 'days': [0, 10, 13, 9, 12, 6, 0]},
       {'total': 26, 'week': 1597536000, 'days': [0, 7, 9, 4, 1, 4, 1]},
       {'total': 29, 'week': 1598140800, 'days': [0, 3, 3, 5, 10, 8, 0]},
       {'total': 40, 'week': 1598745600, 'days': [0, 1, 3, 17, 11, 8, 0]},
       {'total': 44, 'week': 1599350400, 'days': [1, 0, 8, 14, 6, 14, 1]},
       {'total': 48, 'week': 1599955200, 'days': [0, 11, 19, 4, 4, 10, 0]},
       {'total': 34, 'week': 1600560000, 'days': [0, 4, 7, 6, 9, 8, 0]},
       {'total': 60, 'week': 1601164800, 'days': [2, 10, 6, 15, 18, 9, 0]},
       {'total': 130, 'week': 1601769600, 'days': [0, 19, 22, 20, 26, 43, 0]},
       {'total': 58, 'week': 1602374400, 'days': [0, 16, 15, 14, 13, 0, 0]},
       {'total': 64, 'week': 1602979200, 'days': [0, 16, 11, 7, 8, 22, 0]},
       {'total': 49, 'week': 1603584000, 'days': [0, 13, 17, 7, 6, 6, 0]},
       {'total': 81, 'week': 1604188800, 'days': [0, 12, 3, 11, 35, 20, 0]},
       {'total': 70, 'week': 1604793600, 'days': [0, 9, 18, 16, 10, 17, 0]},
       {'total': 50, 'week': 1605398400, 'days': [0, 16, 4, 12, 16, 2, 0]},
       {'total': 18, 'week': 1606003200, 'days': [0, 4, 8, 3, 0, 3, 0]},
       {'total': 0, 'week': 1606608000, 'days': [0, 0, 0, 0, 0, 0, 0]}],
      'issues_closed': 8415,
      'issues_all': 9211,
      'pull_requests_closed': 5041,
      'pull_requests_all': 5146,
      'comments': 195,
      'languages': {'Go': 10117251,
       'JavaScript': 1082533,
       'SCSS': 310456,
       'Handlebars': 279468,
       'Shell': 261282,
       'Gherkin': 159219,
       'HCL': 44157,
       'CSS': 28226,
       'Makefile': 18378,
       'Dockerfile': 1394,
       'HTML': 1205},
      'date_of_last_commit': None,
      'feature_files': ['ui/packages/consul-ui/tests/acceptance/dc/acls/roles/as-many/add-new.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/auth-methods/navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/permissions/create.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/create.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/permissions/warn.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/kvs/index.feature',
       'ui/packages/consul-ui/tests/acceptance/navigation-links.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/kvs/edit.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/index.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show/topology.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/auth-methods/index.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/auth-methods/sorting.feature',
       'ui/packages/consul-ui/tests/acceptance/page-navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/sessions/list.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/instances/navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/as-many/add-existing.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/roles/as-many/add-existing.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/index.feature',
       'ui/packages/consul-ui/tests/acceptance/components/acl-filter.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/empty-ids.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/index.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/instances/show.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/instances/exposed-paths.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/instances/upstreams.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show/intentions.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/delete.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/sorting.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/list-blocking.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show-topology.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/kvs/update.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/as-many/add-new.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/forwarding.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/roles/create.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/update.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/services/list.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/sorting.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/roles/navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/clone.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/view-management.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/kvs/delete.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/as-many/nspaces.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/update.feature',
       'ui/packages/consul-ui/tests/acceptance/components/copy-button.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/dc-switch.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/kvs/create.feature',
       'ui/packages/consul-ui/tests/acceptance/components/text-input.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/roles/as-many/list.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/index.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/sorting.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nspaces/index.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/index.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/error.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/list-blocking.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show-routing.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/roles/index.feature',
       'ui/packages/consul-ui/tests/acceptance/components/kv-filter.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show/dc-switch.feature',
       'ui/packages/consul-ui/tests/acceptance/token-header.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/roles/as-many/remove.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/roles/sorting.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/error.feature',
       'ui/packages/consul-ui/tests/acceptance/login-errors.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/sessions/invalidate.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/no-leader.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/sorting.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/delete.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/list.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/kvs/sessions/invalidate.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nspaces/update.feature',
       'ui/packages/consul-ui/tests/acceptance/settings/show.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/as-many/remove.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/instances/error.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nspaces/manage.feature',
       'ui/packages/consul-ui/tests/acceptance/settings/update.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show/services.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/create.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/form-select.feature',
       'ui/packages/consul-ui/tests/acceptance/submit-blank.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/use.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/sorting.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nspaces/sorting.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/index.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nspaces/delete.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/filtered-select.feature',
       'ui/packages/consul-ui/tests/acceptance/login.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/list.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/intentions/update.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show-with-slashes.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/own-no-delete.feature',
       'ui/packages/consul-ui/tests/acceptance/index-forwarding.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/anonymous-no-delete.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/roles/update.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/instances/gateway.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/create.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/kvs/trailing-slash.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/list-order.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/use.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nspaces/create.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/as-many/list.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/update.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/policies/as-many/reset.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/navigation.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/acls/tokens/legacy/update.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/index.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/kvs/list-order.feature',
       'ui/packages/consul-ui/tests/acceptance/deleting.feature',
       'ui/packages/consul-ui/tests/acceptance/startup.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/nodes/show.feature',
       'ui/packages/consul-ui/tests/acceptance/dc/services/show/upstreams.feature']},
     {'name': 'hub',
      'full_name': 'github/hub',
      'url': 'https://github.com/github/hub',
      'fork': False,
      'num_forks': 2141,
      'num_contributors': 218,
      'num_commits': 3306,
      'num_stars': 20563,
      'num_watchers': 477,
      'commit_activities': [{'total': 2,
        'week': 1575763200,
        'days': [0, 0, 1, 0, 0, 0, 1]},
       {'total': 4, 'week': 1576368000, 'days': [3, 1, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1576972800, 'days': [1, 0, 0, 0, 0, 0, 0]},
       {'total': 2, 'week': 1577577600, 'days': [0, 0, 1, 0, 0, 1, 0]},
       {'total': 1, 'week': 1578182400, 'days': [0, 1, 0, 0, 0, 0, 0]},
       {'total': 3, 'week': 1578787200, 'days': [0, 0, 0, 0, 1, 0, 2]},
       {'total': 28, 'week': 1579392000, 'days': [5, 1, 19, 0, 0, 0, 3]},
       {'total': 4, 'week': 1579996800, 'days': [0, 1, 0, 3, 0, 0, 0]},
       {'total': 1, 'week': 1580601600, 'days': [0, 0, 0, 0, 0, 1, 0]},
       {'total': 0, 'week': 1581206400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 2, 'week': 1581811200, 'days': [0, 0, 0, 1, 1, 0, 0]},
       {'total': 0, 'week': 1582416000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 8, 'week': 1583020800, 'days': [1, 0, 0, 0, 6, 0, 1]},
       {'total': 3, 'week': 1583625600, 'days': [1, 1, 0, 0, 1, 0, 0]},
       {'total': 6, 'week': 1584230400, 'days': [0, 1, 0, 0, 0, 5, 0]},
       {'total': 0, 'week': 1584835200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 3, 'week': 1585440000, 'days': [0, 0, 0, 0, 0, 3, 0]},
       {'total': 0, 'week': 1586044800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 8, 'week': 1586649600, 'days': [2, 5, 0, 0, 1, 0, 0]},
       {'total': 0, 'week': 1587254400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1587859200, 'days': [1, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1588464000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1589068800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1589673600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 2, 'week': 1590278400, 'days': [0, 2, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1590883200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1591488000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1592092800, 'days': [0, 0, 0, 0, 0, 0, 1]},
       {'total': 0, 'week': 1592697600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1593302400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1593907200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1594512000, 'days': [0, 0, 0, 0, 0, 1, 0]},
       {'total': 0, 'week': 1595116800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1595721600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 5, 'week': 1596326400, 'days': [5, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1596931200, 'days': [0, 0, 1, 0, 0, 0, 0]},
       {'total': 0, 'week': 1597536000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1598140800, 'days': [0, 1, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1598745600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1599350400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1599955200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1600560000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1601164800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1601769600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1602374400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1602979200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1603584000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1604188800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1604793600, 'days': [0, 0, 1, 0, 0, 0, 0]},
       {'total': 3, 'week': 1605398400, 'days': [0, 1, 2, 0, 0, 0, 0]},
       {'total': 0, 'week': 1606003200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1606608000, 'days': [0, 0, 0, 0, 0, 0, 0]}],
      'issues_closed': 2325,
      'issues_all': 2567,
      'pull_requests_closed': 739,
      'pull_requests_all': 781,
      'comments': 178,
      'languages': {'Go': 358904,
       'Gherkin': 289265,
       'Shell': 39220,
       'Ruby': 30354,
       'Makefile': 2397,
       'Batchfile': 2112,
       'Vim script': 2060,
       'HTML': 1666,
       'Dockerfile': 551},
      'date_of_last_commit': None,
      'feature_files': ['features/fetch.feature',
       'features/fork.feature',
       'features/pr-checkout.feature',
       'features/browse.feature',
       'features/pr-show.feature',
       'features/merge.feature',
       'features/pr-merge.feature',
       'features/clone.feature',
       'features/am.feature',
       'features/init.feature',
       'features/apply.feature',
       'features/pull_request.feature',
       'features/sync.feature',
       'features/help.feature',
       'features/release.feature',
       'features/zsh_completion.feature',
       'features/fish_completion.feature',
       'features/gist.feature',
       'features/issue.feature',
       'features/compare.feature',
       'features/git_compatibility.feature',
       'features/checkout.feature',
       'features/bash_completion.feature',
       'features/remote_add.feature',
       'features/ci_status.feature',
       'features/cherry_pick.feature',
       'features/pr-list.feature',
       'features/api.feature',
       'features/issue-transfer.feature',
       'features/push.feature',
       'features/alias.feature',
       'features/authentication.feature',
       'features/delete.feature',
       'features/submodule_add.feature',
       'features/create.feature']},
     {'name': 'influxdb',
      'full_name': 'influxdata/influxdb',
      'url': 'https://github.com/influxdata/influxdb',
      'fork': False,
      'num_forks': 2825,
      'num_contributors': 323,
      'num_commits': 34431,
      'num_stars': 20017,
      'num_watchers': 755,
      'commit_activities': [{'total': 49,
        'week': 1575763200,
        'days': [0, 5, 19, 10, 8, 7, 0]},
       {'total': 62, 'week': 1576368000, 'days': [0, 8, 21, 15, 9, 8, 1]},
       {'total': 23, 'week': 1576972800, 'days': [0, 4, 0, 2, 4, 13, 0]},
       {'total': 36, 'week': 1577577600, 'days': [6, 8, 8, 1, 8, 5, 0]},
       {'total': 49, 'week': 1578182400, 'days': [0, 10, 7, 15, 8, 7, 2]},
       {'total': 64, 'week': 1578787200, 'days': [0, 8, 8, 19, 13, 16, 0]},
       {'total': 50, 'week': 1579392000, 'days': [2, 0, 9, 12, 17, 10, 0]},
       {'total': 7, 'week': 1579996800, 'days': [0, 0, 0, 1, 2, 4, 0]},
       {'total': 47, 'week': 1580601600, 'days': [0, 13, 6, 5, 16, 7, 0]},
       {'total': 44, 'week': 1581206400, 'days': [0, 5, 13, 5, 10, 11, 0]},
       {'total': 33, 'week': 1581811200, 'days': [0, 0, 1, 9, 10, 13, 0]},
       {'total': 35, 'week': 1582416000, 'days': [0, 5, 10, 6, 8, 6, 0]},
       {'total': 46, 'week': 1583020800, 'days': [0, 5, 5, 9, 12, 15, 0]},
       {'total': 49, 'week': 1583625600, 'days': [0, 8, 9, 9, 13, 10, 0]},
       {'total': 66, 'week': 1584230400, 'days': [0, 22, 11, 11, 13, 9, 0]},
       {'total': 78, 'week': 1584835200, 'days': [0, 14, 12, 15, 10, 27, 0]},
       {'total': 67, 'week': 1585440000, 'days': [2, 2, 17, 17, 9, 19, 1]},
       {'total': 62, 'week': 1586044800, 'days': [0, 13, 12, 21, 5, 10, 1]},
       {'total': 52, 'week': 1586649600, 'days': [0, 9, 13, 16, 11, 3, 0]},
       {'total': 64, 'week': 1587254400, 'days': [0, 10, 9, 15, 19, 11, 0]},
       {'total': 49, 'week': 1587859200, 'days': [0, 3, 10, 21, 7, 8, 0]},
       {'total': 57, 'week': 1588464000, 'days': [2, 6, 7, 21, 13, 8, 0]},
       {'total': 50, 'week': 1589068800, 'days': [0, 2, 18, 11, 11, 8, 0]},
       {'total': 36, 'week': 1589673600, 'days': [1, 4, 6, 11, 8, 6, 0]},
       {'total': 48, 'week': 1590278400, 'days': [1, 3, 15, 9, 11, 9, 0]},
       {'total': 58, 'week': 1590883200, 'days': [0, 10, 17, 6, 18, 7, 0]},
       {'total': 76, 'week': 1591488000, 'days': [0, 13, 12, 20, 19, 12, 0]},
       {'total': 105, 'week': 1592092800, 'days': [1, 20, 28, 27, 29, 0, 0]},
       {'total': 70, 'week': 1592697600, 'days': [0, 16, 9, 14, 19, 12, 0]},
       {'total': 61, 'week': 1593302400, 'days': [0, 24, 11, 17, 8, 1, 0]},
       {'total': 44, 'week': 1593907200, 'days': [0, 7, 10, 4, 17, 6, 0]},
       {'total': 34, 'week': 1594512000, 'days': [0, 6, 13, 5, 4, 6, 0]},
       {'total': 48, 'week': 1595116800, 'days': [0, 6, 6, 6, 15, 13, 2]},
       {'total': 61, 'week': 1595721600, 'days': [1, 10, 15, 14, 15, 6, 0]},
       {'total': 53, 'week': 1596326400, 'days': [0, 17, 7, 20, 6, 3, 0]},
       {'total': 41, 'week': 1596931200, 'days': [0, 5, 9, 11, 6, 10, 0]},
       {'total': 32, 'week': 1597536000, 'days': [2, 7, 8, 3, 7, 5, 0]},
       {'total': 56, 'week': 1598140800, 'days': [0, 7, 15, 17, 16, 1, 0]},
       {'total': 30, 'week': 1598745600, 'days': [0, 9, 2, 6, 4, 9, 0]},
       {'total': 19, 'week': 1599350400, 'days': [0, 1, 1, 4, 7, 1, 5]},
       {'total': 9, 'week': 1599955200, 'days': [0, 1, 1, 5, 2, 0, 0]},
       {'total': 27, 'week': 1600560000, 'days': [3, 7, 7, 4, 5, 1, 0]},
       {'total': 23, 'week': 1601164800, 'days': [0, 4, 12, 0, 3, 4, 0]},
       {'total': 10, 'week': 1601769600, 'days': [0, 1, 2, 2, 3, 2, 0]},
       {'total': 15, 'week': 1602374400, 'days': [1, 4, 2, 7, 1, 0, 0]},
       {'total': 15, 'week': 1602979200, 'days': [0, 5, 4, 2, 2, 2, 0]},
       {'total': 42, 'week': 1603584000, 'days': [0, 8, 9, 8, 8, 9, 0]},
       {'total': 31, 'week': 1604188800, 'days': [0, 10, 6, 4, 8, 3, 0]},
       {'total': 45, 'week': 1604793600, 'days': [0, 15, 12, 6, 4, 8, 0]},
       {'total': 32, 'week': 1605398400, 'days': [0, 9, 14, 6, 2, 1, 0]},
       {'total': 17, 'week': 1606003200, 'days': [0, 9, 7, 1, 0, 0, 0]},
       {'total': 0, 'week': 1606608000, 'days': [0, 0, 0, 0, 0, 0, 0]}],
      'issues_closed': 18772,
      'issues_all': 20021,
      'pull_requests_closed': 9096,
      'pull_requests_all': 9218,
      'comments': 351,
      'languages': {'Go': 14021584,
       'TypeScript': 3730682,
       'JavaScript': 679270,
       'SCSS': 216574,
       'Gherkin': 189456,
       'Python': 42767,
       'Shell': 41532,
       'Makefile': 16739,
       'Dockerfile': 1798,
       'HTML': 804},
      'date_of_last_commit': None,
      'feature_files': []},
     {'name': 'kibana',
      'full_name': 'elastic/kibana',
      'url': 'https://github.com/elastic/kibana',
      'fork': False,
      'num_forks': 6167,
      'num_contributors': 353,
      'num_commits': 38439,
      'num_stars': 15187,
      'num_watchers': 847,
      'commit_activities': [{'total': 168,
        'week': 1575763200,
        'days': [0, 31, 22, 44, 37, 31, 3]},
       {'total': 175, 'week': 1576368000, 'days': [9, 28, 41, 35, 37, 24, 1]},
       {'total': 16, 'week': 1576972800, 'days': [0, 8, 4, 1, 2, 1, 0]},
       {'total': 38, 'week': 1577577600, 'days': [2, 4, 4, 0, 15, 10, 3]},
       {'total': 163, 'week': 1578182400, 'days': [2, 25, 34, 32, 29, 35, 6]},
       {'total': 221, 'week': 1578787200, 'days': [2, 44, 71, 33, 41, 24, 6]},
       {'total': 162, 'week': 1579392000, 'days': [0, 14, 41, 37, 28, 40, 2]},
       {'total': 167, 'week': 1579996800, 'days': [1, 37, 51, 27, 29, 19, 3]},
       {'total': 161, 'week': 1580601600, 'days': [0, 34, 31, 29, 41, 26, 0]},
       {'total': 172, 'week': 1581206400, 'days': [1, 33, 36, 29, 31, 39, 3]},
       {'total': 148, 'week': 1581811200, 'days': [5, 14, 41, 35, 30, 22, 1]},
       {'total': 162, 'week': 1582416000, 'days': [0, 20, 40, 36, 34, 29, 3]},
       {'total': 182, 'week': 1583020800, 'days': [2, 26, 48, 45, 32, 28, 1]},
       {'total': 148, 'week': 1583625600, 'days': [0, 20, 32, 28, 38, 29, 1]},
       {'total': 209, 'week': 1584230400, 'days': [2, 32, 40, 44, 41, 46, 4]},
       {'total': 267, 'week': 1584835200, 'days': [2, 53, 66, 57, 54, 31, 4]},
       {'total': 203, 'week': 1585440000, 'days': [1, 40, 50, 37, 30, 41, 4]},
       {'total': 190, 'week': 1586044800, 'days': [0, 55, 34, 40, 36, 23, 2]},
       {'total': 180, 'week': 1586649600, 'days': [1, 31, 30, 33, 44, 38, 3]},
       {'total': 161, 'week': 1587254400, 'days': [1, 26, 30, 37, 32, 31, 4]},
       {'total': 157, 'week': 1587859200, 'days': [0, 40, 31, 37, 43, 4, 2]},
       {'total': 279, 'week': 1588464000, 'days': [1, 65, 88, 47, 53, 24, 1]},
       {'total': 209, 'week': 1589068800, 'days': [3, 42, 47, 43, 32, 40, 2]},
       {'total': 110, 'week': 1589673600, 'days': [0, 19, 35, 21, 29, 6, 0]},
       {'total': 124, 'week': 1590278400, 'days': [1, 10, 29, 32, 28, 20, 4]},
       {'total': 146, 'week': 1590883200, 'days': [0, 20, 25, 36, 36, 27, 2]},
       {'total': 172, 'week': 1591488000, 'days': [0, 31, 40, 38, 30, 31, 2]},
       {'total': 150, 'week': 1592092800, 'days': [0, 37, 36, 23, 44, 7, 3]},
       {'total': 149, 'week': 1592697600, 'days': [0, 0, 27, 29, 52, 38, 3]},
       {'total': 208, 'week': 1593302400, 'days': [0, 46, 42, 47, 51, 21, 1]},
       {'total': 219, 'week': 1593907200, 'days': [1, 49, 46, 44, 68, 11, 0]},
       {'total': 345, 'week': 1594512000, 'days': [0, 88, 109, 60, 51, 36, 1]},
       {'total': 212, 'week': 1595116800, 'days': [0, 46, 44, 56, 56, 9, 1]},
       {'total': 227, 'week': 1595721600, 'days': [2, 39, 70, 33, 41, 39, 3]},
       {'total': 132, 'week': 1596326400, 'days': [0, 38, 33, 32, 26, 3, 0]},
       {'total': 138, 'week': 1596931200, 'days': [0, 24, 31, 26, 24, 30, 3]},
       {'total': 162, 'week': 1597536000, 'days': [0, 27, 32, 39, 31, 32, 1]},
       {'total': 157, 'week': 1598140800, 'days': [1, 53, 27, 35, 37, 4, 0]},
       {'total': 182, 'week': 1598745600, 'days': [0, 34, 36, 35, 38, 39, 0]},
       {'total': 127, 'week': 1599350400, 'days': [1, 18, 32, 43, 27, 6, 0]},
       {'total': 232, 'week': 1599955200, 'days': [3, 47, 47, 48, 47, 39, 1]},
       {'total': 188, 'week': 1600560000, 'days': [2, 34, 37, 52, 55, 8, 0]},
       {'total': 312, 'week': 1601164800, 'days': [0, 47, 65, 59, 81, 55, 5]},
       {'total': 249, 'week': 1601769600, 'days': [4, 78, 97, 39, 28, 3, 0]},
       {'total': 211, 'week': 1602374400, 'days': [1, 32, 48, 44, 41, 42, 3]},
       {'total': 154, 'week': 1602979200, 'days': [0, 40, 44, 40, 28, 2, 0]},
       {'total': 168, 'week': 1603584000, 'days': [1, 35, 33, 29, 36, 33, 1]},
       {'total': 200, 'week': 1604188800, 'days': [1, 49, 45, 31, 38, 33, 3]},
       {'total': 155, 'week': 1604793600, 'days': [3, 37, 44, 31, 35, 2, 3]},
       {'total': 203, 'week': 1605398400, 'days': [1, 31, 33, 46, 43, 49, 0]},
       {'total': 143, 'week': 1606003200, 'days': [8, 44, 48, 29, 11, 2, 1]},
       {'total': 7, 'week': 1606608000, 'days': [1, 6, 0, 0, 0, 0, 0]}],
      'issues_closed': 76758,
      'issues_all': 84422,
      'pull_requests_closed': 54235,
      'pull_requests_all': 54703,
      'comments': 366,
      'languages': {'TypeScript': 76447863,
       'JavaScript': 12680353,
       'SCSS': 422251,
       'CSS': 208557,
       'Shell': 199002,
       'HTML': 161081,
       'Groovy': 105501,
       'Kotlin': 42999,
       'Perl': 12787,
       'Handlebars': 10144,
       'Dockerfile': 7829,
       'Python': 6902,
       'Batchfile': 3743,
       'Gherkin': 1736},
      'date_of_last_commit': None,
      'feature_files': ['x-pack/plugins/apm/e2e/cypress/integration/apm.feature',
       'x-pack/plugins/apm/e2e/cypress/integration/csm_dashboard.feature']},
     {'name': 'diaspora',
      'full_name': 'diaspora/diaspora',
      'url': 'https://github.com/diaspora/diaspora',
      'fork': False,
      'num_forks': 2915,
      'num_contributors': 281,
      'num_commits': 20279,
      'num_stars': 12570,
      'num_watchers': 502,
      'commit_activities': [{'total': 0,
        'week': 1575763200,
        'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1576368000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1576972800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1577577600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1578182400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 3, 'week': 1578787200, 'days': [0, 0, 0, 0, 3, 0, 0]},
       {'total': 6, 'week': 1579392000, 'days': [0, 0, 2, 1, 0, 3, 0]},
       {'total': 6, 'week': 1579996800, 'days': [1, 0, 0, 1, 3, 0, 1]},
       {'total': 14, 'week': 1580601600, 'days': [7, 1, 3, 0, 0, 1, 2]},
       {'total': 21, 'week': 1581206400, 'days': [3, 1, 15, 2, 0, 0, 0]},
       {'total': 1, 'week': 1581811200, 'days': [0, 0, 0, 0, 1, 0, 0]},
       {'total': 0, 'week': 1582416000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1583020800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1583625600, 'days': [0, 0, 0, 0, 1, 0, 0]},
       {'total': 69, 'week': 1584230400, 'days': [0, 1, 0, 0, 0, 68, 0]},
       {'total': 1, 'week': 1584835200, 'days': [0, 0, 0, 0, 0, 0, 1]},
       {'total': 0, 'week': 1585440000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1586044800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1586649600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1587254400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1587859200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1588464000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1589068800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1589673600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1590278400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1590883200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 4, 'week': 1591488000, 'days': [0, 0, 0, 0, 2, 0, 2]},
       {'total': 2, 'week': 1592092800, 'days': [2, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1592697600, 'days': [0, 0, 0, 1, 0, 0, 0]},
       {'total': 0, 'week': 1593302400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 5, 'week': 1593907200, 'days': [0, 0, 0, 1, 0, 4, 0]},
       {'total': 0, 'week': 1594512000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1595116800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1595721600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1596326400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1596931200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1597536000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1598140800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 3, 'week': 1598745600, 'days': [0, 0, 0, 1, 2, 0, 0]},
       {'total': 0, 'week': 1599350400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 1, 'week': 1599955200, 'days': [0, 0, 0, 1, 0, 0, 0]},
       {'total': 0, 'week': 1600560000, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1601164800, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1601769600, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1602374400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 4, 'week': 1602979200, 'days': [0, 0, 2, 0, 0, 1, 1]},
       {'total': 7, 'week': 1603584000, 'days': [0, 0, 0, 0, 0, 5, 2]},
       {'total': 11, 'week': 1604188800, 'days': [1, 5, 2, 2, 0, 1, 0]},
       {'total': 1, 'week': 1604793600, 'days': [0, 0, 0, 0, 0, 0, 1]},
       {'total': 0, 'week': 1605398400, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1606003200, 'days': [0, 0, 0, 0, 0, 0, 0]},
       {'total': 0, 'week': 1606608000, 'days': [0, 0, 0, 0, 0, 0, 0]}],
      'issues_closed': 7702,
      'issues_all': 8151,
      'pull_requests_closed': 3491,
      'pull_requests_all': 3521,
      'comments': 1727,
      'languages': {'Ruby': 2291824,
       'JavaScript': 754337,
       'Haml': 172647,
       'Gherkin': 142724,
       'SCSS': 112108,
       'Handlebars': 50607,
       'Shell': 22095,
       'Dockerfile': 1454},
      'date_of_last_commit': None,
      'feature_files': ['features/desktop/follows_tags.feature',
       'features/desktop/posts_from_main_page.feature',
       'features/desktop/profile_photos.feature',
       'features/mobile/not_safe_for_work.feature',
       'features/desktop/mentions_from_profile_page.feature',
       'features/desktop/mentions.feature',
       'features/desktop/aspect_navigation.feature',
       'features/mobile/home.feature',
       'features/desktop/change_settings.feature',
       'features/desktop/logs_in_and_out.feature',
       'features/desktop/help.feature',
       'features/desktop/post_preview.feature',
       'features/desktop/getting_started.feature',
       'features/desktop/photo_gallery.feature',
       'features/desktop/search.feature',
       'features/desktop/keyboard_navigation.feature',
       'features/mobile/more-button.feature',
       'features/desktop/user_applications.feature',
       'features/desktop/show_more.feature',
       'features/desktop/single_post_view_moderation.feature',
       'features/desktop/blocks_user.feature',
       'features/desktop/posts_from_profile_page.feature',
       'features/desktop/conversations.feature',
       'features/mobile/drawer.feature',
       'features/mobile/conversations.feature',
       'features/mobile/invitations.feature',
       'features/desktop/stops_following_users.feature',
       'features/desktop/edits_profile.feature',
       'features/mobile/change_password.feature',
       'features/desktop/donations.feature',
       'features/desktop/public_stream.feature',
       'features/mobile/people_aspects.feature',
       'features/desktop/activity_stream.feature',
       'features/mobile/activity_stream.feature',
       'features/mobile/reactions.feature',
       'features/desktop/posts_from_tag_page.feature',
       'features/mobile/stream.feature',
       'features/desktop/notifications.feature',
       'features/desktop/invitations.feature',
       'features/mobile/tags.feature',
       'features/desktop/hovercards.feature',
       'features/desktop/connects_users.feature',
       'features/desktop/media-embed.feature',
       'features/desktop/diaspora_links_resolve.feature',
       'features/desktop/reshare.feature',
       'features/desktop/change_password.feature',
       'features/desktop/post_with_a_poll.feature',
       'features/desktop/closes_account.feature',
       'features/desktop/logged_out_browsing.feature',
       'features/desktop/not_safe_for_work.feature',
       'features/mobile/registrations.feature',
       'features/mobile/multiphoto.feature',
       'features/mobile/edits_profile.feature',
       'features/desktop/oidc_implicit_flow.feature',
       'features/mobile/posts_from_main_page.feature',
       'features/desktop/manages_aspects.feature',
       'features/desktop/comments.feature',
       'features/mobile/user_applications.feature',
       'features/mobile/closes_account.feature',
       'features/desktop/download_photos.feature',
       'features/mobile/logged_out_browsing.feature',
       'features/desktop/oidc_auth_code_flow.feature',
       'features/desktop/two_factor_authentication.feature',
       'features/desktop/registrations.feature',
       'features/desktop/auto_follow_back.feature',
       'features/mobile/getting_started.feature',
       'features/desktop/screenshots.feature',
       'features/mobile/reshare.feature',
       'features/desktop/oembed.feature',
       'features/desktop/likes.feature']},
     {'name': 'server',
      'full_name': 'nextcloud/server',
      'url': 'https://github.com/nextcloud/server',
      'fork': False,
      'num_forks': 2291,
      'num_contributors': 378,
      'num_commits': 56380,
      'num_stars': 12369,
      'num_watchers': 509,
      'commit_activities': [{'total': 57,
        'week': 1575763200,
        'days': [10, 12, 10, 7, 12, 4, 2]},
       {'total': 54, 'week': 1576368000, 'days': [4, 15, 12, 8, 8, 4, 3]},
       {'total': 36, 'week': 1576972800, 'days': [11, 14, 1, 2, 2, 5, 1]},
       {'total': 32, 'week': 1577577600, 'days': [8, 8, 2, 1, 4, 8, 1]},
       {'total': 72, 'week': 1578182400, 'days': [13, 6, 8, 17, 16, 11, 1]},
       {'total': 54, 'week': 1578787200, 'days': [2, 12, 11, 6, 16, 5, 2]},
       {'total': 52, 'week': 1579392000, 'days': [6, 9, 13, 7, 5, 11, 1]},
       {'total': 50, 'week': 1579996800, 'days': [6, 6, 10, 6, 7, 11, 4]},
       {'total': 54, 'week': 1580601600, 'days': [3, 14, 3, 9, 11, 13, 1]},
       {'total': 49, 'week': 1581206400, 'days': [9, 9, 3, 11, 6, 10, 1]},
       {'total': 51, 'week': 1581811200, 'days': [7, 8, 16, 7, 5, 3, 5]},
       {'total': 55, 'week': 1582416000, 'days': [14, 4, 6, 7, 9, 8, 7]},
       {'total': 47, 'week': 1583020800, 'days': [5, 8, 10, 5, 5, 10, 4]},
       {'total': 65, 'week': 1583625600, 'days': [5, 10, 14, 9, 14, 10, 3]},
       {'total': 62, 'week': 1584230400, 'days': [9, 9, 4, 4, 19, 13, 4]},
       {'total': 43, 'week': 1584835200, 'days': [5, 4, 8, 7, 6, 7, 6]},
       {'total': 49, 'week': 1585440000, 'days': [4, 3, 8, 8, 10, 10, 6]},
       {'total': 83, 'week': 1586044800, 'days': [12, 5, 4, 10, 26, 24, 2]},
       {'total': 59, 'week': 1586649600, 'days': [6, 5, 13, 14, 3, 16, 2]},
       {'total': 45, 'week': 1587254400, 'days': [8, 2, 13, 6, 8, 6, 2]},
       {'total': 47, 'week': 1587859200, 'days': [4, 4, 2, 14, 9, 8, 6]},
       {'total': 42, 'week': 1588464000, 'days': [2, 6, 4, 6, 15, 7, 2]},
       {'total': 56, 'week': 1589068800, 'days': [8, 11, 6, 13, 8, 7, 3]},
       {'total': 25, 'week': 1589673600, 'days': [3, 6, 7, 5, 1, 2, 1]},
       {'total': 44, 'week': 1590278400, 'days': [4, 8, 5, 8, 13, 5, 1]},
       {'total': 42, 'week': 1590883200, 'days': [3, 4, 7, 8, 9, 8, 3]},
       {'total': 39, 'week': 1591488000, 'days': [3, 17, 4, 4, 9, 2, 0]},
       {'total': 53, 'week': 1592092800, 'days': [3, 15, 8, 12, 5, 8, 2]},
       {'total': 63, 'week': 1592697600, 'days': [6, 10, 13, 12, 8, 9, 5]},
       {'total': 40, 'week': 1593302400, 'days': [1, 6, 12, 6, 4, 6, 5]},
       {'total': 74, 'week': 1593907200, 'days': [2, 9, 11, 20, 11, 16, 5]},
       {'total': 68, 'week': 1594512000, 'days': [2, 12, 17, 9, 21, 5, 2]},
       {'total': 53, 'week': 1595116800, 'days': [1, 5, 10, 11, 14, 6, 6]},
       {'total': 47, 'week': 1595721600, 'days': [0, 8, 3, 5, 17, 13, 1]},
       {'total': 69, 'week': 1596326400, 'days': [0, 9, 13, 24, 8, 10, 5]},
       {'total': 75, 'week': 1596931200, 'days': [3, 11, 14, 15, 10, 18, 4]},
       {'total': 104, 'week': 1597536000, 'days': [1, 10, 19, 22, 36, 11, 5]},
       {'total': 56, 'week': 1598140800, 'days': [4, 15, 10, 8, 10, 6, 3]},
       {'total': 55, 'week': 1598745600, 'days': [2, 13, 7, 9, 8, 12, 4]},
       {'total': 69, 'week': 1599350400, 'days': [2, 19, 10, 8, 19, 9, 2]},
       {'total': 75, 'week': 1599955200, 'days': [0, 6, 22, 11, 17, 14, 5]},
       {'total': 37, 'week': 1600560000, 'days': [2, 3, 5, 13, 6, 5, 3]},
       {'total': 49, 'week': 1601164800, 'days': [2, 2, 5, 7, 6, 15, 12]},
       {'total': 53, 'week': 1601769600, 'days': [5, 7, 4, 12, 10, 11, 4]},
       {'total': 65, 'week': 1602374400, 'days': [6, 13, 13, 5, 10, 9, 9]},
       {'total': 60, 'week': 1602979200, 'days': [1, 7, 8, 20, 11, 4, 9]},
       {'total': 87, 'week': 1603584000, 'days': [3, 13, 15, 18, 12, 13, 13]},
       {'total': 91, 'week': 1604188800, 'days': [3, 7, 11, 13, 14, 26, 17]},
       {'total': 87, 'week': 1604793600, 'days': [3, 12, 22, 15, 13, 10, 12]},
       {'total': 43, 'week': 1605398400, 'days': [1, 7, 7, 3, 4, 12, 9]},
       {'total': 33, 'week': 1606003200, 'days': [3, 7, 9, 4, 4, 4, 2]},
       {'total': 9, 'week': 1606608000, 'days': [7, 2, 0, 0, 0, 0, 0]}],
      'issues_closed': 21473,
      'issues_all': 24118,
      'pull_requests_closed': 12535,
      'pull_requests_all': 12705,
      'comments': 140,
      'languages': {'PHP': 19363568,
       'JavaScript': 10473779,
       'Vue': 450209,
       'Gherkin': 442992,
       'SCSS': 225639,
       'Shell': 87159,
       'HTML': 65215,
       'CSS': 35946,
       'Handlebars': 11322,
       'Makefile': 2793},
      'date_of_last_commit': None,
      'feature_files': ['build/integration/features/provisioning-v1.feature',
       'build/integration/sharing_features/sharing-v1.feature',
       'build/integration/federation_features/cleanup-remote-storage.feature',
       'build/integration/collaboration_features/autocomplete.feature',
       'tests/acceptance/features/header.feature',
       'tests/acceptance/features/users.feature',
       'build/integration/features/avatar.feature',
       'tests/acceptance/features/app-files-sharing-link.feature',
       'tests/acceptance/features/app-files-sharing.feature',
       'build/integration/features/transfer-ownership.feature',
       'tests/acceptance/features/app-files.feature',
       'tests/acceptance/features/app-comments.feature',
       'tests/acceptance/features/access-levels.feature',
       'tests/acceptance/features/app-files-tags.feature',
       'build/integration/sharing_features/sharing-v1-video-verification.feature',
       'build/integration/setup_features/setup.feature',
       'build/integration/features/checksums.feature',
       'build/integration/features/ratelimiting.feature',
       'build/integration/ldap_features/ldap-ocs.feature',
       'build/integration/features/comments-search.feature',
       'build/integration/sharing_features/sharing-v1-part2.feature',
       'build/integration/remoteapi_features/remote.feature',
       'build/integration/features/provisioning-v2.feature',
       'build/integration/sharees_features/sharees_provisioningapiv2.feature',
       'build/integration/features/auth.feature',
       'build/integration/ldap_features/openldap-uid-username.feature',
       'build/integration/features/webdav-related.feature',
       'build/integration/features/ocs-v1.feature',
       'build/integration/features/external-storage.feature',
       'build/integration/ldap_features/ldap-openldap.feature',
       'build/integration/features/maintenance-mode.feature',
       'build/integration/features/dav-v2.feature',
       'build/integration/sharing_features/sharing-v1-part3.feature',
       'build/integration/capabilities_features/capabilities.feature',
       'build/integration/ldap_features/openldap-numerical-id.feature',
       'build/integration/filesdrop_features/filesdrop.feature',
       'build/integration/features/tags.feature',
       'build/integration/features/trashbin.feature',
       'build/integration/features/caldav.feature',
       'build/integration/sharees_features/sharees.feature',
       'build/integration/federation_features/federated.feature',
       'build/integration/features/download.feature',
       'build/integration/features/carddav.feature',
       'tests/acceptance/features/apps.feature',
       'build/integration/features/comments.feature',
       'build/integration/features/favorites.feature',
       'tests/acceptance/features/login.feature',
       'tests/acceptance/features/app-theming.feature']}]




```python
save_to_file(repo_stats['repos'], name='manual_feature_file_list')
```


```python
len(repo_stats['repos'])
```




    318


