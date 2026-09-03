# Regras de Negócio e Requisitos — Sistema de Gestão de Fazenda

## 0. Contexto

Sistema de gestão de fazenda com cinco perfis de usuário:

- **Fazendeiro** — acessa os dados da fazenda, gerencia funcionários (cadastro) e cria/atribui tarefas.
- **Funcionário / Analista** — acessa e atualiza suas tarefas, e é responsável por atualizar dados operacionais da fazenda.
- **Administrador do sistema** — atualiza dados e configurações do sistema, faz manutenção.
- **Gerente Financeiro** — controla compras e vendas de produtos, gerencia pagamento dos funcionários.
- **Estoquista** — gerencia o estoque (consulta e movimentações de entrada/saída).

O fluxo geral segue: login → identificação do perfil → redirecionamento ao dashboard correspondente → execução dos processos de negócio de cada perfil, com persistência nas bases de dados (Usuários, Fazenda, Tarefas, Clientes/Fornecedores, Compras/Vendas, Pagamentos, Estoque, Sistema).

---

## 1. Regras de Negócio (RN)

```markdown
## RN-001 — Perfil de acesso único por login

**Título:**
Vinculação de um único perfil de acesso por usuário autenticado.

**Descrição:**
Cada usuário autenticado no sistema possui um único perfil de acesso (Fazendeiro, Funcionário, Administrador, Gerente Financeiro ou Estoquista), que determina o dashboard e as funcionalidades disponíveis.

**Origem:**
Mapa de usuários do projeto.

**Stakeholders envolvidos:**
Fazendeiro, Funcionário, Administrador, Gerente Financeiro, Estoquista.

**Condição:**
Aplicada imediatamente após a autenticação bem-sucedida.

**Regra:**
O sistema deve identificar o perfil vinculado ao usuário e direcioná-lo exclusivamente ao dashboard correspondente ao seu perfil.

**Exceções:**
Nenhuma identificada até o momento.

**Dados envolvidos:**
Usuário, perfil de acesso.

**Prioridade:**
Crítica

**Status:**
Aprovado

**Requisitos relacionados:**
RF-001, RF-002

**Observações:**
Perfis múltiplos por usuário podem ser avaliados em versão futura.
```

```markdown
## RN-002 — Cadastro de funcionários restrito ao Fazendeiro

**Título:**
Exclusividade do Fazendeiro para cadastro de funcionários.

**Descrição:**
Somente o usuário com perfil Fazendeiro pode cadastrar novos funcionários no sistema.

**Origem:**
Mapa de usuários do projeto.

**Stakeholders envolvidos:**
Fazendeiro, Funcionário.

**Condição:**
Aplicada sempre que houver uma tentativa de cadastro de funcionário.

**Regra:**
O sistema somente deve permitir a operação de cadastro de funcionário quando executada por um usuário com perfil Fazendeiro.

**Exceções:**
O Administrador do sistema pode realizar o cadastro em situações de manutenção, mediante permissão especial.

**Dados envolvidos:**
Usuário solicitante, perfil do usuário solicitante, dados do novo funcionário.

**Prioridade:**
Alta

**Status:**
Aprovado

**Requisitos relacionados:**
RF-004

**Observações:**
Nenhuma.
```

```markdown
## RN-003 — Atribuição de tarefas restrita ao Fazendeiro

**Título:**
Exclusividade do Fazendeiro para criação e atribuição de tarefas.

**Descrição:**
Apenas o Fazendeiro pode criar novas tarefas e atribuí-las a um ou mais funcionários.

**Origem:**
Fluxo de negócio (Criar e atribuir tarefa).

**Stakeholders envolvidos:**
Fazendeiro, Funcionário.

**Condição:**
Aplicada sempre que uma nova tarefa for criada.

**Regra:**
O sistema deve permitir a criação de tarefas somente a usuários com perfil Fazendeiro, associando a tarefa a um funcionário existente e enviando notificação a ele.

**Exceções:**
Nenhuma identificada até o momento.

**Dados envolvidos:**
Tarefa, funcionário responsável, status da tarefa.

**Prioridade:**
Alta

**Status:**
Aprovado

**Requisitos relacionados:**
RF-005, RF-006

**Observações:**
Nenhuma.
```

