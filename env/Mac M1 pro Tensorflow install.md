
인공지능 수업을 들으면서 python을 가지고 실습 과제가 있어 Macbook M1 에서 Tensor flow을 사용하라면 Miniforge를 설치해야해서 설치기를 아래와 같이 작성해 본다


1. m1 Mac Pro 기준으로 verison  문제로 아래와 같이 오류가 확인된다.
tensorflow 맞는 버전이 없다고 하는 것인데 , python 3.10 에 맞는 tensorflow 가 없어 오류가 뜨는 것으로 확인 된다.
![[Screen Shot 2022-09-24 at 9.15.06 PM.png]]



2. 무시하고 miniforge 설치는 가능하다.
```
brew install --cask miniforge
```

![[Screen Shot 2022-09-24 at 9.27.52 PM.png]]


3. Conda 환경 생성
```
conda create --name tensor-conda python=3.10
```
![[Screen Shot 2022-09-24 at 9.29.16 PM.png]]

5. Conda 환경 활성화
	1.Shell 환경에 따라 `source ~/.zshrc`  실행 후,  `conda activate tensor-conda` 실행할 수 있다.
![[Screen Shot 2022-09-24 at 9.35.17 PM.png]]

6.  경우에 따라선 pip 업그레이드가 필요하다.
```
pip install --upgrade pip

conda update -n base conda

conda update -all
```




```
오류나서 다음과 같이 정리하였다
#파이썬 3.10 으로 설치되어 있더라도 아래와 같이 3.9 으로 conda 실행 시, 자동으로 파이썬이 3.9으로 재설치 된다.

conda create --name tensor-conda python=3.9

#conda tensor flow 설치
conda activate tensor-conda

#conda 환경이 활성화 되어 있는 지 확인
conda env list

#안정성을 위해 2.7  설치하기
conda install -c apple tensorflow-deps=2.7.0

#tensorflow 설치하기
python -m pip install tensorflow-macos

#metal 설치하기
python -m pip install tensorflow-metal

#기타 라이브러리 설치
conda install jupyterlab pandas scikit-learn numpyt matplot typeguard python-flabuffers



#tf 데이터 셋 설치
pip install tensorflow_datasets


#jupyter notebook 실행
jupyter notebook
```

![[Screen Shot 2022-09-24 at 10.15.13 PM 1.png]]


![[Screen Shot 2022-09-24 at 10.19.24 PM.png]]
#참고
https://younsik-tech.tistory.com/14