<template>
  <nav class="sticky top-0 z-10">
    <div class="relative px-4 py-2 flex flex-row items-center justify-between gap-4 bg-white dark:bg-neutral-900">
      <button type="button" @click="emit('menu')" class="ml-px dark:text-white flex lg:hidden">
        <i class="ti ti-menu text-xl"></i>
      </button>
      <button
        class="inline-flex items-center space-x-1 text-white bg-neutral-900 dark:bg-white dark:text-black box-border border border-transparent hover:bg-neutral-800 focus:ring-4 focus:ring-blue-500 shadow-xs font-normal leading-5 rounded-xl text-[13px] px-4 py-1.5 focus:outline-none">
        <i class="ti ti-plus text-xl"></i>
        <span>{{ t('new') }}</span>
      </button>
      <DialogRoot>
        <DialogTrigger
          class="flex items-center space-x-2 text-md md:text-sm py-1.75 px-3 rounded-lg md:border border-muted md:bg-card hover:bg-muted text-muted-foreground min-w-xs">
          <div class="inline-flex flex-1 items-center">
            <svg class="size-4 mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <g stroke-linejoin="round" stroke-linecap="round" stroke-width="2.5" fill="none" stroke="currentColor">
                <circle cx="11" cy="11" r="8"></circle>
                <path d="m21 21-4.3-4.3"></path>
              </g>
            </svg>
            <span>{{ t('search') }}</span>
          </div>
          <span class="inline-flex text-xs">
            <kbd>⌘ K</kbd>
          </span>
        </DialogTrigger>
        <DialogPortal>
          <DialogOverlay class="bg-background/80 fixed inset-0 z-100" />
          <DialogContent
            class="fixed top-[10%] left-[50%] max-h-[85vh] w-[90vw] max-w-187.5 translate-x-[-50%] rounded-xl bg-card shadow-xl overflow-hidden focus:outline-none z-120 border border-muted">

            <div class="w-full px-6 flex items-center">
              <input type="text" placeholder="Search..."
                class="w-full h-12 bg-transparent text-sm focus:outline-none" />
              <DialogClose>
                <i class="ti ti-x text-md"></i>
              </DialogClose>
            </div>
            <div class="py-4 px-6 prose prose-stone dark:prose-invert text-sm hidden md:flex items-center gap-4"><span
                class="inline-flex items-center gap-1 leading-4">
                <kbd aria-label="Up arrow">
                  <i class="ti ti-arrow-up"></i>
                </kbd>
                <kbd aria-label="Down arrow">
                  <i class="ti ti-arrow-down"></i>
                </kbd> to navigate </span>
              <span class="inline-flex items-center gap-1 leading-4">
                <kbd aria-label="Enter"> enter </kbd> to select </span>
              <span class="inline-flex items-center gap-1 leading-4">
                <kbd aria-label="Escape">esc</kbd> to close </span>
            </div>
          </DialogContent>
        </DialogPortal>
      </DialogRoot>
      <DropdownMenuRoot>
        <DropdownMenuTrigger as-child>
          <button type="button"
            class="inline-flex items-center justify-center gap-2 rounded-xl bg-white text-sm size-8 font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2 dark:bg-neutral-800 dark:text-white dark:hover:bg-neutral-700">
            <span class="sr-only">Open user menu</span>
            <img class="h-full w-full rounded-lg object-cover" :src="app.user.avatar" :alt="app.user.name" />
          </button>
        </DropdownMenuTrigger>
        <DropdownMenuPortal>
          <DropdownMenuContent
            class="w-56 origin-top-right rounded-xl bg-white shadow-lg ring-1 ring-black/5 focus:outline-none flex flex-col justify-start shadow-gray-500 dark:bg-neutral-800 dark:text-white dark:shadow-none px-2 py-3">
            <div class="px-4 py-2">
              <div class="flex items-center gap-2">
                <p class="text-sm font-bold text-slate-700 dark:text-white">
                <div class="flex items-center gap-2">
                  <img class="h-8 w-8 rounded-xl" :src="app.user.avatar" :alt="app.user.name" />
                  <div class="flex flex-col">
                    <span class="text-md font-normal">{{ app.user.name }}</span>
                    <span class="text-[10px] font-normal text-slate-500 dark:text-white/70">
                      {{ app.user.email }}
                    </span>
                  </div>
                </div>
                </p>
              </div>
            </div>
            <div class="mt-3 h-px w-full bg-gray-200 dark:bg-white/20" />
            <div class="mt-3 ml-4 flex flex-col">
              <DropdownMenuItem @select="() => router.push('/settings')"
                class="text-sm no-underline text-gray-800 dark:text-white hover:dark:text-white">
                {{ t("settings") }}
              </DropdownMenuItem>
              <DropdownMenuItem @select="() => router.push('/account')"
                class="mt-3 text-sm no-underline text-gray-800 dark:text-white hover:dark:text-white">
                {{ t("manage_account") }}
              </DropdownMenuItem>
              <DropdownMenuItem as="button" @select="handleLogout"
                class="mt-3 text-sm text-left text-gray-800 dark:text-white hover:dark:text-white cursor-pointer">
                {{ t("log_out") }}
              </DropdownMenuItem>
              <DropdownMenuItem class="mt-3 flex items-center justify-between">
                <ColorModeButton />
              </DropdownMenuItem>
            </div>
          </DropdownMenuContent>
        </DropdownMenuPortal>
      </DropdownMenuRoot>
    </div>
  </nav>
</template>
<script lang="ts" setup>
import { useRoute } from 'vue-router'
import { useAppStore } from '~/stores/app'
import { useMobile } from '~/composables/useMobile'
import { DialogClose, DialogContent, DialogOverlay, DialogPortal, DialogRoot, DialogTrigger, DropdownMenuContent, DropdownMenuItem, DropdownMenuPortal, DropdownMenuRoot, DropdownMenuTrigger } from 'reka-ui'

const { t } = useI18n()
const router = useRouter()
const emit = defineEmits(['menu'])

const route = useRoute()
const app = useAppStore()
const mobile = useMobile(768)

const handleLogout = () => {
  app.logout()
  router.push('/login')
}
</script>
