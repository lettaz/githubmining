```python
from github import Github
import os
from pprint import pprint
import requests
import json
import pandas as pd
import statistics
import scipy.stats as stats
from cliffs_delta import cliffs_delta

import matplotlib.pyplot as plt
```


```python
token = "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"
g = Github(token)
```

## Repo list


```python
repos = ["eugenp/tutorials", "neo4j/neo4j", "geoserver/geoserver", "apache/servicecomb-pack", 
         "microservices-patterns/ftgo-application", "apache/tinkerpop", "iotaledger/iri", "SmartBear/soapui", 
         "w3c/epubcheck", "aws/aws-sdk-java-v2", "bugsnag/bugsnag-android", "blox/blox", "ddd-by-examples/factory", 
         "FluentLenium/FluentLenium", "AppiumTestDistribution/AppiumTestDistribution", "mzheravin/exchange-core", 
         "iriusrisk/bdd-security", "jbangdev/jbang", "SoftInstigate/restheart", "intuit/karate", "cucumber/common", 
         "cucumber/cucumber-jvm", "JetBrains/intellij-plugins"]
```

## Parse file and count Gherkin syntaxes

1. Iterate over each repo, fetch all the files containing `Gherkin` <br>
2. For each file, parse it and fetch the first words for each line <br>
3. Match it with Gherkin syntaxes
4. If they match, save the stats in a dict for each file in each repo

## Use this only if running for the first time otherwise, import the previous stats from repo_stats.json file


```python
'''keywords = ["and", "then", "when", "feature:", "scenario:", "scenario outline:", "given", "background:", "|", "examples:"]

repo_stats = {}
repo_stats['repos'] = {}'''
```


```python
'''def initialize_count():
    # Initialize the counter for keyword_stats
    count = {
        "and": 0,
        "then": 0,
        "when": 0,
        "feature:" : 0,
        "scenario:": 0,
        "given": 0,
        "@": 0,
        "background:": 0,
        "|":0,
        "scenario outline:": 0,
        "examples:":0}
    
    scenario_encountered = False
    table_detected = False
    code_break = False
    
    n_cols = 0
    n_rows = 0
    n_tables = 0
    
    return count, scenario_encountered, table_detected, n_cols, n_rows, n_tables, code_break

def count_cols(line):
    words = line.split()
    n_cols = words.count("|") - 1
    
    return n_cols

def check_first_word(first_word):
    # boolean tags that check if the first word is a keyword and if it has occured previously
    is_keyword = first_word in keywords or "@" in first_word
    
    # If the line contains a tag i.e @, set 'is_tag' to True
    is_tag = "@" in first_word
    
    return is_keyword, is_tag'''
```


```python
'''for repo in repos:
    print("repo_name:", repo)
    
    file_stats = {}
    file_count = 0
    files_with_table = 0
    avg_cols_table = 0
    avg_rows_table = 0
    
    result = g.search_code(f'repo:{repo}  language:Gherkin')
    
    for file in result:
        file_count += 1
        
        count, scenario_encountered, table_detected, n_cols, n_rows, n_tables, code_break = initialize_count()
        file_name = f'{file.download_url}'
        
        r = requests.get(file.download_url)
        mh = r.content 
        
        loc_val = 0
        loc_scenario_val = 0
        for line in mh.splitlines():
            try:
                line = line.decode("utf-8")
            except:
                code_break = True
                break
            line = line.strip()
            if (len(line) > 0):
                line = line.lower()
                first_word = line.split()[0]
                first_word = first_word.strip()
                if len(line.split()) > 1:
                    second_word = line.split()[1]
                    second_word = second_word.strip()                
                
                if "#" not in first_word:
                    loc_val += 1
                    
                    if "scenario" in first_word and "outline" in second_word:
                        first_word = "scenario outline:"
                    
                    is_keyword, is_tag = check_first_word(first_word)

                    if "scenario" in first_word and not scenario_encountered:
                        loc_scenario_val = loc_val - 1
                        scenario_encountered = True

                    if is_keyword:
                        if is_tag:
                            count["@"] = count["@"] + 1
                        else:
                            count[first_word] = count[first_word] + 1

                        if "|" in first_word:
                            if not table_detected:
                                n_cols = count_cols(line)
                                n_tables += 1
                                n_rows += 1
                                table_detected = True
                            else:
                                n_rows += 1
                        else:
                            if table_detected:
                                table_detected = False
        
        if not code_break:
            file_stats[file_name] = {}
            file_stats[file_name]['LoC'] = loc_val
            
            if loc_scenario_val > 0:
                file_stats[file_name]['LoC_scenario'] = loc_val - loc_scenario_val
            else:
                file_stats[file_name]['LoC_scenario'] = 0
            
            file_stats[file_name]['n_cols_table'] = n_cols
            file_stats[file_name]['n_rows_table'] = n_rows
            file_stats[file_name]['n_tables'] = n_tables
            file_stats[file_name]['keyword_stats'] = count
            
            if count["|"] > 0: 
                files_with_table += 1
    
    repo_stats['repos'][repo] = {}
    repo_stats['repos'][repo]['files'] = file_count
    repo_stats['repos'][repo]['files_with_tables'] = files_with_table
    repo_stats['repos'][repo]['file_stats'] = file_stats'''
```

    repo_name: eugenp/tutorials
    repo_name: neo4j/neo4j
    repo_name: geoserver/geoserver
    repo_name: apache/servicecomb-pack
    repo_name: microservices-patterns/ftgo-application
    repo_name: apache/tinkerpop
    repo_name: iotaledger/iri
    repo_name: SmartBear/soapui
    repo_name: w3c/epubcheck
    repo_name: aws/aws-sdk-java-v2
    repo_name: bugsnag/bugsnag-android
    repo_name: blox/blox
    repo_name: ddd-by-examples/factory
    repo_name: FluentLenium/FluentLenium
    repo_name: AppiumTestDistribution/AppiumTestDistribution
    repo_name: mzheravin/exchange-core
    repo_name: iriusrisk/bdd-security
    repo_name: jbangdev/jbang
    repo_name: SoftInstigate/restheart
    repo_name: intuit/karate
    repo_name: cucumber/common
    repo_name: cucumber/cucumber-jvm
    repo_name: JetBrains/intellij-plugins
    


```python
'''r = json.dumps(repo_stats)'''
```


```python
'''with open('repo_stats.json', 'w') as fp:
    json.dump(repo_stats, fp)'''
```

## Load the data mentioned in the paper

Load the repowise stats from json file which give the exact results as mentioned in the paper


```python
with open('repo_stats.json') as f:
    repo_stats = json.load(f)
```


```python
repo_stats
```

## Tables in Feat files


