# Arquitetura e repositórios

[[Decisao Repositorio|← Índice]]

## Três decisões diferentes

| Dimensão | Pergunta | Exemplos |
| --- | --- | --- |
| Repositórios | Onde o código é versionado? | Monorepo ou multirepo |
| Arquitetura | Como as funcionalidades se organizam e se comunicam? | Aplicações integradas ou monólito modular |
| Deploy | O que é publicado como uma unidade? | Publicação conjunta ou independente |

**Monorepo e monólito modular não são alternativas equivalentes.** O primeiro descreve a organização do código; o segundo, a arquitetura da aplicação. Eles podem ser usados juntos.

## Opção A — Multirepo com aplicações integradas

Manter os três repositórios e criar um repositório para o Hub, caso ele precise de código próprio.

```text
ifro-hub       → portal e navegação
ifro-cantina   → aplicação da Cantina
ifro-horarios  → aplicação de Horários
ifro-chatbot   → aplicação do Chatbot
```

As aplicações se integram por autenticação padronizada e, quando necessário, APIs. O Hub não precisa copiar ou importar o código dos outros projetos.

**Vantagens**
- Preserva a organização dos projetos atuais.
- Facilita permissões de acesso ao código por projeto.
- Favorece ciclos de revisão e publicação próprios, quando os deploys também são independentes.

**Custos e riscos**
- Exige coordenar contratos de integração e suas versões.
- Autenticação, monitoramento e publicação precisam ser configurados entre aplicações.
- Uma funcionalidade que atravessa vários projetos pode exigir múltiplos PRs e uma ordem de publicação.

**Faz sentido quando:** os sistemas têm autonomia real e a equipe consegue manter a integração e a operação de várias aplicações.

## Opção B — Monorepo com aplicações separadas

Reunir os projetos em um repositório, sem obrigatoriamente transformá-los em uma aplicação única.

```text
ifro-plataforma/
  apps/
    hub/
    cantina/
    horarios/
    chatbot/
  packages/
  infra/
```

Estrutura ilustrativa, a adaptar às tecnologias existentes. Cada aplicação pode ter seu próprio build e deploy.

**Vantagens**
- Centraliza a descoberta do código e os padrões de colaboração.
- Permite revisar alterações coordenadas em um mesmo PR.
- Facilita manter bibliotecas compartilhadas junto das aplicações consumidoras.

**Custos e riscos**
- Exige pipelines que identifiquem aplicações afetadas, inclusive por dependências compartilhadas.
- Permissões de leitura do repositório não são normalmente separadas por diretório.
- Centralizar o código não elimina a necessidade de integrar as aplicações em execução.

**Faz sentido quando:** o grupo quer centralizar o trabalho no GitHub, mas preservar aplicações e publicações independentes.

## Opção C — Monorepo com monólito modular

Migrar e adaptar as funcionalidades para uma aplicação única, com limites internos claros.

```text
ifro-plataforma/
  app/
  modules/
    Cantina/
    Horarios/
    Chatbot/
  shared/
  tests/
```

Se o framework escolhido permitir, namespaces e interfaces explícitas ajudam a organizar os módulos. **Separar pastas, por si só, não garante baixo acoplamento.**

**Vantagens**
- Pode simplificar autenticação, chamadas internas e operação.
- Permite compartilhar infraestrutura e componentes da aplicação.
- Pode ter um único processo de publicação.

**Custos e riscos**
- A migração pode exigir adaptações relevantes de código, dados e dependências.
- Mudanças no núcleo compartilhado têm maior potencial de impacto.
- Em deploy único, o rollback operacional normalmente envolve a aplicação inteira.

**Faz sentido quando:** o objetivo é uma aplicação única e o custo de adaptação é menor que o de manter integrações distribuídas.

## Comparação resumida

| Critério | A: multirepo integrado | B: monorepo, aplicações separadas | C: monólito modular |
| --- | --- | --- | --- |
| Repositórios | Vários | Um | Um, na proposta apresentada |
| Deploy independente | Possível por aplicação | Possível por aplicação | Não no modelo de deploy único |
| Comunicação | Identidade compartilhada e APIs quando necessárias | Igual à opção A | Interfaces internas; APIs externas se necessário |
| Mudança coordenada | Múltiplos PRs e releases | Pode caber em um PR | Pode caber em um PR |
| Restrição de acesso ao código | Por repositório | Geralmente no repositório inteiro | Geralmente no repositório inteiro |
| Isolamento de falhas | Depende da execução e das dependências | Depende da execução e das dependências | Menor isolamento de recursos no deploy único |

Nenhuma opção garante, sozinha, menos conflitos, melhor desempenho ou menor custo. A decisão precisa considerar o estado dos sistemas e a capacidade de manutenção do grupo.
