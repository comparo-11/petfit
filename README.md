# PetFit

**あなたの心と体に寄り添う、新感覚ウェルビーイング・パートナー**

## 概要

このリポジトリは、ウェルビーイングアプリ「PetFit」の開発用リポジトリです。
ユーザーの多面的な健康状態（スマホ習慣、運動習慣、メンタル）をAIキャラクターの姿に映し出すことで、無機質なデータを愛着の持てる**“自分事”**として捉え直させます。
ウェブフロントエンドは`webapp/`、Unityによるキャラクター表現は`unity/`ディレクトリで管理します。

## 技術スタック

-   **Frontend:** Next.js, React, TypeScript
-   **Character:** Unity (WebGL), C#
-   **Styling:** Tailwind CSS
-   **Deployment:** Vercel

## 開発ロードマップ（初期実装計画）

まず、MVP（Minimum Viable Product）として、以下のコア機能の実装を目指します。

### WebApp (`webapp/`)
1.  **UIコンポーネントの作成:**
    * `CharacterDisplay.tsx`: Unityの描画エリアを表示するコンポーネント。
    * 状態入力フォーム：1日の活動量やメンタル状態を簡単なスタンプ等で入力するUI。
    * ヘッダー/フッター等の基本レイアウト。
2.  **APIの実装:**
    * `/api/health-status`: ユーザーの各種データを統合し、総合的なウェルネススコアを算出するAPIエンドポイント。
3.  **Unityとの連携:**
    * `CharacterDisplay.tsx`に`react-unity-webgl`を導入。
    * APIから取得したウェルネススコアを、`sendMessage`経由でUnityに送信するロジックを実装。

### Unity (`unity/`)
1.  **キャラクターモデルの実装:**
    * メインとなるキャラクターの3Dモデルをシーンに配置。
    * URP(Universal Render Pipeline)を用いて、高品質なマテリアルとライティングを設定。
2.  **基本アニメーションの実装:**
    * 「平常」「元気」「不調」など、基本的な状態に対応する待機アニメーションを作成。
3.  **WebAppとの連携:**
    * `CharacterManager.cs`を作成し、WebAppから送られてくるウェルネススコア（または状態を示す文字列）に応じて、キャラクターのアニメーションを切り替えるロジックを実装。

---

## 開発環境のセットアップ

このリポジトリで開発を始めるための手順です。

### 1. リポジトリのクローン

まず、このリポジトリを自分のPCにクローン（コピー）します。

```bash
git clone [https://github.com/your-team/petfit.git](https://github.com/your-team/petfit.git)
cd petfit
```

### 2. WebApp環境のセットアップ

ウェブアプリ（Next.js）を動かすための準備です。

```bash
# webappディレクトリに移動
cd webapp

# 必要なライブラリをインストール
npm install

# 開発サーバーを起動
npm run dev
```
ブラウザで `http://localhost:3000` を開くと、開発中のアプリが表示されます。

### 3. Unity環境のセットアップ

キャラクター（Unity）を開発するための準備です。

1.  **Unity Hub** を開きます。
2.  「リストに追加」または「Add project from disk」を選択し、クローンしたリポジトリ内の`unity`フォルダを選択します。
3.  プロジェクトを開き、必要なアセットなどをインポートして開発を開始します。

---

## チーム開発のルールとフロー

このプロジェクトでは、Git-flowを参考にした以下のルールで開発を進めます。

-   `main`ブランチ：本番リリースのための安定ブランチ。直接のコミットは禁止。
-   `develop`ブランチ：開発の主軸となるブランチ。全てのフィーチャーは一度このブランチにマージされます。

### 作業の流れ（ブランチ作成からプルリクエストまで）

#### 1. 作業開始前：最新の状態を取得

必ず`develop`ブランチを最新の状態にしてから作業を始めます。

```bash
git switch develop
git pull origin develop
```

#### 2. ブランチの作成

新しい機能追加やバグ修正は、必ず`develop`から新しいブランチを作成して行います。

```bash
# 機能追加の場合: feature/わかりやすい名前
git switch -c feature/add-login-page

# バグ修正の場合: fix/わかりやすい名前
git switch -c fix/header-layout-bug
```

#### 3. 開発とコミット

ブランチでコードを書き、区切りの良いところでコミットします。コミットメッセージは分かりやすく書きましょう。

```bash
git add .
git commit -m "feat: ログインページのUIコンポーネントを作成"
```

#### 4. プッシュ前のローカルチェック【重要】**

チーム全体のコード品質を保つため、プッシュする前に必ずローカルでフォーマットとLintのチェックを実行します。

```bash
# webappディレクトリに移動して実行
cd webapp

# 1. コードを自動整形
npm run format

# 2. コードのエラーチェック
npm run lint

# エラーが出たら修正して、再度コミット
```

#### 5. プッシュ**

自分の作業ブランチをリモート（GitHub）にプッシュします。

```bash
git push origin feature/add-login-page
```

#### 6. プルリクエスト（PR）の作成

GitHub上で、プッシュしたブランチから`develop`ブランチへのプルリクエストを作成します。
-   **Title / Description:** 何を、なぜ変更したのかを明確に記述します。
-   **Reviewers:** チームメンバーにレビューを依頼します。

#### 7. レビューとマージ**

レビューで修正指摘があれば、自分のブランチで修正→コミット→プッシュを繰り返します。
承認（Approve）されたら、`develop`ブランチにマージします。マージ後、作業ブランチは削除して構いません。