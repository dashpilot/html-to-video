
<div class="wrap mt-3">

 
  <div id="stage" class="stage">

  	<slot name="stage"></slot>
  
  </div>

  
  <div class="btn-group w-100">
	
	{#if playing}
	  <button class="btn btn-outline-dark w-50" on:click={pause}><Fa icon={faPause} /><span class="caption"> pause</span></button>
	{:else}
	  <button class="btn btn-outline-dark w-50" on:click={play}><Fa icon={faPlay} /><span class="caption"> play</span></button>
	{/if}
   
	<button class="btn btn-outline-dark w-50" on:click="{() => showExport = true}"><Fa icon={faDownload} /><span class="caption"> export video</span></button>
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
