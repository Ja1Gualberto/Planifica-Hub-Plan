# Integração e deploy

[[Decisao Repositorio|← Índice]]

## Um repositório pode “usar” outros?

Sim, mas existem duas necessidades diferentes:

- **Usar uma aplicação em execução:** acessar sua interface ou API, sem incluir seu código no Hub.
- **Reutilizar código:** instalar um pacote ou incorporar uma referência/cópia de outro repositório durante o desenvolvimento ou build.

| Mecanismo | Uso | Não resolve sozinho |
| --- | --- | --- |
| APIs HTTP | Trocar dados e executar operações entre aplicações | Login único e compatibilidade de versões |
| OpenID Connect | Autenticar usuários por um provedor de identidade | Permissões de negócio de cada módulo |
| Composer, se o código for PHP | Instalar bibliotecas compartilhadas e versionadas | Integração entre aplicações completas |
| Git submodule | Referenciar outro repositório em um commit específico | Execução, autenticação ou publicação |
| Git subtree | Incorporar código externo e permitir sincronizações posteriores | Limites de arquitetura e operação |
| GitHub Actions | Automatizar testes, builds e deploys | Hospedagem permanente da aplicação |

Submodules e subtrees não são obrigatórios para construir um Hub. Se os sistemas continuam como aplicações independentes, integrar suas interfaces costuma ser mais relevante do que reunir seus códigos.

## Exemplo de acesso com login único

Endereços apenas ilustrativos:

1. O usuário entra em `hub.ifro.edu.br`.
2. É redirecionado ao provedor de identidade para autenticação.
3. O Hub recebe e valida a resposta de autenticação.
4. O usuário escolhe a Cantina e acessa `cantina.ifro.edu.br`.
5. A Cantina também usa o provedor de identidade. Uma sessão válida no provedor pode evitar nova digitação de credenciais.
6. A Cantina aplica suas regras de autorização.

O Hub não precisa ser o provedor de identidade. Se a instituição já possui um serviço compatível, avaliar sua utilização antes de construir uma solução própria.

**Login único não significa permissão irrestrita:** cada aplicação precisa validar a identidade e verificar o que o usuário pode fazer. As permissões podem combinar papéis centrais com regras locais.

## Onde o GitHub entra no deploy?

Fluxo típico:

```text
PR → revisão e testes → merge → build versionado → deploy na hospedagem
```

O GitHub Actions pode executar esse fluxo, mas a aplicação precisa de infraestrutura de execução. GitHub Pages hospeda conteúdo estático; não executa um backend PHP ou um banco MySQL.

Se PHP/CodeIgniter/MySQL forem confirmados, uma opção é hospedagem com PHP-FPM, Nginx ou Apache e um serviço MySQL. A escolha depende dos recursos disponíveis.

### Aplicações independentes — em um ou vários repositórios

Cada aplicação pode ter pipeline, versão e publicação próprios. Um monorepo também permite isso, desde que o pipeline identifique o que precisa ser construído e publicado.

### Monólito modular com deploy único

O build reúne os módulos em uma versão da aplicação. A publicação e o rollback operacional normalmente envolvem essa versão completa.

## Isolamento de falhas

Repositórios separados não garantem sistemas isolados. Avaliar:

- **Recursos:** CPU, memória, disco e conexões com o banco compartilhados.
- **Dependências:** indisponibilidade do provedor de identidade, do banco ou de uma API central.
- **Comunicação:** chamadas sem timeout ou dependências síncronas em cadeia.
- **Dados:** alterações de schema que quebram outros consumidores.
- **Publicação:** mudanças incompatíveis entre versões das aplicações.

Containers ou processos separados ajudam no isolamento, mas, se estiverem no mesmo servidor, uma falha desse servidor ainda pode atingir todos. Não é obrigatório ter um servidor físico por aplicação.

O Hub deve tratar módulos indisponíveis sem deixar de apresentar os demais. Uma falha na Cantina não deveria impedir a navegação para Horários, desde que não exista dependência obrigatória entre eles.

## Banco de dados e rollback

- Definir quem é responsável por cada conjunto de dados.
- Evitar que todos os módulos escrevam diretamente nas tabelas dos outros sem um contrato explícito.
- Preferir mudanças de schema compatíveis durante transições de versão.
- Planejar backup, restauração e teste de recuperação.
- Não considerar uma migração destrutiva automaticamente reversível ao republicar código antigo.

## Caminho gradual

1. Inventariar os sistemas e seus processos de publicação.
2. Criar um Hub mínimo e integrar apenas um módulo.
3. Padronizar identidade, contratos, logs e tratamento de falhas.
4. Validar deploy e rollback antes de ampliar a integração.
5. Reavaliar a organização dos repositórios com base no esforço observado.
