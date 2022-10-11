

<div class="wrap mt-3">
  
 
  <div id="stage" class="stage">
  <div id="slide" class:playing="{playing === true}">hello</div>
  </div>

  
  <div class="btn-group w-100">
    
    {#if playing}
    <button class="btn btn-outline-dark" on:click={stop}><Fa icon={faStop} /><span class="caption"> stop</span></button>
 
  {:else}
  <button class="btn btn-outline-dark" on:click={play}><Fa icon={faPlay} /><span class="caption"> play</span></button>
  {/if}
 
 
  <button class="btn btn-outline-dark" on:click="{() => showExport = true}"><Fa icon={faDownload} /><span class="caption"> export video</span></button>
  </div>
  
  <div id="result"></div>
  
  
  {#if showExport}
  <ExportVideo bind:playing={playing} bind:duration={duration} bind:showExport={showExport} />
    {/if}

</div>


<script>

import Fa from 'svelte-fa/src/fa.svelte'
import { faPlay, faStop, faCircle, faDownload } from '@fortawesome/free-solid-svg-icons/index.js'

import ExportVideo from './components/ExportVideo.svelte'


let showExport = false;
let playing = false;
let duration = 4000;


function play(){
 playing = true;
}

function stop(){
  playing = false;
}

</script>


<style>
  .wrap{
    width: 720px;
    margin: 20px auto;
  }
  .stage{
    width: 720px;
    height: 405px;
  
    border: 1px solid black;
    position: relative;
    overflow: hidden;
    border-top-left-radius: 4px;
    border-top-right-radius: 4px;
  }
  
  .btn-group .btn{
    border-top: 0;
  }
  
  .btn-group .btn:first-child{
    border-top-left-radius: 0;
  }
  .btn-group .btn:last-child{
    border-top-right-radius: 0;
  }
  
  @media only screen and (max-width: 600px) {
    .wrap{
      width: 360px;
    }
    .stage{
      width: 360px;
      height: 203px;
    }
    
    .caption{
      display: none;
    }
  }
  
  
  /* animations */

  #slide {
      position: absolute;
      bottom: 10%;
      left: -100px;
      padding: 10px;
      padding-left: 20px;
      padding-right: 20px;
      background: #B8392A;
      color: white;
      font-weight: bold;
  }
  
  #slide.playing {
    -webkit-animation: slide 0.5s forwards;
    animation: slide 5s forwards;
  }
  
  @-webkit-keyframes slide {
    100% {
      left: 0;
    }
  }
  
  @keyframes slide {
    100% {
      left: 0;
    }
  }


</style>