<svelte:head>
	<link rel="stylesheet" href="/css/Jogostyle.css">
</svelte:head>

<script>

import VoltarMenu from './CVoltarMenu.svelte'
import { estado } from "./CEstado.js"
import { trocarEstadoDoJogo } from './CEstado.js'


let labi = [
    ['S', 1, 1, 1, 1, 1, 1, 1, 1, 'E'],
    [1, 0, 1, 0, 0, 0, 0, 0, 1, 1],
    [1, 0, 1, 1, 1, 1, 1, 0, 0, 1],
    [1, 0, 0, 0, 0, 0, 1, 1, 0, 1],
    [1, 1, 1, 1, 1, 0, 1, 0, 0, 1],
    [1, 0, 0, 0, 1, 0, 1, 0, 1, 1],
    [1, 0, 1, 0, 1, 0, 1, 0, 0, 1],
    [1, 0, 1, 0, 1, 1, 1, 1, 0, 1],
    [1, 1, 1, 0, 0, 0, 0, 1, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
  ];

  let playerX = 0;
  let playerY = 0;
  let gameEnd = false;
  let bombas = [];

  let timer = 0; //Variavel do tempirizador
  let timerIntervalo;

//Começa a rodar de pois incrementa
  function startTimer() {
    timerIntervalo = setInterval(() => {
      timer++;
    }, 1000 );
  }

  //para e limpa o timer
  function stopTimer(){
    clearInterval(timerIntervalo);
  }

  // Geraçao aleatoria de bombas
  function geraBombas(){
    for (let i=0 ;i <10; i++){
      let x, y;
      do {
        x = Math.floor((Math.random()*10));
        y = Math.floor((Math.random()*10));
      } while (labi[x][y] !== 1 && labi[y][x] !== 'S' && labi[y][x] !== 'E' && bombas.some(bomba => bomba.x === x && bomba.y === y));
      bombas.push({ x, y});

      //Essa é a definiçao pra dar um estilo a mais
      labi[y][x] = 'bomba';
    }
  }

  //Tcheca as colisões com as bombas
  function checaColisaion() {
    for (const bomba of bombas) {
      if (bomba.x === playerX && bomba.y === playerY){
        gameEnd = true;
        stopTimer();
        alert('Você explodiu e perdeu!\nTempo decorrido: ' + timer + ' segundos')
      }
    }
  }

  function movePlayer(event) {
    if (gameEnd) return; //Ve se o jogo teminou

    if (!timerIntervalo) {
      startTimer(); //Começa o o jogador der o primeiro passo
      geraBombas();
    }

    if (event.key === "ArrowRight" && playerX < labi[0].length - 1 && labi[playerY][playerX + 1] !== 0) {
      playerX++;
    } else if (event.key === "ArrowLeft" && playerX > 0 && labi[playerY][playerX - 1] !== 0) {
      playerX--;
    } else if (event.key === "ArrowDown" && playerY < labi.length - 1 && labi[playerY + 1][playerX] !== 0) {
      playerY++;
    } else if (event.key === "ArrowUp" && playerY > 0 && labi[playerY - 1][playerX] !== 0) {
      playerY--;
    }

    if (labi[playerY][playerX] === 'E') {
      gameEnd = true;
      stopTimer();
      alert('Você chegou ao final do labirinto!\nTempo decorrido: ' + timer + ' segundos');
    } else {
      checaColisaion();
    }
  }

</script>
<body>

  <nav id="linha">
    <h2>
      Campo Minado Nível 2
    </h2>
  </nav>

  
  <div on:keydown={movePlayer} tabindex="0">
    <div class="maze">
      {#each labi as row, y}
        {#each row as cell, x}
          <div class="cell"
           class:start={cell === 'S'} 
           class:end={cell === 'E'} 
           class:wall={cell === 0} 
           class:player={x === playerX && y === playerY}
           class:bomba={cell === 'bomba'}>
          </div>
        {/each}
      {/each}
    </div>
  </div>
  <p> Tempo: {timer}</p>
  <VoltarMenu/>
	<div id="ajuda" class='menu' on:click="{() => trocarEstadoDoJogo('ajuda jogo2')}">
	Ajuda
	</div>

	</body>
