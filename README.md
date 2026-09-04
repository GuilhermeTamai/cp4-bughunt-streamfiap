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

**1. Como a Injeção de Dependência (`@Autowired`) ajuda na arquitetura e reduz o acoplamento?**
No `AluguelController`, a anotação `@Autowired` permite que o Spring instancie e gerencie os repositórios (`UsuarioRepository` e `ConteudoRepository`) automaticamente. Isso elimina a necessidade de o controlador criar instâncias manuais com o operador `new`, desacoplando a camada Web da implementação da camada de dados e facilitando testes unitários e futuras manutenções.

**2. Qual a diferença prática entre usar JDBC puro e Spring Data JPA neste projeto?**
No JDBC puro, seria necessário escrever SQL manualmente (como `SELECT * FROM conteudos WHERE LOWER(categoria) = LOWER(?)`), gerenciar conexões e iterar sobre o `ResultSet` para criar objetos Java. Com Spring Data JPA, basta declarar o método `findByCategoriaIgnoreCase(String categoria)` na interface `ConteudoRepository`, e o framework gera a consulta SQL otimizada e o mapeamento automático para os objetos do domínio.

**3. Por que a `ClassificacaoIndicativaException` exige `throws` e outras exceções não?**
A `ClassificacaoIndicativaException` é uma *Checked Exception* (herda diretamente de `Exception`), forçando o compilador a exigir a assinatura `throws ClassificacaoIndicativaException` no método `alugar` de `Usuario` e no `AluguelController`. Ela representa uma regra de negócio que a aplicação deve explicitamente prever. Já exceções como `ConteudoNaoEncontradoException` são *Unchecked* (herdam de `RuntimeException`), dispensando o `throws` e sendo tratadas diretamente pelo `@RestControllerAdvice`.

**4. Diferencie Overload (Sobrecarga) de Override (Sobrescrita) usando o bug da classe `Serie`.**
O bug da `Serie` ocorria porque existia o método `calcularPrecoAluguel(double desconto)` com parâmetro novo, caracterizando uma Sobrecarga (Overload), o que mantinha o método padrão sem parâmetros executando o preço base de R$ 9,90 da superclasse. A solução foi aplicar Sobrescrita (Override), mantendo a mesma assinatura sem parâmetros com a anotação `@Override`, garantindo a execução dinâmica do cálculo específico de R$ 4,90 por temporada.

**5. Por que as regras de negócio devem ficar nos Models (`Usuario`) e não nos Controllers?**
Regras de negócio como a validação de idade (`idade < c.getClassificacaoEtaria()`) e saldo suficiente pertencem à classe de domínio `Usuario.java` para garantir alta coesão e encapsulamento. Se essa lógica ficasse no `AluguelController`, qualquer outro serviço que tentasse realizar um aluguel precisaria duplicar essas checagens, aumentando o risco de inconsistências no sistema.

**6. Qual a diferença prática entre a classe abstrata `Conteudo` e a interface `Promocionavel`?**
A classe abstrata `Conteudo` atua como uma estrutura base de dados e comportamentos comuns (atributos como `titulo`, `duracaoMinutos`, anotações JPA e o método base `calcularPrecoAluguel()`) herdados por `Filme`, `Serie` e `Documentario`. Já a interface `Promocionavel` funciona puramente como um contrato: ela define o método `aplicarPromocao(double preco)` sem guardar estado, sendo implementada apenas por `Filme` e `Serie` para aplicar 20% de desconto sem forçar o `Documentario` a ter uma regra promocional.
