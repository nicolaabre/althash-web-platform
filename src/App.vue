<template>
  <v-app light>
    <v-toolbar :class="headerClass" app fixed height="82px" clipped-left>
      <v-spacer></v-spacer>
      <v-menu 
        bottom open-on-hover offset-y
        :nudge-width="100"
      >
        <v-btn large icon slot="activator">
          <v-icon large color="blue darken-4">apps</v-icon>
        </v-btn>
          <v-toolbar color="light-blue darken-4" dark height="40px">
            <span class="title bold">
              DAPPS
            </span>
          </v-toolbar>
          <v-card>
            <v-card-text>
              <v-container fluid grid-list-md>
                <v-layout row wrap align-center text-xs-center>
                  <v-flex xs4>
                    <v-btn icon large dark slot="activator" @click="openURL('https://health.althash.org')">
                      <img src="~assets/images/althash_health_menu_btn.png" style="height:50px;width:auto;">
                    </v-btn>
                  </v-flex>
                  <v-flex xs4>
                    <v-btn icon large dark slot="activator" @click="openDapp('dapp_myoffspring')">
                      <img src="~assets/images/myoffspring_menu_btn.png" style="height:50px;width:auto;">
                    </v-btn>
                  </v-flex>
                  <v-flex xs4>
                    <v-btn icon large dark slot="activator" @click="openDapp('create_token')">
                      <img src="~assets/images/tokenfarm_menu_btn.png" style="height:50px;width:auto;">
                    </v-btn>
                  </v-flex>
                  <v-flex xs4>
                    <v-btn icon large dark slot="activator" @click="openDapp('dapp_altproof')">
                      <img src="~assets/images/altproof_menu_btn.png" style="height:50px;width:auto;">
                    </v-btn>
                  </v-flex>
                  <v-flex xs4>
                    <v-btn icon large dark slot="activator" @click="openDapp('dapp_crypticmag')">
                      <img src="~assets/images/crypticmag_menu_btn.png" style="height:50px;width:auto;">
                    </v-btn>
                  </v-flex>
                  <v-flex xs4>
                    <v-btn icon large dark slot="activator" @click="openDapp('dapp_biffyplutonium')">
                      <img src="~assets/images/BIFP_menu_black_btn.png" style="height:50px;width:auto;">
                    </v-btn>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-card-text>
          </v-card>
        </v-card>
      </v-menu>
      <v-spacer></v-spacer>
      <span class="title">
        <span class="text"><img src="~assets/images/logo_althash_webplatform.png" alt="Althash Web Platform Logo" class="cursor" @click="changeView('home')"></span>
      </span>
      <v-spacer></v-spacer>
      <v-menu bottom open-on-hover offset-y
              :nudge-width="100"
              v-if="!this.wallet"
      >
        <v-btn large icon slot="activator">
          <v-icon large color="blue darken-4">open_in_browser</v-icon>
        </v-btn>
        <v-list>
          <v-list-tile
            v-for="loginItem in loginItems"
            v-show="!notShow[loginItem.name]"
            :key="loginItem.title"
            @click="changeView(loginItem.name)"
           >
             <v-list-tile-content>
               <v-list-tile-title class="body-2">{{ loginItem.title }}</v-list-tile-title>
             </v-list-tile-content>
             <v-list-tile-action>
               <v-icon color="blue darken-4">{{ loginItem.action }}</v-icon>
             </v-list-tile-action>
           </v-list-tile>
           <v-divider></v-divider>
           <v-list-tile
            v-show="!notShow['safe_send']"
            @click="changeView('safe_send')"
           >
             <v-list-tile-content>
               <v-list-tile-title class="body-2">Safe Send</v-list-tile-title>
             </v-list-tile-content>
             <v-list-tile-action>
               <v-icon color="blue darken-4">security</v-icon>
             </v-list-tile-action>
           </v-list-tile>
        </v-list>
      </v-menu>
      <v-menu bottom open-on-hover offset-y
              :nudge-width="100"
              v-if="this.wallet"
      >
        <v-btn large icon slot="activator">
          <v-icon large color="blue darken-4">account_balance_wallet</v-icon>
        </v-btn>
        <v-toolbar color="light-blue darken-4" dark v-if="this.wallet">
          <center>
            <span class="caption cursor" @click="changeView('view')">
              <b>
                {{ this.wallet.info['address'] }}
              </b>
              <br>
                {{ this.wallet.info['balance'] }}
            </span>
          </center>
        </v-toolbar>
        <v-list>
          <v-list-tile
            v-for="walletItem in walletItems"
            v-show="!notShow[walletItem.name]"
            :key="walletItem.title"
            @click="changeView(walletItem.name)"
           >
             <v-list-tile-content>
               <v-list-tile-title class="body-2">{{ walletItem.title }}</v-list-tile-title>
             </v-list-tile-content>
             <v-list-tile-action>
               <v-icon color="blue darken-4">{{ walletItem.action }}</v-icon>
             </v-list-tile-action>
           </v-list-tile>
           <v-divider></v-divider>
           <v-list-tile
            v-for="smartContractItem in smartContractItems"
            v-show="!notShow[smartContractItem.name]"
            :key="smartContractItem.title"
            @click="changeView(smartContractItem.name)"
           >
             <v-list-tile-content>
               <v-list-tile-title class="body-2">{{ smartContractItem.title }}</v-list-tile-title>
             </v-list-tile-content>
             <v-list-tile-action>
               <v-icon color="blue darken-4">{{ smartContractItem.action }}</v-icon>
             </v-list-tile-action>
           </v-list-tile>
           <v-divider></v-divider>
           <v-list-tile @click="changeView('settings')">
            <v-list-tile-content>
              <v-list-tile-title class="body-2">Settings</v-list-tile-title>
            </v-list-tile-content>
            <v-list-tile-action>
              <v-icon color="blue darken-4">settings</v-icon>
            </v-list-tile-action>
           </v-list-tile>
           <v-divider></v-divider>
           <v-list-tile @click="logout_dialog = true">
            <v-list-tile-content>
              <v-list-tile-title class="body-2">Log Out</v-list-tile-title>
            </v-list-tile-content>
            <v-list-tile-action>
              <v-icon color="blue darken-4">power_settings_new</v-icon>
            </v-list-tile-action>
           </v-list-tile>
        </v-list>
      </v-menu>
      <v-spacer></v-spacer>  
    </v-toolbar>
    <main>
      <v-content>
        <v-container fluid fill-height justify-center>
          <v-layout row wrap>
            <v-flex xs10 offset-xs1>
              <home v-show="isCurrent['home']" @openDapp="openDapp" @openURL="openURL"></home>
              <create-wallet :view="isCurrent['create']" @created="setWallet" v-show="isCurrent['create']"></create-wallet>
              <create-mnemonic :view="isCurrent['create_from_mnemonic']" @created="setWallet" v-show="isCurrent['create_from_mnemonic']"></create-mnemonic>
              <restore-wallet @restored="setWallet" v-show="isCurrent['restore_from_mnemonic']"></restore-wallet>
              <restore-wif @restored="setWallet" v-show="isCurrent['restore_from_wif']"></restore-wif>
              <restore-mobile @restored="setWallet" v-show="isCurrent['restore_from_mobile']"></restore-mobile>
              <restore-key-file @restored="setWallet" v-show="isCurrent['restore_from_key_file']"></restore-key-file>
              <restore-ledger @restored="setWallet"  v-if="isCurrent['restore_from_ledger']"></restore-ledger>
              <view-wallet :view="isCurrent['view']" v-if="isCurrent['view']"></view-wallet>
              <view-tx :view="isCurrent['transactions']" v-if="isCurrent['transactions']"></view-tx>
              <safe-send @send="setWallet" v-if="isCurrent['safe_send']"></safe-send>
              <send @send="setWallet" v-if="isCurrent['send']"></send>
              <request-payment v-if="isCurrent['request_payment']"></request-payment>
              <dump-key-file v-if="isCurrent['dump_as_key_file']"></dump-key-file>
              <create-contract v-if="isCurrent['create_contract']"></create-contract>
              <send-to-contract v-if="isCurrent['send_to_contract']"></send-to-contract>
              <call-contract v-if="isCurrent['call_contract']"></call-contract>
              <create-token v-if="isCurrent['create_token']"></create-token>
              <dapp-myoffspring v-if="isCurrent['dapp_myoffspring']"></dapp-myoffspring>
              <dapp-crypticmag v-if="isCurrent['dapp_crypticmag']"></dapp-crypticmag>
              <dapp-alt-proof v-if="isCurrent['dapp_altproof']"></dapp-alt-proof>
              <dapp-biffy-plutonium v-if="isCurrent['dapp_biffyplutonium']"></dapp-biffy-plutonium>
              <config v-if="isCurrent['settings']"></config>
            </v-flex>
          </v-layout>
        </v-container>
      </v-content>
    </main>
    <v-dialog
      v-model="logout_dialog"
      max-width="290"
    >
      <v-card>
        <v-card-title class="headline">Log out?</v-card-title>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            color="red darken-1"
            flat="flat"
            @click="logout_dialog = false"
          >
            No
          </v-btn>

          <v-btn
            color="green darken-1"
            flat="flat"
            @click="logout"
          >
            Yes
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <notify></notify>
    <warning></warning>
  </v-app>
