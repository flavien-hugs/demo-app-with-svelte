<script>
	import 'materialize-css/dist/css/materialize.min.css';

	import { Router, Link, Route } from 'svelte-routing';

	import Home from './pages/Home.svelte';
	import About from './pages/About.svelte';
	import Contact from './pages/Contact.svelte';
	import Navbar from './layouts/Navbar.svelte';
</script>

<Router>
	<Navbar/>
	<div class="container">
		<Route path="/" component={Home}/>
		<Route path="/page/about/" component={About}/>
		<Route path="/page/contact/" component={Contact}/>
	</div>
</Router>

<style>
</style>
