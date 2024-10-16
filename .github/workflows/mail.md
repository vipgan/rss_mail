name: Emails 

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
          pip install -r requirements.txt  # 安装依赖

      - name: Run fetch_emails script
        env:  # 通过环境变量传递必要的信息
          EMAIL_USER: ${{ secrets.EMAIL_USER }}
          EMAIL_PASSWORD: ${{ secrets.EMAIL_PASSWORD }}
          IMAP_SERVER: ${{ secrets.IMAP_SERVER }}
          TELEGRAM_API_KEY: ${{ secrets.TELEGRAM_API_KEY }}
          TELEGRAM_CHAT_ID: ${{ secrets.TELEGRAM_CHAT_ID }}
          DB_HOST: ${{ secrets.DB_HOST }}
          DB_NAME: ${{ secrets.DB_NAME }}
          DB_USER: ${{ secrets.DB_USER }}
          DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
        run: python mail.py  # 运行你的脚本，替换为实际脚本名
