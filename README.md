# KO-EN_Translation
 Machine Translation Task를 수행하기 위해 모델을 한국어-영어 병렬 말뭉치로 fine-tuning하여 성능을 확인한다. 성능 검증은 테스트 데이터셋의 ROUGE score와 간단한 문장입력으로 구글 번역기와 모델의 출력을 비교해볼 것이다.

## 1. 알고리즘 순서도
<table border ="0">
    <tr>
      <td><img src="https://user-images.githubusercontent.com/86700191/233922133-5fe1675c-2442-4d58-bead-65f112b1fd34.png" width="100%" height="100%"></td>
      <td><img src="https://user-images.githubusercontent.com/86700191/233922140-bb9f60fa-2426-49ec-8ae5-4c39d8dcca61.png" width="100%" height="100%"></td>
    </tr>
    <tr>
      <td align ="center">KO-EN_Translation.ipynb</td>
      <td align ="center">KO-EN_Translation_Inference.ipynb</td>
</tr>
</table>

## 2. 결과
- Test 데이터셋에 대한 ROUGE 점수<br>
<table border ="0">
    <tr align="center">
      <th></th><th>ROUGE-1</th><th>ROUGE-2</th><th>ROUGE-L</th>
    </tr>
    <tr align="center">
      <th>Precision</th><td>0.428</td><td>0.177</td><td>0.353</td>
    </tr>
    <tr align="center">
      <th >Recall</th><td>0.392</td><td>0.161</td><td>0.324</td>
    </tr>
</table>

- 문장 번역 (Korpora 소개글의 첫 문단) <br>
<table border ="0">
  <tr>
      <th align="center">한글 원문</th><td>최근 자연어 처리에 관심이 높아지면서 정부와 기업은 물론 뜻있는 개인에 이르기까지 데이터를 무료로 공개하는 추세입니다.</td>
  </tr>
  <tr>
      <th align="center">영어 원문</th><td>Due to the growing interest in natural language processing, governments, businesses, and individuals are disclosing their data for free.</td>
  </tr>
  <tr>
      <th align="center">모델 결과</th><td>The government, and other people are getting the data free, as the current interest is increasing in the language process.</td>
  </tr>
  <tr>
      <th align="center">구글 결과</th><td>Recently, as interest in natural language processing has increased, governments and corporations, as well as willing individuals, are making their data available free of charge.</td>
  </tr>
</table>
<table border ="0">
  <tr>
      <th align="center">한글 원문</th><td>하지만 데이터가 곳곳에 산재해 있다보니 품질 좋은 말뭉치임에도 그 존재조차 잘 알려지지 않은 경우가 많습니다.</td>
  </tr>
  <tr>
      <th align="center">영어 원문</th><td>However, even for a high-quality corpus, its existence is often unknown as datasets are scattered in different locations.</td>
  </tr>
  <tr>
      <th align="center">모델 결과</th><td>But it's often unknown even if it's a high-profile clown, as the data is out there.</td>
  </tr>
  <tr>
      <th align="center">구글 결과</th><td>However, since data is scattered all over the place, there are many cases where even its existence is not well known even though it is a high-quality corpus.</td>
  </tr>
</table>
<table border ="0">
  <tr>
      <th align="center">한글 원문</th><td>파일 포맷과 저장 형식 등이 각기 달라 사용이 쉽지 않습니다.</td>
  </tr>
  <tr>
      <th align="center">영어 원문</th><td>Furthermore, each of their file or saved format is often different, making it even more difficult to use them.</td>
  </tr>
  <tr>
      <th align="center">모델 결과</th><td>It's not easy to use, different from file formats and storage forms.</td>
  </tr>
  <tr>
      <th align="center">구글 결과</th><td>It is not easy to use because the file format and storage format are different.</td>
  </tr>
</table>
<table border ="0">
  <tr>
      <th align="center">한글 원문</th><td>개별 사용자들은 다운로드나 전처리 코드를 그때그때 개발해서 써야 하는 수고로움이 있습니다.</td>
  </tr>
  <tr>
      <th align="center">영어 원문</th><td>Therefore, individuals need to painstakingly create download or preprocessing codes for every instance.</td>
  </tr>
  <tr>
      <th align="center">모델 결과</th><td>Individuals have a complaint for developing download code or pre-process code at the time.</td>
  </tr>
  <tr>
      <th align="center">구글 결과</th><td>Individual users have the trouble of developing and writing downloads or pre-processing codes at any time.</td>
  </tr>
</table>

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

- M2M100 <br>
M2M100은 translation task를 위한 multilingual encoder-decoder (seq-to-seq) 모델이다. 기존 단일 모델들은 모든 언어 쌍 간에 번역할 수 있도록 교육함으로써 대규모 다국어 기계 번역을 수행할 수 있게 되었다.
하지만 이런 모델들은 주로 영어에서 타 언어 혹은 타 언어에서 영어로 번역된 데이터서만 훈련함으로써 영어 중심적이게 되었고, 이에 반해 M2M100은 100개 언어 쌍 간에 직접 번역할 수 있는 진정한 Many-to-Many multilingual translation model이라고 소개되어있다.
  - Source & Target Text Format <br>
MBart와 다르게 Source Text와 Target Text의 Format이 동일하다. Format은 [lang_code] X [eos]이며 lang_code는 언어 ID 토큰이며, X는 Source 혹은 Target Text이다. 
  - Total parameters <br>
M2M100은 두가지의 모델로 facebook/m2m100_418M, facebook/m2m100_1.2B이 있다. 뒤 숫자는 파라미터의 개수를 나타내며, 사용한 모델은 MBart보다 모델의 크기, 복잡성이 더 작은 모델을 지향하기에 facebook/m2m100_418M을 사용하였다.

## 4. 사용 모델과 데이터 및 참고링크
- [KoBART](https://github.com/SKT-AI/KoBART)
- [MBart & MBart-50](https://huggingface.co/docs/transformers/model_doc/mbart)
- [M2M100](https://huggingface.co/docs/transformers/model_doc/m2m_100)
- [한국어-영어 병렬 말뭉치](https://github.com/jungyeul/korean-parallel-corpora)
- [KoBART Translation Example](https://github.com/seujung/KoBART-translation)
- [Gradient Accumulation](https://kozodoi.me/blog/20210219/gradient-accumulation)
