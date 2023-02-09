---
title: "[Dash] 데이터 시각화하기"
excerpt: "BigData Camp 2월 8일"
categories: [Cpp]
tags: [cpp]
toc: true
toc_sticky: true
---

## 1. Dash
### Dash란?
dash는 프론트엔드로 plotly.js와 React를 사용하고 백엔드로 Flask를 사용하는 데이터 시각화 프레임워크
* plotly 그래프를 웹에서 표현할 수 있도록 하는 라이브러리
* dash를 사용하면 파이썬 하나만으로 데이터 분석 결과를 간편하게 웹 서비스로 구현할 수 있음
* HTML을 이용해 화면을 구성

### HTML 기초
모든 웹사이트의 메인 페이지가 되는 index.html
* 'google.com'에 접속하면 사실상 'google.com/index.html'에 접속하는 것 

## 2. Dash 사용하기
### Dash 시작하기
필요한 패키지 불러오기
```python
from dash import dcc, html, Dash
import plotly.express as px
import pandas as pd

app = dash.Dash(__name__)
```

샘플 데이터셋 만들고 바그래프를 이용하여 데이터 나타내기
```python
df = pd.DataFrame(
    {
        "Fruit": [
            "Apples",
            "Oranges",
            "Bananas",
            "Apples",
            "Oranges",
            "Bananas",
        ],
        "Amount": [4, 1, 2, 2, 4, 5],
        "City": ["SF", "SF", "SF", "Montreal", "Montreal", "Montreal"],
    }
)

# 데이터 나타내기
fig = px.bar(df, x="Fruit", y="Amount", color="City", barmode="group")
```

HTML요소 배치
```python
app.layout = html.Div(
    children=[
        html.H1(children='Hello Dash'),
        html.Div(
            children='''
        Dash: A web application framework for your data.
    '''
        ),
        dcc.Graph(id='example-graph', figure=fig),
    ]
)
```
* app.layout 내부에 html 요소 배치
* dcc.Graph 클래스를 이용하여 plotly 그래프 추가

서버를 실행하는 코드
```python
if __name__ == '__main__':
    app.run_server(debug=True)
```
* 기본으로 localhost:8050 에서 서버 시작
* debug=True 설정 시 디버그 모드로 시작

### Dash 어플리케이션의 두 가지 요소
1. 레이아웃: 페이지가 화면에 어떻게 나타나는지 정의
2. 콜백, 상호작용 요소

## 3-1. 레이아웃(Layout)
### 레이아웃이란?
Dash 어플레케이션을 구성하는 두가지 요소(layout, callback) 중 하나

레이아웃은 html모듈과 dcc모듈로 나뉨
* html 모듈: html 태그들을 나타냄
* dcc 모듈: 미리 만들어져 있는 고급 기능

[예시 코드]
```python
from dash import html

html.Div(
    [
        html.H1('Hello Dash'),
        html.Div(
            [
                html.P('Dash converts Python classes into HTML'),
                html.P(
                    "This conversion happens behind the scenes"
                    " by Dash's JavaScript front-end"
                ),
            ]
        ),
    ]
)
```

위의 코드는 아래와 같이 변환됨

```html
<div>
    <h1>Hello Dash</h1>
    <div>
        <p>Dash converts Python classes into HTML</p>
        <p>This conversion happens behind the scenes by Dash's JavaScript front-end</p>
    </div>
</div>
```

### CSS 적용하기
* 클래스 이름은 'className', 아이디는 'id'
* 프로퍼티는 style 파라미터에 입력(camelCase 규칙)

[예시 코드]
```python
from dash import html

html.Div([
    html.Div('Example Div', style={'color': 'blue', 'fontSize': 14}),
    html.P('Example P', className='my-class', id='my-p-element')
], style={'marginBottom': 50, 'marginTop': 25})
```

위의 코드는 아래와 같이 변환됨

```html
<div style="margin-bottom: 50px; margin-top: 25px;">
    <div style="color: blue; font-size: 14px">
        Example Div
    </div>
    <p class="my-class", id="my-p-element">
        Example P
    </p>
</div>
```

### dcc.Graph
dcc는 plotly 그래프를 표현하는데 사용

### 마크다운
HTML마크업 대신 마크다운 사용 가능

[예시 코드]
```python
from dash import Dash, html, dcc

markdown_text = '''
# h1 Heading
## h2 Heading
### h3 Heading

***

1. Lorem ipsum dolor sit amet
2. Consectetur adipiscing elit
3. Integer molestie lorem at massa
'''

app = Dash(__name__)
app.layout = html.Div([dcc.Markdown(markdown_text)])


if __name__ == '__main__':
app.run_server(debug=True)
```

### 여러 선택 요소
Dropdown
* option: 드롭다운에서 선택할 수 있는 선택지
* value: 기본적으로 선택되어 있는 값

[예시 코드]
```python
import dash
from dash import dcc, html

app = dash.Dash(__name__)

app.layout = html.Div(
    [
        html.Div(
            [
                html.Label('Dropdown'),
                dcc.Dropdown(
                    options=[
                        {'label': 'New York City', 'value': 'NYC'},
                        {'label': u'Montréal', 'value': 'MTL'},
                        {'label': 'San Francisco', 'value': 'SF'},
                    ],
                    value='MTL',
                ),
                html.Br(),
                html.Label('Multi-Select Dropdown'),
                dcc.Dropdown(
                    options=[
                        {'label': 'New York City', 'value': 'NYC'},
                        {'label': u'Montréal', 'value': 'MTL'},
                        {'label': 'San Francisco', 'value': 'SF'},
                    ],
                    value=['MTL', 'SF'],
                    multi=True,
                ),
            ],
            style={'padding': 10, 'flex': 1},
        ),
    ],
    style={'display': 'flex', 'flex-direction': 'row'},
)

if __name__ == '__main__':
    app.run_server(debug=True)
```

RadioItem, Input, slider, 버튼도 존재


## 3-2. 콜백(callbacks)
컴포넌트의 프로퍼티가 변경될 때마다 자동으로 호출되는 함수
* 컴포넌트의 상태에 따라 화면의 요소를 유기적으로 업데이트

[예제 코드]
```python
from dash import Dash, dcc, html, Input, Output

app = Dash(__name__)

app.layout = html.Div([
    html.H6("Change the value in the text box to see callbacks in action!"),
    html.Div([
        "Input: ",
        dcc.Input(id='my-input', value='initial value', type='text')
    ]),
    html.Br(),
    html.Div(id='my-output'),

])


@app.callback(
    Output(component_id='my-output', component_property='children'),
    Input(component_id='my-input', component_property='value')
)
def update_output_div(input_value):
    return f'Output: {input_value}'


if __name__ == '__main__':
    app.run_server(debug=True)
```