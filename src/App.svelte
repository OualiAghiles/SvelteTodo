<script>
	let completed
	let newTodo
	let filtredTodos
	let todos= [
		{
			id: Math.floor(Math.random() *100),
			name:"this is a todo",
			completed: false
		},
		{
			id: Math.floor(Math.random()*100),
			name:"this is a todo2",
			completed: true
		}
	]
	$: filtredTodos = todos
	
	function addNewTodo(e) {
		
		if(e.key == 'Enter' && e.target.value !== ''){
			newTodo = 	{
		
			id: Math.floor(Math.random() *100),
			completed: false,
			name: e.target.value,
		}
			todos = [newTodo, ...todos]
			e.target.value = ""
		}
	}
	
	function taskState (e) {
		completed = e.target.value
		let found = todos.find(element => element.id == e.target.id);
		console.log(found)
		found.completed = completed
		}
		/**
		 * 
		 * @param e
		 */
		function deleteTask(e) {
			console.log(e.path[3].dataset.target)
			let t = todos.filter(element => element.id != e.path[3].dataset.target);
			filtredTodos = [...t]
			console.log(filtredTodos)
		}
		function showCompleted() {
			let completeArr = todos.filter(element => element.completed);
			console.log(completeArr)
			filtredTodos =completeArr
		}
		function showRemainig() {
			let remainigArr = todos.filter(element => !element.completed);
			filtredTodos = remainigArr
		}
		function showAll() {
			filtredTodos = todos
		}
</script>
<style>


.checkbox-input {
  font-size: 15px;
  display: flex;
  align-items: center;
}
.checkbox-input input[type=checkbox] {
  width: 0px;
  opacity: 0;
	position: absolute; 
}
.checkbox-input .custom-checkbox {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  width: 20px;
  height: 20px;
  background-color: #0c0d1f;
  border: 1px solid #adaedf;
  margin-right: 8px;
  border-radius: 2px;
}
.checkbox-input .custom-checkbox svg {
  opacity: 0;
  width: 80%;
  height: 80%;
}
.checkbox-input .custom-checkbox svg path {
  fill: white;
}
.checkbox-input input[type=checkbox]:checked ~ .custom-checkbox {
  background-color: #299769;
  border-color: #299769;
}
.checkbox-input input[type=checkbox]:checked ~ .custom-checkbox svg {
  opacity: 1;
}

/**
 * Tooltip Styles
 */

/* Add this attribute to the element that needs a tooltip */
[data-tooltip] {
  position: relative;
  z-index: 2;
  cursor: pointer;
}

/* Hide the tooltip content by default */
[data-tooltip]:before,
[data-tooltip]:after {
  visibility: hidden;
  -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
  filter: progid: DXImageTransform.Microsoft.Alpha(Opacity=0);
  opacity: 0;
  pointer-events: none;
}

/* Position tooltip above the element */
[data-tooltip]:before {
  position: absolute;
  bottom: 150%;
  left: 50%;
  margin-bottom: 5px;
  margin-left: -80px;
  padding: 7px;
  width: 160px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  border-radius: 3px;
  background-color: #000;
  background-color: hsla(0, 0%, 20%, 0.9);
  color: #fff;
  content: attr(data-tooltip);
  text-align: center;
  font-size: 14px;
  line-height: 1.2;
}

/* Triangle hack to make tooltip look like a speech bubble */
[data-tooltip]:after {
  position: absolute;
  bottom: 150%;
  left: 50%;
  margin-left: -5px;
  width: 0;
  border-top: 5px solid #000;
  border-top: 5px solid hsla(0, 0%, 20%, 0.9);
  border-right: 5px solid transparent;
  border-left: 5px solid transparent;
  content: " ";
  font-size: 0;
  line-height: 0;
}

/* Show tooltip content on hover */
[data-tooltip]:hover:before,
[data-tooltip]:hover:after {
  visibility: visible;
  -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=100)";
  filter: progid: DXImageTransform.Microsoft.Alpha(Opacity=100);
  opacity: 1;
}
</style>
<section class="section">
	<main class="container">
		<div class="columns">
			<div class="column is-one-third">
				<article class=" panel is-info">
					<p class="panel-heading is-flex is-justify-content-space-between">
						
						<span>
							<span class="icon">
								<i class="fas fa-clipboard-list"></i>
							</span>
							Todos
						</span>
					</p>
					<p class="panel-tabs">
						<a on:click="{showAll}">All</a>
						<a on:click="{showCompleted}">Done</a>
						<a on:click="{showRemainig}">Remaning</a>
					</p>
					<div class="panel-block">
						<p class="control has-icons-left">
							
							<input
								class="input is-info"
								type="text"
								placeholder="Ajouter une tache"
								on:keyup="{addNewTodo}"
							/>
							
							<span class="icon is-left">
								<i class="fas fa-tasks" aria-hidden="true" />
							</span>
						</p>
					</div>
					{#each filtredTodos as todo}
					<span class="panel-block is-flex is-justify-content-space-between" class:has-background-primary-light={todo.completed} data-target="{todo.id}" >
						

						<label class='checkbox-input' for='{todo.id}'>
							<input type='checkbox' id='{todo.id}' bind:checked={todo.completed} on:change={taskState}/>
							<span class='custom-checkbox'>
								<svg viewBox="0 0 512 512" width="100" title="check">
									<path d="M173.898 439.404l-166.4-166.4c-9.997-9.997-9.997-26.206 0-36.204l36.203-36.204c9.997-9.998 26.207-9.998 36.204 0L192 312.69 432.095 72.596c9.997-9.997 26.207-9.997 36.204 0l36.203 36.204c9.997 9.997 9.997 26.206 0 36.204l-294.4 294.401c-9.998 9.997-26.207 9.997-36.204-.001z" />
								</svg>
								</span>
							<span>
								{todo.name}
							</span>
								
						</label>
						<span class="actions">
							<a href="#"  class="icon" data-tooltip="Edit"><i class="fas fa-edit"></i></a>
							
							<a href="#" class='has-text-danger-dark icon ' on:click="{deleteTask}" data-tooltip="Remove"><i class="fas fa-trash-alt "></i></a>
						</span>

					</span>
					{/each}
					<div class="panel-block">
						<div class="tags are-medium">
							<span class="tag">
						Tasks  {filtredTodos.length} / {todos.length}</span>
						</div>
					</div>
				</article>
			</div>
		</div>
	</main>
</section>
