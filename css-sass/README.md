# Guia de estilo CSS & SASS

Guia de estilo de codificação para projetos CSS e SASS.

Outras guias de estilo

- [Geral](../README.md)
- [HTML](../html/README.md)
- [Javascript](../javascript/README.md)
- [php](../php/README.md)

## Índice

1. [CSS](#css)
    - [Formatação](#formatação)
    - [Comentários](#comentários)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selector](#id-selector)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
1. [Sass](#sass)
    - [Sintaxe](#sintaxe)
    - [Ordenação](#ordenação-das-declarações-de-propriedade)
    - [Variáveis](#variáveis)
    - [Mixins](#mixins)
    - [Diretiva de extensão](#diretiva-de-extensão)
    - [Seletores aninhados](#seletores-aninhados)

## CSS

### Formatação

- Use tabulações suaves (2 espaços) para recuo.
- Prefira travessões a camelCasing nos nomes das classes.
  - Sublinhados e PascalCasing são aceitáveis se você estiver usando BEM (consulte [OOCSS e BEM](#oocss-and-bem) abaixo).
- Não use seletores de ID.
- Ao usar vários seletores em uma declaração de regra, dê a cada seletor sua própria linha.
- Coloque um espaço antes da chave de abertura `{` nas declarações de regras.
- Nas propriedades, coloque um espaço após, mas não antes, o caractere `:`.
- Coloque colchetes `}` nas declarações de regras em uma nova linha.
- Coloque linhas em branco entre declarações de regras.

**Mau**

```css
.avatar {
 border-radius: 50%;
 border: 2px solid white;
}
.no,
.nope,
.not_good {
 // ...
}
#lol-no {
 // ...
}
```

**Bom**

```css
.avatar {
 border-radius: 50%;
 border: 2px solid white;
}

.one,
.selector,
.per-line {
 // ...
}
```

### Ordem de Declaração

As declarações de propriedade devem ser agrupadas na seguinte ordem:

- Posicionamento
- modelo de caixa
- Tipográfico
- Visual
- Diversos

> O posicionamento vem em primeiro lugar porque pode remover um elemento do fluxo normal do documento e substituir os estilos relacionados ao modelo de caixa. O modelo de caixa - seja flexível, flutuante, grade ou tabela - segue conforme dita as dimensões, posicionamento e alinhamento de um componente. Todo o resto ocorre dentro do componente ou sem afetar as duas secções anteriores e, portanto, elas vêm por último.
> Embora faça border parte do modelo de caixa, a maioria dos sistemas redefine globalmente box-sizing para border-box que border-width isso não afete as dimensões gerais. Isso, combinado com manter border perto border-radius, é o motivo pelo qual está na secção Visual.
> Mixins e funções do pré-processador devem aparecer onde for mais apropriado. Por exemplo, um border-top-radius() mixin iria no lugar das border-radius propriedades, enquanto uma responsive-font-size() função iria no lugar das font-size propriedades.

```css
.declaration-order {
  // Posicionamento
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  // Box model
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;

  // Tipografia
  font: normal 14px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;
  text-decoration: underline;

  // Visual
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  // Diversos
  opacity: 1;
}
```

### Comentários

- Prefira comentários de linha (`//` em Sass-land) para bloquear comentários.
- Prefira comentários em sua própria linha. Evite comentários de fim de linha.
- Escreva comentários detalhados para o código que não é autodocumentado:
  - Usos do z-index
  - Compatibilidade ou hacks específicos do navegador

### OOCSS and BEM

Encorajamos alguma combinação de OOCSS e BEM por estas razões:

- Ajuda a criar relacionamentos claros e rígidos entre CSS e HTML
- Isso nos ajuda a criar componentes reutilizáveis e combináveis
- Permite menos aninhamento e menor especificidade
- Ajuda na construção de folhas de estilo escaláveis

**OOCSS**, ou “Object Oriented CSS”, é uma abordagem para escrever CSS que o incentiva a pensar em suas folhas de estilo como uma coleção de “objetos”: snippets reutilizáveis e repetíveis que podem ser usados independentemente em um site.

- Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
- Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, or “Block-Element-Modifier”, é uma _convenção de nomenclatura_ para classes em HTML e CSS. Ele foi originalmente desenvolvido pela Yandex com grandes bases de código e escalabilidade em mente e pode servir como um conjunto sólido de diretrizes para a implementação do OOCSS.

- CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
- Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

Recomendamos uma variante do BEM com “blocos” PascalCased, que funciona particularmente bem quando combinado com componentes (por exemplo, React). Sublinhados e traços ainda são usados para modificadores e filhos.

**Exemplo**

```jsx
// ListingCard.jsx
function ListingCard() {
 return (
  <article class="ListingCard ListingCard--featured">
   <h1 class="ListingCard__title">
    Adorable 2BR in the sunny Mission
   </h1>

   <div class="ListingCard__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
   </div>
  </article>
 );
}
```

```css
/* ListingCard.css */
.ListingCard {
}
.ListingCard--featured {
}
.ListingCard__title {
}
.ListingCard__content {
}
```

- `.ListingCard` é o “bloco” e representa o componente de nível superior
- `.ListingCard__title` é um “elemento” e representa um descendente de `.ListingCard` que ajuda a compor o bloco como um todo.
- `.ListingCard--featured` é um “modificador” e representa um estado ou variação diferente no bloco `.ListingCard`.

### ID selector

Embora seja possível selecionar elementos por ID em CSS, geralmente deve ser considerado um anti-padrão. Os seletores de ID introduzem um nível desnecessariamente alto de [especificidade](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) em suas declarações de regras e não são reutilizáveis.

Para saber mais sobre esse assunto, leia [o artigo do CSS Wizardry](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) sobre como lidar com a especificidade.

### JavaScript hooks

Evite vincular à mesma classe em seu CSS e JavaScript. Combinar os dois geralmente leva, no mínimo, a perda de tempo durante a refatoração, quando um desenvolvedor deve fazer referência cruzada a cada classe que está alterando e, na pior das hipóteses, os desenvolvedores ficam com medo de fazer alterações por medo de quebrar a funcionalidade.

Recomendamos a criação de classes específicas de JavaScript para vincular, prefixadas com `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

Use `0` invés de `none` para especificar que um estilo não tem borda.

**Mau**

```css
.foo {
 border: none;
}
```

**Bom**

```css
.foo {
 border: 0;
}
```

### Brevidade

Mantenha seu código conciso. Use propriedades abreviadas e evite usar várias propriedades ao
não é necessário.

**Mau**

```css
div {
  transition: all 1s;
  top: 50%;
  margin-top: -10px;
  padding-top: 5px;
  padding-right: 10px;
  padding-bottom: 20px;
  padding-left: 10px;
}
```

**Bom**

```css
div {
  transition: 1s;
  top: calc(50% - 10px);
  padding: 5px 10px 20px;
}
```

### Prefixos de Fornecedores

Elimine prefixos de fornecedores obsoletos agressivamente. Se precisar usá-los, insira-os antes do
propriedade padrão.

**Mau**

```css
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}
```

**Bom**

```css
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

### Animações

Dê preferência às transições sobre as animações. Evite animar outras propriedades além de
`opacity` e `transform`.

**Mau**

```css
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}
```

**Bom**

```css
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

### Unidades

Use valores sem unidades quando puder. Favorecer `rem` se você usar unidades relativas. Prefira segundos a
milissegundos.

**Mau**

```css
div {
  margin: 0px;
  font-size: .9em;
  line-height: 22px;
  transition: 500ms;
}
```

**Bom**

```css
div {
  margin: 0;
  font-size: .9rem;
  line-height: 1.5;
  transition: .5s;
}
```

### Cores

Se você precisar de transparência, use `rgba`. Caso contrário, use sempre o formato hexadecimal.

Use minúsculas em todos os valores hexadecimais, por exemplo, #fff. As letras minúsculas são muito mais fáceis de discernir ao digitalizar um documento, pois tendem a ter formas mais exclusivas.

**Mau**

```css
div {
  color: hsl(103, 54%, 43%);
}
```

**Bom**

```css
div {
  color: #5a3;
  background-color: #fff;
}
```

**[⬆ Topo](#índice)**

## Sass

### Sintaxe

- Use a sintaxe `.scss`, nunca a sintaxe `.sass` original
- Ordene suas declarações regulares de CSS e `@include` logicamente (veja abaixo)

### Ordenação das declarações de propriedade

1. Declarações de propriedade

  Liste todas as declarações de propriedade padrão, qualquer coisa que não seja um `@include` ou um seletor aninhado.

  ```scss
  .btn-green {
    background: green;
    font-weight: bold;
     // ...
  }
  ```

1. `@include` declarações

  Agrupar `@include`s no final facilita a leitura de todo o seletor

  ```scss
  .btn-green {
   background: green;
   font-weight: bold;
   @include transition(background 0.5s ease);
   // ...
  }
  ```

1. Seletores aninhados

Seletores aninhados, _se necessário_, vão por último e nada vai depois deles. Adicione espaço em branco entre suas declarações de regra e seletores aninhados, bem como entre seletores aninhados adjacentes. Aplique as mesmas diretrizes acima aos seus seletores aninhados.

  ```scss
  .btn {
   background: green;
   font-weight: bold;
   @include transition(background 0.5s ease);

   .icon {
    margin-right: 10px;
    }
  }
  ```

### Variáveis

Prefira nomes de variáveis com hífen (por exemplo, `$minha-variável`) em vez de nomes de variáveis com camelCased ou snake_case. É aceitável prefixar nomes de variáveis que devem ser usados apenas dentro do mesmo arquivo com um sublinhado (por exemplo, `$_minha-variável`).

### Mixins

Mixins devem ser usados para secar seu código, adicionar clareza ou complexidade abstrata - da mesma forma que funções bem nomeadas. Mixins que não aceitam argumentos podem ser úteis para isso, mas observe que, se você não estiver compactando sua carga útil (por exemplo, gzip), isso pode contribuir para a duplicação desnecessária de código nos estilos resultantes.

### Diretiva de extensão

`@extend` deve ser evitado porque tem um comportamento não intuitivo e potencialmente perigoso, especialmente quando usado com seletores aninhados. Mesmo a extensão dos seletores de espaço reservado de nível superior pode causar problemas se a ordem dos seletores for alterada posteriormente (por exemplo, se eles estiverem em outros arquivos e a ordem em que os arquivos são carregados muda). O gzipping deve lidar com a maior parte da economia que você teria obtido usando `@extend`, e você pode DRY acima de suas folhas de estilo com mixins.

### Seletores aninhados

**Não aninhe seletores com mais de três níveis de profundidade!**

```scss
.page-container {
 .content {
  .profile {
   // STOP!
  }
 }
}
```

Quando os seletores se tornam tão longos, você provavelmente está escrevendo CSS que é:

- Fortemente acoplado ao HTML (frágil) _—OU—_
- Excessivamente específico (poderoso) _—OU—_
- Não reutilizável

Novamente: **nunca aninhe seletores de ID!**

Se você deve usar um seletor de ID em primeiro lugar (e você realmente deve tentar não fazê-lo), eles nunca devem ser aninhados. Se você estiver fazendo isso, precisará revisitar sua marcação ou descobrir por que uma especificidade tão forte é necessária. Se você estiver escrevendo HTML e CSS bem formados, você **nunca** precisará fazer isso.

**[⬆ Topo](#índice)**

## Referência

Este guia de estilo foi baseado no guia de estilo de CSS & SASS do Airbnb.

- [Airbnb CSS / Sass Style Guide](https://github.com/airbnb/css)
