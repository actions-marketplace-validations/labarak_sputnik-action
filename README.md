# Sputnik  

<img src="https://github.com/labarak/sputnik/raw/master/app-icon.png" width="100" height="100" />

This action will allow you not to spend time configuring bitrise with github.

## Installation

Setup the SSH key from bitrise to github as explained [here.](https://blog.bitrise.io/connecting-github-to-bitrise)

#### Parameters 

[mandatory] `bitrise_app_slug`: The app you want the workflow to be triggered upon

[mandatory] `bitrise_build_trigger_token`: Coming from your bitrise code trigger app 

[mandatory] `bitrise_workflow`: The bitrise workflow you want the github action to trigger.

[optional] `trigger_on`: When your workflow will be triggered. By default it will be triggered on every event but you can choose to trigger it manually using the `command` parameter. 

[optional] `command_alias`: The name of your workflow which you'll need in order to trigger it from a PR comment.  

The flexible and modular approach of the action allows you to define multiple workflow pointing to different app.

__Workflow Example__

```yaml
- uses: actions/sputnik@v1
  name: build PR for testing 
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    bitrise_app_slug: ca04e0425716e1ce
    bitrise_build_trigger_token: Gs7vZtsWEz-UZ_vNdNxDIA
    bitrise_workflow: primary
    command_alias: primaryBuild 
    trigger_on: manual
- uses: actions/sputnik@v1
  name: biild end to end tests on IOS
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    bitrise_app_slug: ca04e0425716e1ce
    bitrise_build_trigger_token: Gs7vZtsWEz-UZ_vNdNxDIA
    bitrise_workflow: buildend2endIOS 
- uses: actions/sputnik@v1
  name: Build end to end tests on Android 
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    bitrise_workflow: secondary
    bitrise_app_slug:  ewpfih2hf00fwe0
    bitrise_build_trigger_token: Gs7vZtsWEz-UZ_vNdNxDIA
    command_alias: buildend2endAndroid
```

 
## Usage 

This action subscribes to
- pull requests events in order to trigger a bitrise build when a new pull request is created
- push on master whenever a PR is merged to the master branch in order to run a build from the master branch
- issue_comment to trigger manually a build by calling the github-action in a PR comment.  

```
on:
  push:
    branches: [master]
  pull_request:
    types: [synchronize, opened, reopened, ready_for_review]
  issue_comment:
    types: [created]
```



#### How to manually trigger a build

You need to create a new comment on a PR and type the `command_alias` parameter declared in your workflow

```
/workflowAlias 
```
 

