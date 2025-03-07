# Unity-ROS2

Unity-ROS2間の通信用パッケージ

## 目次
<!-- TOC -->

- [概要](#概要)
- [開発環境](#開発環境)
- [インストール方法](#インストール方法)
- [使用方法](#使用方法)

<!-- /TOC -->

## 概要

UnityとROS2の間の通信を可能にすることで、ROS2を用いたUnityでのシミュレーション環境の構築を実現するためのパッケージを公開しています。

## 開発環境

- Ubuntu Linux - Jammy Jellyfish (22.04)
- ROS 2 Humble Hawksbill

## インストール方法

以下の手順に従ってパッケージのインストールを行います。
### 1. ROS 2 Humbleのセットアップ:  
   [こちら](https://docs.ros.org/en/humble/Installation.html)の手順に従って、ROS 2 Humbleをインストールしてください。既にROS2 Humbleのインストールが完了していればこの操作は不要です。
   
### 2. [Unity Hub](https://unity.com/ja/download) のダウンロード:
- パブリックキーの追加
   ```bash
   wget -qO - https://hub.unity3d.com/linux/keys/public | gpg --dearmor | sudo tee /usr/share/keyrings/Unity_Technologies_ApS.gpg > /dev/null

- リポジトリの追加
   ```bash
   sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/Unity_Technologies_ApS.gpg] https://hub.unity3d.com/linux/repos/deb stable main" > /etc/apt/sources.list.d/unityhub.list'

- ダウンロード
   ```bash
   sudo apt update
   sudo apt-get install unityhub
    ```

### 3. [Unity](https://unity.com/) のインストール:

- 上記でダウンロードしたUnity Hubを起動
- Unity Hubへのサインインを求められるので、サインインをする。アカウントが無ければ作成する
- InstallsからInstall Editorを選択し、Unity 6をインストール。或いはインストールするようポップアップが出る。

### 4. Unity-ROS2間の統合:

- ワークスペース作成
   ```bash
   mkdir -p unity_ws/src
   ```

- [ROS2 branch of the ROS-TCP-Endpoint](https://github.com/ndlab-ros2/Unity-ROS2/tree/main/ROS-TCP-Endpoint)リポジトリと[ROS2_Package](https://github.com/ndlab-ros2/Unity-ROS2/tree/main/ros2_packages)リポジトリの追加:
   ```bash
   cd unity_ws/src
   git clone https://github.com/ndlab-ros2/Unity-ROS2.git
   ```

- ビルド
   ```bash
   source install/setup.bash
   colcon build
   source install/setup.bash
   ```

⚠️ `source install/setup.bash`ソースコマンドは２回実行する必要があります。1回目は`colcon build
`ビルドを実行する際の環境設定を行い、2回目は新しくビルドされたパッケージを環境に追加します。

- IPアドレスの変更

   次のコマンドを実行して自身のIPアドレスを調べる。
   ```bash
   hostname -I
   ```

## 使用方法
