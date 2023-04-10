# KO-EN_Translation
 Machine Translation Task를 수행하기 위해 모델을 한국어-영어 병렬 말뭉치로 fine-tuning하여 성능을 확인한다. 성능 검증은 테스트 데이터셋의 ROUGE score와 간단한 문장입력으로 구글 번역기와 모델의 출력을 비교해볼 것이다.

## 1. 알고리즘 순서도

## 2. 결과

## 3. 문제점 & 주의점
- KoBART의 vocab <br>
KoBART는 40GB 이상의 한국어 텍스트에 대해서 학습한 모델이라고 소개되어있다. 이렇게 많은 데이터양을 학습할때 영어도 포함되어 있을 것이라는 생각과 함께 translation task를 수행하려 했으나 vocab에 등록된 영어는 단어가 아닌 알파벳과 접두사, 접미사 등만 있는 것을 확인했다. 
따라서, 수많은 영어단어를 학습처리하기에 힘들 것으로 판단하여 Multilingual Model을 찾기 시작했다.<br>
아래는 KoBART와 ALBERT의 동일문장의 토큰을 디코딩한 결과로 길이차이가 심하다는 것을 알 수있다.(KoBART의 경우 공백토큰을 넣어 단어가 형성이 안된다는 것을 알수있게 해놓았다.)
<br><table border ="0">
    <tr>
      <td><img src="https://user-images.githubusercontent.com/86700191/223634558-2d43eb45-ffdc-401a-8546-b49b2cc97cf1.PNG" width="100%" height="100%"></td>
      <td><img src="https://user-images.githubusercontent.com/86700191/223634552-dd6b6c54-98fa-4fba-ac8f-dd0e9bacb007.PNG" width="100%" height="100%"></td>
    </tr>
    <tr>
      <td align ="center">KoBART</td>
      <td align ="center">ALBERT</td>
    </tr></table>

- MBart <br>
MBart는 BART objective를 이용하여 여러 언어로 된 대규모 단일 언어 말뭉치에 대해 pretrain된 sequence-to-sequence denoising auto-encoder이다. 선택이유로는 기존 Bart의 구성으로 한국어, 영어에 대해 학습이 되어있으며, MBart의 Tokenizer는 KoBART와 달리 두 언어에 대해 단어단위로 Tokenizing해주는 것을 확인하였기 때문이다. <br>
    - Source & Target Text Format <br>
Source Text의 Format은 X [eos, src_lang_code], Target Text의 Format은 [tgt_lang_code] X [eos]이다(X는 각각 Source text, Target text). 특이한 점으로는 어떤 언어인지 확인하기 위해 lang_code가 들어간다는 점이다(한국어: ko_KR, 영아: en_XX).
    - 메모리 부족현상 <br>
동일한 데이터로 KoBART를 fine-tuning을 할 때와 달리 MBart 사용시 CUDA out of memory 에러가 계속 떴다. 이를 해결 하기 위해 batch size를 줄이는 방법에 Gradient Accumulation을 사용하여 이를 해결하려 하였으나 batch size가 2임에도 학습 실행이 안정적이지 않았다. <br>
따라서, 데이터의 크기 문제도 있겠지만 데이터의 수를 줄이는 것보단 모델의 크기나 복잡성이 더 적은 모델을 찾아 사용해보는 것이 더 낫다고 판단하였다.(MBart는 large모델 밖에 없다는 점도 아쉬운 점이다.)
<br><table border ="0">
  <tr align ="center">
    <th width = "200">모델</th>
    <td width = "200">MBart</td>
    <td width = "200">BERT-large</td>
    <td width = "200">RoBERTa-large</td>
    <td width = "200">BART-large</td>
  </tr>
  <tr align ="center">
    <th>파라미터 수</th>
    <td>610M</td>
    <td>340M</td>
    <td>355M</td>
    <td>400M</td>
  </tr></table>

## 4. 사용 모델과 데이터 및 참고링크
- [KoBART](https://github.com/SKT-AI/KoBART)
- [MBart & MBart-50](https://huggingface.co/docs/transformers/model_doc/mbart)
- [한국어-영어 병렬 말뭉치](https://github.com/jungyeul/korean-parallel-corpora)
- [KoBART Translation Example](https://github.com/seujung/KoBART-translation)
- [Gradient Accumulation](https://kozodoi.me/blog/20210219/gradient-accumulation)