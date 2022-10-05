<script>
  import { createEventDispatcher } from 'svelte';
  import TextInput from '../UI/TextInput.svelte';
  import Button from '../UI/Button.svelte';
  import Modal from '../UI/Modal.svelte';
  import { isEmpty, isValidEmail } from '../helpers/validation';

  let title = '';
  let subtitle = '';
  let address = '';
  let email = '';
  let description = '';
  let imageUrl = '';

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
    dispatch('save', { title: title, subtitle, address, email, description, imageUrl });
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
  </form>
</Modal>

<style>
  form {
    width: 100%;
  }
</style>
