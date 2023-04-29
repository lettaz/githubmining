```python
import pickle
import pandas as pd
import os
import requests
import secrets
import time

import github
from github import Github
```


```python
g = Github('f9b312d6ab0fa60f7a52bfb89a24991df82acc5c')
```


```python
from fetch_data.repo_stats import get_feature_files, get_step_files
```

## Fetch the name and full_name of the repos


```python
file_list = pd.read_pickle(r'data/manual_feature_file_list.pickle')

repos_feat = []

for val in file_list:
    repo_dict = {}
    repo_dict['name'] = val['name']
    repo_dict['full_name'] = val['full_name']    
    
    repos_feat.append(repo_dict)
    
len(repos_feat)
```




    318




```python
repos_feat[0]
```




    {'name': 'linux', 'full_name': 'torvalds/linux'}



## Search the feature files in each repo


```python
for repo in repos_feat:
    repo_full_name = g.get_repo(repo['full_name'])
    print(repo_full_name)
    repo["feature_files"] = get_feature_files(repo_full_name, g)
```

## Find intial commit and last commit of repos


```python
for repo in repos:
    last_modified = g.get_repo(repo['full_name']).last_modified
    created = g.get_repo(repo['full_name']).created_at
    print("repo:", repo['full_name'], " ", "created:", created, " ", "last_modified:", last_modified)
```

## Finding files with Gherkin - Task discussed on 20.04.2021


```python
i = 0
repo_full_name = []
for file in file_list:
    if 'Gherkin' in file['languages'].keys():
        repo_full_name.append(file['full_name'])
        i += 1

print("Count of repos with 'Gherkin'", i)
```

    Count of repos with 'Gherkin' 318
    

## Repo full_names


```python
repos = repo_full_name
len(repos)
```




    318



## Read an existing csv file into a dataframe


