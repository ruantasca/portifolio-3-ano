----------------------------SUBCONSULTA-----------------------------------------------------------------
--1 Nome dos clientes que moram na mesma cidade do Manoel, exceto o próprio Manoel:
<br/>
SELECT nome, idmunicipio FROM cliente
WHERE idmunicipio = (SELECT idmunicipio
FROM cliente WHERE nome = 'Manoel') 
AND nome not like 'Manoel';
<br/>
<br/>
--2 Data e valor dos pedidos cujo valor é menor que a média de todos os pedidos:
<br/>
SELECT data_pedido, valor FROM pedido
WHERE valor < (SELECT AVG(valor) FROM pedido);
<br/>
<br/>
--3 Data, valor, cliente e vendedor dos pedidos que possuem 2 ou mais produtos:
<br/>
select data_pedido,valor, idcliente, idvendedor from pedido
where idpedido IN (SELECT DISTINCT IDPEDIDO FROM PEDIDO_PRODUTO WHERE QUANTIDADE >= 2)
<br/>
<br/>
--4Nome dos clientes que moram na mesma cidade da transportadora BSTransportes:
<br/>
SELECT NOME FROM CLIENTE 
WHERE IDMUNICIPIO IN (SELECT IDMUNICIOPIO FROM TRASPORTADORA 
WHERE NOME = 'BS. TRANSPORTES');
<br/>
<br/>
--5Nome e município dos clientes localizados no mesmo município de qualquer transportadora:
<br/>
SELECT NOME, IDMUNICIPIO FROM CLIENTE
WHERE IDMUNICIPIO IN (SELECT IDMUNICIPIO FROM TRASPORTADORA 
WHERE IDMUNICIPIO IS NOT NULL);
<br/>
<br/>
--6 Atualizar o valor dos pedidos em 5% onde o somatório do valor total dos produtos é maior que a média de todos os pedidos:
<br/>
UPDATE pedido
SET valor = VALOR +(VALOR * 0.05)
WHERE VALOR > (SELECT AVG(VALOR) FROM PEDIDO);
SELECT * FROM PEDIDO
SELECT * FROM PEDIDO_PRODUTO
<br/>
<br/>
--7 Nome do cliente e a quantidade de pedidos feitos por ele:
<br/>
SELECT cliente.nome, 
       (SELECT SUM(pedido_produto.quantidade) 
        FROM pedido, pedido_produto 
        WHERE pedido.idpedido = pedido_produto.idpedido 
        AND pedido.idcliente = cliente.idcliente) AS quantidade_total
FROM cliente;
<br/>
<br/>
--8 . Para revisar, refaça o exercício anterior (número 07) utilizando group by e mostrando somente os clientes que fizeram pelo menos um pedido.
<br/>
SELECT cliente.nome, COUNT(pedido.idpedido) AS quantidade_pedidos
FROM cliente
JOIN pedido ON cliente.idcliente = pedido.idcliente
GROUP BY cliente.nome
HAVING COUNT(pedido.idpedido) > 0;