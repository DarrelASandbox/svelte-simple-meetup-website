<script>
  import { EditMeetup, MeetupDetail, MeetupGrid, meetups } from './Meetups';
  import { Button, Header } from './UI';

  let editMode;
  let editedId;
  let page = 'overview';
  let pageData = {};

  const cancelEdit = () => {
    editMode = null;
    editedId = null;
  };
  const savedMeetup = () => {
    editMode = null;
    editedId = null;
  };

  const showDetails = (e) => {
    page = 'details';
    pageData.id = e.detail;
  };

  const closeDetails = (e) => {
    page = 'overview';
    pageData = {};
  };

  const startEdit = (e) => {
    editMode = 'edit';
    editedId = e.detail;
  };
</script>

<Header />

<main>
  {#if page === 'overview'}
    {#if editMode === 'edit'}
      <EditMeetup id={editedId} on:save={savedMeetup} on:cancel={cancelEdit} />
    {/if}
    <MeetupGrid
      meetups={$meetups}
      on:showdetails={showDetails}
      on:edit={startEdit}
      on:add={() => (editMode = 'edit')}
    />
  {:else}
    <MeetupDetail id={pageData.id} on:close={closeDetails} />
  {/if}
</main>

<style>
  main {
    margin-top: 5rem;
  }
</style>
