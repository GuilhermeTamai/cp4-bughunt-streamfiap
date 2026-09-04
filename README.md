# Checkpoint 4 — Bug Hunt StreamFIAP

## Identificação

**Grupo:** StreamFIAP

### Integrantes do Grupo
| Nome Completo | RM | Turma |
| :--- | :--- | :--- |
| Guilherme Vasques Tamai | RM563276 | 2CCPG |
| Mirella Mascarenhas | RM562092 | 2CCPG |
| Caio Castelão Carminato | RM563630 | 2CCPG |
| Vitor Komura de Freitas | RM563694 |2CCPG |
| André Ayello de Nóbrega | RM561754 | 2CCPG |

| Campo | Total |
| :--- | :--- |
| **Total de bugs corrigidos** | 12 / 12 |
| **Total de ajustes de Clean Code** | 6 / 6 |

---

## 🛠️ Correções de Bugs (12 Fixes)

### Bug 01: Sombra de Parâmetro no Nome do Usuário
* **Sintoma:** Ao cadastrar um usuário, o nome não era atribuído à instância.
* **Causa Raiz:** O construtor de `Usuario` atribuía `nome = nome;` em vez de referenciar a propriedade do objeto.
* **Correção:** Alterado para `this.nome = nome;`.
* **Conceito Envolvido:** Encapsulamento e escopo de variáveis em OO.

### Bug 02: Lógica Invertida na Verificação de Créditos
* **Sintoma:** O sistema aceitava aluguéis quando o usuário não possuía saldo.
* **Causa Raiz:** O método `temCreditosSuficientes` comparava `preco >= creditos`.
* **Correção:** Ajustado para `this.creditos >= preco`.
* **Conceito Envolvido:** Lógica condicional em Domain Models.

### Bug 03: Ausência de Validação de Disponibilidade no Aluguel
* **Sintoma:** Conteúdos indisponíveis podiam ser alugados normalmente.
* **Causa Raiz:** O método `alugar` em `Usuario` não checava o estado do atributo `disponivel`.
* **Correção:** Incluída a validação `if (!c.isDisponivel())` lançando `ConteudoIndisponivelException`.
* **Conceito Envolvido:** Validação de estado e exceções de negócio.

### Bug 04: Atributos de Herança Não Inicializados em Série
* **Sintoma:** Séries eram salvas sem título, categoria, duração e classificação.
* **Causa Raiz:** O construtor de `Serie` não repassava os parâmetros para a superclasse `Conteudo`.
* **Correção:** Adicionada a chamada `super(...)` no construtor.
* **Conceito Envolvido:** Herança e inicialização de superclasses.

### Bug 05: Sobrescrita Incorreta do Preço de Aluguel em Série
* **Sintoma:** O cálculo de aluguel de séries ignorava a quantidade de temporadas.
* **Causa Raiz:** A classe `Serie` definia `calcularPrecoAluguel(double desconto)` criando uma sobrecarga em vez de sobrescrever o método herdado.
* **Correção:** Removido o parâmetro e adicionada a anotação `@Override`.
* **Conceito Envolvido:** Polimorfismo e Sobrescrita de Métodos.

### Bug 06: Cálculo Invertido na Promoção de Filme
* **Sintoma:** Filmes promocionais ficavam 20% mais caros.
* **Causa Raiz:** O método `aplicarPromocao` multiplicava o valor por `1.2`.
* **Correção:** Alterado o fator multiplicativo para `0.8`.
* **Conceito Envolvido:** Interfaces e regras promocionais.

### Bug 07: Cobrança Indevida em Documentários
* **Sintoma:** Documentários eram cobrados a R$ 9,90.
* **Causa Raiz:** A classe `Documentario` não sobrescrevia `calcularPrecoAluguel()`, herdando a regra padrão da superclasse.
* **Correção:** Sobrescrito o método retornando `0.0`.
* **Conceito Envolvido:** Especialização de comportamento via Herança.

### Bug 08: Bloco Try-Catch Silenciando Exceção de Conteúdo Não Encontrado
* **Sintoma:** Requisição de ID inexistente retornava HTTP 200 OK com corpo vazio.
* **Causa Raiz:** O controller capturava a exceção em um bloco `try-catch` genérico e retornava `null`.
* **Correção:** Removido o `try-catch` para permitir o tratamento global da exceção.
* **Conceito Envolvido:** Fluxo de Exceções em APIs REST.

