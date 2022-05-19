# projetoVeiculoPython_SQL
Realização de projeto para fixação de conceitos em Python e SQL

### 1 - Tecnologias Utilizadas:
* SQLite
* Python

### 2 - Modelagem de Dados:

![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/imagem/modelagem.png)

### 3 - Tutorial para inserir arquivos no Google Colab
#### 3.1 - Baixar os arquivos
[Listas](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/tree/main/listas)

#### 3.2 - Seguir instruções de inserção
#### 3.2.1 Primeiramente, clicar na pasta, conforme print abaixo
![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/adicionando_listas/1.png)

#### 3.2.2 Posteriormente, clicar para fazer o upload dos arquivos baixados anteriormente
![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/adicionando_listas/2.png)

#### 3.2.3 Selecionar todos os arquivos baixados
![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/adicionando_listas/3.png)

#### 3.2.4 Os arquivos serão carregados no canto esquerdo, conforme o print abaixo
![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/adicionando_listas/4.png)

### 4 - Exemplos de Scripts realizados:
#### *Obs: Todos os dados foram inseridos de forma aleatória, portanto, não possui nenhuma ligação com a realidade*

#### 4.1 - Consultar o nome do cliente, numero da placa, nome do fabricante e nome do modelo dos carros alugados pelos clientes
```
con = cur.execute (''' select cliente.nome, veiculo.numero_placa, fabricante.nome_fabricante, modelo.nome_modelo from cliente
inner join reserva on reserva.id_cliente = cliente.id_cliente
inner join veiculo on veiculo.id_veiculo = reserva.id_veiculo
inner join modelo on modelo.id_modelo = veiculo.id_modelo
inner join fabricante on fabricante.id_fabricante = modelo.id_fabricante;
'''
).fetchall()
```
![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/consultas/consulta1.png)

#### 4.2 - Consultar a quantidade reservada de cada modelo e fabricante de carro
```
con = cur.execute (
''' 
select  fabricante.nome_fabricante, modelo.nome_modelo, count(*) as quantidade_modelo_alugado from cliente
inner join reserva on reserva.id_cliente = cliente.id_cliente
inner join veiculo on veiculo.id_veiculo = reserva.id_veiculo
inner join modelo on modelo.id_modelo = veiculo.id_modelo
inner join fabricante on fabricante.id_fabricante = modelo.id_fabricante
group by modelo.nome_modelo
order by quantidade_modelo_alugado desc;
'''
).fetchall()
```
![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/consultas/consulta2.png)

#### 4.3 - Consultar quais e quantos carros foram devolvidos antes da data prevista de devolução
```
con = cur.execute (
''' 
select  fabricante.nome_fabricante, modelo.nome_modelo, count(*) as quantidade_modelo_alugado from reserva
inner join veiculo on veiculo.id_veiculo = reserva.id_veiculo
inner join modelo on modelo.id_modelo = veiculo.id_modelo
inner join fabricante on fabricante.id_fabricante = modelo.id_fabricante
where data_devolucao < data_prev_devolucao
group by modelo.nome_modelo
order by quantidade_modelo_alugado desc;
'''
).fetchall()
```
![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/consultas/consulta3.png)

#### 4.4 - Consultar a média de idade dos clientes por fabricante de carro
```
vetor = []
for i in fabricantes:
  vetor.append(con)  
  con = cur.execute (
  f'''
  select round(avg("{data_atual}"-cliente.nascimento),0) as media_idade,  fabricante.nome_fabricante from cliente
  inner join reserva on reserva.id_cliente = cliente.id_cliente
  inner join veiculo on veiculo.id_veiculo = reserva.id_veiculo
  inner join modelo on modelo.id_modelo = veiculo.id_modelo
  inner join fabricante on fabricante.id_fabricante = modelo.id_fabricante
  where fabricante.nome_fabricante == "{i}";  
  '''
  ).fetchall()

vetor1= sorted(vetor, reverse = True)  
```
![alt text](https://github.com/GabrielSouza-git/projetoVeiculoPython_SQL/blob/main/consultas/consulta%204.png)
