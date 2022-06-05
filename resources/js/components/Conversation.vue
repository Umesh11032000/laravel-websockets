<template>
    <div class="conversation">
        <h1>{{contact ? contact.name : 'Choose a contact'}}</h1>
        <MessagesFeed :contact="contact" :messages="messages"></MessagesFeed>
        <MessagesComposer @send="sendMessage"></MessagesComposer>
    </div>
</template>

<script type="text/javascript">
    import MessagesFeed from "./MessagesFeed";
    import MessagesComposer from "./MessagesComposer";

    export default{
        props: {
            contact: {
                type: Object,
                default: null,
            },
            messages: {
                type: Array,
                default: [],
            }
        },
        methods: {
            sendMessage(text){
                if (!this.contact) {
                    return;
                }

                axios.post('/conversation/send', {
                    contact_id: this.contact.id,
                    text: text,
                }).then(response => {
                    this.$emit('new', response.data);
                    
                });
            }
        },
        components : {MessagesFeed, MessagesComposer}
    }
</script>

<style lang="scss" scoped>
    .conversation {
        flex: 5;
        display: flex;
        flex-direction: column;
        justify-content: space-between;

        h1 {
            font-size: 20px;
            padding: 10px;
            border-bottom: 1px dotted #5c4e4e;
        }
    }
</style>