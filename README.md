# This is a basic workflow to help you get started with Actions

name: CI

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Install
        uses: actions/setup-node@v2
        with:
          node-version: 16.13.1
          cache: 'npm'
      - run: npm install

      - name: Build
        run: npm run build

      - name: deploy file to server
        uses: ilCollez/ssh-scp-deploy@main
        with:
          host: ${{ secrets.SERVER_IP }}
          port: 22
          username: 'root'
          password: ${{ secrets.SERVER_PASSWORD }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          files: './github-cd/*'
          remote-path: '/usr/share/nginx/html'
          clean: true
