# SQL_Server_Stored_Procedure
Uso de store procedure no cotidiano é de extrema importância, visto que podemos armazenar as instruções para que possam ser usadas posteriormente.

Realizando a seguinte consulta de forma detalhada, precisarei somente alterar os parametros de entrada para que o retorno dado seja satisfatório, sem a necessidade de reescrever o código.

    Create Procedure Detalhes_pedidos_por_cliente
    @dt1 datetime2,
    @dt2 datetime2,
    @cliente Varchar(50) = '%'
    											   
    as begin
    select	
    		c.NomeCompleto as Nome,
    		Count(P.NumeroPedido) Quantidade_Pedidos,
    		Sum(D.Preco*D.Quantidade) as Preço,
    		d.Desconto As Desconto,
    		P.Frete as Frete,
    		Sum(D.Preco*D.Quantidade) - Convert(decimal(10,2),Sum(Round(d.Preco*D.Quantidade*D.Desconto,2))) as [Valor com desconto],
    		Sum(D.Preco*D.Quantidade) + p.frete as [Valor com Frete],
    		Year(P.DataPedido) as Periodo
    		
    	from TB_PEDIDO P
    	join 
            TB_DETALHE_PEDIDO as D
    	join
            TB_PRODUTO as TP
            on D.ProdutoId = TP.ProdutoId
            on p.NumeroPedido = d.NumeroPedido
    	join T
             B_CLIENTE as C 
    	on P.ClienteId = C.ClienteId
    	WHERE 
             p.DataPedido  between @dt1 and @dt2 and c.NomeCompleto like @cliente  
    	GROUP BY 
             year(P.DataPedido), C.NomeCompleto, d.Desconto, p.Frete
    	end
    	
    exec Detalhes_pedidos_por_cliente4 '1990-01-01', '1999-12-31', 'Around the Horn'

*Resultado* <br>
<img src="https://github.com/matheus-oliveir4/SQL_Server_Stored_Procedure/blob/main/imagem_2025-04-06_153207065.png" alt=" Consultas" width = 800px>
    
