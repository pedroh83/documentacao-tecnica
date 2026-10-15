# Instalação do servidor BPMN (MCP)

**Procedimento operacional** — para criar diagramas de processo com o Claude Desktop no Windows

---

## 1. Objetivo

Orientar a instalação de um servidor MCP (Model Context Protocol) de BPMN no Claude Desktop, para que o Claude consiga criar, editar e exportar diagramas de processo em BPMN 2.0 diretamente na conversa.

## 2. Aplicação

Aplica-se a computadores com Windows 10 ou 11. Este procedimento usa o projeto público **BPMN-MCP** (github.com/dattmavis/BPMN-MCP), executado localmente no computador.

> **Sobre a versão:** existem vários servidores BPMN para MCP, alguns com mais recursos (layout automático, raias, exportação em PNG). A instalação de todos segue o mesmo padrão. Para usar outro projeto, troque apenas o endereço do repositório (passo 5.4.2) e o caminho do arquivo no passo 5.5.3, seguindo as instruções do README desse projeto.

## 3. Conceitos rápidos

| Termo | O que é |
|---|---|
| MCP | Padrão que permite ao Claude usar ferramentas externas instaladas no seu computador. |
| Servidor MCP | Pequeno programa que oferece essas ferramentas. Aqui, as ferramentas de desenhar BPMN. |
| Node.js / npm | Ambiente que executa o servidor (Node.js) e instala suas dependências (npm). |
| Git | Programa que baixa o código do servidor a partir do GitHub. |
| `claude_desktop_config.json` | Arquivo de configuração onde você diz ao Claude Desktop quais servidores MCP iniciar. |

## 4. Pré-requisitos

| Item | Requisito |
|---|---|
| Sistema operacional | Windows 10 ou 11 |
| Claude Desktop | Aplicativo instalado e com login feito |
| Node.js | Versão LTS atual (o projeto exige no mínimo v16) |
| Git | Qualquer versão atual |
| Internet | Para baixar os programas e as dependências |
| Conhecimento | Abrir o Prompt de Comando e colar comandos |

## 5. Procedimento

### 5.1 Instalar o Claude Desktop

1. Acesse **claude.ai/download** e baixe a versão para Windows.
2. Execute o instalador e faça login com sua conta Claude.
3. Se o Claude Desktop já estiver instalado, apenas confirme que está atualizado.

### 5.2 Instalar o Node.js

1. Acesse **nodejs.org** e baixe a versão **LTS** para Windows.
2. Execute o instalador mantendo as opções padrão (inclusive a opção que adiciona o Node ao PATH).
3. Abra um **novo** Prompt de Comando (tecla Windows, digite `cmd`, Enter) e confira:

```cmd
node -v
npm -v
```

Os dois comandos devem mostrar números de versão. Se aparecer "não é reconhecido como comando", feche e abra o Prompt de novo; se persistir, reinicie o computador.

### 5.3 Instalar o Git

1. Acesse **git-scm.com**, baixe o instalador para Windows e instale com as opções padrão.
2. Em um novo Prompt de Comando, confira:

```cmd
git --version
```

### 5.4 Baixar e compilar o servidor BPMN

1. No Prompt de Comando, vá para sua pasta de usuário:

   ```cmd
   cd %USERPROFILE%
   ```

2. Baixe o projeto:

   ```cmd
   git clone https://github.com/dattmavis/BPMN-MCP.git
   ```

3. Entre na pasta e instale as dependências (pode levar alguns minutos):

   ```cmd
   cd BPMN-MCP
   npm install
   ```

4. Compile o projeto:

   ```cmd
   npm run build
   ```

5. Confirme que o arquivo principal foi gerado e anote o caminho completo mostrado:

   ```cmd
   dir dist\index.js
   cd
   ```

O caminho será parecido com `C:\Users\SEU_USUARIO\BPMN-MCP\dist\index.js`.

### 5.5 Configurar o Claude Desktop

1. Abra o Claude Desktop e vá em **Configurações (Settings) > Desenvolvedor (Developer)**.
2. Clique em **Edit Config**. Isso abre a pasta com o arquivo `claude_desktop_config.json`. Abra esse arquivo no Bloco de Notas.
3. Se o arquivo estiver vazio ou só com `{}`, substitua o conteúdo por:

   ```json
   {
     "mcpServers": {
       "bpmn": {
         "command": "node",
         "args": ["C:\\Users\\SEU_USUARIO\\BPMN-MCP\\dist\\index.js"]
       }
     }
   }
   ```

