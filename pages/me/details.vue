<template>
    <div class="section">
        <h2>Kontaktdaten</h2>
        <form class="form" @submit.prevent="updateUser">
            <div class="form-item">
                <span class="label">Vorname</span>
                <input class="input-text" disabled type="text" v-model="user.profile.firstName" name="" id=""/>
            </div>
            <div class="form-item">
                <span class="label">Nachname</span>
                <input class="input-text" disabled type="text" v-model="user.profile.lastName" name="" id=""/>
            </div>
            <div class="form-item">
                <span class="label">Adresse</span>
                <input class="input-text" type="text" v-model="user.profile.address" name="" id=""/>
            </div>
            <div class="form-item">
                <span></span>
                <input class="input-text" type="text" v-model="user.profile.address2" name="" id=""/>
            </div>
            <div class="form-item">
                <span class="label">PLZ</span>
                <input class="input-text" type="text" v-model="user.profile.zip" name="" id=""/>
            </div>
            <div class="form-item">
                <span class="label">Stadt</span>
                <input class="input-text" type="text" v-model="user.profile.city" name="" id=""/>
            </div>
            <div class="button-row">
                <div v-if="loading">
                    Saving…
                </div>
                <button v-else type="submit" class="input-button-primary">Speichern</button>
            </div>
        </form>
    </div>
</template>

<script>
    export default {
        middleware: 'authenticated',
        data () {
            return {
                loading: false,
                done: false,
            }
        },
        created() {
            this.getUser();
        },
        methods: {
            updateUser(event) {
                this.loading = true;
                this.$store.dispatch('updateUser', Object.assign({}, this.user.profile)).then(() => {
                    this.loading = false;
                    this.$notify({
                        title: 'Yay!',
                        text: 'Änderungen gespeichert.'
                    });
                }).catch((e) => {
                    this.loading = false;
                    this.$notify({
                        title: 'Error',
                        type: 'error',
                        text: 'Ein Fehler ist aufgetreten.'
                    });
                });
            },
            getUser() {
                this.$store.dispatch('getUser').then((data) =>{
                    console.log(data);
                })
            }
        },
        computed: {
            user() {
                console.log(this.$store.state.user);
                return this.$store.state.user;
            },
        }
    }
</script>

<style lang="scss">
    @import '@/assets/scss/styles.scss';
    .input-button-primary {
        cursor: pointer;
        background-color: #ff6f00;
        color: #FFF;
        border: 1px solid #ff8c33;
        padding: 7px 12px 8px;
        line-height: 1;
        outline: none;
        align-self: center;
        margin-top: 20px;
    }
</style>
