<script lang="ts">
	import '../styles/global.css';
	import '../styles/jogar.css';

	// função para recarrega a pagina
	function reloadPage() {
		window.location.reload();
	}

	let palavras:string[][] = [["SOL"],["LUA"],["MAR"],["RIO"],["NUVEM"]]

	// função para gerar numero aleatorio entre min e max
	function geraNumAleatorio(min:number, max:number){
		min = Math.ceil(min);
		max = Math.floor(max+1);
		return Math.floor(Math.random() * (max - min) + min);
	}

	// função para gerar padrão aleatorio	
	function PadraoAleatorio(): string[][] {
   	  	let alfabeto: string[] = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'];
  	  	let padrao: string[][] = [];

      	for (let linha: number = 0; linha < 5; linha++) {
     		padrao[linha] = [];
      	 	for (let coluna: number = 0; coluna < 5; coluna++) {
            	padrao[linha][coluna] = alfabeto[geraNumAleatorio(0, 25)];
        	}
    	}

   		return padrao;
	}
	
	let grade:string[][] = PadraoAleatorio()
	let gradeboolean:boolean[][] = [[false,false,false,false,false,],
 	                                [false,false,false,false,false,],
									[false,false,false,false,false,],
									[false,false,false,false,false,],
									[false,false,false,false,false,],]
	
	function EmbaralhaPalavras(){
		let palavra:string = "SOL"
		let random:number = geraNumAleatorio(1,1)
		let random2:number = geraNumAleatorio(0,2)
		let random3:number = geraNumAleatorio(0,4)
		if(random == 1){
			grade[random3][random2] = palavra[0]
			grade[random3][random2+1] = palavra[1]
			grade[random3][random2+2] = palavra[2]
			gradeboolean[random3][random2] = true
			gradeboolean[random3][random2+1] = true
			gradeboolean[random3][random2+2] = true
		} else{
			grade[0][0] = palavra[0]
			grade[1][0] = palavra[1]
			grade[2][0] = palavra[2]
		}
	}
	EmbaralhaPalavras()

	console.log(gradeboolean)
</script>

<h1>Encontre as Palavras!</h1>

<table class="palavrasnv1">
	{#each palavras as linha, i}
		<tr>
			{#each linha as celula, j}
				<td >{palavras[i][j]}</td>
			{/each}
		</tr>
	{/each}
</table>

<table class="gradeletras" id="nivel1">
    {#each grade as linha, i}
        <tr>
            {#each linha as celula, j}
				<td class="cell">{grade[i][j]}</td>
            {/each}
        </tr>
    {/each}
</table>
<br>

<button id="botaoreloadnivel1" on:click={() => reloadPage()}>
	<img class="home"  src="https://i.pinimg.com/564x/17/b1/f4/17b1f41cc9188146734a6c657f26bd09.jpg" alt="">
</button> 


<a  id="botaomenunivel1" href="/">
	<img class="home" src="https://i.pinimg.com/564x/77/eb/49/77eb49298ec7ee7107ca0d8cb98a507e.jpg" alt="">
</a>
