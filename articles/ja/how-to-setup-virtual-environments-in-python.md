---
title: Python で仮想環境を設定する方法 – その必要性と利便性
date: 2024-08-26T12:04:41.820Z
author: Stephen Sanwo
authorURL: https://www.freecodecamp.org/news/author/stephen-sanwo/
originalURL: https://www.freecodecamp.org/news/how-to-setup-virtual-environments-in-python/
posteditor: ""
proofreader: ""
---

Python を使ってソフトウェアを開発する場合、基本的な方法は、マシンに Python をインストールし、必要なライブラリを全てターミナルからインストールし、すべてのコードを単一の .py ファイルやノートブックに書き、ターミナルで Python プログラムを実行することです。

<!-- more -->

これは多くの初心者や、データ解析のために Python を使っていた人にとって一般的なアプローチです。

こうした方法は、単純な Python スクリプトプロジェクトでは問題なく動作します。しかし、Python ライブラリ、API、またはソフトウェア開発キットを構築するような複雑なソフトウェア開発プロジェクトでは、複数のファイル、複数のパッケージおよび依存関係を扱うことが多く、その特定のプロジェクト用に Python 開発環境を分離する必要があります。

例えば、システムにインストールされた Python を使用してアプリ A に取り組んでいるとします。そして、グローバル Python ライブラリに packageX バージョン 1.0 をインストールしました。次に、ローカルマシンでプロジェクト B に切り替え、同じ packageX のバージョン 2.0 をインストールしましたが、バージョン 1.0 と 2.0 の間には破壊的な変更が含まれています。

この状態で再びアプリ A を実行しようとすると、さまざまなエラーが発生し、アプリは動作しません。これは、Python でソフトウェアを構築する際によく遭遇するシナリオです。これを回避するために、仮想環境を利用することができます。

このチュートリアルでは、仮想環境の基本から、**Virtualenv** を使って仮想環境をセットアップする方法までを網羅します。

## 仮想環境とは？

Python の公式ドキュメントには以下のように記載されています：

> "仮想環境とは、ある Python 環境であり、その Python インタプリタ、ライブラリ、スクリプトは他の仮想環境にインストールされたものから独立しており（デフォルトでは）、システムの Python、つまりオペレーティングシステムの一部としてインストールされた Python のライブラリとは別にインストールされます"

これを分かりやすく言い換えると、プロジェクトのために仮想環境をアクティベートすると、そのプロジェクトはシステムにインストールされた Python やそのモジュールから独立した、自己完結型のアプリケーションとなります。

新しい仮想環境にはライブラリをインストールするための独自の pip、自分のライブラリフォルダ、新しいライブラリが追加される場所、そして仮想環境を活性化するために使った Python のバージョンのための独自の Python インタプリタが含まれます。

この新しい環境により、アプリケーションは自己完結型になり、以下のようなメリットがあります：

- 開発環境がプロジェクト内に閉じ込められ、システムにインストールされた Python や他の仮想環境と干渉しない
- 複数の Python バージョン用に新しい仮想環境を作成できる
- 管理者権限なしにプロジェクトにパッケージをダウンロードできる
- アプリケーションを簡単にパッケージ化して、他の開発者と共有しやすくなる
- プロジェクトに使用する依存関係とサブ依存関係のリストをファイルに簡単に作成できるので、他の開発者がその環境を複製してすべての依存関係をインストールしやすくなる

仮想環境の使用は、通常単一の Python スクリプトから発展するソフトウェア開発プロジェクトに推奨されます。Python には、仮想環境を作成・使用するための複数の方法が提供されています。

以下のセクションでは、より低レベルな制御が可能な **venv** を使用して仮想環境を設定する方法について説明します。

仮想環境を設定するもう一つの一般的な方法は、より高レベルなアプローチである **pipenv** を使用することです。

## Venv を使用して仮想環境をインストールする方法

**Virtualenv** は Python 環境を設定するためのツールです。Python 3.3 以降、このツールのサブセットが venv モジュールとして標準ライブラリに統合されました。ターミナルで次のコマンドを実行して、ホスト Python に venv をインストールできます：

```bash
pip install virtualenv
```

