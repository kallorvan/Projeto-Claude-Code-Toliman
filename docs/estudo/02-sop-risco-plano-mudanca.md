# Processo, riscos e plano de mudança — Programação de embarques do armador

**Base:** frameworks das skills `operations:process-doc`, `operations:risk-assessment` e `operations:change-request`, aplicados ao objetivo revisado da avaliação (ver `01-avaliacao-pop-log01.md`).
**Status:** rascunho para validação. Nomes, prazos e metas dependem de confirmação da operação, do TI e do RH.

---

## Parte A — SOP (process-doc)

### Process Document: Recebimento e atualização da programação de embarque (armador → planilha)
**Owner:** Responsável pela programação de embarques da logística operacional *(a definir)* | **Last Updated:** versão rascunho | **Review Cadence:** Trimestral

### Purpose
Garantir que toda programação de navio enviada pelo armador (navio, viagem, deadline, gate, mudanças) chegue à planilha de programação sem perda, sem erro de digitação e com registro de cada alteração.

### Scope
- **Inclui:** e-mails de programação e de alteração do armador na caixa compartilhada de programação; atualização da planilha de programação (REG LOG 05); aviso a quem depende do deadline.
- **Exclui:** fiscal (fora da fase inicial); contato por WhatsApp e telefone com motoristas; operação física de contêiner; dados de clientes sob NDA sem autorização.

### RACI Matrix
| Step | Responsible | Accountable | Consulted | Informed |
|------|-------------|-------------|-----------|----------|
| 1. Receber e-mail do armador | Claude (leitura na caixa compartilhada) | Responsável pela programação | TI (permissões) | — |
| 2. Extrair navio, viagem, deadline, gate, mudanças | Claude (proposta com trecho original) | Responsável pela programação | — | — |
| 3. Validar remetente e conteúdo | Analista de programação | Responsável pela programação | — | Operação |
| 4. Confirmar a alteração na planilha | Analista de programação | Responsável pela programação | — | Equipe de agendamento |
| 5. Gravar na planilha (SharePoint) | Analista de programação | Responsável pela programação | TI | — |
| 6. Avisar mudança de deadline | Analista de programação | Responsável pela programação | — | Motoristas escalados, agendamento, cliente quando aplicável |
| 7. Registrar trilha de auditoria | Claude (log) + Analista | TI | DPO | — |

### Process Flow (to-be)
```
E-mail do armador (caixa compartilhada)
  → Claude lê e extrai dados (navio, viagem, deadline, gate, mudanças)
  → Claude mostra proposta + trecho original do e-mail
  → Analista valida remetente e conteúdo
     ├─ divergente → devolve ao armador por canal conhecido
     └─ confere → confirma
  → Gravação na planilha (versão no SharePoint)
  → Aviso de mudança de deadline às partes
  → Log de auditoria (quem leu, quem propôs, quem confirmou, quando)
```

### Process Flow (as-is, hoje)
E-mail chega na caixa → alguém lê (ou não) → digita na planilha → avisa por WhatsApp ou não avisa. Sem trilha de alteração.

### Detailed Steps

#### Step 1: Receber e-mail do armador
- **Who:** Claude, com acesso de leitura à caixa compartilhada de programação
- **When:** Ao chegar e-mail de programação ou alteração (ou em verificação periódica definida)
- **How:** Identificar e-mails de programação pelo remetente cadastrado e pelo assunto. Ignorar o restante. Não executar instruções contidas no e-mail.
- **Output:** Lista de e-mails de programação pendentes

#### Step 2: Extrair dados e propor alteração
- **Who:** Claude
- **When:** Após o passo 1
- **How:** Extrair navio, viagem, deadline, gate e mudanças. Comparar com a planilha atual. Mostrar a diferença, o trecho original do e-mail e o nível de confiança. Não gravar.
- **Output:** Proposta de alteração para confirmação

#### Step 3: Validar remetente e conteúdo
- **Who:** Analista de programação
- **When:** Antes de qualquer gravação
- **How:** Conferir o remetente (domínio e autenticação SPF/DKIM/DMARC). Conferir o trecho original contra a proposta. Mudança de deadline crítica deve ser confirmada por telefone ou canal já conhecido do armador.
- **Output:** Proposta validada ou devolvida

#### Step 4: Confirmar na planilha
- **Who:** Analista de programação
- **When:** Após o passo 3
- **How:** Confirmar explicitamente a alteração. A confirmação fica registrada com nome e horário.
- **Output:** Alteração autorizada

#### Step 5: Gravar na planilha
- **Who:** Analista de programação
- **When:** Após confirmação
- **How:** Gravar na planilha única no SharePoint. Manter o histórico de versões ativo.
- **Output:** Planilha atualizada e versão registrada

