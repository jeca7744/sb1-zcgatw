<script lang="ts">
  import { pb } from './pocketbase';

  let name = '';
  let email = '';
  let message = '';
  let submitStatus = '';
  let isSuccess = false;

  async function handleSubmit() {
    try {
      const record = await pb.collection('contact_forms').create({
        name,
        email,
        message
      });
      console.log('Record created:', record);
      submitStatus = 'Form submitted successfully!';
      isSuccess = true;
      name = '';
      email = '';
      message = '';
    } catch (error) {
      console.error('Error submitting form:', error);
      if (error.status === 0) {
        submitStatus = 'Unable to connect to PocketBase. Please ensure it\'s running and configured correctly.';
      } else {
        submitStatus = 'Error submitting form. Please try again.';
      }
      isSuccess = false;
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <div class="input-group">
    <input id="name" type="text" bind:value={name} required placeholder=" ">
    <label for="name">Name</label>
  </div>
  <div class="input-group">
    <input id="email" type="email" bind:value={email} required placeholder=" ">
    <label for="email">Email</label>
  </div>
  <div class="input-group">
    <textarea id="message" bind:value={message} required rows="4" placeholder=" "></textarea>
    <label for="message">Message</label>
  </div>
  <button type="submit">Submit</button>
</form>

{#if submitStatus}
  <p class="status-message {isSuccess ? 'success' : 'error'}">{submitStatus}</p>
{/if}

<p class="note">Note: This form requires a running PocketBase instance. Please ensure PocketBase is set up and running on the specified URL in the pocketbase.ts file.</p>

<style>
  .note {
    margin-top: 1rem;
    font-size: 0.875rem;
    color: #666;
    text-align: center;
  }
</style>