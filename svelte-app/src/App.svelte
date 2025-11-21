<script lang="ts">
    import Cart from "./lib/Cart.svelte";
    import Lightbox from "./lib/Lightbox.svelte";
    import Menu from "./lib/Menu.svelte";
    import { onMount } from "svelte";
    import data from "./data/db.json";
    let products = $state(data);
    let qty = $state(0);
    let imgNum = $state(0);
    let showCartBox = $state(false);
    let showLightBox = $state(false);
    let mobileView = $state(false);
    let width = $state(0);
    let showMenu = $state(false);

    // Reactive window size check
    onMount(() => {
        width = window.innerWidth;
        const handleResize = () => {
            width = window.innerWidth;
            if (width < 1024) {
                showLightBox = true;
                mobileView = true;
            } else {
                showLightBox = false;
                mobileView = false;
            }
        };
        // On mount we check the width
        handleResize();
        window.addEventListener("resize", handleResize);

        return () => window.removeEventListener("resize", handleResize);
    });

    data[0].bigImages = Array.from(
        { length: 4 },
        (_, i) => `image-product-${i + 1}.jpg`,
    );
    data[0].smallImages = Array.from(
        { length: 4 },
        (_, i) => `image-product-${i + 1}-thumbnail.jpg`,
    );

    const defaultProduct = $state(data[0]);
    let currentProduct = $state(defaultProduct);
    let cart = $state<Record<string, number>>({});

    function addProduct() {
        qty += 1;
        currentProduct.qty += 1;
        products[0].qty += 1;
    }
    function removeProduct() {
        if (qty > 0) {
            qty -= 1;
            currentProduct.qty -= 1;
            products[0].qty -= 1;
        }
    }

    function addToCart(id) {
        cart[id] = qty;
    }

    function showCart() {
        showCartBox = !showCartBox;
    }
</script>

<!-- We need a lightbox only for desktop display -->
{#if showLightBox && !mobileView}
    <Lightbox bind:showLightBox {currentProduct} />
{/if}
{#if showMenu}
    <div class="overlay"></div>
{/if}
<header class="row spread">
    <div class="left row g2">
        {#if mobileView}
            <nav>
                <button
                    onclick={() => (showMenu = true)}
                    class="dropdown-btn open"
                    aria-label="open dropdown button"
                    type="button"
                ></button>
                {#if showMenu}
                    <div class="dropdown-container">
                        <button
                            onclick={() => (showMenu = false)}
                            class="dropdown-btn close"
                            aria-label="close dropdown button"
                            type="button"
                        ></button>
                        <Menu />
                    </div>
                {/if}
            </nav>
        {/if}

        <img src="logo.svg" alt="logo" class="logo" />
        {#if !mobileView}
            <nav>
                <Menu />
            </nav>
        {/if}
    </div>
    <div class="right row g2">
        <button
            onclick={() => showCart()}
            aria-label="cart button"
            class="cart-btn"
            type="button"
        >
            <small>{cart[currentProduct.id]}</small>
        </button>
        <img src="image-avatar.png" alt="avatar" class="profile-pic" />
    </div>
    {#if showCartBox}
        <Cart bind:cart bind:products />
    {/if}
</header>
<main class={mobileView ? "col mobile" : "row"}>
    <section>
        {#if !mobileView}
            <button
                type="button"
                id="saveForm"
                onclick={() => (showLightBox = !showLightBox)}
            >
                <img
                class="main-img"
                    src={currentProduct.bigImages[imgNum]}
                    alt={`${currentProduct.title} Big Image`}
                />
            </button>
            <div class="row spread small-img-container">
                {#each currentProduct.smallImages as smallImg, i}
                    <img
                        aria-hidden="true"
                        src={smallImg}
                        alt={`${currentProduct.title} Image-${i}`}
                        class:selectedItem={imgNum === i}
                        onclick={() => {
                            imgNum = i;
                        }}
                    />
                {/each}
            </div>
        {:else}
            <Lightbox bind:showLightBox bind:mobileView {currentProduct} />
        {/if}
    </section>
    <section>
        <h2>{currentProduct.company}</h2>
        <h1>{currentProduct.title}</h1>
        <p>{currentProduct.description}</p>
        <div class="price-container row g1">
            <div class="new-price">
                ${(currentProduct.old_price * currentProduct.discount).toFixed(
                    2,
                )}
            </div>
            <div class="discount">{currentProduct.discount * 100}%</div>
        </div>
        <div class="old-price">${currentProduct.old_price.toFixed(2)}</div>
        <div class="cart-section {mobileView ? 'col' : 'row'} g1">
            <div class="row g1">
                <button
                    onclick={() => removeProduct()}
                    class="remove-product"
                    aria-label="Remove product"
                    type="button"
                ></button>
                <p>{currentProduct.qty ? currentProduct.qty : "0"}</p>
                <button
                    onclick={() => addProduct()}
                    class="add-product"
                    aria-label="Add product"
                    type="button"
                ></button>
            </div>
            <button
                class="add-cart"
                onclick={() => addToCart(currentProduct.id)}
                type="button"
                >Add to cart
            </button>
        </div>
    </section>
</main>
