# Colaboração e versionamento

[[Decisao Repositorio|← Índice]]

## Como cerca de 10 pessoas podem trabalhar juntas?

Um repositório único não gera conflitos automaticamente. O Git costuma integrar mudanças em arquivos diferentes. Conflitos textuais aparecem principalmente em alterações incompatíveis nas mesmas regiões; conflitos de comportamento podem existir mesmo quando o merge ocorre sem avisos.

O maior ponto de atenção são áreas compartilhadas: autenticação, rotas, configurações, dependências e contratos entre módulos.

## Processo mínimo recomendado

1. Proteger a branch `main` e evitar alterações diretas.
2. Criar branches curtas, com objetivo definido.
3. Abrir PRs pequenos, sem misturar mudanças não relacionadas.
4. Exigir revisão e testes automatizados antes do merge.
5. Definir responsáveis por módulos e pelo núcleo compartilhado.
6. Relacionar PRs às tarefas e registrar impacto, testes e mudanças de banco.
7. Identificar versões publicadas e manter um histórico de deploys.

No multirepo, aplicar padrões equivalentes em todos os projetos. No monorepo, estabelecer responsáveis por diretório e testes por aplicação afetada.

## Revisões com CODEOWNERS

Exemplo para um monorepo, usando equipes fictícias de uma organização GitHub:

```text
# .github/CODEOWNERS
/modules/Cantina/  @organizacao/equipe-cantina
/modules/Horarios/ @organizacao/equipe-horarios
/modules/Chatbot/  @organizacao/equipe-chatbot
/shared/          @organizacao/equipe-core
```

Os caminhos e equipes devem corresponder ao projeto real. Os responsáveis precisam ter as permissões adequadas.

> [!warning] Revisão não é restrição de edição
> CODEOWNERS pode solicitar revisores e, com as regras apropriadas, exigir aprovação dos responsáveis. Ele não impede alguém de modificar arquivos nem restringe a leitura por pasta. A disponibilidade de regras também depende do plano e da visibilidade do repositório.

## Testes no monorepo

- Mudança na Cantina: testar a Cantina e as integrações afetadas.
- Mudança em componente compartilhado: testar todos os consumidores afetados.
- Mudança em autenticação, configuração ou dependências globais: ampliar a validação.

Filtros por caminho ajudam, mas devem incluir arquivos compartilhados e dependências. Um workflow obrigatório ignorado por filtros pode deixar a verificação pendente; planejar uma verificação final que sempre reporte o resultado ao PR.

## Rastreabilidade

O Git registra commits e permite consultar o histórico de cada arquivo em ambos os modelos. Para localizar a origem de um problema, manter:

- Descrição clara do PR e vínculo com a tarefa.
- Registro do commit e do artefato efetivamente publicados.
- Resultado dos testes e identificação da versão em produção.
- Registro das migrações de banco e alterações de configuração.

O método de merge influencia o histórico: squash reúne o PR em um commit; outras estratégias podem preservar os commits individuais. Definir uma convenção para o grupo.

## Reverter código não é o mesmo que voltar produção

| Operação | Resultado | Atenção |
| --- | --- | --- |
| Reverter commit | Cria uma mudança inversa, normalmente com `git revert` | Pode conflitar com mudanças posteriores |
| Reverter PR | Desfaz as mudanças do PR conforme o histórico e a estratégia de merge | Pode afetar vários módulos se o PR os alterou |
| Rollback de deploy | Republica uma versão anterior | Precisa continuar compatível com banco, configuração e integrações |

Uma reversão não desfaz automaticamente efeitos já produzidos: dados gravados, mensagens enviadas ou operações externas.

**Regra prática:** PRs pequenos facilitam a reversão; versões publicadas e migrações compatíveis facilitam o rollback.
