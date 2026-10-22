# Roteiro de falas — Planifica Hub

**Apresentação:** [[PlanificaHub-Repo-Arquitetura-1.pdf]]  
**Base das falas:** [[Canvas/Proposta em foco.canvas]], especialmente os blocos de propostas, comparação, implementação e avaliação dos pilotos.  
**Duração estimada:** 12 a 15 minutos, dependendo das pausas.  
**Tom:** explicativo e natural, apresentando alternativas em avaliação, não uma integração já implementada.

> [!important] Ajustes no PDF antes de apresentar
> - **Slide 3:** substituir “Monorepo + monolito modular” por **“Multirepo integrado”**. O desenho apresenta repositórios separados e corresponde à proposta A.
> - **Slides 6 e 11:** o PDF usa **Hub + Horários**, enquanto o canvas define **Hub + Cantina** como piloto. Os slides 4 e 8 também usam Cantina. Para manter a apresentação fiel ao canvas e comparar o mesmo fluxo, este roteiro adota **Cantina**. Ajustar os slides 6 e 11 para esse exemplo. Se o grupo preferir Horários, padronizar todos os exemplos e atualizar o planejamento; a escolha ainda precisa ser confirmada.
> - O PDF e o canvas originais não foram alterados.

As seções **Fala sugerida** são o texto para ensaiar. As observações e as indicações visuais são apoio, não precisam ser lidas em voz alta.

## Slide 1 — Planifica Hub: três caminhos de arquitetura

### Fala sugerida

“O objetivo desta apresentação é discutir como integrar Hub, Cantina, Horários e Chatbot, aproveitando o que já existe e entendendo o esforço de cada alternativa.

No canvas, organizei três propostas. A primeira mantém os repositórios separados e integra as aplicações. A segunda reúne o código e transforma os sistemas em uma aplicação única, organizada em módulos. A terceira também reúne o código, mas preserva as aplicações e suas publicações separadas.

As três buscam uma experiência mais integrada para o usuário. O que muda é como o grupo desenvolve, conecta e publica esses sistemas.

Não são três etapas obrigatórias de uma migração. São caminhos alternativos, e a ideia é comparar os ganhos e os cuidados antes de decidir.”

## Slide 2 — Repositório, arquitetura e deploy não são a mesma coisa

### Fala sugerida

“Antes de comparar as propostas, é importante separar três decisões que podem parecer a mesma coisa.

Repositório é onde mantemos o código e seu histórico no Git. Arquitetura é como organizamos as responsabilidades e a comunicação entre as funcionalidades. Deploy é a publicação de uma versão para uso.

Por exemplo: colocar Cantina e Horários no mesmo repositório não faz com que virem uma aplicação só. Eles podem continuar funcionando e sendo publicados separadamente. Esse é justamente o caso da proposta C.

Da mesma forma, oferecer uma entrada central pelo Hub não exige copiar todos os sistemas para dentro dele.

Então, a ideia principal aqui é: monorepo não significa monólito, e uma experiência integrada não depende necessariamente de uma aplicação única.”

## Slide 3 — A: código separado, experiência integrada

**Apoio visual:** apontar primeiro para o Hub, depois para os três repositórios abaixo e, por fim, para as setas.

### Fala sugerida

“Na proposta A, cada caixa representa um repositório Git diferente e uma aplicação separada. Teríamos o repositório do Hub e os repositórios da Cantina, dos Horários e do Chatbot.

O Hub seria o ponto central de acesso, com navegação e, conforme a integração evoluir, informações consolidadas dos outros sistemas. Ele não precisa incorporar o código dessas aplicações.

As setas representam acesso e integração. Não representam importação de código nem uma obrigação de publicar tudo junto.

Cada projeto pode manter suas dependências e seu processo de publicação. Mas a integração de identidade e dados ainda precisa ser construída.

Também é importante não confundir repositórios separados com isolamento de falhas: se os sistemas dependerem do mesmo serviço de identidade ou de consultas aos bancos uns dos outros, continuam existindo dependências operacionais.”

