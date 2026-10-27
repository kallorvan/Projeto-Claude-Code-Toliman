# Avaliação do POP LOG 01 (rev. 15) — insumos para a implantação do Claude Code

**Documento avaliado:** `TOLIMAN_2.2.3_Procedimento_Transporte_rev_15.pdf`
**Título:** Procedimentos Operacionais da Área de Logística — POP LOG 01, Revisão 15 (12/05/2026)
**Elaboração:** Aloisio Alves · **Revisão:** Alef Azevedo, Ana Carolinne Guimarães, Taillyse Guimarães · **Aprovação:** Altemar Campos
**Páginas:** 30 (leitura integral)

**Escopo da fase inicial:** apenas a área **operacional**, e dentro dela apenas a **programação de navios** (armador, navio, viagem, booking, deadline, gate e mudanças). O fiscal fica fora por enquanto. Caminhões, motoristas, veículos e rastreamento de veículos também ficam fora desta fase.

**Objetivo revisado:** reduzir os processos manuais de e-mail e planilha, evitando (a) perda de e-mails, (b) erro de digitação no Excel, (c) perda de deadline de navio enviado pelo armador, inclusive as mudanças durante o processo. A ferramenta principal passa a ser o Claude com o conector **Microsoft 365** (Outlook e Excel), e o Claude Code fica como opção para a automação técnica posterior.

**Pré-requisito:** a conexão do conector Microsoft 365 depende de autorização formal de **TI e RH**, por envolver dados da empresa. Até lá, nenhum conector é conectado e nenhum dado da empresa é lido ou gravado.

Os números de página citados abaixo são os do próprio PDF (`p.X/30`).

---

## 1. Resumo executivo

- O POP é extenso e cobre bem o ciclo de transporte: coleta, estufagem, emissão fiscal, monitoramento, emergências, lacres, rastreadores e registros. Tem fluxos de exceção e controles antifraude bem estruturados (item 5.2, a–c).
- **Para ser base de automação, ele ainda precisa de correções.** Encontrei contradições de fato (tecnologia de rastreio por tipo de veículo, origem dos lacres), responsabilidades não definidas (item 4 vazio), termos usados sem definição, e uma tabela de retenção documental com vários prazos "indeterminado".
- **Viabilidade:** o Claude Code é uma ferramenta de linha de comando, pensada para quem programa e mexe com dados. Ele encaixa melhor na equipe de TI/BI/dados, que pode automatizar cruzamentos (NF-e × Ordem de Carregamento, planilhas de programação, controle de lacres, alertas de prazo) e versionar o próprio POP. Para a operação de campo (motoristas, conferentes, agendamento), a ferramenta mais adequada seria outra da linha Claude.
- **Primeiro passo recomendado:** transformar o POP em texto versionado (Markdown) neste repositório, corrigir as inconsistências listadas na seção 4 e só então desenhar automações sobre ele.

---

## 2. Pontos fortes

- **Fluxo ponta a ponta** por tipo de operação (5.1 nacional, 5.2 importação, 5.3/5.4/5.5 exportação, 5.6 a 5.12 tecnologias e controles).
- **Notas de exceção explícitas** (parada no Redex, comodidade, pré-stacking, lacre rompido, não conformidade do ticket).
- **Controle antifraude escalonado** (5.2 a/b/c e 5.3): critérios de identificação, comunicado com campos obrigatórios e cadeia de escalonamento até o OEA/RFB.
- **Tabela de prazos** por terminal e margem (5.5.1, Nota 3) e critérios de emergência objetivos (5.7).
- **Rastreabilidade do documento**: histórico de revisões desde 2021 (p.30) e lista de documentos envolvidos (p.29).

---

## 3. Lacunas estruturais

