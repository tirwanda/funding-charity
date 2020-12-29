<script>
	import router from "page";
	import Home from "./pages/home.svelte";
	import About from "./pages/About.svelte";
	import Contact from "./pages/Contact.svelte";
	import NotFound from "./pages/NotFound.svelte";

	let page;

	router("/", () => (page = Home));
	router("/about", () => (page = About));
	router("/contact", () => (page = Contact));
	router("/*", () => (page = NotFound));

	router.start();
</script>

<!-- <About /> -->

<svelte:component this={page} />
