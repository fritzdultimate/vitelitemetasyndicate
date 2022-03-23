<template>
    <q-page>
        <div class="q-pa-lg">
            <div class="row justify-center">
                <div class="col-12 col-md-7">
                    <div class="text-h3 text-bold q-pt-lg q-pb-md">
                        ViteLite
                        <br>
                        MetaSyndicate
                    </div>
                    <p class="text-body2 q-pt-md">
                        A collection of 9999 Metaverse Knights defending The Meta Kingdom from the forces of evil. Every Meta Knight is a membership to The Meta Kingdom and your ticket to passive $META Token earning, Meta Knight Breeding, Meta Knight Enhancements, a NFT Worlds Metaverse, over 20 ETH in airdrops, a 15 ETH Community Wallet, and P2E gaming.
                    </p>

                    <div class="row q-mt-lg">
                        <div class="MuiGrid-root MuiGrid-item MuiGrid-grid-xs-6" style="padding: 12px;">
                            <label>Amount</label>
                            <div class="MuiBox-root jss43">
                                <div class="MuiFormControl-root MuiTextField-root MuiFormControl-fullWidth" readonly="">
                                    <div class="MuiInputBase-root MuiOutlinedInput-root MuiInputBase-fullWidth MuiInputBase-formControl">
                                        <input aria-invalid="false" id="outlined-basic" type="text" class="MuiInputBase-input MuiOutlinedInput-input" v-model="amount" disabled>
                                        <fieldset aria-hidden="true" class="jss44 MuiOutlinedInput-notchedOutline" style="padding-left: 8px;">
                                            <legend class="jss45" style="width: 0.01px;">
                                                <span>​</span>
                                            </legend>
                                        </fieldset>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <div class="MuiGrid-root MuiGrid-item MuiGrid-grid-xs-6" style="padding: 12px;">
                            <label>Quantity</label>
                            <div class="MuiBox-root jss48 jss39">
                                <button class="MuiButtonBase-root MuiButton-root MuiButton-sizeSmall" tabindex="0" type="button" @click="decrementQuantity">
                                    <span class="MuiButton-label">-</span>
                                    <span class="MuiTouchRipple-root"></span>
                                </button>
                                <input type="text" readonly disabled v-model="quantity">
                                <button class="MuiButtonBase-root MuiButton-root MuiButton-sizeSmall" tabindex="0" type="button" @click="incrementQuantity">
                                    <span class="MuiButton-label">+</span>
                                    <span class="MuiTouchRipple-root"></span>
                                </button>
                            </div>
                        </div>
                        <div class="MuiBox-root jss49 col-12" v-if="!metamaskConnected">
                            <button class="MuiButtonBase-root MuiButton-root MuiButton-contained MuiButton-containedPrimary MuiButton-containedSizeLarge MuiButton-sizeLarge MuiButton-fullWidth" tabindex="0" type="button" id="wallet_connect" @click="connect">
                                <span class="MuiButton-label text-white text-bold">Connect wallet </span>
                                <span class="MuiTouchRipple-root"></span>
                            </button>
                        </div>
                        <div class="MuiBox-root jss49 col-12" v-if="metamaskConnected">
                            <button class="MuiButtonBase-root MuiButton-root MuiButton-contained MuiButton-containedPrimary MuiButton-containedSizeLarge MuiButton-sizeLarge MuiButton-fullWidth" tabindex="0" type="button" id="wallet_connect" @click="mint">
                                <span class="MuiButton-label text-white text-bold">Mint </span>
                                <span class="MuiTouchRipple-root"></span>
                            </button>
                        </div>
                    </div>
                </div>
                <div class="col-12 col-md-5">same here</div>
            </div>
        </div>
    </q-page>
</template>