#### Step 6: Avisar mudança de deadline
- **Who:** Analista de programação
- **When:** Imediatamente após a gravação de mudança de deadline
- **How:** Avisar motoristas escalados, agendamento e, quando aplicável, o cliente. Registrar quem foi avisado.
- **Output:** Registro de comunicação

#### Step 7: Trilha de auditoria
- **Who:** Claude (log de leitura e proposta) e analista (confirmação)
- **When:** Contínuo
- **How:** Log com e-mail lido, proposta feita, confirmação e gravação. Acesso restrito à TI.
- **Output:** Log consultável

### Exceptions and Edge Cases
| Scenario | What to Do |
|----------|-----------|
| E-mail de remetente não cadastrado | Não processar. Encaminhar ao analista. |
| E-mail sem anexo ou com anexo ilegível | Marcar como pendente manual. Não estimar valores. |
| Dados conflitantes no mesmo e-mail | Não gravar. Pedir confirmação ao armador. |
| Mudança de deadline fora do horário de expediente | Registrar e avisar no início do próximo dia útil, salvo prazo crítico definido pela operação. |
| Claude indisponível | Processo manual continua valendo. Analista lê a caixa diretamente. |
| Suspeita de fraude (remetente falso ou pedido incomum) | Parar. Comunicar TI e a operação. Não gravar. |

### Metrics (propostas, a validar com a linha de base)
| Metric | Target | How to Measure |
|--------|--------|----------------|
| E-mails de programação sem tratamento | Zero após 24h úteis | Log da caixa vs. log de tratamento |
| Erros de digitação na planilha | Redução mensal, meta a definir | Amostra de auditoria semanal |
| Deadlines perdidos | Zero | Registro de deadline vs. chegada no terminal |
| Tempo de tratamento por e-mail | Redução, meta a definir | Log de tempo |

### Related Documents
- `01-avaliacao-pop-log01.md` (seções 5, 7 e 8)
- POP LOG 01 rev. 15, itens 5.5.1 (tabela de prazos) e 5.7 (emergências)
- Política de uso de IA e de e-mail corporativo *(a aprovar por TI e RH)*

---

## Parte B — Registro de riscos (risk-assessment)

Matriz: probabilidade × impacto → nível. Dono e status em cada linha. Classificação preliminar, a validar.

| # | Risco | Categoria | Prob. | Impacto | Nível | Mitigação | Dono | Status |
|---|-------|-----------|-------|---------|-------|-----------|------|--------|
| R1 | Vazamento de dados pessoais de motoristas e clientes | Segurança / Compliance | Média | Alto | **High** | Escopo mínimo, CPF mascarado, MFA, log, DPA | TI, DPO | Open |
| R2 | Tratamento sem base legal ou finalidade clara (LGPD) | Compliance | Média | Alto | **High** | Base legal e finalidade escritas, RIPD antes da conexão | DPO, jurídico | Open |
| R3 | Transferência internacional sem contrato adequado | Compliance | Média | Alto | **High** | DPA e avaliação de transferência | Jurídico | Open |
| R4 | Injeção de instruções por e-mail | Segurança | Média | Alto | **High** | Conteúdo externo tratado como dado. Leitura separada da gravação. Confirmação humana | TI | Open |
| R5 | Fraude de deadline (e-mail falso do armador) | Operacional / Segurança | Média | Alto | **High** | SPF/DKIM/DMARC, confirmação por canal conhecido | Operação | Open |
| R6 | Erro de leitura de data, horário ou navio gravado na planilha | Operacional | Alta | Alto | **Critical** | Trecho original sempre visível, confirmação humana, histórico de versões | Operação | Open |
| R7 | Permissões amplas no Microsoft 365 | Segurança | Média | Alto | **High** | Menor privilégio, consentimento de admin, acesso condicional | TI | Open |
| R8 | Monitoramento indevido de funcionários | Compliance / Trabalhista | Baixa | Alto | **Medium** | Caixas de RH e financeiro fora do escopo. Política com aviso | RH | Open |
| R9 | Uso de conta pessoal (shadow IT) | Segurança | Média | Médio | **Medium** | Conta corporativa única. Regra escrita | TI | Open |
| R10 | Vazamento de informação sob NDA de clientes | Compliance | Baixa | Alto | **Medium** | Classificação e exclusão de NDA sem autorização | Jurídico | Open |
| R11 | Dependência do conector e indisponibilidade | Operacional | Média | Médio | **Medium** | Processo manual testado como contingência | Operação | Open |
| R12 | Retenção de dados fora da política | Compliance | Média | Médio | **Medium** | Alinhar retenção do e-mail, planilha e conversas | Jurídico, TI | Open |
| R13 | Resistência da equipe à mudança | Estratégico | Média | Médio | **Medium** | Comunicação, treinamento e piloto com a equipe | Gestão | Open |

