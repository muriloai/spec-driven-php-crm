<!--
Sync Impact Report
- Version change: 1.0.0 → 1.1.0
- Modified principles:
  - Artigo III. Segurança, Privacidade e Segredos → Artigo III. Segurança, Privacidade e Menor Privilégio (expansão material para 10 incisos cobrindo validação, segurança da aplicação/OWASP, privilégios separados de runtime e migração, gestão e revogação de segredos, finalidade/base legal, retenção/descarte em bancos e backups, direitos dos titulares, proteção criptográfica, restrições ao uso de IA/MCP e resposta a incidentes)
- Added sections: Nenhuma (expansão interna do Artigo III)
- Removed sections: Nenhuma
- Templates / artefatos verificados:
  - ✅ .specify/memory/constitution.md (atualizado para v1.1.0)
  - ✅ .cursorrules (preservado; compatível)
- Follow-up TODOs / Pendências técnicas (aguardando definição em specs/planos futuros):
  - TODO(STACK_PHP_VERSION): Definir versão alvo do PHP e extensões obrigatórias
  - TODO(FRAMEWORK_OR_ARCH): Definir uso de framework ou PHP puro e estrutura de diretórios
  - TODO(DATABASE_ENGINE): Definir SGBD, estratégia de conexão e ferramenta de migrações
  - TODO(QUALITY_TOOLCHAIN): Definir comandos de testes, análise estática e verificação de estilo
  - TODO(HOSTING_AND_ENV): Definir ambiente de execução, servidor web, gestão de segredos/chaves e estratégia de backup
-->

# Constituição do Projeto spec-driven-php-crm

## Princípios Fundamentais

### Artigo I. Idioma e Clareza

- **OBRIGATÓRIO**: Toda documentação produzida para o projeto DEVE ser escrita em português
  brasileiro (`pt-BR`), abrangendo esta constituição, especificações (`spec.md`), planos de
  implementação (`plan.md`), listas de tarefas (`tasks.md`), registros de decisão, checklists,
  mensagens de commit (conforme `.cursorrules`) e relatórios de validação.
- **OBRIGATÓRIO**: Identificadores técnicos de código, nomes de APIs, pacotes, comandos de
  terminal, variáveis de ambiente e caminhos de arquivos DEVEM ser preservados no idioma original
  sempre que a tradução comprometer sua precisão ou execução.
- **OBRIGATÓRIO**: Os mesmos termos de negócio DEVEM ser utilizados de forma consistente nas
  especificações, no código-fonte, no esquema de dados e na interface do usuário. Conceitos de
  domínio que possam gerar ambiguidade ou confusão DEVEM ser explicitamente definidos na spec
  correspondente antes da implementação.

**Racional**: A padronização em português brasileiro com vocabulário único elimina ruídos de
interpretação entre requisitos e implementação, enquanto a preservação de termos técnicos evita
ambiguidades operacionais.

### Artigo II. Simplicidade e Organização

- **OBRIGATÓRIO (KISS)**: Deve-se sempre escolher a solução mais simples que satisfaça
  integralmente os requisitos confirmados.
- **OBRIGATÓRIO (YAGNI)**: É PROIBIDO implementar capacidades, parâmetros, tabelas, pontos de
  extensão ou estruturas arquiteturais para necessidades futuras ainda não confirmadas em spec.
- **OBRIGATÓRIO (DRY com critério)**: Regras de negócio compartilhadas DEVEM ter representação
  única, evitando duplicação entre interface, controllers, tarefas agendadas e integrações. Não se
  DEVE criar abstrações prematuras apenas por semelhança superficial de código.
- **OBRIGATÓRIO (SOLID pragmático)**: As classes e módulos DEVEM manter responsabilidades claras,
  dependências explícitas e contratos coerentes, sem exigir a criação sistemática de interfaces ou
  camadas intermediárias para todas as estruturas quando não houver variação real.
- **OBRIGATÓRIO**: As regras de negócio DEVEM permanecer separadas da camada de apresentação e dos
  detalhes de infraestrutura (acesso a banco, transporte HTTP, sistema de arquivos e serviços
  externos).
