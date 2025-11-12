<script lang="ts">
    let { showLightBox = $bindable(), currentProduct } = $props();
    let imgNum = $state(0);
</script>

<section class="lightbox">
    
    <section class="g2">
        <button onclick={() => (showLightBox = !showLightBox)} aria-label="close button"></button>
        <input
            type="image"
            src={currentProduct.bigImages[imgNum]}
            alt={`${currentProduct.title} Big Image`}
            name="saveForm"
            class="main-img"
            id="saveForm"
        />
        <div class="row g2 small-img-container">
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
</section>
