# 🐾Clinica.petsvet
 Sistema de gerenciamento de agendamento veterinário com interface gráfica em Python, conectado ao banco de dados PostgreSQL. Permite cadastro, pet e consultas em agendamento

#📍Titulação
**Aluno:** Seu Nome Completo  
**Disciplina:** Nome da Disciplina  
**Período:** 2026.3

**Instituição:** Nome da Faculdade  

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia     | Versão  | Finalidade                          |
|----------------|---------|-------------------------------------|
| Python         | 3.11    | Linguagem principal da aplicação    |
| PostgreSQL     | 15      | Sistema Gerenciador de Banco de Dados |
| psycopg2       | 2.9.6   | Conexão Python ↔ PostgreSQL         |
| Tkinter        | —       | Interface gráfica (GUI)             |

---

## 📸 Prints da Aplicação

### _Tela de Login
> O usuário insere suas credenciais para acessar o sistema.
<img width="380" height="480" alt="Login — Clínica Vet 23_04_2026 16_03_02" src="https://github.com/user-attachments/assets/f23294a3-29e8-438f-86af-e377a1b1010d" />

---

### _Menu Principal
> Após o login, o menu exibe todas as funcionalidades disponíveis.
<img width="900" height="620" alt="FOP Pet Shop — Clínica Vet 23_04_2026 16_03_38" src="https://github.com/user-attachments/assets/ad8810c9-14fe-4a21-8105-c13d3214d213" />

---

### _Consulta com JOIN
> Relatório gerado a partir de uma consulta SQL com JOIN entre as tabelas `cliente`, `pets` e `consultas`.
<img width="207" height="313" alt="FOP Pet Shop — Clínica Vet 23_04_2026 16_03_38" src="https://github.com/user-attachments/assets/7c9c416e-cc60-461f-9eb8-14db1d16ac87" />

### O join no sql para gerar esse resultado:

```sql
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

```
### 🗂️ Acesso para agendamento clínico:
|  Testes        |  Senhas |
|----------------|---------|
| Usuário(login)        | admin   |
| senha          | 123     |

---
### 🗃️ Vídeo demonstrativo(realizado pela celular)
https://drive.google.com/file/d/1_W-9wgT_GjbXR-TfDSO4ZI5uQixArXIv/view?usp=sharing

---
## 🚀 Instruções de Execução:

### Pré-requisitos

Certifique-se de ter instalado em sua máquina:

- [Python 3.11+](https://www.python.org/downloads/)
- [PostgreSQL 15+](https://www.postgresql.org/download/)

---

### 🐾Autor(a)
*Letícia de Oliveira Soares Leandro — @MusMus19Leh*

*Unifsa  — BAnco de Dados  — Prof.Anderson Costa*


