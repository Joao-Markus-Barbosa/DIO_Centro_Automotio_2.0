# DIO_Centro_Automotio_2.0
# README - Projeto Rede Centro Automotivo

Este projeto consiste em um banco de dados MySQL para gerenciar uma rede de centros automotivos. O banco de dados foi projetado para armazenar informações sobre clientes, veículos, mecânicos, ordens de serviço, pagamentos e centros automotivos. O objetivo é facilitar a gestão de serviços automotivos, desde o cadastro de clientes e veículos até o acompanhamento de ordens de serviço e pagamentos.

## Estrutura do Banco de Dados

O banco de dados é composto por várias tabelas que se relacionam entre si para fornecer uma visão completa do funcionamento da rede de centros automotivos. Abaixo estão as principais tabelas e suas funções:

### Tabelas Principais

1. **`T_AUTOMOTIVE_CENTER`**: Armazena informações sobre os centros automotivos, como nome social, CNPJ, endereço e contato.
2. **`T_CLIENT`**: Contém dados dos clientes, incluindo nome, endereço, contato, tipo (PF ou PJ), CPF ou CNPJ.
3. **`T_VEHICLE`**: Registra os veículos dos clientes, com informações como modelo, placa, nome e quilometragem.
4. **`T_MECHANIC`**: Armazena dados dos mecânicos, como nome, especialidade e endereço.
5. **`T_ORDER_SERVICE`**: Gerencia as ordens de serviço, com detalhes como número da OS, status, descrição do serviço, datas de emissão e conclusão, e valor do serviço.
6. **`T_PAYMENT`**: Registra os pagamentos realizados, com informações sobre o valor, método de pagamento, status e data.

### Tabelas de Relacionamento N:M

1. **`T_CLIENT_AUT_CENTER`**: Relaciona clientes aos centros automotivos.
2. **`T_SERVICE_MECHANIC`**: Relaciona mecânicos às ordens de serviço.
3. **`T_AUT_CENTER_VEHICLE`**: Relaciona veículos aos centros automotivos.
4. **`T_ORDER_SERVICE_VEHICLE`**: Relaciona veículos às ordens de serviço.

---

## Funcionalidades do Projeto

### 1. **Recuperação de Dados Simples**
   - **`SELECT`**: Consultas básicas para recuperar informações de clientes, veículos, mecânicos, etc.
   - Exemplo: Recuperar o nome, tipo de cliente e contato de todos os clientes.
     ```sql
     SELECT client_Name, client_Type, client_Contact
     FROM T_CLIENT;
     ```

### 2. **Filtros com `WHERE`**
   - **`WHERE`**: Filtra registros com base em condições específicas.
   - Exemplo: Recuperar veículos do tipo "Carro".
     ```sql
     SELECT *
     FROM T_VEHICLE
     WHERE vehicle_Model = 'Carro';
     ```

### 3. **Atributos Derivados**
   - **Expressões**: Cria atributos derivados, como valores com desconto ou valores pendentes.
   - Exemplo: Calcular o valor total dos serviços com um desconto de 10%.
     ```sql
     SELECT 
         id_OS,
         os_Value_Service,
         os_Value_Service * 0.9 AS Valor_Com_Desconto
     FROM T_ORDER_SERVICE;
     ```

### 4. **Ordenação de Dados**
   - **`ORDER BY`**: Ordena os resultados com base em uma ou mais colunas.
   - Exemplo: Ordenar veículos pela quilometragem em ordem decrescente.
     ```sql
     SELECT *
     FROM T_VEHICLE
     ORDER BY vehicle_Km DESC;
     ```

### 5. **Filtros com `HAVING`**
   - **`HAVING`**: Filtra grupos de registros com base em condições agregadas.
   - Exemplo: Filtrar métodos de pagamento com valor total pago maior que 500.
     ```sql
     SELECT 
         payment_Method,
         SUM(payment_Amount) AS Total_Pago
     FROM T_PAYMENT
     WHERE payment_Status = 'Pago'
     GROUP BY payment_Method
     HAVING SUM(payment_Amount) > 500;
     ```

### 6. **Junções entre Tabelas**
   - **`JOIN`**: Combina dados de várias tabelas para fornecer uma visão mais complexa.
   - Exemplo: Recuperar os clientes e os veículos associados a eles.
     ```sql
     SELECT 
         C.client_Name,
         V.vehicle_Name,
         V.vehicle_Plate
     FROM T_CLIENT C
     INNER JOIN T_VEHICLE V ON C.id_Client = V.id_Client;
     ```

---

## Inserção de Dados

O projeto também inclui scripts para inserção de dados iniciais nas tabelas, permitindo a população do banco de dados com informações de exemplo. Isso facilita o teste e a validação das consultas e funcionalidades.

### Exemplo de Inserção de Dados

```sql
INSERT INTO T_AUTOMOTIVE_CENTER (aut_Center_Social_Name, aut_Center_CNPJ, aut_Center_Address, aut_Center_Contact)
VALUES 
    ('AutoCenter A', '12345678000101', 'Rua das Oficinas, 123', '11987654321');
