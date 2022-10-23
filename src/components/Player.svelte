
<nav class="navbar navbar-dark bg-dark ps-3">
  
  <div class="row w-100">
	
	<div class="col-6 brand"><a class="navbar-brand">Zinematic</a></div>
	<div class="col-6 text-end"><button class="btn btn-success" on:click="{() => showExport = true}">Export Video</button></div>
  </div>
  
  
</nav>



<div class="wrap mt-4">

 
  <div id="stage" class="stage">

  	<slot name="stage"></slot>
  
  </div>

  
  <div class="btn-group">
	
	{#if playing}
	  <button class="btn btn-outline-dark w-50" on:click={pause}><Fa icon={faPause} /><span class="caption"> &nbsp;pause</span></button>
	{:else}
	  <button class="btn btn-outline-dark w-50" on:click={play}><Fa icon={faPlay} /><span class="caption"> &nbsp;play</span></button>
	{/if}
  
	</div>
	
	<div id="result"></div>
	
	{#if showExport}
	  <div transition:fade>
	  <ExportVideo bind:playing bind:duration bind:framerate bind:showExport />
	  </div>
	{/if}
	
	<div class="editor">
		<slot name="editor"></slot>
	</div>

</div>






<script>

import Fa from 'svelte-fa/src/fa.svelte'
import { faPlay, faPause, faCircle, faDownload } from '@fortawesome/free-solid-svg-icons/index.js'

import { fade } from 'svelte/transition';


import ExportVideo from './ExportVideo.svelte'


export let playing = false;
export let duration = 5000;
let framerate = 10;


let showExport = false;

function play(){
 playing = true;
 carousel.cycle();
}

function pause(){
  playing = false;
  carousel.pause();
}

</script>
