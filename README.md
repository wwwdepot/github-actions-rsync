# Rsync Deployments

Rsync files from a GitHub repo to a destination server over SSH

## Environment variables

| Variable           | Description                                                                                                                      |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------|
| `SSH_PRIVATE_KEY`  | The private key part of an SSH key pair. The public key part should be added to the `authorized_keys` on the destination server. |
| `SSH_USERNAME`     | The username to use when connecting to the destination server                                                                    |
| `SSH_HOSTNAME`     | The hostname of the destination server                                                                                           |
| `SSH_PORT`         | The SSH port of the destination server, default `22`                                                                             |


## Required arguments

| Argument           | Description                                                                                                                                          |
|--------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| `RSYNC_OPTIONS`    | Rsync-specific options when running the command. Exclusions, deletions, etc                                                                          |
| `RSYNC_TARGET`     | Where to deploy the files on the server                                                                                                              |
| `RSYNC_SOURCE`     | What files to deploy from the repo (starts at root) **NOTE**: a trailing `/` deploys the _contents_ of the directory instead of the entire directory |

## Example usage

```yaml
name: Create Sandbox

on: pull_request

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v1

      - name: Deploy to sandbox via rsync
        uses: trendyminds/github-actions-rsync@master
        with:
          RSYNC_OPTIONS: -avzr --delete --exclude node_modules --exclude '.git*'
          RSYNC_TARGET: /path/to/target/folder/on/server
          RSYNC_SOURCE: /src/public/
        env:
          SSH_PRIVATE_KEY: ${{secrets.SSH_PRIVATE_KEY}}
          SSH_USERNAME: ${{secrets.SSH_USERNAME}}
          SSH_HOSTNAME: ${{secrets.SSH_HOSTNAME}}
```

## Disclaimer

Check your keys. Check your deployment paths. And use at your own risk!