```python
repo_df = pd.read_csv('Repo_Feat_files.csv')
repo_df
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
      <th>Repository</th>
      <th># Feat files</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>torvalds/linux</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>iluwatar/java-design-patterns</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>jekyll/jekyll</td>
      <td>28</td>
    </tr>
    <tr>
      <th>3</th>
      <td>eugenp/tutorials</td>
      <td>13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>hashicorp/consul</td>
      <td>114</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>307</th>
      <td>javrasya/django-river</td>
      <td>4</td>
    </tr>
    <tr>
      <th>308</th>
      <td>code-ready/crc</td>
      <td>9</td>
    </tr>
    <tr>
      <th>309</th>
      <td>peej/tonic</td>
      <td>25</td>
    </tr>
    <tr>
      <th>310</th>
      <td>code-dot-org/code-dot-org</td>
      <td>200</td>
    </tr>
    <tr>
      <th>311</th>
      <td>facebook/remodel</td>
      <td>28</td>
    </tr>
  </tbody>
</table>
<p>312 rows × 2 columns</p>
</div>



## Convert the data frame to a dictionary


```python
repo_dict_feature = {}
```


```python
repo_dict_feature = repo_df.set_index('Repository')['# Feat files'].to_dict()
repo_dict_feature
```




    {'torvalds/linux': 1,
     'iluwatar/java-design-patterns': 1,
     'jekyll/jekyll': 28,
     'eugenp/tutorials': 13,
     'hashicorp/consul': 114,
     'github/hub': 35,
     'influxdata/influxdb': 0,
     'elastic/kibana': 2,
     'diaspora/diaspora': 70,
     'nextcloud/server': 49,
     'capistrano/capistrano': 8,
     'Instagram/IGListKit': 1,
     'karma-runner/karma': 23,
     'gradle/gradle': 1,
     'AlDanial/cloc': 1,
     'aio-libs/aiohttp': 28,
     'docker/labs': 5,
     'teamcapybara/capybara': 2,
     'dbcli/mycli': 8,
     'dbcli/pgcli': 8,
     'thoughtbot/paperclip': 3,
     'thoughtbot/bourbon': 3,
     'activeadmin/activeadmin': 58,
     'carrierwaveuploader/carrierwave': 12,
     'neo4j/neo4j': 26,
     'TooTallNate/Java-WebSocket': 1,
     'raspberrypi/linux': 1,
     'owncloud/core': 467,
     'thoughtbot/factory_bot': 1,
     'matrix-org/synapse': 7,
     'Compass/compass': 2,
     'pytest-dev/pytest': 1,
     'middleman/middleman': 101,
     'woocommerce/woocommerce': 7,
     'aws/aws-sdk-js': 65,
     'webdriverio/webdriverio': 7,
     'Sylius/Sylius': 633,
     'qutebrowser/qutebrowser': 27,
     'github/super-linter': 3,
     'guard/guard': 12,
     'bigbluebutton/bigbluebutton': 2,
     'guardian/frontend': 1,
     'jnunemaker/httparty': 9,
     'PrestaShop/PrestaShop': 143,
     'cucumber/cucumber-ruby': 155,
     'aws/aws-sdk-php': 79,
     'vcr/vcr': 49,
     'Vedenin/useful-java-links': 1,
     'mRemoteNG/mRemoteNG': 2,
     'jrnl-org/jrnl': 16,
     'cloudera/hue': 1,
     'buildbot/buildbot': 4,
     'rspec/rspec-rails': 42,
     'lolcommits/lolcommits': 2,
     'microsoft/WSL2-Linux-Kernel': 1,
     'greenplum-db/gpdb': 21,
     'wp-cli/wp-cli': 24,
     'intuit/karate': 395,
     'Codeception/Codeception': 11,
     'simplecov-ruby/simplecov': 45,
     'twisted/twisted': 1,
     'Project-OSRM/osrm-backend': 206,
     'cucumber/cucumber-js': 66,
     'zephyrproject-rtos/zephyr': 0,
     'sdkman/sdkman-cli': 25,
     'zalando/patroni': 7,
     'apiaryio/dredd': 1,
     'Hygieia/Hygieia': 14,
     'xcpretty/xcpretty': 10,
     'Behat/Behat': 43,
     'troessner/reek': 29,
     'aws/aws-sdk-java': 57,
     'mapsme/omim': 107,
     'asciidoctor/asciidoctor': 4,
     'javaparser/javaparser': 0,
     'moodle/moodle': 0,
     'aws/aws-sdk-ruby': 136,
     'quantumblacklabs/kedro': 12,
     'codeceptjs/CodeceptJS': 16,
     'facebookresearch/hydra': 22,
     'cucumber/cucumber': 517,
     'carrot/share-button': 13,
     'emacs-lsp/lsp-mode': 1,
     'ckan/ckan': 3,
     'phpDocumentor/phpDocumentor': 30,
     'whitesmith/rubycritic': 0,
     'DatabaseCleaner/database_cleaner': 7,
     'jejacks0n/mercury': 29,
     'thoughtbot/factory_bot_rails': 4,
     'google/hover': 1,
     'tensorflow/datasets': 0,
     'eclipse/openj9': 12,
     'thiagopradi/octopus': 2,
     'graknlabs/grakn': 1,
     'python-openxml/python-docx': 59,
     'lucadegasperi/oauth2-server-laravel': 1,
     'alohaeditor/Aloha-Editor': 49,
     'SAP/openui5': 17,
     'cucumber/cucumber-jvm': 97,
     'OrchardCMS/Orchard': 28,
     'behave/behave': 195,
     'cappuccino/cappuccino': 2,
     'Dolibarr/dolibarr': 0,
     'geoserver/geoserver': 4,
     'motiv-labs/janus': 6,
     'minishift/minishift': 23,
     'sharetribe/sharetribe': 113,
     'tinytacoteam/zazu': 7,
     'novoda/android-demos': 1,
     'flowplayer/flowplayer': 4,
     'git-town/git-town': 232,
     'magnars/multiple-cursors.el': 14,
     'pnp/PnP': 53,
     'calabash/calabash-ios': 12,
     'crossbario/crossbar': 1,
     'apache/servicecomb-pack': 30,
     'EasyEngine/easyengine': 1,
     'Covid-19Radar/Covid19Radar': 5,
     'dchelimsky/rspec': 40,
     'test-kitchen/test-kitchen': 11,
     'phpspec/phpspec': 58,
     'hyperledger-archives/composer': 23,
     'retorquere/zotero-better-bibtex': 3,
     'api-platform/core': 104,
     'radiant/radiant': 9,
     'iridakos/duckrails': 3,
     'SpecFlowOSS/SpecFlow': 86,
     'calabash/calabash-android': 9,
     'wal-g/wal-g': 2,
     'appium-boneyard/sample-code': 2,
     'barnybug/cli53': 10,
     'JetBrains/intellij-plugins': 104,
     'osm-search/Nominatim': 42,
     'robinhood-unofficial/pyrh': 4,
     'aio-libs/aioredis': 0,
     'radar/guides': 2,
     'microservices-patterns/ftgo-application': 2,
     'technicalpickles/jeweler': 16,
     'Yelp/paasta': 22,
     'petems/tugboat': 3,
     'overtone/emacs-live': 55,
     'egonSchiele/contracts.ruby': 29,
     'FriendsOfPHP/pickle': 8,
     'zipmark/rspec_api_documentation': 20,
     'CodelyTV/php-ddd-example': 3,
     'Luracast/Restler': 15,
     'Brightify/Cuckoo': 1,
     'OpenCover/opencover': 2,
     'apache/tinkerpop': 59,
     'microsoftarchive/cqrs-journey': 30,
     'gabrielfalcao/lettuce': 82,
     'vim-vdebug/vdebug': 5,
     'RedisGraph/RedisGraph': 206,
     'cucumber/godog': 29,
     'hyperledger-archives/fabric': 7,
     'browsermedia/browsercms': 52,
     'iotaledger/iri': 5,
     'scanny/python-pptx': 54,
     'JoshCheek/seeing_is_believing': 6,
     'shoes/shoes-deprecated': 4,
     'shopware/shopware': 26,
     'davetron5000/gli': 0,
     'humbug/humbug': 13,
     'thought-machine/please': 3,
     'gitana/alpaca': 18,
     'tuist/tuist': 24,
     'SmartBear/soapui': 31,
     'email-spec/email-spec': 8,
     'millejoh/emacs-ipython-notebook': 11,
     'Sceptre/sceptre': 21,
     'cask/cask': 12,
     'naparuba/shinken': 1,
     'DyCI/dyci-main': 3,
     'chef/omnibus': 7,
     'jtpereyda/boofuzz': 13,
     'rspec/rspec-expectations': 37,
     'berkshelf/berkshelf': 24,
     'piotrmurach/github': 58,
     'rspec/rspec-core': 73,
     'w3c/epubcheck': 36,
     'magnars/expand-region.el': 14,
     'tahoe-lafs/tahoe-lafs': 4,
     'paulelliott/fabrication': 9,
     'dbcli/mssql-cli': 8,
     'rspec/rspec-mocks': 37,
     'apache/usergrid': 1,
     'aws/aws-sdk-java-v2': 53,
     'bugsnag/bugsnag-android': 45,
     'cucumber/cucumber-rails': 13,
     'particle-iot/device-os': 5,
     'jeremyw/stamp': 1,
     'GeoNode/geonode': 1,
     'blox/blox': 10,
     'kmmbvnr/django-jenkins': 1,
     'kuzzleio/kuzzle': 15,
     'ddd-by-examples/factory': 2,
     'progrium/buildstep': 2,
     'collectiveidea/json_spec': 7,
     'openshift/origin-server': 57,
     'boto/botocore': 52,
     'nextcloud/spreed': 61,
     'cucumber/aruba': 102,
     'Behat/Gherkin': 34,
     'coderwall/coderwall-legacy': 1,
     'gauntlt/gauntlt': 10,
     'screwdriver-cd/screwdriver': 15,
     'inukshuk/jekyll-scholar': 19,
     'PolySync/oscc': 13,
     'arches/table_print': 7,
     'buffer/thug': 0,
     'ttscoff/doing': 1,
     'teslamotors/linux': 1,
     'backlogs/redmine_backlogs': 31,
     'K-Phoen/rulerz': 4,
     'errata-ai/vale': 9,
     'KKBOX/CompassApp': 2,
     'Xilinx/linux-xlnx': 1,
     'stelligent/cfn_nag': 1,
     'emacs-lsp/dap-mode': 13,
     'FluentLenium/FluentLenium': 11,
     'akeneo/pim-community-dev': 352,
     'OTRS/otrs': 0,
     'codeliner/php-ddd-cargo-sample': 1,
     'EnMarche/en-marche.fr': 66,
     'AppiumTestDistribution/AppiumTestDistribution': 3,
     'kantord/LibreLingo': 14,
     'lonelyplanet/rizzo': 2,
     'bugsnag/bugsnag-laravel': 12,
     'OfficeDev/TrainingContent': 22,
     'RefugeRestrooms/refugerestrooms': 1,
     'owenthereal/gh': 16,
     'Cocolabs-SAS/cocorico': 30,
     'rodjek/librarian-puppet': 11,
     'iain/http_accept_language': 1,
     'breser/git2consul': 1,
     'multipath-tcp/mptcp': 1,
     'copycopter/copycopter-server': 9,
     'Enalean/tuleap': 2,
     'johanvts/emacs-fireplace': 1,
     'clojure-emacs/clj-refactor.el': 27,
     'Tencent/TencentOS-kernel': 1,
     'mzheravin/exchange-core': 2,
     'matthewrenze/clean-architecture-demo': 6,
     'hyperledger/fabric-sdk-node': 8,
     'meteor-velocity/velocity': 0,
     'vagrant-landrush/landrush': 4,
     'Azure/azure-xplat-cli': 2,
     'phpdocker-io/phpdocker.io': 1,
     'otwcode/otwarchive': 163,
     'flapjack/flapjack': 14,
     'pytroll/satpy': 3,
     'j-bennet/wharfee': 5,
     'input-output-hk/daedalus': 47,
     'alphagov/whitehall': 0,
     'unused/airstream': 1,
     'Behat/MinkExtension': 1,
     'ari/jobsworth': 3,
     'KKBOX/FireApp': 2,
     'nextcloud/deck': 4,
     'oroinc/platform': 164,
     'adhearsion/adhearsion': 7,
     'SpaceMadness/lunar-unity-console': 1,
     'jqr/heroku_san': 5,
     'google/ktsan': 1,
     'SteeltoeOSS/Samples': 11,
     'QafooLabs/php-refactoring-browser': 5,
     'cyberark/summon': 5,
     'LiskHQ/lisk-desktop': 12,
     'kennethkalmer/daemon-kit': 8,
     'emacsorphanage/god-mode': 10,
     'oroinc/crm': 86,
     'mojotech/pioneer': 8,
     'howl-editor/howl': 1,
     'qaprosoft/carina': 1,
     'tmm1/test-queue': 3,
     'tomafro/recap': 4,
     'apache/camel-k': 14,
     'beagleboard/linux': 1,
     'AutomateThePlanet/AutomateThePlanet-Learning-Series': 5,
     'ianwhite/pickle': 8,
     'BBVA/kapow': 20,
     'chamilo/chamilo-lms': 32,
     'rake-compiler/rake-compiler': 8,
     'Stratio/sparta': 52,
     'iriusrisk/bdd-security': 11,
     'serenity-bdd/serenity-core': 70,
     'bwalex/tc-play': 18,
     'OmniSharp/omnisharp-emacs': 2,
     'jbangdev/jbang': 16,
     'limpkin/mooltipass': 6,
     'ushahidi/platform': 66,
     'gterrono/houston': 5,
     'bugsnag/bugsnag-js': 66,
     'karlfreeman/middleman-deploy': 1,
     'cheezy/page-object': 42,
     'eerkunt/terraform-compliance': 107,
     'xaviershay/enki': 4,
     'luminus-framework/luminus-template': 1,
     'TheBrainFamily/cypress-cucumber-preprocessor': 20,
     'SoftInstigate/restheart': 26,
     'codegram/spinach': 18,
     'riscv/riscv-linux': 0,
     'apache/isis': 2,
     'OpenLiberty/open-liberty': 699,
     'dmaicher/doctrine-test-bundle': 1,
     'cschiewek/devise_ldap_authenticatable': 1,
     'SonarOpenCommunity/sonar-cxx': 10,
     'javrasya/django-river': 4,
     'code-ready/crc': 9,
     'peej/tonic': 25,
     'code-dot-org/code-dot-org': 200,
     'facebook/remodel': 28}



## Fetch the no.of feat files in the repos


```python
for repo in repos[315:]:
    repo_full = g.get_repo(repo)
    feat = get_feature_files(repo_full, g)
    repo_dict_feature[repo] = len(feat)