```python
total_files = 0
files_with_table = 0

n_cols_list = []
n_rows_list = []

n_tables_files = 0

for repo in repo_stats['repos'].keys():
    total_files += repo_stats['repos'][repo]['files']
    files_with_table += repo_stats['repos'][repo]['files_with_tables']
    n_tables_repo = 0

    for file in repo_stats['repos'][repo]['file_stats'].keys():
        n_cols_table = repo_stats['repos'][repo]['file_stats'][file]['n_cols_table']
        n_rows_table = repo_stats['repos'][repo]['file_stats'][file]['n_rows_table']
        n_tables = repo_stats['repos'][repo]['file_stats'][file]['n_tables']
                
        n_tables_files += n_tables
        n_tables_repo += n_tables
        
        if n_cols_table > 0:
            n_cols_list.append(n_cols_table)
        
        if n_rows_table > 0:
            n_rows_list.append(n_rows_table/n_tables)
    
    print(f">> Stats for repo {repo}:")
    
    if repo_stats['repos'][repo]['files_with_tables'] > 0:
        print(f"An average number of tables per feature file are : {round(n_tables_repo/repo_stats['repos'][repo]['files_with_tables'],2)}\n")
    else:
        print(f"No feature file with tables\n")

avg_n_cols = statistics.mean(n_cols_list)
avg_n_rows = statistics.mean(n_rows_list)
        
print(f"Out of {len(repo_stats['repos'])} repos, we analyzed around {total_files} feat files of which only {files_with_table} used tables")

print(f"The average no. of columns per table are: {avg_n_cols}")
print(f"The average no. of rows per table are: {avg_n_rows}")

print(f"\nAn average number of tables per feature file are : {round(n_tables_files/files_with_table,2)}")
```

    >> Stats for repo eugenp/tutorials:
    An average number of tables per feature file are : 2.0
    
    >> Stats for repo neo4j/neo4j:
    An average number of tables per feature file are : 13.04
    
    >> Stats for repo geoserver/geoserver:
    No feature file with tables
    
    >> Stats for repo apache/servicecomb-pack:
    An average number of tables per feature file are : 3.63
    
    >> Stats for repo microservices-patterns/ftgo-application:
    No feature file with tables
    
    >> Stats for repo apache/tinkerpop:
    An average number of tables per feature file are : 10.81
    
    >> Stats for repo iotaledger/iri:
    An average number of tables per feature file are : 5.5
    
    >> Stats for repo SmartBear/soapui:
    An average number of tables per feature file are : 2.0
    
    >> Stats for repo w3c/epubcheck:
    An average number of tables per feature file are : 1.54
    
    >> Stats for repo aws/aws-sdk-java-v2:
    An average number of tables per feature file are : 1.17
    
    >> Stats for repo bugsnag/bugsnag-android:
    An average number of tables per feature file are : 2.17
    
    >> Stats for repo blox/blox:
    An average number of tables per feature file are : 4.4
    
    >> Stats for repo ddd-by-examples/factory:
    An average number of tables per feature file are : 16.5
    
    >> Stats for repo FluentLenium/FluentLenium:
    No feature file with tables
    
    >> Stats for repo AppiumTestDistribution/AppiumTestDistribution:
    An average number of tables per feature file are : 1.0
    
    >> Stats for repo mzheravin/exchange-core:
    An average number of tables per feature file are : 24.0
    
    >> Stats for repo iriusrisk/bdd-security:
    An average number of tables per feature file are : 1.4
    
    >> Stats for repo jbangdev/jbang:
    No feature file with tables
    
    >> Stats for repo SoftInstigate/restheart:
    No feature file with tables
    
    >> Stats for repo intuit/karate:
    An average number of tables per feature file are : 2.07
    
    >> Stats for repo cucumber/common:
    An average number of tables per feature file are : 21.04
    
    >> Stats for repo cucumber/cucumber-jvm:
    An average number of tables per feature file are : 2.31
    
    >> Stats for repo JetBrains/intellij-plugins:
    An average number of tables per feature file are : 1.2
    
    Out of 23 repos, we analyzed around 1572 feat files of which only 590 used tables
    The average no. of columns per table are: 1.7108843537414966
    The average no. of rows per table are: 2.6087884841833766
    
    An average number of tables per feature file are : 11.41
    

## LoC and LoC_Scenario in Feat Files


