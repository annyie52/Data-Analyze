

#  Youtube 데이터 분석하기

## 기본 설정

```python
!sudo apt-get install -y fonts-nanum
!sudo fc-cache -fv
!rm ~/.cache/matplotlib -rf
```

```python
import matplotlib.pyplot as plt
plt.rc('font', family='NanumBarunGothic')
%config InlineBackend.figure_format = 'retina'
```

```python
FILE_PATH = "/content/drive/MyDrive/파일상세경로명"
```



시각화를 할 때는 **한글 시각화가 잘 되는지 확인해 보는 게 좋다.**

```python
plt.plot([0, 1], [1, 0], label="한글 테스트용")
plt.legend()
plt.show()
```



## 데이터 로딩

```python
import pandas as pd
import seaborn as sns
```

```python
KRVideo = pd.read_csv(FILE_PATH)
KRVideo.head()
```

```python
KRVideo.isnull().sum()
```

```python
KRVideo.info()
```



## Youtube 인기 채널 분석

### 1. 필요 컬럼 추리기

```python
cols = ["title", "channelTitle", "view_count"]
df = KRVideo[cols]
df.head()
```



### 2. 조회 수 내림차순 정렬

```python
df_sorted = df.sort_values(by="view_count", ascending=False)
df_sorted.head()
```

```python
df_sorted.tail()
```



### 3. 중복 제거

조회수가 제일 많았던 동영상만 남기고 다 삭제

```python
# 채널 제목과 영상 제목이 같으면 삭제
df_drop_sorted = df_sorted.drop_duplicates(["title", "channelTitle"], keep='first')
df_drop_sorted
```



### 4. 인기 채널 순위 구하기

**채널별 조회수 합계**

```python
df_channel_view_sum = df_drop_sorted.groupby(["channelTitle"]).sum()
df_channel_view_sum
```

```python
df_channel_view_sorted = df_channel_view_sum.sort_values(by="view_count", ascending=False)
df_channel_view_sorted
```



### 5. top 100 채널 시각화

```python
df_top100 = df_channel_view_sorted[:100]
```

```python
plt.figure(figsize=(20, 8))

sns.barplot(x=df_top100.index, y="view_count", data=df_top100)
plt.xticks(rotation=90)
plt.show()
```

