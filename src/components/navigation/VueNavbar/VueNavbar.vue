<template>
  <vue-box padding="0" align-y="center" :class="$style.vueNavBar" as="nav">
    <vue-content-block>
      <vue-columns space="0" :class="$style.layout" align-y="center">
        <vue-column width="content" align-y="center" no-grow>
          <vue-icon-menu
            v-if="showMenuIcon"
            tabindex="0"
            :class="$style.menu"
            data-testid="menu"
            @click.stop.prevent="/* c8 ignore start */ $emit('menu-click', $event) /* c8 ignore end */"
          />
        </vue-column>

        <vue-column align-x="center">
          <slot name="center" />
        </vue-column>

        <vue-column v-if="userName || userImage" no-grow>
          <vue-dropdown
            :items="[
              { label: 'Profile', value: 'profile', leadingIcon: 'user' },
              { label: 'Settings', value: 'settings', leadingIcon: 'cog' },
              { label: '', value: 'separator' },
              { label: 'Logout', value: 'logout', leadingIcon: 'logout' },
            ]"
            :duration="dropdownDuration"
            @item-click="$emit('menu-item-click', $event)"
          >
            <vue-avatar :name="userName" :src="userImage" size="sm" />
          </vue-dropdown>
        </vue-column>

        <vue-column v-else no-grow>
          <slot name="right" />
        </vue-column>
      </vue-columns>
    </vue-content-block>
  </vue-box>
</template>

<script setup lang="ts">
import { useCssModule } from 'vue';
import VueBox from '~/components/layout/VueBox/VueBox.vue';
import VueColumn from '~/components/layout/VueColumns/VueColumn/VueColumn.vue';
import VueColumns from '~/components/layout/VueColumns/VueColumns.vue';
import VueIconMenu from '~/components/icons/VueIconMenu.vue';
import VueAvatar from '~/components/data-display/VueAvatar/VueAvatar.vue';
import VueDropdown from '~/components/input-and-actions/VueDropdown/VueDropdown.vue';
import VueContentBlock from '~/components/layout/VueContentBlock/VueContentBlock.vue';
import type { IItem } from '~/interfaces/IItem';

// Interface

defineProps({
  userName: { type: String, default: null },
  userImage: { type: String, default: null },
  showMenuIcon: { type: Boolean, default: true },
  dropdownDuration: { type: Number, default: 250 },
});
defineEmits<{
  (event: 'menu-click', e: MouseEvent): void;
  (event: 'menu-item-click', item: IItem): void;
}>();

const $style = useCssModule();
</script>

<style lang="scss" module>
@import 'assets/_design-system.scss';

.vueNavBar {
  height: $navbar-height;
  background: $navbar-bg;
  z-index: $navbar-index;
  box-shadow: $navbar-elevation;
  border-bottom: $navbar-border;

  .layout {
    height: 100%;

    .menu {
      width: $navbar-menu-icon-size;
      height: $navbar-menu-icon-size;
      outline: none;
      cursor: pointer;
      border-radius: 2px;

      &:focus {
        box-shadow: $navbar-menu-outline;
      }
    }
  }
}
</style>
