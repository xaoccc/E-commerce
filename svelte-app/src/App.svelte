<script>
    let products = $state([]);
    let qty = $state(0);
    let imgNum = $state(0);
    // To Do: Use JSON
    const defaultProduct = {
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
    };
    let currentProduct = $state(defaultProduct);
</script>

<header class="row spread">
    <div class="left row g2">
        <img src="logo.svg" alt="logo" class="logo" />
        <nav>
            <ul class="row g2">
                <li><a href="#">Collections</a></li>
                <li><a href="#">Men</a></li>
                <li><a href="#">Women</a></li>
                <li><a href="#">About</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </nav>
    </div>
    <div class="right row g2">
        <button aria-label="cart button" class="cart-btn"></button>
        <img src="image-avatar.png" alt="avatar" class="profile-pic" />
    </div>
</header>
<main class="row">
    <section>
        <img src={currentProduct.bigImages[imgNum]} alt="" />
        <div class="row spread">
            {#each currentProduct.smallImages as smallImg, i}
                <img
                    src={smallImg}
                    alt=""
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
            <button class="row g1">
                <img src="icon-minus.svg" alt="icon minus" />
                <p>{qty}</p>
                <img src="icon-plus.svg" alt="icon plus" />
            </button>
            <button class="add-cart">Add to cart</button>
        </div>
    </section>
</main>
