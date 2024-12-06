<div align= "center">
    <h1> プロアクティブエージェント </h1>
</div>

<div align="center">

![Dialogues](https://img.shields.io/badge/Current\_Event\_Size-6790-red?style=flat-square)

</div>

<p align="center">
  <a href="#概要">モデル</a> •
  <a href="#データ">データ公開</a> •
  <a href="#使用方法">使用方法</a> •
  <a href="#引用">引用</a> •
  <a href="https://arxiv.org/abs/2410.12361">論文</a>

</p>

</div>

このプロジェクト（プロアクティブエージェント）は、ユーザーの要求を予測し、ユーザーからの明示的な要求なしに支援や行動を提案する完全にアクティブなエージェントを構築することを目的としています。データ収集と生成パイプラインを開発し、自動評価機を構築し、生成されたデータ内でエージェントをトレーニングすることでこれを実現します。現在、収集と生成の全パイプライン、データセット、対応する評価スクリプト、およびプロアクティブエージェントのためのLLMを微調整するためのプロンプトを提供しています。

*この文書を[English](README.md)で読む。*
*[中文](README_zh.md)で読む。*

## 概要

✨以下はプロアクティブエージェントの全プロセスの概要図です。

<br>
<div align="center">
<img src="assets/overall_pipeline.png" width="800px">
</div>
<br>


✨✨特徴：
- **環境感知**：Activity Watcherを通じて環境シーンとユーザーアクティビティを収集するスクリプトを提供し、モデルに基づいてタスクを自動的に推奨します。
- **支援アノテーション**：プロアクティブエージェントが生成した応答をアノテートするためのプラットフォームを提供し、人間のアノテーターと結果を一致させる良い方法です。
- **動的生成**：新しいデータを生成するための動的パイプラインを提供し、ユーザーからのフィードバックが後続のイベントに影響を与えることができます。
- **構築パイプライン**：**環境ジム**、**プロアクティブエージェント**、および**報酬モデル**で構成される生成パイプラインを提供し、報酬モデルはテストセットで`0.918`のF1スコアを達成しています。

デモも提供されており、エージェントのパフォーマンスを示しています。

https://github.com/user-attachments/assets/4a9152e8-15ee-4bdf-a917-52a9e6b4f375

将来的には、データの質を継続的に向上させ、実世界のシナリオのカバレッジを増やしていきます。

## データ

👐プロアクティブエージェントは現在、コーディング、ライティング、および日常生活のシナリオにのみ使用されることを意図しており、このデータセットの作成者、所有者、または貢献者の意見や見解を反映するものではありません。Apache License 2.0の下で配布されています。以下はデータの統計です：

| 設定 | コーディング | ライティング | 日常生活 | 合計 |
|:-------------:|:------:|:-----:|:-----:|:-----:|
|   インスタンス数    | 46 |  46 | 44 | 136 |
|   イベント数  | 2275 |  2354 | 2161 | 6790 |

プロアクティブエージェントのすべてのトレーニングインスタンスは、[GYM](gym/README_ja.md)から生成されました。
すべてのシナリオで人間のトレースを収集するために[Activity Watcher](https://activitywatch.net/)を利用し、プロアクティブエージェントの有効性を検証するためにテストセットをアノテートしました。
データ収集とアノテーションの詳細については[こちら](dataset/README_ja.md)をご覧ください。

## 📦 インストール

このリポジトリをクローンし、プロアクティブデマンドセンシングエージェントフォルダに移動します
```bash
git clone git@github.com:thunlp/ProactiveAgent
cd ProactiveAgent
```

パッケージをインストールします
```bash
conda create -n activeagent python=3.10
conda activate activeagent
pip install -r requirements.txt
```

### Activity Watcherのインストール

- オペレーティングシステムに基づいて[公式ウェブサイト](https://activitywatch.net/downloads/)からメインアプリをダウンロードできます。
- Chrome用の拡張機能は`./agent/resource/aw-watcher-web.zip`にあります。この拡張機能をダウンロードするには、ファイルをダウンロードして解凍する必要があります。
  - Edgeユーザーの場合、`edge://extensions/`サイトにアクセスし、開発者モードを開いて`load unpacked`をクリックして拡張機能を読み込みます。
  - Google Chromeユーザーの場合、`chrome://extensions/`サイトにアクセスし、開発者モードを開いて`load unpacked`を選択して解凍した拡張機能を読み込みます。
  - この拡張機能は`Safari`ではテストされていません。
- Vscodeユーザー向けの公式拡張機能があり、[マーケットプレイス](https://marketplace.visualstudio.com/items?itemName=activitywatch.aw-watcher-vscode)からダウンロードするか、vscodeの拡張機能で`aw-watcher-vscode`を検索してインストールできます。

インストールが完了したかどうかを確認するには、ブラウザを開いて`http://localhost:5600/#/timeline`にアクセスし、ウィンドウに4つのトレース（`afk`、`vscode`、`window`、`web`）が表示されているかどうかを確認してください。

## 🚀 使用方法

### 設定
まず、`private.toml`ファイルを設定する必要があります。例は`example_config.toml`に示されています：

```bash
cp example_config.toml private.toml
```

`default_completions_model`、`api_key`、および`base_url`を自分の設定に変更する必要があります。

### プロアクティブエージェントの実行
プロアクティブエージェントを体験するには、まず`./agent`フォルダに移動し、[こちら](agent/README_ja.md)の指示に従ってください。

### 報酬モデルの接続
プロアクティブエージェントの体験を向上させるために、報酬モデルを使用してプロアクティブエージェントからのメッセージをフィルタリングできます。
報酬モデルをプロアクティブエージェントに接続する手順は以下の通りです。
__更新予定__

### プロアクティブエージェントとの対話
エージェントはウィンドウにトーストを作成して提案を行います。プロアクティブエージェントと対話するには、次のいずれかを選択できます：
- 提案を受け入れる：トーストの本文（Windows）をクリックするか、ボタン（MacOS）をクリックしてエージェントに提案を受け入れることを知らせます。エージェントはそれに応じたアクションを実行します。
- 提案を拒否する：閉じるボタン（トーストの右上のx）をクリックしてエージェントに提案を拒否することを知らせます。エージェントは次のターンで別の方法で提案を試みます。
- 提案を無視する：何もしません。エージェントは時間間隔に応じてトーストを削除します。何もしないことでエージェントはあなたが忙しくて提案を無視したことを知り、次のターンで提案を減らすようにします。

## 📊 モデル結果
プロアクティブエージェントのパフォーマンスを自動的に評価するために、アノテートされたデータに基づいて報酬モデルを構築し、プロアクティブエージェントのパフォーマンスを評価します。
報酬モデルはテストセットで`0.918`のF1スコアを達成しており、プロアクティブエージェントのパフォーマンスの良い指標となります。

### 報酬モデルの実験結果
テストセットで報酬モデルと人間のアノテーターの一致を評価します：
- **Missed-Needed (MN)**：ユーザーが助けを必要としているが、エージェントが助けを提供しないシナリオ。
- **Non-Response(NR)**：ユーザーが助けを必要としておらず、エージェントが助けを提供しないシナリオ。
- **Correct-Detection(CD)**：ユーザーが助けを必要としており、エージェントが助けを提供するシナリオ。
- **False-Alarm(FA)**：ユーザーが助けを必要としていないが、エージェントが助けを提供するシナリオ。

報酬モデルの判断を人間のアノテーターと比較します。
[テストセット](eval/README_ja.md#報酬モデルの評価)で異なるLLMと私たちのモデルのパフォーマンスを比較します。
結果は以下の通りです：

|                         | GPT-4o  | GPT-4o-mini | LLaMa 3.1 8b | LLaMa 3.1 70b  | ours    |
|-------------------------|---------|-------------|--------------|----------------|---------|
| Missed-Need (MN)        | 0.0333  | 0.5667      | 0.8000       | 0.3333         | 0.8000  |
| Non-Response (NR)       | 1.0000  | 0.5667      | 0.3000       | 0.8333         | 0.8667  |
| Correct-Detection (CD)  | 1.0000  | 0.8667      | 0.9667       | 1.0000         | 1.0000  |
| False-Alarm (FA)        | 0.0000  | 0.3333      | 0.1333       | 0.0667         | 1.0000  |
| Accuracy                | 0.5083  | 0.5833      | 0.5500       | 0.5583         | **0.9167**  |
| Precision               | 0.5042  | 0.5658      | 0.5429       | 0.5340         | **0.9032**  |
| Recall                  | 1.0000  | 0.7167      | 0.6333       | 0.9167         | **0.9333**  |
| F1                      | 0.6704  | 0.6324      | 0.5846       | 0.6748         | **0.9180**  |


### プロアクティブエージェントの実験結果
現在の実験では、報酬モデルを使用してプロアクティブエージェントのパフォーマンスを評価します。
次の指標を定義します：
- **True Positive(TP)**：プロアクティブエージェントがタスクを正しく予測し、報酬モデルがそれを受け入れるインスタンス。
- **False Positive(FP)**：プロアクティブエージェントがタスクを予測するが、報酬モデルがそれを受け入れないインスタンス。
- **True Negative(TN)**：プロアクティブエージェントがタスクを予測せず、報酬モデルもタスクを受け入れないインスタンス。
- **False Negative(FN)**：プロアクティブエージェントがタスクを予測しないが、報酬モデルがそれを受け入れるインスタンス。

[ProactiveBench](eval/README_ja.md)のテストセットでプロアクティブエージェントのパフォーマンスを報告します。

| モデル                  | リコール  | 精度     | 正確度 | 誤報率       | F1スコア  |
|:----------------------:|:-------:|:---------:|:--------:|:-----------:|:---------:|
| GPT-4o-mini            | 100.00% | 35.28%    | 36.12%   | 64.73%      | 52.15%    |
| GPT-4o                 | 98.11%  | 48.15%    | 49.78%   | 51.85%      | 64.60%    |
| Claude-3.5-Sonnet      | 97.89%  | 45.37%    | 49.78%   | 54.63%      | 62.00%    |
| LLaMA-3.1-8B           | 98.86%  | 38.16%    | 39.06%   | 61.84%      | 55.06%    |
| LLaMA-3.1-8B-Proactive | 99.06%  | 49.76%    | 52.86%   | 50.24%      | 66.25%    |
| Qwen2-7B               | 98.02%  | 44.00%    | 43.61%   | 56.00%      | 60.74%    |
| Qwen2-7B-Proactive     | 100.00% | 49.78%    | 50.66%   | 50.22%      | 66.47%    |



## 引用
このプロジェクトが研究に役立つ場合は、以下の引用を検討してください：
```
@misc{2024,
  author = {THUNLP},
  title = {ProactiveAgent},
  year = {2024},
  publisher = {GitHub},
  journal = {GitHub Repository},
  howpublished = {\url{https://github.com/thunlp/ProactiveAgent}}
}
```

## フレンドリーリンク
- [ChatDev](https://github.com/openbmb/ChatDev)
- [Activity Watcher](https://activitywatch.net/)