# US GLASS 現場見積管理

積算ツールで作成したPDF見積書を、Google Driveへ建築会社・現場ごとに自動保存するWebアプリです。

## 機能

- 建築会社・現場の登録・管理
- 見積金額／原価／粗利の記録
- ステータス管理（見積中 / 受注 / 施工中 / 完了 / 失注）
- Google Drive への自動フォルダ作成＋PDFアップロード
  - `US_GLASS_見積管理 → 建築会社名 → 現場名` の階層で保存
- Drive リンクをアプリに保存（現場詳細から即アクセス）
- JSONバックアップ・復元
- iPhone Safari / Mac Chrome 対応

## セットアップ

### 1. Google Cloud Console

1. [console.cloud.google.com](https://console.cloud.google.com/) でプロジェクト作成
2. **APIとサービス → ライブラリ** → `Google Drive API` を有効化
3. **APIとサービス → 認証情報 → 認証情報を作成 → OAuthクライアントID**
4. アプリの種類：**ウェブアプリケーション**
5. **承認済みのJavaScript生成元** に追加：
   ```
   https://あなたのユーザー名.github.io
   ```
6. 作成されたクライアントIDをコピー

### 2. GitHub Pages 公開

1. このリポジトリを GitHub にプッシュ
2. リポジトリの **Settings → Pages**
3. Source: **Deploy from a branch → main → / (root) → Save**
4. 公開URL: `https://あなたのユーザー名.github.io/リポジトリ名/`
5. この URL を Google Cloud の「承認済みJavaScript生成元」に登録

### 3. アプリの初期設定

1. 公開された GitHub Pages URL をブラウザで開く
2. **設定タブ** → Google Client IDを貼り付けて「保存」
3. ヘッダーの **「Googleログイン」** をタップ
4. 会社・現場を登録して、PDFをアップロード

## セキュリティ設計

| 項目 | 設計 |
|------|------|
| OAuth スコープ | `drive.file` のみ（アプリ作成ファイルのみアクセス可能） |
| Client ID 管理 | コード非記載・アプリ設定画面から入力（デバイス内localStorageに保存） |
| アクセストークン | メモリのみ保存（ページリロードで消去、localStorageに保存しない） |
| データ送信 | 外部サーバーへの送信なし（Drive APIのみ） |
| ファイルアクセス | ユーザーがログインしていないと操作不可 |

## 注意事項

- `file://` プロトコルでは Google ログインが動作しません（必ず `https://` で使用）
- iPhone Safari でポップアップがブロックされる場合：**設定 → Safari → ポップアップブロック** をオフに
- データは各デバイスの localStorage に保存されます。機種変更時はバックアップを取得してください

## ファイル構成

```
index.html   # アプリ本体（単一ファイル）
README.md    # このファイル
```
