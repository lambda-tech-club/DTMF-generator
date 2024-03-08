# DTMF Generator for SMS

ショートメッセージサービス用のDTMF信号を生成する

<a href="https://colab.research.google.com/github/yoidea/DTMF-generator/blob/main/DTMF_generator.ipynb"><img data-canonical-src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab" src="https://camo.githubusercontent.com/f5e0d0538a9c2972b5d413e0ace04cecd8efd828d133133933dfffec282a4e1b/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667"></a>

## Usage

`事前準備`、`初期設定`、`出力波形の生成処理`を実行してから目的に合わせて次に進む

### 電話番号の送信

- 電話番号を`generate_DTMF_wave`の引数に与えるとDTMF波形が得られる
- 半角数字以外の文字は無視される

```python
# 電話番号を入力
DIAL_NUMBER: str = "090-0123-4567"
# DTMF信号に変換
data = generate_DTMF_wave(DIAL_NUMBER)
# 信号送出
Audio(data, rate=SAMPLING_RATE, autoplay=True)
```

### メッセージの送信

- メッセージの文字列をメッセージ番号に変換して`generate_DTMF_wave`の引数に与える
- 半角カタカナ、半角英数、一部記号のみに対応
- 詳細仕様: https://www.docomo.ne.jp/service/sms/usage/

```python
# メッセージを入力（半角カタカナと半角英数、一部記号のみに対応）
MESSAGE: str = "ﾃｷｽﾄﾒﾂｾｰｼﾞ"
# メッセージ番号に変換
message_numbers = (CODE_TABLE[m] for m in normalized_message if m in CODE_TABLE)
# 仕様に合わせて先頭と末尾に番号追加
dial_number = "*2*2" + "".join(message_numbers) + "##"
# DTMF信号に変換
data = generate_DTMF_wave(dial_number)
# 信号送出
Audio(data, rate=SAMPLING_RATE, autoplay=True)
```

### メッセージの送信（全角やひらがなを使いたい場合）

- 全角やひらがなを使いたい場合は`jaconv`などを使い半角カタカナに変換すれば良い

```python
# メッセージを入力
MESSAGE: str = "テキストメッセージ"
# 半角カタカナに合わせて正規化
normalized_message = z2h(hira2kata(MESSAGE), kana=True, ascii=True, digit=True)
# メッセージ番号に変換
message_numbers = (CODE_TABLE[m] for m in normalized_message if m in CODE_TABLE)
# 仕様に合わせて先頭と末尾に番号追加
dial_number = "*2*2" + "".join(message_numbers) + "##"
# DTMF信号に変換
data = generate_DTMF_wave(dial_number)
# 信号送出
Audio(data, rate=SAMPLING_RATE, autoplay=True)
```
