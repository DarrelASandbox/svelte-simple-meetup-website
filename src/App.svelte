<script>
  import { EditMeetup, MeetupGrid, meetups } from './Meetups';
  import { Button, Header } from './UI';

  let editMode;

  const cancelEdit = () => (editMode = null);
  const addMeetup = () => (editMode = null);
</script>

<Header />

<main>
  <div class="meetup-controls">
    <Button on:click={() => (editMode = 'add')}>New Meetup</Button>
  </div>
  {#if editMode === 'add'}
    <EditMeetup on:save={addMeetup} on:cancel={cancelEdit} />
  {/if}
  <MeetupGrid meetups={$meetups} />
</main>

<style>
  main {
    margin-top: 5rem;
  }

  .meetup-controls {
    margin: 1rem;
  }
</style>