```python
loc_files_total = 0
loc_scenario_files_total = 0
loc_file_table_total = 0
loc_scenario_file_table_total = 0
total_files = 0
files_with_table = 0

loc_files_w_tables = []
loc_files_wo_tables = []

for repo in repo_stats['repos'].keys():
    loc_file, loc_scenario_file, loc_file_table, loc_scenario_file_table = 0, 0, 0, 0
    
    total_files_in_repo = repo_stats['repos'][repo]['files']
    files_with_tables_in_repo = repo_stats['repos'][repo]['files_with_tables']
    
    total_files += total_files_in_repo
    files_with_table += files_with_tables_in_repo

    for file in repo_stats['repos'][repo]['file_stats'].keys():
        loc_file += repo_stats['repos'][repo]['file_stats'][file]['LoC']
        loc_scenario_file += repo_stats['repos'][repo]['file_stats'][file]['LoC_scenario']
        
        if repo_stats['repos'][repo]['file_stats'][file]['n_tables'] > 0:
            loc_file_table += repo_stats['repos'][repo]['file_stats'][file]['LoC']
            loc_scenario_file_table += repo_stats['repos'][repo]['file_stats'][file]['LoC_scenario']
            loc_files_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['LoC'])
        else:
            loc_files_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['LoC'])
        
    loc_files_total += loc_file
    loc_scenario_files_total += loc_scenario_file
    loc_file_table_total += loc_file_table
    loc_scenario_file_table_total += loc_scenario_file_table
    
    print(f">> For repo {repo} : ")
    print(f"Average LoC per feature file : {round(loc_file/total_files_in_repo,2)}")
    print(f"Average LoC in Scenario per feature file : {round(loc_scenario_file/total_files_in_repo,2)}")
    if repo_stats['repos'][repo]['files_with_tables'] > 0:
        print(f"Average LoC per feature file(having tables) : {round(loc_file_table/files_with_tables_in_repo,2)}")
        print(f"Average LoC in Scenario per feature file(having tables) : {round(loc_scenario_file_table/files_with_tables_in_repo,2)}\n")
    else:
        print(f"\n")
    
print(f"In general for the feature files:")
print(f"Average LoC per feature file : {round(loc_files_total/total_files,2)}")
print(f"Average LoC in Scenario per feature file : {round(loc_scenario_files_total/total_files,2)}")
print(f"Average LoC per feature file(having tables): {round(loc_file_table_total/files_with_table,2)}")
print(f"Average LoC in Scenario per feature file (having tables): {round(loc_scenario_file_table_total/files_with_table,2)}")
```

    >> For repo eugenp/tutorials : 
    Average LoC per feature file : 13.93
    Average LoC in Scenario per feature file : 11.6
    Average LoC per feature file(having tables) : 19.6
    Average LoC in Scenario per feature file(having tables) : 15.8
    
    >> For repo neo4j/neo4j : 
    Average LoC per feature file : 217.15
    Average LoC in Scenario per feature file : 215.11
    Average LoC per feature file(having tables) : 217.15
    Average LoC in Scenario per feature file(having tables) : 215.11
    
    >> For repo geoserver/geoserver : 
    Average LoC per feature file : 62.0
    Average LoC in Scenario per feature file : 58.75
    
    
    >> For repo apache/servicecomb-pack : 
    Average LoC per feature file : 25.97
    Average LoC in Scenario per feature file : 24.97
    Average LoC per feature file(having tables) : 25.97
    Average LoC in Scenario per feature file(having tables) : 24.97
    
    >> For repo microservices-patterns/ftgo-application : 
    Average LoC per feature file : 15.0
    Average LoC in Scenario per feature file : 12.0
    
    
    >> For repo apache/tinkerpop : 
    Average LoC per feature file : 157.39
    Average LoC in Scenario per feature file : 156.39
    Average LoC per feature file(having tables) : 160.5
    Average LoC in Scenario per feature file(having tables) : 159.5
    
    >> For repo iotaledger/iri : 
    Average LoC per feature file : 75.4
    Average LoC in Scenario per feature file : 71.6
    Average LoC per feature file(having tables) : 118.5
    Average LoC in Scenario per feature file(having tables) : 116.5
    
    >> For repo SmartBear/soapui : 
    Average LoC per feature file : 31.87
    Average LoC in Scenario per feature file : 29.45
    Average LoC per feature file(having tables) : 29.0
    Average LoC in Scenario per feature file(having tables) : 26.67
    
    >> For repo w3c/epubcheck : 
    Average LoC per feature file : 97.69
    Average LoC in Scenario per feature file : 89.58
    Average LoC per feature file(having tables) : 145.38
    Average LoC in Scenario per feature file(having tables) : 137.54
    
    >> For repo aws/aws-sdk-java-v2 : 
    Average LoC per feature file : 10.51
    Average LoC in Scenario per feature file : 8.42
    Average LoC per feature file(having tables) : 10.51
    Average LoC in Scenario per feature file(having tables) : 8.42
    
    >> For repo bugsnag/bugsnag-android : 
    Average LoC per feature file : 38.2
    Average LoC in Scenario per feature file : 37.04
    Average LoC per feature file(having tables) : 50.17
    Average LoC in Scenario per feature file(having tables) : 49.0
    
    >> For repo blox/blox : 
    Average LoC per feature file : 23.4
    Average LoC in Scenario per feature file : 19.1
    Average LoC per feature file(having tables) : 29.8
    Average LoC in Scenario per feature file(having tables) : 24.8
    
    >> For repo ddd-by-examples/factory : 
    Average LoC per feature file : 84.5
    Average LoC in Scenario per feature file : 58.5
    Average LoC per feature file(having tables) : 84.5
    Average LoC in Scenario per feature file(having tables) : 58.5
    
    >> For repo FluentLenium/FluentLenium : 
    Average LoC per feature file : 8.82
    Average LoC in Scenario per feature file : 7.82
    
    
    >> For repo AppiumTestDistribution/AppiumTestDistribution : 
    Average LoC per feature file : 12.67
    Average LoC in Scenario per feature file : 7.67
    Average LoC per feature file(having tables) : 17.0
    Average LoC in Scenario per feature file(having tables) : 12.0
    
    >> For repo mzheravin/exchange-core : 
    Average LoC per feature file : 89.0
    Average LoC in Scenario per feature file : 82.5
    Average LoC per feature file(having tables) : 89.0
    Average LoC in Scenario per feature file(having tables) : 82.5
    
    >> For repo iriusrisk/bdd-security : 
    Average LoC per feature file : 42.09
    Average LoC in Scenario per feature file : 37.18
    Average LoC per feature file(having tables) : 23.4
    Average LoC in Scenario per feature file(having tables) : 18.8
    
    >> For repo jbangdev/jbang : 
    Average LoC per feature file : 14.83
    Average LoC in Scenario per feature file : 13.22
    
    
    >> For repo SoftInstigate/restheart : 
    Average LoC per feature file : 101.87
    Average LoC in Scenario per feature file : 90.16
    
    
    >> For repo intuit/karate : 
    Average LoC per feature file : 19.02
    Average LoC in Scenario per feature file : 16.04
    Average LoC per feature file(having tables) : 45.47
    Average LoC in Scenario per feature file(having tables) : 41.34
    
    >> For repo cucumber/common : 
    Average LoC per feature file : 39.78
    Average LoC in Scenario per feature file : 36.6
    Average LoC per feature file(having tables) : 80.55
    Average LoC in Scenario per feature file(having tables) : 78.72
    
    >> For repo cucumber/cucumber-jvm : 
    Average LoC per feature file : 11.48
    Average LoC in Scenario per feature file : 9.06
    Average LoC per feature file(having tables) : 21.38
    Average LoC in Scenario per feature file(having tables) : 18.88
    
    >> For repo JetBrains/intellij-plugins : 
    Average LoC per feature file : 8.51
    Average LoC in Scenario per feature file : 7.07
    Average LoC per feature file(having tables) : 15.17
    Average LoC in Scenario per feature file(having tables) : 14.06
    
    In general for the feature files:
    Average LoC per feature file : 38.53
    Average LoC in Scenario per feature file : 35.53
    Average LoC per feature file(having tables): 73.41
    Average LoC in Scenario per feature file (having tables): 71.11
    

## Scenario and Scenario_Outline in Feat files


