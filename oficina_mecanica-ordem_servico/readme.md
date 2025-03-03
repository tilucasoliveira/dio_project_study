# Sistema de Controle e Gerenciamento de Ordens de Serviço

Este sistema é desenvolvido para o controle e gerenciamento da execução de ordens de serviço em uma oficina mecânica. O sistema permite que os clientes levem seus veículos para consertos ou revisões periódicas, e as ordens de serviço sejam gerenciadas com facilidade, incluindo a alocação de mecânicos, serviços e peças necessárias.

## Funcionalidades

- Cadastro de clientes, veículos e mecânicos.
- Criação de ordens de serviço (OS) com serviços e peças.
- Cálculo do valor de cada serviço e peça, com base em uma tabela de referência.
- Acompanhamento do status da ordem de serviço (Em andamento, Concluída, etc.).
- Relacionamento entre ordens de serviço, serviços, peças, veículos e mecânicos.

## Estrutura do Banco de Dados

### Tabelas

- **Cliente**: Contém informações sobre os clientes.
- **Veiculo**: Contém informações sobre os veículos dos clientes.
- **Mecanico**: Contém informações sobre os mecânicos da oficina.
- **Ordem_Servico**: Contém as ordens de serviço, com número, data de emissão, status, entre outros.
- **Servico**: Contém os serviços que podem ser executados nas ordens de serviço.
- **Peca**: Contém as peças que podem ser utilizadas nas ordens de serviço.
- **Equipe**: Contém informações sobre as equipes de mecânicos responsáveis pelas ordens de serviço.
- **OS_Servico**: Relaciona as ordens de serviço com os serviços executados.
- **OS_Peca**: Relaciona as ordens de serviço com as peças utilizadas.
- **Mecanico_Equipe**: Relaciona os mecânicos com as equipes.
- **Mecanico_OS**: Relaciona os mecânicos com as ordens de serviço.

### Consultas SQL para Gerenciamento de Ordens de Serviço

#### 1. Criar uma Nova Ordem de Serviço (OS)

```sql
-- Criar uma nova Ordem de Serviço (OS)
INSERT INTO Ordem_Servico (numero, data_emissao, valor_total, status, data_conclusao, id_veiculo, id_equipe)
VALUES
    (12345, '2025-03-03', 1000.00, 'Em andamento', '2025-03-10', 1, 2);
```

#### 2. Adicionar Serviços a uma Ordem de Serviço (OS)
```sql
-- Adicionar um serviço a uma Ordem de Serviço (OS)
INSERT INTO OS_Servico (id_os, id_servico)
VALUES
    (1, 1),  -- OS 1, Serviço 1
    (1, 2);  -- OS 1, Serviço 2
```
#### 3. Adicionar Peças a uma Ordem de Serviço (OS)
```sql


-- Adicionar uma peça a uma Ordem de Serviço (OS)
INSERT INTO OS_Peca (id_os, id_peca)
VALUES
    (1, 1),  -- OS 1, Peça 1
    (1, 3);  -- OS 1, Peça 3
```
#### 4. Consultar Detalhes de uma Ordem de Serviço (OS)
```sql


-- Consultar os detalhes de uma Ordem de Serviço (OS)
SELECT 
    OS.id_os,
    OS.numero,
    OS.data_emissao,
    OS.valor_total,
    OS.status,
    OS.data_conclusao,
    V.placa AS veiculo,
    E.nome_equipe AS equipe,
    S.descricao AS servico,
    P.descricao AS peca
FROM 
    Ordem_Servico OS
JOIN 
    Veiculo V ON OS.id_veiculo = V.id_veiculo
JOIN 
    Equipe E ON OS.id_equipe = E.id_equipe
JOIN 
    OS_Servico OS_S ON OS.id_os = OS_S.id_os
JOIN 
    Servico S ON OS_S.id_servico = S.id_servico
JOIN 
    OS_Peca OS_P ON OS.id_os = OS_P.id_os
JOIN 
    Peca P ON OS_P.id_peca = P.id_peca
WHERE 
    OS.id_os = 1;
```
#### 5. Consultar Todos os Serviços em uma Ordem de Serviço
```sql

-- Consultar todos os serviços de uma Ordem de Serviço (OS)
SELECT 
    S.id_servico, 
    S.descricao, 
    S.valor_unitario
FROM 
    Servico S
JOIN 
    OS_Servico OS_S ON S.id_servico = OS_S.id_servico
WHERE 
    OS_S.id_os = 1;
```
#### 6. Consultar Todas as Peças em uma Ordem de Serviço
```sql

-- Consultar todas as peças de uma Ordem de Serviço (OS)
SELECT 
    P.id_peca, 
    P.descricao, 
    P.valor_unitario
FROM 
    Peca P
JOIN 
    OS_Peca OS_P ON P.id_peca = OS_P.id_peca
WHERE 
    OS_P.id_os = 1;

```
#### 7. Atualizar o Status de uma Ordem de Serviço (OS)
```sql

-- Atualizar o status de uma Ordem de Serviço (OS)
UPDATE Ordem_Servico
SET status = 'Concluído', data_conclusao = '2025-03-10'
WHERE id_os = 1;
```
#### 8. Excluir uma Ordem de Serviço (OS)
```sql

-- Excluir uma Ordem de Serviço (OS)
DELETE FROM OS_Servico WHERE id_os = 1;  -- Excluir os serviços relacionados
DELETE FROM OS_Peca WHERE id_os = 1;     -- Excluir as peças relacionadas
DELETE FROM Ordem_Servico WHERE id_os = 1;  -- Excluir a ordem de serviço
```
#### 9. Consultar Ordens de Serviço por Status
```sql

-- Consultar ordens de serviço pelo status (exemplo: 'Em andamento')
SELECT 
    id_os, 
    numero, 
    data_emissao, 
    valor_total, 
    status, 
    data_conclusao
FROM 
    Ordem_Servico
WHERE 
    status = 'Em andamento';
```
#### 10. Consultar Ordens de Serviço por Cliente
```sql


-- Consultar ordens de serviço de um cliente específico (exemplo: cliente com id_cliente = 1)
SELECT 
    OS.id_os, 
    OS.numero, 
    OS.data_emissao, 
    OS.valor_total, 
    OS.status, 
    OS.data_conclusao
FROM 
    Ordem_Servico OS
JOIN 
    Veiculo V ON OS.id_veiculo = V.id_veiculo
WHERE 
    V.id_cliente = 1;
```
