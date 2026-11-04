---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザーエクスペリエンスの改善や機能追加を目的に、GitHub Copilot cloud agent に3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェントページ][agents-page]もその1つです。このミッションコントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

高コントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに変更しているため、同じ作業に含めるのが適切です。エージェントにライトモードも実装するよう追加指示を送ります。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域に表示されるタスクの一覧を確認します。たとえば、次のタスクが表示されます。
   - コードベースのドキュメント更新。
   - ゲーム変更用 API の生成。
   - Web サイトへの高コントラストモードの追加。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。次のような操作が含まれる場合があります。
   - リポジトリのチェックアウト。
   - Copilot が作業する環境の作成。
   - MCP サーバーの設定。
   - 割り当てられたタスクを完了するための各種手順。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) 領域も確認します。ここでは、PR と変更されたファイルを確認できます。

## cloud agent に追加の指示を送る

実行中のタスクを確認したので、エージェントが高コントラストモードを作成している間に、ライトモードの切り替えも追加するよう依頼します。

1. 高コントラストモードを追加するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents ページのタスク画面。作業中のセッションに追加の指示を送るダイアログが強調表示されている](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業計画に追加したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後に、新しい依頼を計画に取り込みます。

前と同じように完了まで数分かかります。その間に、これまでに学んだ内容を振り返ってください。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視してガイドする中心的な場所である、Copilot agents ページを確認しました。このミッションコントロールでは、進行中または完了したタスクを追跡し、作業内容をレビューし、進行中のタスクに指示を送って範囲を調整できます。

このレッスンでは、次のことを行いました。

- ミッションコントロールとエージェントページを確認し、cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を完成させている間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
