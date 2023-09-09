<svelte:head>
	<link rel="stylesheet" href="/css/Jogostyle.css">
</svelte:head>

<script>

import {Contagem} from './Logica.js'
import VoltarMenu from './CVoltarMenu.svelte'
import { estado } from "./CEstado.js"
import { trocarEstadoDoJogo } from './CEstado.js'


</script>
<!--modelo do jogo em html-->
<body>

<nav id="linha">
	<h2>
		Campo Minado Nível 2
	</h2>
</nav>

 

<section id="cruzada">
	
</section>


<VoltarMenu/>
	<div id="ajuda" class='menu' on:click="{() => trocarEstadoDoJogo('ajuda jogo2')}">
	Ajuda
	</div>
	</body>