### Bug 09: Comparação Incorreta de Strings de Categoria
* **Sintoma:** A busca por categoria retornava lista vazia ou falhava ao ignorar diferenças entre maiúsculas e minúsculas.
* **Causa Raiz:** Uso do operador `==` para comparar referências de String.
* **Correção:** Substituído pelo método derivado `findByCategoriaIgnoreCase` do Spring Data JPA.
* **Conceito Envolvido:** Comparação de objetos e Consultas JPA.

### Bug 10: Permissão de Cadastro com Duração Minutos <= 0
* **Sintoma:** O sistema permitia cadastrar conteúdos com duração zerada ou negativa.
* **Causa Raiz:** Ausência de validação de pré-condições nas rotas de cadastro.
* **Correção:** Adicionada checagem `duracaoMinutos <= 0` lançando `IllegalArgumentException`.
* **Conceito Envolvido:** Validação de DTO/Entrada de dados.

### Bug 11: Ausência de Handler para ClassificacaoIndicativaException
* **Sintoma:** Violação de classificação etária gerava HTTP 500.
* **Causa Raiz:** Falta de Mapeamento no `GlobalExceptionHandler`.
* **Correção:** Criado o método `@ExceptionHandler(ClassificacaoIndicativaException.class)` retornando HTTP 403.
* **Conceito Envolvido:** Tratamento global com `@RestControllerAdvice`.

### Bug 12: Ausência de Geração Automática de ID para Usuário
* **Sintoma:** Falha e erro 500 ao tentar cadastrar novos usuários sem especificar um ID manualmente.
* **Causa Raiz:** Ausência da anotação de geração automática de chave primária na entidade `Usuario`.
* **Correção:** Adicionada a anotação `@GeneratedValue(strategy = GenerationType.IDENTITY)` sobre a chave `@Id`.
* **Conceito Envolvido:** Mapeamento ORM e Persistência JPA.

---

## 🧹 Refatorações de Clean Code (6 Refactors)

### Clean Code 01: Encapsulamento
* **Sintoma:** Atributo `duracaoMinutos` declarado como `public` na classe `Conteudo`.
* **Correção:** Modificado para `private` e adaptados os acessos via método getter `getDuracaoMinutos()`.
* **Princípio:** Encapsulamento de dados.

### Clean Code 02: Remoção de Código Morto
* **Sintoma:** Método não utilizado (`calcularDescontoAntigo`) e comentários `// TODO:` espalhados.
* **Correção:** Removidos métodos sem uso e comentários desnecessários.
* **Princípio:** Código Limpo (KISS).

### Clean Code 03: Saídas de Console em Métodos de Negócio
* **Sintoma:** `System.out.println` utilizados dentro de métodos do modelo para simular recibos.
* **Correção:** Removidas as chamadas de impressão do console do método `alugar` da classe `Usuario`.
* **Princípio:** Separação de Responsabilidades.

### Clean Code 04: Comentários Incoerentes
* **Sintoma:** Comentário afirmando que adicionaria créditos posicionado sobre uma operação de subtração.
* **Correção:** Comentário removido da regra de débito na entidade `Usuario`.
* **Princípio:** Clareza e precisão na documentação interna do código.

### Clean Code 05: Correção do Contrato de Aluguel e Persistência
* **Sintoma:** Método de aluguel desrespeitava o contrato REST e não persistia a alteração de disponibilidade no banco de dados.
* **Correção:** Restaurado o endpoint `@PostMapping` com `@RequestParam`, mantida a assinatura `throws` e reincluída a chamada `conteudoRepository.save(conteudo)`.
* **Princípio:** Integridade do Contrato API e Persistência Robusta.

### Clean Code 06: Uso de Queries Derivadas do Spring Data JPA
* **Sintoma:** Iteração manual com laço `for` em memória para filtrar itens por categoria.
* **Correção:** Implementada a assinatura `findByCategoriaIgnoreCase` na interface `ConteudoRepository` delegando a busca ao banco de dados.
* **Princípio:** Aproveitamento dos recursos da infraestrutura e Framework.

---

## 💬 Reflexões Finais (6 Perguntas)

**1. Injeção de dependência (Aula 13)**
Os controladores recebem as interfaces de repositório via `@Autowired` para que o Spring gerencie o ciclo de vida e a instanciação dos Beans. Se tentássemos utilizar um `new ConteudoRepository()`, o Java lançaria um erro de compilação, pois `ConteudoRepository` é uma interface e não pode ser instanciada diretamente. Ao injetar o bean, o Spring Data JPA cria em tempo de execução uma classe proxy concreta que implementa essa interface, gerencia o pool de conexões e gerencia as transações com a base de dados. O `new` comum ignoraria todo esse ecossistema do Spring, resultando em objetos sem contexto de persistência.

