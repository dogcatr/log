
# Sharp

## 概要
このページで説明すること
1. Sharpのインストール
1. 3Dモデルの再構成

## インストール手順

1. WSL2でUbuntu-24.04環境を用意\
基本的な機能のインストール
    ~~~
    sudo apt update
    sudo apt install build-essential
    ~~~

1. condaをインストール、環境作成

    ~~~
    wget https://repo.anaconda.com/archive/Anaconda3-2025.12-2-Linux-x86_64.sh
    bash Anaconda3-2025.12-2-Linux-x86_64.sh
    source anaconda3/bin/activate
    conda init
    conda create -n sharp python=3.13
    ~~~

1. CUDA-toolkitをインストール

    ~~~
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-keyring_1.1-1_all.deb
    sudo dpkg -i cuda-keyring_1.1-1_all.deb
    sudo apt-get update
    sudo apt-get -y install cuda-toolkit-13-0
    ~~~

1. Sharpをインストール

    ~~~
    https://github.com/apple/ml-sharp
    cd ~/ml-sharp
    pip install -r requirements.txt
    ~~~

1. 画像から3DGSを作成

    ~~~
    sharp predict -i <image.png> -o <output_folder>
    ~~~

## 利用ソフトウェア・データおよびライセンス

ここで利用しているソフトウェア・データは以下のもの

### 利用ソフトウェア
- **ml-sharp**\
  ライセンス：商用利用などは不可能なライセンス\
  公式リポジトリ：https://github.com/apple/ml-sharp

---

### 注意事項
- 本リポジトリは研究・学習・情報共有目的で作成されています。
- 各ソフトウェアおよびデータの著作権は、それぞれの権利者に帰属します。