- **OBRIGATÓRIO**: É PROIBIDO impor DDD, Clean Architecture, CQRS, microsserviços, Event Sourcing
  ou outros padrões complexos sem necessidade técnica e de negócio demonstrada e aprovada.

**Racional**: O controle rigoroso de complexidade reduz o custo de manutenção e impede que
abstrações especulativas dificultem a evolução do sistema.

### Artigo III. Segurança, Privacidade e Menor Privilégio

1. **Validação e autorização.** Toda entrada externa DEVE ser validada no componente confiável que
   a recebe, quanto a tipo, formato, tamanho, limites e regras aplicáveis. A autorização DEVE ser
   verificada em cada operação protegida e para cada recurso acessado, aplicando menor privilégio e
   negação por padrão.
2. **Segurança da aplicação.** O sistema DEVE utilizar mecanismos consolidados de autenticação e
   proteção de sessões, tokens ou outras credenciais, conforme a arquitetura adotada. O plano DEVE
   identificar as ameaças aplicáveis e os controles verificáveis para mitigá-las, usando
   referências como OWASP.
3. **Privilégios de execução.** Em ambientes compartilhados e de produção, as identidades
   utilizadas pela aplicação DEVEM ter somente as permissões necessárias ao funcionamento, sem
   privilégios administrativos ou de alteração estrutural do banco. Migrações DEVEM utilizar
   identidade separada, com permissões limitadas ao necessário e execução controlada. As
   credenciais de migração NÃO DEVEM estar disponíveis ao runtime.
4. **Segredos.** Segredos DEVEM permanecer fora do código-fonte e dos arquivos versionados (sendo
   proibido versionar credenciais, tokens, chaves privadas, arquivos de ambiente com segredos,
   dumps ou dados reais de clientes, e devendo exemplos de configuração conter somente valores
   fictícios), sendo disponibilizados por mecanismo de configuração ou gestão de segredos adequado
   ao ambiente. NÃO DEVEM ser expostos em logs, respostas, documentação, prompts ou saídas de
   ferramentas. Deve existir procedimento de revogação e substituição em caso de exposição.
5. **Finalidade e classificação.** O sistema DEVE coletar, exibir e registrar apenas os dados
   pessoais necessários à finalidade definida. As especificações que envolvam dados pessoais DEVEM
   identificar categorias de dados, finalidades, acessos e requisitos de compartilhamento. A base
   legal e as obrigações aplicáveis DEVEM ser definidas ou validadas pelo responsável pelo
   tratamento, sem serem presumidas pelo agente.
6. **Retenção e descarte.** As especificações pertinentes DEVEM definir prazos ou critérios de
   retenção, eventos que iniciam sua contagem e hipóteses justificadas de conservação. O plano DEVE
   estabelecer descarte ou anonimização verificáveis, contemplando bancos, arquivos, logs, réplicas
   e o ciclo de expiração dos backups. Restaurações NÃO DEVEM reintroduzir permanentemente dados
   cuja eliminação já tenha sido determinada.
7. **Direitos dos titulares.** O sistema DEVE permitir o atendimento dos direitos aplicáveis, com
   verificação adequada da identidade do solicitante e registro do atendimento. Eliminação,
   bloqueio e anonimização DEVEM respeitar obrigações de conservação e preservar a integridade
   necessária dos registros. Exclusão lógica, mascaramento e pseudonimização NÃO DEVEM ser
   apresentados como eliminação ou anonimização efetiva.
8. **Proteção criptográfica.** Dados classificados como confidenciais ou sensíveis DEVEM ser
   protegidos em trânsito e em repouso, incluindo suas cópias de segurança. O plano DEVE definir o
   alcance dessa proteção e a gestão das chaves, com controle de acesso e separação adequada dos
   dados protegidos. Senhas usadas para autenticação DEVEM ser armazenadas por hash específico para
   senhas, com salt e parâmetros adequados.
