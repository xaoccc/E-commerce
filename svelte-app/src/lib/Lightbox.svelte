<script lang="ts">
    let {
        showLightBox = $bindable(),
        mobileView = $bindable(),
        currentProduct,
    } = $props();
    let imgNum = $state(0);
    const lastImgIdx = currentProduct.smallImages.length - 1;
    function showPrev() {
        imgNum == 0 ? (imgNum = lastImgIdx) : (imgNum -= 1);
    }
    function showNext() {
        imgNum == lastImgIdx ? (imgNum = 0) : (imgNum += 1);
    }
</script>

<section class={mobileView ? "lightbox-mobile" : "lightbox"}>
    <section class="g1">
        {#if !mobileView}
            <button
                onclick={() => (showLightBox = !showLightBox)}
                aria-label="close button"
                type="button"
            ></button>
        {/if}
        <div class="row main-img-container">
            <button
                onclick={() => showPrev()}
                aria-label="previous"
                class={mobileView ? "btn-prev lb" : "btn-prev"}
                type="button"
            ></button>
            <input
                type="image"
                src={currentProduct.bigImages[imgNum]}
                alt={`${currentProduct.title} Big Image`}
                name="saveForm"
                class="main-img"
                id="saveForm"
            />
            <button
                onclick={() => showNext()}
                aria-label="next"
                class={mobileView ? "btn-next lb" : "btn-next"}
                type="button"
            ></button>
        </div>
        {#if !mobileView}
            <div class="row g2 small-img-container">
                {#each currentProduct.smallImages as smallImg, i}
                    <div class="lb-img-bg row">
                        <img
                            aria-hidden="true"
                            src={smallImg}
                            alt={`${currentProduct.title} Image-${i}`}
                            class:selectedItem={imgNum === i}
                            onclick={() => {
                                imgNum = i;
                            }}
                        />
                    </div>
                {/each}
            </div>
        {/if}
    </section>
</section>
