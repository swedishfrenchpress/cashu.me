<template>
  <q-dialog
    v-model="showReceiveDialog"
    position="bottom"
    :full-width="$q.screen.lt.sm"
    :full-height="$q.screen.lt.sm"
    transition-show="slide-up"
    transition-hide="slide-down"
    backdrop-filter="blur(2px) brightness(60%)"
  >
    <q-card class="bg-card-bg text-white q-px-xs bottom-card q-pb-md">
      <q-card-section class="row items-center q-pb-none q-pt-md">
        <q-btn flat round dense v-close-popup class="q-ml-sm">
          <XIcon />
        </q-btn>
        <div class="col text-center">
          <span class="receive-title">Receive</span>
        </div>
        <q-btn flat round dense class="q-mr-sm">
          <ScanIcon />
        </q-btn>
      </q-card-section>

      <q-card-section class="q-pt-lg">
        <div class="q-gutter-y-md">
          <q-btn
            class="full-width custom-btn bg-card-btn-bg"
            @click="showReceiveTokensDialog"
          >
            <div class="row items-center full-width">
              <div class="icon-background q-mr-md">
                <BanknoteIcon />
              </div>
              <div class="text-left q-mt-xs">
                <div>ECASH</div>
              </div>
            </div>
          </q-btn>

          <q-btn
            class="full-width custom-btn bg-card-btn-bg"
            @click="showInvoiceCreateDialog"
          >
            <div class="row items-center full-width">
              <div class="icon-background q-mr-md">
                <ZapIcon />
              </div>
              <div class="text-left q-mt-xs">
                <div>LIGHTNING</div>
              </div>
            </div>
          </q-btn>
        </div>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script>
import { defineComponent } from "vue";
import { useReceiveTokensStore } from "src/stores/receiveTokensStore";
import { mapActions, mapState, mapWritableState } from "pinia";
import { useUiStore } from "src/stores/ui";
import { useWalletStore } from "src/stores/wallet";
import { X as XIcon, Banknote as BanknoteIcon, Zap as ZapIcon, Scan as ScanIcon } from 'lucide-vue-next';

export default defineComponent({
  name: "ReceiveDialog",
  components: {
    XIcon,
    BanknoteIcon,
    ZapIcon,
    ScanIcon,
  },
  mixins: [windowMixin],
  props: {},
  data: function () {
    return {
      currentPage: 1,
      pageSize: 5,
    };
  },
  computed: {
    ...mapWritableState(useUiStore, [
      "showInvoiceDetails",
      "showReceiveDialog",
    ]),
    ...mapWritableState(useReceiveTokensStore, [
      "showReceiveTokens",
      "receiveData",
    ]),
    ...mapWritableState(useWalletStore, ["invoiceData"]),
  },
  methods: {
    showReceiveTokensDialog: function () {
      this.receiveData.tokensBase64 = "";
      this.showReceiveTokens = true;
      this.showReceiveDialog = false;
    },
    showInvoiceCreateDialog: async function () {
      console.log("##### showInvoiceCreateDialog");
      this.invoiceData.amount = "";
      this.invoiceData.bolt11 = "";
      this.invoiceData.hash = "";
      this.invoiceData.memo = "";
      this.showInvoiceDetails = true;
      this.showReceiveDialog = false;
    },
  },
  created: function () {},
});
</script>

<style lang="scss" scoped>
.custom-btn {
  color: white;
  border-radius: 8px;
  height: 80px;
  box-shadow: none;
  font-size: 18px;
}

.full-width-card {
  width: 100%;
  max-width: 100%;
}

.q-dialog__inner--minimized > div {
  max-width: 100%;
}

.icon-background {
  background-color: $grey-10;
  border-radius: 8px;
  padding: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.lucide {
  width: 24px;
  height: 24px;
}

.qcard {
  border-top-left-radius: 16px;
  border-top-right-radius: 16px;
}

.q-card-section:first-child {
  padding-top: 16px;
}

.q-card-section:nth-child(2) {
  padding-top: 32px;
}

.bottom-card {
  border-top-left-radius: 16px;
  border-top-right-radius: 16px;
}

.receive-title {
  font-size: 22px;
  font-weight: bold;
}
</style>
