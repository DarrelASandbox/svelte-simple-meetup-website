<script>
  import { Modal } from './';

  export let message;
</script>

<Modal title="An error occurred!" on:cancel>
  <p>{message}</p>
</Modal>
