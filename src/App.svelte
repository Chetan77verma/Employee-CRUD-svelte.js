<script>
  import "../node_modules/materialize-css/dist/css/materialize.min.css";
  import "../node_modules/materialize-css/dist/js/materialize.min.js";
  import { Router, Route } from "svelte-routing";
  import About from "./pages/about.svelte";
  import Home from "./pages/home.svelte";
  import Navbar from "./layout/navbar.svelte";
</script>

<style>
</style>

<Router>
  <Navbar />
  <div class="container">
    <Route path="/" component={Home} />
    <Route path="/about" component={About} />
  </div>
</Router>
