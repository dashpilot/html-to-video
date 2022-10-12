
  <Player bind:playing bind:duration>
  
  <div slot="stage" class="stage-inner">
  <div id="slide" class:playing="{playing === true}">{title}</div>
  </div>
  
  <div slot="editor">
	  <div class="label">Title</div>
	<input type="text" class="form-control" bind:value={title}  />
	</div>
  
  </Player>
  
  
  <script>
	import Player from './../components/Player.svelte'
	let playing = true;
	let duration = 4000;
	
	let title = "Hello World"
  </script>
  
  <style>
  
	/* animations */
  
	#slide {
		position: absolute;
		display: inline-block;
		bottom: 10%;
		left: -200px;
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