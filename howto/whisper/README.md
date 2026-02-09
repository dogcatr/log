
# Whisper

## 概要
このページで説明すること
1. [Whisper](https://github.com/openai/whisper)のインストール
1. 変換(音声ファイル=>テキスト)

## TODOLIST
- 概要
- フォルダ構成

## インストール手順

1. WSL2でUbuntu-22.04環境を用意\
基本的な機能のインストール
    ~~~
    sudo apt update
    sudo apt install build-essential
    sudo apt install ffmpeg
    ~~~

1. condaをインストール \
すべてYesと答える

    ~~~
    cd ~
    wget https://repo.anaconda.com/archive/Anaconda3-2025.12-2-Linux-x86_64.sh
    bash Anaconda3-2025.12-2-Linux-x86_64.sh
    ~~~

1. python3.9の環境を作成

    ~~~
    source anaconda3/bin/activate
    conda init
    conda create -n whisper python=3.9 anaconda
    conda activate whisper
    ~~~

1. Whisperのインストール \
(インストール時に問題は発生しなかった)
    ~~~
    pip install -U openai-whisper
    ~~~

1. インストール確認

    ~~~
    whisper --help
    ~~~

## 変換(音声->テキスト)

1. 入力用の[データ](https://www.openslr.org/12)をダウンロード

    ~~~
    wget https://openslr.trmal.net/resources/12/dev-clean.tar.gz
    tar -zxvf dev-clean.tar.gz
    ~~~

1. Whisperで音声を処理\
(テスト時は"LibriSpeech/dev-clean/84/121123/84-121123-0000.flac"を利用)

    ~~~
    whisper <音声ファイル> --model turbo
    ~~~

## 利用ソフトウェア・データおよびライセンス

ここで利用しているソフトウェア・データは以下のもの

### 利用ソフトウェア
- **Whisper (OpenAI)**\
  ライセンス：MIT License\
  公式リポジトリ：https://github.com/openai/whisper

---

### 利用データ
- **Whisper (OpenAI)**\
    ライセンス：CC BY 4.0\
    公式リポジトリ：https://www.openslr.org/

---

### 注意事項
- 本リポジトリは研究・学習・情報共有目的で作成されています。
- 各ソフトウェアおよびデータの著作権は、それぞれの権利者に帰属します。