```python
scen_cnt_total = 0
scen_out_cnt_total = 0
scen_cnt_table_total = 0
scen_out_cnt_table_total = 0
total_files = 0
files_with_table = 0

scen_cnt_w_tables = []
scen_cnt_wo_tables = []
scen_out_cnt_w_tables = []
scen_out_cnt_wo_tables = []


for repo in repo_stats['repos'].keys():
    scen_cnt, scen_out_cnt, scen_cnt_table, scen_out_cnt_table = 0, 0, 0, 0
    total_files += repo_stats['repos'][repo]['files']
    files_with_table += repo_stats['repos'][repo]['files_with_tables']

    for file in repo_stats['repos'][repo]['file_stats'].keys():
        scen_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario:']
        scen_out_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario outline:']
        
        if repo_stats['repos'][repo]['file_stats'][file]['n_tables'] > 0:
            scen_cnt_table += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario:']
            scen_out_cnt_table += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario outline:']
            scen_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario:'])
            scen_out_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario outline:'])
        else:
            scen_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario:'])
            scen_out_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario outline:'])
    
    scen_cnt_total += scen_cnt
    scen_out_cnt_total += scen_out_cnt
    scen_cnt_table_total += scen_cnt_table
    scen_out_cnt_table_total += scen_out_cnt_table
    
    print(f">> For repo {repo} : ")
    print(f"Average no.of Scenario per feature file : {round(scen_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"Average no.of Scenario_outline per feature file : {round(scen_out_cnt/repo_stats['repos'][repo]['files'],2)}")
    if repo_stats['repos'][repo]['files_with_tables'] > 0:
        print(f"Average no.of Scenario per feature file(having tables) : {round(scen_cnt_table/repo_stats['repos'][repo]['files_with_tables'],2)}")
        print(f"Average no.of Scenario_outline per feature file(having tables) : {round(scen_out_cnt_table/repo_stats['repos'][repo]['files_with_tables'],2)}\n")
    else:
        print(f"\n")
    
print(f"In general for the feature files:")
print(f"Average no.of Scenario per feature file : {round(scen_cnt_total/total_files,2)}")
print(f"Average no.of Scenario_outline per feature file : {round(scen_out_cnt_total/total_files,2)}")
print(f"Average no.of Scenario per feature file(having tables): {round(scen_cnt_table_total/files_with_table,2)}")
print(f"Average no.of Scenario_outline per feature file (having tables): {round(scen_out_cnt_table_total/files_with_table,2)}")
```

    >> For repo eugenp/tutorials : 
    Average no.of Scenario per feature file : 2.33
    Average no.of Scenario_outline per feature file : 0.07
    Average no.of Scenario per feature file(having tables) : 2.6
    Average no.of Scenario_outline per feature file(having tables) : 0.2
    
    >> For repo neo4j/neo4j : 
    Average no.of Scenario per feature file : 12.0
    Average no.of Scenario_outline per feature file : 0.22
    Average no.of Scenario per feature file(having tables) : 12.0
    Average no.of Scenario_outline per feature file(having tables) : 0.22
    
    >> For repo geoserver/geoserver : 
    Average no.of Scenario per feature file : 4.25
    Average no.of Scenario_outline per feature file : 0.0
    
    
    >> For repo apache/servicecomb-pack : 
    Average no.of Scenario per feature file : 1.17
    Average no.of Scenario_outline per feature file : 0.0
    Average no.of Scenario per feature file(having tables) : 1.17
    Average no.of Scenario_outline per feature file(having tables) : 0.0
    
    >> For repo microservices-patterns/ftgo-application : 
    Average no.of Scenario per feature file : 1.5
    Average no.of Scenario_outline per feature file : 0.0
    
    
    >> For repo apache/tinkerpop : 
    Average no.of Scenario per feature file : 11.95
    Average no.of Scenario_outline per feature file : 0.0
    Average no.of Scenario per feature file(having tables) : 12.43
    Average no.of Scenario_outline per feature file(having tables) : 0.0
    
    >> For repo iotaledger/iri : 
    Average no.of Scenario per feature file : 4.8
    Average no.of Scenario_outline per feature file : 0.0
    Average no.of Scenario per feature file(having tables) : 7.5
    Average no.of Scenario_outline per feature file(having tables) : 0.0
    
    >> For repo SmartBear/soapui : 
    Average no.of Scenario per feature file : 4.39
    Average no.of Scenario_outline per feature file : 0.45
    Average no.of Scenario per feature file(having tables) : 1.0
    Average no.of Scenario_outline per feature file(having tables) : 1.67
    
    >> For repo w3c/epubcheck : 
    Average no.of Scenario per feature file : 21.22
    Average no.of Scenario_outline per feature file : 0.0
    Average no.of Scenario per feature file(having tables) : 32.0
    Average no.of Scenario_outline per feature file(having tables) : 0.0
    
    >> For repo aws/aws-sdk-java-v2 : 
    Average no.of Scenario per feature file : 2.0
    Average no.of Scenario_outline per feature file : 0.0
    Average no.of Scenario per feature file(having tables) : 2.0
    Average no.of Scenario_outline per feature file(having tables) : 0.0
    
    >> For repo bugsnag/bugsnag-android : 
    Average no.of Scenario per feature file : 2.49
    Average no.of Scenario_outline per feature file : 0.0
    Average no.of Scenario per feature file(having tables) : 4.0
    Average no.of Scenario_outline per feature file(having tables) : 0.0
    
    >> For repo blox/blox : 
    Average no.of Scenario per feature file : 2.9
    Average no.of Scenario_outline per feature file : 0.0
    Average no.of Scenario per feature file(having tables) : 3.0
    Average no.of Scenario_outline per feature file(having tables) : 0.0
    
    >> For repo ddd-by-examples/factory : 
    Average no.of Scenario per feature file : 4.5
    Average no.of Scenario_outline per feature file : 0.0
    Average no.of Scenario per feature file(having tables) : 4.5
    Average no.of Scenario_outline per feature file(having tables) : 0.0
    
    >> For repo FluentLenium/FluentLenium : 
    Average no.of Scenario per feature file : 1.91
    Average no.of Scenario_outline per feature file : 0.0
    
    
    >> For repo AppiumTestDistribution/AppiumTestDistribution : 
    Average no.of Scenario per feature file : 1.33
    Average no.of Scenario_outline per feature file : 0.33
    Average no.of Scenario per feature file(having tables) : 1.0
    Average no.of Scenario_outline per feature file(having tables) : 1.0
    
    >> For repo mzheravin/exchange-core : 
    Average no.of Scenario per feature file : 1.5
    Average no.of Scenario_outline per feature file : 0.5
    Average no.of Scenario per feature file(having tables) : 1.5
    Average no.of Scenario_outline per feature file(having tables) : 0.5
    
    >> For repo iriusrisk/bdd-security : 
    Average no.of Scenario per feature file : 3.82
    Average no.of Scenario_outline per feature file : 0.64
    Average no.of Scenario per feature file(having tables) : 1.2
    Average no.of Scenario_outline per feature file(having tables) : 1.0
    
    >> For repo jbangdev/jbang : 
    Average no.of Scenario per feature file : 3.17
    Average no.of Scenario_outline per feature file : 0.0
    
    
    >> For repo SoftInstigate/restheart : 
    Average no.of Scenario per feature file : 6.03
    Average no.of Scenario_outline per feature file : 0.0
    
    
    >> For repo intuit/karate : 
    Average no.of Scenario per feature file : 1.92
    Average no.of Scenario_outline per feature file : 0.19
    Average no.of Scenario per feature file(having tables) : 2.87
    Average no.of Scenario_outline per feature file(having tables) : 1.18
    
    >> For repo cucumber/common : 
    Average no.of Scenario per feature file : 3.11
    Average no.of Scenario_outline per feature file : 0.43
    Average no.of Scenario per feature file(having tables) : 5.92
    Average no.of Scenario_outline per feature file(having tables) : 0.94
    
    >> For repo cucumber/cucumber-jvm : 
    Average no.of Scenario per feature file : 1.49
    Average no.of Scenario_outline per feature file : 0.27
    Average no.of Scenario per feature file(having tables) : 2.16
    Average no.of Scenario_outline per feature file(having tables) : 0.78
    
    >> For repo JetBrains/intellij-plugins : 
    Average no.of Scenario per feature file : 1.39
    Average no.of Scenario_outline per feature file : 0.29
    Average no.of Scenario per feature file(having tables) : 2.43
    Average no.of Scenario_outline per feature file(having tables) : 0.74
    
    In general for the feature files:
    Average no.of Scenario per feature file : 3.47
    Average no.of Scenario_outline per feature file : 0.26
    Average no.of Scenario per feature file(having tables): 5.85
    Average no.of Scenario_outline per feature file (having tables): 0.65
    