## Slide 4 — A: preservar os projetos e integrar gradualmente

### Fala sugerida

“O principal ganho da proposta A é preservar a organização dos projetos e concentrar o esforço inicial na integração. Também conseguimos controlar o acesso ao código por repositório e manter publicações independentes, desde que os contratos sejam compatíveis.

Em contrapartida, uma mudança que envolva Hub e Cantina pode exigir dois PRs, ou seja, duas solicitações de revisão, além de uma ordem de publicação combinada.

O roteiro do canvas começa com um Hub mínimo, depois integra a identidade e escolhe uma informação autorizada da Cantina para exibir no portal.

Essa consulta pode acontecer por uma API ou por uma leitura controlada do banco. A leitura direta exige cuidado: ela não executa automaticamente as regras de permissão da Cantina.

Por fim, o piloto precisa demonstrar que conseguimos atualizar a Cantina sem republicar o Hub e tratar sua indisponibilidade sem bloquear o restante do portal.”

## Slide 5 — B: um repositório, uma aplicação modular

**Apoio visual:** destacar os dois contornos — repositório e aplicação — e a saída única para publicação.

### Fala sugerida

“Na proposta B, a mudança vai além de reunir o código. Hub, Cantina, Horários e Chatbot passam a fazer parte de uma única aplicação.

O núcleo comum concentra recursos como autenticação, sessão e navegação. Os módulos continuam responsáveis pelas suas regras e pelos seus dados.

Para mostrar uma informação da Cantina, por exemplo, o Hub pode chamar um serviço interno do módulo. Não precisamos criar uma chamada HTTP entre cada funcionalidade que está dentro da mesma aplicação.

Mas essa colaboração precisa ter limites. O Hub não deve acessar livremente as tabelas ou os detalhes internos da Cantina.

Aqui, os cards são módulos, não aplicações com publicação própria. O resultado dos testes e da preparação da versão é uma publicação do conjunto. Essa é a diferença central em relação às outras duas propostas.”

## Slide 6 — B: unificar a aplicação exige adaptação

**Observação para o apresentador:** fala baseada no piloto Hub + Cantina do canvas; ajustar “Hub + Horários” no slide.

### Fala sugerida

“A proposta B facilita centralizar recursos comuns e revisar alterações de vários módulos em um mesmo PR. Só que isso exige uma adaptação maior do que simplesmente mover pastas.

Precisamos verificar as versões de PHP e CodeIgniter, resolver dependências e adaptar rotas, configurações e logins. Ter tecnologias em comum ajuda na análise, mas não comprova que os projetos sejam diretamente compatíveis.

O roteiro é preparar o núcleo, incorporar primeiro Hub e Cantina, definir as interfaces e permissões, testar e publicar o conjunto.

Os bancos atuais podem ser mantidos no início. Uma aplicação única não obriga um banco único.

O ponto de atenção é que uma alteração localizada entra na versão da aplicação completa. Se houver necessidade de voltar uma versão, o rollback normalmente será do conjunto. Por isso, essa proposta faz sentido quando realmente queremos uma aplicação única e aceitamos esse ciclo comum de publicação.”

## Slide 7 — C: código junto, aplicações separadas

**Apoio visual:** apontar o contorno único do repositório e depois as quatro saídas de publicação.

### Fala sugerida

“Na proposta C, reunimos os projetos em um único repositório, mas cada pasta dentro de apps continua sendo uma aplicação separada.

Então, apps/cantina não é outro repositório e também não é um módulo interno como na proposta B. A Cantina mantém sua inicialização, suas dependências e sua configuração.

Um PR pode incluir uma mudança na Cantina e a adaptação correspondente no Hub. Isso ajuda a revisar a mudança como um todo, mas não significa que as duas aplicações precisem ser publicadas ao mesmo tempo.

Cada uma pode gerar seu próprio pacote de publicação, com versão e recuperação próprias. Para isso funcionar, as versões em produção precisam continuar compatíveis.

