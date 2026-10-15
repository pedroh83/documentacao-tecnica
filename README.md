# Documentação Técnica: Procedimentos Operacionais

Procedimentos que escrevi para configurar ferramentas de tecnologia e IA no Windows, no padrão de documentação de Sistemas de Gestão (objetivo, escopo, procedimento, verificação, tratamento de falhas e controle de revisões).

Venho da área de Qualidade, onde um processo só está pronto quando outra pessoa consegue repeti-lo a partir do documento. Estes POPs seguem essa regra: foram escritos durante a implantação real e testados até o critério de aceitação.

---

## Procedimentos

| Documento | O que resolve | Destaques |
|---|---|---|
| [Instalação do servidor BPMN (MCP) no Claude Desktop](POP_instalacao_servidor_bpmn_mcp.md) | Permite que o Claude crie, edite e exporte diagramas de processo em BPMN 2.0 | Node.js, Git, configuração JSON do MCP, testes de verificação e tabela de problemas comuns |
| [Inicialização automática de app no Windows](POP_inicializacao_automatica_app_windows.md) | Faz um app web instalado pelo Chrome abrir sozinho ao ligar o computador | Pasta de Inicialização, Gerenciador de Tarefas, critério de aceitação e como desativar |

## Estrutura padrão dos documentos

1. Objetivo e escopo
2. Pré-requisitos e conceitos
3. Procedimento passo a passo
4. Verificação e critério de aceitação
5. Solução de problemas (sintoma → causa → ação)
6. Segurança
7. Controle de revisões

## Relacionados

- [Agente de Recolocação](https://github.com/pedroh83/agente-recolocacao): POP e guia de dois agentes em Python.
- [Prompt Master](https://github.com/pedroh83/prompt-master): Instrução de Trabalho para engenharia de prompts.

---

**Pedro Henrique Simas**: Qualidade, Processos e Requisitos | Salvador, BA  
[LinkedIn](https://www.linkedin.com/in/pedro-henrique-simas/)
