## webアプリケーションのセキュリティに関して
- [参考](https://www.atmarkit.co.jp/ait/articles/0309/20/news001.html)

## webアプリの検査方法
- 一般に「検査」とは、外部からアプリにブラウザからアクセスする、ブラックボックステストを示す
    - アプリの脆弱性を発見するというより、よくある脆弱性を発見するために行う

## 攻撃いろいろ
- OSコマンドインジェクション
    - 文字入力を受け付けるアプリで、OSのコマンドを混ぜることで予期しない挙動を引き起こさせる攻撃
    - SQL インジェクションの OS コマンドバージョンといった認識
    - 対策
        - 想定しない文字列をエスケープする。(注意が必要な特殊文字はggればでてくる)
        - シェルで用いられる関数が狙われるので、シェルを使用しない関数を使用する
        - WAF を導入する
- バッファオーバーフロー
    - バッファに想定以上の大きさのデータを送信することで、システムの停止や、それを利用した不正な挙動を目的とする攻撃
    - バッファ制御がプログラマ依存の C/C++ のプログラムで意図せず発生することがある
- XSS 攻撃
    - ユーザ入力値に html タグや js のコードを入力し、不正なタグやコードを含むサイトを作成する。その作成したサイトにほかのユーザを誘導し、不正なアクションを起こさせる
    - 情報処理推進機構調べによると、Web サイトで発見、報告された脆弱性の過半数が XSS だったらしい
- デバックオプション (暴露)
    - プログラマの開発効率化のために要したサイトのデバッグモードを活性化させる攻撃。
    - サイトの URL に `?debug=true` のようにすることで、サイトをデバッグモードに移行させ、サイト情報を不正に入手すること
- 強制ブラウズ (Forceful Browsing)
    - URL に細工したり、Cookie を改ざんすることで、認証をパスし不正アクセスする
- セッションハイジャック
    - 何らかの方法でセッションID を入手し、入手したセッションを使用するユーザの情報を抜き取ること
- パストラバーサル (Path Traversal)/ ディレクトリトラバーサル (Directory Traversal)
    - 「一つ上の階層へ上る」ことを指示する「../」のパスを組み合わせて指定することで、公開されているディレクトリの上階層から、非公開層にアクセスする攻撃
- Client Side Comment
    - HTML のコメントなど、ユーザが確認できるプログラムのコメントから、システムの脆弱性を探し出す方法
- Error Codes
    - エラーメッセージから、システムの構造、OS の種類など確認し、それを起点に攻撃する

## 防御いろいろ
- WAF
    - OSI 基本構造でいう、7層目(アプリケーション層) に対する攻撃(SQLインジェクション、XSS攻撃など) を防御する FW
    - WAF の種類
        - アプライアンス型 WAF
            - 既存のネットワークに専用のハードを設置する WAF。Web サーバの構造などに左右されないが、コストが高い。
        - ソフトウェア型 WAF
            - サーバにインストールするタイプの WAF。サーバ台数が多いとめんどくさいが、ランニングコストは低い
        - クラウド型 WAF
            - クラウド上にある、仮想アプライアンス型 WAF。導入時はネットワーク設定(DNS) を変更して利用する
            - 基本的にアプライアンス型だが、サーバ管理費がかからないので、ランニングコストも低め。
            - 性能評価がサービス提供者にゆだねられるので、見極めが必要

- IDS/IPS
    - OSI 基本構造でいう、5,4(トランス、ネットワーク)層に対する攻撃(DDoS, 盗聴, IPなりすまし) を防御する FW
    - IDS (Intrusion Detection System)
        - アプリとクライアントの通信を監視し、不正アクセスを**detect 感知**するシステム。
        - 異常な通信が発見されると、管理者に通知する。(これがトリガーとすることで、不正アクセスをブロックする)
    - IPS (Intrusion Prevention System)
        - 通信を監視し、不正アクセスを **prevent 回避**するように仕向けるシステム
        - 不正アクセスを感知するとその通信をブロックする
    - 主な違い
        - トリガーにしてブロックするか、ブロックしてもらうかの違い
        - IPS は通信そのものをブロックする(通信経路の間に入る)必要があるため、IDS ネットワーク構成が異なる場合がある
