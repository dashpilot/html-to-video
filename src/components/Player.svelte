
<div class="wrap mt-3">

 
  <div id="stage" class="stage">

  	<slot name="stage"></slot>
  
  </div>

  
  <div class="btn-group w-100">
	
	{#if playing}
	  <button class="btn btn-outline-dark w-50" on:click={stop}><Fa icon={faStop} /><span class="caption"> stop</span></button>
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
import { faPlay, faStop, faCircle, faDownload } from '@fortawesome/free-solid-svg-icons/index.js'

import { fade } from 'svelte/transition';


import ExportVideo from './ExportVideo.svelte'


export let playing = false;
export let duration = 5000;
let framerate = 20;


let showExport = false;

function play(){
 playing = true;
}

function stop(){
  playing = false;
}

</script>
