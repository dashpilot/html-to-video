


<div class="backdrop">
	
	
	
	
	<div class="modal mt-5" style="display: block;">
	  <div class="modal-dialog">
		<div class="modal-content">
		  <div class="modal-header">
			<h5 class="modal-title">Scene Editor</h5>
			<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" on:click="{() => showEditor = false}"></button>
		  </div>
		  <div class="modal-body p-3">
			  
			  
			  <ImageUpload img_width="1920" bind:scenes="{scenes}" bind:index="{index}" bind:showEditor="{showEditor}" />	 
				  
				  <br>
			  
			  
			  <img src="{scenes[index].image}" class="img-fluid mt-3" />
			  
			  
			  <!-- off-canvas capture -->
			  <div id="capture" class="slide" style="background-image: url({scenes[index].image})">
				  
			 
				  
				  
			 <!--
			  <div class="row g-0">
				  
				  <div class="col-8 p-3">
					
					  
						
						
						<div class="row">
							<div class="col-8">
								
								
								<div class="p-4">
								<h4>{scenes[index].title}</h4>
								
								
								<p>{scenes[index].description}</p>
								</div>
								
								
								
							</div>
							<div class="col-4">
								
							
							</div>
						</div>  
						
					  </div>
					
					  
				  </div>
				  <div class="col-4 side">
					  
					  
					  <div class="label">Image</div>
					  
					 
						  
						  
						  
						<div class="label">Title</div>
							
						<input type="text" class="form-control" bind:value="{scenes[index].title}">
						
						
						<div class="label">Description</div>
						<textarea class="form-control" bind:value="{scenes[index].description}"></textarea>
					  
					  
					  
				  </div>
				  -->
			  </div>
		 
			  
			  
			  
		
		  </div>
		
		  <div class="modal-footer">
			  
			  
			  <button class="btn btn-primary float-end" on:click="{capture}">Save</button>
					  

		
		  </div>
		</div>
	  </div>
	</div>
	
	
	
	
	
	
	
	
	<!--

	
		
		<button class="btn btn-outline-danger float-end me-2" on:click="{deleteScene}">Delete</button>
		

-->
	
	
</div>

<script>
import html2canvas from 'html2canvas';
  import ImageUpload from './ImageUpload.svelte'


export let showEditor;
export let scenes;
export let index;




function deleteScene(){
	if(confirm('Are you sure you want to delete this scene?')){
		scenes.splice(index, 1);
		scenes = scenes;
		showEditor = false;
	}
}

	
	
	function capture(){
	html2canvas(document.querySelector("#capture"), {scale: 2}).then(mycanvas => {
		scenes[index].rendered = mycanvas.toDataURL('image/jpeg')
		showEditor = false;
	});
	}
	

	
	
</script>


<style>

.slide{
	position: absolute;
	right: 9999px;
	top: 9999px;
	width: 1280px;
	height: 720px;
	background-size: cover;
	background-color: #999;
}

.slide h4{
	margin-top: 305px;
	color: white;
	margin-bottom: 0;
}

.slide p{
	color: white;
}


.side{
	border-left: 1px solid #DDD;
	padding: 0px 30px 20px 20px;
}

.modal-body{
	padding: 0;
}

textarea{
	height: 100px;
	resize: none;
}
	

	.label{
		font-size: 14px;
		text-transform: uppercase;
		letter-spacing: 0.03em;
		margin-top: 15px;
		margin-bottom: 4px;
	}
	

</style>