---
title: "Certified Kubernetes Security Specialist \"不\"合格体験記"
emoji: "☔"
type: "idea"
topics: ["kubernetes"]
published: true
---

## TL;DR

自称Cloud Native Security Architectが、[Certified Kubernetes Security Specialist](https://training.linuxfoundation.org/certification/certified-kubernetes-security-specialist/)（以下、CKS）を受験しました。合格ライン67%に対し、1回目62%、2回目64%とあと一歩届きませんでした。準備してきたことをふりかえり、再挑戦に向けた備忘録とします。

## 想定読者

* 半年〜1年後にCKS再挑戦する私
* CKA/CKAD受験経験者

CKA/CKAD/CKSに共通する試験フォーマットや注意事項は割愛します。

## whoami

レベル感の参考になればと思い、簡単に自己紹介します。

* SOC（Security Operation Center）のメンバーとして活動
* Kubernetesのプロダクション運用経験なし
* 社内のKubernetes as a Serviceプラットフォームのセキュリティ対策立案、推進
  * Kubernetes audit policy作成
  * Sysdigによるセキュリティ監視、Falco Rulesのカスタマイズ
* 業務外で、[Kubernetes公式ドキュメント翻訳](https://kubernetes.io/ja/docs/home/)レビュアーを1年ほど経験（病気療養のため離脱中）


## 結果

|受験日|区分|スコア|
|----|----|----:|
|2020/11/17|Simulator（後述）|1%|
|2020/11/??|Simulator|43%|
|2020/11/27|本番|62%|
|2020/12/31|Simulator|77%|
|2021/01/05|本番|64%|

![](https://storage.googleapis.com/zenn-user-upload/rjqfhgacuzjzktj7vsu5q69x8isa)

![](https://storage.googleapis.com/zenn-user-upload/kdo8ij07bp8pi015ruoucaaulaep)

## 準備

7月に[CNCFのブログ](https://www.cncf.io/blog/2020/07/15/certified-kubernetes-security-specialist-cks-coming-in-november/)で発表されたときから、Kubernetes大好きなセキュリティエンジニアとしては絶対に合格するぞと意気込んでいました。ところが、[カリキュラム](https://github.com/cncf/curriculum)を見ても、どのような問題が出るのか、どのような対策をすればよいのか、まったく見当がつきませんでした。

#### CKS Simulator

https://killer.sh/cks

* 本番同様120分で、本番よりやや難しいとされる22問に挑戦できる
* 同一の問題を2回受験できる
* 受験後36時間以内は、試験環境にアクセスして復習できる

11月のCKS GA前後に見つけて登録しました。ただし、後述の**Udemy講座を購入するとSimulatorを利用できます。3回以上挑戦したい場合などを除き、Simulator単体を購入する必要はありません。**

1回目は、複数の設問で共用するクラスターを復旧不可能にしてしまい、ひどいありさまでした。
`/etc/kubernetes/manifests/`配下のYAMLを編集すると`kubelet`が自動的にStaticPodを再起動してくれることを知らず、VMを`reboot`したところ`kube-apiserver`が起動しなくなりました・・・

2回目の後すぐエラーで試験環境にアクセスできなくなりました。復習に使えないと困っていたところ、数日後に未使用のセッション（受験単位をセッションと呼んでいます）が2つ増えていました！

こちらの復習をして1回目の本試験に挑みました。

#### Kubernetes CKS 2021 Complete Course + Simulator

https://www.udemy.com/course/certified-kubernetes-security-specialist

CKS Simulatorの作者が講師をしているUdemy講座です。**購入する場合は、DISCOUNT CODEの入力を忘れないようにしましょう。**

#### Kubernetes完全ガイド

https://www.amazon.co.jp/dp/B08FZX8PYW/

言わずとしれた名著です。Udemyで学習した内容のフォローアップとして部分的に読みました。

#### Docker/Kubernetes開発・運用のためのセキュリティ実践ガイド

https://www.amazon.co.jp/dp/B085C8LYDC/

発売当時に通読していましたが、こちらも部分的に読み直しました。

#### OPA Policy Authoring

https://academy.styra.com/courses/opa-rego

[kubenews #4](https://kubenews.connpass.com/event/199270/)で紹介されていた無料トレーニングを完走しました。CKS本番ではOPAのドキュメントを参照できないことを考慮すると、あまり難しい問題は出ない気がしますので、ここまでしなくてもよさそうです。私は前々からOPAは気になっていたものの取っ掛かりがつかめないという状態でしたので、これを機に受けてみました。

## 試験登録のタイミング

例年、Linux Foundationのトレーニングと試験はBlack FridayまたはCyber Mondayでセールが開催されます。Course + Exam、CKA Exam + CKS ExamでCKS単体より安くなるはずですので、急いでいないのであれば11月末から12月頭頃のセールを待つとよいでしょう。

![](https://storage.googleapis.com/zenn-user-upload/loqo68r11r9ee35jjx9tgoajmdy6)

CKS公開直後はCourse + Examのバンドルがなかったため、セールを待たずにCKS単体で$300払ってしまいました。1~2週間待てばよかったです。

## 合格体験記

https://tetsuya-isogai.medium.com/certified-kubernetes-security-specialist-cks-%E8%A9%A6%E9%A8%93%E3%81%AEfeedback-c732b6e2deaa

これぐらいしか見当たりませんでした。

## Tips

`kubectl`コマンドでリソースを作成する際、1からYAMLを書き始めるのではなく`-o yaml --dry-run=client`を活用して雛形を作成するのがよいです。Simulatorの解説で紹介されています。

```
kubectl run nginx --image=nginx -o yaml --dry-run=client > pod.yaml
kubectl create deploy nginx --image=nginx -o yaml --dry-run=client > deploy.yaml
```

## おわりに

不合格は残念ですが、CKSはCKA/CKADよりはるかに難易度が高く、実践的な試験で楽しかったです。セールまで待つか、鉄は熱いうちにたたいてしまうか悩ましいですが、絶対に再挑戦します！

