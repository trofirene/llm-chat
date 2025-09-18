<template>
  <div
    class="card response"
    v-if="messageHistory"
    v-for="message in messageHistory"
    :class="message.sender === 'user' ? 'user-message' : 'ai-message'"
  >
    {{ message.text }}
    <div class="message-actions">
      <button class="btn btn-delete" @click="deleteMessage(message)">
        <img src="/src/icons/Trash.svg" alt="delete message" />
      </button>
      <button
        class="btn btn-save"
        @click="saveToClipboard(message)"
        v-if="!copiedMessage[message.timestamp]"
      >
        <img src="/src/icons/Copy.svg" alt="save message" />
      </button>
      <button class="btn btn-done" v-else>
        <img src="/src/icons/Tick.svg" alt="copied" />
      </button>
    </div>
  </div>
</template>

<script>
import { inject, ref } from 'vue'

export default {
  setup() {
    const message = inject('message')
    const AIresponse = inject('AIresponse')
    const messageHistory = inject('messageHistory')
    const copiedMessage = ref({})

    function deleteMessage(msgToDelete) {
      const index = messageHistory.value.findIndex((msg) => msg.timestamp === msgToDelete.timestamp)
      messageHistory.value.splice(index, 1)
      localStorage.setItem('chatHistory', JSON.stringify(messageHistory.value))
    }

    async function saveToClipboard(msgToSave) {
      try {
        let txt = msgToSave.text
        await navigator.clipboard.writeText(txt)
        copiedMessage.value[msgToSave.timestamp] = true

        setTimeout(() => {
          copiedMessage.value[msgToSave.timestamp] = false
        }, 500)
      } catch (err) {
        console.log('Error', err)
      }
    }

    return {
      message,
      AIresponse,
      messageHistory,
      deleteMessage,
      saveToClipboard,
      copiedMessage,
    }
  },
}
</script>

<style>
.user-message {
  background-color: white;
}

.ai-message {
  background-color: lightgrey;
}

.card.user-message {
  display: flex;
}

.card.ai-message {
  display: flex;
}

.message-actions {
  display: flex;
  margin-left: auto;
  margin-top: auto;
}

.btn-delete {
  background-color: none;
}
</style>
