<template>
  <q-expansion-item
    v-if="submenu"
    :icon="icon"
    :label="title"
    class="menu-title"
    :caption="caption"
  >
    <q-item
      v-for="item in submenu"
      clickable
      :href="item?.link"
      style="padding-left: 4.5rem;"
    >
      <q-item-section
        v-if="item?.icon"
        avatar
        class=""
      >
        <q-icon :name="item?.icon" />
      </q-item-section>

      <q-item-section>
        <q-item-label class="submenu-title">{{ item?.title }}</q-item-label>
        <q-item-label caption>{{ item?.caption }}</q-item-label>
      </q-item-section>
    </q-item>
  </q-expansion-item>
  <q-item
    v-else
    clickable
    :href="link"
  >
    <q-item-section
      v-if="icon"
      avatar
      class="menu-icon"
    >
      <q-icon :name="icon" />
    </q-item-section>

    <q-item-section>
      <q-item-label class="menu-title">{{ title }}</q-item-label>
      <q-item-label caption>{{ caption }}</q-item-label>
    </q-item-section>
  </q-item>
</template>

<script setup lang="ts">
import { ref } from 'vue';

export interface EssentialLinkProps {
  title: string;
  caption?: string;
  link?: string;
  icon?: string;
  submenu?: Array<EssentialLinkProps>;
};

withDefaults(defineProps<EssentialLinkProps>(), {
  caption: '',
  link: '#',
  icon: '',
});
const isDark = ref(localStorage.getItem('theme') === 'dark')

</script>

<style lang="scss">

.q-icon {
  color: $primary !important;
}

.menu-title {
  font-weight: 500;
}

.submenu-title {
  font-weight: 400;
}
</style>
