<template>
    <div class="bg-white">
      <header class="absolute inset-x-0 top-0 z-50">
        <nav class="flex items-center justify-between p-6 lg:px-8" aria-label="Global">
          <div class="flex lg:flex-1">
            <a href="#" class="-m-1.5 p-1.5">
              <span class="sr-only">Nuxt 3 & OpenAI common task</span>
              
            </a>
          </div>
          <div class="flex lg:hidden">
            <button type="button" class="-m-2.5 inline-flex items-center justify-center rounded-md p-2.5 text-gray-700" @click="mobileMenuOpen = true">
              <span class="sr-only">Open main menu</span>
              <Bars3Icon class="h-6 w-6" aria-hidden="true" />
            </button>
          </div>
          <div class="hidden lg:flex lg:gap-x-12">
            <a v-for="item in navigation" :key="item.name" :href="item.href" class="text-sm font-semibold leading-6 text-gray-900">{{ item.name }}</a>
          </div>
          <div class="hidden lg:flex lg:flex-1 lg:justify-end">
          </div>
        </nav>
        <Dialog as="div" class="lg:hidden" @close="mobileMenuOpen = false" :open="mobileMenuOpen">
          <div class="fixed inset-0 z-50" />
          <DialogPanel class="fixed inset-y-0 right-0 z-50 w-full overflow-y-auto bg-white px-6 py-6 sm:max-w-sm sm:ring-1 sm:ring-gray-900/10">
            <div class="flex items-center justify-between">
              <a href="#" class="-m-1.5 p-1.5">
                <span class="sr-only">Nuxt 3 & OpenAI common task</span>
                
              </a>
              <button type="button" class="-m-2.5 rounded-md p-2.5 text-gray-700" @click="mobileMenuOpen = false">
                <span class="sr-only">Close menu</span>
                <XMarkIcon class="h-6 w-6" aria-hidden="true" />
              </button>
            </div>
            <div class="mt-6 flow-root">
              <div class="-my-6 divide-y divide-gray-500/10">
                <div class="space-y-2 py-6">
                  <a v-for="item in navigation" :key="item.name" :href="item.href" class="-mx-3 block rounded-lg px-3 py-2 text-base font-semibold leading-7 text-gray-900 hover:bg-gray-50">{{ item.name }}</a>
                </div>
                <div class="py-6">
                  <a href="#" class="-mx-3 block rounded-lg px-3 py-2.5 text-base font-semibold leading-7 text-gray-900 hover:bg-gray-50">Log in</a>
                </div>
              </div>
            </div>
          </DialogPanel>
        </Dialog>
      </header>
  
      <div class="relative isolate px-6 pt-14 lg:px-8">
        <div class="absolute inset-x-0 -top-40 -z-10 transform-gpu overflow-hidden blur-3xl sm:-top-80" aria-hidden="true">
          <div class="relative left-[calc(50%-11rem)] aspect-[1155/678] w-[36.125rem] -translate-x-1/2 rotate-[30deg] bg-gradient-to-tr from-[#ff80b5] to-[#9089fc] opacity-30 sm:left-[calc(50%-30rem)] sm:w-[72.1875rem]" style="clip-path: polygon(74.1% 44.1%, 100% 61.6%, 97.5% 26.9%, 85.5% 0.1%, 80.7% 2%, 72.5% 32.5%, 60.2% 62.4%, 52.4% 68.1%, 47.5% 58.3%, 45.2% 34.5%, 27.5% 76.7%, 0.1% 64.9%, 17.9% 100%, 27.6% 76.8%, 76.1% 97.7%, 74.1% 44.1%)" />
        </div>
        <div class="mx-auto max-w-2xl py-8 sm:py-12 lg:py-16">
          <div class="text-center">
            <h1 class="text-4xl font-bold tracking-tight text-gray-900 sm:text-6xl">Inferring</h1>
            <span class="mt-5 block text-sm font-medium text-slate-700">Information Extraction (for example, Identify names )</span>
            <form class="pt-6" @submit.prevent>
                <label class="block mt-6">
                    <span class="block text-sm font-medium text-slate-700">Add your OPENAI API Key</span>
                    <!-- Using form state modifiers, the classes can be identical for every input -->
                    <input v-model="openaikey" type="text" class="mt-1 w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400"/>
                </label>
                <label class="block mt-6">
                    <span class="block text-sm font-medium text-slate-700">Add your text sample</span>
                    <!-- Using form state modifiers, the classes can be identical for every input -->
                    <textarea v-model="textInput" rows="5" class="mt-1 w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400"/>
                </label>
                <label class="block mt-6">
                    <span class="block text-sm font-medium text-slate-700">Add names to recognize followed by a comma (,)</span>
                    <!-- Using form state modifiers, the classes can be identical for every input -->
                    <input v-model="names" type="text" placeholder="john, doe" class="mt-1 w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400"/>
                </label>
                <label class="block mt-6">
                    <button @click="() => submit()" type="submit" class="rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Start</button>
                </label>
                <label class="block mt-6">
                    <span class="mt-5 block text-sm font-medium text-slate-700">{{ result }}</span>
                </label>
            </form>
          </div>
        </div>
        <div class="absolute inset-x-0 top-[calc(100%-100%)] -z-10 transform-gpu overflow-hidden blur-3xl sm:top-[calc(100%-100%)]" aria-hidden="true">
          <div class="relative left-[calc(50%+3rem)] aspect-[1155/678] w-[36.125rem] -translate-x-1/2 bg-gradient-to-tr from-[#ff80b5] to-[#9089fc] opacity-30 sm:left-[calc(50%+36rem)] sm:w-[72.1875rem]" style="clip-path: polygon(74.1% 44.1%, 100% 61.6%, 97.5% 26.9%, 85.5% 0.1%, 80.7% 2%, 72.5% 32.5%, 60.2% 62.4%, 52.4% 68.1%, 47.5% 58.3%, 45.2% 34.5%, 27.5% 76.7%, 0.1% 64.9%, 17.9% 100%, 27.6% 76.8%, 76.1% 97.7%, 74.1% 44.1%)" />
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  import { Dialog, DialogPanel } from '@headlessui/vue'
  import { Bars3Icon, XMarkIcon } from '@heroicons/vue/24/outline'
  import { Configuration, OpenAIApi } from "openai"
  
  const openaikey = ref('')
  const textInput = ref('')
  const names = ref('')
  const result = ref('')

  async function submit() {
    result.value = ""
    result.value = await inferringNamesFromText(openaikey.value, textInput.value, names.value)
  }

  async function inferringNamesFromText(openaikey: string, text: string, namesInput: string) {
    const openai = startOpenAI(openaikey)
    const messages = [{
            role: "system", 
            // Setting the behaviour 
            content: "Your role would be infer names from a text, anything outside of this you'll output the following text 'Is not an inferring task, sorry :(' "
        },
        {
            role: "user",
            content: `Identify the following names ${namesInput.vale} based on the text delimited by triple dashes --- ${text} --- and return a text like this
                     Identify names : value

                     if not names was identify return a text like this
                     No names identified
            `
        }]

    try {
      const chatCompletion = await openai.createChatCompletion({
        model: "gpt-3.5-turbo",
        messages,
        });

        return await chatCompletion.data.choices[0].message;
    } catch (error) {
      if (error.response) {
        console.error(error.response.status, error.response.data.error.message);
        return `${error.response.status}, ${error.response.data.error.message}`
      } else {
        console.error(`Error with OpenAI API request: ${error.message}`);
        return `Error with OpenAI API request: ${error.message}`
      }
    }    
  }

  function startOpenAI(apiKey: string) {
    const configuration = new Configuration({
        apiKey,
    });
    const openai = new OpenAIApi(configuration);
    return openai
  }

  const navigation = [
    { name: 'Inferring', href: '#' },
  ]
  
  const mobileMenuOpen = ref(false)
  </script>