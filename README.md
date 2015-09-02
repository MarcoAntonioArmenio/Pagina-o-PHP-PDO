# Pagina-o-PHP-PDO

Paginação com PHP E PDO::::::


<?php
	

	$links   = 2;
	$limite  = 3;
	$pagina  = (isset($_GET["pagina"])) ? (int)$_GET["pagina"] : 1;
	$start   = ($limite * $pagina) - $limite;

	$conexao = new PDO("mysql:host=localhost;dbname=NomeDoBanco","root","");
	$query   = "SELECT * FROM NomeDaTabela ORDER BY id ASC LIMIT  $start , $limite ";
	$stmt    = $conexao->prepare($query);
	$stmt->execute(); ?>

	<center>
			<table border="1">
				<td>Id</td>
				<td>Nome</td>
				<td>idade</td>
				<td>Email</td>
				<td>Alterar</td>
				<td>Excluir</td>

	<?php foreach($stmt as $linha) { ?>

			<tr>
				<td> <?php echo $linha['id'];?></td>
				<td> <?php echo $linha['nome'];?></td>
				<td> <?php echo $linha['idade'];?></td>
				<td> <?php echo $linha['email'];?></td>
				<td> <a href="alterar.php?id=<?php echo $linha['id'];?>">Alterar</td>
				<td> <a href="excluir.php?id=<?php echo $linha['id'];?>">Excluir</td>
			</tr>

 				<?php }?>

			</table>
		    <br><br>


<?php
	$countQuery   = "SELECT id FROM usuarios";
	$stmt         = $conexao->prepare($countQuery);
	$stmt->execute();

	$resultado    = $stmt->rowCount(PDO::FETCH_ASSOC);
	$countPaginas = ceil($resultado/$limite);

		if ($countPaginas > $limite) {
			echo "<a href='?pagina=1'> Primeira Pagina</a>";

			for ($i= $pagina - $links;  $i <= $pagina -1; $i++) { 
				if ($i >= 1) {

					echo "<a href='?pagina=$i'>  $i </a>";
				} }

					echo '<span>' .$pagina. '</span>';


			for ($i= $pagina + 1; $i <= $pagina + $links; $i++) { 
				if ($i <= $countPaginas) {

					echo "<a href='?pagina=$i'>  $i </a>";

		        } }

		           echo "<a href='?pagina=$countPaginas'> Ultima Pagina</a>";
	} ?>


</center>
