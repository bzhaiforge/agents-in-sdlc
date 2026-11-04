---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画し、コードを作成し、動作をテストし、プルリクエストをレビューし、エージェントを使って作業を管理します。

## シナリオ

あなたは、ボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に加わったばかりの開発者です。チームのバックログは GitHub Issue として用意されています。カテゴリやパブリッシャーによる絞り込みやページネーションなどの機能開発に加えて、アクセシビリティやコーディング規約の改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、普段使っている環境で利用できます。開発したい環境を選んでください。各ワークショップはそれぞれのセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使用します。Plan モードと Autopilot モード、カスタム指示、スキル、カスタムエージェント、Playwright Model Context Protocol (MCP)、Agent Merge、実用的なスラッシュコマンドを活用します。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業の進行を見守り、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
