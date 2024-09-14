<script lang="ts">

    import '../styles/global.css';
    import '../styles/jogar.css';

    function reloadPage() { // Reinicializa o estado do jogo
        palavrasEncontradas.clear();
        grade = PadraoAleatorio();
        gradeboolean = Array.from({ length: 5 }, () => Array(5).fill(false));
        CelulaSelecionada = Array.from({ length: 5 }, () => Array(5).fill(false));
        EmbaralhaPalavras();
    }

    const palavras = ["SOL", "RIO", "LUA", "NUVEM"]; // palavras do caça palavras
    const palavrasEncontradas = new Set<string>(); // palavras que são encontradas

    const geraNumAleatorio = (min: number, max: number) => Math.floor(Math.random() * (max - min + 1)) + min; // gera um numero aleatorio, entre min e max

    const PadraoAleatorio = (): string[][] => Array.from({ length: 5 }, () => Array.from({ length: 5 }, () => 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'[geraNumAleatorio(0, 25)])); // gera um padrao aleatorio de 5x5

    let grade = PadraoAleatorio();  // grade exibida
    let gradeboolean = Array.from({ length: 5 }, () => Array(5).fill(false)); // grade gabarito

    function EmbaralhaPalavras() { // embaralha as palavras na grade exibida
        palavras.forEach(palavra => {
            let colocada = false;
            while (!colocada) { // defini a posicao das palavras
                const direcao = Math.random() < 0.5 ? 'horizontal' : 'vertical';
                const linha = geraNumAleatorio(0, direcao === 'horizontal' ? 4 : 5 - palavra.length);
                const coluna = geraNumAleatorio(0, direcao === 'horizontal' ? 5 - palavra.length : 4);

                if (verificaEspaco(linha, coluna, palavra.length, direcao)) { // aplica as palavras
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

    let CelulaSelecionada = Array.from({ length: 5 }, () => Array(5).fill(false)); // array boolean 5x5 completos por false

    function CliqueCelula(i: number, j: number) { // indica se uma célula foi selecionada
        CelulaSelecionada[i][j] = !CelulaSelecionada[i][j];
        ConclusaoDePalavras();
    }

    function ConclusaoDePalavras() { // Verifica se marcamos todas as palavras
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
            setTimeout(() => {
                alert('Parabéns! Você terminou o nível. Clique em "Recarregar" para jogar novamente.');
            }, 100);
        }
    }

    function verificaSelecao(linha: number, coluna: number, palavra: string, direcao: 'horizontal' | 'vertical') { // verifica se uma determinada palavra foi encontrada na grade
        return Array.from({ length: palavra.length }).every((_, i) =>
            CelulaSelecionada[direcao === 'horizontal' ? linha : linha + i][direcao === 'horizontal' ? coluna + i : coluna] && // Verifica se a célula na posição calculada foi selecionada
            grade[direcao === 'horizontal' ? linha : linha + i][direcao === 'horizontal' ? coluna + i : coluna] === palavra[i] //  Verifica se o caractere na grade na posição calculada é igual ao caractere correspondente da palavra
        );
    }

</script>

<h1>Encontre as Palavras!</h1>

<div class="game-container">
    <!-- Grade de letras -->
    <div class="grade-container">
        <table class="gradeletras" id="nivel1">
            {#each grade as linha, i}
                <tr>
                    {#each linha as celula, j}
                        <td class="cell" on:click={() => CliqueCelula(i, j)}
                            style="
                                background-color: {CelulaSelecionada[i][j] ? 'gray' : ''}; /* Azul escuro quando selecionado */
                                border: 2px solid white;
                            ">
                            {celula}
                        </td>
                    {/each}
                </tr>
            {/each}
        </table>
    </div>

    <!-- Ficha com as palavras a serem encontradas -->
    <div class="palavras-ficha">
        <h3>Palavras a encontrar</h3>
        <ul>
            {#each palavras as palavra}
                <li>{palavra}</li>
            {/each}
        </ul>
    </div>
</div>
<br>

<!-- Botões de navegação -->
<button id="botaoreloadnivel1" on:click={() => reloadPage()}>
    <img class="home" src="https://i.pinimg.com/564x/17/b1/f4/17b1f41cc9188146734a6c657f26bd09.jpg" alt="Recarregar">
</button>

<a id="botaomenunivel1" href="/">
    <img class="home" src="https://i.pinimg.com/564x/77/eb/49/77eb49298ec7ee7107ca0d8cb98a507e.jpg" alt="Menu">
</a>
