# vue-my-template

Vue + Firestore リアルタイムアンケートをさくっと作った

 - 他の人が投票すると自分の画面の結果表示も同期する

 - 「About」のページから質問を更新可能、更新されたら全員の画面の質問項目が同期される

![demo](https://i.gyazo.com/fa7ed04aaed068b1656e76b83acf8297.gif)
![demo](https://i.gyazo.com/1be0aba15368438c45256a6182988088.gif)

元ネタ
[ZOZO大忘年会でFirebaseを使った1,000人規模のリアルタイムアンケートを費用2円で制作した話](https://qiita.com/tomohito_takahashi/items/fea4a931ad1734585d1c)


# Development


Create `.env.local` file in the root folder

Please enter the value of firebaseConfig
```
VUE_APP_API_KEY=
VUE_APP_AUTH_DOMAIN=
VUE_APP_DATABASE_URL=
VUE_APP_PROJECT_ID=
VUE_APP_MESSAGING_SENDERID=
VUE_APP_APP_ID=
```


### Features Vue Settings
Using vue-cli3(@vue/cli)
- TypeScript
- Router
- Vuex
- CSS Pre-Processors
  - Stylus
- Linter / Formatter
  - ESLint
  - Prettier


### Features
- class-style component syntax
- Using Pug as a template engine
  - Rewriting vue's initial template using pug and stylus
- Auto Lint on save

## Snippets
User Snippets for Visual Studio Code

### .vue
```
	"vue-template": {
			"prefix": "vue-template",
			"body": [
				"<template lang=\"pug\">",
				"  .${1:name}",
				"</template>",
				"<script lang=\"ts\">",
				"import { Component, Vue } from \"vue-property-decorator\";",
				"import router from \"@/router\";",
				"",
				"@Component({",
				"  name: \"${1:name}\",",
				"  components: {}",
				"})",
				"export default class ${2:class} extends Vue {}",
				"</script>",
				"<style lang=\"stylus\" scoped>",
				".${1:name}",
				"</style>",
			],
			"description": "simple vue template"
	}
```

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```