| # | Lacuna | Onde | Por que importa |
|---|---|---|---|
| L1 | **Item 4 "Responsabilidades" não lista responsáveis**, só remete ao item 5 (p.6). | p.6 | Quem é dono de cada passo (motorista, conferente, agendamento, GR, fiscal, Redex) não fica explícito. Sem isso não há como atribuir automação nem auditoria. |
| L2 | **Escopo (item 2) não cobre quem executa boa parte do processo**: motoristas próprios e terceiros, conferentes, equipe de agendamento, Redex Dínamo, unidade de apoio em Santos. | p.1 | Os papéis aparecem no corpo, mas não no escopo. |
| L3 | **Termos usados e não definidos**: OEA, COANA, FRI Tecnologia, MBM, VVTT, Raster, GR, Ticket de Intercâmbio, Cinta/Catraca, Jammer, ISCA, AT&M, RC-DC, RCTR-C, Gerenciadora de Riscos. | seções 3, 5 | Um agente de IA ou um novo colaborador vai interpretar esses termos de forma diferente. |
| L4 | **Termo definido e nunca usado**: OIC (Organização Internacional do Café). | p.2 | Ruído no glossário, ou procedimento faltando. |
| L5 | **Política de retenção incompleta**: vários registros com "INDETERMINADO" sem justificativa e documentos de importação ausentes da tabela. | p.28–29 | Não se sabe o que pode ser descartado. Não aparecem DI, BL, GNRE, GLME, DAT, VPO, DANFE/NF-e, Minuta OST, SM, ticket de intercâmbio. |
| L6 | **Anexos "Não aplicável"** (item 8), mas o POP cita formulários (REG LOG 01, 03, 04, 05, 07), planilhas e POPs externos. | p.30 | Os formulários citados não estão no pacote. Não dá para auditar o que se espera neles. |

---

## 4. Inconsistências e erros a corrigir

### 4.1 Contradições de fato (prioridade alta)

| # | Assunto | O que o texto diz | Onde | Ação sugerida |
|---|---|---|---|---|
| C1 | **Rastreador de cavalo** (**resolvido:** Sascar, confirmado na rodada 1) | 5.6: "Tracker" é usada para "frota cavalos e semirreboques" (GPS+RF+jammer). Mas no mesmo item "Sascar" é usada para "veículo Tóliman, cavalo" (GPS+satelital). Em 5.11, o rastreador principal do cavalo é a **Sascar**, e a Isca Tracker fica para **rebocadores e semirreboques**. | p.23–24, p.27 | Confirmar com GR qual tecnologia é a oficial por tipo de veículo e reescrever 5.6 e 5.11 de forma única. |
| C2 | **Origem do lacre provisório** | Seção 3: o lacre provisório Tóliman é "fornecido pela transportadora". Em 5.9: a aquisição do lacre provisório e do GIUC é feita pelo **depto. de compras**. | p.4, p.18, p.26 | Definir quem compra, quem fornece e quem recebe. |
| C3 | **Origem do lacre GIUC** | Seção 3: o GIUC é "fornecido pela transportadora". Em 5.10: o GIUC entra por compras (POP COMP 01) e é recebido no almoxarifado via NF. | p.4, p.26 | Mesma correção de C2. |
| C4 | **Limiar de falta de sinal** | 5.11: "falta de sinal por mais de 1 hora" aciona a gerenciadora. Em 5.7 (Situação 1): "parado ou com ignição desligada por 30 minutos" já aciona a logística. | p.25, p.27 | Não é necessariamente erro (são gatilhos diferentes), mas precisa estar explícito e cruzado. |
| C5 | **"Conforme POP LOG 01"** | 5.10 manda comunicar "conforme o POP LOG 01", mas este documento **é** o POP LOG 01. | p.26 | Trocar pelo número do POP correto ou pelo item específico. |
| C6 | **Nomes de terminais** | "DP Word" na tabela de prazos (deveria ser DP World). | p.22 | Corrigir. |

### 4.2 Erros de redação e numeração (prioridade média)

