name: Rss

on:
  schedule:
    - cron: '0 */2 * * *'  # 每2小时执行一次
  workflow_dispatch:  # 允许手动触发

jobs:
  fetch_emails:
    runs-on: ubuntu-latest  # 在最新的 Ubuntu 环境中运行
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.9'  # 设置 Python 版本

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run rss.py
        env:
          DB_HOST: ${{ secrets.DB_HOST }}
          DB_NAME: ${{ secrets.DB_NAME }}
          DB_USER: ${{ secrets.DB_USER }}
          DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
          TELEGRAM_BOT_TOKEN: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          SECOND_TELEGRAM_BOT_TOKEN: ${{ secrets.SECOND_TELEGRAM_BOT_TOKEN }}
          ALLOWED_CHAT_IDS: ${{ secrets.ALLOWED_CHAT_IDS }}
          SECOND_ALLOWED_CHAT_IDS: ${{ secrets.SECOND_ALLOWED_CHAT_IDS }}
          EMAIL_USER: ${{ secrets.EMAIL_USER }}
          EMAIL_PASSWORD: ${{ secrets.EMAIL_PASSWORD }}
          IMAP_SERVER: ${{ secrets.IMAP_SERVER }}
        run: python rss.py
