<template>
  <van-tabbar @change="onChange" v-model="activeTab" :safe-area-inset-bottom="true" :fixed="true">
    <van-tabbar-item :name="tabConstants.dashboard" @click="onChange(tabConstants.dashboard)">
      Home
      <template #icon="{ active }">
        <!--        <IconDeviceDesktopAnalytics />-->
        <app-icon :icon="TablerIconConstants.dashboard" :size="20" :stroke="getStrokeWidth(active)" />
      </template>
    </van-tabbar-item>

    <van-tabbar-item :name="tabConstants.transactionList" @click="onChange(tabConstants.transactionList)">
      Transactions
      <template #icon="{ active }">
        <app-icon :icon="TablerIconConstants.transaction" :size="20" :stroke="getStrokeWidth(active)" />
      </template>
    </van-tabbar-item>

    <!--    <van-tabbar-item :name="tabConstants.transactionTemplateList"-->
    <!--                     @click="onChange(tabConstants.transactionTemplateList)">-->
    <!--      Templates-->
    <!--      <template #icon="{active}">-->
    <!--        <Icon :name="IconifyConstants.transactionTemplate" :width="getStrokeWidth(active)"/>-->
    <!--      </template>-->
    <!--    </van-tabbar-item>-->

    <van-tabbar-item :name="tabConstants.add" @click="onChange(tabConstants.add)">
      <template #icon="{ active }">
        <svg-add-icon width="40" height="40" />
        <!--        <Icon :name="IconifyConstants.add" width="40" height="40"/>-->
      </template>
    </van-tabbar-item>

    <van-tabbar-item :name="tabConstants.extras" @click="onChange(tabConstants.extras)">
      Extras
      <template #icon="{ active }">
        <app-icon :icon="TablerIconConstants.extras" :size="20" :stroke="getStrokeWidth(active)" />
      </template>
    </van-tabbar-item>

    <van-tabbar-item :name="tabConstants.settings" @click="onChange(tabConstants.settings)" :dot="appStore.isNewVersionAvailable">
      Settings
      <template #icon="{ active }">
        <app-icon :icon="TablerIconConstants.settings" :size="20" :stroke="getStrokeWidth(active)" />
      </template>
    </van-tabbar-item>
  </van-tabbar>
</template>

<script setup>
import { useDataStore } from '~/stores/dataStore'
import { useAppStore } from '~/stores/appStore'
import RouteConstants from '~/constants/RouteConstants'
import TablerIconConstants from '~/constants/TablerIconConstants'

const dataStore = useDataStore()
const appStore = useAppStore()
const route = useRoute()

const tabConstants = {
  add: 'add',
  transactionList: 'transactionList',
  dashboard: 'dashboard',
  transactionTemplateList: 'transactionTemplateList',
  extras: 'extras',
  settings: 'settings',
}
const activeTab = ref(null)

watch(
  () => route.path,
  (newValue) => {
    if (newValue === RouteConstants.ROUTE_TRANSACTION_LIST || RouteConstants.isForm(RouteConstants.ROUTE_TRANSACTION_ID, newValue)) {
      activeTab.value = tabConstants.transactionList
    }

    if (newValue === RouteConstants.ROUTE_TRANSACTION_TEMPLATE_LIST || RouteConstants.isForm(RouteConstants.ROUTE_TRANSACTION_TEMPLATE_ID, newValue)) {
      activeTab.value = tabConstants.transactionTemplateList
    }

    if (
      [RouteConstants.ROUTE_EXTRAS, RouteConstants.ROUTE_TAG_LIST, RouteConstants.ROUTE_ACCOUNT_LIST, RouteConstants.ROUTE_CATEGORY_LIST].includes(newValue) ||
      RouteConstants.isForm(RouteConstants.ROUTE_TAG_ID, newValue) ||
      RouteConstants.isForm(RouteConstants.ROUTE_ACCOUNT_ID, newValue) ||
      RouteConstants.isForm(RouteConstants.ROUTE_CATEGORY_ID, newValue)
    ) {
      activeTab.value = tabConstants.extras
    }

    if ([RouteConstants.ROUTE_SETTINGS].includes(newValue)) {
      activeTab.value = tabConstants.settings
    }

    // let normalMappedRoutes = [
    //   RouteConstants.ROUTE_TRANSACTION_LIST,
    //   RouteConstants.ROUTE_TRANSACTION_TEMPLATE_LIST,
    //   RouteConstants.ROUTE_EXTRAS,
    //   RouteConstants.ROUTE_SETTINGS,
    //   RouteConstants.ROUTE_TRANSACTION_ID,
    // ]
    // if (normalMappedRoutes.includes(path)) {
    //   activeTab.value = path
    //   return
    // }
    // if (/\/transactions\/[0-9]+/.test(path)) {
    //   activeTab.value = RouteConstants.ROUTE_TRANSACTION_LIST
    // }
    //
    // if ([RouteConstants.ROUTE_TRANSACTION_TEMPLATE_LIST, RouteConstants.ROUTE_TRANSACTION_TEMPLATE_ID].includes(path)) {
    //   activeTab.value = RouteConstants.ROUTE_EXTRAS
    // }
    //
  },
  { deep: true, immediate: true },
)

onMounted(async () => {})

const getStrokeWidth = (active) => {
  return active ? 2.2 : 1.7
}

const onChange = async (code) => {
  switch (code) {
    case tabConstants.dashboard:
      await navigateTo(RouteConstants.ROUTE_DASHBOARD)
      break

    case tabConstants.add:
      await navigateTo(RouteConstants.ROUTE_TRANSACTION_ID)
      break

    case tabConstants.transactionList:
      await navigateTo(RouteConstants.ROUTE_TRANSACTION_LIST)
      break

    case tabConstants.transactionTemplateList:
      await navigateTo(RouteConstants.ROUTE_TRANSACTION_TEMPLATE_LIST)
      break

    case tabConstants.extras:
      await navigateTo(RouteConstants.ROUTE_EXTRAS)
      break

    case tabConstants.settings:
      await navigateTo(RouteConstants.ROUTE_SETTINGS)
      break
  }
}
</script>