- **Numeração das notas reinicia em cada subitem.** Na seção 5.2 há duas "Nota 1" (uma sobre AT&M no início, outra sobre canhoto no fim), e a Nota 1 de 5.1 repete o mesmo texto da 5.2 (p.7 e p.9). Sugestão: numeração global por seção ou títulos nomeados.
- **Texto duplicado** entre 5.1 e 5.2 (emissão de documentos, averbação, lacre, recebimento) e entre 5.3 e 5.4 (inspeção de oito pontos). Vale extrair para um item comum e referenciar.
- **Erros de digitação**: "identifcado" (p.9), "Trata- se" (p.3), "Pais de Exportação" (p.3, deveria ser País), "estrategica" (p.28), "indicares" (p.28), "Tóliman" / "Toliman" / "Tóliman" grafados de formas diferentes, "Dinamo" / "Dínamo" alternados, "Depto." / "dep." / "departamento" misturados.
- **Pendências de tabela**: a tabela de registros (p.28–29) tem texto cortado na linha do RNC ("BACKUP/ FÍSICO ... ONOLÓGICA/ NOME"). Revisar o layout.

### 4.3 Termos possivelmente desatualizados (validar com fiscal)

- **DI × DUIMP.** O POP define ambas (p.3). A 5.2 exige upload da DI **e** da DUIMP ao site do terminal (p.9). Confirmar com o fiscal se a DI ainda é exigida nesse processo ou se a DUIMP a substituiu para essas operações.
- **LI (Licença de Importação).** Definida em p.3 como vinculada à DI. Confirmar se o termo atual para o licenciamento é outro (ex.: LPCO) e atualizar a definição.
- Termos "Porto Seco", "CLIA" e "Terminal Portuário seco" aparecem misturados em p.2–3 e 5.5. Vale unificar.

---

## 5. Oportunidades para o Claude Code

Os candidatos abaixo estão ordenados por **valor × viabilidade**, supondo que a execução fica com a equipe técnica (TI/BI/dados). Todos dependem de validação de quem executa o processo.

| Prioridade | Candidato | Seção do POP | Hoje | Como o Claude Code ajudaria | Pré-requisito |
|---|---|---|---|---|---|
| 1 | **Versionar o POP em Markdown** neste repositório, com glossário e referências cruzadas, e rodar uma checagem de consistência (termos, notas, referências a POPs). | Todo o POP | PDF de Word, sem histórico de diff | Converter, corrigir o item 4, detectar termo sem definição e referência quebrada. Serve de base para as demais automações. | Dono do POP aprovar a versão em Markdown como fonte. |
| — | *Fora do escopo inicial (fiscal).* **Validador NF-e × Ordem de Carregamento** (conferência fiscal). | 5.1, 5.2 a/b, 5.3, 5.5.2 | Conferência manual dos dados entre NF-e e OC | Ler o XML da NF-e, comparar com a OC e listar divergências antes da emissão de CT-e/MDF-e. | Acesso ao XML e aprovação do fiscal sobre as regras. |
| 3 | **Conciliação de lacres** (inventário, entregas, devoluções, não conformidades). | 5.10, REG LOG 07 | Registro no app FRI e formulário | Cruzar os registros de lacres com as entregas e sinalizar divergências e lacres sem baixa. | Exportação dos módulos Lacres do FRI. |
| — | *Fora do escopo desta fase (veículo).* **Alerta de rastreador a cada 30 dias.** | 5.11 | GR consulta o portal Raster à mão e agenda com 8 dias de antecedência | Gerar lista de veículos com check-list vencendo e o aviso de 8 dias. | Exportação do portal Raster. |
| 5 | **Relatório de programação de navios** (REG LOG 05, somente as colunas de navio e embarque) a partir das exportações Rodopar e MBM. Colunas de veículo ficam fora. | 5.1, 5.3, 5.5 | Planilha alimentada à mão e compartilhada no Teams | Montar a planilha e o resumo diário a partir das exportações. | Acesso às exportações Rodopar/MBM. |
| — | *Fora do escopo desta fase (chegada de veículo ao terminal).* **Monitor de prazo de chegada ao terminal.** A parte de deadline de navio entra no piloto. | 5.5.1 (tabela, Nota 3), 5.7 | Equipe de monitoramento acompanha por alerta | Cruzar ETA, data de saída e prazo de chegada por terminal e sinalizar risco. | Integração com dados de posição (Raster/Sascar). Não é trivial. |
| 7 | **Inventário e rotina de retenção documental** (somente registros operacionais). | 6 | Vários prazos "indeterminado" | Gerar a lista de registros operacionais, seus prazos e os vencidos para descarte aprovado. | Política de retenção dos registros operacionais aprovada pelo responsável da operação. Os registros fiscais ficam fora desta fase. |
| 8 | **Assistente de dúvidas sobre o POP** (para novos colaboradores). | Todo o POP | Perguntas ao gestor | Responder com base no POP versionado, citando o item. | Passo 1 concluído. Atenção ao item 6 (dados sensíveis). |

