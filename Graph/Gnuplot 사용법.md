# Gnuplot 사용법

수학 함수나 실험 데이터의 그래프를 그리기 위한 프로그램. Apache Bench v3.2 으로 웹부하 테스트를 진행하고 있는데 이에 관한 데이터를 바탕으로 그래프를 그려보도록한다



#### 환경

- "Ubuntu" "18.04.5 LTS (Bionic Beaver)"
- 로드밸런서 환경으로 AB 테스트 진행





1. ab 명령어로 웹부하 데이터 추출, 결과 값은 result.plot 에서 확인할 수  있다.

```
ab -n 1000 -c 100 -g result.plot http://127.0.0.1/index.html
```



2. AB 테스트를 진행할 때,  `-g` 옵션을 사용하여 측정한 모든 값을 gnuplot 혹은 TSV 파일에 기록한다. 라벨은 output 파일의 첫 번째 라인을 참고하도록 한다.

```powershell
starttime       seconds ctime   dtime   ttime   wait
Tue Aug 30 14:27:39 2022        1661837259      3       6       9       6
Tue Aug 30 14:27:39 2022        1661837259      3       6       9       6
Tue Aug 30 14:27:39 2022        1661837259      3       6       9       6
Tue Aug 30 14:27:39 2022        1661837259      3       6       9       6
Tue Aug 30 14:27:39 2022        1661837259      3       6       9       6
Tue Aug 30 14:27:39 2022        1661837259      3       6       9       6
Tue Aug 30 14:27:39 2022        1661837259      3       6       9       6
Tue Aug 30 14:27:39 2022        1661837259      3       6       9       6
Tue Aug 30 14:27:39 2022        1661837259      3       7       9       7

```



>- **starttime** : request가 시작된 시간
>- **seconds** : starttime을 unix timestamp로 표현
>- **ctime** : connection 시간으로 request를 write하기 위해 서버와 socket을 여는 시간
>- **dtime** :  processing 시간 -> 결과를 반환받기 위해 wait하는 시간 + 서버 작업 시간 + 결과 반환 시간.
>  - dtime = ttime – ctime
>- **ttime** : request가 전체 수행된 시간(ttime = ctime + dtime)
>- **wait**: request를 보내고나서 response를 받기 전까지 서버사이드에서 처리되는 시간
>  - network 시간 = dtime – wait





3. vi script.slot 생성하기

```powershell
# 터미널 사이즈 조정(이미지 사이즈)
set terminal png size 1024,768

# 가로, 세로 비율
set size 1,0.5

# 결과 파일 설정
set output "result_229.png"

# 범례/key 위치
set key left top

# y축 grid line
set grid y

# Label the x-axis
set xlabel 'requests'

# Label the y-axis
set ylabel "response time (ms)"

# Tell gnuplot to use tabs as the delimiter instead of spaces (default)
set datafile separator '\t'

# Plot the data
plot "result_229.plot" every ::2 using 5 title 'response time' with lines
exit

```



4. 아래와 같이 설정 파일 실행 시, 현재 경로에 * .png 파일이 떨어진다.

```powershell
gnuplot script.plot
```



![img](https://github.com/jinsirie/Tools/blob/7274e03c371666e98e0a1d36daac5a3fafd35854/img%20env/result.png)







* Ver2 (시계열 그래프: Scatter chart)

![result_ver2](https://github.com/jinsirie/Tools/blob/7274e03c371666e98e0a1d36daac5a3fafd35854/img%20env/result_ver2.png)

```powershell
# 터미널 사이즈 조정(이미지 사이즈)
set terminal png size 1024,768

# 결과 파일 설정
set output "result.png"

# 범례/key 위치
set key left top

# y축 grid line
set grid y

# x축 데이터 지정
set xdata time

# input time format
set timefmt "%s"

# x축 time format 시:분:초
set format x "%H:%M:%S"

# Label the x-axis
set xlabel 'H:M:S'

# Label the y-axis
set ylabel "response time (ms)"

# 구분자로 탭을 사용
set datafile separator '\t'

# Plot the data
plot "result.plot" every ::2 using 2:5 title 'response time' with points

exit
```
*References : https://blog.hkwon.me/ab-apache-http-server-benchmarking-tool/

