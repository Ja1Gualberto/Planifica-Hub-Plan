# Levantamento e decisão

[[Decisao Repositorio|← Índice]]

## 1. Perguntas para a reunião

### Objetivo da unificação
- [ ] O professor espera uma aplicação única ou um portal com aplicações integradas?
- [ ] O usuário precisa de login único, aparência comum e navegação integrada? Quais desses itens são obrigatórios?
- [ ] É aceitável acessar módulos em endereços diferentes?

### Autonomia e operação
- [ ] Cada módulo precisa ser publicado sem republicar os demais?
- [ ] Quais funcionalidades devem continuar disponíveis se um módulo falhar?
- [ ] Quem manterá hospedagem, autenticação, backups e pipelines?
- [ ] Qual prazo e capacidade da equipe para migração ou integração?

### Código, dados e segurança
- [ ] Quais tecnologias e versões cada projeto utiliza?
- [ ] Como cada sistema autentica usuários atualmente?
- [ ] Há um provedor de identidade institucional disponível?
- [ ] Quais regras de negócio e dados precisam ser compartilhados?
- [ ] Existem testes, documentação e responsáveis definidos?
- [ ] Algum projeto exige restrições específicas de acesso ao código ou aos dados?

## 2. Inventário dos projetos

Preencher sem assumir que os três sistemas possuem a mesma estrutura.

| Item | Cantina | Horários | Chatbot |
| --- | --- | --- | --- |
| URL do repositório | A levantar | A levantar | A levantar |
| Responsáveis | A levantar | A levantar | A levantar |
| Linguagem e framework | A levantar | A levantar | A levantar |
| Versões e dependências | A levantar | A levantar | A levantar |
| Banco e dados utilizados | A levantar | A levantar | A levantar |
| Autenticação e permissões | A levantar | A levantar | A levantar |
| Hospedagem e deploy | A levantar | A levantar | A levantar |
| Testes automatizados | A levantar | A levantar | A levantar |
| Integrações existentes | A levantar | A levantar | A levantar |

## 3. Prova de conceito proposta

**Objetivo:** medir o esforço de integrar um sistema existente sem migrar os três antecipadamente.

**Candidato inicial:** Cantina, sujeito à disponibilidade da equipe e à análise do código.

### Entregas e critérios de sucesso

- [ ] Hub mínimo com navegação e autenticação por provedor de identidade.
- [ ] Cantina integrada à mesma identidade, sem depender de cópia manual de senhas.
- [ ] Usuário autorizado consegue acessar as funcionalidades previstas.
- [ ] Usuário sem permissão é bloqueado também ao acessar diretamente o módulo.
- [ ] Cantina é atualizada sem republicar o Hub.
- [ ] Em ambiente de teste, indisponibilidade da Cantina não impede o acesso ao Hub e aos módulos disponíveis.
- [ ] Uma versão anterior pode ser republicada, com compatibilidade de dados verificada.
- [ ] Esforço, dificuldades e requisitos de operação são registrados.

Simulações de falha devem ocorrer em ambiente de teste, sem comprometer o atendimento real.

### O que comparar depois

| Critério | Evidência esperada |
| --- | --- |
| Esforço inicial | Tempo gasto e adaptações necessárias |
| Reaproveitamento | Código preservado e código reescrito |
| Autonomia | Publicação e rollback demonstrados |
| Segurança | Autenticação e autorização verificadas |
| Disponibilidade | Comportamento durante falhas simuladas |
| Manutenção | Pessoas, ferramentas e rotinas necessárias |
| Alternativa de migração | Estimativa de esforço para levar o mesmo módulo a uma aplicação única |

## 4. Orientação preliminar

Investigar primeiro **manter as aplicações e os repositórios existentes**, caso o objetivo seja um portal integrado. Isso evita iniciar uma migração ampla antes de conhecer seus benefícios e custos.

Essa direção não é uma decisão final:

- Se a integração distribuída exigir operação que o grupo não consiga manter, reavaliar.
- Se o objetivo for uma aplicação única e as tecnologias forem compatíveis, considerar o monólito modular.
- Se a dificuldade estiver na coordenação do código, mas não na operação, considerar monorepo com aplicações separadas.

**Não escolher multirepo apenas para evitar conflitos, nem monorepo apenas para dar ao usuário a sensação de sistema único.**

## 5. Registro da decisão — preencher após o levantamento

| Campo | Registro |
| --- | --- |
| Status | Proposta em levantamento |
| Data da decisão | A definir |
| Participantes | A definir |
| Objetivo de unificação acordado | A definir |
| Organização dos repositórios | A definir |
| Arquitetura da aplicação | A definir |
| Unidades de deploy | A definir |
| Estratégia de identidade e permissões | A definir |
| Responsáveis pelos dados | A definir |
| Motivos e evidências | A registrar |
| Alternativas descartadas e motivos | A registrar |
| Riscos aceitos e medidas de mitigação | A registrar |
| Plano de transição | A definir |
| Condições para revisar a decisão | A definir |

### Cuidados caso haja migração de código

- Planejar se o histórico Git será importado ou se os repositórios antigos permanecerão como referência.
- Não arquivar os projetos antigos antes de validar a substituição e combinar a transição com os responsáveis.
- Preservar autoria, licenças e referências às tarefas existentes.
- Não copiar credenciais ou segredos para o novo repositório.
- Migrar por etapas, com validação funcional, de dados e de recuperação.
