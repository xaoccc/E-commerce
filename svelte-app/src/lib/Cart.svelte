<script lang="ts">
    let { cart, products } = $props();
    const cartIds = Object.keys(cart);
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
            </div>
        {/each}
    {:else}
        <p>Your cart is empty</p>
    {/if}
</div>
