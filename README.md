# ras-rm-spinnaker-action

> A GitHub Action for publishing PRs to specific ras-rm environments. Trigger the action by commenting on a pr
```
/deploy <namespace> key=value
```

This will deploy a pr into a specified namespace in the dev cluster. `key=value` allows you to override a specific helm chart config for a deployment, its optional and is most useful for testing feature flags.

deploy a PR using a secific config branch:

```
/deploy <namespace> configBranch=test-branch
```

## Usage

```yaml
on:
  issue_comment:
    types: [created]

env:
  SERVICE: case

jobs:
  comment:
    runs-on: ubuntu-latest
    if: contains(github.event.comment.body, '/deploy')
    steps:
      - uses: onsdigital/ras-rm-spinnaker-action@main
        with:
          comment-body: ${{ github.event.comment.body }}
          gcp-project: ${{ secrets.GOOGLE_PROJECT_ID }}
          bot-token: ${{ secrets.BOT_TOKEN }}
          spinnaker-topic: ${{ secrets.SPINNAKER_TOPIC }}
          artifact-bucket: ${{ secrets.ARTIFACT_BUCKET }}
          helm-chart: ${{ env.SERVICE }}/${{ env.SERVICE }}-latest.tgz
```

## Configuration options

| Argument  | Location | Description                                                                                                                 | Required | 
| --------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------- | -------- |
| comment-body         | with     | mist be of the form `/deploy <namespace> key=value`                                        | yes      |
| bot-token            | with     | Github token | yes    |
| gcp-project          | with     | gcp project containing the pubsub topic that spinnaker listens to | yes       |
| spinnaker-topic      | with     | spinnaker pubsub topic          | yes       |
| artifact-bucket      | with     | Bucket that stores helm charts          | yes       |
| helm-chart:          | with     | path to helm chart in bucket     | yes    |
