---
description: Typst 튜토리얼.
---

# 템플릿 만들기
이 튜토리얼의 이전 세 장에서는 Typst에서 문서를 작성하고, 기본 스타일을 적용하고, 게시자의 스타일 가이드에 맞게 모양을 자세히 사용자 지정하는 방법을 배웠습니다. 이전 장에서 작성한 논문이 큰 성공을 거두었으므로 같은 학회에 후속 논문을 작성해 달라는 요청을 받았습니다. 이번에는 이전 장에서 만든 스타일을 가져와 재사용 가능한 템플릿으로 만들고 싶습니다. 이 장에서는 귀하와 귀하의 팀이 단 하나의 표시 규칙으로 사용할 수 있는 템플릿을 만드는 방법을 배웁니다. 시작해 봅시다!

## 장난감 템플릿 { #toy-template }
Typst에서 템플릿은 전체 문서를 래핑할 수 있는 함수입니다. 이를 수행하는 방법을 배우려면 먼저 자신만의 함수를 작성하는 방법을 복습해 보겠습니다. 원하는 모든 작업을 수행할 수 있으므로 약간 미친 짓을 해보는 것은 어떨까요?

```example
#let amazed(term) = box[✨ #term ✨]

당신은 #amazed[아름다워요]!
```

이 함수는 단일 인수 `term`을 사용하고 반짝이로 둘러싸인 `term`이 있는 콘텐츠 블록을 반환합니다. 또한 놀란 용어가 줄 바꿈으로 인해 반짝이와 분리되지 않도록 전체를 상자에 넣었습니다.

Typst와 함께 제공되는 많은 함수에는 선택적 명명된 매개변수가 있습니다. 우리 함수에도 있을 수 있습니다. 텍스트 색상을 선택할 수 있는 매개변수를 함수에 추가해 보겠습니다. 매개변수가 지정되지 않은 경우 기본 색상을 제공해야 합니다.

```example
#let amazed(term, color: blue) = {
  text(color, box[✨ #term ✨])
}

당신은 #amazed[아름다워요]!
나는 #amazed(color: purple)[놀랐어요]!
```

이제 템플릿은 `amazed`와 같은 사용자 지정 함수로 전체 문서를 래핑하여 작동합니다. 그러나 전체 문서를 거대한 함수 호출로 래핑하는 것은 번거로울 것입니다! 대신 "모든 것" 표시 규칙을 사용하여 더 깔끔한 코드로 동일한 작업을 수행할 수 있습니다. 이러한 표시 규칙을 작성하려면 표시 키워드 바로 뒤에 콜론을 넣은 다음 함수를 제공합니다. 이 함수에는 문서의 나머지 부분이 매개변수로 제공됩니다. 그런 다음 함수는 이 콘텐츠로 무엇이든 할 수 있습니다. `amazed` 함수는 단일 콘텐츠 인수로 호출할 수 있으므로 이름으로 표시 규칙에 전달하기만 하면 됩니다. 시도해 봅시다.

```example
>>> #let amazed(term, color: blue) = {
>>>   text(color, box[✨ #term ✨])
>>> }
#show: amazed
나는 내 삶의 좋은 점에 집중하고 부정적인 생각이나 신념을 버리기로 선택합니다.
사실, 나는 굉장해요!
```

이제 전체 문서가 마치 래핑한 것처럼 `amazed` 함수에 전달됩니다. 물론 이 특정 함수에서는 특별히 유용하지 않지만 설정 규칙 및 명명된 인수와 결합하면 매우 강력할 수 있습니다.

## 설정 및 표시 규칙 포함 { #set-and-show-rules }
템플릿에 일부 설정 및 표시 규칙을 적용하려면 함수 내 콘텐츠 블록 내에서 `set` 및 `show`를 사용한 다음 해당 콘텐츠 블록에 문서를 삽입할 수 있습니다.

