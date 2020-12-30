<script>
    // import { onMount } from "svelte";
    import CharityList from "../components/CharityList.svelte";
    import Header from "../components/Header.svelte";
    import Welcome from "../components/Welcome.svelte";
    import Promo from "../components/Promo.svelte";
    import Footer from "../components/Footer.svelte";
    import Loader from "../components/Loader.svelte";
    //import { charities } from "../data/charities";
    let title = "Charity";
    // let charities = [];
    let data = getData();

    async function getData() {
        const res = await fetch(
            "http://charity-api-bwa.herokuapp.com/charities"
        );
        const data = await res.json();

        if (res.ok) {
            return data;
        } else {
            throw new Error(data);
        }
    }
</script>

<Header />
<Welcome />
{#await data}
    <Loader />
{:then charities}
    <CharityList {charities} />
{/await}
<Promo />
<Footer />