9. **Uso de IA no desenvolvimento.** É PROIBIDO disponibilizar dados pessoais reais de clientes,
   segredos ou conteúdo confidencial de produção aos assistentes de desenvolvimento, inclusive por
   arquivos, prompts, logs, terminal, MCP ou resultados de consultas. Utilizar dados sintéticos ou
   dados cuja anonimização tenha sido validada. Esquemas e metadados também DEVEM ser revisados
   antes de serem disponibilizados. Arquivos de ignore NÃO DEVEM ser tratados como barreira
   suficiente de acesso.
10. **Evidência e resposta a incidentes.** Os controles acima DEVEM possuir verificações
    proporcionais ao risco. Antes da operação com dados reais, DEVEM estar definidos responsáveis e
    procedimentos para identificar, conter e tratar incidentes, preservando evidências e apoiando
    as comunicações legalmente aplicáveis.

**Racional**: Um CRM manipula dados pessoais e estratégicos; a aplicação estrita de menor
privilégio (inclusive entre runtime, migrações e assistentes de IA), ciclo de vida de dados e
controles criptográficos e de resposta a incidentes garante conformidade legal e proteção efetiva.

### Artigo IV. Integridade dos Dados

- **OBRIGATÓRIO**: Invariantes de negócio DEVEM ser garantidas tanto por validações na aplicação
  quanto por restrições apropriadas no banco de dados (chaves estrangeiras, obrigatoriedade `NOT
  NULL`, tipos adequados, checks e índices de unicidade quando exigidos pelo domínio).
- **OBRIGATÓRIO**: Operações que envolvam múltiplas alterações dependentes entre si DEVEM ser
  executadas dentro de transações ACID, garantindo conclusão integral ou reversão conjunta. O
  código NÃO DEVE presumir que o rollback de banco desfaz chamadas externas já disparadas.
- **OBRIGATÓRIO**: Fluxos sujeitos a alterações simultâneas DEVEM tratar concorrência de forma
  explícita (por exemplo, bloqueio otimista ou pessimista conforme o caso), impedindo perda
  silenciosa de atualizações (`lost updates`).
- **OBRIGATÓRIO**: Regras de unicidade e detecção de duplicidade DEVEM ser definidas na spec
  conforme a regra real do negócio. É PROIBIDO presumir sem confirmação que atributos como nome ou
  e-mail identificam unicamente um cliente ou contato.
- **OBRIGATÓRIO**: Toda alteração estrutural de banco ou transformação de dados DEVE ser versionada
  por migrações ou scripts controlados. Migrações já aplicadas em ambientes compartilhados NÃO
  DEVEM ser editadas retroativamente para alterar seu efeito.
- **OBRIGATÓRIO**: Mudanças com risco de perda ou alteração destrutiva de dados DEVEM documentar
  previamente o impacto, a estratégia de execução e o procedimento de recuperação. Não se DEVE
  presumir que toda migração possui reversão automática sem perda.

**Racional**: A confiabilidade de um CRM depende diretamente da consistência, rastreabilidade e
recuperabilidade dos dados persistidos.

### Artigo V. Confiabilidade e Operação

- **OBRIGATÓRIO**: Falhas DEVEM ser tratadas explicitamente. É PROIBIDO silenciar exceções ou
  reportar sucesso ao usuário ou chamador quando uma operação falhar total ou parcialmente.
- **OBRIGATÓRIO**: Operações sujeitas a repetição (reenvio de formulários, retentativas de rede,
  webhooks ou tarefas agendadas) DEVEM utilizar mecanismos de idempotência ou deduplicação sempre
  que efeitos duplicados forem prejudiciais ao negócio.
- **OBRIGATÓRIO**: Quando houver integrações externas, a implementação DEVE definir limites de
  tempo (`timeouts`), política controlada de novas tentativas (`retries`) e tratamento explícito de
  falhas parciais, evitando retentativas ilimitadas e repetição insegura de efeitos externos.
- **OBRIGATÓRIO**: O sistema DEVE produzir logs estruturados ou claros o suficiente para
  diagnóstico operacional, identificando o contexto da operação e omitindo ou mascarando dados
  sensíveis e segredos.
- **OBRIGATÓRIO**: As specs DEVEM definir quais ações críticas de negócio ou segurança exigem
  trilha de auditoria, registrando no mínimo autor, data/hora, recurso afetado e ação realizada, com
  proteção do histórico contra acesso ou adulteração indevidos.
