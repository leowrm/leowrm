name: Update README cards

on:
  schedule:
    - cron: "0 3 * * *"   # roda todo dia às 03:00
  workflow_dispatch:        # permite rodar manualmente pelo botão "Run workflow"
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4

      # Cards de projetos fixados
      - name: Generate pin card - compara-estruturas
        uses: stats-organization/github-readme-stats-action@v2
        with:
          card: pin
          options: username=leowrm&repo=compara-estruturas&theme=default
          path: profile/pin-compara-estruturas.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Generate pin card - verificador-fraude
        uses: stats-organization/github-readme-stats-action@v2
        with:
          card: pin
          options: username=leowrm&repo=verificador-fraude&theme=default
          path: profile/pin-verificador-fraude.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      # Streak de contribuições
      - name: Generate streak stats
        uses: DenverCoder1/github-readme-streak-stats@main
        with:
          options: user=leowrm&theme=default&hide_border=true&background=FFFFFF&ring=8B5CF6&fire=8B5CF6&currStreakLabel=8B5CF6
          path: profile/streak.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Commit generated cards
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add profile/*.svg
          git commit -m "Update README cards" || exit 0
          git push
