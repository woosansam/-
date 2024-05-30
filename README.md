 음.. Attention is all you need의 모델을 구현해보았는데요. 망했습니다. loss 값이 크게 감소하지 않는걸 보아서는 epoch 수의 문제는 아닌듯한데 번역 결과가 모두 the로 나오는 결과가 나왔습니다. 모르겠습니다 딴 사람꺼 참고해서 다시 만들어볼듯요.............


<논문 정리>
Attention is all you need


Seq2Seq model => 입력된 문장을 하나의 context vector로 만듦 이 값을 디코더 부분에서 계속 반영해주어서 문장을 생성
				=> 문제점, 하나의 context vector에 전체 문장을 압축해야함 => 병목 문제, 성능 저하

그래서 해결방안 => 매번 소스(입력) 문장에서의 출력 전부를 입력으로 받는다는 아이디어
 ( Gpu는 빠른 병목 처리를 지원하기에 가능한 아이디아) => Seq2Seq with attention 디코더는 인코더의 모든 출력을 참고함 
 => context vector뿐 아니라 weight vector(문장 중 일부에 더 가중치를 줌)


본문

입력문장을 Embedding Metrix화 시키고 RNN을 사용하지 않았기에 Positional Encoding(같은 차원의 metrix) -> 입력 값 사이 Attention을 구함.(Self Attention) 그런데 이 네트워크에 Residual Learning 이라는 아이디어를 통해 보다 빠른 수렴속도를 보장함(잔여 학습) 이렇게 Attention과 Nomalization 를 중첩하여 반복함 => 나온 vector를 디코더에 지속적으로 반응 


=> Multi-layer attention 입력값은 Query, key, value 3개 존재 
-쿼리 - 무엇을 물어보는 주체 -> i
-키 - 물음을 받는 -> i am a teather
-값 -> 연산 과정으로 최종 Attention value 값을 구할 수 있음

=> input, output dimension이 동일해서 반복적으로 학습이 가능함


총 3가지 Attention이 사용된다.
-Encoder self attention
-Masked Decoder self attention => 생성자이기에 뒷부분의 내용을 반영하지 않아야함
-Encoder-Decoder attention

Positional Encoding은 sin과 같은 주기함수를 사용하여 model에게 각 문장 단어의 position을 전달함
