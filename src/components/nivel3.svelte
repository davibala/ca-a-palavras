<script lang="ts">
	import '../styles/global.css';
	import '../styles/jogar.css';
	
	let grade:string[][] = [["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],
							["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O",],]

</script>

<h1>Encontre as Palavras!</h1>

<table class="menupalavra" id="nivel3">
    {#each grade as linha, i}
        <tr>
            {#each linha as celula, j}
				<td class="cell">{grade[i][j]}</td>
            {/each}
        </tr>
    {/each}
</table>

<br />

<a class="botao" id="botaomenunivel3" href="/">Voltar ao Menu</a>
