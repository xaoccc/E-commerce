<script lang="ts">
    let { showLightBox = $bindable() } = $props();

</script>
<section class="lightbox">
    <button onclick={() => showLightBox = !showLightBox}>hide</button>

</section>