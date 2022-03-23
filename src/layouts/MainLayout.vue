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

        <q-btn v-if="!connected" class="glossy bg-grey-1 text-dark text-bold" dense rounded label="Connect Wallet" push  @click="connect"/>

         <q-badge rounded color="primary" :label="balance + ' Eth'" />
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
import { ethers } from 'ethers';

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
    let connected = ref(false);
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const balance = ref(0)
    // const balance = await provider.getBalance(ethereum.selectedAddress);
    // console.log(balance)
    if(ethereum.isConnected) {
        connected.value = true;
    }

    async function getBalance() {
        const balance_ = await provider.getBalance(ethereum.selectedAddress);
        let eth = ethers.utils.formatEther(balance_._hex)
        balance.value = eth;
        console.log( eth );
    }

    getBalance();
    async function connect() {
        const provider = await detectEthereumProvider();
        if(!provider) {
            showNotif('top',  { color: 'negative', message: 'Please install MetaMask!', icon: 'report_problem' })
        }

        try {
            ethereum
            .request({ method: 'eth_requestAccounts' })
            .then(handleAccountsChanged)
            .catch((err) => {
            if (err.code === 4001) {
                // EIP-1193 userRejectedRequest error
                // If this happens, the user rejected the connection request.
                showNotif('top',  { color: 'negative', message: 'Please connect to MetaMask!', icon: 'report_problem' })
            } else {
                console.error(err);
                showNotif('top',  { color: 'negative', message: err.message, icon: 'report_problem' })
            }
        });
        } catch (err) {
            console.log(err)
            showNotif('top',  { color: 'negative', message: err.message, icon: 'report_problem' })
        }

        connected.value = true;
    }

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      },
      connect,
      connected,
      balance
    }
  }
})
</script>