```example
#let template(doc) = [
  #set text(font: "Inria Serif")
  #show "멋진 것": [Typst]
  #doc
]

#show: template
오늘 멋진 것을 배우고 있어요.
지금까지 잘 되고 있어요!
```

이전 장에서 이미 발견한 것처럼 설정 규칙은 해당 콘텐츠 블록 내의 모든 것에 적용됩니다. 모든 표시 규칙이 전체 문서를 `template` 함수에 전달하므로 템플릿의 텍스트 설정 규칙과 문자열 표시 규칙이 전체 문서에 적용됩니다. 이 지식을 사용하여 이전 장에서 작성한 논문의 본문 스타일을 재현하는 템플릿을 만들어 보겠습니다.

```example
#let conf(title, doc) = {
  set page(
    paper: "us-letter",
>>> margin: auto,
    header: align(
      right + horizon,
      title
    ),
    columns: 2,
<<<     ...
  )
  set par(justify: true)
  set text(
    font: "Libertinus Serif",
    size: 11pt,
  )

  // 제목 표시 규칙.
<<<   ...
>>>  show heading.where(
>>>    level: 1
>>>  ): it => block(
>>>    align(center,
>>>      text(
>>>        13pt,
>>>        weight: "regular",
>>>        smallcaps(it.body),
>>>      )
>>>    ),
>>>  )
>>>  show heading.where(
>>>    level: 2
>>>  ): it => box(
>>>    text(
>>>      11pt,
>>>      weight: "regular",
>>>      style: "italic",
>>>      it.body + [.],
>>>    )
>>>  )

  doc
}

#show: doc => conf(
  [논문 제목],
  doc,
)

= 서론
#lorem(90)

<<< ...
>>> == 동기
>>> #lorem(140)
>>>
>>> == 문제 설명
>>> #lorem(50)
>>>
>>> = 관련 연구
>>> #lorem(200)
```

이전 장에서 대부분의 코드를 복사하여 붙여넣었습니다. 두 가지 차이점은 다음과 같습니다.

1. 모든 표시 규칙을 사용하여 `conf` 함수로 모든 것을 래핑했습니다.
   함수는 몇 가지 설정 및 표시 규칙을 적용하고 마지막에 전달된 내용을 반복합니다.

2. 또한 콘텐츠 블록 대신 중괄호로 묶인 코드 블록을 사용했습니다. 이렇게 하면 모든 설정 규칙과 함수 호출 앞에 `#`을 붙일 필요가 없습니다. 대신 코드 블록에 직접 마크업을 작성할 수 없습니다.

또한 제목이 어디서 왔는지 주목하십시오. 이전에는 변수 안에 있었습니다.
이제 템플릿 함수의 첫 번째 매개변수로 받고 있습니다. 그렇게 하기 위해 모든 표시 규칙에 클로저(즉시 사용되는 이름 없는 함수)를 전달했습니다. `conf` 함수는 제목과 본문이라는 두 개의 위치 인수를 예상하지만 표시 규칙은 본문만 전달하기 때문에 그렇게 했습니다. 따라서 논문 제목을 설정하고 표시 규칙의 단일 매개변수를 사용할 수 있도록 하는 새 함수 정의를 추가합니다.

## 명명된 인수가 있는 템플릿 { #named-arguments }
이전 장의 논문에는 제목과 저자 목록이 있었습니다. 템플릿에 이러한 항목을 추가해 보겠습니다. 제목 외에도 소속이 있는 저자 목록과 논문 초록을 템플릿에서 허용하려고 합니다. 가독성을 유지하기 위해 명명된 인수로 추가하겠습니다. 결국 다음과 같이 작동하기를 원합니다.

```typ
#show: doc => conf(
  title: [개선된 모델링을 향하여],
  authors: (
    (
      name: "테레사 텅스텐",
      affiliation: "아르토스 연구소",
      email: "tung@artos.edu",
    ),
    (
      name: "유진 데클란",
      affiliation: "온두라스 주립대학교",
      email: "e.deklan@hstate.hn",
    ),
  ),
  abstract: lorem(80),
  doc,
)

...
```