**Fora do escopo do Claude Code** (melhor atendidos por outras ferramentas da linha Claude ou por processo): o atendimento por WhatsApp e telefone com motoristas e clientes, a conferência física de lacres e contêineres, e a operação de agendamento no site dos terminais. Para esses casos, a alternativa é uma ferramenta de chat para o time de operação.

---

## 6. Riscos e restrições para a implantação

1. **Público.** O Claude Code é uma ferramenta de terminal, pensada para desenvolvimento. A equipe de campo não é o público natural dele.
2. **Dados sensíveis no repositório.** O POP descreve controles antifraude, cercas eletrônicas, rastreadores, lacres e comunicação com OEA e RFB. Esse conteúdo não deve ficar em repositório público. Também há dados pessoais de motoristas (nome, CPF, placa) e dados fiscais de clientes. Qualquer envio ao modelo precisa de regra de anonimização e de conformidade com a LGPD.
3. **Credenciais de portais.** O POP menciona login e senha nos sites dos terminais (5.2, 5.5.2) e sistemas Rodopar, FRI, MBM, Raster e Sascar. Credenciais nunca devem ir para o repositório ou para o contexto do Claude. Use cofre de segredos.
4. **Sistemas sem API documentada.** O POP não informa se Rodopar, FRI, MBM, Raster ou Sascar oferecem API ou exportação automática. Isso define se o caminho é integração ou leitura de arquivos exportados.
5. **Responsabilidade fiscal e de segurança.** Uma automação que valida documentos não substitui a análise do fiscal nem a decisão de escalonar ao OEA. Qualquer automação precisa manter trilha de auditoria.
6. **Canal informal para eventos críticos.** Incidentes, lacres rompidos e desvios de rota são comunicados por WhatsApp e e-mail (5.2, 5.5, 5.7). Isso dificulta a rastreabilidade. Vale considerar um registro formal que a automação possa ler.

---

## 7. Perguntas abertas (para a entrevista de validação)

Estas perguntas dependem de decisão ou conhecimento de quem opera o processo, não de leitura do documento. São a base da entrevista com o `grill-me`.

1. Quem será o público inicial do Claude Code: equipe técnica/BI ou também a operação?
2. O repositório pode ser privado e conter o POP completo? Há política interna sobre envio de dados fiscais e pessoais a ferramentas de IA?
3. Quais sistemas têm exportação ou API (Rodopar, FRI, MBM, Raster, Sascar, portais dos terminais)?
4. Qual tecnologia de rastreio é a oficial por tipo de veículo (C1)?
5. Quem compra e quem fornece os lacres provisório e GIUC (C2, C3)?
6. A DI ainda é exigida além da DUIMP nas operações de importação (4.3)?
7. Qual a política de retenção para os registros marcados "indeterminado" (seção 6)?
8. Quem é o dono do POP e aceita que correções sejam propostas por este repositório?
9. Qual caso de uso deve ser o primeiro piloto?

