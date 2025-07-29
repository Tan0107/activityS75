<template>
  <section id="contact">
    <div class="container bg-white">
      <h2>Contact Me</h2>
      <p>Let's get in touch!</p>

      <form @submit.prevent="submitForm" class="mt-4 mx-auto contact-form mb-3">
        <div class="mb-3 text-start">
          <label for="name" class="form-label">Name</label>
          <input type="text" v-model="name" class="form-control" id="name" required />
        </div>

        <div class="mb-3 text-start">
          <label for="email" class="form-label">Email</label>
          <input type="email" v-model="email" class="form-control" id="email" required />
        </div>

        <div class="mb-3 text-start">
          <label for="message" class="form-label">Message</label>
          <textarea v-model="message" class="form-control" id="message" rows="4" required></textarea>
        </div>

        <button type="submit" class="btn btn-primary">
          {{ isLoading ? "Sending..." : "Submit" }}
        </button>

        <!-- ✅ FIXED: reCAPTCHA div wrapper closed properly -->
        <div class="d-flex justify-content-end mt-3">
          <div ref="recaptchaContainer"></div>
        </div>
      </form>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Notyf } from 'notyf'
import 'notyf/notyf.min.css'

const notyf = new Notyf()
const WEB3FORMS_ACCESS_KEY = '7b2d9ab7-d2ba-4ebb-a035-7a0ea66439fa'
const SITE_KEY = '6LfKxpIrAAAAAISOL7FzL6XxM89Eq6SqTb-stAZ9' // Your actual reCAPTCHA Site Key

const name = ref('')
const email = ref('')
const message = ref('')
const isLoading = ref(false)

const recaptchaContainer = ref(null)
const recaptchaWidgetId = ref(null)
const recaptchaToken = ref('')

function onRecaptchaSuccess(token) {
  recaptchaToken.value = token
}

function onRecaptchaExpired() {
  recaptchaToken.value = ''
}

function renderRecaptcha() {
  if (!window.grecaptcha) {
    console.error('reCAPTCHA not loaded')
    return
  }

  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
    sitekey: SITE_KEY,
    size: 'normal',
    callback: onRecaptchaSuccess,
    'expired-callback': onRecaptchaExpired,
  })
}

function resetRecaptcha() {
  if (recaptchaWidgetId.value !== null) {
    window.grecaptcha.reset(recaptchaWidgetId.value)
    recaptchaToken.value = ''
  }
}

const submitForm = async () => {
  if (!recaptchaToken.value) {
    notyf.error('Please verify that you are not a robot.')
    return
  }

  isLoading.value = true

  try {
    const response = await fetch('https://api.web3forms.com/submit', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Accept: 'application/json',
      },
      body: JSON.stringify({
        access_key: WEB3FORMS_ACCESS_KEY,
        subject: 'New message from portfolio contact form',
        name: name.value,
        email: email.value,
        message: message.value,
      }),
    })

    const result = await response.json()

    if (result.success) {
      notyf.success('Message Sent!')
    } else {
      notyf.error('Failed to send message.')
    }
  } catch (error) {
    console.error(error)
    notyf.error('Failed to send message.')
  } finally {
    isLoading.value = false
    resetRecaptcha()
  }
}

onMounted(() => {
  const interval = setInterval(() => {
    if (window.grecaptcha && window.grecaptcha.render) {
      renderRecaptcha()
      clearInterval(interval)
    }
  }, 100)

  onBeforeUnmount(() => {
    clearInterval(interval)
  })
})
</script>