- **OBRIGATÓRIO**: Antes de iniciar operação com dados reais, a estratégia de backup, o
  procedimento de restauração e a evidência prática de teste de recuperação DEVEM estar
  documentados e validados.

**Racional**: Sistemas de negócio precisam ter comportamento previsível sob falha e capacidade
comprovada de diagnóstico, auditoria e recuperação.

### Artigo VI. Qualidade e Evidências

- **OBRIGATÓRIO**: Regras de negócio críticas, controles de autorização e a integridade dos fluxos
  criados ou alterados DEVEM ser verificados por testes automatizados.
- **OBRIGATÓRIO**: Toda correção de bug DEVE incluir teste automatizado de regressão quando viável.
  Quando a automação não for tecnicamente viável, o responsável DEVE registrar a justificativa e a
  evidência da verificação alternativa executada.
- **OBRIGATÓRIO**: A suíte de testes DEVE priorizar cenários relevantes de negócio, casos de borda,
  limites e caminhos de falha. Não se DEVE adotar um percentual arbitrário de cobertura de código
  como prova isolada de qualidade.
- **OBRIGATÓRIO**: O plano de implementação (`plan.md`) DEVE especificar os comandos exatos e
  reproduzíveis para execução de testes, análise estática e verificação de estilo apropriados à
  stack definida.
- **OBRIGATÓRIO**: Todas as verificações aplicáveis DEVEM ser efetivamente executadas antes de
  declarar uma tarefa ou entrega como concluída. O relatório de entrega DEVE informar os comandos
  executados, os resultados obtidos, eventuais falhas e quaisquer verificações não executadas com
  seus respectivos impedimentos.
- **OBRIGATÓRIO**: É PROIBIDO desativar verificações, suprimir alertas sem justificativa técnica,
  enfraquecer asserções de testes ou alterar critérios de aceite da spec apenas para forçar a
  aprovação de uma entrega.

**Racional**: Qualidade verificável baseia-se em evidências reais e reproduzíveis de funcionamento
nos cenários críticos, e não em métricas artificiais ou declarações não comprovadas.

### Artigo VII. Experiência de Uso e Desempenho

- **OBRIGATÓRIO**: A interface DEVE manter padrões consistentes de navegação, estrutura de
  formulários, estados de carregamento e mensagens de feedback em todo o sistema.
- **OBRIGATÓRIO**: Os fluxos implementados DEVEM exibir mensagens de erro claras e compreensíveis
  para o usuário, preservar os dados já preenchidos no formulário em caso de erro de validação
  sempre que seguro e permitir operação completa por teclado.
- **OBRIGATÓRIO**: Ações destrutivas ou irreversíveis DEVEM exigir confirmação ou proteção
  proporcional ao risco, informando claramente ao usuário as consequências da operação antes de sua
  efetivação.
- **OBRIGATÓRIO**: Listagens, buscas, relatórios e processamentos em lote potencialmente volumosos
  DEVEM ser paginados ou limitados. A implementação DEVE evitar consultas repetitivas (como o
  problema `N+1`) e o carregamento desnecessário de registros ou colunas em memória.
- **OBRIGATÓRIO**: Metas de desempenho DEVEM ser definidas nas specs apenas quando forem
  relevantes para o fluxo especificado e verificadas com volume de dados representativo. É PROIBIDO
  inventar metas numéricas arbitrárias sem fundamentação no negócio ou na operação.

**Racional**: Produtividade operacional em um CRM exige interface previsível, acessível por
teclado, segura contra erros acidentais e responsiva diante do crescimento da base de dados.

### Artigo VIII. Dependências e Compatibilidade

- **OBRIGATÓRIO**: Toda nova dependência externa DEVE ser justificada pela necessidade concreta que
  atende no escopo aprovado.
- **OBRIGATÓRIO**: Antes de adicionar ou substituir qualquer dependência direta, o agente ou
  desenvolvedor DEVE apresentar ao responsável pelo projeto: finalidade, alternativas avaliadas
  (incluindo solução nativa), estado de manutenção, licença e impactos relevantes, aguardando
  aprovação explícita antes de instalá-la ou incluí-la no manifesto.