```markdown
## RN-004 — Atualização de status restrita ao funcionário responsável

**Título:**
Restrição de atualização de status de tarefa ao funcionário designado.

**Descrição:**
Um funcionário somente pode atualizar o status das tarefas que lhe foram atribuídas.

**Origem:**
Fluxo de negócio (Atualizar status da tarefa).

**Stakeholders envolvidos:**
Funcionário, Fazendeiro.

**Condição:**
Aplicada sempre que houver tentativa de atualização de status de uma tarefa.

**Regra:**
O sistema somente deve permitir a atualização do status de uma tarefa quando o usuário solicitante for o funcionário responsável por ela.

**Exceções:**
O Fazendeiro que criou a tarefa também pode alterar seu status.

**Dados envolvidos:**
Tarefa, funcionário responsável, status.

**Prioridade:**
Alta

**Status:**
Aprovado

**Requisitos relacionados:**
RF-007

**Observações:**
Nenhuma.
```

```markdown
## RN-005 — Atualização de dados operacionais da fazenda pelo funcionário

**Título:**
Responsabilidade do funcionário pela atualização de dados operacionais.

**Descrição:**
O funcionário é responsável por manter atualizados os dados operacionais da fazenda gerados no dia a dia das atividades.

**Origem:**
Mapa de usuários do projeto.

**Stakeholders envolvidos:**
Funcionário, Fazendeiro.

**Condição:**
Aplicada sempre que houver alteração em dados operacionais da fazenda.

**Regra:**
O sistema deve permitir que o funcionário registre atualizações nos dados operacionais da fazenda, mantendo o histórico de quem realizou a alteração.

**Exceções:**
O Fazendeiro pode também atualizar esses dados diretamente.

**Dados envolvidos:**
Dados da fazenda, usuário responsável pela atualização.

**Prioridade:**
Média

**Status:**
Aprovado

**Requisitos relacionados:**
RF-008

**Observações:**
Nenhuma.
```

```markdown
## RN-006 — Operações financeiras restritas ao Gerente Financeiro

**Título:**
Exclusividade do Gerente Financeiro para compras, vendas e pagamentos.

**Descrição:**
Somente o Gerente Financeiro pode registrar compras e vendas de produtos, bem como gerenciar o pagamento dos funcionários.

**Origem:**
Mapa de usuários do projeto.

**Stakeholders envolvidos:**
Gerente Financeiro, Fazendeiro, Funcionário.

**Condição:**
Aplicada sempre que houver tentativa de registro de compra, venda ou pagamento.

**Regra:**
O sistema somente deve permitir essas operações a usuários com perfil Gerente Financeiro.

**Exceções:**
Nenhuma identificada até o momento.

**Dados envolvidos:**
Cliente/fornecedor, produto, valor, funcionário (no caso de pagamento).

**Prioridade:**
Crítica

**Status:**
Aprovado

**Requisitos relacionados:**
RF-009, RF-010, RF-011, RF-012

**Observações:**
Nenhuma.
```

```markdown
## RN-007 — Toda movimentação de estoque deve ser classificada

**Título:**
Classificação obrigatória das movimentações de estoque.

**Descrição:**
Toda movimentação registrada no estoque deve ser classificada como entrada ou saída de produto.

**Origem:**
Fluxo de negócio (Registrar movimentação).

**Stakeholders envolvidos:**
Estoquista.

**Condição:**
Aplicada sempre que uma movimentação de estoque for registrada.

**Regra:**
O sistema deve exigir a classificação da movimentação (entrada ou saída) e atualizar a quantidade em estoque de acordo com o tipo informado.

**Exceções:**
Nenhuma identificada até o momento.

**Dados envolvidos:**
Produto, quantidade, tipo de movimentação, data.

**Prioridade:**
Alta

**Status:**
Aprovado

**Requisitos relacionados:**
RF-014, RF-015

**Observações:**
Nenhuma.
```

```markdown
## RN-008 — Manutenção e configuração restritas ao Administrador

**Título:**
Exclusividade do Administrador para manutenção e configuração do sistema.

**Descrição:**
Somente o Administrador do sistema pode realizar manutenção e atualizar configurações e dados de sistema.

**Origem:**
Mapa de usuários do projeto.

**Stakeholders envolvidos:**
Administrador do sistema.

**Condição:**
Aplicada sempre que houver tentativa de manutenção ou alteração de configuração.

**Regra:**
O sistema somente deve permitir essas operações a usuários com perfil Administrador.

**Exceções:**
Nenhuma identificada até o momento.

**Dados envolvidos:**
Configurações do sistema, logs.

**Prioridade:**
Crítica

**Status:**
Aprovado

**Requisitos relacionados:**
RF-016, RF-017

**Observações:**
Nenhuma.
```

