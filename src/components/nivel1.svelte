<script lang="ts">
	import '../styles/global.css';
	import '../styles/nivel1.css';

	const palavras = ['SOL', 'RIO', 'LUA', 'NUVEM'];

	let palavrasEncontradas = new Set<string>();

	let gradeboolean = Array.from({ length: 5 }, () => Array(5).fill(false));

	let CelulaSelecionada = Array.from({ length: 5 }, () => Array(5).fill(false));
	
	const geraNumAleatorio = (min: number, max: number) => Math.floor(Math.random() * (max - min + 1)) + min;
	
	const PadraoAleatorio = () => Array.from({ length: 5 }, () => Array.from({ length: 5 }, () => 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'[geraNumAleatorio(0, 25)]));

	let grade = PadraoAleatorio();

	function reloadPage() {
		palavrasEncontradas.clear();
		grade = PadraoAleatorio();
		gradeboolean = Array.from({ length: 5 }, () => Array(5).fill(false));
		CelulaSelecionada = Array.from({ length: 5 }, () => Array(5).fill(false));
		EmbaralhaPalavras();
	}

	function EmbaralhaPalavras() {
		palavras.forEach((palavra) => {
			let colocada = false;
			while (!colocada) {
				const direcao = Math.random() < 0.5 ? 'horizontal' : 'vertical';
				const linha = geraNumAleatorio(0, direcao === 'horizontal' ? 4 : 5 - palavra.length);
				const coluna = geraNumAleatorio(0, direcao === 'horizontal' ? 5 - palavra.length : 4);

				if (verificaEspaco(linha, coluna, palavra.length, direcao)) {
					for (let i = 0; i < palavra.length; i++) {
						grade[direcao === 'horizontal' ? linha : linha + i][
							direcao === 'horizontal' ? coluna + i : coluna
						] = palavra[i];
						gradeboolean[direcao === 'horizontal' ? linha : linha + i][
							direcao === 'horizontal' ? coluna + i : coluna
						] = true;
					}
					colocada = true;
				}
			}
		});
	}

	function verificaEspaco(linha: number, coluna: number,comprimento: number, direcao: 'horizontal' | 'vertical') {
		for (let i = 0; i < comprimento; i++) {
			if (gradeboolean[direcao === 'horizontal' ? linha : linha + i][direcao === 'horizontal' ? coluna + i : coluna]) {
				return false;
			}
		}
		return true;
	}

	function CliqueCelula(i: number, j: number) {
		CelulaSelecionada[i][j] = !CelulaSelecionada[i][j];
		ConclusaoDePalavras();
	}

	function ConclusaoDePalavras() {
		palavras.forEach((palavra) => {
			for (let i = 0; i < 5; i++) {
				for (let j = 0; j <= 5 - palavra.length; j++) {
					if (verificaSelecao(i, j, palavra, 'horizontal')) {
						palavrasEncontradas.add(palavra);
						break;
					}
				}
			}

			for (let i = 0; i <= 5 - palavra.length; i++) {
				for (let j = 0; j < 5; j++) {
					if (verificaSelecao(i, j, palavra, 'vertical')) {
						palavrasEncontradas.add(palavra);
						break;
					}
				}
			}
		});

		if (palavrasEncontradas.size === palavras.length) {
			setTimeout(() => {
				alert('Parabéns! Você terminou o nível.');
				reloadPage();
			}, 100);
		}
	}

	function verificaSelecao(linha: number, coluna: number, palavra: string, direcao: 'horizontal' | 'vertical') {
		if (direcao === 'horizontal' && coluna + palavra.length > 5) {
			return false;
		}
		if (direcao === 'vertical' && linha + palavra.length > 5) {
			return false;
		}

		return Array.from({ length: palavra.length }).every(
			(_, i) => CelulaSelecionada[direcao === 'horizontal' ? linha : linha + i]?.[direcao === 'horizontal' ? coluna + i : coluna] && 
				grade[direcao === 'horizontal' ? linha : linha + i]?.[direcao === 'horizontal' ? coluna + i : coluna] === palavra[i]);
	}

	EmbaralhaPalavras();
</script>

<h1 class="titulo" id="tituloNivel1">Encontre as Palavras!</h1>

<div class="flex-grade-palavras">
	<div>
		<button class="btnRecarregar" on:click={() => reloadPage()}>
			<img class="imagem" src="/images/icons8-actualizar-30.png" alt="Recarregar"/>
		</button>
		<div class="flex-botoes">
			<a id="btnVoltar" href="/">
				<img class="imagem" src="/images/icons8-casa-50.png" alt="Menu"/>
			</a>
		</div>
	</div>
	<table class="gradeLetras">
		{#each grade as linha, i}
			<tr>
				{#each linha as celula, j}
					<td	class="celula" on:click={() => CliqueCelula(i, j)} style="background-color: {CelulaSelecionada[i][j]? 'rgba(255, 255, 255, 0.14)': ''};">
						{celula}
					</td>
				{/each}
			</tr>
		{/each}
	</table>
	<div class="palavrasFicha">
		<h3>Palavras a encontrar</h3>
		{#each palavras as palavra}
			<p>{palavra}</p>
		{/each}
	</div>
</div>

