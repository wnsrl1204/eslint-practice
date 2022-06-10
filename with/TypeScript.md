# TypeScript추가하기
eslint에 typescript를 추가해보자.

## 모듈 설치
```
npm i -D @typescript-eslint/eslint-plugin @typescript-eslint/parser
```



## .eslintrc.json 파일
```
{
  "env": {
    "browser": true,
    "es6": true,
    "node": true
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "plugins": ["@typescript-eslint"],
  "extends": ["plugin:@typescript-eslint/recommended"],
  "rules": {
    // ...
  }
}
```



## 출처
[typescripteslint]: https://typescript-eslint.io/docs/development/architecture/packages/
[typescript-eslint.io][typescripteslint]
