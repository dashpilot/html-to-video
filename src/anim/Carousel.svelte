 
   <Player bind:playing bind:duration>
	   
	  
   
	   <div slot="stage" class="stage-inner">
 
 
 <div id="myCarousel" class="carousel slide">
   <div class="carousel-inner">
	 <div class="carousel-item active">
	   <img src="./img/1.jpg" class="d-block w-100" alt="...">
	   <div class="carousel-caption d-none d-md-block">
		   <h5>First slide label</h5>
		   <p>Some representative placeholder content for the first slide.</p>
		 </div>
	 </div>
	 <div class="carousel-item">
	   <img src="./img/2.jpg" class="d-block w-100" alt="...">
	   <div class="carousel-caption d-none d-md-block">
		   <h5>Second slide label</h5>
		   <p>Some representative placeholder content for the second slide.</p>
		 </div>
	 </div>
	 <div class="carousel-item">
	   <img src="./img/3.jpg" class="d-block w-100" alt="...">
	   <div class="carousel-caption d-none d-md-block">
		  <h5>Third slide label</h5>
		  <p>Some representative placeholder content for the third slide.</p>
		</div>
	 </div>
   </div>
   
 </div>


	   </div>
	   
	   
	  <!-- 
	   <div slot="editor">
		   
	
		 
		   <button class="btn btn-outline-dark mt-2" on:click={goto}>Test</button>
	
		 
	   </div>
	   -->
	   
	   
   </Player>


<script>
import { onMount } from 'svelte';


import Player from './../components/Player.svelte'
let playing = false;
let duration = 3 * 10000;


onMount(async () => {
		
	var myCarousel = document.querySelector("#myCarousel");
	window.carousel = new bootstrap.Carousel(myCarousel, {interval:5000});
	
	/*
	myCarousel.addEventListener("slide.bs.carousel", function (info) {
  	console.log(info.to);
  	app.activeStep = info.to;
	});
	*/
	
	// listen for recording start
	document.body.addEventListener('record', (e) => { 
		
		// transition to first frame with hard cut
		// then slow down transition
		carousel.pause();
		document.querySelectorAll('.carousel-item').forEach((el)=>{
			el.classList.add('hardcut')
		})
		carousel.to(0);
		//carousel.interval(6000)
		document.querySelectorAll('.carousel-item').forEach((el)=>{
			el.classList.remove('hardcut')
			el.classList.add('slomo')
			el.setAttribute('data-bs-interval', 10000);
		})
		carousel.cycle();
		
	 }, false);

});


/*
$: playing && playhead();

function playhead(){
	console.log("status: "+playing)

}
*/



function goto(){
	//carousel.pause();
	//carousel.to(0);
	console.log('ok')
	
	carousel.pause();
	document.querySelectorAll('.carousel-item').forEach((el)=>{
		el.classList.add('hardcut')
	})
	carousel.to(0);
	document.querySelectorAll('.carousel-item').forEach((el)=>{
		el.classList.remove('hardcut')
		el.classList.add('slomo')
		el.setAttribute('data-bs-interval', 10000);
	})

	carousel.cycle();
	
}

</script>

<style>
	.carousel-caption{
		position: absolute;
		top: 280px;
		left: 0;
		bottom: auto;
		right: auto;
		background-color: #9F2923;
		text-align: left;
		padding: 20px;
	}
	
	p{
		margin-bottom: 0;
	}
</style>