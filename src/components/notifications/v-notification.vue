<template>
    <div class="v-notification">
        <transition-group
            name="v-transition-animate"
            class="messages_list"
        >
            <div class="v-notification__content"
                 v-for="message in messages"
                 :key="message.id"
                 :class="message.icon"
            >
                <div class="v-notification__content-text">
                    <span>{{ message.name }}</span>
                    <i class="material-icons">{{message.icon}}</i>
                </div>
                <div class="v-notification__content-buttons">
                    <button v-if="leftButton.length">OK</button>
                    <button v-if="rightButton.length">CANCEL</button>
                </div>
            </div>
        </transition-group>
    </div>
</template>

<script>
    export default {
        name: "v-notification",
        props: {
            messages: {
                type: Array,
                default: () => {
                    return []
                }
            },
            rightButton: {
                type: String,
                default: ''
            },
            leftButton: {
                type: String,
                default: ''
            },
            timeOut: {
                type: Number,
                default: 3000
            }
        },
        watch: {
            messages() {
                this.hideNotification()
            }
        },
        methods: {
            hideNotification() {
                let vm = this
                if(this.messages.length) {
                    setTimeout(() => {
                        vm.messages.splice(vm.messages.length -1, 1)
                    },vm.timeOut)
                }
            }
        },
        mounted() {
            this.hideNotification()
        }
    }
</script>

<style scoped lang="scss">
    .v-notification {
        position: fixed;
        top: 100px;
        right: 16px;
        z-index: 10;
        display: flex;
        flex-direction: column-reverse;
        &__content {
            padding: 16px;
            border-radius: 4px;
            color: #fff;
            background: green;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 50px;
            margin-bottom: 20px;
            &.error {
                background: red;
            }
            &.check_circle {
                background: green;
            }
            &.warning {
                background: orange;
            }
        }
        &__buttons {
            display: flex;
        }
        .material-icons {
            margin-left: 16px;
        }
        .messages_list {
            display: flex;
            flex-direction: column-reverse;
        }
    }

    .v-transition-animate {
        &-enter {
            transform: translateX(120px);
            opacity: 0;
        }
        &-enter-active {
            transition: all .6s ease;
        }
        &-enter-to {
            opacity: 1;
        }
        &-leave {
            height: 50px;
            opacity: 1;
        }
        &-leave-active {
            transition: transform .6s ease, opacity .6s, height .6s .2s;
        }
        &-leave-to {
            height: 0;
            transform: translateX(120px);
            opacity: 0;
        }
        &-move {
            transition: transform .6s ease;
        }
    }

</style>