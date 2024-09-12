<script lang="ts">
    import '../styles/global.css';
    import '../styles/jogar.css';

    // Função para recarregar a página
    function reloadPage() {
        window.location.reload();
    }

    const palavras = ["SOL", "RIO", "LUA", "NUVEM"];
    const palavrasEncontradas = new Set<string>();

    const geraNumAleatorio = (min: number, max: number) => Math.floor(Math.random() * (max - min + 1)) + min;

    const PadraoAleatorio = (): string[][] => 
        Array.from({ length: 5 }, () => Array.from({ length: 5 }, () => 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'[geraNumAleatorio(0, 25)]));

    let grade = PadraoAleatorio();
    let gradeboolean = Array.from({ length: 5 }, () => Array(5).fill(false));

    function EmbaralhaPalavras() {
        palavras.forEach(palavra => {
            let colocada = false;
            while (!colocada) {
                const direcao = Math.random() < 0.5 ? 'horizontal' : 'vertical';
                const linha = geraNumAleatorio(0, direcao === 'horizontal' ? 4 : 5 - palavra.length);
                const coluna = geraNumAleatorio(0, direcao === 'horizontal' ? 5 - palavra.length : 4);

                if (verificaEspaco(linha, coluna, palavra.length, direcao)) {
                    for (let i = 0; i < palavra.length; i++) {
                        grade[direcao === 'horizontal' ? linha : linha + i][direcao === 'horizontal' ? coluna + i : coluna] = palavra[i];
                        gradeboolean[direcao === 'horizontal' ? linha : linha + i][direcao === 'horizontal' ? coluna + i : coluna] = true;
                    }
                    colocada = true;
                }
            }
        });
    }

    function verificaEspaco(linha: number, coluna: number, comprimento: number, direcao: 'horizontal' | 'vertical') {
        for (let i = 0; i < comprimento; i++) {
            if (gradeboolean[direcao === 'horizontal' ? linha : linha + i][direcao === 'horizontal' ? coluna + i : coluna]) {
                return false;
            }
        }
        return true;
    }

    EmbaralhaPalavras();

    const selectedCells = Array.from({ length: 5 }, () => Array(5).fill(false));

    function handleCellClick(i: number, j: number) {
        selectedCells[i][j] = !selectedCells[i][j];
        checkWordCompletion();
    }

    function checkWordCompletion() {
        palavras.forEach(palavra => {
            for (let i = 0; i < 5; i++) {
                for (let j = 0; j < 5 - palavra.length + 1; j++) {
                    if (verificaSelecao(i, j, palavra, 'horizontal') || verificaSelecao(i, j, palavra, 'vertical')) {
                        palavrasEncontradas.add(palavra);
                        break;
                    }
                }
            }
        });

        if (palavrasEncontradas.size === palavras.length) {
            alert('Parabéns! Você completou o primeiro nível.');
        }
    }

    function verificaSelecao(linha: number, coluna: number, palavra: string, direcao: 'horizontal' | 'vertical') {
        return Array.from({ length: palavra.length }).every((_, i) =>
            selectedCells[direcao === 'horizontal' ? linha : linha + i][direcao === 'horizontal' ? coluna + i : coluna] &&
            grade[direcao === 'horizontal' ? linha : linha + i][direcao === 'horizontal' ? coluna + i : coluna] === palavra[i]
        );
    }
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

<!-- Grade de letras com fundo visível (branco) e realce em verde-claro ao clicar -->
<table class="gradeletras" id="nivel1">
    {#each grade as linha, i}
        <tr>
            {#each linha as celula, j}
                <td class="cell" on:click={() => handleCellClick(i, j)}
                    style="background-color: {selectedCells[i][j] ? 'lightblue' : 'black'}; border: 1px solid black;">
                    {celula}
                </td>
            {/each}
        </tr>
    {/each}
</table>
<br>

<button id="botaoreloadnivel1" on:click={() => reloadPage()}>
    <img class="home" src="https://i.pinimg.com/564x/17/b1/f4/17b1f41cc9188146734a6c657f26bd09.jpg" alt="">
</button>

<a id="botaomenunivel1" href="/">
    <img class="home" src="https://i.pinimg.com/564x/77/eb/49/77eb49298ec7ee7107ca0d8cb98a507e.jpg" alt="">
</a>
