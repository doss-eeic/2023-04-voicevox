# ビルド手順
## 成功例
※上手くいったのは、自分のディレクトリが本家から落としたやつで7zがそんざいしていたからと思われる。
※gitignoreの書き方的に7zは追跡されない（消えてしまう）のでは…？
何はともあれ、本家のリポジトリをcloneしよう。
### 環境構築
既に入っていたnodeを消して、[volta](https://www.geeklibrary.jp/counter-attack/volta/#%25e8%2587%25aa%25e5%258b%2595%25e8%25a8%25ad%25e5%25ae%259a%25e3%2581%2599%25e3%2582%258b%25e4%25ba%25ba%25e7%2594%25a8)を導入した。
```
npm ci
```
### 実行
.env.productionをコピーして.envを作成する。.envの中身は次のとおり。
```
VITE_APP_NAME=voicevox
VITE_DEFAULT_ENGINE_INFOS=`[
    {
        "uuid": "074fc39e-678b-4c13-8916-ffca8d505d1d",
        "name": "VOICEVOX Engine",
        "executionEnabled": true,
        "executionFilePath": "/Applications/VOICEVOX.app/Contents/MacOS/run",
        "executionArgs": [],
        "host": "http://127.0.0.1:50021"
    }
]`
VITE_GTM_CONTAINER_ID=GTM-DUMMY

```
npm run electron:serve
```
