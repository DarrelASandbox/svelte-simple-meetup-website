<script>
  import { createEventDispatcher } from 'svelte';
  import TextInput from '../UI/TextInput.svelte';
  import Button from '../UI/Button.svelte';
  import Modal from '../UI/Modal.svelte';

  let title = '';
  let subtitle = '';
  let address = '';
  let email = '';
  let description = '';
  let imageUrl = '';

  const dispatch = createEventDispatcher();

  const cancel = () => dispatch('cancel');

  const submitForm = () => {
    dispatch('save', { title: title, subtitle, address, email, description, imageUrl });
  };
</script>

<Modal title="Edit Meetup Data" on:cancel>
  <form on:submit|preventDefault={submitForm}>
    <TextInput id="title" label="Title" bind:value={title} />
    <TextInput id="subtitle" label="Subtitle" bind:value={subtitle} />
    <TextInput id="address" label="Address" bind:value={address} />
    <TextInput id="imageUrl" label="Image URL" bind:value={imageUrl} />
    <TextInput
      id="email"
      label="E-Mail"
      controlType="email"
      value={email}
      on:input={(e) => (email = e.target.value)}
    />
    <TextInput
      id="description"
      label="Description"
      controlType="textarea"
      bind:value={description}
    />
    <Button type="button" mode="outline" slot="footer" on:click={cancel}>Cancel</Button>
    <Button type="button" slot="footer" on:click={submitForm}>Save</Button>
  </form>
</Modal>

<style>
  form {
    width: 100%;
  }
</style>
