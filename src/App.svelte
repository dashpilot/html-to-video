

<div class="wrap">
  
  <div id="stage" class="stage">
  <div id="slide">hello</div>
  </div>
  
  <div class="btn-group w-100">
  <button class="btn btn-outline-dark" on:click={grabFrame}>grab frame</button>
  <button class="btn btn-outline-dark" on:click={recordVideo}>record video</button>
  <button class="btn btn-outline-dark" on:click={stopRecording}>stop recording</button>
  <button class="btn btn-outline-dark" on:click={log}>log</button>
  <button class="btn btn-outline-dark" on:click={() => showExport = true}>render video</button>
  </div>
  
  <div id="result"></div>
  
  
  {#if showExport}
  <ExportVideo bind:frames={frames} bind:showExport={showExport} />
    {/if}


  
</div>


<script>
import * as htmlToImage from 'html-to-image';
import { toPng, toJpeg, toBlob } from 'html-to-image';
import ExportVideo from './components/ExportVideo.svelte'

let interval;
let showExport = false;
let frames = [];

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
 interval = setInterval(function() {
   // method to be executed;
   grabFrame();
 }, 100); // 10 fps
 
 setTimeout(()=>{
   clearInterval(interval);
 }, 2000)
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
  
 
  
  #slide {
      position: absolute;
      top: 300px;
      left: -100px;
      padding: 10px;
      background: #B8392A;
      color: white;
      font-weight: bold;
      -webkit-animation: slide 0.5s forwards;
      -webkit-animation-delay: 5s;
      animation: slide 5s forwards;
      animation-delay: 2s;
  }
  
  @-webkit-keyframes slide {
      100% { left: 0; }
  }
  
  @keyframes slide {
      100% { left: 0; }
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

</style>