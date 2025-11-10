<script lang="ts">
    let { cart = $bindable(), products = $bindable() } = $props<{
        cart: Record<string, number>;
        products: Record<string, number>;
    }>();
    ;
    const cartIds = $derived(Object.keys(cart));
    function removeItem(product: string) {
        delete cart[product];
    }
</script>

<div class="cart">
    <p>Cart</p>
    <hr />
    {#if cartIds.length != 0}
        {#each cartIds as product}
            <div class="product-data-container row g1">
                <img src={products[product].smallImages[0]} alt="" />
                <div>
                    <p>{products[product].title}</p>
                    <p>
                        ${(
                            products[product].old_price *
                            products[product].discount
                        ).toFixed(2)} x
                        {products[product].qty}
                        <span class="bold"
                            >${products[product].old_price *
                                products[product].discount *
                                products[product].qty}</span
                        >
                    </p>
                </div>
                <button
                    aria-label="remove item"
                    class="remove-item"
                    onclick={() => removeItem(product)}
                ></button>
            </div>
            <button class="checkout-btn">Chechout</button>
        {/each}
    {:else}
        <p>Your cart is empty</p>
    {/if}
</div>
