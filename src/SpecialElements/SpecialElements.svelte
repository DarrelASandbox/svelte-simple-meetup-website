<script>
  import { CartItem, Product } from './';

  let renderedComponent = { cmp: Product, title: 'Test Product', id: 'p1' };

  const toggle = () => {
    renderedComponent.cmp === Product
      ? (renderedComponent = {
          cmp: CartItem,
          title: 'CardItem title from SpecialElement.svelte',
          id: 'p2',
        })
      : (renderedComponent = {
          cmp: Product,
          title: 'Product title from SpecialElement.svelte',
          id: 'p1',
        });
  };
</script>

<button on:click={toggle}>Toggle Display</button>

<svelte:component
  this={renderedComponent.cmp}
  title={renderedComponent.title}
  id={renderedComponent.id}
/>
