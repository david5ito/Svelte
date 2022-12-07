<script>
	let empleados=[];
	let activado = true;
	let datosEmpleado={
		id:null,
		nombre:"",
		correo:""
	}

	let mostrarEmpleados=()=>{
		fetch('http://localhost/empleados/')
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{

			empleados = datosRespuesta

			datosEmpleado = {
				id:null,
				nombre:"",
				correo:""
			}
			activado = true;
			console.log(empleados);

		}).catch(console.log)
	}
	
	let agregrarEmpleado=()=>{

		const nuevoEmpleado ={
			id: datosEmpleado.id,
			nombre: datosEmpleado.nombre,
			correo: datosEmpleado.correo
		}

		fetch('http://localhost/empleados/?insertar=1', {
			method: 'POST',
			body:JSON.stringify(nuevoEmpleado)
		})
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
			console.log(datosRespuesta);
			mostrarEmpleados();
		}).catch(console.log)
	}

	let borrarEmpleado = id =>{
		fetch('http://localhost/empleados/?borrar=' + id)
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
				mostrarEmpleados();

		}).catch(console.log)
	}

	let editarEmpleado = empleado =>{
		activado = false;
		datosEmpleado = empleado;
	}

	let actualizarEmpleado = () =>{
		fetch('http://localhost/empleados/?actualizar=' + datosEmpleado.id, {
			method: 'POST',
			body:JSON.stringify(datosEmpleado)
		})
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
			console.log(datosRespuesta);
			mostrarEmpleados();
		}).catch(console.log)
	}

	mostrarEmpleados();

	

</script>

<div class="container pt-5">
	<div class="row">

		<div class="col-md-5">
			<div class="card">
				<div class="card-header">
					Empleados
				</div>

				<div class="card-body">
					<form>

						<div class="mb-3">
						  <label for="" class="form-label">ID</label>
						  <input readonly
						  bind:value={datosEmpleado.id}
						  type="text" class="form-control" name="id" id="id" aria-describedby="helpId" placeholder="">
						</div>

						<div class="mb-3">
						  <label for="" class="form-label">Nombre</label>
						  <input 
						  bind:value={datosEmpleado.nombre}
						  type="text" class="form-control" name="nombre" id="nombre" aria-describedby="helpId" placeholder="Escribe el nombre">
						</div>
						
						<div class="mb-3">
							<label for="" class="form-label">Correo</label>
							<input
							bind:value={datosEmpleado.correo}
							type="email" class="form-control" name="correo" id="correo" aria-describedby="emailHelpId" placeholder="Escribe un correo">
						</div>

						<button type="button" class="btn btn-success py-2"
						on:click|preventDefault={agregrarEmpleado}
						disabled={!activado}
						>Agrerar</button>
					
						<button type="button" class="btn btn-primary py-2"
						on:click|preventDefault={actualizarEmpleado}
						disabled={activado}
						>Actualizar</button>
						
						<button type="button" class="btn btn-primary py-2"
						on:click|preventDefault={mostrarEmpleados}
						>Cancelar</button>
					</form>

				</div>
			</div>
		</div>

		<div class="col-md-7">
			<table class="table">
				<thead>
					<tr>
						<th>Id</th>
						<th>Nombre</th>
						<th>Correo</th>
						<th>Acciones</th>
					</tr>
				</thead>
				<tbody>

				{#each empleados as empleado}
					<tr>
						<td>{empleado.id}</td>
						<td>{empleado.nombre}</td>
						<td>{empleado.correo}</td>
						<td>
							<button type="submit" class="btn btn-warning" 
							on:click={editarEmpleado(empleado)}
							>Editar</button>
							| 
							<button type="submit" class="btn btn-danger"
							on:click={borrarEmpleado(empleado.id)}
							>Borrar</button>	
						</td>
					</tr>
				{/each}

				</tbody>
			</table>
		</div>
		
	</div>
</div>