```markdown
## RN-009 — Autenticação obrigatória

**Título:**
Exigência de autenticação para acesso ao sistema.

**Descrição:**
Nenhum usuário pode acessar qualquer dashboard ou funcionalidade sem antes realizar login válido.

**Origem:**
Fluxo de acesso (Entrar no aplicativo → Fazer login).

**Stakeholders envolvidos:**
Todos os perfis.

**Condição:**
Aplicada em toda tentativa de acesso ao sistema.

**Regra:**
O sistema deve exigir autenticação válida antes de identificar o perfil e liberar o acesso ao respectivo dashboard.

**Exceções:**
Nenhuma.

**Dados envolvidos:**
Credenciais do usuário, usuário, perfil.

**Prioridade:**
Crítica

**Status:**
Aprovado

**Requisitos relacionados:**
RF-001

**Observações:**
Nenhuma.
```

---

## 2. Requisitos Funcionais (RF)

```markdown
## RF-001 — Autenticar usuário

**Título:**
Login no sistema.

**Descrição:**
O sistema deve permitir que o usuário faça login informando suas credenciais.

**Objetivo:**
Garantir que apenas usuários cadastrados acessem o sistema.

**Stakeholders:**
Todos os perfis.

**Ator principal:**
Usuário (qualquer perfil).

**Pré-condições:**
Usuário previamente cadastrado no sistema.

**Entradas:**
Identificador do usuário, senha.

**Processamento esperado:**
O sistema deve validar as credenciais informadas contra a base de usuários.

**Saídas/Resultados:**
Sessão autenticada ou mensagem de erro.

**Pós-condições:**
Usuário autenticado e pronto para ter seu perfil identificado.

**Fluxos alternativos/exceções:**
- Credenciais inválidas.
- Usuário inativo.

**Regras de negócio relacionadas:**
RN-009

**Prioridade:**
Crítica

**Status:**
Aprovado

**Critérios de aceite:**
- Não permitir acesso com credenciais inválidas.
- Bloquear login de usuário inativo.
- Redirecionar para identificação de perfil após sucesso.

**Casos de uso relacionados:**
UC-001

**Tarefas relacionadas:**
TASK-001

**Casos de teste relacionados:**
CT-001, CT-002
```

```markdown
## RF-002 — Identificar perfil e redirecionar dashboard

**Título:**
Redirecionamento por perfil de acesso.

**Descrição:**
O sistema deve identificar o perfil do usuário autenticado e redirecioná-lo ao dashboard correspondente (Fazendeiro, Funcionário, Administrador, Financeiro ou Estoque).

**Objetivo:**
Assegurar que cada perfil visualize apenas as funcionalidades pertinentes a ele.

**Stakeholders:**
Todos os perfis.

**Ator principal:**
Usuário autenticado.

**Pré-condições:**
Login realizado com sucesso.

**Entradas:**
Identificador do usuário autenticado.

**Processamento esperado:**
O sistema deve consultar o perfil vinculado ao usuário e selecionar o dashboard correspondente.

**Saídas/Resultados:**
Dashboard específico do perfil exibido.

**Pós-condições:**
Usuário posicionado no dashboard de seu perfil.

**Fluxos alternativos/exceções:**
Usuário sem perfil definido.

**Regras de negócio relacionadas:**
RN-001

**Prioridade:**
Crítica

**Status:**
Aprovado

**Critérios de aceite:**
- Cada perfil deve ser direcionado exclusivamente ao seu próprio dashboard.
- Usuário sem perfil definido deve receber mensagem de erro.

**Casos de uso relacionados:**
UC-001

**Tarefas relacionadas:**
TASK-002

**Casos de teste relacionados:**
CT-003
```

```markdown
## RF-003 — Consultar dados da fazenda

**Título:**
Consulta de dados da fazenda.

**Descrição:**
O sistema deve permitir que o Fazendeiro consulte os dados cadastrais e operacionais da fazenda.

**Objetivo:**
Fornecer visibilidade das informações da fazenda ao Fazendeiro.

**Stakeholders:**
Fazendeiro.

**Ator principal:**
Fazendeiro.

**Pré-condições:**
Usuário autenticado com perfil Fazendeiro.

**Entradas:**
Nenhuma entrada obrigatória (consulta direta).

**Processamento esperado:**
O sistema deve recuperar os dados da fazenda associados ao usuário.

**Saídas/Resultados:**
Dados da fazenda exibidos.

**Pós-condições:**
Nenhuma alteração de estado.

**Fluxos alternativos/exceções:**
Fazenda sem dados cadastrados.

**Regras de negócio relacionadas:**
—

**Prioridade:**
Média

**Status:**
Aprovado

**Critérios de aceite:**
- Exibir todos os dados cadastrais vigentes da fazenda.

**Casos de uso relacionados:**
UC-002

**Tarefas relacionadas:**
TASK-003

**Casos de teste relacionados:**
CT-004
```

