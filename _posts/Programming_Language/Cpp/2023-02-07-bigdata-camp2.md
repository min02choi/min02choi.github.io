---
title: "[Plotly] 그래프 그리기"
excerpt: "BigData Camp 2월 7일"
categories: [Cpp]
tags: [cpp]
toc: true
toc_sticky: true
---

## 1. Plotly
### Plotly란?
대화형 오픈 소스 플로팅 라이브러리
* 다양한 통계, 금융, 지리, 과학 및 3차원 사용 사례를 포괄하는 40개 이상의 고유 차트 유형을 지원
* 내부적으로 샘플 데이터셋을 포함하고 있음
* 주피터 노트북에 바로 표시하거나 별도의 HTML 파일에 저장 가능
* plotly와 함께 kaleido, pandas 라이브러리가 같이 사용됨
   * kaleido: 그림 저장 라이브러리
   * pandas: 데이터 분석 라이브러리

### Plotly API 레퍼런스
그 외 Plotly의 기능은 [여기](https://plotly.com/python-api-reference/)를 참고


## 2. Plotly로 다양한 그래프 그려보기
### Plotly express
Plotly 그래프들을 쉽고 간편하게 그릴 수 있도록 하는 모듈 <br/>
미리 만들어져 있는 기본 그래프들을 가져와 사용하는 느낌

```python
import pandas as pd
import plotly.express as px
```

### 산점도 그래프
Plotly가 내부적으로 포함하고 있는 샘플 데이터셋인 iris 데이터셋을 불러옴
* iris 데이터셋: 붓꽃 데이터셋
```python
iris = px.data.iris()
iris.head()
```

```python
fig = px.scatter(
    iris,
    x="petal_length",       # 원하는 칼럼의 이름
    y="petal_width",        # 원하는 칼럼의 이름
    color="species",        # 각 포인트의 색상
    size="sepal_length",

    hover_data=["sepal_width"], # 마우스로 해당 데이터 포인터를 가리켰을 떄 나타나는 툴팁 메세지를 어느 칼럼에서 가져올 지
    height=400,
    width=800,
)
fig.show()
```
* facet_: 특정 기준에 대해 여러 개의 그래프를 생성할 수 있음('산점도 차트'에서 잠시 설명)


그림 파일로 저장하기 위해서는 아래 코드를 마지막에 추가
```python
fig.write_image('iris_scatter.png')
```

### 매트릭스
앞서 사용한 붓꽃 데이터셋을 이어서 사용
```python
fig = px.scatter_matrix(
    iris,
    dimensions=[
        'petal_width',
        'petal_length',
        'sepal_width',
        'sepal_length',
    ],

    color='species',
    width=1000,
    height=1000
)
fig.show()
```

### 산점도 차트
갭마인더 데이터셋 사용
* 대륙별, 나라별, 연도별로 각 나라의 GDP와 인구 데이터를 포함하고 있는 데이터셋

```python
gapminder = px.data.gapminder()
gapminder.head()
```

2007년도 데이터에 한해서 사용
```python
df = gapminder[gapminder['year'] == 2007]
df.head()
```

[산점도 그래프 부가 설명]
```python
fig = px.scatter(
    df,
    x='gdpPercap',
    y='lifeExp',
    size='pop',
    color='continent',
    hover_name='country',
    log_x=True,         # 로그 스케일로 그림을 그린 것
    size_max=100,       # 가장 큰 셀의 크기를 설정
    width=1000
)
fig.show()
```
* log_x: 그래프의 x축을 로그 스케일로 나타내는 옵션

```python
fig = px.scatter(
    gapminder,
    x="gdpPercap",
    y="lifeExp",
    size="pop",
    color="continent",
    facet_col='year',       # 연도별로
    facet_col_wrap=6,       # 한 열에 6개 (word-wrap)
    log_x=True,
    width=1000,
)
fig.show()
```
* facet_col: 그래프를 그릴 기준 설정
* facet_col_wrap: 한 열에 몇 개의 그래프를 그릴지 설정

### 라인 플롯
선 그래프
  * 선과 마커에 스타일 별도 지정 가능
```python
fig = px.line(
    gapminder[gapminder['continent'] == 'Oceania'],
    x='year',
    y='lifeExp',
    color='country',
    hover_data=['country'],
    markers='o-',
    width=1000,
)
fig.show()
```

[마커 스타일 확인 방법]
```python
from plotly.validators.scatter.marker import SymbolValidator
import plotly.graph_objects as go

raw_symbols = SymbolValidator().values
namestems = []
namevariants = []
symbols = []

for i in range(0, len(raw_symbols), 3):
    name = raw_symbols[i + 2]
    symbols.append(raw_symbols[i])
    namestems.append(name.replace("-open", "").replace("-dot", ""))
    namevariants.append(name[len(namestems[-1]) :])
fig = go.Figure(
    go.Scatter(
        mode="markers",
        x=namevariants,
        y=namestems,
        marker_symbol=symbols,
        marker_line_color="midnightblue",
        marker_color="lightskyblue",
        marker_line_width=2,
        marker_size=15,
        hovertemplate="name: %{y}%{x}<br>number: %{marker.symbol}<extra></extra>",
    )
)
fig.update_layout(
    title="Mouse over symbols for name & number!",
    xaxis_range=[-1, 4],
    yaxis_range=[len(set(namestems)), -1],
    margin=dict(b=0, r=0),
    xaxis_side="top",
    height=1400,
    width=400,
)
fig.show()
```

### 파이 차트
tips 데이터셋 사용, 시각화에는 px.pie 함수 사용
```python
df = px.data.tips()
df.head()
```
```python
fig = px.pie(df, values="tip", names="day")
fig.show()
```

### (가제) 주가 차트: 시계열 데이터
외부 데이터셋 사용: 애플의 주가 데이터
```python
apple = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/finance-charts-apple.csv")
apple.head()
```
```python
fig = px.line(apple, x='Date', y='AAPL.Close', width=1000)
fig.show()
```
* 시계열 데이터이므로 시간을 잘 표현할 것

```python
# 일정 범위 선택
fig = px.line(
    apple, x='Date', y='AAPL.Close', range_x=['2016-03', '2017-03'], width=1000
)
fig.show()
```
* 기간을 지정하여 일정 기간의 그래프를 확인할 수 있음

```python
# 슬라이더 생성
fig = px.line(
    apple, x='Date', y='AAPL.Close', range_x=['2016-03', '2017-03'], width=1000
)
fig.update_xaxes(rangeslider_visible=True)
fig.show()
```
* 가로축에 슬라이더 생성 가능
* 슬라이더를 조절하여 원하는 범위의 그래프 확인 가능

### 맵
geojson의 미국 지도 정보를 다운로드 받아 사용
```python
import requests

response = requests.get(
    "https://raw.githubusercontent.com/plotly/datasets/master/geojson-counties-fips.json"
)
counties = response.json()      # request로 json데이터를 받아옴
counties["features"][0]
```
* 반환: json 파일
* 반환값 중 'id'엔트리가 'FIPS'라고 하는 식별자 -> 아래의 실업률 데이터와 연동

> request
> * 통신규격(http)을 사용해서 요청을 보냄(get -> 데이터를 받아온다)
> * 서버로부터 응답이 오는데 이를 변수 respose로 받음.

두 데이터를 한번에 지도애 표시
```python
fig = px.choropleth_mapbox(
    df,
    geojson=counties,
    locations='fips',
    color='unemp',
    color_continuous_scale="Viridis",
    range_color=(0, 12),
    mapbox_style="carto-positron",
    zoom=3,
    center={"lat": 37.0902, "lon": -95.7129},
    opacity=0.5,
    labels={'unemp': 'unemployment rate'},
)
fig.update_layout(margin={"r": 0, "t": 0, "l": 0, "b": 0})
fig.show()
```
* matbox: 외부 API

[에니메이션 기능 지원]
```python
px.scatter(
  gapminder,
  x="gdpPercap",
  y="lifeExp",
  animation_frame="year",
  animation_group="continent",
  size="pop",
  color="continent",
  hover_name="country",
  log_x=True,
  size_max=200,
  range_x=[100, 100000],
  range_y=[25, 90],
)
```

### Graph Object
plotly express는 사전에 만들어둔 간편한 프리셋이기 때문에, 세부적인 변경이 필요할 경우에는 Graph Object를 사용
```python
import plotly.graph_objects as go
```

Graph Object는 추후 정리...