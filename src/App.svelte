<script>
  import { EditMeetup, MeetupGrid, meetups } from './Meetups/index';
  import { Button, Header } from './UI';

  let loadedMeetups = meetups;

  let editMode;

  const cancelEdit = () => (editMode = null);
  const addMeetup = (e) => {
    const meetupData = { ...e.detail };

    meetups.addMeetup(meetupData);
    editMode = null;
  };

  const toggleFavorite = (e) => {
    const id = e.detail;
    meetups.toggleFavorite(id);
  };
</script>

<Header />

<main>
  <div class="meetup-controls">
    <Button on:click={() => (editMode = 'add')}>New Meetup</Button>
  </div>
  {#if editMode === 'add'}
    <EditMeetup on:save={addMeetup} on:cancel={cancelEdit} />
  {/if}
  <MeetupGrid meetups={$meetups} on:togglefavorite={toggleFavorite} />
</main>

<style>
  main {
    margin-top: 5rem;
  }

  .meetup-controls {
    margin: 1rem;
  }
</style>
