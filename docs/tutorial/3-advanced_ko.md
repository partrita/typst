---
description: Typst 튜토리얼.
---

# 고급 스타일링
이 튜토리얼의 이전 두 장에서는 Typst에서 문서를 작성하고 서식을 변경하는 방법을 배웠습니다. 지난 두 장에 걸쳐 작성한 보고서는 A+를 받았고 지도교수님은 이를 바탕으로 학회 논문을 작성하고 싶어 하십니다! 물론 보고서는 학회 스타일 가이드를 준수해야 합니다. 이를 달성하는 방법을 살펴보겠습니다.

시작하기 전에 팀을 만들고 지도교수님을 초대하여 팀에 추가해 보겠습니다. 편집기 왼쪽 상단의 뒤로 가기 아이콘을 사용하여 앱 대시보드로 돌아가서 이 작업을 수행할 수 있습니다. 그런 다음 왼쪽 도구 모음에서 더하기 아이콘을 선택하고 팀을 만듭니다. 마지막으로 새 팀을 클릭하고 팀 이름 옆의 '팀 관리'를 클릭하여 설정으로 이동합니다. 이제 이메일로 지도교수님을 초대할 수 있습니다.

![팀 설정](3-advanced-team-settings.png)

다음으로 프로젝트를 팀으로 옮깁니다. 프로젝트를 열고 왼쪽 도구 모음에서 톱니바퀴 아이콘을 선택한 다음 소유자 드롭다운에서 새 팀을 선택하여 설정으로 이동합니다. 변경 사항을 저장하는 것을 잊지 마십시오!