---

## 8. Riscos de LGPD e outros perigos da conexão com o Microsoft 365

*Não é parecer jurídico. DPO e jurídico validam antes da conexão.*

### 8.1 LGPD

| Tema | Risco | Controle proposto | Dono |
|---|---|---|---|
| Dados pessoais | E-mails e planilhas com nome, CPF, CNH, placa vinculada a pessoa, telefone e e-mail de motoristas, clientes e armador. | Escopo mínimo: só a caixa de programação e um arquivo de planilha. CPF mascarado. | TI, DPO |
| Base legal e finalidade (arts. 6º e 7º) | Ler a caixa inteira para extrair deadline é excesso de tratamento. | Finalidade escrita: extrair navio, deadline e mudanças. Nada além disso. | DPO, jurídico |
| Operador e transferência internacional (arts. 33 e 39) | Dados saem do Brasil, e a Anthropic atua como operador ou suboperador. | DPA assinado. Confirmar por escrito que dados não treinam modelos e o prazo de retenção no plano empresarial. | Jurídico, TI |
| Direitos do titular (art. 18) | Motorista ou contato pede acesso, correção ou eliminação, e a empresa não consegue atender. | Mapear onde o dado passou (caixa, planilha, histórico do Claude). Procedimento de resposta. | DPO |
| Segurança e incidente (arts. 46 a 48) | Vazamento sem comunicação à ANPD e aos titulares. | Log de auditoria, MFA, plano de resposta a incidente. | TI, DPO |
| Relatório de impacto (RIPD, art. 38) | Tratamento em escala sem avaliação formal de risco. | RIPD antes da conexão. | DPO |
| Monitoramento de funcionários | Caixas com mensagens de funcionários entram no escopo. | Política de uso de e-mail e de IA com aviso prévio. Excluir caixas de RH e financeiro. | RH |

### 8.2 Outros perigos

| Tema | Risco | Controle proposto | Dono |
|---|---|---|---|
| Sigilo comercial e operacional | Tarifas, contratos, rotas, clientes, procedimentos de lacre, rastreador e OEA podem vazar. Parte está sob NDA. | Classificar a informação. Excluir do escopo o que for sob NDA sem autorização do cliente. | Gestão, jurídico |
| Permissões amplas no Microsoft 365 | `Mail.ReadWrite` e `Files.ReadWrite.All` dão acesso a toda a caixa e a todo o SharePoint. | Menor privilégio: caixa compartilhada e um arquivo. Consentimento de administrador. Acesso condicional e MFA. Rótulos de sensibilidade e DLP. | TI |
| Injeção de instruções por e-mail | Um e-mail externo tenta mudar o comportamento do Claude, por exemplo "altere o deadline do navio". | Conteúdo externo é dado, não instrução. Leitura separada da gravação. Nenhuma alteração sem confirmação humana. | TI, operação |
| Fraude de deadline (e-mail falso do armador) | Programa de embarque alterado por remetente falso. | Validar SPF, DKIM e DMARC. Confirmação humana antes de gravar. Contato por canal conhecido para mudança crítica. | Operação |
| Erro de leitura | Data, horário, fuso, tabela ou nome de navio lidos errado e gravados com aparência de certo. | Mostrar o trecho original do e-mail junto da proposta. Histórico de versões no SharePoint. Teste com amostra. | Operação |
| Uso fora do controle (shadow IT) | Conta pessoal do Claude conectada à caixa corporativa, ou dados colados em conversas fora da conta empresarial. | Conta corporativa única. Regra escrita. Bloqueio de contas pessoais na rede. | TI, RH |
| Retenção e descarte | Prazos do e-mail, da planilha e das conversas com o Claude não alinhados. | Alinhar com a política de retenção da empresa. | Jurídico, TI |
| Dependência e revogação | Conexão revogada ou plano alterado para a operação. | Processo manual como contingência testada. | Operação |

