# Guia de estilo de codificação 👨‍💻

Este é um guia de estilos para codificação dos projetos na SAFEQ-AO.
Todos os programadores devem seguir os guias de estilos para melhor comunicação interna e leitura de códigos entre os projetos da SAFEQ.

## Tópicos

- [Projetos](#projetos)
- [Nome dos arquivos](#nome-dos-arquivos-e-estrutura-de-pastas)
- [Guia de estilo Git](#guia-de-estilo-git)
  - [Branches](#branches)
  - [Commits](#commits)
  - [Mensagens](#mensagens)
  - [Merging](#merging)
  - [Outras regras](#outras-regras-do-git)
- [Documentação dos projetos](#documentação-dos-projetos)
- [Testes](#testes)
- [Estilo de código](#estilo-de-código)
- [Ferramentas](#ferramentas)
- [Extensões VSCode](#extensões-vscode)
- [Referência](#referência)

## Projetos

Todos os arquivos de código fonte dos projetos devem estar num repositório [Git](https://git-scm.com/) e depois hospedados em [Github](https://github.com/) para permitir que os membros da equipe trabalhem no projeto.

> O repositório do projeto deve ser criado na conta da organização e não em um perfil pessoal.

## Nome dos arquivos e estrutura de pastas

![Structure and Naming](/images/folder-tree.png)

Os nomes das pastas devem estar em _lowercase_ e devem ser separados por traços (-).

Arquivos de código fonte em projetos devem estar na pasta **src**, excepto o _index.html_.

Dentro de **src**:

- as páginas do site devem estar em **pages**;
- as estilos do site devem estar em **css**;
- os arquivos javascript do site devem estar em **js**.
- imagens, vídeos, ícones, fontes e outros elementos externos devem estar em **assets** e em pastas separadas pela categoria;
- bibliotecas externas devem estar em **assets/lib**.

**Mau**

```files
├── images
├── css
├── js
├── index.html
```

**Bom**

```files
.
├── src
|  ├── assets
|  ├── css
|  └── js
|  └── pages
├── index.html
```

## Guia de estilo Git

Este é um guia de estilo Git inspirado pelo [How to Get Your Change Into the Linux
Kernel](https://www.kernel.org/doc/Documentation/process/submitting-patches.rst),
pela [página do git](http://git-scm.com/doc) e várias práticas populares
dentro da comunidade.

### Branches

- Escolha nomes _curtos_ e _descritivos_:

  ```shell
  # bom
  $ git checkout -b oauth-migration

  # ruim - muito vago
  $ git checkout -b login_fix
  ```

- Identificadores correspondentes de tickets de um serviço externo (Ex. Issues do GitHub
) também são bons candidatos para usar em nomes de branches. Por exemplo:

  ```shell
  # GitHub Issue #15
  $ git checkout -b issue-15
  ```

- Use _traços_ para separar palavras.

- Quando várias pessoas estão trabalhando na _mesma_ funcionalidade, pode ser conveniente
  ter um branch de funcionalidade _pessoal_ e um branch de funcionalidade para a _equipe_.
  Use a seguinte convenção de nomenclatura:

  ```shell
  git checkout -b feature-a/master # Branch da equipe
  git checkout -b feature-a/maria  # Branch pessoal da Maria
  git checkout -b feature-a/nick   # Branch pessoal do Nick
  ```

Realizar o merge nos branchs pessoais para o branch da equipe (ver ["Merging"](#merging)).
Eventualmente, o branch da equipe será integrado ao "master".

- Apague seu branch do upstream do repositório depois de integrado (a menos
  que haja uma razão específica para não fazê-lo).

  Dica: Use o seguinte comando quando estiver no "master" para listar os branches
  que foram feitos merge:

  ```shell
  git branch --merged | grep -v "\*"
  ```

### Commits

- Cada commit deve ser uma _mudança lógica_ simples. Não faça várias
  _mudanças lógicas_ em um commit. Por exemplo, se uma alteração corrige um bug e
  otimiza a performance de uma funcionalidade, o divida em dois commits separados.

- Não divida uma _mudança lógica_ simples em vários commits. Por exemplo,
  a implementação de uma funcionalidade e os testes correspondentes à ela devem estar no mesmo commit.

- Commit _cedo_ e _frequentemente_. Commits pequenos e autônomos são mais fáceis de entender e reverter
  quando algo dá errado.

- Commits devem ser ordenados _logicamente_. Por exemplo, se _commit X_ depende
  de uma mudança feita no _commit Y_, então _commit Y_ deve vir antes do _commit X_.

### Mensagens

- Use o editor, não o terminal, quando estiver escrevendo a mensagem do commit:

  ```shell
  # bom
  $ git commit

  # ruim
  $ git commit -m "Correção rápida"
  ```

Por fim, quando estiver escrevendo uma mensagem do commit, pense sobre o que você precisaria saber olhando para o commit daqui um ano.

- Se um _commit A_ depende do _commit B_, a dependência deve ser evidenciada na mensagem
  do _commit A_. Use o hash do commit quando se referir a commits.

  Similarmente, se o _commit A_ corrige um bug introduzido pelo _commit B_, deve ser evidenciada na mensagem
  do _commit A_.

- Se um commit sofrerá squash de outro commit use o `--squash` e
`--fixup` sintaxes respectivamente, a fim tornar sua intenção clara:

```shell
git commit --squash f387cab2
```

*(Dica: Use a `--autosquash` marcação quando estiver realizando rebase. Os commits
terão o squash realizado automaticamente.)*

### Merging

- **Não reescreva histórico publicado.** O histórico do repositório é valioso a sua maneira e muito importante para permitir dizer _o que realmente aconteceu_. Alterar histórico publicado é uma fonte comum de problemas para qualquer um que trabalhe no projeto.

- Contudo, há casos em que reescrever o histórico é legítimo. Estes são quando:

  - Você é o único trabalhando no branch e não está sendo inspecionado.

  - Você quer arrumar seu branch (eg. commits squash ) e/ou realizar rebase dele para o "master" para realizar o merge depois.

  Dito isso, _nunca reescreva o histórico do branch "master"_ ou quaisquer
  branchs especiais (ie. usado em produção ou servidores de Integração Contínua).

- Mantenha o histórico _limpo_ e _simples_. _Bem antes de realizar o merge_ em seu branch:

    1. Tenha certeza que está em conformidade com o guia de estilo e realize qualquer ação necessária
       se não (squash/reordenar seus commits, refazer mensagens etc.)

    2. Rebase em outro branch em que será feito:

       ```shell
       [meu-branch] $ git fetch
       [meu-branch] $ git rebase origin/master
       # então merge
       ```

       Isto resulta em um branch que pode ser diretamente aplicado no final do
       branch "master" e resulta em um histórico bem simples.

       *(Nota: Esta estratégia é mais adequada para projetos com branches
        recentes. Caso contrário é melhor ocasionalmente realizar o merge do
        branch "master" em vez de fazer rebase nele.)*

- Se seu branch inclui mais de um commit, não faça merge como um branch avançado:

  ```shell
  # bom - garante que o commit de merge seja criado
  $ git merge --no-ff meu-branch

  # ruim
  $ git merge meu-branch
  ```

### Outras regras do Git

- Trabalhe em uma branch diferente da main.

  _Por que?:_

  > Porque desse jeito todo o código é criado isolado em uma branch específica ao invés de poluir a branch principal com trabalho em progresso. Isso vai permitir você abrir vários pull requets sem confusão. Você pode continuar com uma branch em progresso sem correr o risco de quebrar a branch principal com código instável. [Leia mais sobre...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)

- Nunca dê push direto na branch `main` ou `master`. Sempre faça Pull Requests.

  _Por que?_

  > Isso permite outros membros do time saberem que você terminou uma feature. Também possibilita code review e discussões sobre o código que está prestes a ser introduzido no code base.

- Atualize sua branch local e faça rebase interativo antes de subir sua feature e abrir um Pull Request.

  _Por que?_

  > Rebase vai fazer um merge do branch destino do pull request e aplicar os commits que você tem localmente no topo da história sem criar um commit de merge (assumindo que não tem conflitos). Como resultado você tem uma história limpa no seu repositório. [Leia mais sobre ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

- Resolva os conflitos enquanto faz o rebase e antes de abrir o Pull Request.

- Delete feature branches, local e remoto, depois de realizar o merge.

  _Por que?_

  > Vai reduzir sua lista de branches removendo branches mortas. Vai garantir que você apenas faça o merge de uma branch uma única vez. Feature branches só devem existir enquanto o código ainda está em progresso.

- Antes de fazer um Pull Request, tenha certeza que sua feature branch está fazendo build corretamente e passando em todos os testes (incluindo os padrões de estilo de código).

  _Por que?_

  > Você está prestes a colocar seu código em uma branch estável. Se sua feature branch faz algum teste falhar, a chance é alta de que você vai quebrar o build na branch destino. Você também precisa conferir o code style antes de fazer um Pull Request. Isso contribui para legibilidade e reduz a chance de algum problema de formatação is para o code base com as outras alterações.

- Faça uso desse [`.gitignore`](./.gitignore).

  _Por que:_

  > É uma lista que já contém arquivos de sistemas que não devem ser enviados para o seu repositório remoto. E também exclui pastas de configuração e os arquivos comumente usado por editores e obviamente, também, pastas de dependência.

- Proteja (Bloqueie) a `develop` e `master`.

  _Por que?_

  > Protege suas branchs que devem, em teoria, estarem prontas para irem para produção de receberem códigos e mudanças irreversíveis. Leia mais sobre... [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html) e [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)

### Escrevendo boas mensagens de commit

Ter um bom padrão para criar commits e se atentar a ele faz com que trabalhar com Git e colaborar com outros seja muito mais fácil. Aqui estão algumas boas práticas ([fonte](https://chris.beams.io/posts/git-commit/#seven-rules)):

- Separe o assunto e a mensagem com uma nova linha entre eles.

  _Por que?_

  > Git é inteligente o suficiente para identificar a primeira linha do seu commit como um resumo. Na verdade, se você tentar shortlog, ao invés de git log, você vai ver uma longa lista de mensagens de commits, com apenas o id e o resumo do commit.

- Máximo de 50 caracteres para o assunto e 72 para a mensagem.

  _Por que?_

  > Commits devem ser objetivos e claros, não é o momento para ser verboso. [Leia mais sobre...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

- Capitalize a linha do assunto.
- Não use um ponto para finalizar a linha do assunto.
- Use [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) na linha do assunto.

  _Por que?_

  > É melhor que o commit diga o que vai acontecer no projeto depois daquele commit do que o que o que aconteceu dentro do commit em si. [Lei mais sobre...](https://news.ycombinator.com/item?id=2079612)

- Use a mensagem para explicar **o que** e **porque** ao invés de **como**.

## Documentação dos projetos

- Use esse [modelo](./README.modelo.md) para `README.md`, sinta-se a vontade para adicionar secções que achar necessárias.
- Para projetos com mais de um repositório adicione todos os respetivos links nos `README.md` de todos os projetos.
- Mantenha o `README.md` enquanto o projeto evolui.
- Comente seu código. Tente sempre deixar claro o que uma grande parte do código tem a intenção de fazer.
- Se existe alguma referência em relação a forma como você resolveu o problema ou uma discussão em aberto, adicione os links.
- Não use comentários como desculpa para fazer um código ruim. Mantenha seu código limpo.
- Não use código limpo como uma desculpa para não fazer nenhum comentário.
- Mantenha apenas os comentários relevantes enquanto o código evolui.

## Testes

![Testes](/images/testing.png)

- Tenha um ambiente the `test` se necessário

  _Por que?_

  > Embora algumas vezes testes end to end em `produção` possam parecer suficientes, existem algumas exceções: Um exemplo é que você não vai querer colocar dados analíticos em `produção` e assim poluir o dashboard de alguém com dados de teste. Outro exemplo é que sua API pode ter algumas limitações enquanto em `produção` e chamadas de teste depois de uma certa quantidade.

- Coloque os arquivos de teste junto com os arquivos a serem testados usando a convenção `*.test.js` ou `*.spec.js` para nomear os arquivos, como `moduleName.spec.js`.

  _Por que?_

  > Você não quer ter que navegar em várias pastas para achar um teste unitário. [Leia mais sobre...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

- Coloque seus arquivos de testes adicionais em uma pasta separada para evitar confusão.

  _Por que?_

  > Alguns arquivos de testes não tem nenhuma relação com qualquer outro arquivo. Você deve coloca-los em uma pasta fácil de ser encontrada pelos outros desenvolvedores do time, como por exemplo: Uma pasta `__test__`. Essa nomeação é padrão e reconhecida pela maioria de frameworks de teste de JavaScript.

- Escreva código testável, evite efeitos colaterais (side effects), escreva funções puras

  _Por que?_

  > Você vai querer testar uma regra de negócio como uma unidade separada. Voce tem que "minimizar o impacto de aleatoriedade e processos não determinísticos no seu código". [Leia mais sobre...](https://medium.com/javascript-scene/tdd-the-rite-way-53c9b46f45e3)
  > Uma função pura é uma função que sempre retorna o mesmo valor para uma entrada específica. Por outro lado, uma função impura é uma função que pode ter efeitos colaterais e depender de condições externas para retornar algum valor. Isso reduz a capacidade de prever o que o código vai realizar. [Leia mais sobre...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

- Use uma checagem de tipo estática

  _Por que?_

  > As vezes você vai precisar de checagem de tipo estática. O que também aumenta a regidibilidade e legibilidade do seu código. [Leia mais sobre...](https://medium.freecodecamp.org/why-use-static-types-in-javascript-part-1-8382da1e0adb)

- Rode os testes localmente antes de abrir um pull request para  `main`.

  _Por que?_

  > Você não quer ser a pessoa a fazer com que a branch com código pronto para produção pare de funcionar. Rode seus teste depois que fizer `rebase` e antes de fazer push para sua feature branch.

- Documente seus testes incluindo instruções importantes em uma seção no arquivo `README.md`.

  _Por que?_

  > Vai ser de muita ajuda para outros desenvolvedores, DevOps, QA ou qualquer um que tiver a sorte de trabalhar com seu código.

## Estilo de código

![Code style](/images/code-style.png)

Use as guias de estilo no repositório para cada linguagem em projetos.

- [HTML](html/README.md)
- [CSS & SASS](css-sass/README.md)
- [Javascript](javascript/README.md)
- [PHP](javascript)

- Dependendo do tamanho da task, use comentários com `//TODO:` para ajudar na criação de novas tasks para o backlog.

  _Por que?_

  > Você vai deixar um lembrete para os outros, e para você mesmo, de pequenas tarefas ou correções (como refatorar uma função ou atualizar um comentário). Para tarefas maiores escreva `//TODO(#3456)` fazendo referência ao ticket aberto no backlog para aquela task.

- Sempre faça comentários relevantes. Delete código morto ou comentado.

  _Por que?_

  > Você deve prezar pela legibilidade do seu código, então se livre de qualquer distração possível no código. Se você refatorou uma função, não deixe a antiga lá apenas comentada, delete-a.

- Evite comentários irrelevantes, engraçados ou ofensivos.

  _Por que?_

  > Mesmo que seu processo de build possa remove-los, as vezes seu código pode ser pego por alguém diferente, uma empresa terceirizada ou um chefe de outra área e isso pode não ser tão tranquilo.

- Use nomes com significados, fáceis de pesquisar e sem abreviações para suas variáveis ou funções. O nome de uma função deve ser um verbo ou uma frase e precisa de deixar claro a sua intenção.

  _Por que?_

  > Faz com que o seu código seja mais legível e natual.

## Ferramentas

- [Git](https://git-scm.com/)
- [Github](https://github.com/)
- [Visual Studio Code](https://code.visualstudio.com/)

## Extensões VSCode

Lista de algumas extensões que serão úteis para os desenvolvedores.

- [Better Comments](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)
- [Color Highlight](https://marketplace.visualstudio.com/items?itemName=naumovs.color-highlight)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens)
- [Error Lens](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css)
- [IntelliCode](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode)
- [IntelliSense for CSS](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion)
- [JavaScript (ES6) Code Snippets](https://marketplace.visualstudio.com/items?itemName=xabikos.JavaScriptSnippets)
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
- [Live Share](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare)
- [Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)
- [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)
- [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)
- [Prettier - Code formator](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

## Aprenda

- [Por que temos padrões de código e guias de estilo](https://digitalcommunications.wp.st-andrews.ac.uk/2016/12/01/why-we-have-code-standards/)
- [Padrões e Diretrizes de Codificação](https://www.geeksforgeeks.org/coding-standards-and-guidelines/)

## Referência

[Padrões de Projeto - ElseWhenCode](https://github.com/elsewhencode/project-guidelines/blob/master/README-pt-BR.md)