## Keyword Analysis in Feat files


```python
and_cnt_total, then_cnt_total, when_cnt_total, feature_cnt_total = 0, 0, 0, 0
given_cnt_total, at_cnt_total, background_cnt_total, pipe_cnt_total, examples_cnt_total = 0, 0, 0, 0, 0
total_files = 0

given_cnt_w_tables = []
when_cnt_w_tables = []
then_cnt_w_tables = []
and_cnt_w_tables = []
at_cnt_w_tables = []
feature_cnt_w_tables = []
background_cnt_w_tables = []
example_cnt_w_tables = []

given_cnt_wo_tables = []
when_cnt_wo_tables = []
then_cnt_wo_tables = []
and_cnt_wo_tables = []
at_cnt_wo_tables = []
feature_cnt_wo_tables = []
background_cnt_wo_tables = []
example_cnt_wo_tables = []

for repo in repo_stats['repos'].keys():
    and_cnt, then_cnt, when_cnt, feature_cnt, given_cnt, at_cnt, background_cnt, pipe_cnt, examples_cnt = 0, 0, 0, 0, 0, 0, 0, 0, 0
    total_files += repo_stats['repos'][repo]['files']

    for file in repo_stats['repos'][repo]['file_stats'].keys():
        and_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['and']
        then_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['then']
        when_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['when']
        feature_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['feature:']
        given_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['given']
        at_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['@']
        background_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['background:']
        pipe_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['|']
        examples_cnt += repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['examples:']
        
        if repo_stats['repos'][repo]['file_stats'][file]['n_tables'] > 0:
            given_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['given'])
            when_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['when'])
            then_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['then'])
            and_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['and'])
            at_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['@'])
            feature_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['feature:'])
            background_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['background:'])
            example_cnt_w_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['examples:'])
        else:
            given_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['given'])
            when_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['when'])
            then_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['then'])
            and_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['and'])
            at_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['@'])
            feature_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['feature:'])
            background_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['background:'])
            example_cnt_wo_tables.append(repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['examples:'])
    
    and_cnt_total += and_cnt
    then_cnt_total += then_cnt
    when_cnt_total += when_cnt
    feature_cnt_total += feature_cnt
    given_cnt_total += given_cnt
    at_cnt_total += at_cnt
    background_cnt_total += background_cnt
    pipe_cnt_total += pipe_cnt
    examples_cnt_total += examples_cnt
    
    print(f">> For repo {repo} : ")
    print(f"Average no.of Given per feature file : {round(given_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"Average no.of When per feature file : {round(when_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"Average no.of Then per feature file : {round(then_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"Average no.of And per feature file : {round(and_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"Average no.of Feature per feature file : {round(feature_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"Average no.of @ per feature file : {round(at_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"Average no.of Background per feature file : {round(background_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"Average no.of Examples per feature file : {round(examples_cnt/repo_stats['repos'][repo]['files'],2)}")
    print(f"\n")
    
print(f"In general for the feature files:")
print(f"Average no.of Given per feature file : {round(given_cnt_total/total_files,2)}")
print(f"Average no.of When per feature file : {round(when_cnt_total/total_files,2)}")
print(f"Average no.of Then per feature file : {round(then_cnt_total/total_files,2)}")
print(f"Average no.of And per feature file : {round(and_cnt_total/total_files,2)}")
print(f"Average no.of Feature per feature file : {round(feature_cnt_total/total_files,2)}")
print(f"Average no.of @ per feature file : {round(at_cnt_total/total_files,2)}")
print(f"Average no.of Background per feature file : {round(background_cnt_total/total_files,2)}")
print(f"Average no.of Examples per feature file : {round(examples_cnt_total/total_files,2)}")
```

    >> For repo eugenp/tutorials : 
    Average no.of Given per feature file : 1.8
    Average no.of When per feature file : 2.4
    Average no.of Then per feature file : 2.4
    Average no.of And per feature file : 0.8
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.2
    Average no.of Background per feature file : 0.13
    Average no.of Examples per feature file : 0.07
    
    
    >> For repo neo4j/neo4j : 
    Average no.of Given per feature file : 7.67
    Average no.of When per feature file : 12.89
    Average no.of Then per feature file : 12.3
    Average no.of And per feature file : 22.59
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.33
    Average no.of Background per feature file : 0.33
    Average no.of Examples per feature file : 0.22
    
    
    >> For repo geoserver/geoserver : 
    Average no.of Given per feature file : 4.25
    Average no.of When per feature file : 4.25
    Average no.of Then per feature file : 4.25
    Average no.of And per feature file : 16.25
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 6.5
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo apache/servicecomb-pack : 
    Average no.of Given per feature file : 1.63
    Average no.of When per feature file : 1.17
    Average no.of Then per feature file : 2.57
    Average no.of And per feature file : 4.47
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.0
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo microservices-patterns/ftgo-application : 
    Average no.of Given per feature file : 4.5
    Average no.of When per feature file : 1.5
    Average no.of Then per feature file : 2.5
    Average no.of And per feature file : 2.0
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.0
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo apache/tinkerpop : 
    Average no.of Given per feature file : 11.95
    Average no.of When per feature file : 11.64
    Average no.of Then per feature file : 11.93
    Average no.of And per feature file : 19.47
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.0
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo iotaledger/iri : 
    Average no.of Given per feature file : 4.8
    Average no.of When per feature file : 3.4
    Average no.of Then per feature file : 6.6
    Average no.of And per feature file : 6.4
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.0
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo SmartBear/soapui : 
    Average no.of Given per feature file : 3.1
    Average no.of When per feature file : 2.97
    Average no.of Then per feature file : 3.16
    Average no.of And per feature file : 12.03
    Average no.of Feature per feature file : 0.94
    Average no.of @ per feature file : 1.0
    Average no.of Background per feature file : 0.03
    Average no.of Examples per feature file : 0.45
    
    
    >> For repo w3c/epubcheck : 
    Average no.of Given per feature file : 1.89
    Average no.of When per feature file : 21.22
    Average no.of Then per feature file : 21.75
    Average no.of And per feature file : 23.69
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.0
    Average no.of Background per feature file : 1.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo aws/aws-sdk-java-v2 : 
    Average no.of Given per feature file : 0.0
    Average no.of When per feature file : 2.0
    Average no.of Then per feature file : 2.0
    Average no.of And per feature file : 0.23
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 1.0
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo bugsnag/bugsnag-android : 
    Average no.of Given per feature file : 0.0
    Average no.of When per feature file : 2.55
    Average no.of Then per feature file : 2.29
    Average no.of And per feature file : 28.94
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.33
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo blox/blox : 
    Average no.of Given per feature file : 2.4
    Average no.of When per feature file : 3.0
    Average no.of Then per feature file : 3.0
    Average no.of And per feature file : 3.2
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 2.2
    Average no.of Background per feature file : 0.3
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo ddd-by-examples/factory : 
    Average no.of Given per feature file : 7.0
    Average no.of When per feature file : 6.0
    Average no.of Then per feature file : 16.5
    Average no.of And per feature file : 0.5
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.0
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo FluentLenium/FluentLenium : 
    Average no.of Given per feature file : 1.55
    Average no.of When per feature file : 1.91
    Average no.of Then per feature file : 1.91
    Average no.of And per feature file : 0.36
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.18
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo AppiumTestDistribution/AppiumTestDistribution : 
    Average no.of Given per feature file : 1.0
    Average no.of When per feature file : 1.67
    Average no.of Then per feature file : 1.33
    Average no.of And per feature file : 0.67
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 1.0
    Average no.of Background per feature file : 0.0
    Average no.of Examples per feature file : 0.33
    
    
    >> For repo mzheravin/exchange-core : 
    Average no.of Given per feature file : 3.5
    Average no.of When per feature file : 10.5
    Average no.of Then per feature file : 10.5
    Average no.of And per feature file : 18.5
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 2.0
    Average no.of Background per feature file : 0.5
    Average no.of Examples per feature file : 0.5
    
    
    >> For repo iriusrisk/bdd-security : 
    Average no.of Given per feature file : 2.09
    Average no.of When per feature file : 3.64
    Average no.of Then per feature file : 4.73
    Average no.of And per feature file : 13.36
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 5.27
    Average no.of Background per feature file : 0.27
    Average no.of Examples per feature file : 0.64
    
    
    >> For repo jbangdev/jbang : 
    Average no.of Given per feature file : 0.0
    Average no.of When per feature file : 2.94
    Average no.of Then per feature file : 3.0
    Average no.of And per feature file : 0.28
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.06
    Average no.of Background per feature file : 0.28
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo SoftInstigate/restheart : 
    Average no.of Given per feature file : 12.1
    Average no.of When per feature file : 11.61
    Average no.of Then per feature file : 11.42
    Average no.of And per feature file : 26.13
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 1.32
    Average no.of Background per feature file : 1.0
    Average no.of Examples per feature file : 0.0
    
    
    >> For repo intuit/karate : 
    Average no.of Given per feature file : 0.81
    Average no.of When per feature file : 0.88
    Average no.of Then per feature file : 1.0
    Average no.of And per feature file : 1.74
    Average no.of Feature per feature file : 1.0
    Average no.of @ per feature file : 0.44
    Average no.of Background per feature file : 0.44
    Average no.of Examples per feature file : 0.22
    
    
    >> For repo cucumber/common : 
    Average no.of Given per feature file : 3.54
    Average no.of When per feature file : 0.06
    Average no.of Then per feature file : 0.05
    Average no.of And per feature file : 8.71
    Average no.of Feature per feature file : 0.85
    Average no.of @ per feature file : 0.66
    Average no.of Background per feature file : 0.19
    Average no.of Examples per feature file : 0.48
    
    
    >> For repo cucumber/cucumber-jvm : 
    Average no.of Given per feature file : 1.62
    Average no.of When per feature file : 1.16
    Average no.of Then per feature file : 1.28
    Average no.of And per feature file : 0.3
    Average no.of Feature per feature file : 0.95
    Average no.of @ per feature file : 0.35
    Average no.of Background per feature file : 0.13
    Average no.of Examples per feature file : 0.39
    
    
    >> For repo JetBrains/intellij-plugins : 
    Average no.of Given per feature file : 2.1
    Average no.of When per feature file : 0.54
    Average no.of Then per feature file : 0.22
    Average no.of And per feature file : 0.44
    Average no.of Feature per feature file : 0.96
    Average no.of @ per feature file : 0.15
    Average no.of Background per feature file : 0.04
    Average no.of Examples per feature file : 0.3
    
    
    In general for the feature files:
    Average no.of Given per feature file : 2.78
    Average no.of When per feature file : 2.12
    Average no.of Then per feature file : 2.19
    Average no.of And per feature file : 7.26
    Average no.of Feature per feature file : 0.94
    Average no.of @ per feature file : 0.55
    Average no.of Background per feature file : 0.25
    Average no.of Examples per feature file : 0.29
    

