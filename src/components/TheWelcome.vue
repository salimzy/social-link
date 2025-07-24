<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-50 to-indigo-50 flex flex-col items-center py-8 px-4 sm:px-6 lg:px-8">
    <!-- Floating background elements -->
    <div class="fixed inset-0 overflow-hidden pointer-events-none">
      <div class="absolute top-20 left-10 w-64 h-64 rounded-full bg-indigo-100 opacity-20 blur-3xl"></div>
      <div class="absolute bottom-20 right-10 w-64 h-64 rounded-full bg-pink-100 opacity-20 blur-3xl"></div>
    </div>

    <!-- Main content container -->
    <div class="relative w-full max-w-2xl flex flex-col items-center z-10">
      <!-- Profile Section -->
      <div class="w-full flex flex-col items-center mb-10 animate-fade-in">
        <div class="relative group">
          <div class="absolute -inset-2 rounded-full bg-gradient-to-r from-pink-500 to-indigo-600 opacity-75 blur-md group-hover:opacity-100 transition-all duration-300"></div>
          <img 
            :src="profile.avatar" 
            alt="Profile Picture"
            class="relative w-32 h-32 sm:w-40 sm:h-40 rounded-full object-cover border-4 border-white shadow-lg transition-transform duration-300 group-hover:scale-105"
            loading="lazy"
          >
        </div>
        <h1 class="mt-6 text-3xl sm:text-4xl font-bold text-gray-900 text-center bg-clip-text bg-gradient-to-r from-pink-500 to-indigo-600 text-transparent">
          {{ profile.name }}
        </h1>
        <p class="mt-3 text-lg text-gray-600 text-center max-w-md leading-relaxed">
          {{ profile.bio }}
        </p>
      </div>

      <!-- Links Section - Grid layout for better responsiveness -->
      <div class="w-full grid grid-cols-1 md:grid-cols-2 gap-4 mb-10">
        <a
          v-for="(link, index) in links"
          :key="index"
          :href="link.url"
          target="_blank"
          rel="noopener noreferrer"
          class="relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-sm p-5 hover:bg-white transition-all duration-200 hover:shadow-md hover:-translate-y-1 border border-gray-100/50 overflow-hidden group"
        >
          <!-- Hover effect background -->
          <div class="absolute inset-0 bg-gradient-to-br from-pink-50/30 to-indigo-50/30 opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
          
          <div class="relative flex items-center">
            <div class="w-12 h-12 flex items-center justify-center mr-4 bg-white rounded-xl shadow-sm border border-gray-100">
              <img 
                :src="link.icon" 
                :alt="`${link.title} icon`"
                class="w-6 h-6 object-contain"
                loading="lazy"
              >
            </div>
            <div class="flex-1 min-w-0">
              <h2 class="font-semibold text-gray-800 truncate">{{ link.title }}</h2>
              <p class="text-sm text-gray-500 truncate">{{ link.description }}</p>
            </div>
            <svg class="w-5 h-5 text-gray-400 group-hover:text-indigo-500 transition-colors" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
            </svg>
          </div>
        </a>
      </div>

      <!-- Contact Form -->
      <div class="w-full bg-white/80 backdrop-blur-sm rounded-2xl shadow-sm p-6 md:p-8 mb-10 border border-gray-100/50">
        <h2 class="text-2xl font-bold text-gray-800 mb-6">Get in Touch</h2>
        <form @submit.prevent="submitForm" class="space-y-5">
          <div>
            <label for="name" class="block text-sm font-medium text-gray-700 mb-2">Name</label>
            <input
              type="text"
              id="name"
              v-model="form.name"
              class="w-full px-4 py-3 border border-gray-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all placeholder-gray-400"
              :class="{ 'border-red-300 focus:ring-red-500': errors.name }"
              placeholder="Your name"
            >
            <p v-if="errors.name" class="mt-2 text-sm text-red-600">{{ errors.name }}</p>
          </div>

          <div>
            <label for="email" class="block text-sm font-medium text-gray-700 mb-2">Email</label>
            <input
              type="email"
              id="email"
              v-model="form.email"
              class="w-full px-4 py-3 border border-gray-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all placeholder-gray-400"
              :class="{ 'border-red-300 focus:ring-red-500': errors.email }"
              placeholder="your@email.com"
            >
            <p v-if="errors.email" class="mt-2 text-sm text-red-600">{{ errors.email }}</p>
          </div>

          <div>
            <label for="message" class="block text-sm font-medium text-gray-700 mb-2">Message</label>
            <textarea
              id="message"
              v-model="form.message"
              rows="4"
              class="w-full px-4 py-3 border border-gray-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all placeholder-gray-400"
              :class="{ 'border-red-300 focus:ring-red-500': errors.message }"
              placeholder="How can we help you?"
            ></textarea>
            <p v-if="errors.message" class="mt-2 text-sm text-red-600">{{ errors.message }}</p>
          </div>

          <button
            type="submit"
            class="w-full bg-gradient-to-r from-indigo-600 to-pink-500 text-white py-3 px-6 rounded-xl hover:from-indigo-700 hover:to-pink-600 transition-all duration-300 shadow-md hover:shadow-lg flex items-center justify-center font-medium"
            :disabled="isSubmitting"
            :class="{ 'opacity-75 cursor-not-allowed': isSubmitting, 'hover:shadow-indigo-200/50': !isSubmitting }"
          >
            <span v-if="!isSubmitting" class="flex items-center">
              <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
              </svg>
              Send Message
            </span>
            <span v-else class="flex items-center">
              <svg class="animate-spin -ml-1 mr-2 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              Sending...
            </span>
          </button>

          <transition name="fade">
            <div v-if="submitStatus === 'success'" class="mt-4 p-4 bg-green-50/90 text-green-700 rounded-lg border border-green-200 flex items-start backdrop-blur-sm">
              <svg class="w-5 h-5 mr-2 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"/>
              </svg>
              <span>Thank you! Your message has been sent successfully.</span>
            </div>
            <div v-else-if="submitStatus === 'error'" class="mt-4 p-4 bg-red-50/90 text-red-700 rounded-lg border border-red-200 flex items-start backdrop-blur-sm">
              <svg class="w-5 h-5 mr-2 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
              </svg>
              <span>Oops! Something went wrong. Please try again.</span>
            </div>
          </transition>
        </form>
      </div>

      <!-- Social Icons -->
      <div class="flex space-x-4">
        <a
          v-for="(social, index) in socials"
          :key="index"
          :href="social.url"
          target="_blank"
          rel="noopener noreferrer"
          :aria-label="`Follow us on ${social.name}`"
          class="text-gray-500 hover:text-indigo-600 transition-colors duration-300 p-2 rounded-full hover:bg-indigo-50"
        >
          <component :is="social.icon" class="w-6 h-6" />
        </a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.animate-fade-in {
  animation: fadeIn 0.6s ease-out forwards;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-20px); }
  to { opacity: 1; transform: translateY(0); }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>