```markdown
## RF-004 — Cadastrar funcionário

**Título:**
Cadastro de novo funcionário.

**Descrição:**
O sistema deve permitir que o Fazendeiro cadastre um novo funcionário, informando seus dados básicos e vinculando-o à fazenda.

**Objetivo:**
Permitir a gestão da equipe de funcionários da fazenda.

**Stakeholders:**
Fazendeiro, Funcionário.

**Ator principal:**
Fazendeiro.

**Pré-condições:**
Usuário autenticado com perfil Fazendeiro.

**Entradas:**
Nome, dados de contato, credenciais de acesso do funcionário.

**Processamento esperado:**
O sistema deve validar os dados informados e criar o registro do novo funcionário vinculado à fazenda.

**Saídas/Resultados:**
Funcionário cadastrado e habilitado para login.

**Pós-condições:**
Novo funcionário disponível para receber tarefas.

**Fluxos alternativos/exceções:**
- Dados obrigatórios ausentes.
- Funcionário já cadastrado.

**Regras de negócio relacionadas:**
RN-002

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Impedir cadastro duplicado.
- Exigir preenchimento dos dados obrigatórios.
- Habilitar login do novo funcionário após cadastro.

**Casos de uso relacionados:**
UC-003

**Tarefas relacionadas:**
TASK-004, TASK-005

**Casos de teste relacionados:**
CT-005, CT-006
```

```markdown
## RF-005 — Criar e atribuir tarefa

**Título:**
Criação e atribuição de tarefa a funcionário.

**Descrição:**
O sistema deve permitir que o Fazendeiro crie uma nova tarefa e a atribua a um funcionário cadastrado.

**Objetivo:**
Organizar e distribuir o trabalho entre os funcionários da fazenda.

**Stakeholders:**
Fazendeiro, Funcionário.

**Ator principal:**
Fazendeiro.

**Pré-condições:**
Funcionário previamente cadastrado.

**Entradas:**
Descrição da tarefa, prazo, funcionário responsável.

**Processamento esperado:**
O sistema deve registrar a tarefa com status inicial e associá-la ao funcionário indicado.

**Saídas/Resultados:**
Tarefa criada e disponível para o funcionário.

**Pós-condições:**
Notificação enviada ao funcionário responsável.

**Fluxos alternativos/exceções:**
Funcionário informado inexistente ou inativo.

**Regras de negócio relacionadas:**
RN-003

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Não permitir atribuição a funcionário inexistente.
- Definir status inicial da tarefa automaticamente.
- Disparar notificação ao funcionário.

**Casos de uso relacionados:**
UC-004

**Tarefas relacionadas:**
TASK-006

**Casos de teste relacionados:**
CT-007
```

```markdown
## RF-006 — Notificar funcionário sobre nova tarefa

**Título:**
Notificação de nova tarefa atribuída.

**Descrição:**
O sistema deve notificar o funcionário sempre que uma nova tarefa lhe for atribuída.

**Objetivo:**
Garantir que o funcionário tenha ciência imediata de novas responsabilidades.

**Stakeholders:**
Funcionário, Fazendeiro.

**Ator principal:**
Sistema (acionado após RF-005).

**Pré-condições:**
Tarefa criada e atribuída.

**Entradas:**
Identificador da tarefa, identificador do funcionário.

**Processamento esperado:**
O sistema deve gerar e enviar a notificação ao funcionário responsável.

**Saídas/Resultados:**
Notificação entregue no dashboard do funcionário.

**Pós-condições:**
Tarefa disponível na lista de tarefas do funcionário.

**Fluxos alternativos/exceções:**
Falha no envio da notificação.

**Regras de negócio relacionadas:**
RN-003

**Prioridade:**
Média

**Status:**
Aprovado

**Critérios de aceite:**
- Notificação deve ocorrer imediatamente após a criação da tarefa.
- Tarefa deve aparecer na lista do funcionário mesmo se a notificação falhar.

**Casos de uso relacionados:**
UC-004

**Tarefas relacionadas:**
TASK-007

**Casos de teste relacionados:**
CT-008
```

