# KO-EN_Translation
 Machine Translation Task를 수행하기 위해 KoBART를 한국어-영어 병렬 말뭉치로 fine-tuning하여 성능을 확인한다. 성능 검증은 테스트 데이터셋의 ROUGE score와 간단한 문장입력으로 구글 번역기와 모델의 출력을 비교해볼 것이다.

## 1. 알고리즘 순서도

## 2. 결과

## 3. 주의점
- KoBART의 vocab <br>
KoBART는 40GB 이상의 한국어 텍스트에 대해서 학습한 모델이라고 소개되어있다. 이렇게 많은 데이터양을 학습할때 영어도 포함되어 있을 것이라는 생각과 함께 translation task를 수행하려 했으나 vocab에 등록된 영어는 단어가 아닌 알파벳과 접두사, 접미사 등만 있는 것을 확인했다.<br>
아래는 KoBART와 ALBERT의 동일문장의 토큰을 디코딩한 결과로 길이차이가 심하다는 것을 알 수있다.(KoBART의 경우 공백토큰을 넣어 단어가 형성이 안된다는 것을 알수있게 해놓았다.)<br>
<table border ="0">
    <tr>
      <td><img src="https://user-images.githubusercontent.com/86700191/223634558-2d43eb45-ffdc-401a-8546-b49b2cc97cf1.PNG" width="100%" height="100%"></td>
      <td><img src="https://user-images.githubusercontent.com/86700191/223634552-dd6b6c54-98fa-4fba-ac8f-dd0e9bacb007.PNG" width="100%" height="100%"></td>
    </tr>
    <tr>
      <td align ="center">KoBART</td>
      <td align ="center">ALBERT</td>
    </tr>
</table>

## 4. 사용 모델과 데이터 및 참고링크
- [KoBART](https://github.com/SKT-AI/KoBART)
- [한국어-영어 병렬 말뭉치](https://github.com/jungyeul/korean-parallel-corpora)