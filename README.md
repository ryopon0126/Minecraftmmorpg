# Realm Codex（GitHub Pages版）

MMOCore・MythicMobs・MMOItems 向けに、ジョブ・モンスター・素材・武器・防具・アイテムのアイデアを登録し、スプレッドシート形式で一覧できるツールです。データは Firebase（Firestore）に保存され、チーム全員がリンクを開くだけで同じデータをリアルタイムに閲覧・編集できます。

同梱ファイル:

- `index.html` — ツール本体（このファイルをそのまま公開します）
- `firestore.rules` — Firestore のセキュリティルール（Firebaseコンソールに貼り付けます）
- `README.md` — このファイル

## 1. Firebaseプロジェクトを作成する

1. [Firebaseコンソール](https://console.firebase.google.com/) を開き、Googleアカウントでログインします。
2. 「プロジェクトを追加」から新しいプロジェクトを作成します（Googleアナリティクスは不要なら無効のままでOK）。

## 2. Firestore Database を作成する

1. 左メニューの「構築」→「Firestore Database」を開き、「データベースの作成」をクリックします。
2. ロケーションは `asia-northeast1`（東京）など任意の近いリージョンを選びます。
3. モードは「本番環境モード」「テストモード」どちらでも構いません（次の手順でルールを上書きします）。
4. 作成後、「ルール」タブを開き、内容をすべて削除して同梱の `firestore.rules` の中身を貼り付け、「公開」をクリックします。
   - **注意**：このルールは「リンクとFirebase設定値さえ知っていれば誰でも読み書きできる」設定です。チーム限定で使う場合は、リポジトリやURLを外部に共有しないよう注意してください。後からアクセス制限（Googleログイン必須など）を追加することも可能です。必要であれば教えてください。

## 3. ウェブアプリを登録し、設定値を取得する

1. Firebaseコンソールの左上の歯車アイコン →「プロジェクトの設定」を開きます。
2. 「マイアプリ」欄で `</>`（ウェブ）アイコンをクリックし、アプリ名（例: `realm-codex`）を入力して「アプリを登録」します。
3. 表示される `firebaseConfig` の内容（`apiKey`、`authDomain` などの6項目）をコピーします。

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "xxxx.firebaseapp.com",
  projectId: "xxxx",
  storageBucket: "xxxx.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:xxxxxxxx"
};
```

4. `index.html` をテキストエディタで開き、ファイル内の `firebaseConfig` の値（`YOUR_API_KEY` などのプレースホルダー部分）を、コピーした内容に書き換えて保存します。

## 4. GitHubリポジトリを作成してアップロードする

1. GitHubで新しいリポジトリを作成します（チーム限定にしたい場合は「Private」を選択。ただしGitHub Pagesを無料で使うにはPro/Team/Enterpriseプランが必要になる場合があります。無料プランの場合は「Public」で作成してください）。
2. 作成したリポジトリに、設定値を書き換えた `index.html`（と `firestore.rules`、このREADMEもあれば）をアップロードします。

ターミナルを使う場合の例:

```bash
git init
git add index.html firestore.rules README.md
git commit -m "Realm Codexを追加"
git branch -M main
git remote add origin https://github.com/あなたのアカウント名/リポジトリ名.git
git push -u origin main
```

GitHubのWeb画面から直接ドラッグ&ドロップでアップロードしても構いません。

## 5. GitHub Pagesを有効化する

1. リポジトリの「Settings」タブ →左メニューの「Pages」を開きます。
2. 「Build and deployment」の「Source」を「Deploy from a branch」にし、Branchを `main` / `/ (root)` に設定して「Save」します。
3. 数分待つと、ページ上部に公開URL（`https://あなたのアカウント名.github.io/リポジトリ名/`）が表示されます。

## 6. チームに共有する

発行されたURLをチームに共有すれば、誰でもブラウザでアクセスして登録・閲覧できます。ページを開いたときに「セットアップが必要です」という画面が出る場合は、`firebaseConfig` の書き換えがまだ反映されていません（3〜4の手順を再確認してください）。

## 補足

- データはブラウザではなく Firestore 側に保存されるため、誰か1人が登録した内容は、他の人が開いているページにもリアルタイムで反映されます。
- 無料枠（Sparkプラン）で運用する場合、Firestoreの読み書き回数には上限がありますが、チーム利用程度であれば通常は無料枠内に収まります。
- 各カテゴリの一覧画面にある「CSVエクスポート」ボタンで、いつでもExcel等で開けるCSVファイルとして書き出せます。
