<template>
  <v-slide-y-reverse-transition>
    <div v-if="props.show" class="drawer-toggle">
      <v-btn variant="plain" @click.stop="visible = !visible">About</v-btn>
    </div>
  </v-slide-y-reverse-transition>

  <v-navigation-drawer v-model="visible" location="right" class="pa-5" width="350" temporary>
    <div class="d-flex flex-column justify-center w-100 h-100">
      <v-spacer />

      <div class="mb-10">
        <v-img src="/android-chrome-192x192.png" max-height="256" />
      </div>

      <p class="title text-overline mb-10">Criminal Clock</p>

      <p class="text-center font-weight-light">Inspired by the show</p>
      <strong class="text-center text-subtitle-1 mb-10">Criminal UK/Germany/Spain/France</strong>

      <v-menu v-model="colorMenu" location="start" :close-on-content-click="false" contained>
        <template #activator="{ props: menuProps }">
          <v-btn color="primary" v-bind="menuProps"> Change Color </v-btn>
        </template>

        <v-card class="pa-3" border>
          <v-color-picker v-model="theme.current.value.colors.primary" :modes="['hex']" mode="hex" />
          <v-btn variant="tonal" block @click="resetColor">Reset</v-btn>
        </v-card>
      </v-menu>

      <v-spacer />

      <p class="d-flex align-center justify-center text-center mb-1">
        Made with
        <Heart />
      </p>

      <div class="credits">
        <p>Baran D.</p>
        <span>|</span>
        <a
          href="https://fledra.dev"
          target="_blank"
          rel="noopener noreferrer"
          class="text-decoration-none text-primary font-weight-bold"
        >
          fledra.dev
        </a>
      </div>

      <v-btn variant="tonal" @click="hide">Close</v-btn>
    </div>
  </v-navigation-drawer>
</template>

<script setup lang="ts">
const props = defineProps<{ show: boolean }>();

const theme = useTheme();
const color = useStorage('cc-color', '#ED0104', localStorage, { mergeDefaults: true });

const visible = ref(false);
const colorMenu = ref(false);

function hide() {
  visible.value = false;
  colorMenu.value = false;
}

function resetColor() {
  theme.themes.value.dark.colors.primary = '#ED0104';
  theme.themes.value.light.colors.primary = '#ED0104';
}

whenever(() => !props.show, hide);

watchDebounced(
  () => theme.current.value.colors.primary,
  (newColor) => {
    color.value = newColor;
  },
  { debounce: 500, maxWait: 2000 },
);
</script>

<style lang="scss" scoped>
.drawer-toggle {
  display: inline-flex;
  align-items: center;
  position: absolute;
  bottom: 1.5rem;
  right: 1rem;
}

.title {
  text-align: center;
  font-size: 1.5rem !important;
}

.credits {
  display: inline-flex;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
</style>
