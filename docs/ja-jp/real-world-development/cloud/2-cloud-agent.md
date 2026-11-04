---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、技術的負債に悩まされています。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフトプルリクエスト (PR) を作成します。時間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた Issue の重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。Web サイトの初期版を作成した開発者が、ドキュメントをほとんど残していません。まず、アプリケーションのすべてのエクスポート済み関数に TSDoc コメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの別のゲームを表示し、利用者が引き続きゲームを探せるようにします。完成度の高い実装ではなく、ユーザーエクスペリエンス (UX) の受け入れテストに使えるものを求めています。この作業は現在のボトルネックですが、より優先度の高い Issue もあります。

どちらも優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、非同期に作業を進められます。その間に別の作業に取り組み、後から戻って作業結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境を準備して作業を開始します。作業中や完了後にセッションを確認できます。提案された解決策をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot は、考える作業を省略する魔法ではありません。一見簡単な操作にも、掘り下げると複雑な部分があります。[Copilot cloud agent に割り当てるタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習を終えている場合は、`.github/copilot-instructions.md` が機能する様子を見ています。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認してください。後で影響を把握しやすくなります。

リポジトリの GitHub Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` の **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントの要件を見ます。
- `.github/instructions/unit-tests.instructions.md` の `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てるとき、生成される PR に TSDoc コメント、TypeScript の規約、ファイルヘッダーが反映されているかを確認します。後の演習で PR をレビューするときにも、これらの指示に触れます。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、環境やセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェアエンジニアのように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用セットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を用意できます。この演習用に事前設定されたワークフローでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを行った SQLite データベースを準備します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に対して、可能な限り最小の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # セットアップ手順でリポジトリをクローンする場合 (依存関係のインストールなど) は、contents: read が必要です。
      # セットアップでクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションし、シードデータを追加します。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じ形式ですが、次の点に注意してください。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot が実行するのを待たずに正しく動作するか確認できます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか不足しています。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成にも役立つため、このタスクを Copilot cloud agent に割り当て、後の演習で作業結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しいプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が作成した To Do リストを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかることがあります。Copilot が環境を準備し (前述のワークフローを実行し)、プロジェクトを分析して、作業の方針を決めています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 状態の更新が表示されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールし、Copilot がどのように問題を解決しているか確認します。コードを調べて状態を理解し、方針を検討してからコードを作成する様子がわかります。

作業には数分かかることがあります。Copilot cloud agent の主な利点は、非同期でタスクを進める間に別の作業ができることです。もう1つのタスクを Copilot cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の利点の1つは作業を分けられることです。エージェントに関連ゲームの表示を任せ、その間に別の作業を進めます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

間もなく Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを確認できます。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組み始めます。cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てて非同期に作業させることができます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに活用できます。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズする方法を確認しました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