## Unusual events analysis in Feat files


```python
no_given_cnt_total, no_then_cnt_total, no_when_cnt_total, no_scenario_cnt_total = 0, 0, 0, 0
no_given_cnt_table_total, no_then_cnt_table_total, no_when_cnt_table_total, no_scenario_cnt_table_total = 0, 0, 0, 0
total_files = 0
files_with_table = 0

for repo in repo_stats['repos'].keys():
    no_given_cnt, no_then_cnt, no_when_cnt, no_scenario_cnt = 0, 0, 0, 0
    no_given_cnt_table, no_then_cnt_table, no_when_cnt_table, no_scenario_cnt_table = 0, 0, 0, 0
    total_files += repo_stats['repos'][repo]['files']
    files_with_table += repo_stats['repos'][repo]['files_with_tables']

    for file in repo_stats['repos'][repo]['file_stats'].keys():
        if repo_stats['repos'][repo]['file_stats'][file]['n_tables'] > 0:
            if repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['given'] == 0:
                no_given_cnt_table += 1
        
            if repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['then'] == 0:
                no_then_cnt_table += 1

            if repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['when'] == 0:
                no_when_cnt_table += 1

            if repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario:'] == 0:
                no_scenario_cnt_table += 1
        else :
            if repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['given'] == 0:
                no_given_cnt += 1
        
            if repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['then'] == 0:
                no_then_cnt += 1

            if repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['when'] == 0:
                no_when_cnt += 1

            if repo_stats['repos'][repo]['file_stats'][file]['keyword_stats']['scenario:'] == 0:
                no_scenario_cnt += 1
        
    
    no_given_cnt_total += no_given_cnt
    no_then_cnt_total += no_then_cnt
    no_when_cnt_total += no_when_cnt
    no_scenario_cnt_total += no_scenario_cnt
    
    no_given_cnt_table_total += no_given_cnt_table
    no_then_cnt_table_total += no_then_cnt_table
    no_when_cnt_table_total += no_when_cnt_table
    no_scenario_cnt_table_total += no_scenario_cnt_table

    print(f">> For repo {repo} : ")
    print(f"No.of feature files without the keyword 'Given' : {no_given_cnt}")
    print(f"No.of feature files without the keyword 'Then' : {no_then_cnt}")
    print(f"No.of feature files without the keyword 'When' : {no_when_cnt}")
    print(f"No.of feature files without the keyword 'Scenario' : {no_scenario_cnt}")
    print(f"\n")
    print(f"No.of feature files (which have tables) without the keyword 'Given' : {no_given_cnt_table}")
    print(f"No.of feature files (which have tables) without the keyword 'Then' : {no_then_cnt_table}")
    print(f"No.of feature files (which have tables) without the keyword 'When' : {no_when_cnt_table}")
    print(f"No.of feature files (which have tables) without the keyword 'Scenario' : {no_scenario_cnt_table}")
    print(f"\n")
    
    
print(f"In general for the feature files:")
print(f"No.of feature files without the keyword 'Given' : {no_given_cnt_total}")
print(f"No.of feature files without the keyword 'Then' : {no_then_cnt_total}")
print(f"No.of feature files without the keyword 'When' : {no_when_cnt_total}")
print(f"No.of feature files without the keyword 'Scenario' : {no_scenario_cnt_total}")
print(f"\n")
print(f"No.of feature files (which have tables) without the keyword 'Given' : {no_given_cnt_table_total}")
print(f"No.of feature files (which have tables) without the keyword 'Then' : {no_then_cnt_table_total}")
print(f"No.of feature files (which have tables) without the keyword 'When' : {no_when_cnt_table_total}")
print(f"No.of feature files (which have tables) without the keyword 'Scenario' : {no_scenario_cnt_table_total}")
```

    >> For repo eugenp/tutorials : 
    No.of feature files without the keyword 'Given' : 3
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 1
    
    
    >> For repo neo4j/neo4j : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo geoserver/geoserver : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo apache/servicecomb-pack : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo microservices-patterns/ftgo-application : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo apache/tinkerpop : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 1
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo iotaledger/iri : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 1
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo SmartBear/soapui : 
    No.of feature files without the keyword 'Given' : 7
    No.of feature files without the keyword 'Then' : 7
    No.of feature files without the keyword 'When' : 8
    No.of feature files without the keyword 'Scenario' : 2
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 2
    
    
    >> For repo w3c/epubcheck : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo aws/aws-sdk-java-v2 : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 53
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo bugsnag/bugsnag-android : 
    No.of feature files without the keyword 'Given' : 43
    No.of feature files without the keyword 'Then' : 7
    No.of feature files without the keyword 'When' : 1
    No.of feature files without the keyword 'Scenario' : 1
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 6
    No.of feature files (which have tables) without the keyword 'Then' : 2
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo blox/blox : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo ddd-by-examples/factory : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo FluentLenium/FluentLenium : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo AppiumTestDistribution/AppiumTestDistribution : 
    No.of feature files without the keyword 'Given' : 1
    No.of feature files without the keyword 'Then' : 1
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo mzheravin/exchange-core : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo iriusrisk/bdd-security : 
    No.of feature files without the keyword 'Given' : 1
    No.of feature files without the keyword 'Then' : 0
    No.of feature files without the keyword 'When' : 1
    No.of feature files without the keyword 'Scenario' : 1
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 3
    
    
    >> For repo jbangdev/jbang : 
    No.of feature files without the keyword 'Given' : 18
    No.of feature files without the keyword 'Then' : 7
    No.of feature files without the keyword 'When' : 2
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo SoftInstigate/restheart : 
    No.of feature files without the keyword 'Given' : 0
    No.of feature files without the keyword 'Then' : 2
    No.of feature files without the keyword 'When' : 0
    No.of feature files without the keyword 'Scenario' : 0
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 0
    No.of feature files (which have tables) without the keyword 'Then' : 0
    No.of feature files (which have tables) without the keyword 'When' : 0
    No.of feature files (which have tables) without the keyword 'Scenario' : 0
    
    
    >> For repo intuit/karate : 
    No.of feature files without the keyword 'Given' : 230
    No.of feature files without the keyword 'Then' : 227
    No.of feature files without the keyword 'When' : 233
    No.of feature files without the keyword 'Scenario' : 1
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 54
    No.of feature files (which have tables) without the keyword 'Then' : 53
    No.of feature files (which have tables) without the keyword 'When' : 52
    No.of feature files (which have tables) without the keyword 'Scenario' : 37
    
    
    >> For repo cucumber/common : 
    No.of feature files without the keyword 'Given' : 157
    No.of feature files without the keyword 'Then' : 312
    No.of feature files without the keyword 'When' : 309
    No.of feature files without the keyword 'Scenario' : 132
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 15
    No.of feature files (which have tables) without the keyword 'Then' : 229
    No.of feature files (which have tables) without the keyword 'When' : 240
    No.of feature files (which have tables) without the keyword 'Scenario' : 110
    
    
    >> For repo cucumber/cucumber-jvm : 
    No.of feature files without the keyword 'Given' : 19
    No.of feature files without the keyword 'Then' : 20
    No.of feature files without the keyword 'When' : 25
    No.of feature files without the keyword 'Scenario' : 11
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 3
    No.of feature files (which have tables) without the keyword 'Then' : 5
    No.of feature files (which have tables) without the keyword 'When' : 9
    No.of feature files (which have tables) without the keyword 'Scenario' : 5
    
    
    >> For repo JetBrains/intellij-plugins : 
    No.of feature files without the keyword 'Given' : 29
    No.of feature files without the keyword 'Then' : 60
    No.of feature files without the keyword 'When' : 47
    No.of feature files without the keyword 'Scenario' : 12
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 4
    No.of feature files (which have tables) without the keyword 'Then' : 24
    No.of feature files (which have tables) without the keyword 'When' : 26
    No.of feature files (which have tables) without the keyword 'Scenario' : 15
    
    
    In general for the feature files:
    No.of feature files without the keyword 'Given' : 508
    No.of feature files without the keyword 'Then' : 643
    No.of feature files without the keyword 'When' : 628
    No.of feature files without the keyword 'Scenario' : 160
    
    
    No.of feature files (which have tables) without the keyword 'Given' : 135
    No.of feature files (which have tables) without the keyword 'Then' : 313
    No.of feature files (which have tables) without the keyword 'When' : 327
    No.of feature files (which have tables) without the keyword 'Scenario' : 173
    