```markdown
## RF-007 — Consultar e atualizar status da tarefa

**Título:**
Gestão de status das tarefas pelo funcionário.

**Descrição:**
O sistema deve permitir que o funcionário consulte suas tarefas atribuídas e atualize o status de cada uma.

**Objetivo:**
Permitir o acompanhamento da execução das tarefas.

**Stakeholders:**
Funcionário, Fazendeiro.

**Ator principal:**
Funcionário.

**Pré-condições:**
Usuário autenticado com perfil Funcionário; tarefa previamente atribuída.

**Entradas:**
Identificador da tarefa, novo status.

**Processamento esperado:**
O sistema deve validar se o funcionário é o responsável pela tarefa e atualizar seu status.

**Saídas/Resultados:**
Status da tarefa atualizado.

**Pós-condições:**
Fazendeiro pode visualizar o novo status.

**Fluxos alternativos/exceções:**
Funcionário tenta atualizar tarefa não atribuída a ele.

**Regras de negócio relacionadas:**
RN-004

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Impedir atualização de tarefas de terceiros.
- Registrar data/hora da atualização de status.

**Casos de uso relacionados:**
UC-005

**Tarefas relacionadas:**
TASK-008

**Casos de teste relacionados:**
CT-009, CT-010
```

```markdown
## RF-008 — Atualizar dados operacionais da fazenda

**Título:**
Atualização de dados operacionais pelo funcionário.

**Descrição:**
O sistema deve permitir que o funcionário registre atualizações nos dados operacionais da fazenda.

**Objetivo:**
Manter os dados da fazenda atualizados a partir das atividades diárias.

**Stakeholders:**
Funcionário, Fazendeiro.

**Ator principal:**
Funcionário.

**Pré-condições:**
Usuário autenticado com perfil Funcionário.

**Entradas:**
Dados operacionais a atualizar.

**Processamento esperado:**
O sistema deve validar e persistir as alterações, registrando o autor da atualização.

**Saídas/Resultados:**
Dados da fazenda atualizados.

**Pós-condições:**
Fazendeiro pode consultar os dados atualizados.

**Fluxos alternativos/exceções:**
Dados informados inválidos.

**Regras de negócio relacionadas:**
RN-005

**Prioridade:**
Média

**Status:**
Aprovado

**Critérios de aceite:**
- Registrar usuário e data/hora da atualização.
- Validar formato dos dados antes de salvar.

**Casos de uso relacionados:**
UC-006

**Tarefas relacionadas:**
TASK-009

**Casos de teste relacionados:**
CT-011
```

```markdown
## RF-009 — Selecionar cliente ou fornecedor

**Título:**
Seleção de cliente/fornecedor para operação financeira.

**Descrição:**
O sistema deve permitir que o Gerente Financeiro selecione um cliente ou fornecedor cadastrado antes de registrar uma compra ou venda.

**Objetivo:**
Vincular corretamente as movimentações financeiras a um cliente ou fornecedor.

**Stakeholders:**
Gerente Financeiro.

**Ator principal:**
Gerente Financeiro.

**Pré-condições:**
Cliente ou fornecedor previamente cadastrado.

**Entradas:**
Identificador ou nome do cliente/fornecedor.

**Processamento esperado:**
O sistema deve localizar o cliente/fornecedor na base correspondente.

**Saídas/Resultados:**
Cliente/fornecedor selecionado para a operação.

**Pós-condições:**
Operação de compra ou venda pode prosseguir.

**Fluxos alternativos/exceções:**
Cliente/fornecedor não encontrado.

**Regras de negócio relacionadas:**
RN-006

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Exibir mensagem quando não houver correspondência.

**Casos de uso relacionados:**
UC-007

**Tarefas relacionadas:**
TASK-010

**Casos de teste relacionados:**
CT-012
```

```markdown
## RF-010 — Registrar compra de produto

**Título:**
Registro de compra de produto.

**Descrição:**
O sistema deve permitir que o Gerente Financeiro registre a compra de produtos junto a um fornecedor.

**Objetivo:**
Controlar as aquisições de produtos para a fazenda.

**Stakeholders:**
Gerente Financeiro.

**Ator principal:**
Gerente Financeiro.

**Pré-condições:**
Fornecedor selecionado (RF-009).

**Entradas:**
Produto, quantidade, valor, fornecedor.

**Processamento esperado:**
O sistema deve validar os dados e registrar a movimentação financeira de compra.

**Saídas/Resultados:**
Compra registrada.

**Pós-condições:**
Movimentação disponível para consulta financeira.

**Fluxos alternativos/exceções:**
Dados obrigatórios ausentes ou inválidos.

**Regras de negócio relacionadas:**
RN-006

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Registrar data da compra automaticamente.
- Impedir valores negativos ou nulos.

**Casos de uso relacionados:**
UC-007

**Tarefas relacionadas:**
TASK-011

**Casos de teste relacionados:**
CT-013
```