```


```python
repo_df = pd.DataFrame(repo_dict_feature.items(), columns=['Repository', '# Feat files'])
```


```python
repo_df
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
      <th>Repository</th>
      <th># Feat files</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>torvalds/linux</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>iluwatar/java-design-patterns</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>jekyll/jekyll</td>
      <td>28</td>
    </tr>
    <tr>
      <th>3</th>
      <td>eugenp/tutorials</td>
      <td>13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>hashicorp/consul</td>
      <td>114</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>307</th>
      <td>javrasya/django-river</td>
      <td>4</td>
    </tr>
    <tr>
      <th>308</th>
      <td>code-ready/crc</td>
      <td>9</td>
    </tr>
    <tr>
      <th>309</th>
      <td>peej/tonic</td>
      <td>25</td>
    </tr>
    <tr>
      <th>310</th>
      <td>code-dot-org/code-dot-org</td>
      <td>200</td>
    </tr>
    <tr>
      <th>311</th>
      <td>facebook/remodel</td>
      <td>28</td>
    </tr>
  </tbody>
</table>
<p>312 rows × 2 columns</p>
</div>



## Fetch repositories for a repo


```python
repo_full = g.get_repo('jekyll/jekyll')
feat = get_feature_files(repo_full, g)
feat
```




    ['features/include_tag.feature',
     'features/theme_gem.feature',
     'features/post_url_tag.feature',
     'features/site_configuration.feature',
     'features/plugins.feature',
     'features/pagination.feature',
     'features/site_data.feature',
     'features/rendering.feature',
     'features/create_sites.feature',
     'features/highlighting.feature',
     'features/post_data.feature',
     'features/include_relative_tag.feature',
     'features/collections.feature',
     'features/layout_data.feature',
     'features/permalinks.feature',
     'features/markdown.feature',
     'features/collections_dir.feature',
     'features/theme.feature',
     'features/drafts.feature',
     'features/frontmatter_defaults.feature',
     'features/data.feature',
     'features/link_tag.feature',
     'features/embed_filters.feature',
     'features/incremental_rebuild.feature',
     'features/post_excerpts.feature',
     'features/hooks.feature',
     'features/cache.feature',
     'features/theme_configuration.feature']



