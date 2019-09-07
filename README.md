# Chat_System

#News ・・・News apiから最新の記事を取得し、取得した記事の内容から発話を生成する
   ＃create_infos.ipynb
   *get_headlines(_category)・・・_categoryで指定されたカテゴリの最新記事を複数取得
   *get_ramdom_news(headline)・・・得られた最新記事からランダムに１つの記事を選択
   *select_normaselect_normalization_representative_notation(fstring)・・・KNPで得られた文節の正規化代表表記を抽出
   *select_dependency_structure(line)・・・文節の係り受け構造を抽出
   *get_gimonshi(bnst, bnst_dic)・・・文節からどの疑問詞に相当するかを判断
                            ex) ５時 に 行く　＝＞　５時　-> when   行く-> do
   *generate_knowledge(sentence)・・・文から得られる情報を抽出し、構造化
                            ex) ５時 に 行く　＝＞　{who:None, do:"行く", what:None, when:”5時”, where:None, how:None, whose:None}
   *create_infos(topic)・・・カテゴリから最新記事を1つ取得し、その記事から見出しと抽出した全ての情報を返す。
   *clean_sentence(sentence)・・・記号や絵文字を削除
   *confirm_parent_bnst(sentence)・・・係り受け構造を可視化
   
   #U_gimonshi.ipynb
   *question(bnst, bnst_dic)・・・ユーザが何を質問しているかを判断
   *generate_utterance(u_sen, all_infos)・・・ユーザの質問の答えとなる情報を探索する。答えがあれば応答を返す。
   
   
   
#seq2seq・・・Twitterから取得した22万対の対話データを用いて発話を生成する。生成方法はseq2se2 + attention
   #seq2seq_bert.ipynb ・・・単語の埋め込み表現にBertを用いている。
   Bertの次元数 = 768
   decoderの語彙数 = 16001
   入力される文の最大の長さ = 20（jumanで切った時の単語数）
   encoder, decoderのユニット数 = 1024
   decoderのembedding層の次元数 = 256
   バッチサイズ = 256
   
   #seq2seq_no_bert.ipynb ・・・Bertを用いていない。
   入力に用いるデータはSentencePieceで分かち書きしている。
   出力に用いるデータはJumanpp分かち書きしている
   encoderの語彙数 = 16001
   decoderの語彙数 = 16001
   入力される文の最大の長さ = 20
   encoder, decoderのユニット数 = 1024
   encoderのembedding層の次元数 = 256
   decoderのembedding層の次元数 = 256
   バッチサイズ = 256
   
   
#対話破綻・・・DBDCの対話データから、検索手法を用いて、ユーザーの発話に最も適切な応答を返す。
   Bertによって1つの文を768次元の分散表現にする。入力文とDBDCのシステム発話とのコサイン類似度を計算し、最も値が大きくなったシステム発話の次のユーザ発話(DBDCデータ内)をシステムの応答としている。
   
   *input_embedding(sentence)・・・入力文を分散表現へ変換
   *load_sys_utterance()・・・DBDCのシステム発話を全て分散表現へ変換
   *cos_sim(v1, v2)・・・2つのベクトルv1,v2のコサイン類似度を計算
   *search_utterance(u_utterance, embedding_dict)・・・入力文と最も似た意味を持つシステム発話を検索し、そのシステム発話に対するユーザ発話を入力文の応答として返す
