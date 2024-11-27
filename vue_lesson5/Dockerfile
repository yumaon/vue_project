# Node.jsの公式イメージをベースにする
FROM node:18

# 作業ディレクトリを設定
WORKDIR /app

# 必要な依存パッケージをインストール
RUN npm install -g @vue/cli

# ローカルファイルをコンテナにコピー
COPY . .

# コンテナ起動時に実行するコマンドを指定
CMD ["npm", "run", "serve"]