## Write the dataframe to a CSV file


```python
repo_df.to_csv('Repo_Feat_files.csv', index=False)
```

## No.of Repos with 0 feature files


```python
print(repo_df[repo_df['# Feat files'] == 0].count())
repo_df[repo_df['# Feat files'] == 0]
```

    Repository      14
    # Feat files    14
    dtype: int64
    




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
      <th>Repository</th>
      <th># Feat files</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>influxdata/influxdb</td>
      <td>0</td>
    </tr>
    <tr>
      <th>63</th>
      <td>zephyrproject-rtos/zephyr</td>
      <td>0</td>
    </tr>
    <tr>
      <th>74</th>
      <td>javaparser/javaparser</td>
      <td>0</td>
    </tr>
    <tr>
      <th>75</th>
      <td>moodle/moodle</td>
      <td>0</td>
    </tr>
    <tr>
      <th>85</th>
      <td>whitesmith/rubycritic</td>
      <td>0</td>
    </tr>
    <tr>
      <th>90</th>
      <td>tensorflow/datasets</td>
      <td>0</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Dolibarr/dolibarr</td>
      <td>0</td>
    </tr>
    <tr>
      <th>134</th>
      <td>aio-libs/aioredis</td>
      <td>0</td>
    </tr>
    <tr>
      <th>161</th>
      <td>davetron5000/gli</td>
      <td>0</td>
    </tr>
    <tr>
      <th>209</th>
      <td>buffer/thug</td>
      <td>0</td>
    </tr>
    <tr>
      <th>221</th>
      <td>OTRS/otrs</td>
      <td>0</td>
    </tr>
    <tr>
      <th>244</th>
      <td>meteor-velocity/velocity</td>
      <td>0</td>
    </tr>
    <tr>
      <th>253</th>
      <td>alphagov/whitehall</td>
      <td>0</td>
    </tr>
    <tr>
      <th>301</th>
      <td>riscv/riscv-linux</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## No.of Repos with 1 feature file


