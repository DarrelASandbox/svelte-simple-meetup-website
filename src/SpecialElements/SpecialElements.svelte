<script>
  import { CartItem, Product } from './';

  let showProduct = true;

  const toggle = () => (showProduct = !showProduct);
</script>

<button on:click={toggle}>Toggle Display</button>

{#if showProduct}
  <Product title="Product title from SpecialElement.svelte" id="p1" />
{:else}
  <CartItem title="CartItem title from SpecialElement.svelte" id="p1" />
{/if}
