# Guia de estilo Javascript

Guia de estilo de codificação para projetos Javascript.

Outras guias de estilo

- [Geral](../README.md)
- [CSS & Sass](../css-sass/README.md)
- [HTML](../html/README.md)
- [PHP](../php/README.md)

## Índice

  1. [Strings](#strings)
  1. [Funções](#funções)
  1. [Módulos](#módulos)
  1. [Comentários](#comentários)
  1. [Convenções de nome](#convenções-de-nomes)
  1. [Links](#links)
  1. [Referência](#referência)

## Strings

- Use aspas simples `''` para strings. [`quotes`](https://eslint.org/docs/rules/quotes)

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // bad - template literals should contain interpolation or newlines
    const name = `Capt. Janeway`;

    // good
    const name = 'Capt. Janeway';
    ```

- Strings que fazem com que a linha ultrapasse 100 caracteres não devem ser escritas em várias linhas usando concatenação de strings.

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

- Ao criar strings programaticamente, use strings de modelo em vez de concatenação. [`prefer-template`](https://eslint.org/docs/rules/prefer-template) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // bad
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

**[⬆ Topo](#índice)**

## Funções

- Espaçamento em uma assinatura de função. [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > Por quê? A consistência é boa e você não precisa adicionar ou remover um espaço ao adicionar ou remover um nome.

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

## Módulos

- Use sempre modulos (`import`/`export`) em um sistema de módulo não padrão. Você sempre pode transpilar para o seu sistema de módulo preferido.

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

- Não use importações curinga.

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

- Importe apenas de um caminho em um só lugar. [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)

    ```javascript
    // bad
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // good
    import foo, { named1, named2 } from 'foo';

    // good
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

## Comentários

- Use `/** ... */` para comentários de múltiplas linhas.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

- Use `//` para comentários de linha única. Coloque comentários de linha única em uma nova linha acima do assunto do comentário. Coloque uma linha vazia antes do comentário, a menos que esteja na primeira linha de um bloco.

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

- Comece todos os comentários com um espaço para melhor leitura. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true;

    // good
    // is current tab
    const active = true;

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

## Convenções de nomes

- Evite nomes com uma única letra. Seja descritivo com sua nomenclatura. [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // bad
    function q() {
      // ...
    }

    // good
    function query() {
      // ...
    }
    ```

- Use camelCase ao nomear objetos, funções e instâncias. [`camelCase`](https://eslint.org/docs/rules/camelcase)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

- Use PascalCase somente ao nomear construtores ou classes. [`new-cap`](https://eslint.org/docs/rules/new-cap)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

- Não use sublinhados à direita ou à esquerda. [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle)

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';

    // good, in environments where WeakMaps are available
    // see https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, 'Panda');
    ```

- Um nome de arquivo base deve corresponder exatamente ao nome de sua exportação padrão.

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // em outro arquivo
    // bad
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // bad
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // good
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    ```

- Acrônimos e inicialismos devem sempre estar todos em maiúsculas ou em minúsculas.

    ```javascript
    // bad
    import SmsContainer from './containers/SmsContainer';

    // bad
    const HttpRequests = [
      // ...
    ];

    // good
    import SMSContainer from './containers/SMSContainer';

    // good
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const requests = [
      // ...
    ];
    ```

- Você pode, opcionalmente, colocar uma constante em maiúscula apenas se (1) for exportada, (2) for uma `const` (não pode ser reatribuída) e (3) o programador pode confiar nela (e em suas propriedades aninhadas) para nunca mudar.

    > Por quê? Esta é uma ferramenta adicional para auxiliar em situações em que o programador não tem certeza se uma variável pode mudar. UPPERCASE_VARIABLES estão deixando o programador saber que eles podem confiar na variável (e suas propriedades) para não mudar.
  - E todas as variáveis `const`? - Isso é desnecessário, então letras maiúsculas não devem ser usadas para constantes dentro de um arquivo. No entanto, deve ser usado para constantes exportadas.
  - E os objetos exportados? - Maiúsculas no nível superior de exportação (por exemplo, `EXPORTED_OBJECT.key`) e manter que todas as propriedades aninhadas não mudem.

    ```javascript
    // bad
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

    // bad
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

    // bad
    export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

    // ---

    // allowed but does not supply semantic value
    export const apiKey = 'SOMEKEY';

    // better in most cases
    export const API_KEY = 'SOMEKEY';

    // ---

    // bad - unnecessarily uppercases key while adding no semantic value
    export const MAPPING = {
      KEY: 'value'
    };

    // good
    export const MAPPING = {
      key: 'value',
    };
    ```

**[⬆ Topo](#índice)**

## Links

### Ferramentas

- Code Style Linters
  - [ESlint](https://eslint.org/)
  - [JSHint](https://jshint.com/)
  - [Prettier](https://prettier.io/)

### Outros Guias de Estilo

- [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
- [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/)
- [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)
- [StandardJS](https://standardjs.com)
- [Javascript Style Guide Airbnb](https://github.com/airbnb/javascript)

### Outros Estilos

- [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
- [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
- [Popular JavaScript Coding Conventions on GitHub](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
- [Multiple var statements in JavaScript, not superfluous](https://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**[⬆ Topo](#índice)**

## Referência

Este guia de estilo foi baseado no guia de estilo de Javascript do Airbnb.

- [Javascript Style Guide Airbnb](https://github.com/airbnb/javascript)