```python
print(repo_df[repo_df['# Feat files'] == 1].count())
repo_df[repo_df['# Feat files'] == 1]
```

    Repository      52
    # Feat files    52
    dtype: int64
    




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
      <th>Repository</th>
      <th># Feat files</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>torvalds/linux</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>iluwatar/java-design-patterns</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Instagram/IGListKit</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>gradle/gradle</td>
      <td>1</td>
    </tr>
    <tr>
      <th>14</th>
      <td>AlDanial/cloc</td>
      <td>1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>TooTallNate/Java-WebSocket</td>
      <td>1</td>
    </tr>
    <tr>
      <th>26</th>
      <td>raspberrypi/linux</td>
      <td>1</td>
    </tr>
    <tr>
      <th>28</th>
      <td>thoughtbot/factory_bot</td>
      <td>1</td>
    </tr>
    <tr>
      <th>31</th>
      <td>pytest-dev/pytest</td>
      <td>1</td>
    </tr>
    <tr>
      <th>41</th>
      <td>guardian/frontend</td>
      <td>1</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Vedenin/useful-java-links</td>
      <td>1</td>
    </tr>
    <tr>
      <th>50</th>
      <td>cloudera/hue</td>
      <td>1</td>
    </tr>
    <tr>
      <th>54</th>
      <td>microsoft/WSL2-Linux-Kernel</td>
      <td>1</td>
    </tr>
    <tr>
      <th>60</th>
      <td>twisted/twisted</td>
      <td>1</td>
    </tr>
    <tr>
      <th>66</th>
      <td>apiaryio/dredd</td>
      <td>1</td>
    </tr>
    <tr>
      <th>82</th>
      <td>emacs-lsp/lsp-mode</td>
      <td>1</td>
    </tr>
    <tr>
      <th>89</th>
      <td>google/hover</td>
      <td>1</td>
    </tr>
    <tr>
      <th>93</th>
      <td>graknlabs/grakn</td>
      <td>1</td>
    </tr>
    <tr>
      <th>95</th>
      <td>lucadegasperi/oauth2-server-laravel</td>
      <td>1</td>
    </tr>
    <tr>
      <th>108</th>
      <td>novoda/android-demos</td>
      <td>1</td>
    </tr>
    <tr>
      <th>114</th>
      <td>crossbario/crossbar</td>
      <td>1</td>
    </tr>
    <tr>
      <th>116</th>
      <td>EasyEngine/easyengine</td>
      <td>1</td>
    </tr>
    <tr>
      <th>146</th>
      <td>Brightify/Cuckoo</td>
      <td>1</td>
    </tr>
    <tr>
      <th>171</th>
      <td>naparuba/shinken</td>
      <td>1</td>
    </tr>
    <tr>
      <th>185</th>
      <td>apache/usergrid</td>
      <td>1</td>
    </tr>
    <tr>
      <th>190</th>
      <td>jeremyw/stamp</td>
      <td>1</td>
    </tr>
    <tr>
      <th>191</th>
      <td>GeoNode/geonode</td>
      <td>1</td>
    </tr>
    <tr>
      <th>193</th>
      <td>kmmbvnr/django-jenkins</td>
      <td>1</td>
    </tr>
    <tr>
      <th>203</th>
      <td>coderwall/coderwall-legacy</td>
      <td>1</td>
    </tr>
    <tr>
      <th>210</th>
      <td>ttscoff/doing</td>
      <td>1</td>
    </tr>
    <tr>
      <th>211</th>
      <td>teslamotors/linux</td>
      <td>1</td>
    </tr>
    <tr>
      <th>216</th>
      <td>Xilinx/linux-xlnx</td>
      <td>1</td>
    </tr>
    <tr>
      <th>217</th>
      <td>stelligent/cfn_nag</td>
      <td>1</td>
    </tr>
    <tr>
      <th>222</th>
      <td>codeliner/php-ddd-cargo-sample</td>
      <td>1</td>
    </tr>
    <tr>
      <th>229</th>
      <td>RefugeRestrooms/refugerestrooms</td>
      <td>1</td>
    </tr>
    <tr>
      <th>233</th>
      <td>iain/http_accept_language</td>
      <td>1</td>
    </tr>
    <tr>
      <th>234</th>
      <td>breser/git2consul</td>
      <td>1</td>
    </tr>
    <tr>
      <th>235</th>
      <td>multipath-tcp/mptcp</td>
      <td>1</td>
    </tr>
    <tr>
      <th>238</th>
      <td>johanvts/emacs-fireplace</td>
      <td>1</td>
    </tr>
    <tr>
      <th>240</th>
      <td>Tencent/TencentOS-kernel</td>
      <td>1</td>
    </tr>
    <tr>
      <th>247</th>
      <td>phpdocker-io/phpdocker.io</td>
      <td>1</td>
    </tr>
    <tr>
      <th>254</th>
      <td>unused/airstream</td>
      <td>1</td>
    </tr>
    <tr>
      <th>255</th>
      <td>Behat/MinkExtension</td>
      <td>1</td>
    </tr>
    <tr>
      <th>261</th>
      <td>SpaceMadness/lunar-unity-console</td>
      <td>1</td>
    </tr>
    <tr>
      <th>263</th>
      <td>google/ktsan</td>
      <td>1</td>
    </tr>
    <tr>
      <th>272</th>
      <td>howl-editor/howl</td>
      <td>1</td>
    </tr>
    <tr>
      <th>273</th>
      <td>qaprosoft/carina</td>
      <td>1</td>
    </tr>
    <tr>
      <th>277</th>
      <td>beagleboard/linux</td>
      <td>1</td>
    </tr>
    <tr>
      <th>293</th>
      <td>karlfreeman/middleman-deploy</td>
      <td>1</td>
    </tr>
    <tr>
      <th>297</th>
      <td>luminus-framework/luminus-template</td>
      <td>1</td>
    </tr>
    <tr>
      <th>304</th>
      <td>dmaicher/doctrine-test-bundle</td>
      <td>1</td>
    </tr>
    <tr>
      <th>305</th>
      <td>cschiewek/devise_ldap_authenticatable</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
