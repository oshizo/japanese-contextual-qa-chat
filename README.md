# japanese-contextual-qa-chat

抽出型QAのデータセット[JSQuad](https://github.com/yahoojapan/JGLUE)と[JaQuAD](https://github.com/SkelterLabsInc/JaQuAD)の回答を、チャットボットの回答のような形式に変換したデータです。

変換には[cyberagent/calm2-7b-chat](https://huggingface.co/cyberagent/calm2-7b-chat)を利用し、7-shotで生成を行いました。

生成パラメータや、出力したテキストのフィルタリング条件については```process_jaquad_calm2.ipynb```を確認してください。

## データ形式

JSQuADの形式をそのまま利用し、answersのリスト要素のdictに"text_chat"キーを追加しました。

JSQuAD train-v1.1.json
```
{
    "data": [
        {
            "title": "造語",
            "paragraphs": [
                {
                    "qas": [
                        {
                            "question": "新たに語（単語）を造ることや、既存の語を組み合わせて新たな意味の語を造ること",
                            "id": "a1000888p0q0",
                            "answers": [
                                {
                                    "text": "造語",
                                    "answer_start": 0
                                }
                            ],
```

jsquad-train-chat.json
```
{
    "data": [
        {
            "title": "造語",
            "paragraphs": [
                {
                    "qas": [
                        {
                            "question": "新たに語（単語）を造ることや、既存の語を組み合わせて新たな意味の語を造ること",
                            "id": "a1000888p0q0",
                            "answers": [
                                {
                                    "text": "造語",
                                    "answer_start": 0,
                                    "text_chat": "新たに語（単語）を造ることは「造語」と呼ばれます。"
                                }
                            ],
```


## License

データファイルはそれぞれ元のデータのライセンスに従います。

yahoojapan/JGLUE
> This work is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.

SkelterLabsInc/JaQuAD
> The JaQuAD dataset is licensed under the [CC BY-SA 3.0] (https://creativecommons.org/licenses/by-sa/3.0/) license.


## 生成テンプレート

```
TMPL = """

USER: 質問への回答をチャット形式に変換せよ
質問: 日本では改名には何の許可が必要ですか？
回答: 家庭裁判所
ASSISTANT: 日本では、改名するには家庭裁判所の許可が必要です。<|endoftext|>

USER: 質問への回答をチャット形式に変換せよ
質問: モーリシャスの信仰で最も多いのは
回答: ヒンドゥー教
ASSISTANT: モーリシャスで最も信仰されている宗教はヒンドゥー教です。<|endoftext|>

USER: 質問への回答をチャット形式に変換せよ
質問: 核反応を利用した電池を何と呼ぶ
回答: 「原子力電池」
ASSISTANT: 核反応を利用した電池は「原子力電池」と呼ばれます。<|endoftext|>

USER: 質問への回答をチャット形式に変換せよ
質問: カール・マルクスの妻イェニーが、亡くなったのはいつか。
回答: 1881年12月2日
ASSISTANT: イェニーは1881年12月2日に亡くなりました。<|endoftext|>

USER: 質問への回答をチャット形式に変換せよ
質問: 北京市、上海市、深圳市と共に何に分類されている？
回答: 一線都市
ASSISTANT: 北京市は、一線都市に分類されています。<|endoftext|>

USER: 質問への回答をチャット形式に変換せよ
質問: 武田薬品工業の取締役は何名か
回答: 13名
ASSISTANT: 取締役は13名で構成されています。<|endoftext|>

USER: 質問への回答をチャット形式に変換せよ
質問: 辺野古移設のための埋め立て工事に賛成何%？
回答: 18.99%
ASSISTANT: 18.99%の人が埋め立て工事に賛成しています。<|endoftext|>

USER: 質問への回答をチャット形式に変換せよ
質問: {q}
回答: {a}
ASSISTANT: """
```