```markdown
## RF-011 — Registrar venda de produto

**Título:**
Registro de venda de produto.

**Descrição:**
O sistema deve permitir que o Gerente Financeiro registre a venda de produtos a um cliente.

**Objetivo:**
Controlar as vendas de produtos da fazenda.

**Stakeholders:**
Gerente Financeiro.

**Ator principal:**
Gerente Financeiro.

**Pré-condições:**
Cliente selecionado (RF-009).

**Entradas:**
Produto, quantidade, valor, cliente.

**Processamento esperado:**
O sistema deve validar os dados e registrar a movimentação financeira de venda.

**Saídas/Resultados:**
Venda registrada.

**Pós-condições:**
Movimentação disponível para consulta financeira.

**Fluxos alternativos/exceções:**
Dados obrigatórios ausentes ou inválidos.

**Regras de negócio relacionadas:**
RN-006

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Registrar data da venda automaticamente.
- Impedir valores negativos ou nulos.

**Casos de uso relacionados:**
UC-007

**Tarefas relacionadas:**
TASK-012

**Casos de teste relacionados:**
CT-014
```

```markdown
## RF-012 — Gerenciar pagamento de funcionário

**Título:**
Gestão de pagamento de funcionários.

**Descrição:**
O sistema deve permitir que o Gerente Financeiro registre e acompanhe os pagamentos realizados aos funcionários.

**Objetivo:**
Controlar a folha de pagamento da fazenda.

**Stakeholders:**
Gerente Financeiro, Funcionário.

**Ator principal:**
Gerente Financeiro.

**Pré-condições:**
Funcionário previamente cadastrado.

**Entradas:**
Funcionário, valor, período de referência.

**Processamento esperado:**
O sistema deve validar os dados e registrar o pagamento associado ao funcionário.

**Saídas/Resultados:**
Pagamento registrado.

**Pós-condições:**
Histórico de pagamentos do funcionário atualizado.

**Fluxos alternativos/exceções:**
Funcionário inexistente ou inativo.

**Regras de negócio relacionadas:**
RN-006

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Impedir pagamento duplicado para o mesmo período.
- Registrar data do pagamento.

**Casos de uso relacionados:**
UC-008

**Tarefas relacionadas:**
TASK-013

**Casos de teste relacionados:**
CT-015
```

```markdown
## RF-013 — Consultar estoque

**Título:**
Consulta de itens em estoque.

**Descrição:**
O sistema deve permitir que o Estoquista consulte a quantidade e situação dos produtos em estoque.

**Objetivo:**
Fornecer visibilidade da situação do estoque.

**Stakeholders:**
Estoquista.

**Ator principal:**
Estoquista.

**Pré-condições:**
Usuário autenticado com perfil Estoquista.

**Entradas:**
Filtro opcional por produto.

**Processamento esperado:**
O sistema deve recuperar as quantidades atuais de cada produto em estoque.

**Saídas/Resultados:**
Lista de produtos com respectivas quantidades.

**Pós-condições:**
Nenhuma alteração de estado.

**Fluxos alternativos/exceções:**
Estoque vazio ou produto não encontrado.

**Regras de negócio relacionadas:**
—

**Prioridade:**
Média

**Status:**
Aprovado

**Critérios de aceite:**
- Exibir quantidade atualizada de todos os produtos.

**Casos de uso relacionados:**
UC-009

**Tarefas relacionadas:**
TASK-014

**Casos de teste relacionados:**
CT-016
```

```markdown
## RF-014 — Registrar entrada de produto no estoque

**Título:**
Registro de entrada de produto.

**Descrição:**
O sistema deve permitir que o Estoquista registre a entrada de um produto no estoque.

**Objetivo:**
Manter o saldo de estoque atualizado após recebimento de produtos.

**Stakeholders:**
Estoquista.

**Ator principal:**
Estoquista.

**Pré-condições:**
Usuário autenticado com perfil Estoquista.

**Entradas:**
Produto, quantidade, data.

**Processamento esperado:**
O sistema deve registrar a movimentação como entrada e somar a quantidade ao saldo atual do produto.

**Saídas/Resultados:**
Estoque atualizado.

**Pós-condições:**
Nova quantidade disponível para consulta.

**Fluxos alternativos/exceções:**
Quantidade informada inválida (zero ou negativa).

**Regras de negócio relacionadas:**
RN-007

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Impedir quantidade zero ou negativa.
- Atualizar saldo imediatamente após o registro.

**Casos de uso relacionados:**
UC-010

**Tarefas relacionadas:**
TASK-015

**Casos de teste relacionados:**
CT-017
```

