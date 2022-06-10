# eslint-practice
eslint를 사용하는 방법을 알아보자


## 시작하기
ESLint는 "ecmascript와 javascript" 코드에서 발견된 패턴을 식별하고 보고하는 도구이다.

코드의 일관성을 높이고 버그를 방지한다.


### 특징
+ parser로 Espree를 사용한다.
+ AST(Abstract Syntax Tree)를 사용하여 코드의 패턴을 평가한다.
+ 완전히 플러그할 수 있고, 모든 단일 규칙은 플러그 또는 런타임에 추가할 수 있다.


## 설치 및 사용법
### 설치
```
npm i -D eslint

# or

yarn add eslint --dev
```

구성파일( ex) .eslitrc.json )을 설정해야하는데, 가장쉬운 방법으로는
```
npm init @eslint/config

#or

yarn create @eslint/config
```
npm init @eslint/config는 기본적으로 package.json 파일이 있다고 가정해서,

만약에 package.json이 없다면, npm init으로 만들어야 실행할 수 있다고 한다.


eslint 실행
```
npx eslint yourfile.js

#or

yarn run eslint yourfile.js
```
전역 설치( npm install eslint --global )는 권장하지 않고,

모든 플러그인이나 공유 가능한 config는 로컬설치를 권장한다.

## 구성 Configuration
```
# .eslintrc.json

{
  "extends": "eslint:recommended",
  "rules": {
    "semi": ["error", "always"],
    "quotes": ["error", "double"]
  }
}
```
### rules

[eslintrules]: https://eslint.org/docs/rules/ "ESLint rules"
이름은 [rules][eslintrules]에 나온 규칙들에서 선택할 수 있다.

[rules][eslintrules]문서를 보면 ✓체크 된 사항은 "eslint:recommended"를 확장했을 경우

자동적으로 등록된다고 한다.

배열 중 첫번째 값은
+ <code>"off"</code> or <code>0</code> - 규칙을 끄기
+ <code>"warn"</code> or <code>1</code> - 주의(exit 코드에 영향을 미치지 않는다.)
+ <code>"error"</code> or <code>2</code> - 에러(exit 코드 1을 반환한다.)
위의 3가지 수준을 통해서 ESLint는 세밀한 제어를 가능하게 한다.

### extends
rules에 확장된 값을 추가한다.

만약 rules와 eslint 둘 다 존재하지 않으면, ESLint는 파일 검사 수행하지 않는다.

npm init @eslint/config를 했을때 기본적으로

"extends" :"eslint: recommended"가 추가되는데

[rules][eslintrules]페이지에 ✓체크된 rules가 자동적으로 포함된다.

다른사람의 config file을 보고싶으면

[npmjseslintconfig]: https://www.npmjs.com/search?q=eslint-config "npmjs에서 eslint-config검색"
[npmjs.com][npmjseslintconfig]에 들어가서 확인하면 된다.

이중 airbnb에서 만든 "eslint-config-airbnb"는 인기있는 확장자 중 하나인데

"extends"에서는 eslint-config- 지시자를 생략하고 사용할 수 있다.

```
{
  "extends": [
    "eslint: recommended",
    "airbnb"
  ]
}
```


### env

[eslintenv]: https://eslint.org/docs/user-guide/configuring/language-options#specifying-environments "eslint env"
사전 정의된 전역 변수를 제공한다. [공식문서][eslintenv]

"browser", "es2022" 등등 

### globals
전역변수를 세부적으로 컨트롤 할 수 있다.
"writable" 덮어쓰기 혀용, "readonly" 읽기만 가능, "off" 사용불가

### parserOptions
+ ecmaVersion - ECMAScript의 버전을 선택할 수 있다. 숫자나 "latest", 기본값 5
+ sourceType - 우리의 코드가 "script"인지 "module"인지 선택한다. 기본값 "script"
+ allowReserved - 예약어를 식별자로 사용할 수 있다 (ecmaVersion 3만 가능함)
+ ecmaFeature - 추가 언어의 기능을 선택
  + globalReturn - 전역 볌위에서 return문 허용
  + impliedStrict - 엄격모드 활성화
  + jsx - jsx 활성화

### parser

[espree]: https://github.com/eslint/espree "espree"
기본값 [Espree][espree]

[parserrule]: https://eslint.org/docs/developer-guide/working-with-custom-parsers
아래 조건을 충족하는 parser를 사용할 수 있다.
1. config file에서 불러올 수 잇는 노드모듈, npm을 통해 별도로 설치해야함을 뜻한다.
2. parser interface를 준수해야한다. [ESLint 개발자 가이드][parserrule] ( parser를 사용만 할꺼면 User Guide만으로도 충분 )

자주 사용되는 parser

<code>esprima</code>

<code>@babel/eslint-parser</code>

<code>@typescript-eslint/parser</code>

### plugins
타사 플러그인을 사용가능하다.

사용하기 전 npm을 통해 설치를 해야한다.

esint-plugin- 로 시작하는데,

접두사는 생략가능하다. ( eslint-plugin-react 사용하면 react만 써도 됨 )

