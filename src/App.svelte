<script>
  import { EditMeetup, MeetupDetail, MeetupGrid, meetups } from './Meetups';
  import { SpecialElements } from './SpecialElements';
  import { Error, Header, LoadingSpinner } from './UI';

  let editMode;
  let editedId;
  let page = 'overview';
  let pageData = {};
  let isLoading = true;
  let error;

  fetch(
    'https://meetup-8d74b-default-rtdb.asia-southeast1.firebasedatabase.app/meetups.json'
  )
    .then((res) => {
      if (!res.ok) throw new Error('Error fetching');
      return res.json();
    })
    .then((data) => {
      const loadedMeetups = [];
      for (const key in data) {
        loadedMeetups.push({ ...data[key], id: key });
      }

      isLoading = false;
      meetups.setMeetups(loadedMeetups.reverse());
    })
    .catch((err) => {
      error = err;
      isLoading = false;
      console.log(err);
    });

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

  const clearError = () => (error = null);
</script>

{#if error}
  <Error message={error.message} on:cancel={clearError} />
{/if}
<Header />

<main>
  {#if page === 'overview'}
    {#if editMode === 'edit'}
      <EditMeetup id={editedId} on:save={savedMeetup} on:cancel={cancelEdit} />
    {/if}
    {#if isLoading}
      <LoadingSpinner />
    {:else}
      <SpecialElements />

      <hr />

      <MeetupGrid
        meetups={$meetups}
        on:showdetails={showDetails}
        on:edit={startEdit}
        on:add={() => (editMode = 'edit')}
      />
    {/if}
  {:else}
    <MeetupDetail id={pageData.id} on:close={closeDetails} />
  {/if}
</main>

<style>
  main {
    margin-top: 5rem;
  }

  hr {
    margin: 7rem 0rem;
  }
</style>