## Statistical tests on feature files with and without tables

Here we apply Mann-Whitney test to understand the distribution of feature files with and without tables for different metrics

We have two distributions: x and y
x = Metrics for feature files with tables
y = Metrics for feature files without tables

We perform two tests:

1) To check if the two distributions are similar:<br>
    Null Hypothesis : x = y<br>
    Alternate Hypothesis : x != y<br>
    
2) If the null hypothesis is rejected, i.e. x != y, then we calculate the cliff's delta which computes the effective size of the difference.


```python
dist_w_table = [loc_files_w_tables, scen_cnt_w_tables, scen_out_cnt_w_tables, given_cnt_w_tables, 
                when_cnt_w_tables, then_cnt_w_tables, and_cnt_w_tables, at_cnt_w_tables, feature_cnt_w_tables, 
                background_cnt_w_tables, example_cnt_w_tables]

dist_wo_table = [loc_files_wo_tables, scen_cnt_wo_tables, scen_out_cnt_wo_tables, given_cnt_wo_tables, 
                 when_cnt_wo_tables, then_cnt_wo_tables, and_cnt_wo_tables, at_cnt_wo_tables, feature_cnt_wo_tables, 
                 background_cnt_wo_tables, example_cnt_wo_tables]

test_names = ["Lines of Code in Feature files", "Scenario count in Feature files", "Scenario Outline count in Feature files", 
              "Keyword 'Given' count in Feature files", "Keyword 'When' count in Feature files", 
              "Keyword 'Then' count in Feature files", "Keyword 'and' count in Feature files", 
              "Keyword '@' count in Feature files", "Keyword 'Feature' count in Feature files", 
              "Keyword 'Background' count in Feature files", "Keyword 'Examples' count in Feature files"]

for cnt in range(len(test_names)):
    x = dist_w_table[cnt]
    y = dist_wo_table[cnt]

    U1, p = stats.mannwhitneyu(x, y, alternative='two-sided')
    
    print("Test results on",test_names[cnt])
    print("\nU-statstic : ", U1)
    print("p-value : ", p)

    if p < 0.05:
        print("\nNull hypothesis rejected. Two distributions are different\n")
        d, res = cliffs_delta(x, y)
        print("Effective size of difference is", d, ", which is", res)
        print("------------------------------------------")
    else:
        print("\nNull hypothesis accepted. Two distributions are same\n")
        print("------------------------------------------")
```

    Test results on Lines of Code in Feature files
    
    U-statstic :  427753.0
    p-value :  5.726712241422872e-57
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.4780939546294857 , which is large
    ------------------------------------------
    Test results on Scenario count in Feature files
    
    U-statstic :  306273.0
    p-value :  0.04155563038134368
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.05832167107240968 , which is negligible
    ------------------------------------------
    Test results on Scenario Outline count in Feature files
    
    U-statstic :  416816.0
    p-value :  1.3571192261596275e-103
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.44030131826741997 , which is medium
    ------------------------------------------
    Test results on Keyword 'Given' count in Feature files
    
    U-statstic :  388831.0
    p-value :  4.8637649573598876e-33
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.3435995784308644 , which is medium
    ------------------------------------------
    Test results on Keyword 'When' count in Feature files
    
    U-statstic :  320212.5
    p-value :  5.66456327718559e-05
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.10648940030062717 , which is negligible
    ------------------------------------------
    Test results on Keyword 'Then' count in Feature files
    
    U-statstic :  330898.0
    p-value :  5.834999773561153e-08
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.14341298225608598 , which is negligible
    ------------------------------------------
    Test results on Keyword 'and' count in Feature files
    
    U-statstic :  315600.0
    p-value :  0.0002938391205192448
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.09055097703830405 , which is negligible
    ------------------------------------------
    Test results on Keyword '@' count in Feature files
    
    U-statstic :  304969.0
    p-value :  0.01884966302168078
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.05381571899998272 , which is negligible
    ------------------------------------------
    Test results on Keyword 'Feature' count in Feature files
    
    U-statstic :  316044.5
    p-value :  7.033901523875183e-14
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.09208693999550786 , which is negligible
    ------------------------------------------
    Test results on Keyword 'Background' count in Feature files
    
    U-statstic :  263536.0
    p-value :  5.817004182581627e-05
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is -0.08935537932583493 , which is negligible
    ------------------------------------------
    Test results on Keyword 'Examples' count in Feature files
    
    U-statstic :  439738.0
    p-value :  2.5699756044373745e-135
    
    Null hypothesis rejected. Two distributions are different
    
    Effective size of difference is 0.5195079389761399 , which is large
    ------------------------------------------
    

## Visualizing the results

In this section, we present the box plots for different metrics for which Mann-Whitney tests were applied. <br>
**Note** : Box plots are drawn only for the metrics for whom, the difference between feature files with and without tables is 'medium' or 'large'.


```python
for cnt in range(len(test_names)):
    
    x = dist_w_table[cnt]
    y = dist_wo_table[cnt]
    
    d, res = cliffs_delta(x, y)
    
    '''if res in ["large", "medium"]: '''
        
    plot_dict = {"With tables" : x, "Without tables" : y}
        
    plt.rcParams["font.family"] = "Times New Roman"
        
    fig, ax = plt.subplots()
    ax.boxplot(plot_dict.values(), showmeans=True, showfliers=False)
    ax.set_title(test_names[cnt])
    ax.set_xticklabels(plot_dict.keys())
        
    plt.savefig(f"{test_names[cnt]}.svg", bbox_inches='tight')
```


    
![png](Parsing_feat_files_files/Parsing_feat_files_27_0.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_1.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_2.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_3.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_4.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_5.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_6.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_7.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_8.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_9.png)
    



    
![png](Parsing_feat_files_files/Parsing_feat_files_27_10.png)
    



```python

```
