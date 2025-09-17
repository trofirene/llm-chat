<template>
  <div class="container">
    <h1>How can I help you?</h1>
    <div class="card response" v-if="AIresponse" v-for="message in messageHistory">
      {{ message }}
      <!-- <MessageForm /> -->
    </div>
    <div class="card">
      <textarea type="text" placeholder="Enter your message" v-model="message"></textarea>
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
        <button class="btn btn-text" @click="sendMessage">
          <img src="@/icons/Send.svg" alt="send" />
          Send message
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import MessageForm from './api/MessageForm.vue'

export default {
  components: { MessageForm },
  setup() {
    const message = ref('')
    const AIresponse = ref('')
    const messageHistory = ref([])

    onMounted(() => {
      if (localStorage.getItem('chatHistory') !== null) {
        messageHistory.value = JSON.parse(localStorage.getItem('chatHistory'))
      }
    })

    function saveMessage(text, sender) {
      let chatHistory
      let newMessage = {
        text: text,
        sender: sender,
        timestamp: new Date().getTime(),
      }

      if (localStorage.getItem('chatHistory') === null) {
        chatHistory = []
        chatHistory.push(newMessage)
        messageHistory.value.push(newMessage)
        localStorage.setItem('chatHistory', JSON.stringify(chatHistory))
      } else {
        chatHistory = JSON.parse(localStorage.getItem('chatHistory'))
        chatHistory.push(newMessage)
        messageHistory.value.push(newMessage)
        localStorage.setItem('chatHistory', JSON.stringify(chatHistory))
      }
    }

    async function sendMessage() {
      try {
        saveMessage(message.value, 'user')
        let response = await fetch('https://openrouter.ai/api/v1/chat/completions', {
          method: 'POST',
          headers: {
            Authorization:
              'Bearer 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',
            'HTTP-Referer': 'http://localhost:5173',
            'X-Title': 'My LLM Model>',
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            model: 'deepseek/deepseek-chat-v3.1:free',
            messages: [
              {
                role: 'user',
                content: message.value,
              },
            ],
          }),
        })
        let data = await response.json()
        AIresponse.value = data.choices[0].message.content
        saveMessage(AIresponse.value, 'ai')
        message.value = ''
      } catch (error) {
        console.log('Error', error)
      }
    }

    return {
      sendMessage,
      message,
      AIresponse,
      messageHistory,
    }
  },
}
</script>

<style></style>
