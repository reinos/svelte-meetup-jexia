<script>
  import { createEventDispatcher } from 'svelte'
  import TextInput from '@ui/TextInput.svelte'
  import Button from '@ui/Button.svelte'
  import Modal from '@ui/Modal.svelte'
  import { isEmpty, isValidEmail } from '@helpers/validation.js'

  let title = ''
  let subtitle = ''
  let address = ''
  let contactEmail = ''
  let description = ''
  let imageUrl = ''

  const dispatch = createEventDispatcher()

  $: titleValid = !isEmpty(title)
  $: subtitleValid = !isEmpty(subtitle)
  $: addressValid = !isEmpty(address)
  $: descriptionValid = !isEmpty(description)
  $: imageUrlValid = !isEmpty(imageUrl)
  $: emailValid = isValidEmail(contactEmail)
  $: formIsValid =
    titleValid &&
    subtitleValid &&
    addressValid &&
    descriptionValid &&
    imageUrlValid &&
    emailValid

  function submitForm() {
    dispatch('save', {
      title,
      subtitle,
      address,
      contactEmail,
      description,
      imageUrl
    })
  }

  function cancel() {
    dispatch('cancel')
  }
</script>

<style lang="scss">
  @import './style.scss';
</style>

<Modal title="Edit Meetup Data" on:cancel>
  <form on:submit|preventDefault={submitForm}>
    <TextInput
      id="title"
      label="Title"
      valid={titleValid}
      validityMessage="Please enter a valid title."
      value={title}
      on:input={event => (title = event.target.value)} />
    <TextInput
      id="subtitle"
      label="Subtitle"
      valid={subtitleValid}
      validityMessage="Please enter a valid subtitle."
      value={subtitle}
      on:input={event => (subtitle = event.target.value)} />
    <TextInput
      id="address"
      label="Address"
      valid={addressValid}
      validityMessage="Please enter a valid address."
      value={address}
      on:input={event => (address = event.target.value)} />
    <TextInput
      id="imageUrl"
      label="Image URL"
      valid={imageUrlValid}
      validityMessage="Please enter a valid image url."
      value={imageUrl}
      on:input={event => (imageUrl = event.target.value)} />
    <TextInput
      id="email"
      label="E-Mail"
      type="email"
      valid={emailValid}
      validityMessage="Please enter a valid email address."
      value={contactEmail}
      on:input={event => (contactEmail = event.target.value)} />
    <TextInput
      id="description"
      label="Description"
      controlType="textarea"
      valid={descriptionValid}
      validityMessage="Please enter a valid description."
      bind:value={description} />
  </form>
  <div slot="footer">
    <Button type="button" mode="outline" on:click={cancel}>Cancel</Button>
    <Button type="button" on:click={submitForm} disabled={!formIsValid}>Save</Button>
  </div>
</Modal>
