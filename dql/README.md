CREATE TABLE clientes (
id SERIAL PRIMARY KEY,
nome VARCHAR(100),
email VARCHAR(100) UNIQUE
);

CREATE TABLE pet (
id SERIAL PRIMARY KEY,
nome VARCHAR(100),
especie VARCHAR(50),
id_cliente INT,
FOREIGN KEY (id_cliente) REFERENCES clientes(id)
);

CREATE TABLE consultas (
id  SERIAL PRIMARY KEY,
data DATE,
descricao TEXT,
id_pet INT,
FOREIGN KEY (id_pet) REFERENCES pet(id)
);

INSERT INTO clientes (nome, email)
VALUES ('João', 'joao@email.com');

SELECT * FROM clientes
ORDER BY nome ASC;

INSERT INTO pet (nome, especie, id_cliente)
VALUES ('Rex', 'Cachorro', 2);

SELECT * FROM pet;

DELETE FROM pet
WHERE id = 1;

INSERT INTO consultas (data, descricao, id_pet)
VALUES ('2026-04-10', 'Vacinação', 5);

UPDATE consultas
SET data = '2026-05-01',
descricao = 'Nova descrição'
WHERE id = 1;

DELETE FROM clientes
WHERE id = 1;

SELECT 
consultas.id,
clientes.nome  AS cliente,
pet.nome       AS pet,
consultas.data,
consultas.descricao
FROM consultas
INNER JOIN pet ON consultas.id_pet = pet.id
INNER JOIN clientes ON pet.id_cliente = clientes.id
ORDER BY consultas.data ASC;

SELECT * FROM pet;

DELETE FROM pet
WHERE nome = 'Rex' AND especie = 'Cachorro'
AND id NOT IN (
SELECT MIN(id) FROM pet WHERE nome = 'Rex'
);

UPDATE pet
SET id_cliente = 2
WHERE nome = 'Rex' AND especie = 'Cachorro';

SELECT * FROM pet;

SELECT 
pet.nome    AS pet,
pet.especie,
clientes.nome AS dono
FROM pet
INNER JOIN clientes ON pet.id_cliente = clientes.id;

UPDATE pet
SET id_cliente = 2
WHERE id = 1;

SELECT 
pet.nome    AS pet,
pet.especie,
clientes.nome AS dono
FROM pet
INNER JOIN clientes ON pet.id_cliente = clientes.id;
