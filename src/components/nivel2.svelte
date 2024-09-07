<script lang="ts">
	import '../styles/global.css';
	import '../styles/jogar.css';
	
	function reloadPage() {
		window.location.reload();
	}

	function geraNumAleatorio(min:number, max:number){
		min = Math.ceil(min);
		max = Math.floor(max+1);
		return Math.floor(Math.random() * (max - min) + min);
	}
	
	function PadraoAleatorio(): string[][] {
   	  	let alfabeto: string[] = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'];
  	  	let padrao: string[][] = [];

      	for (let linha: number = 0; linha < 10; linha++) {
     		padrao[linha] = [];
      	 	for (let coluna: number = 0; coluna < 10; coluna++) {
            	padrao[linha][coluna] = alfabeto[geraNumAleatorio(0, 25)];
        	}
    	}

   		return padrao;
	}
	let palavras:string[][] = [['lorem'],["ipsum"],["incidunt"],["aliquid"],["magni"],["fuga"],["cupiditate"]]
	let grade:string[][] = PadraoAleatorio()
</script>
<h1>Encontre as Palavras!</h1>

<table class="palavrasnv2">
	{#each palavras as linha, i}
		<tr>
			{#each linha as celula, j}
				<td >{palavras[i][j]}</td>
			{/each}
		</tr>
	{/each}
</table>

<table class="gradeletras" id="nivel2">
    {#each grade as linha, i}
        <tr>
            {#each linha as celula, j}
				<td class="cell">{grade[i][j]}</td>
            {/each}
        </tr>
    {/each}
</table>

<br />

<button id="botaoreloadnivel1" on:click={() => reloadPage()}>
	<img class="home"  src="https://i.pinimg.com/564x/17/b1/f4/17b1f41cc9188146734a6c657f26bd09.jpg" alt="">
</button> 


<a  id="botaomenunivel1" href="/">
	<img class="home" src="https://i.pinimg.com/564x/77/eb/49/77eb49298ec7ee7107ca0d8cb98a507e.jpg" alt="">
</a>

