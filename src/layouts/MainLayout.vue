<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-black">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          ViteLiteMetaSyndicate
        </q-toolbar-title>

        <q-btn class="glossy bg-grey-1 text-dark text-bold" dense rounded label="Connect Wallet" push />
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
          Essential Links
        </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
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

<script>
import { defineComponent, ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'

const linksList = [
    {
        title: 'Road Map',
        caption: 'road map',
        icon: 'rss_feed',
        link: '#road_map'
    },
    {
        title: 'Nft World',
        caption: 'Nft',
        icon: 'rss_feed',
        link: '#nft_world'
    },
    {
        title: 'Our Kingdom',
        caption: '',
        icon: 'rss_feed',
        link: '#our_kingdom'
    },
    {
        title: 'Why Us',
        caption: 'Reason to choose us',
        icon: 'rss_feed',
        link: '#why_us'
    },
    {
        title: 'Team',
        caption: 'Our Team',
        icon: 'rss_feed',
        link: '#team'
    },
    {
        title: 'Faq',
        caption: 'Frequent Questions',
        icon: 'rss_feed',
        link: '#faq'
    },
]

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  setup () {
    const leftDrawerOpen = ref(false)

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
})
</script>
