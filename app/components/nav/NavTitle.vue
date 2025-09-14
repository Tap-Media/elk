<script setup lang="ts">
const { env } = useBuildInfo()
const router = useRouter()
const back = ref<any>('')

const nuxtApp = useNuxtApp()

function onClickLogo() {
  nuxtApp.hooks.callHook('elk-logo:click')
}

onMounted(() => {
  back.value = router.options.history.state.back
})
router.afterEach(() => {
  back.value = router.options.history.state.back
})
</script>

<template>
  <div
    class="relative flex items-center justify-between sticky top-0 bg-base z-1 py-4 native:py-7"
    data-tauri-drag-region
  >
    <!-- Bên trái: placeholder để đối xứng -->
    <div class="w-[60px]"></div>

    <!-- Logo chính giữa: tuyệt đối -->
    <NuxtLink
      class="absolute left-1/2 -translate-x-[calc(50%+70px)] flex items-center justify-center py-2 px-5 text-2xl select-none focus-visible:ring-2 focus-visible:ring-current bg-gray-100 hover:bg-gray-200 rounded-md transition-colors"
      to="/home"
      @click.prevent="onClickLogo"
    >
      <NavLogo shrink-0 aspect="1/1" sm:h-8 xl:h-10 class="rtl-flip" />
    </NuxtLink>

    <!-- Mũi tên back nằm xa bên phải -->
    <div v-if="$route.name !== 'tag'" class="flex items-center pe-4 xl:pe-6">
      <CommonTooltip :content="$t('nav.back')" :distance="0">
        <button
          type="button"
          :aria-label="$t('nav.back')"
          class="btn-text p-3"
          :class="{ 'pointer-events-none op0': !back || back === '/' }"
          @click="$router.go(-1)"
        >
          <div class="text-xl i-ri:arrow-left-line rtl-flip" />
        </button>
      </CommonTooltip>
    </div>
  </div>
</template>