ESLint 다른 규칙에서 사용시( rules, env, process 등등 )
```
{
  "plugins" : [
    "jquery",   // eslint-plugin-jquery
    "@foo/foo", // @foo/eslint-plugin-foo
    "@bar"      // @bar/eslint-plugin
  ],
  "extends": [
    "plugin:@foo/foo/recommended",
    "plugin:@bar/recommended"
  ],
  "rules": {
    "jquery/a-rule": "error",
    "@foo/foo/some-rule": "error",
    "@bar/another-rule": "error"
  },
  "env": {
    "jquery/jquery": true,
    "@foo/foo/env-foo": true,
    "@bar/env-bar": true
  }
}
```


### processor 와 overrides
플러그인과 함께 제공할 수 있다.

javascript를 추출한 다음

코드를 Lint 하거나, 코드를 (preprocessor)전처리같은 목적으로 사용할 수 있다.

특정 종류의 파일을 변경하려면 overrides를 사용한다.

모든 파일
```
{
  "plugins": ["a-plugin"],
  "processor": "a-plugin/a-processor"
}
```

특정 파일
```
{
  "plugins": ["a-plugin"],
  "overrides": [
    {
      "files": ["*.md"],
      "processor": "a-plugin/markdown"
    },
    {
      "files": ["**/*.md/*.js"],
      "rules": {
        "strict": "off"
       }
     }
  ]
}
```

## CLI
```
npx eslint [options] [file|dir|glob]*
```

예
```
# 파일 두개에 대해서 ESLint 실행
npx eslint file1.js file2.js

# 파일 여러개에 대해서 ESLint 실행
npx eslint lib/**
```

주의사항

shell에 따라 glob가 다를 수 있다.

node에서 사용하는 shell을 사용하기 위해서는

아래와 같이 따옴표를 사용하자.
```
npx eslint "lib/**"
```

[eslintcli]: https://eslint.org/docs/user-guide/command-line-interface "Eslint CLI"

전체 명령어는 [공식문서][eslintcli]에서
### --no-eslintrc
비활성화 한다.
```
npx eslint --no-eslintrc file.js
```

### -c, --config
추가 config file를 설정할 수 있다.
```
npx eslint -c ~/my-eslint.json file.js
```
만약 .eslintrc.* 이나 package.json과 함께 사용된다면
-c 옵션의 파일이 우선순위를 갖는다.

### --ext
확장자 검색

ESLint는 기본적으로 * .js와, .eslintrc.* 에서 overides.files에 있는 확장자만 조사를 한다.

기본적으로 탐색 인수가 dir인 경우만 유효하다. ( glob, file 에서는 --ext무시됨 )


### --resolve-plugins-relative-to
플러그인이 다른 위치에 있는경우

플러그인 위치를 바꿀때 사용한다.


### --fix
이것을 사용하면

바꿀 수 있는 파일은 변경하고

그래도 오류가 있다면 그때는 오류를 알린다.

프로세서가 자동수정을 허용하지 않으면, --fix는 작동하지 않는다.


### --fix-dry-run
--fix와 비슷하지만, 파일에 저장하지 않는다.

stdin에 수정된 코드를 출력한다.( --stdin 옵션 사용시 )

--format 포멧터 필요


### --fix-type
위 두개의 fix옵션을 사용할때

수정 유형을 지정할 수 있다.


### --ignore-path
기본적으로 .eslintignore 에 설정가능하다.

위 옵션을 사용하면 override된다.


### --stdin
파일 말고 stdin에서 들어온 입력에 대해서 ESLint 수행
```
cat myfile.js | npx eslint --stdin
```


### --quiet
경고"warn" 비활성화


### --max-warnings
경고 상태가 많은 경우

경고의 갯수를 지정하고 지정횟수보다 적을경우 ESLint 성공

이 옵션을 사용하지 않는다면, -1을 주거나 아예 --max-warnings를 쓰지않는다.

```
npx eslint --max-warnings 10 file.js
```


### -o, --output-file
에러 결과를 출력할 파일 지정


### -f, --format
콘솔 출력 형식을 지정한다. 기본값 stylish
eslint-formatter- 생략 가능


### --no-inline-config
인라인 주석을 무시

인라인 옵션 종류
+ <code>/* eslint-disable*/</code>
+ <code>/* eslint-enable*/</code>
+ <code>/* global*/</code>
+ <code>/* eslint*/</code>
+ <code>/* eslint-env*/</code>
+ <code>// eslint-disable-line</code>
+ <code>// eslint-disable-next-line</code>

### --init
npm init @eslint/config와 동일하다.


### --cache
변경된 파일에 대해서만 ESLint를 수행한다.

캐시는 .eslintcache에 저장된다.


### --debug
디버그가 필요할때 사용


## Exit codes
종료 코드로 ESLint가 제대로 종료 되었는지 확인
+ <code>0</code> - Linting 성공 && ( 에러가 존재하지 않거나 || --max-warings 보다 적은 오류를 가짐 )
+ <code>1</code> - Linting 성공 && ( 에러가 존재 || -max-warings 보다 많은 오류를 가짐 )
+ <code>2</code> - Linting 실패 configuration 문제 또는 내부 에러


## 통합
[eslintintegration]: https://eslint.org/docs/user-guide/integrations "Eslint integrations"
[IDE나 빌드도구 등등][eslintintegration]


## 출처
[eslint]: https://eslint.org/ "eslint"
[ESLint 공식문서][eslint]