Na integração entre sistemas, C se aproxima de A: identidade e troca de dados ainda dependem de mecanismos explícitos. Estar no mesmo Git não transforma essa comunicação em uma chamada interna de módulos.”

## Slide 8 — C: centralizar a colaboração sem unificar a execução

### Fala sugerida

“O principal ganho da proposta C é centralizar o trabalho: código, padrões e revisão ficam no mesmo lugar, preservando aplicações e publicações separadas.

O esforço inicial está em importar os projetos com rastreabilidade e adaptar os pipelines, que são as rotinas de testes e preparação da publicação. Cada aplicação mantém suas dependências; não devemos presumir que um único arquivo de configuração na raiz resolva tudo.

Também precisamos identificar quais aplicações uma mudança afeta. Se alterarmos uma biblioteca compartilhada, não basta testar a pasta onde ela está: precisamos validar seus consumidores.

Outro cuidado é o acesso ao código, que geralmente passa a ser para o repositório inteiro. Definir responsáveis por revisão não cria uma restrição de leitura por pasta.

O piloto começa com Hub e Cantina no mesmo Git e precisa comprovar a integração e a publicação independente. Essa proposta atende à prioridade de centralizar a colaboração sem transformar tudo em uma aplicação só.”

## Slide 9 — O que muda entre A, B e C?

### Fala sugerida

“Em vez de ler toda a tabela, eu destacaria duas comparações.

Entre A e C, continuamos com aplicações separadas e possibilidade de publicação por aplicação. A diferença principal é a organização do código: em A, temos vários repositórios; em C, um repositório compartilhado.

Entre B e C, temos o mesmo número de repositórios, mas arquiteturas diferentes. B transforma os sistemas em uma aplicação modular, publicada em conjunto. C mantém as aplicações separadas, mesmo com mudanças revisadas no mesmo PR.

Por isso, o esforço inicial também muda: em A, a prioridade é integrar; em B, adaptar os sistemas para uma aplicação; em C, reorganizar o Git e os pipelines, sem eliminar o trabalho de integração.

Nenhuma alternativa é melhor só pelo nome. A escolha depende do problema que queremos resolver e do que conseguimos manter depois.”

## Slide 10 — Organizar o código é apenas parte da solução

### Fala sugerida

“Independentemente da proposta, quatro assuntos continuam precisando de planejamento.

O primeiro é identidade: como reconhecer o mesmo usuário nos sistemas. Em A e C, o canvas propõe avaliar um provedor comum de identidade; isso ainda não é uma ferramenta escolhida ou configurada.

O segundo é autorização. Saber quem é o usuário não significa permitir qualquer ação. As permissões precisam ser verificadas em cada funcionalidade, inclusive quando a pessoa acessa uma aplicação diretamente, sem passar pelo Hub.

O terceiro é dados: definir quem é responsável por cada informação e como outros sistemas podem consultá-la ou solicitar alterações, sem contornar as regras do responsável.

O quarto é operação: testar falhas, backup, restauração e rollback.

E há um cuidado que vale para todas as opções: voltar o código para uma versão anterior não desfaz automaticamente dados gravados ou alterações no banco.”

## Slide 11 — Começar pequeno: Hub + Cantina

**Observação para o apresentador:** no PDF, o título e o fluxo usam Horários. Esta fala adota Cantina para manter o mesmo piloto do canvas e dos slides 4 e 8.

### Fala sugerida

“Para não decidir apenas pelos diagramas, a proposta do canvas é começar com um piloto de Hub e Cantina.

O fluxo é pequeno: o usuário se autentica, suas permissões são verificadas e uma informação autorizada da Cantina aparece no Hub. A informação exata ainda precisa ser escolhida com os responsáveis.

Usando o mesmo fluxo, conseguimos observar o esforço de adaptação, o código reaproveitado e o comportamento do login e das permissões. Também precisamos testar publicação, recuperação e o que acontece quando uma dependência falha.