```markdown
## RF-015 — Registrar saída de produto do estoque

**Título:**
Registro de saída de produto.

**Descrição:**
O sistema deve permitir que o Estoquista registre a saída de um produto do estoque.

**Objetivo:**
Manter o saldo de estoque atualizado após consumo ou expedição de produtos.

**Stakeholders:**
Estoquista.

**Ator principal:**
Estoquista.

**Pré-condições:**
Usuário autenticado com perfil Estoquista; saldo suficiente do produto.

**Entradas:**
Produto, quantidade, data.

**Processamento esperado:**
O sistema deve validar o saldo disponível, registrar a movimentação como saída e subtrair a quantidade do saldo atual.

**Saídas/Resultados:**
Estoque atualizado.

**Pós-condições:**
Nova quantidade disponível para consulta.

**Fluxos alternativos/exceções:**
Saldo insuficiente para a saída solicitada.

**Regras de negócio relacionadas:**
RN-007

**Prioridade:**
Alta

**Status:**
Aprovado

**Critérios de aceite:**
- Impedir saída maior que o saldo disponível.
- Atualizar saldo imediatamente após o registro.

**Casos de uso relacionados:**
UC-010

**Tarefas relacionadas:**
TASK-016

**Casos de teste relacionados:**
CT-018
```

```markdown
## RF-016 — Realizar manutenção do sistema

**Título:**
Execução de rotinas de manutenção.

**Descrição:**
O sistema deve permitir que o Administrador execute rotinas de manutenção (ex.: limpeza de logs, verificação de integridade de dados).

**Objetivo:**
Garantir o funcionamento contínuo e saudável do sistema.

**Stakeholders:**
Administrador do sistema.

**Ator principal:**
Administrador do sistema.

**Pré-condições:**
Usuário autenticado com perfil Administrador.

**Entradas:**
Tipo de rotina de manutenção a executar.

**Processamento esperado:**
O sistema deve executar a rotina selecionada e registrar o resultado em log.

**Saídas/Resultados:**
Rotina executada e log gerado.

**Pós-condições:**
Sistema mantido em condição operacional.

**Fluxos alternativos/exceções:**
Falha na execução da rotina.

**Regras de negócio relacionadas:**
RN-008

**Prioridade:**
Média

**Status:**
Aprovado

**Critérios de aceite:**
- Registrar log de toda rotina executada, com sucesso ou falha.

**Casos de uso relacionados:**
UC-011

**Tarefas relacionadas:**
TASK-017

**Casos de teste relacionados:**
CT-019
```

```markdown
## RF-017 — Atualizar dados e configurações do sistema

**Título:**
Atualização de configurações do sistema.

**Descrição:**
O sistema deve permitir que o Administrador atualize parâmetros e configurações gerais do sistema.

**Objetivo:**
Permitir a adaptação do sistema a novas necessidades operacionais.

**Stakeholders:**
Administrador do sistema.

**Ator principal:**
Administrador do sistema.

**Pré-condições:**
Usuário autenticado com perfil Administrador.

**Entradas:**
Parâmetro a alterar, novo valor.

**Processamento esperado:**
O sistema deve validar e persistir a nova configuração.

**Saídas/Resultados:**
Configuração atualizada.

**Pós-condições:**
Sistema passa a operar com a nova configuração.

**Fluxos alternativos/exceções:**
Valor de configuração inválido.

**Regras de negócio relacionadas:**
RN-008

**Prioridade:**
Média

**Status:**
Aprovado

**Critérios de aceite:**
- Validar valor antes de persistir.
- Registrar histórico de alterações de configuração.

**Casos de uso relacionados:**
UC-011

**Tarefas relacionadas:**
TASK-018

**Casos de teste relacionados:**
CT-020
```

---

## 3. Requisitos Não Funcionais (RNF)

```markdown
## RNF-001 — Controle de acesso por perfil

**Categoria:**
Segurança

**Descrição:**
O sistema deve restringir o acesso às funcionalidades exclusivamente ao(s) perfil(is) autorizado(s) para cada operação.

**Justificativa:**
Evitar que usuários realizem operações fora de sua área de responsabilidade (ex.: funcionário registrando pagamento).

**Métrica/Critério mensurável:**
100% das tentativas de acesso indevido devem ser bloqueadas e registradas em log.

**Escopo:**
Todo o sistema.

**Prioridade:**
Crítica

**Status:**
Aprovado

**Requisitos relacionados:**
RF-001, RF-002, RF-004, RF-009 a RF-017

**Casos de teste relacionados:**
CT-021
```

