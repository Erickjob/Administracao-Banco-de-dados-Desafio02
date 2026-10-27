# Questão 1
BEGIN;

INSERT INTO customer (store_id, first_name, last_name, email, address_id, active)
VALUES (1, 'João', 'Silva', 'joao.silva@email.com', 1, 1);

INSERT INTO customer (store_id, first_name, last_name, email, address_id, active)
VALUES (1, NULL, 'Oliveira', 'erro@email.com', 99999, 1);

ROLLBACK;

** Comportamento esperado **
|Terminal A | Terminal B |
|---- | ----- |
|  Begins | Begins  |
| Insere o usuário   | aguardando o terminal A inserir o cliente que esta na memoria local   |
| Aguardando o acesso do terminal B   | Acessa o banco de dados e retorna 0 linhas  |
| Tenta inserir no na tabela costumers, mas gera erro de restrição  | Aguarda o terminal fazer a inserção dos dados, e o terminal mostra erro de violação  |
| Rollback   | O terminal A desfaz todas as alterações pendentes |


# Questão 2

INSERT INTO inventory (film_id, store_id)
VALUES (1, 1);

SAVEPOINT estoque_atualizado;

INSERT INTO inventory (film_id, store_id)
VALUES (99999, 1);

ROLLBACK TO SAVEPOINT estoque_atualizado;

INSERT INTO inventory (film_id, store_id)
VALUES (2, 1);

COMMIT;