</template>

<style>
.cursor:hover {
  cursor: pointer;
}
</style>

<script>
import Vue from "vue";
import createLog from "localstorage-logger";
//Components
import Notify from "components/Notify";
import Warning from "components/Warning";
import Home from "controllers/Home";
import CreateWallet from "controllers/Create";
import CreateMnemonic from "controllers/CreateMnemonic";
import RestoreWallet from "controllers/Restore";
import RestoreWif from "controllers/RestoreWif";
import RestoreMobile from "controllers/RestoreMobile";
import RestoreKeyFile from "controllers/RestoreKeyFile";
import RestoreLedger from "controllers/RestoreLedger";
import ViewWallet from "controllers/View";
import ViewTx from "controllers/ViewTx";
import SafeSend from "controllers/SafeSend";
import Send from "controllers/Send";
import RequestPayment from "controllers/RequestPayment";
import DumpKeyFile from "controllers/DumpKeyFile";
import CreateToken from "controllers/CreateToken";
import DappMyoffspring from "controllers/Dapp-MyOffspring";
import DappCrypticmag from "controllers/Dapp-CrypticMAG";
import DappAltProof from "controllers/Dapp-AltProof";
import DappBiffyPlutonium from "controllers/Dapp-BiffyPlutonium";
import CreateContract from "controllers/CreateContract";
import SendToContract from "controllers/SendToContract.vue";
import CallContract from "controllers/CallContract.vue";
import Config from "controllers/Config";
import config from "libs/config";
import webWallet from "libs/web-wallet";
import i18n from "libs/i18n";
const log = createLog({
  maxLogSizeInBytes: 500 * 1024 // 500KB
});