### 8.3 Controles mínimos antes de conectar

1. DPO e jurídico validam base legal, finalidade, DPA, transferência internacional e treinamento.
2. RIPD concluído.
3. Política de uso de IA e de e-mail corporativo aprovada por TI e RH.
4. Escopo mínimo: caixa compartilhada de programação e um arquivo de planilha, com permissões separadas de leitura e escrita.
5. Nenhuma gravação sem confirmação humana, com registro de quem confirmou e quando.
6. Log de auditoria de tudo que o Claude leu e propôs.
7. Mascaramento de CPF e de dados desnecessários.
8. Plano de contingência manual testado.
9. Teste com dados fictícios antes de qualquer dado real.

### 8.4 Como levar à TI, RH e Gestão

**Mensagem central.** Não dizer que o Claude "elimina" os riscos. Dizer que ele reduz os riscos a níveis aceitáveis e mensuráveis, e que cada controle tem dono e evidência. Isso é mais fácil de aprovar e mais honesto.

**Comparar com o risco atual.** Hoje já existem: e-mails perdidos, planilhas digitadas à mão, dados de motoristas trafegando por WhatsApp, e nenhuma trilha de quem alterou o deadline. O argumento forte é: "o risco atual é alto e não tem registro; com os controles, ele fica menor e auditável".

**Por área.**

- **TI.** Pergunta a responder: o que precisa ser verdadeiro para aprovar? Resposta: escopo mínimo, menor privilégio, consentimento de administrador, MFA, rótulos e DLP, log de auditoria e conta corporativa única. Oferecer demonstração com dados fictícios.
- **RH.** Pergunta a responder: como fica a privacidade dos funcionários? Resposta: caixas de RH e financeiro excluídas, política escrita com aviso prévio, sem avaliação de desempenho a partir da caixa. RH define a política, e a TI aplica.
- **Gestão.** Pergunta a responder: vale o custo e o risco? Resposta: piloto de 30 dias com métricas antes e depois: deadlines perdidos, erros de digitação na planilha, horas de digitação, e-mails sem tratamento. Critério de parada definido de antemão.

**Sequência sugerida.**

1. Reunião conjunta com TI, RH, DPO e Gestão, com este estudo como base.
2. Demonstração com dados fictícios, sem conectar nada à empresa.
3. Aprovação formal da política e do RIPD.
4. Piloto restrito: uma caixa, um arquivo, leitura e proposta apenas.
5. Medição de 30 dias e decisão de continuar, ajustar ou parar.

**Objeções prováveis e respostas.**

| Objeção | Resposta |
|---|---|
| "A IA vai ler nossos dados." | Só a caixa de programação, com finalidade escrita, DPA e log. |
| "A IA pode alterar a planilha sozinha." | Não. Ela só propõe. Quem grava é uma pessoa, com registro. |
| "Vamos vazar dados." | Menor privilégio, MFA, DLP, log, e a informação sob NDA fica fora. Vazamento hoje já acontece por WhatsApp. |
| "Funcionários serão monitorados." | Caixas de RH e financeiro ficam fora. A política é aprovada pelo RH. |
| "E se errar?" | Confirmação humana e trecho original sempre visível. O erro de digitação atual também existe. |
| "Quanto custa?" | Piloto limitado com métrica. Decisão de ampliar só com o ganho medido. |

## 9. Próximos passos propostos

1. Validar as perguntas da seção 7 com o dono do POP e com o fiscal.
2. Converter o POP em Markdown versionado, separado em arquivos por seção, e corrigir as contradições C1 a C6.
3. Definir o primeiro piloto dentro da área operacional (sugestão: caso 4, alerta do check-list do rastreador a cada 30 dias, por ter regra clara e dados de uma única fonte; alternativa: caso 3, conciliação de lacres).
4. Medir ganho e erro do piloto antes de ampliar para os demais candidatos.
