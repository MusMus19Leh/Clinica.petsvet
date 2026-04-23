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