이 새로운 템플릿 함수를 만들어 보겠습니다. 먼저 `title` 인수에 기본값을 추가합니다. 이렇게 하면 제목을 지정하지 않고 템플릿을 호출할 수 있습니다. 또한 빈 기본값이 있는 명명된 `authors` 및 `abstract` 매개변수를 추가합니다. 다음으로 이전 장에서 제목, 초록 및 저자를 생성하는 코드를 템플릿으로 복사하여 고정된 세부 정보를 매개변수로 바꿉니다.

새로운 `authors` 매개변수는 `name`, `affiliation` 및 `email` 키가 있는 [사전]($dictionary)의 [배열]을 예상합니다. 임의의 수의 저자를 가질 수 있으므로 저자 목록에 대해 하나, 둘 또는 세 개의 열이 필요한지 동적으로 결정합니다. 먼저 `authors` 배열에서 [`.len()`]($array.len) 메서드를 사용하여 저자 수를 결정합니다. 그런 다음 열 수를 이 개수와 3 중 작은 값으로 설정하여 세 개 이상의 열을 만들지 않도록 합니다. 저자가 세 명 이상인 경우 대신 새 행이 삽입됩니다. 이러한 목적으로 `grid` 함수에 `row-gutter` 매개변수도 추가했습니다. 그렇지 않으면 행이 너무 가까이 붙어 있게 됩니다. 사전에서 저자에 대한 세부 정보를 추출하려면 [필드 액세스 구문]($scripting/#fields)을 사용합니다.

여전히 각 저자에 대해 그리드에 인수를 제공해야 합니다. 여기서 배열의 [`map` 메서드]($array.map)가 유용합니다. 배열의 각 항목으로 호출되는 함수를 인수로 사용합니다. 각 저자에 대한 세부 정보 서식을 지정하고 콘텐츠 값이 포함된 새 배열을 반환하는 함수를 전달합니다. 이제 그리드에 여러 인수로 사용하려는 값 배열이 하나 있습니다. [`spread` 연산자]($arguments)를 사용하여 이 작업을 수행할 수 있습니다. 배열을 가져와 각 항목을 함수에 별도의 인수로 적용합니다.

결과 템플릿 함수는 다음과 같습니다.

```typ
#let conf(
  title: none,
  authors: (),
  abstract: [],
  doc,
) = {
  // 이전의 설정 및 표시 규칙.
>>> #set page(columns: 2)
<<<   ...

  set align(center)
  text(17pt, title)

  let count = authors.len()
  let ncols = calc.min(count, 3)
  grid(
    columns: (1fr,) * ncols,
    row-gutter: 24pt,
    ..authors.map(author => [
      #author.name \
      #author.affiliation \
      #link("mailto:" + author.email)
    ]),
  )

  par(justify: false)[
    *초록* \
    #abstract
  ]

  set align(left)
  doc
}
```

## 별도의 파일 { #separate-file }
대부분의 경우 템플릿은 다른 파일에 지정된 다음 문서로 가져옵니다. 이렇게 하면 작성하는 기본 파일이 깔끔하게 유지되고 템플릿을 쉽게 재사용할 수 있습니다. 더하기 버튼을 클릭하여 파일 패널에서 새 텍스트 파일을 만들고 이름을 `conf.typ`로 지정합니다. `conf` 함수 정의를 해당 새 파일 안으로 옮깁니다. 이제 표시 규칙 전에 가져오기를 추가하여 기본 파일에서 액세스할 수 있습니다. `{import}` 키워드와 콜론 사이에 파일 경로를 지정한 다음 가져오려는 함수 이름을 지정합니다.

템플릿 적용을 조금 더 우아하게 만드는 또 다른 방법은 함수에서 [`.with`]($function.with) 메서드를 사용하여 모든 명명된 인수를 미리 채우는 것입니다. 이렇게 하면 클로저를 자세히 설명하고 템플릿 목록 하단에 콘텐츠 인수를 추가하는 것을 피할 수 있습니다. [Typst Universe]($universe)의 템플릿은 이러한 스타일의 함수 호출과 함께 작동하도록 설계되었습니다.

```example:single
>>> #let conf(
>>>   title: none,
>>>   authors: (),
>>>   abstract: [],
>>>   doc,
>>> ) = {
>>>  set text(font: "Libertinus Serif", 11pt)
>>>  set par(justify: true)
>>>  set page(
>>>    "us-letter",
>>>    margin: auto,
>>>    header: align(
>>>      right + horizon,
>>>      title
>>>    ),
>>>    numbering: "1",
>>>    columns: 2,
>>>  )
>>>
>>>  show heading.where(
>>>    level: 1
>>>  ): it => block(
>>>    align(center,
>>>      text(
>>>        13pt,
>>>        weight: "regular",
>>>        smallcaps(it.body),
>>>      )
>>>    ),
>>>  )
>>>  show heading.where(
>>>    level: 2
>>>  ): it => box(
>>>    text(
>>>      11pt,
>>>      weight: "regular",
>>>      style: "italic",
>>>      it.body + [.],
>>>    )
>>>  )
>>>
>>>  place(
>>>    top,
>>>    float: true,
>>>    scope: "parent",
>>>    clearance: 2em,
>>>    {
>>>      set align(center)
>>>      text(17pt, title)
>>>      let count = calc.min(authors.len(), 3)
>>>      grid(
>>>        columns: (1fr,) * count,
>>>        row-gutter: 24pt,
>>>        ..authors.map(author => [
>>>          #author.name \
>>>          #author.affiliation \
>>>          #link("mailto:" + author.email)
>>>        ]),
>>>      )
>>>      par(justify: false)[
>>>        *초록* \
>>>        #abstract
>>>      ]
>>>    },
>>>  )
>>>  doc
>>>}
<<< #import "conf.typ": conf
#show: conf.with(
  title: [
    개선된 모델링을 향하여
  ],
  authors: (
    (
      name: "테레사 텅스텐",
      affiliation: "아르토스 연구소",
      email: "tung@artos.edu",
    ),
    (
      name: "유진 데클란",
      affiliation: "온두라스 주립대학교",
      email: "e.deklan@hstate.hn",
    ),
  ),
  abstract: lorem(80),
)

= 서론
#lorem(90)

== 동기
#lorem(140)

== 문제 설명
#lorem(50)

= 관련 연구
#lorem(200)
```

이제 학회 논문을 해당 학회에 재사용 가능한 템플릿으로 변환했습니다! 다른 사람들도 사용할 수 있도록 [포럼](https://forum.typst.app/)이나 [Typst의 Discord 서버](https://discord.gg/2uDybryKPe)에 공유해 보는 것은 어떨까요?

## 검토
축하합니다. Typst 튜토리얼을 완료했습니다! 이 섹션에서는 자신만의 함수를 정의하고 재사용 가능한 문서 스타일을 정의하는 템플릿을 만들고 적용하는 방법을 배웠습니다. 많은 것을 배우고 멀리 왔습니다. 이제 Typst를 사용하여 자신만의 문서를 작성하고 다른 사람들과 공유할 수 있습니다.

저희는 아직 매우 젊은 프로젝트이며 피드백을 찾고 있습니다. 질문, 제안 또는 버그를 발견한 경우 [포럼](https://forum.typst.app/), [Discord 서버](https://discord.gg/2uDybryKPe), [GitHub](https://github.com/typst/typst/) 또는 웹 앱의 피드백 양식(도움말 메뉴에서 항상 사용 가능)을 통해 알려주십시오.

그래서 무엇을 기다리고 계십니까? [가입](https://typst.app)하고 무언가를 작성하십시오!
