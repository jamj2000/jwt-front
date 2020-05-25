<script>
  import {  refreshToken }  from "./store.js";
  let usuario = {};
  let mensaje = '';
  let accessToken = '';

  // let URL = "http://localhost:3000";

  let URL = "https://jwt-back.herokuapp.com";


  function registrar () {
    fetch(URL + "/auth/signup", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(usuario)
    })
      .then(res => res.json())
      .then(data => { mensaje = data.msg; setTimeout(() => (mensaje = ''), 3000); })
      .catch(err => console.log(err));

  }

  function obtenerToken() {
    fetch(URL + "/auth/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(usuario)
    })
      .then(res => res.json())
      .then(token =>  { accessToken = token; console.log(accessToken) })
      .catch(err => console.log(err));

    //await login({ jwt_token })
    // console.log(jwt_token);
  }

//   function getClientes() {
//     fetch(URL + "/api/clientes", {
//       method: "GET",
//       headers: {
//         Authorization: "Bearer " + accessToken
//       }
//     })
//       .then(res => {if (res.status == 401)  console.log('Sin permiso'); res.json()} )
//       .then(data => {
//         console.log(data);        
//       })
//       .catch(err => {
//         console.log(err);
//       });
//   }

 async function getClientes() {
    let authorization = "Bearer " + accessToken;
    let res = await fetch(URL + "/api/clientes", {
      method: "GET",
      headers: {
        "Authorization": authorization
      }
    });
    
    if (res.status == 403 || res.status == 401) { console.log('Acceso denegado o sin acceso'); } 
    
    let data = await res.json();   
    if (data) {
      mensaje = JSON.stringify(data, null, 2); 
      setTimeout(() => (mensaje = ''), 5000);  
      console.log(data);
      }
     
  }
</script>


<div>
  <h1>Iniciar sesión</h1>
  <input
    bind:value={usuario.email}
    type="text"
    name="email"
    placeholder="email"
    tabindex="-2" />
  <br />
  <input
    bind:value={usuario.password}
    type="password"
    name="password"
    placeholder="password"
    tabindex="-1" />
  <br />
  <input type="button" value="Registrarme" on:click={registrar} />
  <input type="button" value="Iniciar sesión" on:click={obtenerToken} /><br>

  <input type="button" value="Obtener clientes" on:click={getClientes} />
</div>
<pre id="feedback">{@html mensaje}</pre>
