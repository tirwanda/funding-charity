<script>
    import router from "page";
    import Header from "../components/Header.svelte";
    import Footer from "../components/Footer.svelte";
    import Loader from "../components/Loader.svelte";
    // import { charities } from "../data/charities";

    export let params;
    let charity,
        amount,
        name,
        email,
        agree = false;

    let data = getCharity(params.id);

    async function getCharity(id) {
        const res = await fetch(
            `http://charity-api-bwa.herokuapp.com/charities/${id}`
        );
        return res.json();
    }

    async function handleForm(event) {
        charity.pledged += parseInt(amount);
        try {
            const res = await fetch(
                `http://charity-api-bwa.herokuapp.com/charities/${params.id}`,
                {
                    method: "PUT",
                    headers: {
                        "content-type": "application/json",
                    },
                    body: JSON.stringify(charity),
                }
            );
            console.log(res);
            //=================== Redirection ==================
            router.redirect("/success");
        } catch (err) {
            console.log(err);
        }
    }
</script>

<style>
    #xs-input-checkbox {
        display: flex;
        align-items: center;
    }

    #xs-donate-agree {
        width: 20px;
        margin: 0;
    }

    label[for="xs-donate-agree"] {
        margin: 0;
        margin-left: 10px;
    }

    .xs-donation-form-images {
        text-align: center;
    }
</style>

<Header />
<!-- welcome section -->
<!--breadcumb start here-->
{#await data}
    <Loader />
{:then charity}
    <section
        class="xs-banner-inner-section parallax-window"
        style="background-image:url('/assets/images/about-donation-bg.jpg');">
        <div class="xs-black-overlay" />
        <div class="container">
            <div class="color-white xs-inner-banner-content">
                <h2>Donate Now</h2>
                <p>{charity.title}</p>
                <ul class="xs-breadcumb">
                    <li class="badge badge-pill badge-primary">
                        <a href="/" class="color-white">Home /</a>
                        Donate
                    </li>
                </ul>
            </div>
        </div>
    </section>
    <!--breadcumb end here-->
    <!-- End welcome section -->
    <main class="xs-main">
        <!-- donation form section -->
        <section class="xs-section-padding bg-gray">
            <div class="container">
                <div class="row">
                    <div class="col-lg-6">
                        <div class="xs-donation-form-images">
                            <img
                                src={charity.thumbnail}
                                class="img-responsive"
                                alt="Family Images" />
                        </div>
                    </div>
                    <div class="col-lg-6">
                        <div class="xs-donation-form-wraper">
                            <div class="xs-heading xs-mb-30">
                                <h2 class="xs-title">{charity.title}</h2>
                                <p class="small">
                                    To learn more about make donate charity with
                                    us visit our "<span
                                        class="color-green">Contact us</span>"
                                    site. By calling
                                    <span class="color-green">+44(0) 800 883
                                        8450</span>.
                                </p><span class="xs-separetor v2" />
                            </div>
                            <!-- .xs-heading end -->
                            <form
                                on:submit|preventDefault={handleForm}
                                action="#"
                                method="post"
                                id="xs-donation-form"
                                class="xs-donation-form"
                                name="xs-donation-form">
                                <div class="xs-input-group">
                                    <label for="xs-donate-name">Donation Amount
                                        <span
                                            class="color-light-red">**</span></label>
                                    <input
                                        type="text"
                                        name="name"
                                        bind:value={amount}
                                        required="true"
                                        id="xs-donate-amount"
                                        class="form-control"
                                        placeholder="Your donation in Rupiah" />
                                </div>
                                <!-- .xs-input-group END -->
                                <div class="xs-input-group">
                                    <label for="xs-donate-name">
                                        Your Name
                                        <span class="color-light-red">**</span>
                                    </label>
                                    <input
                                        type="text"
                                        name="name"
                                        bind:value={name}
                                        required="true"
                                        id="xs-donate-name"
                                        class="form-control"
                                        placeholder="Your Name" />
                                </div>
                                <div class="xs-input-group">
                                    <label for="xs-donate-email">
                                        Your Email
                                        <span class="color-light-red">**</span>
                                    </label>
                                    <input
                                        type="email"
                                        name="email"
                                        bind:value={email}
                                        required="true"
                                        id="xs-donate-email"
                                        class="form-control"
                                        placeholder="email@example.com" />
                                </div>

                                <div
                                    class="xs-input-group"
                                    id="xs-input-checkbox">
                                    <input
                                        type="checkbox"
                                        name="agree"
                                        required="true"
                                        id="xs-donate-agree"
                                        bind:checked={agree} />
                                    <label for="xs-donate-agree">
                                        Agree
                                        <span class="color-light-red">**</span>
                                    </label>
                                </div>
                                <!-- .xs-input-group END -->
                                <button
                                    type="submit"
                                    disabled={!agree}
                                    class="btn btn-warning"><span
                                        class="badge"><i
                                            class="fa fa-heart" /></span>
                                    Donate now</button>
                            </form>
                            <!-- .xs-donation-form #xs-donation-form END -->
                        </div>
                    </div>
                </div>
                <!-- .row end -->
            </div>
            <!-- .container end -->
        </section>
        <!-- End donation form section -->
    </main>
{/await}
<!-- footer section start -->
<Footer />