プロジェクトで venv を使用するには、ターミナルで新しいプロジェクトフォルダを作成し、ターミナルでそのプロジェクトフォルダに移動して、次のコマンドを実行します：

```bash
python<version> -m venv <virtual-environment-name>
```

例えば、次のように実行します：

```bash
mkdir projectA
cd projectA
python3.8 -m venv env
```

新しい projectA フォルダを確認すると、**env** という新しいフォルダが作成されていることがわかります。env は私たちの仮想環境の名前ですが、これは任意の名前に変更できます。

もし env フォルダの内容を確認すると、Mac では bin フォルダが含まれていることがわかります。ここには、仮想環境を管理するための通常のスクリプトや、ライブラリをインストールするための pip などが含まれています。また、インストールした Python バージョンの Python インタプリタも含まれています。（このフォルダは Windows では Scripts と呼ばれます）

lib フォルダには、インストールしたライブラリのリストが含まれています。このリストを確認すると、仮想環境でデフォルトで提供されているライブラリのリストが表示されます。

## 仮想環境をアクティベートする方法



```bash
source env/bin/activate
```

これで仮想環境がアクティブになります。すぐにターミナルのパスに `env` が表示され、仮想環境がアクティブになっていることがわかります。

なお、Windows で仮想環境をアクティブにするには、以下のコードを実行する必要があります（プラットフォーム間の違いについてはこの[リンク][1]をご覧ください）：

```bash
env/Scripts/activate.bat // CMD で実行
env/Scripts/Activate.ps1 // Powershell で実行
```

## 仮想環境は機能しているか？

仮想環境をアクティブにしましたが、プロジェクトがホストの Python から本当に分離されているかどうか確認する方法はいくつかあります。

まず、仮想環境内で以下のコードを実行し、インストールされているパッケージのリストを確認します。仮想環境にはデフォルトで pip と setuptools の 2 つのパッケージしかインストールされていないはずです。

```bash
pip list
```

次に、新しいターミナルを開き仮想環境をアクティブにしない状態で同じコードを実行します。過去にインストールした多くのライブラリがホストの Python に表示されるでしょう。これらのライブラリは、仮想環境にインストールしない限り、仮想環境の一部ではありません。

## 仮想環境にライブラリをインストールする方法

新しいライブラリをインストールするには、pip を使って簡単にインストールできます。仮想環境は自分の pip を使用するため、pip3 を使用する必要はありません。

必要なライブラリをインストールした後、`pip list` を使ってすべてのインストール済みライブラリを表示するか、以下のコードを実行してプロジェクトの依存関係をリスト化したテキストファイルを生成します：

```bash
pip freeze > requirements.txt
```

この `requirements.txt` ファイルの名前は自由に変更できます。

## Requirements ファイル

なぜ requirements ファイルがプロジェクトにとって重要なのでしょうか？プロジェクトを zip ファイル（**env フォルダを除く**）としてパッケージ化し、開発者の友人と共有する場合を考えてみましょう。

友人は上記の手順に従って新しい仮想環境をアクティブにするだけで、開発環境を再現できます。

各依存関係を一つ一つインストールする代わりに、以下のコードを実行することで、プロジェクトのすべての依存関係を自分の環境に一括でインストールできます：

```bash
~ pip install -r requirements.txt
```

一般的には、`env` フォルダを共有することはお勧めしません。新しい環境で簡単に再現できるべきです。

通常、`env` ディレクトリは `.gitignore` ファイルに含まれ（GitHub などのバージョン管理プラットフォームを使用している場合）、プロジェクトリポジトリに環境ファイルがプッシュされないようにします。

## 仮想環境を無効化する方法

仮想環境を無効化するには、ターミナルで以下のコードを実行します：

```bash
~ deactivate
```

## 結論

Python 仮想環境を使用すると、システムにインストールされた Python や他の Python 環境から独立して開発プロジェクトを実行できます。これにより、プロジェクトを完全に管理し、簡単に再現可能にすることができます。

一般的に、.py スクリプトや Jupyter ノートブックの範囲を超えるアプリケーションを開発する場合、仮想環境を使用することをお勧めします。そして、今ではその設定と使用方法を知っているはずです。

[1]: https://docs.python.org/3/library/venv.html
```
