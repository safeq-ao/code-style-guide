# Guia de estilo HTML

Guia de estilo de codificação para projetos HTML.

Outras guias de estilo

- [Geral](../README.md)
- [CSS & Sass](../css-sass/README.md)
- [Javascript](../javascript/README.md)
- [PHP](../php/README.md)

## Índice

- [Editor](#editor)
- [Como começar um arquivo](#como-começar-um-arquivo)
- [Formatador](#formatador)
- [Semântica](#semântica)
- [HTML](#html)

## Editor

Recomendamos o [Visual Studio Code](https://code.visualstudio.com/) como editor padrão para escrever em HTML.

### Como começar um arquivo

Se já usa o VSCode inicie sempre os projetos usando o prefixo `!` para gerar a base do html.

adicionar duas imagens

## Formatador

Use o [Prettier](https://prettier.io/) como um formatador de código para manter o estilo de código consistente (e para evitar discussões fora do tópico).

Prettier formata todo o código e mantém o estilo consistente. No entanto, existem algumas regras adicionais que você precisa seguir.

## Semântica

O HTML5 nos fornece muitos elementos semânticos destinados a descrever com precisão o conteúdo. Certifique-se de se beneficiar de seu rico vocabulário.

**Mau**

```html
<div id=main>
  <div class=article>
    <div class=header>
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>
```

**Bom**

```html
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime=2015-02-21>21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

## HTML

### Doctype

Use sempre HTML5 doctype em seus arquivos .html.

<!DOCTYPE html>

### Linguagem do document

Defina o idioma do documento usando o atributo lang em seu elemento <html>:

```html
<html lang="en-US"></html>
```

### Characterset documento

Você também deve definir o conjunto de caracteres do seu documento da seguinte forma:

```html
<meta charset="utf-8" />
```

Use UTF-8, a menos que tenha um bom motivo para não fazê-lo; ele cobrirá praticamente todas as necessidades de caracteres, independentemente do idioma que você estiver usando em seu documento. Além disso, você sempre deve especificar o conjunto de caracteres o mais cedo possível no bloco <head> do HTML (dentro do primeiro kilobyte), pois ele protege contra uma vulnerabilidade de segurança desagradável do Internet Explorer.

### Meta tag da janela de visualização

Por fim, você sempre deve adicionar a meta tag viewport em seu HTML <head> para dar ao exemplo de código uma melhor chance de funcionar em dispositivos móveis. Você deve incluir pelo menos o seguinte em seu documento, que pode ser modificado posteriormente conforme a necessidade:

```html
<meta name="viewport" content="width=device-width" />
```

Consulte Usando a meta tag viewport para controlar o layout em navegadores móveis para obter mais detalhes.

### Atributos

Você deve colocar todos os valores de atributo entre aspas duplas. É tentador omitir as aspas, já que o HTML5 permite isso, mas a marcação é mais organizada e fácil de ler se você as incluir. Por exemplo, isso é melhor:

```html
<img src="images/logo.jpg" alt="Um ícone de globo circular" class="sem borda">
```

### Nomes

Use letras minúsculas para todos os nomes de elementos e nomes/valores de atributos porque isso parece mais organizado e significa que você pode escrever marcações mais rapidamente.

**Mau**

```html
<P CLASS="WHOA-THERE">Why is my markup shouting?</P>
```

**Bom**

```html
<p class="nice">This looks nice and neat</p>
```

### Nomes de Classes e ID

Use nomes semânticos de classe/ID e separe várias palavras com hífens (-). Não use camelCase.

**Mau**

```html
<p class="bigRedBox">Blah blah blah</p>
```

**Bom**

```html
<p class="editorial-summary">Blah blah blah</p>
```

### Aspas duplas

Use aspas duplas para HTML, não aspas simples.

```html
<p class="important">Yes</p>
```
