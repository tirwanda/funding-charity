<script>
    import { charity, getCharity } from "../stores/data";
    import { params } from "../stores/pages";
    import router from "page";
    import Header from "../components/Header.svelte";
    import Footer from "../components/Footer.svelte";
    import Loader from "../components/Loader.svelte";
    // import { charities } from "../data/charities";

    let amount = 0,
        name,
        email,
        agree = false,
        contribute = 0;

    $: if (charity) {
        contribute = Math.floor((parseInt(amount) / $charity.target) * 100);
    }
    getCharity($params.id);

    async function handleForm(event) {
        agree = false;
        const newData = await getCharity($params.id);
        newData.pledged = newData.pledged + parseInt(amount);

        try {
            const res = await fetch(
                `https://charity-api-bwa.herokuapp.com/charities/${$params.id}`,
                {
                    method: "PUT",
                    headers: {
                        "content-type": "application/json",
                    },
                    body: JSON.stringify(newData),
                }
            );
            const resMid = await fetch(`/.netlify/functions/payment`, {
                method: "POST",
                headers: {
                    "content-type": "application/json",
                },
                body: JSON.stringify({
                    id: $params.id,
                    amount: parseInt(amount),
                    name,
                    email,
                }),
            });
            const midtrans = await resMid.json();
            console.log(midtrans);
            window.location.href = midtrans.url;
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
{#if !$charity}
    <Loader />
{:else}
    <section
        class="xs-banner-inner-section parallax-window"
        style="background-image:url('/assets/images/about-donation-bg.jpg');">
        <div class="xs-black-overlay" />
        <div class="container">
            <div class="color-white xs-inner-banner-content">
                <h2>Donate Now</h2>
                <p>{$charity.title}</p>
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
                                src={$charity.thumbnail}
                                class="img-responsive"
                                alt="Family Images" />
                        </div>
                    </div>
                    <div class="col-lg-6">
                        <div class="xs-donation-form-wraper">
                            <div class="xs-heading xs-mb-30">
                                <h2 class="xs-title">{$charity.title}</h2>
                                <p class="small">
                                    To learn more about make donate charity with
                                    us visit our "<span
                                        class="color-green">Contact us</span>"
                                    site. By calling
                                    <span class="color-green">+44(0) 800 883
                                        8450</span>.
                                </p><span class="xs-separetor v2" />
                                <h5>
                                    Your donation will be contributing
                                    <strong>{contribute} %</strong>
                                    of total current donation
                                </h5>
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
{/if}
<!-- footer section start -->
<Footer />
