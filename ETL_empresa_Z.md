Era uma vez, em uma empresa chamada Z, que vivia no mundo dos negócios, enfrentava uma grande adversidade: problemas de lentidão em seus relatórios e dashboards. Os analistas de dados da Empresa Z lutavam para realizar análises eficientes e obter insights valiosos a partir dos dados.
A causa raiz dessa lentidão foi identificada: a tabela tbestoque era a origem de dados para todas as análises realizadas na empresa. Essa tabela continha informações importantes sobre produtos, fornecedores e lojas, mas sua estrutura desorganizada e a falta de otimização estavam prejudicando a performance das análises.
Se depararam com um verdadeiro desafio. Os relatórios e dashboards que deveriam fornecer informações rápidas e precisas aos gestores estavam demorando uma eternidade para serem carregados. A equipe sentia que estavam perdendo batalhas importantes no mundo competitivo dos negócios.
Determinados a superar esse obstáculo e alcançar um novo patamar de excelência, a diretoria da Empresa Z decidiu buscar uma consultoria especializada em análise de dados. Eles estão em busca de profissionais capacitados que possam ajudá-los a reverter essa situação, propor insights estratégicos e implementar soluções eficientes. A diretoria está comprometida em investir nos recursos necessários para transformar os desafios atuais em oportunidades de crescimento e sucesso.

https://github.com/felipecalixtos/ETL_empresa_Z.git.

Ciente da necessidade de tratamento e aprimoramento da rotina de extração, transormação, carregamento de dados e otimização da performance do banco de dados a solução sugerida seria a criação de um dataware house. A ferramenta utilizada para o tratamento sera o postgrees (SQL).

Para isso, primeiramente seria carregada a tabela Estoque (tbestoque) do banco de dados e no dataware house criadas as tabelas fato e dimensão:
Fato: Estoque
Dimensão: Fornecedor, loja, produto.

Para a criação das tabelas dimensão foram realizados no banco de dados relacional o seguinte comando:
```sql
CREATE TABLE dim_fornecedor AS
SELECT fornecedor_id, endereco_fornecedor, telefone_fornecedor FROM tbestoque;
```
CREATE TABLE dim_loja AS
SELECT loja_id, nome_loja, endereco_loja, cidade_loja FROM tbestoque;

CREATE TABLE dim_produto AS
SELECT produto_id, nome_produto, categoria_produto, preco_produto FROM tbestoque;

Para a criação da tabela fato foi realizado o seguinte comando no banco de dados relacional:
```sql
CREATE TABLE fato_estoque AS
SELECT produto_id, fornecedor_id, loja_id, quantidade_estoque, data_estoque FROM tbestoque;
```
Por fim, para realizado o relacionamento das tabelas, foram incluidas as chaves primarias e secundárias unindo as constraints das tabelas.
```sql
ALTER TABLE dim_produto ADD PRIMARY KEY (produto_id);
ALTER TABLE fato_estoque ADD FOREIGN KEY (fornecedor_id) REFERENCES dim_fornecedor (fornecedor_id);
ALTER TABLE fato_estoque ADD FOREIGN KEY (loja_id) REFERENCES dim_loja (loja_id);
ALTER TABLE fato_estoque ADD FOREIGN KEY (produto_id) REFERENCES dim_produto (produto_id);
```
