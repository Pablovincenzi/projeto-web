# SysmoCloud

Esse projeto foi gerado utilizando [Nx](https://nx.dev).

<p style="text-align: center;">
<img src="https://raw.githubusercontent.com/nrwl/nx/master/images/nx-logo.png" width="450">
</p>

# Configurar VSCode (extensões e preferências)

1 - Pressione as teclas Ctrl+P e na caixa que aparece no topo da tela informe o seguinte comando `ext install Shan.code-settings-sync` e em seguida pressione enter;

2 - Na tela que se abre, clique na opção em azul "Download from public gist". Na caixa que aparece no topo da tela informe o seguinte código `c675c2326e95419233952e0d41e82106` e pressione enter;

3 - Após isso, o VSCode irá solicitar para ser recarregado, clique na opção "Sim". Após recarregado, seu VSCode estará pronto para uso.
<br><br><br>

## Gerar um microfrontend

Execute o comando<br>
`npx nx g @nx/angular:application --directory apps/nome-do-microfrontend --name=nome-do-microfrontend --routing=true --port=porta --tags=scope:nome-do-microfrontend,type:app --no-interactive`

> Substitua `nome-do-microfrontend` pelo nome desejado (no padrão sneak-case) e `porta` pelo número da porta local para startar o microfrontend (verifique o arquivo `inicio.component` para saber qual número de porta utilizar).

Com isso, será gerado um novo microfrontend dentro do diretório apps e também um app com o mesmo nome do microfrontend com o sufixo `-e2e`, para os testes end-to-end.

Execute o comando<br>
`npx nx g @angular-architects/module-federation:config --port=porta --project=nome-do-microfrontend --nxBuilders=true --type=remote`

Com isso, será feita a configuração para que o microfrontend seja consumido pela aplicação principal (shell).

> Substituia `nome-do-microfrontend` pelo mesmo nome utilizado anteriormente e `porta` pela mesma porta utilizada anteriormente.

Delete o arquivo `.gitkeep` que foi gerado dentro do diretório `assets` do novo microfrontend.

Descarte as mudanças feitas aos arquivos `package.json`e `package-lock.json`, pois ela não são necessárias.

Adeque os arquivos da pasta `src` conforme a pasta `src` dos outros microfrontends (ex.: promocao).

Adeque o arquivos `project.json`, `tsconfig.app.json`, `tsconfig.json`, `webpack.config.js` conforme os mesmos arquivos dos outros microfrontends (ex.: promocao).

> ATENÇÃO: No `project.json` lembre-se de adicionar o atributo `deployUrl` nas configurações de produção e de mover o array `assets` para as configurações de desenvolvimento.

Adicione microfrontend criado ao final do arquivo `.eslintrc.json`, que fica na raiz do repositório.

No projeto e2e do microfrontend criado (-e2e), adequar conforme e2e de outros projetos (ex.: marca).

## Gerar uma biblioteca

Existem 5 tipos de bibliotecas no Nx:<br>

1. Data-access
2. Feature
3. Util
4. UI (user interface)
5. App

O Data-access serve para encapsular as requisições enviadas para o backend, para manipular os dados recebidos dessas requisições e para realizar control de estado global.

A Feature serve para as funcionalidades, ex.: lista principal, form principal, formulários secundários, modais.

O Util serve para funções utilitárias, como validações de formulários, formatadores.

A UI serve para componentes visuais "burros", que não possuem lógica, somente recebem parâmetros e emitem eventos.

O App representa os microfrontends/e2e.

Abaixo seguem alguns comandos para gerar bibliotecas:

`npx nx g @nx/angular:library nome-do-microfrontend-data-access --directory libs/nome-do-microfrontend/data-access --tags=scope:nome-do-microfrontend,type:data-access --skipTests`

`npx nx g @nx/angular:library feature-lista-nome-do-microfrontend --directory libs/nome-do-microfrontend/feature-lista-nome-do-microfrontend --tags=scope:nome-do-microfrontend,type:feature  --skipTests`

`npx nx g @nx/angular:library feature-form-nome-do-microfrontend --directory libs/nome-do-microfrontend --tags=scope:nome-do-microfrontend,type:feature --skipTests`

`npx nx g @nx/angular:library feature-forms --directory libs/nome-do-microfrontend --tags=scope:nome-do-microfrontend,type:feature --skipTests`

`npx nx g @nx/angular:library util-forms --directory libs/nome-do-microfrontend --tags=scope:nome-do-microfrontend,type:util --skipTests`

`npx nx g @nx/angular:library shell-ui-badge --directory libs/shell/ui-badge --tags=scope:shell,type:ui --skipTests`

> Lembre-se de substituir `nome-do-microfrontend` pelo nome correto do microfrontend, no padrão sneak-case.

> Após gerar uma biblioteca é necessário reiniciar o VSCode para que ele possa reconhecer o caminho da nova biblioteca. Isso pode ser feito pressionando as teclas `Ctrl+Shift+P` e selecionando a opção `Developer: Reload Window`.

> Após criar biblioteca, remover as seguintes linhas `"noImplicitOverride": true` e `"noPropertyAccessFromIndexSignature": true`

## Iniciar microfrontends

Existem duas maneiras de iniciar os microfrontends:

1. Abre o arquivo `project.json` localizado em `apps/shell`, pesquisa por `serve-mfe` e dentro do array `commands` deixe somente os microfrontends que você deseja iniciar, além do `login` e da `shell`, caso contrário sua máquina não irá aguentar iniciar todos os microfrontends (vai por mim hehe).<br>
   Após realizar essa alteração, execute o comando `npm run serve:all` para iniciar os microfrontends e abra o navegador no endereço http://localhost:4200.

<br>

2. Execute o comando `npx nx run-many --target serve --projects shell, login, microfrontend-desejado`

> O único porém dessa segunda opção é que o comando somente consegue executar **3 microfrontends por vez**, atente-se a isso.

## Criar testes de componente

Para criar teste de componente, é necessário possuir a extensão `Nx Console` instalada no VSCode.

Tendo a extensão instalada, clique com o botão direito sobre o arquivo que deseja criar o teste de componente.

Selecione a opção `Nx generate` e em seguida, no painel superior que se abriu, pesquise pela opção `@nx/angular - cypress-component-configuration`

Na página que se abriu, clique na opção `show all options` no canto inferior direito e marque o checkbox `generateTests` para já criar os testes de componente para todos os componentes daquela biblioteca. Em seguida clique no botão `Generate` para criar os arquivos e configurar o Cypress para testes de componente nessa biblioteca.

Após criar os arquivos, adeque-os com base em outros testes de componente (ex.: arquivos da biblioteca `modal-sincronizar-dados-produto-empresa.component.cy.ts`)

## Rodar testes de componente

Para rodar os teste de componente, clique com o botão direito sob o teste de componente (arquivo com o sufixo `.component.cy.ts`) e selecione a opção `Run target from selected file`, no painel superior que se abriu, marque a opção `component-test`.

Se você deseja visualizar a execução do teste, selecione a opção `watch`, marque ela como `true` e pressione enter. Caso não deseje, pressione enter para rodar o comando para executar o teste de componente.

## Rodar testes end-to-end

Para rodar os teste de componente, clique com o botão direito sob o teste end-to-end (arquivo com o sufixo `.e2e.cy.ts`) e selecione a opção `Run target from selected file`, no painel superior que se abriu, marque a opção `e2e`.

Se você deseja visualizar a execução do teste, selecione a opção `watch`, marque ela como `true` e pressione enter. Caso não deseje, pressione enter para rodar o comando para executar o teste de componente.
<br><br><br>

# Rodar comandos para arquivos afetados

Rodar comandos somente para os arquivos afetados é muito útil, pois somente irá validar/verificar o que foi alterado pelas suas modificações e garantirá que o código desenvolvido não quebrou nenhuma funcionalidade.

## Lint

Para rodar o lint execute o seguinte comando: `npx nx affected --target=lint --parallel=4 --maxWarnings=0`

## Component test

Para rodar os testes de componente execute o seguinte comando: `npx nx affected --target=component-test --parallel=false`

## End-to-end

Para rodar os testes end-to-end execute o seguinte comando: `npx nx affected --target=e2e --parallel=false`

## Mover libs e renomear

Para mover uma lib de um escopo para outro e renomear, utilizar o seguinte comando, por exemplo:

`npx nx g mv --projectName nota-entrada-feature-modal-erros-nota --destination libs/nota-fiscal/feature-modal-erros-nota --newProjectName nota-fiscal-feature-modal-erros-nota`
