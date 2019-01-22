### 'CUDA Toolkit' 설치
nvidia 'cuda'설치.... 현재 9.0을 지원함.
https://developer.nvidia.com/cuda-zone

### 'cuDNN SDK' 설치
cuda 설치 후, nvidia의 developer에서 'cudnn-9.0.#' 다운 받아서 cuda 설치폴더에 덮어쓰기.
https://developer.nvidia.com/cudnn

### 'ANACONDA' 파이썬 버전 바꾸기
>> conda create -n py35 python=3.5 anaconda     #tensorflow가 파이썬 3.5에서 설치됨. 따라서 버전 낮추는 작업.
>> pip install tensorflow-gpu
>> python -m pip install --upgrade pip

--------------------------------------------------------------
>> conda search python   # 아나콘다 파이썬 버전 목록확인
>> conda create -n py36 python=3.6.6 anaconda   # 아나콘다 파이썬 'py36'이라는 이름으로 '3.6.6' 버전 만들기.

>> activate py36          # 3.6.6버전 활성화
>> deactivate py36       # 3.6.6버전 비활성화

>> conda install python=3.6.6     # 아예 기본 파이썬 버전을 바꾸기.
--------------------------------------------------------------

### 'Tensorflow GPU' 설치하기
>> pip install –upgrade tensorflow-gpu

```python
import tensorflow as tf
hello = tf.constant(;hello tensorflow gpu')
sess = tf.Session()
print(sess.run(hello))
```
