<script>
  import { createEventDispatcher } from 'svelte';
  import { isEmpty, isValidEmail } from '../helpers/validation';
  import { meetups } from '../Meetups';
  import { Button, Modal, TextInput } from '../UI';

  export let id = null;

  let title = '';
  let subtitle = '';
  let address = '';
  let email = '';
  let description = '';
  let imageUrl = '';

  if (id) {
    // using shorthand for store subscription
    // refer to commit (f29b89f) for other implementation
    const selectedMeetup = $meetups.find((i) => i.id === id);
    title = selectedMeetup.title;
    subtitle = selectedMeetup.subtitle;
    address = selectedMeetup.address;
    email = selectedMeetup.contactEmail;
    description = selectedMeetup.description;
    imageUrl = selectedMeetup.imageUrl;
  }

  const dispatch = createEventDispatcher();

  $: titleValid = !isEmpty(title);
  $: subtitleValid = !isEmpty(subtitle);
  $: addressValid = !isEmpty(address);
  $: emailValid = isValidEmail(email);
  $: descriptionValid = !isEmpty(description);
  $: imageUrlValid = !isEmpty(imageUrl);
  $: formIsValid =
    titleValid &&
    subtitleValid &&
    addressValid &&
    emailValid &&
    descriptionValid &&
    imageUrlValid;

  const cancel = () => dispatch('cancel');

  const submitForm = () => {
    const meetupData = {
      title,
      subtitle,
      address,
      contactEmail: email,
      description,
      imageUrl,
    };

    if (id) {
      fetch(
        `https://meetup-8d74b-default-rtdb.asia-southeast1.firebasedatabase.app/meetups/${id}.json`,
        {
          method: 'PATCH',
          // take note of id & isFavourite data
          // Firebase generates id for us
          body: JSON.stringify({ ...meetupData }),
          headers: { 'Content-Type': 'application/json' },
        }
      )
        .then((res) => {
          if (!res.ok) throw new Error('Something went wrong!');
          meetups.updateMeetup(id, meetupData);
        })
        .catch((err) => console.log(err));
    } else {
      fetch(
        'https://meetup-8d74b-default-rtdb.asia-southeast1.firebasedatabase.app/meetups.json',
        {
          method: 'POST',
          body: JSON.stringify({ ...meetupData, isFavorite: false }),
          headers: { 'Content-Type': 'application/json' },
        }
      )
        .then((res) => {
          if (!res.ok) throw new Error('Something went wrong!');
          return res.json();
        })
        .then((data) =>
          meetups.addMeetup({ ...meetupData, isFavorite: false, id: data.name })
        )
        .catch((err) => console.log(err));
    }
    dispatch('save'); // refers to App.svelte `on:save={addMeetup}`
  };

  const deleteMeetup = () => {
    fetch(
      `https://meetup-8d74b-default-rtdb.asia-southeast1.firebasedatabase.app/meetups/${id}.json`,
      { method: 'DELETE' }
    )
      .then((res) => {
        if (!res.ok) throw new Error('Something went wrong!');
        meetups.deleteMeetup(id);
        dispatch('save');
      })
      .catch((err) => console.log(err));
  };
</script>

<Modal title="Edit Meetup Data" on:cancel>
  <form on:submit|preventDefault={submitForm}>
    <TextInput
      id="title"
      label="Title"
      valid={titleValid}
      validityMessage="Please enter a valid title."
      bind:value={title}
    />
    <TextInput
      id="subtitle"
      label="Subtitle"
      bind:value={subtitle}
      valid={subtitleValid}
      validityMessage="Please enter a valid subtitle."
    />
    <TextInput
      id="address"
      label="Address"
      bind:value={address}
      valid={addressValid}
      validityMessage="Please enter a valid address."
    />
    <TextInput
      id="imageUrl"
      label="Image URL"
      bind:value={imageUrl}
      valid={imageUrlValid}
      validityMessage="Please enter a valid imageUrl."
    />
    <TextInput
      id="email"
      label="E-Mail"
      controlType="email"
      value={email}
      valid={emailValid}
      validityMessage="Please enter a valid email address."
      on:input={(e) => (email = e.target.value)}
    />
    <TextInput
      id="description"
      label="Description"
      controlType="textarea"
      valid={descriptionValid}
      validityMessage="Please enter a valid description."
      bind:value={description}
    />

    <Button type="button" mode="outline" slot="footer" on:click={cancel}>Cancel</Button>
    <Button type="button" slot="footer" on:click={submitForm} disabled={!formIsValid}
      >Save</Button
    >

    {#if id}
      <Button type="button" on:click={deleteMeetup}>Delete</Button>
    {/if}
  </form>
</Modal>

<style>
  form {
    width: 100%;
  }
</style>
