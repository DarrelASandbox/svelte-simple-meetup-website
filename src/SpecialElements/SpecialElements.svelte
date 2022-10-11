<script>
  import { CartItem, FamilyNode, Product } from './';

  let familyStructure = [
    {
      isParent: true,
      name: 'Chris',
      children: [
        { isParent: true, name: 'Moe', children: [{ isParent: false, name: 'Julie' }] },
      ],
    },
    { isParent: false, name: 'Anna' },
  ];

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

<div>
  {#each familyStructure as familyMember}
    <FamilyNode member={familyMember} />
  {/each}
</div>

<hr />

<div>
  <button on:click={toggle}>Toggle Display</button>
  <svelte:component
    this={renderedComponent.cmp}
    title={renderedComponent.title}
    id={renderedComponent.id}
  />
</div>

<style>
  hr {
    margin: 7rem 0rem;
  }
</style>