이제 지도교수님도 프로젝트를 편집할 수 있으며 두 분 모두 실시간으로 변경 사항을 볼 수 있습니다. [Discord 서버](https://discord.gg/2uDybryKPe)에 가입하여 다른 사용자를 찾고 함께 팀을 사용해 볼 수 있습니다!

## 학회 가이드라인 { #guidelines }
레이아웃 가이드라인은 학회 웹사이트에서 확인할 수 있습니다. 살펴보겠습니다.

- 글꼴은 11pt 세리프 글꼴이어야 합니다.
- 제목은 17pt 굵게 표시되어야 합니다.
- 논문에는 단일 열 초록과 2열 본문 텍스트가 포함됩니다.
- 초록은 가운데 정렬되어야 합니다.
- 본문 텍스트는 양쪽 정렬되어야 합니다.
- 1단계 섹션 제목은 13pt, 가운데 정렬되어야 하며 작은 대문자로 렌더링되어야 합니다.
- 2단계 제목은 런인, 기울임꼴이며 본문 텍스트와 동일한 크기여야 합니다.
- 마지막으로 페이지는 미국 편지 크기여야 하며 바닥글 중앙에 번호가 매겨지고 각 페이지의 오른쪽 상단에는 논문 제목이 포함되어야 합니다.

이러한 작업 중 많은 부분을 이미 수행하는 방법을 알고 있지만 일부 작업에 대해서는 몇 가지 새로운 기술을 배워야 합니다.

## 올바른 설정 규칙 작성 { #set-rules }
문서에 대한 몇 가지 설정 규칙을 작성하는 것부터 시작하겠습니다.

```example
#set page(
>>>  margin: auto,
  paper: "us-letter",
  header: align(right)[
    빙하 흐름에 대한 유체 역학 모델
  ],
  numbering: "1",
)
#set par(justify: true)
#set text(
  font: "Libertinus Serif",
  size: 11pt,
)

#lorem(600)
```

여기서 일어나는 일의 대부분은 이미 익숙합니다. 텍스트 크기를 `{11pt}`로 설정하고 글꼴을 Libertinus Serif로 설정합니다. 또한 단락 양쪽 정렬을 활성화하고 페이지 크기를 미국 편지로 설정합니다.

`header` 인수는 새로운 것입니다. 이를 통해 모든 페이지의 상단 여백을 채울 내용을 제공할 수 있습니다. 머리글에는 학회 스타일 가이드에서 요청한 대로 논문 제목을 지정합니다. `align` 함수를 사용하여 텍스트를 오른쪽으로 정렬합니다.

마지막으로 중요한 것은 `numbering` 인수입니다. 여기서는 페이지 번호 매기기 방법을 정의하는 [번호 매기기 패턴]($numbering)을 제공할 수 있습니다. `{"1"}`로 설정하면 Typst는 페이지 번호만 표시합니다. `{"(1/1)"}`로 설정하면 현재 페이지와 총 페이지 수가 괄호로 묶여 표시됩니다. 그리고 원하는 대로 서식을 지정하기 위해 완전히 사용자 지정 함수를 제공할 수도 있습니다.

## 제목 및 초록 만들기 { #title-and-abstract }
이제 제목과 초록을 추가해 보겠습니다. 제목부터 시작하겠습니다. 가운데 정렬하고 `[*별표*]`로 묶어 글꼴 두께를 늘립니다.

```example
>>> #set page(width: 300pt, margin: 30pt)
>>> #set text(font: "Libertinus Serif", 11pt)
#align(center, text(17pt)[
  *빙하 흐름에 대한 유체 역학 모델*
])
```

이것은 올바르게 보입니다. `text` 함수를 사용하여 이전 텍스트 설정 규칙을 로컬에서 재정의하여 함수의 인수에 대해 크기를 17pt로 늘렸습니다. 저자 목록도 추가해 보겠습니다. 이 논문을 지도교수님과 함께 작성하고 있으므로 우리 이름과 교수님 이름을 추가하겠습니다.

```example
>>> #set page(width: 300pt, margin: 30pt)
>>> #set text(font: "Libertinus Serif", 11pt)
>>>
>>> #align(center, text(17pt)[
>>>   *빙하 흐름에 대한 유체 역학 모델*
>>> ])
#grid(
  columns: (1fr, 1fr),
  align(center)[
    테레즈 텅스텐 \
    아르토스 연구소 \
    #link("mailto:tung@artos.edu")
  ],
  align(center)[
    존 도 박사 \
    아르토스 연구소 \
    #link("mailto:doe@artos.edu")
  ]
)
```

두 저자 블록은 서로 옆에 배치됩니다. 이 레이아웃을 만들기 위해 [`grid`] 함수를 사용합니다. 그리드를 사용하면 각 열의 크기와 각 셀에 들어갈 내용을 정확하게 제어할 수 있습니다. `columns` 인수는 [상대 길이]($relative) 또는 [분수]($fraction) 배열을 사용합니다. 이 경우 동일한 두 개의 분수 크기를 전달하여 사용 가능한 공간을 두 개의 동일한 열로 나누도록 지시했습니다. 그런 다음 그리드 함수에 두 개의 콘텐츠 인수를 전달했습니다. 첫 번째는 우리 세부 정보이고 두 번째는 지도교수님 세부 정보입니다. 다시 `align` 함수를 사용하여 열 내에서 콘텐츠를 가운데 정렬합니다. 그리드는 셀을 지정하는 임의 개수의 콘텐츠 인수를 사용합니다. 행은 자동으로 추가되지만 `rows` 인수를 사용하여 수동으로 크기를 조정할 수도 있습니다.

이제 초록을 추가해 봅시다. 학회에서는 초록을 들쭉날쭉하게 가운데 정렬하도록 요구한다는 점을 기억하십시오.

```example:0,0,612,317.5
>>> #set text(font: "Libertinus Serif", 11pt)
>>> #set par(justify: true)
>>> #set page(
>>>   "us-letter",
>>>   margin: auto,
>>>   header: align(right + horizon)[
>>>     빙하 흐름에 대한 유체 역학 모델
>>>   ],
>>>   numbering: "1",
>>> )
>>>
>>> #align(center, text(17pt)[
>>>   *빙하 흐름에 대한 유체 역학 모델*
>>> ])
>>>
>>> #grid(
>>>   columns: (1fr, 1fr),
>>>   align(center)[
>>>     테레즈 텅스텐 \
>>>     아르토스 연구소 \
>>>     #link("mailto:tung@artos.edu")
>>>   ],
>>>   align(center)[
>>>     존 도 박사 \
>>>     아르토스 연구소 \
>>>     #link("mailto:doe@artos.edu")
>>>   ]
>>> )
>>>
<<< ...

#align(center)[
  #set par(justify: false)
  *초록* \
  #lorem(80)
]
>>> #lorem(600)
```

잘했습니다! 주목할 만한 점은 `align`의 콘텐츠 인수 내에서 설정 규칙을 사용하여 초록에 대한 양쪽 정렬을 해제했다는 것입니다. 이것은 첫 번째 설정 규칙 이후에 지정되었음에도 불구하고 문서의 나머지 부분에 영향을 미치지 않습니다. 왜냐하면 콘텐츠 블록은 스타일링을 _범위 지정_하기 때문입니다. 콘텐츠 블록 내에서 설정된 모든 것은 해당 블록 내의 콘텐츠에만 영향을 미칩니다.

또 다른 조정은 논문 제목을 변수에 저장하여 머리글과 제목에 대해 두 번 입력할 필요가 없도록 하는 것입니다. `{let}` 키워드를 사용하여 이 작업을 수행할 수 있습니다.

```example:single
#let title = [
  빙하 흐름에 대한 유체 역학 모델
]

<<< ...

>>> #set text(font: "Libertinus Serif", 11pt)
>>> #set par(justify: true)
#set page(
>>>   "us-letter",
>>>   margin: auto,
  header: align(
    right + horizon,
    title
  ),
<<<   ...
>>>   numbering: "1",
)

#align(center, text(17pt)[
  *#title*
])

<<< ...

>>> #grid(
>>>   columns: (1fr, 1fr),
>>>   align(center)[
>>>     테레즈 텅스텐 \
>>>     아르토스 연구소 \
>>>     #link("mailto:tung@artos.edu")
>>>   ],
>>>   align(center)[
>>>     존 도 박사 \
>>>     아르토스 연구소 \
>>>     #link("mailto:doe@artos.edu")
>>>   ]
>>> )
>>>
>>> #align(center)[
>>>   #set par(justify: false)
>>>   *초록* \
>>>   #lorem(80)
>>> ]
>>>
>>> #lorem(600)
```

콘텐츠를 `title` 변수에 바인딩한 후에는 함수에서 사용할 수 있으며 마크업 내에서도 사용할 수 있습니다(함수와 마찬가지로 `#` 접두사 사용). 이렇게 하면 다른 제목을 결정하더라도 한 곳에서 쉽게 변경할 수 있습니다.

## 열 및 제목 추가 { #columns-and-headings }
위의 논문은 안타깝게도 납덩어리처럼 보입니다. 이를 수정하기 위해 몇 가지 제목을 추가하고 논문을 2열 레이아웃으로 전환해 보겠습니다. 다행히도 쉽게 할 수 있습니다. `columns` 인수를 사용하여 `page` 설정 규칙을 수정하기만 하면 됩니다.

인수 목록에 `{columns: 2}`를 추가하여 전체 문서를 두 개의 열로 래핑했습니다. 그러나 이렇게 하면 제목과 저자 개요에도 영향을 미칩니다. 전체 페이지에 걸쳐 있도록 하려면 [`{place}`]($place)에 대한 함수 호출로 래핑할 수 있습니다. Place는 정렬과 배치해야 할 내용을 위치 인수로 예상합니다. 명명된 `{scope}` 인수를 사용하면 항목을 현재 열 또는 해당 부모(페이지)를 기준으로 배치할지 여부를 결정할 수 있습니다. 구성해야 할 것이 하나 더 있습니다. 다른 인수가 제공되지 않으면 `{place}`는 해당 내용을 문서 흐름에서 꺼내 컨테이너의 다른 내용 레이아웃에 영향을 주지 않고 다른 내용 위에 배치합니다.

```example
#place(
  top + center,
  rect(fill: black),
)
#lorem(30)
```

여기서 `{place}`를 사용하지 않았다면 정사각형은 자체 줄에 있었겠지만 여기서는 그 뒤에 오는 몇 줄의 텍스트와 겹칩니다. 마찬가지로 해당 텍스트는 정사각형이 없는 것처럼 작동합니다. 이 동작을 변경하려면 `{float: true}` 인수를 전달하여 페이지 상단 또는 하단에 배치된 항목이 차지하는 공간이 다른 콘텐츠로 채워지지 않도록 할 수 있습니다.

```example:single
>>> #let title = [
>>>   빙하 흐름에 대한 유체 역학 모델
>>> ]
>>>
>>> #set text(font: "Libertinus Serif", 11pt)
>>> #set par(justify: true)
>>>
#set page(
>>> margin: auto,
  paper: "us-letter",
  header: align(
    right + horizon,
    title
  ),
  numbering: "1",
  columns: 2,
)

#place(
  top + center,
  float: true,
  scope: "parent",
  clearance: 2em,
)[
>>>  #text(
>>>    17pt,
>>>    weight: "bold",
>>>    title,
>>>  )
>>>
>>>  #grid(
>>>    columns: (1fr, 1fr),
>>>    [
>>>      테레즈 텅스텐 \
>>>      아르토스 연구소 \
>>>      #link("mailto:tung@artos.edu")
>>>    ],
>>>    [
>>>      존 도 박사 \
>>>      아르토스 연구소 \
>>>      #link("mailto:doe@artos.edu")
>>>    ]
>>>  )
<<<   ...

  #par(justify: false)[
    *초록* \
    #lorem(80)
  ]
]

= 서론
#lorem(300)

= 관련 연구
#lorem(200)
```

이 예에서는 또한 [`{v}`]($v) 함수를 사용하는 대신 `{place}` 함수의 `clearance` 인수를 사용하여 본문과의 간격을 제공했습니다. 또한 다양한 부분 주위에 있는 명시적인 `{align(center, ..)}` 호출을 제거할 수 있습니다. 왜냐하면 배치에서 가운데 정렬을 상속하기 때문입니다.

이제 남은 작업은 한 가지뿐입니다. 제목 스타일을 지정하는 것입니다. 가운데 정렬하고 작은 대문자를 사용해야 합니다. `heading` 함수는 이러한 설정을 지정하는 방법을 제공하지 않으므로 자체 제목 표시 규칙을 작성해야 합니다.

```example:50,250,265,270
>>> #let title = [
>>>   빙하 흐름에 대한 유체 역학 모델
>>> ]
>>>
>>> #set text(font: "Libertinus Serif", 11pt)
>>> #set par(justify: true)
>>> #set page(
>>>   "us-letter",
>>>   margin: auto,
>>>   header: align(
>>>     right + horizon,
>>>     title
>>>   ),
>>>   numbering: "1",
>>>   columns: 2,
>>> )
#show heading: it => [
  #set align(center)
  #set text(13pt, weight: "regular")
  #block(smallcaps(it.body))
]

<<< ...
>>>
>>> #place(
>>>   top + center,
>>>   float: true,
>>>   scope: "parent",
>>>   clearance: 2em,
>>> )[
>>>   #text(
>>>     17pt,
>>>     weight: "bold",
>>>     title,
>>>   )
>>>
>>>   #grid(
>>>     columns: (1fr, 1fr),
>>>     [
>>>       테레즈 텅스텐 \
>>>       아르토스 연구소 \
>>>       #link("mailto:tung@artos.edu")
>>>     ],
>>>     [
>>>       존 도 박사 \
>>>       아르토스 연구소 \
>>>       #link("mailto:doe@artos.edu")
>>>     ]
>>>   )
>>>
>>>   #par(justify: false)[
>>>     *초록* \
>>>     #lorem(80)
>>>   ]
>>> ]
>>>
>>> = 서론
>>> #lorem(35)
>>>
>>> == 동기
>>> #lorem(45)
```

훌륭합니다! 모든 제목에 적용되는 표시 규칙을 사용했습니다. 매개변수로 제목을 전달하는 함수를 제공합니다. 해당 매개변수는 내용으로 사용할 수 있지만 `title`, `numbers` 및 `level`과 같은 일부 필드도 있어 사용자 지정 모양을 구성할 수 있습니다. 여기서는 가운데 정렬하고 제목이 기본적으로 굵게 표시되기 때문에 글꼴 두께를 `{"regular"}`로 설정하고 [`smallcaps`] 함수를 사용하여 제목의 제목을 작은 대문자로 렌더링합니다.

남은 유일한 문제는 이제 모든 제목이 동일하게 보인다는 것입니다. "동기" 및 "문제 설명" 하위 섹션은 기울임꼴 런인 헤더여야 하지만 지금은 섹션 제목과 구별할 수 없습니다. 설정 규칙에서 `where` 선택기를 사용하여 이 문제를 해결할 수 있습니다. 이것은 수준별로 필터링할 수 있도록 제목(및 기타 요소)에서 호출할 수 있는 [메서드]($scripting/#methods)입니다. 이를 사용하여 섹션 및 하위 섹션 제목을 구분할 수 있습니다.

```example:50,250,265,245
>>> #let title = [
>>>   빙하 흐름에 대한 유체 역학 모델
>>> ]
>>>
>>> #set text(font: "Libertinus Serif", 11pt)
>>> #set par(justify: true)
>>> #set page(
>>>   "us-letter",
>>>   margin: auto,
>>>   header: align(
>>>     right + horizon,
>>>     title
>>>   ),
>>>   numbering: "1",
>>>   columns: 2,
>>> )
>>>
#show heading.where(
  level: 1
): it => block(width: 100%)[
  #set align(center)
  #set text(13pt, weight: "regular")
  #smallcaps(it.body)
]

#show heading.where(
  level: 2
): it => text(
  size: 11pt,
  weight: "regular",
  style: "italic",
  it.body + [.],
)
>>>
>>> #place(
>>>   top + center,
>>>   float: true,
>>>   scope: "parent",
>>>   clearance: 2em,
>>> )[
>>>   #text(
>>>     17pt,
>>>     weight: "bold",
>>>     title,
>>>   )
>>>
>>>  #grid(
>>>    columns: (1fr, 1fr),
>>>    [
>>>      테레즈 텅스텐 \
>>>      아르토스 연구소 \
>>>      #link("mailto:tung@artos.edu")
>>>    ],
>>>    [
>>>      존 도 박사 \
>>>      아르토스 연구소 \
>>>      #link("mailto:doe@artos.edu")
>>>    ]
>>>  )
>>>
>>>   #par(justify: false)[
>>>     *초록* \
>>>     #lorem(80)
>>>   ]
>>> ]
>>>
>>> = 서론
>>> #lorem(35)
>>>
>>> == 동기
>>> #lorem(45)
```

훌륭합니다! 각각 1단계 및 2단계 제목에 선택적으로 적용되는 두 개의 표시 규칙을 작성했습니다. `where` 선택기를 사용하여 수준별로 제목을 필터링했습니다. 그런 다음 하위 섹션 제목을 런인으로 렌더링했습니다. 또한 하위 섹션 제목 끝에 마침표를 자동으로 추가합니다.

학회 스타일 가이드를 검토해 보겠습니다.
- 글꼴은 11pt 세리프 글꼴이어야 합니다 ✓
- 제목은 17pt 굵게 표시되어야 합니다 ✓
- 논문에는 단일 열 초록과 2열 본문 텍스트가 포함됩니다 ✓
- 초록은 가운데 정렬되어야 합니다 ✓
- 본문 텍스트는 양쪽 정렬되어야 합니다 ✓
- 1단계 섹션 제목은 가운데 정렬되어야 하며 작은 대문자로 렌더링되고 13pt여야 합니다 ✓
- 2단계 제목은 런인, 기울임꼴이며 본문 텍스트와 동일한 크기여야 합니다 ✓
- 마지막으로 페이지는 미국 편지 크기여야 하며 중앙에 번호가 매겨지고 각 페이지의 오른쪽 상단에는 논문 제목이 포함되어야 합니다 ✓

이제 이러한 모든 스타일을 준수하며 학회에 논문을 제출할 수 있습니다! 완성된 논문은 다음과 같습니다.

<img
  src="3-advanced-paper.png"
  alt="완성된 논문"
  style="box-shadow: 0 4px 12px rgb(89 85 101 / 20%); width: 500px; max-width: 100%; display: block; margin: 24px auto;"
>

## 검토
이제 머리글과 바닥글을 만들고, 함수와 범위를 사용하여 스타일을 로컬에서 재정의하고, [`grid`] 함수를 사용하여 더 복잡한 레이아웃을 만들고, 개별 함수 및 전체 문서에 대한 표시 규칙을 작성하는 방법을 배웠습니다. 또한 [`where` 선택기]($styling/#show-rules)를 사용하여 수준별로 제목을 필터링하는 방법도 배웠습니다.

논문은 대성공이었습니다! 학회에서 같은 생각을 가진 많은 연구원을 만났고 내년에 같은 장소에서 발표할 프로젝트를 계획하고 있습니다. 하지만 동일한 스타일 가이드를 사용하여 새 논문을 작성해야 하므로 이제 귀하와 귀하의 팀을 위해 시간을 절약할 수 있는 템플릿을 만들고 싶을 수도 있습니다.

다음 섹션에서는 여러 문서에서 재사용할 수 있는 템플릿을 만드는 방법을 배웁니다. 이것은 더 고급 주제이므로 지금 당장 할 마음이 없다면 나중에 다시 돌아와도 됩니다.
