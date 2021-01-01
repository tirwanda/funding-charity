<script>
	import router from "page";
	import { page, params } from "./stores/pages";
	import Home from "./pages/home.svelte";
	import About from "./pages/About.svelte";
	import Contact from "./pages/Contact.svelte";
	import Donation from "./pages/Donation.svelte";
	import Success from "./pages/Success.svelte";
	import Failure from "./pages/Failure.svelte";
	import NotFound from "./pages/NotFound.svelte";
	import { claim_text } from "svelte/internal";

	export let ready;

	router("/", () => ($page = Home));
	router("/about", () => ($page = About));
	router("/contact", () => ($page = Contact));
	router("/error", () => ($page = Failure));
	router(
		"/donation/:id",
		(ctx, next) => {
			$params = ctx.params;
			next();
		},
		() => ($page = Donation)
	);
	router("/success", () => ($page = Success));

	router("/*", () => ($page = NotFound));

	router.start();
</script>

<!-- <About /> -->

<svelte:component this={$page} {ready} />
