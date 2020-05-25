<script>
  import { refreshToken } from "./store.js";
  let usuario = {};
  let mensaje = "";
  let accessToken = "";

  // let URL = "http://localhost:3000";

  let URL = "https://jwt-back.herokuapp.com";

  function info(msg, timeout) {
    mensaje = msg;
    setTimeout(() => (mensaje = ""), timeout);
  }


  async function registrar() {
    let res = await fetch(URL + "/auth/signup", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(usuario)
    });

    switch (res.status) {
      case 500:
        mensaje = "Error. El usuario no pudo registrarse.";
        setTimeout(() => (mensaje = ""), 4000);
        break;  
      case 401:
        mensaje = "Ya hay un usuario registrado con este email.";
        setTimeout(() => (mensaje = ""), 4000);
        break;
      case 201:
      case 200:
        mensaje = "El usuario fue registrado correctamente.";
        info(mensaje, 4000);
        break;
      default:
    }
  }

  async function iniciar() {
    let data = {};

    let res = await fetch(URL + "/auth/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(usuario)
    });

    switch (res.status) {
      case 401:
        mensaje = "Inicio de sesión incorrecto.";
        setTimeout(() => (mensaje = ""), 4000);
        break;
      case 201:
      case 200:
        mensaje = "Inicio de sesión correcto.";
        accessToken = await res.json();
        info(mensaje, 4000);
        break;
      default:
    }
  }

  async function getClientes() {
    let authorization = "Bearer " + accessToken;
    let data = {};

    let res = await fetch(URL + "/api/clientes", {
      method: "GET",
      headers: {
        Authorization: authorization
      }
    });

    switch (res.status) {
      case 403:
        info("Acceso denegado", 4000);
        break;
      case 401:
        info("No autorizado", 4000);
        break;
      case 201:
      case 200:
        data = await res.json();
        mensaje = JSON.stringify(data, null, 2);
        setTimeout(() => (mensaje = ""), 8000);
        break;
      default:
    }
  }
</script>

<div>
  <h1>Iniciar sesión</h1>
  <input
    bind:value={usuario.email}
    type="text"
    name="email"
    placeholder="email" />
  <br />
  <input
    bind:value={usuario.password}
    type="password"
    name="password"
    placeholder="password" />
  <br />
  <input type="button" value="Registrarme" on:click={registrar} />
  <input type="button" value="Iniciar sesión" on:click={iniciar} />
  <br />

  <input type="button" value="Obtener clientes" on:click={getClientes} />
</div>
<pre id="feedback" style="text-align: left;">
  {@html mensaje}
</pre>