<script setup>
import { ref, markRaw } from 'vue'
import emailjs from '@emailjs/browser';

// Icons - using SVG components for better performance
const InstagramIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/></svg>`
}

const TwitterIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M23.953 4.57a10 10 0 01-2.825.775 4.958 4.958 0 002.163-2.723c-.951.555-2.005.959-3.127 1.184a4.92 4.92 0 00-8.384 4.482C7.69 8.095 4.067 6.13 1.64 3.162a4.822 4.822 0 00-.666 2.475c0 1.71.87 3.213 2.188 4.096a4.904 4.904 0 01-2.228-.616v.06a4.923 4.923 0 003.946 4.827 4.996 4.996 0 01-2.212.085 4.936 4.936 0 004.604 3.417 9.867 9.867 0 01-6.102 2.105c-.39 0-.779-.023-1.17-.067a13.995 13.995 0 007.557 2.209c9.053 0 13.998-7.496 13.998-13.985 0-.21 0-.42-.015-.63A9.935 9.935 0 0024 4.59z"/></svg>`
}

const TikTokIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M12.525.02c1.31-.02 2.61-.01 3.91-.02.08 1.53.63 3.09 1.75 4.17 1.12 1.11 2.7 1.62 4.24 1.79v4.03c-1.44-.05-2.89-.35-4.2-.97-.57-.26-1.1-.59-1.62-.93-.01 2.92.01 5.84-.02 8.75-.08 1.4-.54 2.79-1.35 3.94-1.31 1.92-3.58 3.17-5.91 3.21-1.43.08-2.86-.31-4.08-1.03-2.02-1.19-3.44-3.37-3.65-5.71-.02-.5-.03-1-.01-1.49.18-1.9 1.12-3.72 2.58-4.96 1.66-1.44 3.98-2.13 6.15-1.72.02 1.48-.04 2.96-.04 4.44-.99-.32-2.15-.23-3.02.37-.63.41-1.11 1.04-1.36 1.75-.21.51-.15 1.07-.14 1.61.24 1.64 1.82 3.02 3.5 2.87 1.12-.01 2.19-.66 2.77-1.61.19-.33.4-.67.41-1.06.1-1.79.06-3.57.07-5.36.01-4.03-.01-8.05.02-12.07z"/></svg>`
}

const YouTubeIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg>`
}

// Profile data
const profile = ref({
  name: 'Your Brand Name',
  bio: 'Short bio or tagline about your brand',
  avatar: 'https://images.unsplash.com/photo-1633332755192-727a05c4013d?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1180&q=80'
})

// Links data
const links = ref([
  {
    title: 'Latest Collection',
    url: 'https://example.com/shop',
    description: 'Shop our newest products',
    icon: 'https://cdn-icons-png.flaticon.com/512/3144/3144456.png'
  },
  {
    title: 'YouTube Channel',
    url: 'https://www.bing.com/ck/a?!&&p=74c90acfc2a150c7ab6ae056a20b7f264ab7589eed75c0eaeb8151cc69c333f1JmltdHM9MTc1MzMxNTIwMA&ptn=3&ver=2&hsh=4&fclid=0f8fdef9-f974-6d51-03d5-cb38f8666ca1&psq=facebook+9jarightmp3&u=a1aHR0cHM6Ly93d3cueW91dHViZS5jb20vY2hhbm5lbC9VQ2l4NmROV2MxOHZCdXB6aDhoQlR2NUE&ntb=1',
    description: 'Watch our tutorials and videos',
    icon: 'https://cdn-icons-png.flaticon.com/512/1384/1384060.png'
  },
  {
    title: 'Join Our Community',
    url: 'https://discord.gg/example',
    description: 'Connect with other fans',
    icon: 'https://cdn-icons-png.flaticon.com/512/2111/2111370.png'
  },
  {
    title: 'Facebok',
    url: 'Facebook.com/9jarightmp3',
    description: 'Schedule a 1:1 call with us',
    icon: 'https://cdn-icons-png.flaticon.com/512/1570/1570906.png'
  }, 
   {
    title: 'Facebok',
    url: 'Facebook',
    description: 'Schedule a 1:1 call with us',
    icon: 'https://cdn-icons-png.flaticon.com/512/1570/1570906.png'
  }
])

// Social media links
const socials = markRaw([
  {
    name: 'Instagram',
    url: 'https://instagram.com/9jarightmp3',
    icon: InstagramIcon
  },
  {
    name: 'Twitter',
    url: 'https://twitter.com/starchcrypto',
    icon: TwitterIcon
  },
  {
    name: 'TikTok',
    url: 'https://tiktok.com/@9jarightmp3',
    icon: TikTokIcon
  },
  {
    name: 'YouTube',
    url: 'https://youtube.com/9jaright-mp3Vevoe',
    icon: YouTubeIcon
  }
])

// Contact form logic
const form = ref({
  name: '',
  email: '',
  message: ''
})

const errors = ref({
  name: '',
  email: '',
  message: ''
})

const isSubmitting = ref(false)
const submitStatus = ref(null)

const validateForm = () => {
  let isValid = true
  errors.value = { name: '', email: '', message: '' }

  if (!form.value.name.trim()) {
    errors.value.name = 'Name is required'
    isValid = false
  }

  if (!form.value.email.trim()) {
    errors.value.email = 'Email is required'
    isValid = false
  } else if (!/^\S+@\S+\.\S+$/.test(form.value.email)) {
    errors.value.email = 'Please enter a valid email address'
    isValid = false
  }

  if (!form.value.message.trim()) {
    errors.value.message = 'Message is required'
    isValid = false
  } else if (form.value.message.trim().length < 10) {
    errors.value.message = 'Message should be at least 10 characters'
    isValid = false
  }

  return isValid
}

const submitForm = async () => {
  if (!validateForm()) return

  isSubmitting.value = true
  submitStatus.value = null

  try {
    await emailjs.sendForm(
      'YOUR_SERVICE_ID', 
      'YOUR_TEMPLATE_ID', 
      '#contact-form', // Form element selector
      {
        publicKey: 'YOUR_PUBLIC_KEY',
        name: form.value.name,
        email: form.value.email,
        message: form.value.message
      }
    )
    
    submitStatus.value = 'success'
    form.value = { name: '', email: '', message: '' }
  } catch (error) {
    console.error('Email sending error:', error)
    submitStatus.value = 'error'
  } finally {
    isSubmitting.value = false
  }
}
</script>