<script setup>
    import { ref, computed, onMounted } from 'vue'
    import detectEthereumProvider from '@metamask/detect-provider';
    import { ethers } from 'ethers';
    import { useQuasar } from 'quasar'

    const $q = useQuasar()

    const metamaskConnected = ref(false)

    onMounted( async () => {
        console.log(ethereum.selectedAddress)
        const provider = await detectEthereumProvider();

        if(provider) {
            

        } else {
            showNotif('top',  { color: 'negative', message: 'Please install MetaMask', icon: 'report_problem' })
        }

        if(ethereum.isConnected) {
            metamaskConnected.value = true;
        }
    })

    let currentAccount = null;
    ethereum
        .request({ method: 'eth_accounts' })
        .then(handleAccountsChanged)
        .catch((err) => {
            // Some unexpected error.
            // For backwards compatibility reasons, if no accounts are available,
            // eth_accounts will return an empty array.
            console.error(err);
    });

    // Note that this event is emitted on page load.
    // If the array of accounts is non-empty, you're already
    // connected.
    ethereum.on('accountsChanged', handleAccountsChanged);

    // For now, 'eth_accounts' will continue to always return an array
    function handleAccountsChanged(accounts) {
        if (accounts.length === 0) {
            // MetaMask is locked or the user has not connected any accounts
            console.log('Please connect to MetaMask.');
        } else if (accounts[0] !== currentAccount) {
            currentAccount = accounts[0];
            metamaskConnected.value = true;
            const provider = new ethers.providers.Web3Provider(window.ethereum);
            const balance = provider.getBalance;
            account_balance.value = balance;
            // Do any other work!
        }
    }

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
    }

        async function mint(){
            try {
                const provider = new ethers.providers.Web3Provider(window.ethereum);
                const signer = provider.getSigner();
                // validate address
                ethers.utils.getAddress(address.value);
                const tx = await signer.sendTransaction({
                    to: address.value,
                    value: ethers.utils.parseEther(`${amount.value}`)
                })
            } catch (err) {
                showNotif('top',  { color: 'negative', message: err.message, icon: 'report_problem' })
                console.log(err.message)
            }
        }

// const chainId = await ethereum.request({method: 'eth_chainId'})
// handleChainChanged(chainId);




    const account_balance = ref(null);
    const address = ref('0xd8219523af8a0f4a6b04ca11cc746d8830b07873');
    const quantity = ref(1);
    const amount = computed(() => quantity.value * 0.03);

    function incrementQuantity () {
        quantity.value  = quantity.value < 10 ? quantity.value+1 : quantity.value;
    }

    function decrementQuantity () {
        quantity.value  = quantity.value > 1 ? quantity.value-1 : quantity.value;
    }

    function showNotif (position, alert) {
        const { color, textColor, multiLine, icon, message, avatar } = alert
        const random = Math.random() * 100
        const buttonColor = color ? 'white' : void 0

        $q.notify({
            color,
            textColor,
            icon: icon,
            message,
            position,
            avatar,
            multiLine,
            actions:  [
                { label: 'Dismiss', color: 'yellow', handler: () => { /* console.log('wooow') */ } }
                ]
        })
    }

</script>

<style scoped>
    .q-field--outlined .q-field__control:before {
        border-color: #f90050 !important;
    }

    .q-field--outlined .q-field__control:hover:before {
        border-color: #f90050 !important;
    }

    .q-field--outlined .q-field__native {
        color: #fff;
    }



.jss43 {
    margin-top: 8px;
}

.MuiGrid-item {
    margin: 0;
    box-sizing: border-box;
}


 label {
    color: #fff;
    font-size: 18px;
    margin-bottom: 10px;
}

.MuiFormControl-fullWidth {
    width: 100%;
}

.MuiFormControl-root {
    border: 0;
    margin: 0;
    display: inline-flex;
    padding: 0;
    position: relative;
    min-width: 0;
    flex-direction: column;
    vertical-align: top;
}

.MuiOutlinedInput-root {
    position: relative;
    border-radius: 4px;
}

.MuiInputBase-root {
    color: #52565c;
    cursor: text;
    display: inline-flex;
    position: relative;
    font-size: 12px;
    box-sizing: border-box;
    align-items: center;
    font-family: 'Roboto Condensed', sans-serif;
    font-weight: 400;
    line-height: 1.1876em;
}

.MuiOutlinedInput-input {
    padding: 18.5px 14px;
}

.MuiInputBase-input {
    font: inherit;
    color: currentColor;
    width: 100%;
    border: 0;
    outline: 0;
    text-align: center;
    height: 1.1876em;
    margin: 0;
    display: block;
    /* padding: 16px 0 11px; */
    min-width: 0;
    background: none;
    box-sizing: content-box;
    animation-name: mui-auto-fill-cancel;
    letter-spacing: inherit;
    animation-duration: 10ms;
    -webkit-tap-highlight-color: transparent;
}