```markdown
## RNF-002 — Tempo de resposta das consultas

**Categoria:**
Desempenho

**Descrição:**
O sistema deve apresentar o resultado de consultas de dados (fazenda, tarefas, estoque) em tempo adequado à operação diária.

**Justificativa:**
Consultas lentas prejudicam o uso do sistema em campo, muitas vezes em conexões limitadas.

**Métrica/Critério mensurável:**
O sistema deve apresentar o resultado das consultas em até 2 segundos para 95% das requisições, sob carga operacional definida.

**Escopo:**
Funcionalidades de consulta (RF-003, RF-007, RF-013).

**Prioridade:**
Alta

**Status:**
Aprovado

**Requisitos relacionados:**
RF-003, RF-007, RF-013

**Casos de teste relacionados:**
CT-022
```

```markdown
## RNF-003 — Disponibilidade do sistema

**Categoria:**
Disponibilidade

**Descrição:**
O sistema deve estar disponível para uso durante o horário operacional da fazenda.

**Justificativa:**
Indisponibilidade compromete o registro de tarefas e movimentações no momento em que ocorrem.

**Métrica/Critério mensurável:**
Disponibilidade mínima de 99% no horário operacional definido (06h–20h).

**Escopo:**
Todo o sistema.

**Prioridade:**
Alta

**Status:**
Proposto

**Requisitos relacionados:**
—

**Casos de teste relacionados:**
CT-023
```

```markdown
## RNF-004 — Usabilidade em campo

**Categoria:**
Usabilidade

**Descrição:**
O sistema deve permitir que o funcionário registre atualizações e status de tarefas com poucas interações, mesmo em dispositivos móveis usados no campo.

**Justificativa:**
Funcionários realizam a maior parte de suas interações fora de um escritório, muitas vezes com conectividade limitada.

**Métrica/Critério mensurável:**
Registro de atualização de status de tarefa em no máximo 3 toques/cliques.

**Escopo:**
Dashboard e funcionalidades do Funcionário (RF-007, RF-008).

**Prioridade:**
Média

**Status:**
Proposto

**Requisitos relacionados:**
RF-007, RF-008

**Casos de teste relacionados:**
CT-024
```

```markdown
## RNF-005 — Integridade dos dados financeiros e de estoque

**Categoria:**
Confiabilidade

**Descrição:**
O sistema deve garantir que os saldos de estoque e os registros financeiros permaneçam consistentes mesmo em caso de falhas durante o processamento.

**Justificativa:**
Inconsistências em estoque ou financeiro geram prejuízo direto à operação da fazenda.

**Métrica/Critério mensurável:**
Nenhuma movimentação de estoque ou financeira deve ser parcialmente persistida em caso de falha (operação atômica).

**Escopo:**
RF-010, RF-011, RF-012, RF-014, RF-015.

**Prioridade:**
Crítica

**Status:**
Aprovado

**Requisitos relacionados:**
RF-010, RF-011, RF-012, RF-014, RF-015

**Casos de teste relacionados:**
CT-025
```

---

## 4. Rastreabilidade resumida

```text
RN-001 → RF-001, RF-002
RN-002 → RF-004
RN-003 → RF-005, RF-006
RN-004 → RF-007
RN-005 → RF-008
RN-006 → RF-009, RF-010, RF-011, RF-012
RN-007 → RF-014, RF-015
RN-008 → RF-016, RF-017
RN-009 → RF-001

RNF-001 → RF-001, RF-002, RF-004, RF-009..RF-017
RNF-002 → RF-003, RF-007, RF-013
RNF-003 → (todo o sistema)
RNF-004 → RF-007, RF-008
RNF-005 → RF-010, RF-011, RF-012, RF-014, RF-015
```

---

## 5. Observações finais

- Os identificadores `UC-XXX`, `TASK-XXX` e `CT-XXX` referenciados acima estão reservados para as próximas etapas do fluxo (casos de uso, tarefas de desenvolvimento e casos de teste) e devem ser detalhados quando essas atividades forem realizadas.
- Todos os itens seguem os templates e a nomenclatura de status/prioridade definidos no padrão da atividade.
- Este documento deve ser atualizado conforme a evolução do projeto, mantendo a rastreabilidade entre regra de negócio → requisito → caso de uso → tarefa → teste.
