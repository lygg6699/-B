name: Smart Contract CI
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
 
jobs:
  build-and-test:
    runs-on: ubuntu-latest
 
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
 
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 16
 
      - name: Install dependencies
        run: yarn install
        working-directory: ./CI-project
 
      - name: Install Tenderly CLI
        run: curl https://raw.githubusercontent.com/Tenderly/tenderly-cli/master/scripts/install-linux.sh | sudo sh
 
      - name: Run tests
        run: yarn run test:devnet
        working-directory: ./CI-project
        env:
          TENDERLY_ACCESS_KEY: ${{ secrets.TENDERLY_ACCESS_KEY }}
          TENDERLY_PROJECT_SLUG: '???' # your project slug
          TENDERLY_DEVNET_TEMPLATE: '???' # your devnet template slug
          TENDERLY_ACCOUNT_ID: '???' # your username or organization name
          