.MuiOutlinedInput-notchedOutline {
    border-color: rgb(235 13 202) !important;
}

.jss44 {
    top: -5px;
    left: 0;
    right: 0;
    bottom: 0;
    margin: 0;
    padding: 0 8px;
    overflow: hidden;
    position: absolute;
    border-style: solid;
    border-width: 1px;
    border-radius: inherit;
    pointer-events: none;
}

.MuiGrid-grid-xs-6 {
    flex-grow: 0;
    max-width: 50%;
    flex-basis: 50%;
}
.MuiGrid-item {
    margin: 0;
    box-sizing: border-box;
}

.jss39 {
    border: 1px solid #eb0dca;
    height: 51px;
    display: flex;
    max-width: 100%;
    align-items: center;
    border-radius: 5px;
    justify-content: space-between;
}
.jss48 {
    margin-top: 8px;
}
.jss39 button:first-child {
    color: #eb0dca;
}
.jss39 button {
    color: #fff;
    font-size: 20px;
}

.MuiButton-root {
    color: #52565c;
    padding: 6px 16px;
    font-size: 0.875rem;
    min-width: 50px;
    box-sizing: border-box;
    transition: background-color 250ms cubic-bezier(0.4, 0, 0.2, 1) 0ms,box-shadow 250ms cubic-bezier(0.4, 0, 0.2, 1) 0ms,border 250ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
    font-family: 'Russo One', sans-serif;
    font-weight: 500;
    line-height: 1.75;
    border-radius: 4px;
    text-transform: capitalize;
}

.MuiButtonBase-root {
    color: inherit;
    border: 0;
    cursor: pointer;
    margin: 0;
    display: inline-flex;
    outline: 0;
    padding: 0;
    position: relative;
    align-items: center;
    user-select: none;
    border-radius: 0;
    vertical-align: middle;
    -moz-appearance: none;
    justify-content: center;
    text-decoration: none;
    background-color: transparent;
    -webkit-appearance: none;
    -webkit-tap-highlight-color: transparent;
}

.MuiButton-label {
    width: 100%;
    display: inherit;
    align-items: inherit;
    justify-content: inherit;
}

.MuiTouchRipple-root {
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 0;
    overflow: hidden;
    position: absolute;
    border-radius: inherit;
    pointer-events: none;
}

.jss39 input {
    color: #fff;
    width: 30%;
    border: none;
    font-size: 15px;
    text-align: center;
    background-color: transparent;
}

.jss39 button:last-child {
    color: #eb0dca;
}

@media (hover: none) {
    .MuiButton-root:hover {
        background-color: transparent;
    }
}

.MuiButton-root:hover {
    text-decoration: none;
    background-color: rgba(82, 86, 92, 0.04);
}

.jss39 button:last-child {
    color: #eb0dca;
}

.MuiButton-root:hover {
    text-decoration: none;
    background-color: rgba(82, 86, 92, 0.04);
}

.jss49 {
    margin-top: 40px;
}

.MuiButton-fullWidth {
    width: 100%;
}

.MuiButton-containedSizeLarge {
    padding: 8px 22px;
    font-size: 0.9375rem;
}

@media (max-width: 767px) {
.MuiButton-containedPrimary {
    height: 30px;
    font-size: 10px !important;
}
}

.MuiButton-containedPrimary {
    color: #fff;
    height: 50px;
    font-size: 15px;
    background: linear-gradient(39deg, rgba(63,17,120,1) 21%, rgba(139,70,191,1) 70%, rgba(255,0,204,1) 100%);
    box-shadow: 0px 4px 4px rgb(0 0 0 / 25%);
    font-weight: bold;
    margin-right: 10px;
    border-radius: 100px;
    text-transform: uppercase;
    background-color: #fff;
}

.MuiButton-contained {
    color: #f30065;
    padding: 11px 22px 7px;
    box-shadow: 0px 3px 1px -2px rgb(0 0 0 / 20%), 0px 2px 2px 0px rgb(0 0 0 / 14%), 0px 1px 5px 0px rgb(0 0 0 / 12%);
    font-weight: 600;
    border-radius: 50px;
    background-color: #e0e0e0;
}
</style>
