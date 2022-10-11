 <div class="backdrop">
<div class="modal mt-5" style="display: block;">
  <div class="modal-dialog">
	<div class="modal-content">
	  <div class="modal-header">
		<h5 class="modal-title">Generate Video</h5>
		<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" on:click="{() => showExport = false}"></button>
	  </div>
	  <div class="modal-body">
	
	
		{#if status !== 'done'}
			<div class="text-center" id="videotape">
			<img src="img/videotape.png" class="img-fluid w-50 videotape" />
		</div>
		{/if}
	
			<div id="video-preview" class="ratio ratio-16x9" style="display: none">
			<video id="output-video" controls width="752" height="423"></video>
			</div>
	  
	  </div>
	  <div class="modal-footer">
		  
		  
		  {#if status == 'recording' || status == 'rendering'}
		 <img src="img/loading.png" class="spinner spin" />
		 {/if}
		 
		 {#if status == 'recording' || status == 'rendering'}
		  <span id="message" class="message me-auto">{message}</span>
		  
		  {/if}
		  
		  {#if status == 'recording'}
		  <a class="btn btn-danger" on:click={stopRecording}>Cancel</a>
		  {/if}
		  
		  {#if status == 'rendering'}
			<a class="btn btn-outline-dark disabled" style="border: 1px solid transparent;">@{framerate}fps</a>
		  {/if}
		
		
		  <a class="btn btn-success" id="download" download="myvid.mp4" style="display: none;">Download</a>
		
		  
		  {#if status == 'idle'}
			<button id="start-btn" class="btn btn-primary" on:click={recordVideo}>Generate Video</button> 
		  {/if}
		
	  </div>
	</div>
  </div>
</div>
 </div>


  
  <script>
 
  import * as htmlToImage from 'html-to-image';
  import { toPng, toJpeg, toBlob } from 'html-to-image';
  
	export let showExport;
	export let playing;
	export let duration;
	export let framerate;
	
	let status = "idle";
	let message = "";
	
	let frames = [];
	let interval;
	let timeout;
	
	
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
		
		console.log("duration: "+duration+", framerate: "+framerate)
		
		status = 'recording'
		message = 'Recording...';	
	
		playing = true;
	  	
	 	interval = setInterval(function() {
	   	// method to be executed;
	   	grabFrame();
	 	}, Math.round(1000/framerate)); // fps
	 	
	 	timeout = setTimeout(()=>{
		 	
		 	playing = false;
	   	clearInterval(interval);
	   	image2video();
	   	
	 	}, duration)
	}
	
	function stopRecording(){
		
	   clearInterval(interval);
	   clearTimeout(timeout);
	   playing = false;
	   status = 'idle'
	   frames = [];
	   
	}

	const {
	  createFFmpeg,
	  fetchFile
	} = FFmpeg;
	const ffmpeg = createFFmpeg({
		mainName: 'main',
		corePath: 'https://unpkg.com/@ffmpeg/core-st@0.11.1/dist/ffmpeg-core.js',
	  log: true
	});


	const image2video = async () => {
		
		status = "rendering";
		message = 'Rendering...';
		
		const video = document.getElementById('output-video');
		const video_preview = document.getElementById('video-preview');
		const download = document.getElementById('download');
		download.style.display = 'none';
		
		
	  if(!ffmpeg.isLoaded()){
		  await ffmpeg.load();
	  }
	  
	  var i = 0;
	  for (const frame of frames) {
		  const num = `00${i}`.slice(-3);
		  let img = frame // image.split(',')[1]
		  ffmpeg.FS('writeFile', `tmp.${num}.jpg`, await fetchFile(img));
		  i++;
		  console.log(i)
	  }
	
	  await ffmpeg.run('-framerate', ''+framerate+'', '-pattern_type', 'glob', '-i', '*.jpg', '-c:v', 'libx264', '-pix_fmt', 'yuv420p', 'out.mp4');
	   
	  // -framerate -> set input framerate
	  // -r -> set output framerate
	
	  
	  const data = ffmpeg.FS('readFile', 'out.mp4');
	  var i = 0;
	  for (const frame2 of frames) {
	    const num = `00${i}`.slice(-3);
		ffmpeg.FS('unlink', `tmp.${num}.jpg`);
		i++
	  }

	  video.src = URL.createObjectURL(new Blob([data.buffer], {
		type: 'video/mp4'
	  }));
	  
	  video_preview.style.display = 'block';

	  
	  download.href = URL.createObjectURL(new Blob([data.buffer], {
		  type: 'video/mp4'
		}));
		
	  download.style.display = 'block';
	
	  status = "done";
	  message = 'Done!'
	  
	  frames = [];

	}

	// this doesn't work currently
	function stopRendering(){
		console.log('stopped rendering')
		ffmpeg.exit()
		status = 'idle'
		frames = [];
	}

  </script>
  
  <style>
	
	  .modal-body{
		  background-color: #DFE6EA;
	  }
	  
	  .spinner{
		  width: 25px;
	  }
	  
	  .spin{
		  		
			-webkit-animation:spin 4s linear infinite;
			-moz-animation:spin 4s linear infinite;
			animation:spin 4s linear infinite;		  
				  
				 
	  }
	  
	  @keyframes spin { 
			100% { 
				-webkit-transform: rotate(360deg); 
				transform:rotate(360deg); 
			} 
		}
	 
  </style>