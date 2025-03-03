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
