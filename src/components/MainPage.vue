<template>
  <div class="container">
    <h1>How can I help you?</h1>
    <div class="card">
      <input type="text" placeholder="Enter your message" />
      <hr />
      <div class="btns-container">
        <button class="btn">
          <img src="@/icons/Paperclip.svg" alt="add file" />
        </button>
        <button class="btn">
          <img src="@/icons/Photo.svg" alt="photo" />
        </button>
        <button class="btn">
          <img src="@/icons/Microphone.svg" alt="microphone" />
        </button>
        <button class="btn">
          <img src="@/icons/Grid.svg" alt="grid" />
        </button>
        <button class="btn btn-text">
          <img src="@/icons/Send.svg" alt="send" />
          Send message
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MainPage',
}
</script>

<style></style>
