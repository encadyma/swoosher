<template>
<div class="blocker-page">
    <h1>Blocked Page</h1>
    <p><b>{{websiteName}} is blocked</b> under your current profile.</p><br>
    <button class="button" @click="goToPreviousBrowserPage">Go Back</button><br>
    <div class="blocker-page-actions">
        <a class="blocker-second-action" :href="optionsLink">Go to options</a>
        <span class="blocker-plugin-spacer">&bull;</span>
        <span class="text-sub blocker-plugin-version">Swoosher {{version}}</span>
    </div>
</div>
</template>

<script>
import browser from 'webextension-polyfill'
export default {
    methods: {
        goToPreviousBrowserPage() {
            window.history.go(-1)
        }
    },
    computed: {
        optionsLink() {
            return browser.extension.getURL('/options/options.html')
        },
        version() {
            return browser.runtime.getManifest().version_name
        },
        websiteName() {
            if (!this.$route.query.hostname) return 'This website'
            return this.$route.query.hostname
        }
    }
}
</script>

<style scoped>
.blocker-page {
    /* text-align: center; */
    margin: 0 20vw;
    margin-top: 20vh;
}

h1 {
    font-size: 4em;
}

p {
    font-size: 1.2em;
    line-height: 1.5;
}

.blocker-page-actions {
    font-size: 14px;
}

.blocker-second-action {
    color: rgb(55, 135, 255);
    display: inline-block;
    margin: 10px 0;
    text-decoration: none;
}

.blocker-plugin-version {
    color: #aaa;
}

.blocker-plugin-spacer {
    color: #aaa;
    margin: 0 10px;
}

.blocker-second-action:hover {
    color: rgb(45, 115, 215);
}
</style>