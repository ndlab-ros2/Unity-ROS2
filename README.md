# Unity-ROS2

Unity-ROS2間の通信用パッケージ

![Image](https://github.com/user-attachments/assets/d205efc0-7201-4792-8e70-3fee1f0d3889)

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

- IPアドレスの確認
   ```bash
   hostname -I
   ```

- IPアドレスの変更

  上記のコマンドの実行によって出力された自身のIPアドレスを`your IP address`に書き換えて次のコマンドを実行する。
   ```bash
   ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:=your IP address
   ```


   ○上記のコマンドの出力が以下の様になっていれば変更完了です。ただし、`<your IP address>`この部分には自身のIPアドレスが表示されます。
  
   `[INFO] [1741360093.885079373] [UnityEndpoint]: Starting server on <your IP address>:10000`


   ※もし、サーバーがデフォルトの 10000 とは異なるポートにする必要がある場合は、次のコマンドを実行してください。
   ```bash
   ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:=your IP address -p ROS_TCP_PORT:=10000
   ```
⚠️ `your IP address`:自身のIPアドレスに変更してください。

### 5. [Unity](https://unity.com/) のセットアップ:
-  Unity Hubを起動し、必要であればサインインをする。
-  `Projects`→`New project`から新しいプロジェクトを作成する。
-  新しいプロジェクトを開き、`Window`→`Package Manager`でパッケージマネージャを起動
-  画面左上の`+`ボタンをクリックし、`Install package from git URL...`を選択して、ROS-TCP-Connector(リンクは[こちら](https://github.com/Unity-Technologies/ROS-TCP-Connector))のURLをコピー&ペーストして、**Add**ボタンをクリックしてROS-TCP-Connectorのパッケージを追加する。以下の画面の様になっていれば完了。

![Image](https://github.com/user-attachments/assets/aa0e37b5-cc6b-490a-a8ff-a7926bd0d2ea)

-  先程の操作によってUnityのメインメニューに**Robotics**というタブが追加されていることを確認して、`Robotics`→`ROS Settings`からROSの設定画面を表示

![Image](https://github.com/user-attachments/assets/f6fda079-4141-4718-8a6c-e11668b6a646)

-  `Protocol`を**ROS2**に切り替える
-  `ROS IP Address`に自身のIPアドレスを入力する。自身のIPアドレスが分からない場合は以下のコマンドを実行して、出力されるIPアドレスをコピー&ペーストする
   ```bash
   hostname -I
   ```
-  `ROS Port`に自身で設定したポートを入力する。
⚠️　デフォルトは**10000**であるが、**4. Unity-ROS2間の統合**の「IPアドレスの変更」にて、デフォルトの10000とは異なるポートにした場合は、そのポートの値を入力しなければならない。

### 6. Unity Robotics Demoの実行:
-  Unityのメインメニューで`Robotics`→`Generate ROS Messages...`より、メッセージブラウザーウィンドウを起動
-  「ROS message path」の`Browse`ボタンをクリックし、'~/unity_ws/src/ros2_packages/unity_robotics_demo_msgs'をROSメッセージパスに設定する。
-  「Built message path」の下部に表示される**unity_robotics_demo_msgs**サブフォルダーを展開し、「***msg***」の`Build 2 msgs`と「***srv***」の`Build 2 srvs`をクリックして、ROS.msgファイルと.srvファイルからC#スクリプトを作成する。

## 使用方法

### 1. C#スクリプトの作成:
-  Unityのメインメニューで`Robotics`→`Generate ROS Messages...`より、メッセージブラウザーウィンドウを起動
-  「ROS message path」の`Browse`ボタンをクリックし、'~/unity_ws/src/ros2_packages/unity_robotics_demo_msgs'をROSメッセージパスに設定する。
-  「Built message path」の下部に表示される**unity_robotics_demo_msgs**サブフォルダーを展開し、「***msg***」の`Build 2 msgs`と「***srv***」の`Build 2 srvs`をクリックして、ROS.msgファイルと.srvファイルからC#スクリプトを作成する。

### 2. パブリッシャーの作成:
-  Unityのメインメニューで`Project`→`Assets`→`RosMessages`→`UnityRoboticsDemo`より、作成したC#スクリプトを編集する(C#スクリプトがない場合はその場で作成する)。
-  c#スクリプトの名前は自分が分かりやすい名前に変更しておく。今回は例として`Ros Publisher Example`としている。
-  次のコードをスクリプトに貼り付ける。
   ```bash
   using System.Collections;
   using System.Collections.Generic;
   using UnityEngine;
   using Unity.Robotics.ROSTCPConnector;
   using RosMessageTypes.Std;
   
   public class MyPublisher : MonoBehaviour
   {
       ROSConnection ros;
       float time;
   
       // Start is called before the first frame update
       void Start()
       {
           // ROSコネクションの取得
           ros = ROSConnection.GetOrCreateInstance();
   
           // パブリッシャの登録
           ros.RegisterPublisher<StringMsg>("my_topic");
           
       }
   
       // Update is called once per frame
       void Update()
       {
           time += Time.deltaTime;
           if(time < 1.0f){
               return;
           }
           time = 0.0f;
   
           // メッセージのパブッシュ
           StringMsg msg = new StringMsg("Hello Unity!");
           ros.Publish("my_topic", msg);
       }
   }
   ```

-  Unityのメインメニューの`Hierarchy`にある`+`ボタンをクリックし、`Create Enpty`から空のGameObjectを作成する。(必要であればGameObjectの名前を変更しても良い)
-  そのオブジェクト内にスクリプトを作成して、先程作成したスクリプトを貼り付ける
-  ROS2側のエンドポイントの起動
   ```bash
   ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:=your IP address
   ```

   ※もし、サーバーがデフォルトの 10000 とは異なるポートにする必要がある場合は、次のコマンドを実行してください。
   ```bash
   ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:=your IP address -p ROS_TCP_PORT:=10000
   ```
⚠️ `your IP address`:自身のIPアドレスに変更してください。
