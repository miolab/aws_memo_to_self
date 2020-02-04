# EC2でのDocker環境構築

### AWS EC2 で、Docker環境構築する手順。

- EC2インスタンスを起動したところからスタート。
- 前提 : ローカルとのssh接続は設定済（ec2-user）。
- `Hello World`までやります。


## 実行環境

- ローカル
    - Windows 10
    - Tera Term

- クラウド
    - EC2インスタンスタイプ : t2.micro  
    ※ IAMユーザーで起動

---

## コード（Docker環境構築）

- 下準備（yumをアップデート）
    ```
    $ sudo yum update -y
    ```

- Dockerをyumでインストール
    ```
    $ sudo yum install -y docker
    ```

- Dockerの起動
    ```
    $ sudo service docker start
    ```

- ec2-userをDockerグループに入れる
    ```
    $ sudo usermod -aG docker ec2-user
    ```
    - ec2-userがDockerコマンドを実行できるように設定しておく。

- Dockerの環境確認
    ```
    $ docker info
    ```
    はじかれたら、
    1. `$ exit`で一回ぬけて、再度sshで入りなおす。
        ```
        $ docker info        # ↓ エラーケース ↓
        denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.39/info: dial unix /var/run/docker.sock: connect: permission denied

        $ exit        # いったんDockerからぬける
        ```
        - `exit`は、コンテナ変更内容を保存せず停止するコマンド。

    1. Tera Termでsshで再ログインしなおす  
        - （うまくいかない場合）EC2インスタンスを再起動しなおす

- ssh再接続
    ```
    $ docker info        # Docker環境確認

    Containers: 0
     Running: 0
     Paused: 0
     Stopped: 0
    Images: 0
    Server Version: 18.09.9-ce
    Storage Driver: overlay2
     Backing Filesystem: extfs
     Supports d_type: true
     Native Overlay Diff: true
    Logging Driver: json-file
    Cgroup Driver: cgroupfs
    Plugins:
    .
    .
    .
    ```

ここまでで、Docker環境が整ったことを確認。

---

## コード（DockerでHello World）

- `docker run`コマンドで、`hello-world`イメージを取得しコンテナ作成＆起動
    ```
    $ docker run -it --rm hello-world

    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    1b930d010525: Pull complete
    Digest: sha256:**************************************
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!        # Hello Worldに成功！
    .
    .
    .
    ```

    `Hello from Docker!`の出力を確認できました。

    なお、メッセージでは以下のとおり、どのようなステップをふんで  
    Hello WorldメッセージをDockerで出力したかの説明がつづきます。

    ```
    .
    .
    .
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
    3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/

    For more examples and ideas, visit:
    https://docs.docker.com/get-started/
    ```
