name: generate animated snake

on:
  schedule:
    - cron: "0 3 * * *"   # runs once a day at 3 AM UTC
  push:
    branches:
      - main              # change to "master" if that's your default branch
  workflow_dispatch: {}    # lets you trigger it manually from the Actions tab

jobs:
  generate:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    steps:
      - name: generate snake svg
        uses: Platane/snk@v3
        id: snake-gif
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
            dist/github-contribution-grid-snake.svg

      - name: push svg to the output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