- **OBRIGATÓRIO**: Deve-se priorizar recursos nativos da linguagem/plataforma, bibliotecas oficiais
  e pacotes reconhecidos e ativamente mantidos. Popularidade isolada (número de downloads ou
  estrelas) NÃO é critério suficiente para adoção.
- **OBRIGATÓRIO**: Os arquivos de manifesto e de resolução de versões (`lockfiles`) aplicáveis à
  stack DEVEM ser versionados no Git para assegurar instalações determinísticas e reproduzíveis.
- **OBRIGATÓRIO**: É PROIBIDO alterar a stack tecnológica, versões principais (`major`), contratos
  públicos ou requisitos de infraestrutura sem explicitar previamente o impacto e obter aprovação
  formal do responsável pelo projeto.

**Racional**: O controle estrito de dependências protege o projeto contra riscos de cadeia de
suprimentos, incompatibilidades de licença e obsolescência prematura.

### Artigo IX. Living Spec e Rastreabilidade

- **OBRIGATÓRIO**: O projeto adota o modelo de **Living Spec** como política oficial de manutenção
  das especificações: cada spec aprovada representa o contrato vigente da funcionalidade que
  descreve.
- **OBRIGATÓRIO**: Qualquer mudança no comportamento pretendido do sistema DEVE atualizar primeiro
  a spec correspondente. O plano (`plan.md`) e a lista de tarefas (`tasks.md`) DEVEM ser alinhados
  com a spec atualizada antes do início ou alteração da implementação.
- **OBRIGATÓRIO**: Novas funcionalidades com escopo próprio DEVEM receber especificações próprias
  em diretórios dedicados, sendo PROIBIDO consolidar automaticamente todo o sistema em uma única
  spec monolítica.
- **OBRIGATÓRIO**: O histórico de evolução DEVE ser preservado no Git. Antes de substituir ou
  reestruturar planos ou tarefas existentes, as decisões técnicas e justificativas que continuem
  relevantes DEVEM permanecer registradas.
- **OBRIGATÓRIO**: É PROIBIDO alterar uma spec apenas para legitimar retrospectivamente um
  comportamento incorreto ou bug encontrado no código. Mudanças no contrato funcional exigem
  decisão explícita de produto/negócio.
- **OBRIGATÓRIO**: As listas de tarefas DEVEM manter tarefas já concluídas claramente distinguíveis
  do trabalho novo ou pendente. Nenhuma tarefa DEVE ser marcada como concluída sem a verificação
  efetiva do seu resultado.

**Racional**: Manter especificações vivas e sincronizadas com o código garante que a documentação
continue sendo uma fonte confiável da verdade ao longo de todo o ciclo de vida do produto.

### Artigo X. Governança Compartilhada e Exceções

- **OBRIGATÓRIO**: Esta constituição é o documento normativo compartilhado e vinculante para todos
  os agentes de IA (incluindo **Antigravity** e **Codex**) e colaboradores humanos que atuem neste
  repositório.
- **OBRIGATÓRIO**: Sempre que houver conflito entre uma solicitação pontual e as regras desta
  constituição, o conflito DEVE ser explicitado imediatamente ao responsável pelo projeto para
  tomada de decisão consciente. É PROIBIDO ignorar silenciosamente quer a constituição, quer a
  solicitação do usuário.
- **OBRIGATÓRIO**: Exceções temporárias ou permanentes a qualquer regra desta constituição exigem
  aprovação explícita e registro documentado contendo: motivo da exceção, escopo exato de
  aplicação, riscos assumidos e condição ou prazo de revisão.

**Racional**: Regras de governança unificadas evitam divergência de comportamento entre diferentes
agentes e ferramentas, garantindo decisões transparentes diante de trade-offs.

## Contexto Técnico Vigente e Decisões Pendentes

A inspeção do repositório na data desta versão confirma o seguinte estado real:

