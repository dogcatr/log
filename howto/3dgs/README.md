
# 3DGS

## 概要
このページで説明すること
1. 3DGSのインストール
1. 動画から3Dシーンの再構成

## TODOLIST
- 概要
- フォルダ構成

## インストール手順

1. WSL2でUbuntu-22.04環境を用意\
基本的な機能のインストール
    ~~~
    sudo apt update
    sudo apt install build-essential
    ~~~
1. リポジトリをクローン

    ~~~
    git clone https://github.com/graphdeco-inria/gaussian-splatting --recursive
    ~~~

1. condaをインストール

    ~~~
    wget https://repo.anaconda.com/archive/Anaconda3-2025.12-2-Linux-x86_64.sh
    bash Anaconda3-2025.12-2-Linux-x86_64.sh
    source anaconda3/bin/activate
    conda init
    ~~~

1. cuda-toolkitをインストール\
バージョンは11.8\
(予めGPUドライバーは11.8以降にしておく。13.0でテストした。)

    ~~~
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
    sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
    wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-ubuntu2204-11-8-local_11.8.0-520.61.05-1_amd64.deb
    sudo dpkg -i cuda-repo-ubuntu2204-11-8-local_11.8.0-520.61.05-1_amd64.deb
    sudo cp /var/cuda-repo-ubuntu2204-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
    sudo apt-get update
    sudo apt-get -y install cuda
    ~~~

1. 本体のインストール

    ~~~
    cd ~/gaussian-splatting/
    conda env create --file environment.yml
    conda activate gaussian_splatting
    ~~~

## 3Dシーンの再構成
1. 動画を適当なフォルダに配置\
画像データの場合は手順3へ
1. 動画を画像に分割\
(-r Nで動画一秒からN枚の画像を取得)

    ~~~
    mkdir data/images -p
    ffmpeg -i input.mp4 -r 10 -vcodec png data/images/image_%03d.png
    ~~~

1. 3Dシーン再構成前に特徴点の抽出とマッチング(COLMAP)\
失敗時は画像を増やすか、別のデータに変更

    ~~~
    colmap feature_extractor \
        --database_path data/database.db \
        --image_path data/images/
    colmap exhaustive_matcher \
        --database_path data/database.db
    mkdir data/sparse
    colmap mapper \
        --database_path data/database.db \
        --image_path data/images/ \
        --output_path data/sparse/
    mkdir data/dense
    colmap image_undistorter \
        --image_path data/images/ \
        --input_path data/sparse/0/ \
        --output_path data/dense/ \
        --output_type COLMAP
    ~~~

1. 3Dシーンの再構成

    ~~~
    python train.py \
        -s data/dense/ \
        -m data/dense/output
    ~~~

    outputフォルダに実行結果が入っている

    GPUが高温になって処理が遅い(サーマルスロットリング)時は\
    処理を分割するとよい

    ~~~
    python train.py \
        -s data/dense/ \
        -m data/dense/output \
        --iterations 2000 \
        --checkpoint_iterations 2000
    ~~~
    GPU温度が下がったら、コマンドを一つ実行\
    より高精度にしたい場合は、オプションの数字を書き換えて(この例ではすべて+2000)、実行と待機を繰り返す
    ~~~
    python train.py \
        -s data/dense/ \
        -m data/dense/output \
        --iterations 4000 \
        --checkpoint_iterations 4000 \
        --start_checkpoint data/dense/output/chkpnt2000.pth
    python train.py \
        -s data/dense/ \
        -m data/dense/output \
        --save_iterations 6000 \
        --iterations 6000 \
        --checkpoint_iterations 6000 \
        --start_checkpoint data/dense/output/chkpnt4000.pth
    ~~~

## 利用ソフトウェア・データおよびライセンス

ここで利用しているソフトウェア・データは以下のもの

### 利用ソフトウェア
- **3D Gaussian Splatting for Real-Time Radiance Field Rendering**\
  ライセンス：Gaussian-Splatting License\
  公式リポジトリ：https://github.com/graphdeco-inria/gaussian-splatting

---

### 注意事項
- 本リポジトリは研究・学習・情報共有目的で作成されています。
- 各ソフトウェアおよびデータの著作権は、それぞれの権利者に帰属します。
