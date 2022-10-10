<script>
  import { createEventDispatcher } from 'svelte';
  import { Button } from '../UI';
  import { MeetupFilter, MeetupItem } from './';
  import { scale } from 'svelte/transition';
  import { flip } from 'svelte/animate';

  export let meetups;

  let favsOnly = false;

  const setFilter = (e) => (favsOnly = e.detail === 1);

  const dispatch = createEventDispatcher();

  $: filteredMeetups = favsOnly ? meetups.filter((m) => m.isFavorite) : meetups;
</script>

<section id="meetup-controls">
  <MeetupFilter on:select={setFilter} />
  <Button on:click={() => dispatch('add')}>New Meetup</Button>
</section>

<section id="meetups">
  {#each filteredMeetups as meetup, index (meetup.id)}
    <!-- https://github.com/sveltejs/svelte/issues/5112 -->
    <!-- <MeetupItem {...meetup} on:togglefavorite /> -->

    <!-- wrap MeetupItem component in a div for animation -->
    <!-- since we cannot do it in article which has child elements -->
    <div transition:scale animate:flip={{ duration: 300 }}>
      <MeetupItem
        id={meetup.id}
        title={meetup.title}
        subtitle={meetup.subtitle}
        description={meetup.description}
        imageUrl={meetup.imageUrl}
        email={meetup.contactEmail}
        address={meetup.address}
        isFav={meetup.isFavorite}
        on:showdetails
        on:edit
      />
    </div>
  {:else}
    <p>There are no meetups scheduled!</p>
  {/each}
</section>

<style>
  #meetups {
    width: 100%;
    display: grid;
    grid-template-columns: 1fr;
    grid-gap: 1rem;
  }

  @media (min-width: 768px) {
    #meetups {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  #meetup-controls {
    margin: 1rem;
    display: flex;
    justify-content: space-between;
  }
</style>