- **Configuração presente**:
  - Repositório Git inicializado como `spec-driven-php-crm`.
  - Estrutura Spec Kit (`v1.2.2`) configurada em `.specify/` com scripts PowerShell (`.ps1`) e
    integração de skills em `.agents/skills/`.
  - Regras de padronização de mensagens de commit em `.cursorrules` (Conventional Commits em
    português brasileiro).
- **Decisões técnicas ainda não definidas (registradas como pendências)**:
  - **Linguagem e Runtime (`TODO(STACK_PHP_VERSION)`)**: Embora o nome do repositório indique PHP,
    não há `composer.json`, arquivos `.php` ou versão do interpretador e extensões definidas.
  - **Arquitetura de Aplicação e Framework (`TODO(FRAMEWORK_OR_ARCH)`)**: Não há definição sobre uso
    de framework específico, microframework ou PHP sem framework, nem convenção de diretórios de
    código-fonte.
  - **Banco de Dados e Migrações (`TODO(DATABASE_ENGINE)`)**: Não há SGBD, driver de conexão ou
    ferramenta de migração definidos.
  - **Ferramentas de Qualidade (`TODO(QUALITY_TOOLCHAIN)`)**: Não há suíte de testes, analisador
    estático ou verificador de estilo configurados.
  - **Hospedagem e Operação (`TODO(HOSTING_AND_ENV)`)**: Não há definição de servidor web,
    ambiente de implantação, mecanismo de gestão de segredos/chaves ou rotina operacional de
    backup.

Nenhuma hipótese sobre os itens pendentes acima DEVE ser tratada como obrigação até que seja
explicitamente decidida e registrada no plano ou na constituição.

## Diretrizes Condicionais para Capacidades Futuras

- **OBRIGATÓRIO**: É PROIBIDO inventar funcionalidades de CRM, assumir multitenancy, impor
  arquitetura distribuída, criar filas de mensageria ou estipular metas numéricas de desempenho sem
  solicitação explícita.
- **OBRIGATÓRIO**: Regras específicas para **multitenancy** (isolamento lógico/físico por
  organização), **valores monetários** (precisão decimal, moedas, arredondamento), **anexos**
  (armazenamento, limites de tamanho, validação de MIME/antivírus), **filas assíncronas** e
  **integrações externas** somente DEVEM ser detalhadas e implementadas quando essas capacidades
  forem confirmadas em spec própria.

## Governança

- **Autoridade Normativa**: Esta constituição prevalece sobre práticas informais e orienta todas as
  etapas do fluxo Spec Kit (`/speckit-specify`, `/speckit-clarify`, `/speckit-plan`,
  `/speckit-tasks`, `/speckit-analyze`, `/speckit-checklist`, `/speckit-implement` e
  `/speckit-converge`) tanto no Antigravity quanto no Codex.
- **Procedimento de Emenda**:
  1. Qualquer alteração nos princípios ou regras desta constituição DEVE registrar a justificativa
     da mudança, obter aprovação explícita do responsável pelo projeto e avaliar o impacto sobre
     specs, planos, tarefas e código existentes.
  2. Caso já existam regras compatíveis quando uma revisão for solicitada, as decisões compatíveis
     DEVEM ser preservadas; eventuais conflitos com regras anteriores DEVEM ser informados ao
     responsável antes de sua substituição.
  3. A data de última alteração (`Last Amended`) e a versão (`Version`) DEVEM ser atualizadas a
     cada emenda.
- **Política de Versionamento Semântico (`MAJOR.MINOR.PATCH`)**:
  - `MAJOR`: Remoção de princípio, redefinição incompatível de regra existente ou mudança de
    governança que invalide decisões anteriores.
  - `MINOR`: Adição de novo artigo/princípio, inclusão de decisões técnicas antes pendentes ou
    expansão material das obrigações existentes.
  - `PATCH`: Ajustes de redação, correções ortográficas ou esclarecimentos que não alterem o
    sentido normativo das obrigações.
- **Verificação de Conformidade**: Toda especificação, plano, lista de tarefas e revisão de
  implementação DEVE verificar explicitamente a aderência aos artigos desta constituição antes de
  sua aprovação.

**Version**: 1.1.0 | **Ratified**: 2026-10-26 | **Last Amended**: 2026-10-26
