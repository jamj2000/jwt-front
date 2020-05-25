<script>
  import { setContext } from "svelte";
  import Login from "./Login.svelte";

  const BACKEND = "https://jwt-back.herokuapp.com/api/articulos/";

  setContext("BACKEND", BACKEND);
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <Login />
</main>
