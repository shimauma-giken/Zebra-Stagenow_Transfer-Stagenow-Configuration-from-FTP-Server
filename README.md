### Zebra-Stagenow_Transfer Datawedge Configuration from FTP Server
# FTPサーバからDatawedge設定をダウンロードする環境を構築する方法

外部FTPサーバからZebra TCデバイスにStageNow設定ファイルを転送するには、StageNowのFileMgr機能を活用するのが最も確実です。以下はStageNow 4.1以降を前提とした構成手順です。

---

</br>

### ✅ 前提条件
- TCデバイスがAndroid MX 7.1以上を搭載していること
- PCにStageNow バージョン4.1以上がインストール済み
- 外部FTPサーバが稼働しており、ファイルが配置されている  
  [参考：Zebra-StageNow-Mirror向けにWindows11上でFTPサーバを構築するためのガイド](https://github.com/shimauma-giken/Zebra-StageNow-Mirror_Create_FTP_Server-on-Windows-11)
- FTPサーバのURL、ポート、認証情報（ユーザー名・パスワード）を把握している
- Datawedgeのプロファイルをエクスポート済み


</br>

### 🛠️ 手順概要：StageNowでFTP転送プロファイルを作成

1. **Datawedgeのプロファイルをエクスポートする**
    - TCデバイス > DataWedge > 右上ドット > 設定 > エクスポート/プロファイルのエクスポート
    
    |選択肢|説明|ファイル名|
    |-|-|-|
    | エクスポート | Datawedgeの全設定設定 | datawedge.db
    | プロファイルのエクスポート | アプリ別のDatawedge | dwprofile_[appname].db

1. **エクスポートしたプロファイルをPCにダウンロードする。**

    ダウンロード方法は色々あるが、Windowsコマンドを利用した方法を記載する。

    ```
    C:\Users\moget\Downloads>adb pull /storage/emulated/0/Android/data/com.symbol.datawedge/files/datawedge.db ★
    /storage/emulated/0/Android/data/com.symbol.datawedge/file...file pulled, 0 skipped. 18.1 MB/s (357376 bytes in 0.019s)
    ```

1. ダウンロードしたファイルをFTPサーバにアップロードする。



</br>

### 🛠️ 手順概要：StageNowでFTP転送プロファイルを作成

1. **StageNow Workstationを起動**
   - 管理者ログイン後、「新しいプロファイルの作成」を選択
   - 利用TCデバイスのMXバージョン（例：MX 13.5）を選択

2. **Xpertモードでプロファイル作成**
   - プロファイル名を入力し、「開始」をクリック
   - 「FileMgr」モジュールを追加

3. **FileMgr設定**

    | 設定    | 説明    |  設定例*
    |-|-|-|
    | Target path & file name| プロファイルの転送先 | /enterprise/device/settings/datawedge/autoimport/datawedge.db
    | Source file URI        | プロファイル転送元   | ftp-p://ftpuser:zebra123@192.168.4.144:21/stagenow/datawedge.db **

    \* 設定はご利用の環境に応じて設定ください。

    <img height="300" src="image-2.png"> </br>

    </br>

    <img height="300" src="image-1.png"> </br>

   ** 特殊文字はURLエンコードが必要（例：`@` → `%40`）
    </br>

4. **バーコード生成と転送**
   - プロファイル完了後、PDF417などのバーコードを生成
   - TC27のStageNowクライアントでバーコードをスキャン
   - FTPサーバからファイルが転送される

    <img height="300" src="image-3.png"> </br>

1. Datawedge設定が変更されているか確認をする。（任意）

---

</br>

### ⚠️ トラブルシューティング

- **「FTPサーバーに接続できません」エラー**
  - Wi-Fi接続を確認
  - FTPサーバの証明書が有効か確認（FTPS使用時）
  - ファイアウォールやポート制限をチェック

---

</br>

### 🔗 参考リンク
- [StageNow: 外部 FTPS を使用してファイルを転送する](https://supportcommunity.zebra.com/s/article/000018716?language=ja) – URI形式や特殊文字の扱いについて詳述
- [StageNow外部FTPSファイル転送が失敗する：エラー「FTPサーバーに接続できません」](https://supportcommunity.zebra.com/s/article/000022324?language=ja) – 接続エラーの原因と対処法
- [StageNow 4.1以降の基本的なリモート FTP サーバーのセットアップ](https://supportcommunity.zebra.com/s/article/000018839?language=ja) – プロファイル作成手順の詳細

---

