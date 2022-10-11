<script context="module">
  console.log('This script tag runs only once!');

  let deactivateNode;
</script>

<script>
  export let member;
  let isActive;
  console.log('This script tag runs multile times!');

  const activate = () => {
    if (deactivateNode) deactivateNode();
    isActive = true;
    deactivateNode = deactivate;
  };

  const deactivate = () => (isActive = false);
</script>

<div on:click={activate} class:active={isActive}>
  <h1>{member.name}</h1>
  {#if member.isParent}
    {#each member.children as child}
      <!-- <li>{child.name}</li> -->
      <svelte:self member={child} />
    {/each}
  {/if}
</div>

<style>
  div {
    margin-left: 2rem;
  }

  .active {
    color: red;
  }
</style>
