
# 3DGRUT

## 概要
このページで説明すること
1. 3DGRUTのインストール
1. 3Dモデルの変換

## インストール手順

1. WSL2でUbuntu-24.04環境を用意\
基本的な機能のインストール
    ~~~
    sudo apt update
    sudo apt install build-essential
    cd ~
    sudo apt-get install gcc-11 g++-11
    ~~~

1. condaをインストール

    ~~~
    wget https://repo.anaconda.com/archive/Anaconda3-2025.12-2-Linux-x86_64.sh
    bash Anaconda3-2025.12-2-Linux-x86_64.sh
    source anaconda3/bin/activate
    conda init
    ~~~

1. リポジトリをクローン

    ~~~
    git clone --recursive https://github.com/nv-tlabs/3dgrut.git
    cd 3dgrut
    ~~~

1. 公式サイト通りにインストール

    ~~~
    chmod +x install_env.sh
    # 以前はコード内の'pip install'に'--no-build-isolation'をつける必要があった。
    # 現在ははじめからついていて、この部分で失敗することはない。
    ./install_env.sh 3dgrut WITH_GCC11
    ~~~

1. 環境を有効化して、ファイル変換

    ~~~
    conda activate 3dgrut
    python -m threedgrut.export.scripts.ply_to_usd assets/point_cloud.ply --output_file point_cloud.usdz
    ~~~

## 利用ソフトウェア・データおよびライセンス

ここで利用しているソフトウェア・データは以下のもの

### 利用ソフトウェア
- **3dgrut**\
  ライセンス：Apache-2.0\
  公式リポジトリ：https://github.com/nv-tlabs/3dgrut

---

### 注意事項
- 本リポジトリは研究・学習・情報共有目的で作成されています。
- 各ソフトウェアおよびデータの著作権は、それぞれの権利者に帰属します。
