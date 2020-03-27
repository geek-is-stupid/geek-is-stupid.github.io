---
layout: post
title: "Generate Test Coverage by Feature (Folder) with Slather"
categories: ios
tags: swift objective-c objc ios Unit test asynchronous time-out expectation
fullview: true
---
Continue with Slather to generate test coverage reports for Xcode projects & hook it into CI, and from time to time you wonder how to generate the test for only your features which are contributed by you or your team.

## Get all test coverage percentage:

Collecting the test coverage percentage for all projects:

```
slather_cmd="slather coverage -s --scheme YourSchemeName --workspace YourWorkspaceName.xcworkspace YourProjectName.xcodeproj"
echo "Collecting test reports for all projects..."
eval $slather_cmd
```

## Get test coverage percentage base on a feature (or Folder)

Let’s imaging the project have a structure like this:

```
- YourProject:
  - YourProject:
    - Authentication
    - Home
    - Account
    - Feeds
    - Settings
  - YourProjectTests
  - YourProjectUITests
  - Pods
```

Every physic folders under the YourProject as each feature

To generate the test coverage you probably to ignore the irrelevant stuff likes **Pods** by **ignore_list** in Slather **ignore_list="--ignore \"Pods/*\""**

And the binaries should be only **YourProject**: **binaries="--binary-basename YourProject"**

```
#
# This file to facilitate getting test coverage percentage
#
# How to use: 
# 1. For all test cases: 
# bash test-coverage.sh
# 
# 2. For test cases belong to a feature: 
# bash test-coverage.sh {Feature_Folder_Name}
# Feature_Folder_Name: The name of the folder under YourProject.
# e.g Authentication, Home, Settings, ...

YELLOW=`tput setaf 3`
RESET=`tput sgr0`
BOLD=`tput bold`
GREEN=`tput setaf 2`
RED=`tput setaf 1`

echo -e "${YELLOW}Make sure you run test successfully on Xcode before run this script!${RESET}"
if [ "$1" ]; then
    feature=$1
    echo -e "Collecting test reports for ${BOLD}${GREEN}${feature}${RESET}..."    
    # Ignore Libraries
    ignore_list="--ignore \"Pods/*\""
    binaries="--binary-basename YourProject"  
    prefixPath="./YourProject/"
    for d in ./ShopBack/*; do
        folder=${d#$prefixPath}
        if [ "$feature" != "$folder" ] 
        then
            ignore_list=$ignore_list" -i \"YourProject/$folder/*\""
        fi
    done
    # Combine ignore list and binaries to final slather command to get slather report.
    slather_cmd="slather coverage -s "$binaries" "$ignore_list" --scheme YourSchemeName --workspace YourWorkspaceName.xcworkspace YourProjectName.xcodeproj"
    eval $slather_cmd
else 
    slather_cmd="slather coverage -s --scheme YourSchemeName --workspace YourWorkspaceName.xcworkspace YourProjectName.xcodeproj"
    echo "Collecting test reports for ${BOLD}${RED}all projects${RESET}..."
    eval $slather_cmd
fi
```

Save the script as a file called test-coverage.sh the same level as Pods, YourProjectTests, …

## Executing

**bash test-coverage.sh Home**

## Notes

Make sure you have done running the `CMD` + `U` successfully from your Xcode

## Ref

https://gist.github.com/levantAJ/84b4d81aecb10b444ae95b2aad13ad36


