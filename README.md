# gh_action_provoker
Git hub action provoker 


# Trigger Another Repository's Github Action Workflow
This repository should start the workflow of another repository on every new tag


# Instruction
* Link https://github.community/t/triggering-by-other-repository/16163
* create `secret.PAT_USERNAME` = `your_username`
* create `secret.PAT_TOKEN` = [`Personal access tokens`](https://github.com/settings/tokens)
* create `on_tag.yml` in the `Triggerer` repository:
```yml
on:
  push:
    tags:
      - '*'

jobs:
  build:
    name: Tag is pushed
    runs-on: ubuntu-latest
    steps:
      - name: Run Benchmark
        run: |
          echo "Run Benchmark"
          curl \
            -X POST \
            -u "${{ secrets.PAT_USERNAME }}:${{ secrets.PAT_TOKEN }}" \
            -H "Accept: application/vnd.github.everest-preview+json" \
            -H "Content-Type: application/json" https://api.github.com/repos/{{ YOUR_USERNAME }}/{{ REPOSITORY_RECEIVER }}/actions/workflows/{{ WORKFLOW_TO_BE_ACTIVATED.yaml }}/dispatches \
            --data '{"ref": "{{ BRANCH }}"}'
```
* create workflow in the `Receiver` repository:
```yml
on: workflow_dispatch

jobs:
  build:
    name: Trigger received
    runs-on: ubuntu-latest
    steps:
      - name: Trigger received
        run: |
          echo "Trigger received"
```
* Receiver repository example: https://github.com/almazkun/gh_actions_receiver