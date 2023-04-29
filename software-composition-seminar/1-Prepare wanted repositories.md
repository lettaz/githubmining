# Prepare wanted repos


```python
import pickle
import pandas as pd
```


```python
with open('./wanted.pickle', 'rb') as file:
    data = pickle.load(file)
df = pd.DataFrame(data)
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
      <th>counter</th>
      <th>wanted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20</th>
      <td>56240</td>
      <td>torvalds/linux</td>
    </tr>
    <tr>
      <th>54</th>
      <td>56240</td>
      <td>iluwatar/java-design-patterns</td>
    </tr>
    <tr>
      <th>127</th>
      <td>56240</td>
      <td>jekyll/jekyll</td>
    </tr>
    <tr>
      <th>378</th>
      <td>56240</td>
      <td>eugenp/tutorials</td>
    </tr>
    <tr>
      <th>497</th>
      <td>56240</td>
      <td>hashicorp/consul</td>
    </tr>
    <tr>
      <th>505</th>
      <td>56240</td>
      <td>github/hub</td>
    </tr>
    <tr>
      <th>530</th>
      <td>56240</td>
      <td>influxdata/influxdb</td>
    </tr>
    <tr>
      <th>876</th>
      <td>56240</td>
      <td>elastic/kibana</td>
    </tr>
    <tr>
      <th>1219</th>
      <td>56240</td>
      <td>diaspora/diaspora</td>
    </tr>
    <tr>
      <th>1264</th>
      <td>56240</td>
      <td>nextcloud/server</td>
    </tr>
  </tbody>
</table>
</div>




```python
wanted_keys = [ key for key in data['wanted'].keys() if key < 53457]
wanted_repos = { key: data['wanted'][key] for key in wanted_keys }
wanted_repos
```




    {20: 'torvalds/linux',
     54: 'iluwatar/java-design-patterns',
     127: 'jekyll/jekyll',
     378: 'eugenp/tutorials',
     497: 'hashicorp/consul',
     505: 'github/hub',
     530: 'influxdata/influxdb',
     876: 'elastic/kibana',
     1219: 'diaspora/diaspora',
     1264: 'nextcloud/server',
     1380: 'capistrano/capistrano',
     1414: 'Instagram/IGListKit',
     1445: 'karma-runner/karma',
     1493: 'gradle/gradle',
     1633: 'AlDanial/cloc',
     1638: 'aio-libs/aiohttp',
     1925: 'docker/labs',
     2000: 'teamcapybara/capybara',
     2017: 'dbcli/mycli',
     2042: 'dbcli/pgcli',
     2059: 'thoughtbot/paperclip',
     2082: 'thoughtbot/bourbon',
     2121: 'activeadmin/activeadmin',
     2246: 'carrierwaveuploader/carrierwave',
     2328: 'neo4j/neo4j',
     2727: 'TooTallNate/Java-WebSocket',
     2748: 'raspberrypi/linux',
     2823: 'owncloud/core',
     2941: 'thoughtbot/factory_bot',
     3025: 'matrix-org/synapse',
     3034: 'Compass/compass',
     3097: 'pytest-dev/pytest',
     3134: 'middleman/middleman',
     3226: 'woocommerce/woocommerce',
     3382: 'aws/aws-sdk-js',
     3516: 'webdriverio/webdriverio',
     3558: 'Sylius/Sylius',
     3613: 'qutebrowser/qutebrowser',
     3619: 'github/super-linter',
     3664: 'guard/guard',
     3705: 'bigbluebutton/bigbluebutton',
     4035: 'guardian/frontend',
     4274: 'jnunemaker/httparty',
     4306: 'PrestaShop/PrestaShop',
     4547: 'cucumber/cucumber-ruby',
     4573: 'aws/aws-sdk-php',
     4671: 'vcr/vcr',
     4794: 'Vedenin/useful-java-links',
     4822: 'mRemoteNG/mRemoteNG',
     5171: 'jrnl-org/jrnl',
     5201: 'cloudera/hue',
     5270: 'buildbot/buildbot',
     5355: 'rspec/rspec-rails',
     5391: 'lolcommits/lolcommits',
     5577: 'microsoft/WSL2-Linux-Kernel',
     5601: 'greenplum-db/gpdb',
     5606: 'wp-cli/wp-cli',
     5729: 'intuit/karate',
     5731: 'Codeception/Codeception',
     5823: 'simplecov-ruby/simplecov',
     5900: 'twisted/twisted',
     5931: 'Project-OSRM/osrm-backend',
     5932: 'cucumber/cucumber-js',
     6307: 'zephyrproject-rtos/zephyr',
     6713: 'sdkman/sdkman-cli',
     6848: 'zalando/patroni',
     7027: 'apiaryio/dredd',
     7096: 'Hygieia/Hygieia',
     7171: 'xcpretty/xcpretty',
     7224: 'Behat/Behat',
     7356: 'troessner/reek',
     7365: 'aws/aws-sdk-java',
     7413: 'mapsme/omim',
     7423: 'asciidoctor/asciidoctor',
     7448: 'javaparser/javaparser',
     7508: 'moodle/moodle',
     7987: 'aws/aws-sdk-ruby',
     8050: 'quantumblacklabs/kedro',
     8151: 'codeceptjs/CodeceptJS',
     8195: 'facebookresearch/hydra',
     8342: 'cucumber/cucumber',
     8485: 'carrot/share-button',
     8613: 'emacs-lsp/lsp-mode',
     9202: 'ckan/ckan',
     9376: 'phpDocumentor/phpDocumentor',
     9806: 'whitesmith/rubycritic',
     9863: 'DatabaseCleaner/database_cleaner',
     9970: 'jejacks0n/mercury',
     10027: 'thoughtbot/factory_bot_rails',
     10031: 'google/hover',
     10354: 'tensorflow/datasets',
     10533: 'eclipse/openj9',
     10661: 'thiagopradi/octopus',
     10876: 'graknlabs/grakn',
     10910: 'python-openxml/python-docx',
     11017: 'lucadegasperi/oauth2-server-laravel',
     11236: 'alohaeditor/Aloha-Editor',
     11572: 'SAP/openui5',
     12037: 'cucumber/cucumber-jvm',
     12114: 'OrchardCMS/Orchard',
     12124: 'behave/behave',
     12267: 'cappuccino/cappuccino',
     12368: 'Dolibarr/dolibarr',
     12403: 'geoserver/geoserver',
     12681: 'motiv-labs/janus',
     12773: 'minishift/minishift',
     13082: 'sharetribe/sharetribe',
     13391: 'tinytacoteam/zazu',
     13686: 'novoda/android-demos',
     14521: 'flowplayer/flowplayer',
     14663: 'git-town/git-town',
     14996: 'magnars/multiple-cursors.el',
     15003: 'pnp/PnP',
     15176: 'calabash/calabash-ios',
     15182: 'crossbario/crossbar',
     15373: 'apache/servicecomb-pack',
     15473: 'EasyEngine/easyengine',
     15498: 'Covid-19Radar/Covid19Radar',
     15505: 'dchelimsky/rspec',
     15684: 'test-kitchen/test-kitchen',
     15722: 'phpspec/phpspec',
     15921: 'hyperledger-archives/composer',
     16025: 'retorquere/zotero-better-bibtex',
     16082: 'api-platform/core',
     16136: 'radiant/radiant',
     16214: 'iridakos/duckrails',
     16591: 'SpecFlowOSS/SpecFlow',
     16626: 'calabash/calabash-android',
     16978: 'wal-g/wal-g',
     17000: 'appium-boneyard/sample-code',
     17095: 'barnybug/cli53',
     17164: 'JetBrains/intellij-plugins',
     17213: 'osm-search/Nominatim',
     17502: 'robinhood-unofficial/pyrh',
     17859: 'aio-libs/aioredis',
     18152: 'radar/guides',
     18166: 'microservices-patterns/ftgo-application',
     18311: 'technicalpickles/jeweler',
     18419: 'Yelp/paasta',
     18706: 'petems/tugboat',
     19364: 'overtone/emacs-live',
     19927: 'egonSchiele/contracts.ruby',
     20268: 'FriendsOfPHP/pickle',
     20651: 'zipmark/rspec_api_documentation',
     20748: 'CodelyTV/php-ddd-example',
     20999: 'Luracast/Restler',
     21158: 'Brightify/Cuckoo',
     22435: 'OpenCover/opencover',
     22460: 'OpenCover/opencover',
     22483: 'apache/tinkerpop',
     22578: 'microsoftarchive/cqrs-journey',
     22648: 'gabrielfalcao/lettuce',
     22895: 'vim-vdebug/vdebug',
     23003: 'RedisGraph/RedisGraph',
     23234: 'cucumber/godog',
     23345: 'hyperledger-archives/fabric',
     23419: 'browsermedia/browsercms',
     23443: 'iotaledger/iri',
     23482: 'scanny/python-pptx',
     23517: 'JoshCheek/seeing_is_believing',
     23586: 'shoes/shoes-deprecated',
     23653: 'shopware/shopware',
     23839: 'davetron5000/gli',
     23963: 'humbug/humbug',
     24030: 'thought-machine/please',
     24168: 'gitana/alpaca',
     24178: 'tuist/tuist',
     24376: 'SmartBear/soapui',
     24392: 'email-spec/email-spec',
     24443: 'millejoh/emacs-ipython-notebook',
     24661: 'Sceptre/sceptre',
     24746: 'cask/cask',
     24910: 'naparuba/shinken',
     25024: 'DyCI/dyci-main',
     25154: 'chef/omnibus',
     25195: 'jtpereyda/boofuzz',
     25291: 'rspec/rspec-expectations',
     25461: 'berkshelf/berkshelf',
     25783: 'piotrmurach/github',
     26295: 'rspec/rspec-core',
     26618: 'w3c/epubcheck',
     27025: 'magnars/expand-region.el',
     27068: 'tahoe-lafs/tahoe-lafs',
     27162: 'paulelliott/fabrication',
     27265: 'dbcli/mssql-cli',
     27418: 'rspec/rspec-mocks',
     27795: 'apache/usergrid',
     27820: 'aws/aws-sdk-java-v2',
     28060: 'bugsnag/bugsnag-android',
     28073: 'cucumber/cucumber-rails',
     28293: 'particle-iot/device-os',
     28689: 'jeremyw/stamp',
     28969: 'GeoNode/geonode',
     29166: 'blox/blox',
     29571: 'kmmbvnr/django-jenkins',
     29586: 'kmmbvnr/django-jenkins',
     29712: 'kuzzleio/kuzzle',
     29759: 'ddd-by-examples/factory',
     29947: 'progrium/buildstep',
     30115: 'collectiveidea/json_spec',
     30219: 'openshift/origin-server',
     30410: 'boto/botocore',
     30427: 'nextcloud/spreed',
     30670: 'cucumber/aruba',
     30926: 'Behat/Gherkin',
     31564: 'coderwall/coderwall-legacy',
     32001: 'gauntlt/gauntlt',
     32281: 'screwdriver-cd/screwdriver',
     32735: 'inukshuk/jekyll-scholar',
     32967: 'PolySync/oscc',
     32990: 'eerkunt/terraform-compliance',
     33059: 'xaviershay/enki',
     33263: 'OpenLiberty/open-liberty',
     33301: 'gterrono/houston',
     33747: 'K-Phoen/rulerz',
     33817: 'arches/table_print',
     34002: 'buffer/thug',
     34507: 'ttscoff/doing',
     34702: 'teslamotors/linux',
     35083: 'backlogs/redmine_backlogs',
     35134: 'errata-ai/vale',
     35391: 'KKBOX/CompassApp',
     35840: 'Xilinx/linux-xlnx',
     35986: 'stelligent/cfn_nag',
     36144: 'emacs-lsp/dap-mode',
     36223: 'FluentLenium/FluentLenium',
     36314: 'akeneo/pim-community-dev',
     36344: 'OTRS/otrs',
     36420: 'codeliner/php-ddd-cargo-sample',
     36542: 'EnMarche/en-marche.fr',
     36545: 'AppiumTestDistribution/AppiumTestDistribution',
     36842: 'kantord/LibreLingo',
     37059: 'lonelyplanet/rizzo',
     37215: 'bugsnag/bugsnag-laravel',
     37307: 'OfficeDev/TrainingContent',
     37519: 'RefugeRestrooms/refugerestrooms',
     37683: 'owenthereal/gh',
     37689: 'Cocolabs-SAS/cocorico',
     37742: 'Cocolabs-SAS/cocorico',
     38435: 'rodjek/librarian-puppet',
     38918: 'iain/http_accept_language',
     39053: 'breser/git2consul',
     39267: 'multipath-tcp/mptcp',
     39327: 'copycopter/copycopter-server',
     39335: 'Enalean/tuleap',
     39377: 'johanvts/emacs-fireplace',
     39577: 'clojure-emacs/clj-refactor.el',
     40326: 'Tencent/TencentOS-kernel',
     40468: 'mzheravin/exchange-core',
     40524: 'matthewrenze/clean-architecture-demo',
     41161: 'hyperledger/fabric-sdk-node',
     41188: 'meteor-velocity/velocity',
     41238: 'vagrant-landrush/landrush',
     41435: 'Azure/azure-xplat-cli',
     41446: 'phpdocker-io/phpdocker.io',
     41469: 'otwcode/otwarchive',
     41512: 'flapjack/flapjack',
     41987: 'pytroll/satpy',
     41988: 'j-bennet/wharfee',
     42129: 'input-output-hk/daedalus',
     42395: 'cheezy/page-object',
     42493: 'SoftInstigate/restheart',
     42595: 'dmaicher/doctrine-test-bundle',
     43087: 'code-ready/crc',
     43183: 'peej/tonic',
     43418: 'alphagov/whitehall',
     43429: 'unused/airstream',
     43491: 'Behat/MinkExtension',
     43502: 'ari/jobsworth',
     43782: 'KKBOX/FireApp',
     43788: 'SonarOpenCommunity/sonar-cxx',
     44042: 'TheBrainFamily/cypress-cucumber-preprocessor',
     44621: 'code-dot-org/code-dot-org',
     44644: 'bugsnag/bugsnag-js',
     44866: 'nextcloud/deck',
     44889: 'nextcloud/deck',
     45267: 'oroinc/platform',
     45301: 'adhearsion/adhearsion',
     45533: 'SpaceMadness/lunar-unity-console',
     45570: 'jqr/heroku_san',
     46189: 'riscv/riscv-linux',
     46215: 'facebook/remodel',
     46268: 'cschiewek/devise_ldap_authenticatable',
     46347: 'apache/isis',
     46373: 'ushahidi/platform',
     47063: 'google/ktsan',
     47182: 'SteeltoeOSS/Samples',
     47416: 'QafooLabs/php-refactoring-browser',
     47455: 'cyberark/summon',
     47560: 'LiskHQ/lisk-desktop',
     47887: 'karlfreeman/middleman-deploy',
     47949: 'karlfreeman/middleman-deploy',
     48035: 'codegram/spinach',
     48199: 'javrasya/django-river',
     48617: 'luminus-framework/luminus-template',
     49100: 'kennethkalmer/daemon-kit',
     49405: 'emacsorphanage/god-mode',
     49725: 'oroinc/crm',
     49867: 'mojotech/pioneer',
     50033: 'howl-editor/howl',
     50295: 'qaprosoft/carina',
     50465: 'tmm1/test-queue',
     50865: 'tomafro/recap',
     51369: 'apache/camel-k',
     51486: 'beagleboard/linux',
     51796: 'AutomateThePlanet/AutomateThePlanet-Learning-Series',
     51877: 'ianwhite/pickle',
     52014: 'BBVA/kapow',
     52046: 'BBVA/kapow',
     52064: 'chamilo/chamilo-lms',
     52204: 'rake-compiler/rake-compiler',
     52293: 'Stratio/sparta',
     52413: 'iriusrisk/bdd-security',
     52453: 'serenity-bdd/serenity-core',
     52665: 'bwalex/tc-play',
     53268: 'OmniSharp/omnisharp-emacs',
     53366: 'jbangdev/jbang',
     53378: 'limpkin/mooltipass'}




```python

```
