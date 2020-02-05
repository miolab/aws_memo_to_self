# EC2でDockerにElixir環境を構築

### AWS EC2 で、DockerでElixir環境構築する手順。

Elixirで`Hello World`出すところまでやります。

- EC2インスタンスを起動したところからスタート。
- __前提1__ : ローカルとのssh接続は設定済（ec2-user）。
- __前提2__ : EC2にDockerインストール済
  - 手順→ [EC2でのDocker環境構築](https://github.com/miolab/aws_memo_to_self/blob/master/ec2_docker.md)



## 実行環境

- ローカル
    - Windows 10
    - Tera Term

- クラウド
    - EC2インスタンスタイプ : t2.micro  
    ※ IAMユーザーで起動


---

## コード

- `docker run`コマンドで、Elixirのイメージを取得しコンテナ作成＆起動

    ```
    $ docker run -it --rm elixir

    Unable to find image 'elixir:latest' locally
    latest: Pulling from library/elixir
    8f0fdd3eaac0: Pull complete
    d918eaefd9de: Pull complete
    43bf3e3107f5: Pull complete
    27622921edb2: Pull complete
    dcfa0aa1ae2c: Pull complete
    5e85baa44689: Pull complete
    a2d4d2027b50: Pull complete
    0a05ca1ed286: Pull complete
    7536bf0af39b: Pull complete
    Digest: sha256:*********************************
    Status: Downloaded newer image for elixir:latest
    Erlang/OTP 22 [erts-10.6.2] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1] [hipe]

    Interactive Elixir (1.9.4) - press Ctrl+C to exit (type h() ENTER for help)

    iex(1)>
    ```

- IExに入ってるので、そのまま`Hello World`を出力してみます。

    ```
    iex(1)> IO.puts("Hello World!")
    Hello World!
    :ok
    ```

- `Hello world!` の出力をぶじ確認できました。

    ```
    iex(2)> IO.gets("input your name: ") |> String.trim |> IO.puts
    input your name: im
    im
    :ok
    ```

- 他のElixirコードもIExで問題なく入出力できています。

- IExを抜けるには、`Ctrl + C` を2回押し。
