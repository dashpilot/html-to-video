 
   <Player bind:playing bind:duration>
   
	   <div slot="stage" class="stage-inner">
 
 
 <div id="myCarousel" class="carousel slide" data-bs-ride="carousel">
   <div class="carousel-inner">
	 <div class="carousel-item active">
	   <img src="./img/1.jpg" class="d-block w-100" alt="...">
	 </div>
	 <div class="carousel-item">
	   <img src="./img/2.jpg" class="d-block w-100" alt="...">
	 </div>
	 <div class="carousel-item">
	   <img src="./img/3.jpg" class="d-block w-100" alt="...">
	 </div>
   </div>
 </div>


	   </div>
	   
	   
	   
	   <div slot="editor">
		 
	   </div>
	   
	   
	   
   </Player>


<script>
import { onMount } from 'svelte';


import Player from './../components/Player.svelte'
let playing = true;
let duration = 6000;
	
onMount(async () => {
		
	var myCarousel = document.querySelector("#myCarousel");
	var carousel = new bootstrap.Carousel(myCarousel, {interval:2000});
	
	/*
	myCarousel.addEventListener("slide.bs.carousel", function (info) {
  	console.log(info.to);
  	app.activeStep = info.to;
	});
	*/

});
</script>