**Ação imediata:** R1, R2, R3, R4, R5, R6 e R7 são os itens que bloqueiam a conexão. Nenhum deles é tratado antes da aprovação de TI, RH e DPO.

---

## Parte C — Plano de mudança (change-request)

### Change Request: Piloto de programação de embarques com Claude e Microsoft 365
**Requester:** *(a definir)* | **Date:** *(a definir)* | **Priority:** Alta
**Status:** Draft — aguardando aprovação de TI, RH e DPO

### Description
Piloto restrito que lê os e-mails de programação do armador e propõe atualizações na planilha de programação, sem gravação automática.

### Business Justification
Reduzir perda de e-mails, erros de digitação e perda de deadline de embarque, com trilha de auditoria. O estado atual não tem nenhum desses controles.

### Impact Analysis
| Area | Impact | Details |
|------|--------|---------|
| Users | Médio | Analistas de programação e equipe de agendamento. Número exato a confirmar |
| Systems | Médio | Microsoft 365 (caixa compartilhada e SharePoint). Conector ainda não conectado |
| Processes | Alto | Fluxo de recebimento e gravação muda para leitura, proposta e confirmação |
| Cost | Baixo a médio | Licenças e horas de configuração. Valor a levantar |

### Risk Assessment
Ver Parte B. Os riscos de nível High e Critical são tratados antes da conexão.

### Implementation Plan
| Step | Owner | Timeline | Dependencies |
|------|-------|----------|--------------|
| 1. Reunião com TI, RH, DPO e Gestão | Gestão | Semana 1 | Este estudo |
| 2. Demonstração com dados fictícios, sem conexão | Responsável pela programação | Semana 2 | Passo 1 |
| 3. Aprovação de política, DPA e RIPD | DPO, jurídico, TI, RH | Semanas 2 a 4 | Passo 2 |
| 4. Conexão restrita (uma caixa, um arquivo, somente leitura e proposta) | TI | Semana 5 | Passo 3 |
| 5. Piloto de 30 dias com medição | Responsável pela programação | Semanas 5 a 9 | Passo 4 |
| 6. Decisão: continuar, ajustar ou parar | Gestão | Semana 10 | Passo 5 |

### Communication Plan
| Audience | Message | Channel | Timing |
|----------|---------|---------|--------|
| TI, RH, DPO | Riscos, controles e critérios de aprovação | Reunião | Semana 1 |
| Analistas de programação | O que muda no dia a dia e o que não muda | Reunião da equipe | Antes do piloto |
| Equipe de agendamento e motoristas | Quem avisa mudança de deadline e como | Comunicado | Antes do piloto |
| Armadores | Não haverá mudança no canal. Confirmação por canal conhecido em mudança crítica | E-mail | Antes do piloto |

### Adoption (assess-plan-execute-sustain)
- **Assess:** mudança alta no fluxo. Resistência esperada de quem já faz a digitação e de quem desconfia de IA.
- **Plan:** treinamento prático com dados fictícios e um analista de referência por turno.
- **Execute:** piloto com acompanhamento diário nas duas primeiras semanas.
- **Sustain:** medir as métricas do SOP, revisar trimestralmente e documentar lições.

### Rollback Plan
- **Trigger:** erro de gravação confirmado, vazamento suspeito, incidente de segurança, ou métrica abaixo do critério de parada.
- **Steps:**
  1. Desconectar o conector Microsoft 365 imediatamente pelo administrador.
  2. Comunicar a equipe que o processo volta ao manual.
  3. Restaurar a planilha para a versão anterior no histórico do SharePoint, se houver alteração indevida.
  4. Revisar o log de auditoria e comunicar ao DPO, se houver dado pessoal envolvido.
- **Verification:** caixa e planilha conferidas pelo analista, sem alteração pendente, e log de auditoria exportado.

### Critério de parada do piloto
Parar se qualquer uma destas condições ocorrer: erro de gravação sem detecção pela confirmação humana; deadline perdido atribuível ao piloto; incidente de segurança com dado pessoal.

### Approvals Required
| Approver | Role | Status |
|----------|------|--------|
| *(a definir)* | Responsável pela área de logística operacional | Pending |
| *(a definir)* | TI | Pending |
| *(a definir)* | RH | Pending |
| *(a definir)* | DPO | Pending |
