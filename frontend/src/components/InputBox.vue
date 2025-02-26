<script lang="ts" setup>
import { ref } from 'vue'
import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'
import { PencilIcon } from '@heroicons/vue/24/outline'

const props = defineProps({
  title: {
    type: String,
    required: true,
  },
  message: {
    type: String,
    required: true,
  },
  confirmMessage: {
    type: String,
    required: false,
    default: 'Confirm',
  },
  cancelMessage: {
    type: String,
    required: false,
    default: 'Cancel',
  },
  initial: {
    type: String,
    required: false,
    default: '',
  },
})

const input = ref()

const value = ref(props.initial)
const emit = defineEmits(['cancel', 'confirm'])

function focus($event: Event) {
  if (input.value === props.initial) {
    input.value.selectionEnd = props.initial.length
    $event.preventDefault()
  }
}
</script>

<template>
  <TransitionRoot as="template" :show="true">
    <Dialog as="div" class="relative z-10" @close="emit('cancel')">
      <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100"
                       leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity dark:bg-gray-600"/>
      </TransitionChild>

      <div class="fixed inset-0 z-10 overflow-y-auto">
        <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
          <TransitionChild as="template" enter="ease-out duration-300"
                           enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                           enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200"
                           leave-from="opacity-100 translate-y-0 sm:scale-100"
                           leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
            <DialogPanel
                class="relative transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left shadow-xl transition-all dark:bg-gray-700 sm:my-8 sm:w-full sm:max-w-lg sm:p-6">
              <div class="sm:flex sm:items-start">
                <div
                    class="mx-auto flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-frost-4 sm:mx-0 sm:h-10 sm:w-10">
                  <PencilIcon class="h-6 w-6 text-frost-2" aria-hidden="true"/>
                </div>
                <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left w-full">
                  <DialogTitle as="h3" class="text-lg font-medium leading-6 text-gray-900 dark:text-snow-storm-1">
                    {{ title }}
                  </DialogTitle>
                  <div class="mt-2">
                    <p class="text-sm text-gray-500 dark:text-snow-storm-2">{{ message }}</p>
                  </div>
                  <div class="mt-2">
                    <input ref="input" autofocus autocomplete="off" autocapitalize="off" spellcheck="false" type="text"
                           v-model="value" @focus="focus"
                           class="w-full rounded-md border-gray-300 bg-polar-night-4 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 dark:border-gray-600"/>
                  </div>
                </div>
              </div>
              <div class="mt-5 sm:mt-4 sm:flex sm:flex-row-reverse">
                <button type="button"
                        class="inline-flex w-full justify-center rounded-md bg-aurora-4 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-aurora-5 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 sm:ml-3 sm:w-auto sm:text-sm"
                        @click="emit('confirm', value)">
                  {{ confirmMessage }}
                </button>
                <button type="button"
                        class="mt-3 inline-flex w-full justify-center rounded-md bg-aurora-1 px-4 py-2 text-base font-medium text-gray-700 shadow-sm hover:bg-aurora-5 focus:outline-none focus:ring-offset-2 dark:text-snow-storm-1 sm:mt-0 sm:w-auto sm:text-sm"
                        @click="emit('cancel')">
                  {{ cancelMessage }}
                </button>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>
