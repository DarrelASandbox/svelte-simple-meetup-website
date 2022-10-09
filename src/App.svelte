<script>
  import { EditMeetup, MeetupDetail, MeetupGrid, meetups } from './Meetups';
  import { Button, Header } from './UI';

  let editMode;
  let page = 'overview';
  let pageData = {};

  const cancelEdit = () => (editMode = null);
  const addMeetup = () => (editMode = null);

  const showDetails = (e) => {
    page = 'details';
    pageData.id = e.detail;
  };

  const closeDetails = (e) => {
    page = 'overview';
    pageData = {};
  };
</script>

<Header />

<main>
  {#if page === 'overview'}
    <div class="meetup-controls">
      <Button on:click={() => (editMode = 'add')}>New Meetup</Button>
    </div>
    {#if editMode === 'add'}
      <EditMeetup on:save={addMeetup} on:cancel={cancelEdit} />
    {/if}
    <MeetupGrid meetups={$meetups} on:showdetails={showDetails} />
  {:else}
    <MeetupDetail id={pageData.id} on:close={closeDetails} />
  {/if}
</main>

<style>
  main {
    margin-top: 5rem;
  }

  .meetup-controls {
    margin: 1rem;
  }
</style>
