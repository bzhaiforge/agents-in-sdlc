---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント作成の規約を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進めたい場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も指定できます。絞り込み機能を構築したので、Copilot が参照した指示を確認し、ドキュメント作成の規約を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルがエージェントに読み込まれる方法を確認します。
- コーディング規約に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発方法に関するガイドラインがあります。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` にあるエクスポート済み関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。注入可能な `db` 引数がある場合は、それも含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述します。関連するコードを変更した場合はコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発慣行に沿ってタスクを進めるための情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。指示には、好みの規約やライブラリ、コードに含めるコメントなどを記述できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの **すべての** Copilot リクエストに送られる指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、多くのリクエストに共通する情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記述します。

> [!NOTE]
> 他の指示形式への対応は環境によって異なります。形式を使う前に、[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## プロジェクトの指示ファイルを確認する

スターター プロジェクトには、あらかじめ複数の指示ファイルが含まれています。変更前に内容を確認して、指示の影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内ファイルへのリンクを確認します。大きな指示を再利用できる小さなファイルに分け、コード生成の参考例を指定できます。

## チームのガイダンスに合わせて指示を更新する

既存のファイルには必要な情報が多く含まれていますが、生成した TypeScript に TSDoc コメントを追加する指示はまだありません。`.github/copilot-instructions.md` を更新しましょう。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しを探します。
2. 末尾に次の箇条書きを追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更を保存します。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、会話を終了して再開すると、機能のコンテキストを保ったまま新しいガイダンスを読み込めます。

1. 絞り込みの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、絞り込みの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、新しいガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを読み込む方法を確認し、新しい規約を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルがエージェントに読み込まれる方法を確認しました。
- コーディング規約に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な `quality-checks` スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
