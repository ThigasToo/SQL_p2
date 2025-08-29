# 📚 Library Management System - Project 2

## 📌 Descrição
Este projeto implementa um **Sistema de Gerenciamento de Biblioteca** utilizando SQL.  
O arquivo `library_sql_p2.sql` contém a criação de tabelas, inserção de dados, relacionamentos com **chaves estrangeiras**, consultas, **procedures** e relatórios para simular o funcionamento de uma biblioteca. Foram utilizados os arquivos csv no repositório - return_status, books, issued_status, branch, employees, members. 

## 🛠️ Estrutura do Arquivo
O script SQL inclui:

1. **Criação de Tabelas**:
   - `branch` → Informações de filiais.
   - `employees` → Funcionários da biblioteca.
   - `books` → Livros disponíveis, categorias e preços de aluguel.
   - `members` → Membros registrados.
   - `issued` → Livros emprestados.
   - `return_status` → Livros devolvidos.

2. **Definição de Relacionamentos**:
   - Uso de **chaves estrangeiras (FKs)** para garantir integridade referencial.

3. **Consultas e Operações**:
   - Inserção de novos livros e membros.
   - Atualização de dados de membros.
   - Exclusão de registros de empréstimos.
   - Recuperação de livros emitidos por funcionários.
   - Agrupamentos e contagens (ex: membros com mais de um livro emitido).

4. **Tarefas de Análise e Relatórios**:
   - Renda total por categoria de livros.
   - Relatório de desempenho de filiais.
   - Membros ativos e novos registros.
   - Identificação de membros com livros em atraso.
   - Listagem de livros não devolvidos.

5. **Procedures e Automação**:
   - `add_return_records` → Registra devolução de livros e atualiza status.
   - `issued_book` → Gerencia emissão de livros verificando disponibilidade.

---

## 📊 Exemplos de Consultas

- **Listar membros com mais de um livro emitido**:
```sql
SELECT issued_emp_id,
       COUNT(issued_id) AS total_book_issued
FROM issued
GROUP BY issued_emp_id
HAVING COUNT(issued_id) > 1;
```

- **Livros não devolvidos**:
```sql
SELECT * 
FROM issued AS ist
LEFT JOIN return_status AS rs
ON rs.issued_id = ist.issued_id
WHERE rs.return_id IS NULL;
```
- **Relatório de desempenho das filiais**:
```sql
CREATE TABLE branch_reports AS
SELECT 
    b.branch_id,
    b.manager_id,
    COUNT(ist.issued_id) AS number_book_issued,
    COUNT(rs.return_id) AS number_of_book_return,
    SUM(bk.rental_price) AS total_revenue
FROM issued AS ist
JOIN employees AS e ON e.emp_id = ist.issued_emp_id
JOIN branch AS b ON e.branch_id = b.branch_id
LEFT JOIN return_status AS rs ON rs.issued_id = ist.issued_id
JOIN books AS bk ON ist.issued_book_isbn = bk.isbn
GROUP BY 1, 2;
```
## 🚀 Como Usar
1. Execute o script library_sql_p2.sql em seu banco de dados (PostgreSQL ou compatível).
2. Certifique-se de criar as tabelas na ordem correta, já que existem dependências por chaves estrangeiras.
3. Rode as consultas e procedures conforme as tarefas (Tasks 1 a 18) para simular as operações do sistema de biblioteca.

## 📈 Insights Possíveis
- Controle de livros disponíveis e alugados.
- Identificação de membros ativos e inadimplentes.
- Receita por categoria de livros.
- Desempenho de cada filial da biblioteca.
- Funcionários mais produtivos no gerenciamento de empréstimos.
