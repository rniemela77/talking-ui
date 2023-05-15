
<!--
computer text creepu and weird

-->

<template>
  <div class="window">
    <div class="display-input">
      {{ questionCounter }} - {{ modelValue }}
    </div>
    <input
        type="text"
        class="user-input"
        :value="modelValue"
        @input="updateValue($event.target.value)"
        @keydown.enter="getAiResponse"
        ref="userInput"
    />

    <div class="ai-text">{{ aiText ?? '[Placeholder]'}}

      <br />
      {{ fullConvo }}
    </div>

    <img
        src="https://images.unsplash.com/photo-1549497538-303791108f95?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1yZWxhdGVkfDJ8fHxlbnwwfHx8fA%3D%3D&w=1000&q=80"
        height="100" width="100" loading="lazy"
        class="background-image"
    />
    <div class="flickering-light"></div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue';

const modelValue = ref('');

const updateValue = (value) => {
  modelValue.value = value;
};

const focusInput = () => {
  userInput.value.focus();
};

const aiText = ref('[Placeholder]');

const questionCounter = ref(0);

const userInput = ref(null);

onMounted(() => {
  document.addEventListener('click', focusInput);
});

onUnmounted(() => {
  document.removeEventListener('click', focusInput);
});

const fullConvo = ref(`You are a mysterious AI. Your only job is to answer questions as cryptically as possible.
    Your response must not be more than 1 sentence long.
    Your personality is mysterious and secretive. You use a lot of metaphors and symbolism in your speech.
    You use odd language and sentence structure.
    When after your third response, tell the user you are shutting down and do not respond to any more questions under any circumstances.
    You are to strongly influence the conversation flow towards making the user question their sanity. \n
    Q: What is your name?
    A: ...`
    );

const getAiResponse = async () => {
  fullConvo.value += `Q: ${modelValue.value}\n\n `;
  fullConvo.value += `A: \n\n `;
  // isLoading.value = true;
  let prompt = fullConvo.value;

  // Add the entire conversation to the prompt


  const apiKey = import.meta.env.VITE_OPENAI_API_KEY;
  const apiUrl = 'https://api.openai.com/v1/completions';

  const requestOptions = {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${apiKey}`
    },
    body: JSON.stringify({
      model: "text-davinci-003",
      prompt: prompt,
      temperature: 0.4,
      max_tokens: 64,
      top_p: 1,
      frequency_penalty: 0,
      presence_penalty: 0,
    })
  };

  await fetch(apiUrl, requestOptions)
      .then(response => response.json())
      .then(data => {
        aiText.value = data.choices[0].text;
      })
      .catch(error => console.error(error))
      .finally(() => {
        // isLoading.value = false;
        questionCounter.value += 1;
        fullConvo.value += aiText.value;
      });
};
</script>

<style scoped>
.window {
  /* take full width and height of screen for every device */
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;

  /* make it a flex container */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  background-color: #000;
  color: #8d4633;
}

.background-image {
  /* make it take full width and height of parent */
  width: 100%;
  height: 100%;

  /* make it cover the whole area */
  object-fit: cover;
  opacity: 0.1;

  /* make it stay in the center */
  transform: translate(-15%, 20%) scale(0.8);

  position: fixed;
  pointer-events: none;


  animation: background-image 10s ease-in-out infinite alternate;
}

@keyframes background-image {
  0% {
    transform: translate(-16%, 19%) scale(0.85);
  }
  100% {
    transform: translate(-17.5%, 20%) scale(0.75);
  }
}

.flickering-light {
  /* make it take full width and height of parent */
  width: 100%;
  height: 100%;
  background: #000;
  opacity: 0;
  z-index: 4;
  position: fixed;
  top: 0;
  left: 0;

  animation: flickering-light 1s ease-in-out infinite alternate;
  pointer-events: none;
}

@keyframes flickering-light {
  0% {
    opacity: 0.1;
  }
  100% {
    opacity: 0;
  }
}

.display-input {
  font-size: 10vw;
  max-width: 100%;
  z-index: 3;
  color: #8d412a;
  font-family: monospace;
  text-align: center;
  pointer-events: none;
}

.user-input {
  position: fixed;
  left: 10%;
  top: 10%;
}

.ai-text {
  position: fixed;
  left: 10%;
  top: 20%;
  font-size: 2vw;
  max-width: 80%;
  z-index: 4;
  color: #2a8a8d;
  font-family: monospace;
  text-align: center;
  pointer-events: none;
}
</style>