print("Statistics of Feat files for repos:")
repo_df['# Feat files'].describe()
```

    Statistics of Feat files for repos:
    




    count    312.000000
    mean      32.871795
    std       78.713835
    min        0.000000
    25%        2.000000
    50%        9.000000
    75%       29.000000
    max      699.000000
    Name: # Feat files, dtype: float64



## No.of Repos with > 10 feature files


```python
repo_df[repo_df['# Feat files'] > 10]
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
      <th>Repository</th>
      <th># Feat files</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>jekyll/jekyll</td>
      <td>28</td>
    </tr>
    <tr>
      <th>3</th>
      <td>eugenp/tutorials</td>
      <td>13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>hashicorp/consul</td>
      <td>114</td>
    </tr>
    <tr>
      <th>5</th>
      <td>github/hub</td>
      <td>35</td>
    </tr>
    <tr>
      <th>8</th>
      <td>diaspora/diaspora</td>
      <td>70</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>300</th>
      <td>codegram/spinach</td>
      <td>18</td>
    </tr>
    <tr>
      <th>303</th>
      <td>OpenLiberty/open-liberty</td>
      <td>699</td>
    </tr>
    <tr>
      <th>309</th>
      <td>peej/tonic</td>
      <td>25</td>
    </tr>
    <tr>
      <th>310</th>
      <td>code-dot-org/code-dot-org</td>
      <td>200</td>
    </tr>
    <tr>
      <th>311</th>
      <td>facebook/remodel</td>
      <td>28</td>
    </tr>
  </tbody>
</table>
<p>145 rows × 2 columns</p>
</div>



## Find out step files

### Figure out the repos based on most used programming language


```python
file_list = pd.read_pickle(r'data/manual_feature_file_list.pickle')

repos_with_lang = []

for val in file_list:
    repo_dict = {}
    repo_dict['name'] = val['name']
    repo_dict['full_name'] = val['full_name']
    repo_dict['languages'] = [i[0] for i in list(val['languages'].items())[:1]][0]
    
    
    repos_with_lang.append(repo_dict)
    
len(repos_with_lang)
```




    318




```python
repos_with_lang[0]
```




    {'name': 'linux', 'full_name': 'torvalds/linux', 'languages': 'C'}




```python
repo_lang_dict = {}

for repo in repos_with_lang:
    repo_lang_dict[repo['full_name']] = repo['languages']
```


```python
repo_lang_df = pd.DataFrame(repo_lang_dict.items(), columns=['Repository', 'languages'])
repo_lang_df = pd.merge(repo_df, repo_lang_df, on='Repository', how='left')
repo_lang_df.head()
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
      <th>Repository</th>
      <th># Feat files</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>torvalds/linux</td>
      <td>1</td>
      <td>C</td>
    </tr>
    <tr>
      <th>1</th>
      <td>iluwatar/java-design-patterns</td>
      <td>1</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>2</th>
      <td>jekyll/jekyll</td>
      <td>28</td>
      <td>Ruby</td>
    </tr>
    <tr>
      <th>3</th>
      <td>eugenp/tutorials</td>
      <td>13</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>4</th>
      <td>hashicorp/consul</td>
      <td>114</td>
      <td>Go</td>
    </tr>
  </tbody>
</table>
</div>



### Repos with primary programming language


```python
repowise_lang_cnt = pd.DataFrame(repo_lang_df.groupby('languages')['Repository'].nunique())
repowise_lang_cnt.sort_values(by=['Repository'], ascending=False)
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
      <th>Repository</th>
    </tr>
    <tr>
      <th>languages</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Ruby</th>
      <td>82</td>
    </tr>
    <tr>
      <th>PHP</th>
      <td>38</td>
    </tr>
    <tr>
      <th>Java</th>
      <td>37</td>
    </tr>
    <tr>
      <th>Python</th>
      <td>36</td>
    </tr>
    <tr>
      <th>JavaScript</th>
      <td>33</td>
    </tr>
    <tr>
      <th>C</th>
      <td>17</td>
    </tr>
    <tr>
      <th>Go</th>
      <td>16</td>
    </tr>
    <tr>
      <th>Emacs Lisp</th>
      <td>11</td>
    </tr>
    <tr>
      <th>C#</th>
      <td>10</td>
    </tr>
    <tr>
      <th>TypeScript</th>
      <td>5</td>
    </tr>
    <tr>
      <th>HTML</th>
      <td>4</td>
    </tr>
    <tr>
      <th>C++</th>
      <td>4</td>
    </tr>
    <tr>
      <th>Gherkin</th>
      <td>3</td>
    </tr>
    <tr>
      <th>CoffeeScript</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Groovy</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Objective-C</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Perl</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Swift</th>
      <td>2</td>
    </tr>
    <tr>
      <th>MoonScript</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Clojure</th>
      <td>1</td>
    </tr>
    <tr>
      <th>CSS</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Scala</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Shell</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Objective-J</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### Feat files for repos with Java


```python
# print(f"No.of Java Repos with 0 feature files are : {len(repo_lang_df[(repo_lang_df['languages'] == 'Java') & (repo_lang_df['# Feat files'] == 0)])}")
# print(f"No.of Java Repos with 1 feature file are : {len(repo_lang_df[(repo_lang_df['languages'] == 'Java') & (repo_lang_df['# Feat files'] == 1)])}")
# print(f"No.of Java Repos with >10 feature files are : {len(repo_lang_df[(repo_lang_df['languages'] == 'Java') & (repo_lang_df['# Feat files'] > 10)])}")
len(repo_lang_df[(repo_lang_df['languages'] == 'JavaScript') & (repo_lang_df['# Feat files'] == 0)])
```




    1




