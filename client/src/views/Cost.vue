<script setup lang="ts">
import FormCost from '@/components/Cost/FormCost.vue'
import HistoryListCostVue from '@/components/Cost/HistoryListCost.vue'
import ChartCost from '@/components/Cost/ChartCost.vue'
import { ref } from 'vue'
import type MenuHeaderGreyTypes from '@/types/MenuHeaderGrey'
import MenuHeaderGreyVue from '@/components/Headers/MenuHeaderGrey/MenuHeaderGrey.vue'
import { CostStore } from '@/stores/Cost'
import { onMounted } from 'vue'

const selectedMenu = ref<string>('Cost')
onMounted(async () => {
  const costStore = CostStore()
  await costStore.getAllCosts()
})

const contentMenu = ref<MenuHeaderGreyTypes[]>([
  {
    text: 'Chart',
    icon: 'mdi mdi-chart-box-outline'
  },
  {
    text: 'Cost',
    icon: 'mdi mdi-wallet-plus-outline'
  },
  {
    text: 'History',
    icon: 'mdi mdi-history'
  }
])
</script>

<template>
  <div class="cost container">
    <transition-group name="listInfo">
      <MenuHeaderGreyVue
        v-model="selectedMenu"
        :contentMenu="contentMenu"
        :short="true"
        class="header__cost"
        :key="'MenuHeaderGreyVue'"
      />
      <FormCost v-if="selectedMenu == 'Cost'" :key="'FormCost'" />
      <HistoryListCostVue v-else-if="selectedMenu == 'History'" :key="'HistoryListCostVue'" />
      <ChartCost v-else-if="selectedMenu == 'Chart'" :key="'ChartCost'" />
    </transition-group>
  </div>
</template>

<style lang="scss">
.cost {
  max-width: 550px;
}
.btn {
  color: aliceblue;
}
.header__cost {
  margin-bottom: 20px;
}
</style>
