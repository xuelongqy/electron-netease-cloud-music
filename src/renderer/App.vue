<template>
    <div>
        <AppNav></AppNav>
        <div class="router-view">
            <transition :name="transitionName">
                <keep-alive :include="KeepAlive">
                    <router-view></router-view>
                </keep-alive>
            </transition>
        </div>
        <PlayerBar></PlayerBar>
        <CollectPopup></CollectPopup>
    </div>
</template>

<script>
import AppNav from '@/components/AppNav/AppNav.vue';
import PlayerBar from '@/components/PlayerBar/PlayerBar.vue';
import CollectPopup from '@/components/CollectPopup.vue';

export default {
    name: 'app',
    components: {
        AppNav,
        PlayerBar,
        CollectPopup
    },
    data() {
        return {
            transitionName: 'fade-up'
        };
    },
    created() {
        this.KeepAlive = ['index', 'player', 'favorite'];
        this.$router.beforeEach((to, from, next) => {
            if (to.name === 'player') {
                this.transitionName = 'player-in';
            } else if (from.name === 'player') {
                this.transitionName = 'player-out';
            } else if (window.__NAV_BACK__ === true) {
                this.transitionName = 'fade-down';
            } else {
                this.transitionName = 'fade-up';
            }
            window.__NAV_BACK__ = false;
            next();
        });
    }
};
</script>

<style lang="less">
.router-view {
    height: calc(100vh - 128px);
    overflow: hidden;
    position: relative;
    margin-top: 64px;
    .ncm-page {
        width: 100%;
        height: 100%;
        overflow: auto;
        position: absolute;
    }
}
</style>
