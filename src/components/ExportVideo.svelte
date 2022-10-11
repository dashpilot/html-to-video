 <div class="backdrop">
<div class="modal mt-5" style="display: block;">
  <div class="modal-dialog">
	<div class="modal-content">
	  <div class="modal-header">
		<h5 class="modal-title">Generate Video</h5>
		<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" on:click="{() => showExport = false}"></button>
	  </div>
	  <div class="modal-body">
	
			
			
			
			
		
			<div class="text-center" id="videotape">
			<img src="img/videotape.png" class="img-fluid w-50" />
		</div>
		
			
		
	
			<div id="video-preview" class="ratio ratio-16x9" style="display: none">
			<video id="output-video" controls width="752" height="423"></video>
			</div>
			
			
		
		  
	  </div>
	  <div class="modal-footer">
		  
		 
		  <span id="message" class="message me-auto">{message}</span>
		
		 
		  <a class="btn btn-success" id="download" download="myvid.mp4" style="display: none;">Download</a>
	<button id="start-btn" class="btn btn-primary" on:click={recordVideo}>{@html start}</button> 
		
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
	
	let message = "";
	let start = "Generate Video"
	let converting = false;
	let done = false;
	
	let recording = false;
	let frames = [];
	let interval;
	
	
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
		
	message = 'Recording...';	
		
	  playing = true;
	  recording = true;
	  
	 interval = setInterval(function() {
	   // method to be executed;
	   grabFrame();
	 }, 100); // 10 fps
	 
	 setTimeout(()=>{
		 
	   recording = false;
	   playing = false;
	   clearInterval(interval);
	   
	   
	   image2video();
	   
	   
	 }, duration)
	}
	
	function stopRecording(){
	   clearInterval(interval);
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
		
		converting = true;
		
	    const video = document.getElementById('output-video');
		video.style.display = 'none';
		
		const video_preview = document.getElementById('video-preview');
		
		const videotape = document.getElementById('videotape');
		
		const download = document.getElementById('download');
		download.style.display = 'none';
		
		const start_btn = document.getElementById('start-btn');
		
		
		
		
	  start = '<i class="fas fa-spinner fa-spin"></i> &nbsp;Converting...'

	  message = 'Loading images...';
	  
	  if(!ffmpeg.isLoaded()){
		  await ffmpeg.load();
	  }
	  
	  
	  message = 'Rendering. This may take a while...';
	  // ffmpeg.FS('writeFile', 'audio.ogg', await fetchFile('assets/triangle/audio.ogg'));
	  
	  var i = 0;
	  for (const frame of frames) {
		  const num = `00${i}`.slice(-3);
		  let img = frame // image.split(',')[1]
		  ffmpeg.FS('writeFile', `tmp.${num}.jpg`, await fetchFile(img));
		  i++;
		  console.log(i)
	  }

	  
	  message = 'Creating video... This may take a while.';
	  await ffmpeg.run('-framerate', '25', '-pattern_type', 'glob', '-i', '*.jpg', '-c:v', 'libx264', '-pix_fmt', 'yuv420p', 'out.mp4');
	   
	  // -framerate -> set input framerate
	  // -r -> set output framerate
	  
	  
	  //  '-i', 'audio.ogg', '-c:a', 'copy', '-shortest',
	  
	  
	  // '-filter_complex', "scale=-2:2*ih,zoompan=z='min(zoom+0.0015,1.5)':d=125:x='iw/2-(iw/zoom/2)':y='ih/2-(ih/zoom/2)',scale=-2:450",
	  

	  const data = ffmpeg.FS('readFile', 'out.mp4');
	  //ffmpeg.FS('unlink', 'audio.ogg')
	  var i = 0;
	  for (const frame2 of frames) {
	    const num = `00${i}`.slice(-3);
		ffmpeg.FS('unlink', `tmp.${num}.jpg`);
		i++
	  }

	  video.src = URL.createObjectURL(new Blob([data.buffer], {
		type: 'video/mp4'
	  }));
	  video.style.display = 'block';
	  
	  video_preview.style.display = 'block';
	  videotape.style.display = 'none';
	  
	  download.href = URL.createObjectURL(new Blob([data.buffer], {
		  type: 'video/mp4'
		}));
		
		download.style.display = 'block';
		start_btn.style.display = 'none';
		
	  
	  message = 'Done!'
	  start = 'Generate Video'
	  converting = false;
	  done = true;
			  

	}



  </script>
  
  <style>
	
	  .modal-body{
		  background-color: #DFE6EA;
	  }
	  
	 
  </style>