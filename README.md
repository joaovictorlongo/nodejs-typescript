# Node.JS + Typescript + ESLint + Prettier

Neste guia, vamos entender como iniciar uma aplicação Node.JS utilizando o Typescript com os plugins do ESLint e Prettier. 

Essas configurações foram inspiradas pelo sistema de educação da Rocketseat 💜.

O node deve estar instalado na máquina onde será iniciado o desenvolvimento desta aplicação. 

💚 Para a instalação do node acesse: [nodejs.org](https://nodejs.org/)

A primeira coisa que devemos fazer depois de definir a pasta do projeto é executar o seguinte comando do yarn no terminal:

```json
yarn init
```

> Observação: caso for adicionada a flag -y na frente deste comando, não será questionada as informações do projeto, apenas será gerado um arquivo package.json padrão.

💡 Caso o yarn não estiver instalado, o mesmo pode instalado via npm com o seguinte comando: 

```json
npm install --global yarn
```

Após responder as questões como nome do projeto, versão, autor, vamos dar inicio de fato a configuração do projeto, pois agora, vamos ter o arquivo package.json criado na pasta do projeto.

Uma coisa legal a se fazer de inicio seria a geração do arquivo ".editorconfig" clicando com o botão direito no explorer do VSCode e selecionando a opção "Generate .editorconfig" (necessita da extensão EditorConfig).

A configuração que podemos utilizar no .editorconfig seria:

```jsx
# EditorConfig is awesome: https://EditorConfig.org

# top-most EditorConfig file
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

Essas configurações são as que mais me deixaram confortável no desenvolvimento, mas, não é uma regra absoluta 😆.

A primeira dependência que iremos adicionar ao projeto será o Typescript. Como iremos utilizar esse recurso apenas no desenvolvimento, iremos adicionar a flag "-D" na frente do comando:

```json
yarn add typescript -D
```

Após adicionar o Typescript, vamos iniciar o mesmo, para que seja gerado um arquivo de configuração chamado tsconfig.json

```json
yarn tsc --init
```

Neste arquivo, vamos habilitar as seguintes opções (basta remover o comentário da frente e checar o valor ou copiar o JSON abaixo):

```json
{
	"compilerOptions":{
		"target": "es5",
		"experimentalDecorators": true,
		"emitDecoratorMetadata": true,
		"module": "commonjs",
		"rootDir": "./src",
		"outDir": "./dist",
		"esModuleInterop": true,
		"forceConsistentCasingInFileNames": true,
		"strict": true,
		"strictPropertyInitialization": false,
		"skipLibCheck": true
	}
}
```

Vamos então adicionar o express:

```json
yarn add express
```

Em seguida, como estamos utilizando o Typescript, vamos também adicionar os tipos do express para esta linguagem, como uma dependência de desenvolvimento.

```json
yarn add @types/express -D
```

Dessa forma já estaremos aptos para iniciar a utilização do express.

Agora iremos adicionar o ESLint, que será responsável pela checagem de erros no nosso código. Este plugin também será adicionado como uma dependência de desenvolvimento:

```json
yarn add eslint -D
```

Para iniciarmos a configuração vamos executar o seguinte comando:

```json
yarn eslint --init
```

Ao executar este comando, vamos responder algumas questões para a configuração do ESLint no projeto. Essas são as configurações que estou utilizando no momento:

**"How would you like to use ESLint?"**

= To check syntax, find problem, and enforce code style

**"What type of modules does your project use?"**

= JavaScript modules (import/export)

**"Wich framework does your project use?"**

= None of these

**"Does your project use typescript?"**

= Yes

**"Where does your code run?"**

= [ESPAÇO para desbilitar o Browser] [ESPAÇO para habilitar o Node] [ENTER para prosseguir].

**"How would you like to define a style for your project?"**

= Use a popular style guide.

= Arbnb...

**"What format do you want your config file to be in?"**

= JSON

Em seguida serão listados alguns recursos que devem ser instalados além do ESLint e em seguida  a seguinte pergunta:

**"Would you like to install them with npm?"**

= YES

> Caso queira, você pode fazer a instalação via yarn. Basta copiar exatamente as dependências listadas e instalar com o comando.

Caso não tenha sido incluída automaticamente, vamos adicionar uma dependência de desenvolvimento que gerencia as importações no Typescript, que pode ser adicionada com o seguinte comando:

```json
yarn add eslint-import-resolver-typescript -D
```

Observação: a extensão do ESLint deve estar instalada e habilitada no VSCode.

Vamos então adicionar o Prettier e os demais recursos para a integração do Prettier com o ESLint. Para isso vamos executar o seguinte comando:

```json
yarn add prettier eslint-config-prettier eslint-plugin-prettier -D
```

Ao finalizar a instalação dessas dependências no nosso projeto vamos começar a configurar nossas preferências para estes plugins. 

O que será descrito abaixo não são regras absolutas mas é o que mais funciona para grande parte dos usuários...

Vamos iniciar a configuração adicionando algumas orientações no arquivo ".eslintrc.json". Por fim o arquivo deve ser parecido com isso:

```json
{
    "env": {
        "es2021": true,
        "node": true
    },
    "extends": [
        "airbnb-base",
        "plugin:prettier/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint",
        "prettier"
    ],
    "rules": {
      "prettier/prettier": "error",
      "class-methods-use-this": "off",
      "@typescript-eslint/camelcase": "off",
      "camelcase": "off",
      "@typescript-eslint/no-unused-vars": ["error", {
        "agrsIgnorePattern": "^_"
      }],
      "import/extensions": [
        "error",
        "ignorePackages",
        {
          "ts": "never"
        }
      ]
    },
    "settings": {
      "import/resolver": {
        "typescript": {}
      }
    }
}
```

Agora vamos criar um arquivo chamado ".eslintignore" e seu conteúdo por enquanto será:

```jsx
/*.js
node_modules
dist
```

Feito isso vamos adicionar outro arquivo chamado "prettier.config.js" com as seguintes configurações:

```jsx
module.exports = {
	singleQuote: true,
	trailingComma: "all",
	arrowParens: "avoid"
}
```

Observação: assim como o ESLint, o Prettier também precisa da sua extensão instalada e habilitada no VSCode.

Por último mas não menos importante, vamos adicionar o ts-node-dev no nosso projeto, para que toda vez que algum arquivo for salvo, o servidor reinicie automaticamente com as alterações realizadas. 
Para isso, vamos executar o seguinte comando:

```json
yarn add ts-node-dev -D
```

Em seguida, vamos configurar o nosso package.json para incluir um script de inicialização do servidor, para isso vamos adicionar o seguinte objeto após a propriedade "license":

```json
"scripts": {
	"build": "tsc",
	"dev:server": "ts-node-dev --transpile-only src/server.ts"
},
```

Portanto quando quisermos iniciar o servidor basta executarmos o comando:

```json
yarn dev:server
```

E para gerar o build da aplicação:

```json
yarn build
```

Ufa! Agora sim podemos por a mão na massa e iniciar o desenvolvimento do backend, utilizando Typescript com ESLint + Prettier 🚀