```python
with open('./data/repo_stats.pickle', 'rb') as file:
    repo_stats = pickle.load(file)
df = pd.DataFrame(repo_stats['repos'])
df
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
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>313</th>
      <td>serenity-core</td>
      <td>serenity-bdd/serenity-core</td>
      <td>https://github.com/serenity-bdd/serenity-core</td>
      <td>False</td>
      <td>378</td>
      <td>91</td>
      <td>3281</td>
      <td>509</td>
      <td>94</td>
      <td>[{'total': 6, 'week': 1575763200, 'days': [0, ...</td>
      <td>1907</td>
      <td>2318</td>
      <td>449</td>
      <td>462</td>
      <td>19</td>
      <td>{'HTML': 38083652, 'JavaScript': 8477607, 'Jav...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>314</th>
      <td>tc-play</td>
      <td>bwalex/tc-play</td>
      <td>https://github.com/bwalex/tc-play</td>
      <td>False</td>
      <td>52</td>
      <td>5</td>
      <td>257</td>
      <td>507</td>
      <td>52</td>
      <td>[{'total': 0, 'week': 1575763200, 'days': [0, ...</td>
      <td>72</td>
      <td>77</td>
      <td>14</td>
      <td>14</td>
      <td>0</td>
      <td>{'C': 165412, 'Gherkin': 80135, 'Roff': 36494,...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>315</th>
      <td>omnisharp-emacs</td>
      <td>OmniSharp/omnisharp-emacs</td>
      <td>https://github.com/OmniSharp/omnisharp-emacs</td>
      <td>False</td>
      <td>91</td>
      <td>52</td>
      <td>1472</td>
      <td>502</td>
      <td>23</td>
      <td>[{'total': 0, 'week': 1575763200, 'days': [0, ...</td>
      <td>453</td>
      <td>513</td>
      <td>232</td>
      <td>234</td>
      <td>61</td>
      <td>{'Emacs Lisp': 206411, 'Shell': 3530, 'Gherkin...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>316</th>
      <td>jbang</td>
      <td>jbangdev/jbang</td>
      <td>https://github.com/jbangdev/jbang</td>
      <td>False</td>
      <td>62</td>
      <td>41</td>
      <td>864</td>
      <td>507</td>
      <td>18</td>
      <td>[{'total': 0, 'week': 1575763200, 'days': [0, ...</td>
      <td>465</td>
      <td>549</td>
      <td>278</td>
      <td>289</td>
      <td>21</td>
      <td>{'Java': 400787, 'Shell': 35843, 'Gherkin': 97...</td>
      <td>None</td>
    </tr>
    <tr>
      <th>317</th>
      <td>mooltipass</td>
      <td>limpkin/mooltipass</td>
      <td>https://github.com/limpkin/mooltipass</td>
      <td>False</td>
      <td>109</td>
      <td>33</td>
      <td>3911</td>
      <td>500</td>
      <td>73</td>
      <td>[{'total': 0, 'week': 1575763200, 'days': [0, ...</td>
      <td>536</td>
      <td>574</td>
      <td>379</td>
      <td>380</td>
      <td>125</td>
      <td>{'C': 6169380, 'HTML': 4190322, 'G-code': 2853...</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>318 rows × 17 columns</p>
</div>




```python
series = repo_lang_df[(repo_lang_df['languages'] == 'Java') & (repo_lang_df['# Feat files'] > 1)]['Repository']
for i in series:
    print(df[df['full_name'] == i])
```


```python
repo_lang_df.insert(loc=2, column='# Step files', value=0)
repo_lang_df
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
      <th>Repository</th>
      <th># Feat files</th>
      <th># Step files</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>torvalds/linux</td>
      <td>1</td>
      <td>0</td>
      <td>C</td>
    </tr>
    <tr>
      <th>1</th>
      <td>iluwatar/java-design-patterns</td>
      <td>1</td>
      <td>0</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>2</th>
      <td>jekyll/jekyll</td>
      <td>28</td>
      <td>0</td>
      <td>Ruby</td>
    </tr>
    <tr>
      <th>3</th>
      <td>eugenp/tutorials</td>
      <td>13</td>
      <td>0</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>4</th>
      <td>hashicorp/consul</td>
      <td>114</td>
      <td>0</td>
      <td>Go</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>307</th>
      <td>javrasya/django-river</td>
      <td>4</td>
      <td>0</td>
      <td>Python</td>
    </tr>
    <tr>
      <th>308</th>
      <td>code-ready/crc</td>
      <td>9</td>
      <td>0</td>
      <td>Go</td>
    </tr>
    <tr>
      <th>309</th>
      <td>peej/tonic</td>
      <td>25</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>310</th>
      <td>code-dot-org/code-dot-org</td>
      <td>200</td>
      <td>0</td>
      <td>JavaScript</td>
    </tr>
    <tr>
      <th>311</th>
      <td>facebook/remodel</td>
      <td>28</td>
      <td>0</td>
      <td>TypeScript</td>
    </tr>
  </tbody>
</table>
<p>312 rows × 4 columns</p>
</div>



### Step files in Repos with Java as primary language


