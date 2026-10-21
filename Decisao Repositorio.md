# Planifica Hub — Decisão sobre repositórios

> [!summary] Pergunta central
> Para integrar os sistemas de Cantina, Horários e Chatbot, devemos manter os repositórios existentes ou reunir o código em um novo repositório?

**Status:** levantamento inicial; decisão pendente de alinhamento com o professor e a equipe.

## Navegação

1. [[01 Contexto e objetivos]] — cenário atual, demanda e pontos a confirmar.
2. [[02 Arquitetura e repositorios]] — alternativas e comparação técnica.
3. [[03 Colaboracao e versionamento]] — PRs, conflitos, rastreabilidade e reversões.
4. [[04 Integracao e deploy]] — como integrar, publicar e isolar falhas.
5. [[05 Levantamento e decisao]] — perguntas para reunião, prova de conceito e registro da decisão.

## Síntese para apresentar ao grupo

- **Unificar a experiência do usuário não exige unificar os repositórios.**
- Organização do código, arquitetura e deploy são decisões diferentes: um monorepo pode conter aplicações com deploy independente.
- Ter cerca de 10 desenvolvedores não torna um repositório único inviável. Revisões, testes, limites entre módulos e PRs pequenos são mais importantes do que a quantidade de repositórios.
- O GitHub armazena e versiona o código e pode automatizar a publicação. A integração em produção acontece entre as aplicações, não entre os repositórios.
- A quantidade de repositórios, sozinha, não garante isolamento de falhas nem facilidade de rollback.

## Direção inicial, ainda não aprovada

**Se a proposta for um portal com aplicações independentes:** investigar primeiro a manutenção dos repositórios existentes, com um Hub e autenticação centralizada.

**Se a proposta for uma aplicação única:** avaliar um monólito modular, reaproveitando e adaptando o código existente.

Antes de migrar os três projetos, validar a integração de apenas um deles em uma prova de conceito. Ver [[05 Levantamento e decisao]].

> [!note] Sobre este levantamento
> Estas notas organizam o contexto e a resposta fornecidos na conversa. Não houve inspeção dos códigos, repositórios ou da infraestrutura. As recomendações são preliminares.
