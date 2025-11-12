<script lang="ts">
    import Cart from "./lib/Cart.svelte";
    import Lightbox from "./lib/Lightbox.svelte";
    let products = $state({});
    let qty = $state(0);
    let imgNum = $state(0);
    let showCartBox = $state(false);
    let showLightBox = $state(false);

    const defaultProduct = $state({
        id: 1,
        company: "Sneaker Company",
        title: "Fall Limited Edition Sneakers",
        description: `These low-profile sneakers are your perfect casual wear companion. 
        Featuring a durable rubber outer sole, they'll withstand everything the weather can offer.`,
        old_price: 250,
        discount: 0.5,
        bigImages: Array.from(
            { length: 4 },
            (_, i) => `image-product-${i + 1}.jpg`,
        ),
        smallImages: Array.from(
            { length: 4 },
            (_, i) => `image-product-${i + 1}-thumbnail.jpg`,
        ),
        qty: 0
    });
    let currentProduct = $state(defaultProduct);
    let cart = $state<Record<string, number>>({});

    function addProduct() {
        qty += 1;
        currentProduct.qty += 1;
    }
    function removeProduct() {
        qty -= 1;
        currentProduct.qty -= 1;

    }

    function addToCart(id) {
        cart[id] = qty;
    }

    function showCart() {
        showCartBox = !showCartBox;
    }

    products[1] = defaultProduct;
</script>
{#if showLightBox}
    <Lightbox bind:showLightBox={showLightBox} {currentProduct}/>
{/if}
<header class="row spread">
    <div class="left row g2">
        <img src="logo.svg" alt="logo" class="logo" />
        <nav>
            <ul class="row g2">
                <li><a href=".">Collections</a></li>
                <li><a href=".">Men</a></li>
                <li><a href=".">Women</a></li>
                <li><a href=".">About</a></li>
                <li><a href=".">Contact</a></li>
            </ul>
        </nav>
    </div>
    <div class="right row g2">
        <button onclick={() => showCart()} aria-label="cart button" class="cart-btn">
            <small>{cart[currentProduct.id]}</small>
        </button>
        <img src="image-avatar.png" alt="avatar" class="profile-pic" />
    </div>
    {#if showCartBox}
        <Cart bind:cart={cart} bind:products={products} />
    {/if}
</header>
<main class="row">
    <section>
        <input type="image" src={currentProduct.bigImages[imgNum]} onclick={() => showLightBox = !showLightBox} alt={`${currentProduct.title} Big Image`} name="saveForm" class="main-img" id="saveForm" />
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
        <div class="cart-section row g1">
            <div class="row g1">
                <button
                    onclick={() => removeProduct()}
                    class="remove-product"
                    aria-label="Remove product"
                ></button>
                <p>{currentProduct.qty ? currentProduct.qty : "0" }</p>
                <button
                    onclick={() => addProduct()}
                    class="add-product"
                    aria-label="Add product"
                ></button>
            </div>
            <button class="add-cart" onclick={() => addToCart(currentProduct.id)}>Add to cart</button>
        </div>
    </section>
</main>
