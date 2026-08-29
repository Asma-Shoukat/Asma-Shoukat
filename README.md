name: Generate Snake Animation

on:
  schedule:
    - cron: "0 0 * * *"   # runs once a day at midnight UTC
  workflow_dispatch: {}    # lets you trigger it manually from the Actions tab
  push:
    branches:
      - main

jobs:
  generate:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - name: Generate the snake animation
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-snake.svg?color_snake=FF00E5&color_dots=0D0D0D,3D0E63,8A2BE2,00E5FF,FF00E5
            dist/github-snake-dark.svg?color_snake=FF00E5&color_dots=0D0D0D,3D0E63,8A2BE2,00E5FF,FF00E5

      - name: Push the generated files to the output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