Em A, o piloto exercita a integração entre aplicações em repositórios diferentes. Em B, a colaboração interna entre módulos e a publicação conjunta. Em C, a integração com código no mesmo repositório e publicação separada.

Não precisamos construir três soluções completas. Provas curtas, em homologação, podem revelar os principais riscos e apoiar uma decisão com evidências, sem comprometer os sistemas em uso.”

## Slide 12 — Qual prioridade queremos atender?

### Fala sugerida

“Para concluir, a escolha depende da prioridade do grupo.

Se queremos preservar os projetos e seus repositórios, A é a candidata mais alinhada. Se queremos transformar os sistemas em uma aplicação única, B corresponde a esse objetivo, aceitando a adaptação e a publicação conjunta. Se queremos reunir o código sem unificar a execução, C atende a essa direção.

O canvas ainda não comprova uma vencedora. As estruturas são conceituais, e a compatibilidade real dos projetos precisa ser verificada.

Os próximos passos são levantar versões, dependências, logins e publicações atuais; confirmar hospedagem e automação com o professor; e discutir a identidade com os responsáveis. Depois, validar o piloto e registrar a decisão com seus motivos e limitações.

A pergunta final não é apenas onde colocar o código. É qual organização permite integrar os sistemas com segurança e continuar mantendo o projeto de forma viável.”

---

## Apoio para perguntas após a apresentação

### Qual proposta você recomenda?

“Eu condicionaria a recomendação à prioridade do grupo. A preserva também os repositórios; B unifica a aplicação; C centraliza o código mantendo aplicações separadas. Antes de indicar uma vencedora, precisamos conferir os projetos e validar o esforço do piloto.”

### Monorepo resolve o login único?

“Não. Monorepo organiza o código. Em A e C, precisamos integrar a identidade entre as aplicações. Em B, o login pode ser centralizado na aplicação, mas as contas existentes ainda precisam de uma migração planejada.”

### Já foi escolhido como integrar a identidade?

“Não. O canvas sugere avaliar um provedor mantido pelo grupo, como Keycloak, com OpenID Connect, se a infraestrutura comportar. É uma possibilidade técnica, não uma decisão tomada nem uma integração pronta.”

### O Hub pode consultar os bancos diretamente?

“É uma possibilidade para leituras controladas, com autorização, credenciais mínimas e um contrato estável, preferencialmente por uma view. Isso cria dependência da estrutura consultada e não executa automaticamente as regras da aplicação. Para alterar dados, o caminho deve respeitar as operações da aplicação responsável.”

### Precisamos juntar todos os bancos?

“Não. Nenhuma das três propostas exige banco único. O planejamento permite preservar os bancos atuais no início, mantendo responsabilidades claras sobre os dados.”

### Em C, um PR conjunto permite publicar tudo de uma vez com garantia de sucesso?

“Não. A revisão conjunta não torna as publicações uma operação indivisível. Precisamos planejar a ordem e manter os contratos compatíveis com as versões que coexistirem em produção.”

### Repositórios e deploys separados impedem que uma falha afete os outros sistemas?

“Não automaticamente. O impacto também depende da infraestrutura, do serviço de identidade e das integrações de dados. Precisamos testar essas dependências e o tratamento de indisponibilidade.”

### Isso já está implementado?

“Este material apresenta o estudo das alternativas e os roteiros de implementação. Os exemplos de estrutura e integração são conceituais; o canvas registra que os repositórios ainda precisam ser inspecionados para validar compatibilidade e esforço.”

## Resumo para memorizar

- **A:** código separado, experiência integrada.
- **B:** código junto, uma aplicação modular, publicação conjunta.
- **C:** código junto, aplicações separadas, publicações próprias.
- **Nas três:** identidade, permissões, dados e recuperação exigem planejamento.
- **Decisão:** comparar um mesmo fluxo piloto e escolher conforme a prioridade e a capacidade de manutenção.