export default {
  name: "app",
  i18n,
  data() {
    return {
      wallet: false,
      walletAddress: '',
      walletBalance: '',
      current: "home",
      logout_dialog: false,
      network: config.getNetwork(),
      mode: config.getMode(),
      log: log,
      notifyList: {},
      loginItems: [
        {
          title: "Restore from Key File",
          name: "restore_from_key_file",
          action: "cloud_upload"
        },
        {
          title: "Restore from Mnemonic",
          name: "restore_from_mnemonic",
          action: "sms"
        },
        {
          title: "Restore from WIF",
          name: "restore_from_wif",
          action: "create"
        },
        {
          title: "Restore from Mobile",
          name: "restore_from_mobile",
          action: "phonelink_lock"
        },
        {
          title: "Generate New Wallet",
          name: "create",
          action: "add"
        },
        {
          title: "Create from Mnemonic",
          name: "create_from_mnemonic",
          action: "assignment"
        }
      ],
      walletItems: [
        {
          title: "View Wallet Info",
          name: "view",
          action: "info"
        },
        {
          title: "View Wallet Txs",
          name: "transactions",
          action: "list"
        },
        {
          title: "Send",
          name: "send",
          action: "call_made"
        },
        {
          title: "Request Payment",
          name: "request_payment",
          action: "call_received"
        },
        {
          title: "Dump as Key File",
          name: "dump_as_key_file",
          action: "cloud_download"
        },
        {
          title: "Safe Send",
          name: "safe_send",
          action: "security"
        }
      ],
      smartContractItems: [         
        {
          title: "Send to Contract",
          name: "send_to_contract",
          action: "play_circle_outline"
        },
        {
          title: "Call Contract",
          name: "call_contract",
          action: "pageview"
        },
        {
          title: "Create Contract",
          name: "create_contract",
          action: "build"
        }
      ],
    };
  },
  computed: {
    isCurrent() {
      return { [this.current]: true };
    },
    notShow() {
      return {
        restore_from_ledger: this.network !== "mainnet",
        view: this.mode === "offline" || !this.wallet,
        transactions: this.mode === "offline" || !this.wallet,
        wallet: this.mode === "offline" && !this.wallet,
        safe_send: this.mode === "offline" && !this.wallet,
        send: this.mode === "offline" || !this.wallet,
        request_payment: !this.wallet,
        dump_as_key_file: !this.wallet || !this.wallet.getHasPrivKey(),
        create_token: this.mode === "offline" || !this.wallet,
        contract: this.mode === "offline" || !this.wallet,
        create_contract: this.mode === "offline" || !this.wallet,
        send_to_contract: this.mode === "offline" || !this.wallet,
        call_contract: this.mode === "offline" || !this.wallet,
        dapp_myoffspring: this.mode === "offline" || !this.wallet,
        dapp_crypticmag: this.mode === "offline" || !this.wallet,
        dapp_altproof: this.mode === "offline" || !this.wallet,
        dapp_biffyplutonium: this.mode === "offline" || !this.wallet,
        dapps: this.mode === "offline" || !this.wallet
      };
    },
    headerClass() {
      return this.mode === "normal"
        ? this.network === "mainnet"
          ? "blue-grey lighten-5"
          : "red darken-3"
        : "blue-grey darken-4";
    }
  },
  components: {
    Notify,
    Warning,
    Home,
    CreateWallet,
    CreateMnemonic,
    RestoreWallet,
    RestoreWif,
    RestoreMobile,
    RestoreKeyFile,
    RestoreLedger,
    ViewWallet,
    ViewTx,
    SafeSend,
    Send,
    RequestPayment,
    DumpKeyFile,
    CreateToken,
    DappMyoffspring,
    DappCrypticmag,
    DappAltProof,
    DappBiffyPlutonium,
    CreateContract,
    SendToContract,
    CallContract,
    Config
  },
  methods: {
    setWallet() {
      this.wallet = webWallet.getWallet();
      this.wallet.init();
      if (this.wallet) {
        if (this.mode === "offline") {
          this.current = "request_payment";
        } else {
          this.current = "home";
        }
      }
    },
    logout() {
      this.wallet = null;
      this.current = "home";
      this.logout_dialog = false;
    },
    changeView(name) {
      this.current = name;
    },
    openDapp(name) {
      if (this.mode === "offline" || !this.wallet) {
        alert(
          'You need to either import or create a new Wallet before using Dapps! Please go to the "Add Wallet" menu at the top-right corner of the page.'
        );
      } else {
        this.changeView(name);
      }
    },
    openURL(url) {
      window.open(url, "_blank");
    },
    error(msg, isHtml = false, ttl = 10) {
      this.addNotify(msg, "error", isHtml, ttl);
    },
    success(msg, isHtml = false, ttl = 10) {
      this.addNotify(msg, "success", isHtml, ttl);
    },
    addNotify(msg, type, isHtml = false, ttl = 10) {
      const notifyId = [msg, type].join("_");
      const notify = {
        msg: msg.split(" ").reduce((msg, current) => {
          let tmsg = this.$t("common.notify." + current);
          tmsg = tmsg === "common.notify." + current ? " " + current : tmsg;
          return msg + tmsg;
        }, ""),
        type,
        show: true,
        isHtml
      };
      if (this.notifyList[notifyId] && this.notifyList[notifyId].timer) {
        clearTimeout(this.notifyList[notifyId].timer);
      }
      Vue.set(this.notifyList, notifyId, notify);
      if (ttl > 0) {
        this.notifyList[notifyId].timer = setTimeout(() => {
          Vue.delete(this.notifyList, notifyId);
        }, ttl * 1000);
      }
    }
  }
};
</script>
