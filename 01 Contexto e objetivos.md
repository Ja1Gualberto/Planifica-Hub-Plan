# Contexto e objetivos

[[Decisao Repositorio|← Índice]]

## Cenário informado

O grupo de programação mantém três projetos voltados ao IFRO — Campus Porto Velho Zona Norte:

| Projeto | Finalidade |
| --- | --- |
| Cantina | Controle de refeições |
| Horários | Gestão de horários acadêmicos |
| Chatbot | Atendimento aos alunos |

O professor responsável propôs unificá-los em um sistema ou plataforma. O grupo tem aproximadamente **10 pessoas**, com contribuições por Pull Requests ao longo das semanas.

## Demanda do levantamento

Avaliar como aproveitar os sistemas existentes e organizar o desenvolvimento da plataforma:

1. Manter os repositórios e integrar as aplicações por meio de um Hub.
2. Reunir o código em um novo repositório, preservando aplicações separadas.
3. Migrar e adaptar o código para uma aplicação única, organizada em módulos.

## Preocupações principais

- Como várias pessoas trabalham simultaneamente sem prejudicar os outros módulos?
- Como identificar a origem de uma alteração ou falha?
- Como reverter uma mudança sem desfazer trabalho não relacionado?
- Como publicar atualizações de cada sistema?
- Uma falha em um módulo pode afetar os demais?
- Quanto do código atual pode ser reaproveitado?
- Qual opção o grupo consegue manter no longo prazo?

## Tecnologias confirmadas

O integrante do grupo confirmou as seguintes tecnologias para os três projetos:

| Projeto | Linguagem | Framework | Banco de dados | Versões |
| --- | --- | --- | --- | --- |
| Cantina | PHP | CodeIgniter | MySQL | A confirmar |
| Horários | PHP | CodeIgniter | MySQL | A confirmar |
| Chatbot | PHP | CodeIgniter | MySQL | A confirmar |

Segundo o integrante do grupo, as versões dessas tecnologias são as mesmas nos três projetos. Os números exatos das versões ainda precisam ser verificados.

> [!important] Compatibilidade ainda não verificada
> Os três projetos usam as mesmas tecnologias principais e, conforme informado, as mesmas versões. Isso não confirma compatibilidade de todas as dependências ou estruturas de código. As informações foram fornecidas pelo integrante do grupo; os repositórios ainda não foram inspecionados.

## Organização e operação atuais

Conforme informado pelo integrante do grupo:

| Aspecto | Cantina | Horários | Chatbot |
| --- | --- | --- | --- |
| Banco de dados | Próprio | Próprio | Próprio |
| Autenticação | Login próprio | Login próprio | Login próprio |
| Publicação | Publicado | Publicado | Publicado |
| Processo de deploy | Automatizado | Automatizado | Automatizado |
| Deploy independente | Sim | Sim | Sim |

Cada sistema pode ser atualizado sem republicar os demais. Bancos separados e deploys independentes não significam necessariamente servidores separados ou isolamento de falhas.

Ainda não foram informados os locais de hospedagem ou o grau de isolamento da infraestrutura.

> [!question] Ferramenta de deploy a verificar
> O integrante do grupo acredita que a automação utiliza **GitHub Actions**, mas ainda vai confirmar. A ferramenta não deve ser tratada como informação confirmada.

### Possibilidade de consultas entre bancos

Segundo o integrante do grupo, os bancos são separados, mas **é tecnicamente possível consultar dados entre eles**. Essas consultas não acontecem atualmente.

Isso permite considerar **acesso direto aos bancos como alternativa a uma API para determinadas consultas**. Ainda precisam ser detalhados a configuração das conexões, as permissões e os dados que cada sistema poderá acessar. Essa possibilidade não confirma, por si só, que os bancos estejam no mesmo servidor.

> [!important] Possibilidade técnica não é decisão de arquitetura
> Uma API não é obrigatória para toda leitura entre projetos. Entretanto, consultas diretas fazem o consumidor depender da estrutura de tabelas, colunas e significados dos dados de outro sistema. Uma mudança nesse banco pode exigir alterações nos consumidores, mesmo com repositórios e deploys separados.

Se essa alternativa for adotada, definir acessos de privilégio mínimo, preferencialmente somente leitura para consultas, responsáveis pelos dados e regras para mudanças de schema. Consultar dados diretamente não equivale a executar as regras de negócio ou as verificações de autorização da aplicação que mantém esses dados; operações que alteram estado exigem avaliação própria.

## Infraestrutura e responsabilidade pela manutenção

- **Recursos para o Hub:** a possibilidade de reutilizar a infraestrutura existente ainda não foi avaliada. Não há confirmação sobre hospedagem, domínios ou subdomínios e automação disponíveis para a nova plataforma.
- **Operação e manutenção após a integração:** ficarão sob responsabilidade do próprio grupo de programação.

### Divisão de responsabilidades informada

| Atividade | Responsável |
| --- | --- |
| Hospedagem | Professor responsável |
| Deploy | Professor responsável |
| Backups | Professor responsável |
| Autenticação | Outros membros do grupo |

Os membros responsáveis pela autenticação ainda não foram identificados neste levantamento. Os procedimentos de operação e recuperação também precisam ser documentados.

## Integração pretendida e identidade

- **Identidade dos usuários:** foi indicada como algo a compartilhar na futura plataforma, evitando logins separados.
- **Demais requisitos de compartilhamento:** ainda não definidos, incluindo permissões comuns, dados e regras de negócio entre módulos.
- **Provedor de identidade institucional:** segundo o integrante do grupo, não há serviço disponível para esses projetos. A solução para identidade compartilhada ainda precisa ser definida.

O desejo de compartilhar identidade não determina, sozinho, se a plataforma será uma aplicação única ou um conjunto de aplicações integradas.

## O que ainda não está confirmado

Também faltam confirmar:

- Quais ferramentas automatizam o deploy de cada projeto.
- Os números exatos das versões de PHP, CodeIgniter e MySQL, informadas como iguais nos três projetos, e as versões das demais dependências.
- Quais dados e regras de negócio precisam ser compartilhados.
- Como funcionará o acesso entre bancos, quais permissões serão concedidas e quando usar consultas diretas ou interfaces das aplicações.
- Qual solução será adotada para identidade compartilhada, considerando a indisponibilidade de um provedor institucional para os projetos.
- Quais recursos de hospedagem, domínios e automação estão disponíveis.
- Quais membros cuidarão da autenticação e quais serão os procedimentos de operação, backup e recuperação.

## O que significa “unificar”?

> [!info] Situação confirmada com o integrante do grupo
> O formato da unificação **ainda não foi definido**. O professor apresentou a ideia e espera que o grupo investigue e proponha o formato.

Há pelo menos dois objetivos possíveis a avaliar, sem escolha aprovada até o momento:

**Plataforma de aplicações integradas:** o usuário entra por um portal, usa uma identidade comum e acessa aplicações que continuam independentes.

**Aplicação única:** os recursos passam a fazer parte de um mesmo sistema, com estrutura comum de autenticação, rotas e dependências.

O levantamento deve comparar esses resultados e fundamentar uma proposta para discussão com o professor. A escolha dos repositórios deve acompanhar o formato de unificação acordado, e não determiná-lo antecipadamente.
