# 💬 Automação de Triagem: Do WhatsApp para a Gestão de Tarefas

**Resumo Executivo:** Orquestração de um agente de IA projetado para resolver o gargalo de atendimento e gestão de projetos. A solução lê conversas específicas do WhatsApp, transcreve áudios localmente, extrai promessas e prazos, e roteia automaticamente as tarefas para os repositórios corretos, com um sistema de aprovação humana (*human-in-the-loop*) antes da gravação dos dados.

*(Nota: O código fonte deste projeto é mantido privado por conter dados pessoais e sensíveis de clientes. Este documento detalha a arquitetura da automação e os ganhos operacionais focados na trilha de AI Builder.)*

---

## 🚩 O Desafio Operacional (A Dor)
O gerenciamento de contas e atendimento via WhatsApp gera um alto custo operacional. Perdem-se horas por semana apenas relendo conversas de clientes para resgatar o que foi prometido, quem fez o pedido e quais são os prazos. Isso resulta em falhas de comunicação e perda de produtividade na transição entre o chat e a plataforma de gestão de tarefas.

## 💡 Arquitetura e Fluxograma da Solução

O sistema foi desenhado para atuar como um assistente de triagem inteligente e autônomo, estruturado no seguinte fluxo de ponta a ponta:

<img width="2154" height="702" alt="image" src="https://github.com/user-attachments/assets/fe874f36-a9c6-4499-a316-b928f4cfad17" />


1. **Entrada e Coleta:** O fluxo inicia com um pedido direcionado (ex: "rele Fulano"), que instrui o sistema a extrair estritamente os dados daquela conversa específica no WhatsApp.
2. **Processamento Multimodal (Serviço Local):** Um serviço dedicado transcreve áudios (utilizando o modelo Whisper localmente) e processa os prints e fotos da conversa.
3. **Validação de Integridade:** Um nó de decisão ("Buraco?") verifica se há lacunas de sincronia no histórico do computador. Se faltarem dados, o fluxo é interrompido e o sistema emite uma saída pedindo a exportação manual, em vez de concluir erroneamente que o cliente não respondeu.
4. **Extração e Roteamento (Agentes):** 
   * O primeiro agente ("Separa tarefas") filtra o que é pessoal e extrai apenas as demandas de quem iniciou a solicitação (pedido feito, promessa e prazo combinado).
   * O sistema cruza essas informações com a base de dados de "Projetos" e "Pendências" atuais
