<template>
<div>
    <div class="tabs-list">
        <div class="tabs-list__heading text-sub">
            <span>Saved States</span>
        </div>
        <div class="tabs-list__list">
            <popup-tab 
            v-if="!isLoading" 
            v-for="state in expandArr(allStates, expandAllStates)" 
            :key="state.id" :tabData="state" 
            @click="openState(state)"/>
            <div v-if="isLoading" class="tab-item">
            <div class="tab-item-inner">
                <i class="text-sub text-sub_dimmer">Loading your states...</i>
            </div>
            </div>
            <div v-if="!isLoading && !allStates.length" class="tab-item">
            <div class="tab-item-inner">
                <span class="text-sub text-sub_dimmer">No saved states.</span>
            </div>
            </div>
            <div class="tab-item" v-if="allStates.length > 5" @click="expandAllStates = !expandAllStates">
            <div class="tab-item-inner">
                <i class="text-sub text-sub_dimmer" v-if="!expandAllStates">Show more...</i>
                <i class="text-sub text-sub_dimmer" v-else>Show less...</i>
            </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import PopupTab from './components/PopupTab'
import TabsNavigation from './components/TabsNavigation'
import browser from 'webextension-polyfill'

export default {
    data() {
        return {
            isLoading: true,
            expandSavedStates: false,
            expandDeletedStates: false,
            expandAllStates: false,
            allStates: [],
        }
    },
    components: { PopupTab, TabsNavigation },
    mounted() {
        browser.storage.local.get("states").then((store) => {
            this.allStates = store.states.filter(s => !s.isDeleted) || []
            this.isLoading = false
        })
    },
    computed: {
        savedStates() {
            return this.allStates.filter(s => s.type === 'saved')
        },
        lastDeletedStates() {
            return this.allStates.filter(s => s.type === 'last_deleted')
        }
    },
    methods: {
        expandArr(array, shouldExpand) {
            if (shouldExpand) return array
            return array.slice(0, 5)
        },
        openState(state) {
            this.$router.push({ name: 'ViewState', params: { id: state.id } })
        }
    },
}
</script>

<style scoped>
.tabs-list {
    font-size: 14px;
    padding-top: 8px;
    padding-bottom: 4px;
}

.tabs-list__heading {
    padding: 4px 16px;
}

.tabs-list__list {
    padding: 0;
    margin: 0;
    max-height: 400px;
    overflow-y: auto;
}
</style>