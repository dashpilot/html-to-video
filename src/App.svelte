

<div class="wrap mt-5">
  
 
  <div id="stage" class="stage">
  <div id="slide" class:playing="{playing === true}">hello</div>
  </div>

  
  <div class="btn-group w-100">
    
    {#if playing}
    <button class="btn btn-outline-dark" on:click={stop}><Fa icon={faStop} /><span class="caption"> stop</span></button>
 
  {:else}
  <button class="btn btn-outline-dark" on:click={play}><Fa icon={faPlay} /><span class="caption"> play</span></button>
  {/if}
 
  <button class="btn btn-outline-dark" on:click={recordVideo} class:recording="{recording === true}"><Fa icon={faCircle} /><span class="caption"> record video</span></button>
  <button class="btn btn-outline-dark" on:click="{() => showExport = true}"><Fa icon={faDownload} /><span class="caption"> export video</span></button>
  </div>
  
  <div id="result"></div>
  
  
  {#if showExport}
  <ExportVideo bind:frames={frames} bind:showExport={showExport} />
    {/if}

</div>


<script>
import * as htmlToImage from 'html-to-image';
import { toPng, toJpeg, toBlob } from 'html-to-image';
import Fa from 'svelte-fa/src/fa.svelte'
import { faPlay, faStop, faCircle, faDownload } from '@fortawesome/free-solid-svg-icons/index.js'

import ExportVideo from './components/ExportVideo.svelte'

let interval;
let showExport = false;
let frames = [];
let playing = false;
let recording = false;

function play(){
 playing = true;
}

function stop(){
  playing = false;
  if(recording){
    clearInterval(interval);
    recording = false;
  }
}


function grabFrame(){
  var node = document.getElementById('stage');
  
  htmlToImage.toJpeg(node)
    .then(function (dataUrl) {
      //var img = new Image();
      //img.src = dataUrl;
      // document.getElementById('result').appendChild(img);
      frames.push(dataUrl)
    })
    .catch(function (error) {
      console.error('oops, something went wrong!', error);
    });
}

function recordVideo(){
  recording = true;
  play();
  
 interval = setInterval(function() {
   // method to be executed;
   grabFrame();
 }, 100); // 10 fps
 
 setTimeout(()=>{
   recording = false;
   clearInterval(interval);
   stop();
   
 }, 4000)
}

function stopRecording(){
   clearInterval(interval);
}

function log(){
  console.log(frames)
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
      width: 375px;
    }
    .stage{
      width: 375px;
      height: 211px;
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