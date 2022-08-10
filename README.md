# React + Vite + Typescript

* Criação do projeto
* [Componentes](#componentes)
* Propriedades
* Estado
* Estilização
* Roteamento

---
# O que é o React?

O react é chamado de biblioteca e não de framework porque ele não traz muita opinião na estrutura de pastas em arquivos de configuração.

No máximo traz opinião em cima da linguagem que utilizamos, da forma que criamos os componentes, o visual e nada mais.

---

# Criando projeto

Existem várias formas de criar um projeto em React, e a mais famosa é usando o create react app.
Mas esta não é a forma mais atual de fazer isso!

```bash
$ npx create react app 
```

# Create react app

O create react app é um BOILERPLATE, ele se refere a templates de código que podem ser estilizados para construção de aplicações com pouca ou nenhum alteração.

----

# yarn create vite

A forma mais atual para iniciar um projeto em react é utilizando vite.

Vite é um framework frontend que possui integração com diversos templates em javascript, como por exemplo: react, vue, vanilla, etc.

Ele também tem suporte ao Typescript, o que é uma mão na roda no desenvolvimento para evitar futuras dores de cabeça.

## Para iniciar um projeto com vite:

```bash
$ npm install --global vite
$ yarn create vite name_project --template react-ts
$ cd name_project
$ yarn install
$ yarn dev
```
---

# index.html

```html
  <script type="module" src="/src/main.tsx"></script>
```

Esta tag permite a importação e utilização de modulos do ECMAscript.
Em React tudo é Javascript.
HTML dentro de Javascript é chamado de JSX: Javascript + XML.

```bash
"/src/main.tsx" 
    |-- é o único arquivo que importamos para o HTML.
    |-- Importa o React.DOM um método render, create root
    |-- "tag" <App />
```

---

# Componentes

Componentes são uma forma de quebrar a aplicação em vários pedaços, melhorando o processo de manutenção. Todos os componentes são funções que retornam HTML.

---

# Como criar componentes?

```bash
$ cd src/
$ mkdir components
$ cd components
$ touch Item.tsx
```

- TSX é o formato de arquivo que comporta typescript e html.

### Item.tsx:
```js
export function Item(){
  return (
    <h1>My Item</h1>
  )
}
```

----

### App.tsx:
```js
export function App(){
  return (
    <Item />
  )
}
```
---

### Retornar mais de um componente: ❌
```js
export function App(){
  return (
    <Item />
    <Item />
    <Item />
  )
}
```

---

### Retornar mais de um componente: ✔
```js
export function App(){
  return (
    <div>
      <Item />
      <Item />
      <Item />
    </div>
  )
}
```
Nunca podemos ter componentes um em baixo do outro sem ter algo encapsulando eles.

---

# Propriedades:
### App.tsx:
```js
export function App(){
  return (
    <div>
      <Item name="martelo"/>
      <Item />
      <Item />
    </div>
  )
}
```
Erro, Typescript não está esperando uma propriedade chamada "nome" dentro de Item.

### Item.tsx:

```js
type ItemProps = {
  name: string;
}
```
Desta forma estamos declarando a propriedade "name" como obrigatória.

Para declarar uma propriedade opcional:

```js
type ItemProps = {
  name?: string;
}
```

### Forma errada: ❌
```js
export function Item(props){

}
```
Como estamos utilizando typescript, é necessário informar as propriedades esperadas e suas tipagens.

### Forma certa: ✔
```js
export function Item(props: ItemProps){

}
```

---

### Retornando dinamicamente as propriedades dos componentes: ❌

```js
export function Item(props: ItemProps){
  return(
    <p>props.name</p>
  )
}
```

Desta forma estamos informando para o componente retornar um texto "props.name".

### Retornando dinamicamente as propriedades dos componentes: ✔

```js
export function Item(props: ItemProps){
  return(
    <p>{props.name}</p>
  )
}
```
Sempre que quisermos colocar código javascript dentro de HTML é necessário inciar com {}.

---

# Estados

### App.tsx:

```html
  <button>New Button</button>
```
---

### Errado: ❌

```js
export function App(){
  const itens = useState<string[]>([]);
}
```

### Certo: ✔

```js
export function Item(props: ItemProps){
  const [itens, setItens] = useState<string[]>([]);
}
```

## useState():

Retorna duas informações:
- Itens: Variavel que armazena as informações
- Função que altera o estado das informações

### O React utiliza um conceito da programação funcional chamado de "Imutabilidade"

Isto significa que nunca alteramos diretamente o valor das variáveis, apenas "recolocamos" os valores das variáveis.

---

### App.tsx:

```js
export function App(){
  const [itens, setItens] = useState<string[]>([
    'martelo'
    'camisa'
    'colar'
  ])

  function createItem(){}
  
  <div>
    {
      itens.map(item => {
        return <Item name={item} />
      })
    }
    <button onclick={createItem}>New Button</button>
  </div>
}
```
- items é a variavel que chamamos do state
- map é a função usada para percorrer o vetor de informações
- item é o novo vetor para carregar as informações dentro deste escopo
- Item é o componente que sera renderizado em cada interação do map

--- 

## Função para criar novos itens: ❌

```js
  function createItem(){
    setItem(['caneta'])
  }
```

Neste caso, o resultado será a remoção de todos os itens para a substituição de apenas o item 'caneta'.

## Função para criar novos itens: ✔

```js
  function createItem(){
    setItem([...itens, ['bola']]);
  }
```

"...itens" irá salvar o estado dos itens já salvos, enquanto que adiciona um novo valor ao vetor.

---

## Adicionar input:

```js
const itemName = useRef();
```

## useRef()

useRef() serve para referenciar elementos do html em javascript.

## useRef() com Typescript:

```js
const itemName = useRef() as MutableRefObject<HTMLInputElement>
```

Reforçando novamente: Usando typescript é sempre necessário informar a tipagem esperada dos componentes e suas propriedades.

## Refatorando a função createItem para receber o nome dos itens:

```js
function createItem(){
  setItens([...itens, itemName.current?.value])
}

<input type="text" ref={itemName} />

```
---

# Estilização

```bash
$ touch src/App.css
```

## App.css:

```css
body{
  background: #121214;
  color: #e1e1f6;
}
```

## App.tsx:

```js
import './App.css';

<button
  style={{
    backgroundColor: '#036',
    border: 0,
    padding: '6px 12px',
    color: '#fff'
  }}
>New button</button>
```

Para estilizar diretamente um componente primeiro abrimos {} para inserir código javascript, após isso, abrimos {} novamente porque a estilização direta é um objeto.

---

# Roteamento

A ferramenta mais famosa de roteamento atualmente do React é React Router.

## Install React Router Dom:

```bash
$ yarn add react-router-dom
```

## O que é roteamento?

Roteamento é permitir que a nossa aplicação tenha várias páginas.

```bash
$ mkdir /src/pages
$ cd pages
$ touch Catalogo.tsx
$ touch Carrinho.tsx
```
## Catalogo.tsx:

```js
export function Catalogo(){
  return <h1>Catalogo</h1>
}
```

## Carrinho.tsx:

```js
export function Carrinho(){
  return <h1>Carrinho</h1>
}
```

## Criar Routes:


```bash
$ touch src/routes.tsx
```

## Routes.tsx:❌

```js
export function Routes(){
  
}
```
Nas versões mais atuais do react router dom, isso pode gerar conflitos com o nome dos componentes que são importados e exportados.

## Routes.tsx: ✔

```js
import {
  BrowserRouter as Router,
  Routes,
  Route
}

export function AppRoutes(){
  return(
    <Router>
      <Routes>
        <Route path="/carrinho" element={<Carrinho />} />
        <Route path="/catalogo" element={<Catalogo />} />
      </Routes>
    </Router>
  )
}
```

A propriedade "path" indica qual a rota da página que precisa ser acessada para renderizar o componente.
A propriedade "element" indica qual o componente a ser renderizado quando o path é acessado.

## App.tsx:

Agora no componente App alteramos o que ele vai renderizar, nesse caso, será o gerenciador de rotas.

```js
export function App(){
  return <AppRoutes />
}
```


# Referências:

* [ReactJS](https://pt-br.reactjs.org/)
* [ViteJS](https://vitejs.dev/)
* [react-router-dom](https://v5.reactrouter.com/web/guides/quick-start)