```python
repo_lang_df[repo_lang_df['languages'] == 'Java'].head()
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
      <th>Repository</th>
      <th># Feat files</th>
      <th># Step files</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>iluwatar/java-design-patterns</td>
      <td>1</td>
      <td>0</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>3</th>
      <td>eugenp/tutorials</td>
      <td>13</td>
      <td>0</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>24</th>
      <td>neo4j/neo4j</td>
      <td>26</td>
      <td>0</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>25</th>
      <td>TooTallNate/Java-WebSocket</td>
      <td>1</td>
      <td>0</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Vedenin/useful-java-links</td>
      <td>1</td>
      <td>0</td>
      <td>Java</td>
    </tr>
  </tbody>
</table>
</div>



### Add the step files to the repos manually

Here, I check each repository manually and search for 'Cucumber' in the search section on the GitHub page(top left). All the resulting files are scanned and if any file has 'Cucumber' in the commented sections or in an obsolete component, these files are not considered.


```python
repository = 'Vedenin/useful-java-links'
repo_lang_df.loc[repo_lang_df.Repository == repository, '# Step files'] = 1
```


```python
repo_lang_df[repo_lang_df['languages'] == 'Java'].head()
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
      <th>Repository</th>
      <th># Feat files</th>
      <th># Step files</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>iluwatar/java-design-patterns</td>
      <td>1</td>
      <td>4</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>3</th>
      <td>eugenp/tutorials</td>
      <td>13</td>
      <td>39</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>24</th>
      <td>neo4j/neo4j</td>
      <td>26</td>
      <td>0</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>25</th>
      <td>TooTallNate/Java-WebSocket</td>
      <td>1</td>
      <td>0</td>
      <td>Java</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Vedenin/useful-java-links</td>
      <td>1</td>
      <td>1</td>
      <td>Java</td>
    </tr>
  </tbody>
</table>
</div>



### Step files in Repos with Python as primary language


```python
repo_lang_df[repo_lang_df['languages'] == 'Python'].head()
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
      <th>Repository</th>
      <th># Feat files</th>
      <th># Step files</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15</th>
      <td>aio-libs/aiohttp</td>
      <td>28</td>
      <td>0</td>
      <td>Python</td>
    </tr>
    <tr>
      <th>18</th>
      <td>dbcli/mycli</td>
      <td>8</td>
      <td>0</td>
      <td>Python</td>
    </tr>
    <tr>
      <th>19</th>
      <td>dbcli/pgcli</td>
      <td>8</td>
      <td>0</td>
      <td>Python</td>
    </tr>
    <tr>
      <th>29</th>
      <td>matrix-org/synapse</td>
      <td>7</td>
      <td>0</td>
      <td>Python</td>
    </tr>
    <tr>
      <th>31</th>
      <td>pytest-dev/pytest</td>
      <td>1</td>
      <td>0</td>
      <td>Python</td>
    </tr>
  </tbody>
</table>
</div>



### Add the step files to the repos manually

Here, I check each repository manually and search for 'Cucumber' in the search section on the GitHub page(top left). All the resulting files are scanned and if any file has 'Cucumber' in the commented sections or in an obsolete component, these files are not considered.

## No Cucumber files in any of the Python repo

### Step files in Repos with PHP as primary language


```python
repo_lang_df[repo_lang_df['languages'] == 'PHP'].head()
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
      <th>Repository</th>
      <th># Feat files</th>
      <th># Step files</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>9</th>
      <td>nextcloud/server</td>
      <td>49</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>16</th>
      <td>docker/labs</td>
      <td>5</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>27</th>
      <td>owncloud/core</td>
      <td>467</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>33</th>
      <td>woocommerce/woocommerce</td>
      <td>7</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Sylius/Sylius</td>
      <td>633</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
  </tbody>
</table>
</div>



### Add the step files to the repos manually

Here, I check each repository manually and search for 'Cucumber' in the search section on the GitHub page(top left). All the resulting files are scanned and if any file has 'Cucumber' in the commented sections or in an obsolete component, these files are not considered.


```python
repository = 'nextcloud/server'
repo_lang_df.loc[repo_lang_df.Repository == repository, '# Step files'] = 1
```


```python
repo_lang_df[repo_lang_df['languages'] == 'PHP'].head()
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
      <th>Repository</th>
      <th># Feat files</th>
      <th># Step files</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>9</th>
      <td>nextcloud/server</td>
      <td>49</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>16</th>
      <td>docker/labs</td>
      <td>5</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>27</th>
      <td>owncloud/core</td>
      <td>467</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>33</th>
      <td>woocommerce/woocommerce</td>
      <td>7</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Sylius/Sylius</td>
      <td>633</td>
      <td>0</td>
      <td>PHP</td>
    </tr>
  </tbody>
</table>
</div>



### Step files in Repos with Ruby as primary language


```python
repo_lang_df[repo_lang_df['languages'] == 'Ruby'].head()
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
      <th>Repository</th>
      <th># Feat files</th>
      <th># Step files</th>
      <th>languages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>jekyll/jekyll</td>
      <td>28</td>
      <td>0</td>
      <td>Ruby</td>
    </tr>
    <tr>
      <th>8</th>
      <td>diaspora/diaspora</td>
      <td>70</td>
      <td>0</td>
      <td>Ruby</td>
    </tr>
    <tr>
      <th>10</th>
      <td>capistrano/capistrano</td>
      <td>8</td>
      <td>0</td>
      <td>Ruby</td>
    </tr>
    <tr>
      <th>17</th>
      <td>teamcapybara/capybara</td>
      <td>2</td>
      <td>0</td>
      <td>Ruby</td>
    </tr>
    <tr>
      <th>20</th>
      <td>thoughtbot/paperclip</td>
      <td>3</td>
      <td>0</td>
      <td>Ruby</td>
    </tr>
  </tbody>
</table>
</div>



### Add the step files to the repos manually

Here, I check each repository manually and search for 'Cucumber' in the search section on the GitHub page(top left). All the resulting files are scanned and if any file has 'Cucumber' in the commented sections or in an obsolete component, these files are not considered.


```python
repository = 'jekyll/jekyll'
repo_lang_df.loc[repo_lang_df.Repository == repository, '# Step files'] = 1
```


```python
repo_lang_df[repo_lang_df['languages'] == 'Ruby'].head()
```
