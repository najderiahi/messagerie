<template>
    <div class="card">
        <div class="card-header">
            John
        </div>
        <div class="card-body">

            <Message :message="message" v-for="message in messages" :user="user"/>


            <form action="" method="post">

                <div class="form-group">
                        <textarea name="content" v-model="content" placeholder="érivez votre message"
                                  :class="{'form-control' :true,'is-invalid': errors ['content'] }"
                                  @keypress.enter="sendMessage"></textarea>
                        <div class="invalid-feedback">{{ errors ['content'] }}</div>
                </div>

            </form>


        </div>
    </div>
</template>


<script>
    import Message from './MessageComponent'
    import {mapGetters} from 'vuex'
    export default {
        components:{Message},
        data(){
            return {
                content :'',
                errors:{},
            }
        },
        computed: {
            ...mapGetters(['user']),
            messsages : function () {
                return this.$store.getters.messages(this.$route.params.id)
            }
        },

        mounted() {
            this.loadMessages()
            },
        watch :{
            '$route.params.id' :function () {
                this.loadMessages()
            }
        },
        methods: {
            loadMessages(){
                this.$store.dispatch('loadMessages',this.$route.params.id)
            },
             async sendMessage(e){
                if(e.shiftKey === false){
                    try {
                       await this.$store.dispatch('sendMessage',{
                            content: this.content,
                            userId : this.$route.params.id
                        })
                    }catch (e) {
                        this.errors = e.errors

                        
                    }

                }

            },
        }
    }
</script>