4. Troque `SEU_USUARIO` pelo seu nome de usuário do Windows. Repare que cada barra do caminho é **dupla** (`\\`). Esse é o erro mais comum.
5. Salve o arquivo.

> ⚠️ **Arquivo já tem outros servidores?** Não apague o que existe. Acrescente só o bloco `"bpmn"` dentro de `"mcpServers"`, separando os blocos com vírgula. Um JSON com vírgula sobrando ou faltando impede todos os servidores de carregar.

### 5.6 Reiniciar o Claude Desktop

1. Feche o Claude Desktop **completamente**: clique com o botão direito no ícone do Claude na bandeja do sistema (perto do relógio) e escolha **Sair**. Fechar só a janela não basta, porque o aplicativo continua rodando e não relê a configuração.
2. Abra o Claude Desktop novamente.

### 5.7 Testar

1. Em Configurações > Desenvolvedor, confirme que o servidor **bpmn** aparece como em execução (running).
2. Em uma nova conversa, envie um pedido de teste, por exemplo:

   ```text
   Crie um diagrama BPMN simples: início, tarefa "Receber pedido",
   decisão "Pedido válido?", e fim. Depois exporte o XML.
   ```

3. O Claude deve pedir permissão para usar as ferramentas do servidor BPMN. Autorize e confira se o diagrama é gerado.

### 5.8 Salvar o diagrama em arquivo

1. Peça ao Claude para exportar o diagrama em XML (BPMN).
2. Copie o XML, cole no Bloco de Notas e salve com a extensão `.bpmn` (em "Tipo", escolha "Todos os arquivos").
3. Abra o arquivo em qualquer ferramenta compatível com BPMN 2.0, como o bpmn.io (no navegador) ou o Camunda Modeler.

## 6. Verificação da instalação

- [ ] `node -v` e `git --version` respondem com versões.
- [ ] O arquivo `dist\index.js` existe na pasta do projeto.
- [ ] O servidor "bpmn" aparece em execução em Configurações > Desenvolvedor.
- [ ] O Claude cria e exporta um diagrama de teste.

## 7. Problemas comuns

| Problema | O que fazer |
|---|---|
| "node" ou "git" não é reconhecido | Abra um novo Prompt de Comando após instalar. Se persistir, reinicie o computador ou reinstale marcando a opção de adicionar ao PATH. |
| Erro no `npm install` | Verifique a conexão. Em rede corporativa, o proxy pode bloquear o npm; teste em outra rede ou acione a TI. |
| Servidor não aparece no Claude | Confirme que saiu pela bandeja do sistema antes de reabrir. Revise o JSON: barras duplas, aspas e vírgulas. |
| Servidor aparece com erro (failed) | Confira se o caminho do `index.js` está correto. Os registros ficam em `%APPDATA%\Claude\logs` (arquivos `mcp*.log`). |
| Nada muda mesmo com o JSON correto | Em algumas instalações do Claude Desktop pela Microsoft Store, o aplicativo lê o arquivo em `%LOCALAPPDATA%\Packages\Claude_...\LocalCache\Roaming\Claude\`. Copie o mesmo conteúdo para esse arquivo também. |
| O Claude não usa o servidor | Peça explicitamente: "use a ferramenta BPMN para...". Verifique também se as ferramentas estão habilitadas no menu da conversa. |

## 8. Segurança

- Servidores MCP locais executam código no seu computador. Instale apenas projetos de fontes conhecidas e confira o repositório antes.
- Em computador da empresa, confirme com a TI se a instalação de Node.js, Git e servidores MCP é permitida.

## 9. Referências

- Repositório do servidor: github.com/dattmavis/BPMN-MCP
- Central de ajuda do Claude: support.claude.com (artigo sobre servidores MCP locais no Claude Desktop)
- Documentação do MCP: modelcontextprotocol.io

## 10. Controle de revisões

| Revisão | Data | Descrição |
|---|---|---|
| 00 | 06/10/2026 | Emissão inicial |