**2. JDBC vs Spring Data JPA (Aulas 12 e 13)**
O Spring Data JPA automatiza a abertura/fechamento de conexões, a escrita de SQLs padronizados (CRUD), a conversão de linhas de `ResultSet` em objetos Java (ORM) e a gestão de transações. O JDBC puro/DAO manual ainda resolve melhor cenários de relatórios altamente complexos, *queries* analíticas pesadas (OLAP) ou operações de alteração em lote (*batch updates*) onde o overhead do ORM afeta a performance. O método derivado `findByCategoriaIgnoreCase` funciona sem implementação porque o Spring Data analisa o nome do método em tempo de compilação/inicialização, identifica os termos `findBy`, `Categoria` e `IgnoreCase`, e gera automaticamente a consulta JPQL/SQL equivalente (`WHERE LOWER(c.categoria) = LOWER(?1)`).

**3. Exceções checked vs unchecked (Aula 11)**
A `ClassificacaoIndicativaException` herda diretamente de `Exception` (Checked Exception), o que obriga a ter tratamento explícito (`try-catch`) ou declaração na assinatura (`throws`), sendo usada para regras de negócio previsíveis que o chamador deve tratar. As Unchecked Exceptions herdam de `RuntimeException`, dispensando a declaração na assinatura. O bug ocorria porque a exceção estourava sem tratamento, gerando um erro 500 genérico. A mensagem da regra chegou de forma clara ao cliente da API ao incluir o texto explicativo na instância da exceção em `Usuario.java` e mapeá-la no `@RestControllerAdvice` (`GlobalExceptionHandler`), retornando um JSON estruturado com status HTTP 403 Forbidden.

**4. Sobrescrita vs sobrecarga (Aula 7)**
A sobrescrita (*override*) ocorre quando uma subclasse reescreve um método herdado mantendo exatamente a mesma assinatura (nome, parâmetros e retorno). A sobrecarga (*overload*) ocorre ao criar um método com o mesmo nome, mas com parâmetros diferentes. No bug da classe `Serie`, foi definido o método `calcularPrecoAluguel(double desconto)` com parâmetro, criando uma sobrecarga; ao chamar o método sem parâmetros no aluguel, o sistema executava o preço base de R$ 9,90 da superclasse `Conteudo`. A anotação `@Override` teria impedido o bug porque o compilador apontaria um erro de sintaxe ao notar que não existia um método com aquela assinatura na classe mãe.

**5. Onde blindar o objeto? (Aulas 3, 4 e 13)**
A blindagem deve ocorrer em múltiplos níveis de defesa:
- **Construtores e Setters:** Devem validar invariantes básicas do objeto (ex: garantir que `duracaoMinutos > 0` e inicializar coleções), impedindo a criação de instâncias com estado inválido em memória.
- **Métodos do Model (Negócio):** Devem validar regras de transição de estado (ex: `alugar()` em `Usuario` checando se `creditos >= preco` e se o conteúdo está disponível).
- **Controllers/DTOs:** Devem validar o payload que chega na requisição REST (ex: checar se a duração no JSON do cadastro é <= 0 e retornar erro 400).
Validar só em um lugar não é suficiente porque a validação no Controller protege a API, mas não impede que outra parte do código interno crie um objeto inválido via código Java. Já validar só no Model pode expor erros de infraestrutura não amigáveis para a API.

**6. Abstração e interface (Aulas 8 e 9)**
A classe abstrata `Conteudo` serve para compartilhar estado (atributos como `titulo`, `categoria`, mapeamento JPA) e código comum entre classes correlatas. A interface `Promocionavel` é um contrato comportamental puro que define uma capacidade (`aplicarPromocao`), podendo ser implementada por qualquer classe independente da hierarquia. Se o `Documentario` passasse a ter promoções:
- **Classes/linhas tocadas:** Apenas a declaração de `Documentario` mudaria para adicionar `implements Promocionavel` e a escrita do método `@Override public double aplicarPromocao(double preco)`.
- **Classes/linhas intactas:** `Conteudo`, `Filme`, `Serie`, `Promocionavel` e todos os Controllers e Repositories ficariam 100% intocados.
Isso demonstra que o sistema respeita o Princípio do Aberto/Fechado (Open/Closed Principle do SOLID), permitindo estender funcionalidades sem modificar o código existente.
