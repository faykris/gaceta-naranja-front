<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar :class="isDark ? 'toolbar-dark': 'toolbar-light'">
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title class="toolbar-title">
          La Gaceta
        </q-toolbar-title>

        <div>
          <!-- Quasar v{{ $q.version }} -->
          <q-toggle
            v-model="isDark"
            :checked-icon="isDark ? 'dark_mode' : 'light_mode'"
            :unchecked-icon="isDark ? 'dark_mode' : 'light_mode'"
          />
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Menú
        </q-item-label>

        <EssentialLink
          v-for="link in linksList"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { Dark, is } from 'quasar';
import EssentialLink, { type EssentialLinkProps } from 'components/EssentialLink.vue';
import { onMounted } from 'vue';

const linksList: EssentialLinkProps[] = [

  {
    title: 'Inicio',
    caption: 'Volver a la página principal',
    icon: 'home',
    link: '/'
  },
  {
    title: 'Noticias',
    caption: 'Artículos y más',
    icon: 'public',
    submenu: [
      {
        title: 'Política',
        caption: '',
        icon: '',
        link: '/test-editor'
      },
      {
        title: 'Opinión',
        caption: '',
        icon: '',
        link: 'https://github.com/quasarframework'
      },
      {
        title: 'Memes',
        caption: '',
        icon: '',
        link: '/test-editor'
      },
    ],
  },
  {
    title: 'Quienes somos',
    caption: 'Conozca más de este medio',
    icon: 'chat',
    link: 'https://chat.quasar.dev'
  },
  {
    title: 'Editor',
    caption: 'Administra tus artículos',
    icon: 'edit_document',
    link: 'https://quasar.dev',
    submenu: [
      {
        title: 'Crear nuevo artículo',
        caption: '',
        icon: '',
        link: '/test-editor'
      },
      {
        title: 'Editar artículo existente',
        caption: '',
        icon: '',
        link: 'https://github.com/quasarframework'
      },
    ],
  },
  {
    title: 'Contáctenos',
    caption: '',
    icon: 'record_voice_over',
    link: 'https://forum.quasar.dev'
  },
  {
    title: 'Donaciones',
    caption: '',
    icon: 'volunteer_activism',
    link: 'https://awesome.quasar.dev'
  }
];

const leftDrawerOpen = ref(false);
const isDark = ref(localStorage.getItem('theme') === 'dark') // Detecta si el tema oscuro está activo

onMounted(() => {
  // Carga el modo seleccionado previamente
  Dark.set(isDark.value)
})
  // Sincroniza el tema con los cambios del interruptor
watch(isDark, (val) => {
  localStorage.setItem('theme', val ? 'dark' : 'light')
  Dark.set(val)
})

function toggleLeftDrawer () {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
</script>

<style lang="scss">
.toolbar-dark {
  background-color: $dark;
  color: $light-page;
}
.toolbar-light {
  background-color: $light;
  color: $primary;
}
.toolbar-title {
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>
