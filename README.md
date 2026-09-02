# Checkpoint 4 — Bug Hunt StreamFIAP

## Integrantes do Grupo
| Nome Completo | RM |
| :--- | :--- |
| Guilherme Vasques Tamai | RM563276 |
| Mirella Mascarenhas | RM562092 |
| Caio Castelão carminato | RM563630|
| Vitor Komura de Freitas | RM563694|
| André Ayello de Nóbrega | RM561754|


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
* **Sintoma:** A busca por categoria retornava sempre lista vazia.
* **Causa Raiz:** Uso do operador `==` para comparar referências de String.
* **Correção:** Alterado para `.equalsIgnoreCase()`.
* **Conceito Envolvido:** Comparação de objetos em Java.

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

### Bug 12: Ausência de Handler para IllegalArgumentException
* **Sintoma:** Exceções de argumentos inválidos retornavam sem estrutura de erro padrão.
* **Causa Raiz:** Não havia tratamento para `IllegalArgumentException` no handler global.
* **Correção:** Adicionado handler com retorno de status HTTP 400.
* **Conceito Envolvido:** Padronização de mensagens de erro.

---

## 🧹 Refatorações de Clean Code (6 Refactors)

### Clean Code 01: Encapsulamento
* **Sintoma:** Atributo `duracaoMinutos` declarado como `public` na classe `Conteudo`.
* **Correção:** Modificado para `private`.
* **Princípio:** Encapsulamento de dados.

### Clean Code 02: Remoção de Código Morto
* **Sintoma:** Método não utilizado (`calcularDescontoAntigo`) e comentários `// TODO:` espalhados.
* **Correção:** Removidos métodos sem uso e comentários desnecessários.
* **Princípio:** Código Limpo (KISS).

### Clean Code 03: Saídas de Console em Métodos de Negócio
* **Sintoma:** `System.out.println` utilizados dentro de métodos do modelo para simular recibos.
* **Correção:** Removidas as chamadas de impressão do console.
* **Princípio:** Separação de Responsabilidades.

### Clean Code 04: Comentários Incoerentes
* **Sintoma:** Comentário afirmando que adicionaria créditos posicionado sobre uma operação de subtração.
* **Correção:** Comentário removido.
* **Princípio:** Clareza e precisão na documentação interna do código.

### Clean Code 05: Persistência Redundante
* **Sintoma:** Chamadas desnecessárias a `conteudoRepository.save()` e assinaturas com `throws` dispensáveis.
* **Correção:** Limpeza do método no controller de aluguel.
* **Princípio:** DRY e simplificação de assinaturas.

### Clean Code 06: Uso de Queries Spring Data JPA
* **Sintoma:** Iteração manual com laço `for` para filtrar itens por categoria.
* **Correção:** Uso direto do método `findByCategoria` exposto pelo repository.
* **Princípio:** Aproveitamento dos recursos da infraestrutura/framework.

---

## 💬 Reflexões Finais

1. **Impacto do Clean Code:** Manter o código limpo reduz o custo de manutenção e evita efeitos colaterais ao adicionar novas regras.
2. **Importância dos Testes:** Testes manuais e automatizados ajudam a garantir que o contrato da API seja cumprido sem quebras involuntárias.
3. **Tratamento Centralizado de Erros:** Unificar o tratamento de exceções desacopla a camada de controlador e melhora a experiência dos clientes da API.