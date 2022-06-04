<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card border border-dark shadow-0 mb-3">
                    <div class="card-header bg-transparent border-dark">Chat Room</div>
                    <div class="card-body text-dark">
                        <ul class="list-unstyled" style="height:300px; overflow-y: scroll;" v-chat-scroll>
                            <li class="p-2" v-for="(message, index) in messages" :key="index">
                                <strong>{{ message.user.name }}</strong>
                                {{ message.message }}
                            </li>
                        </ul>
                    </div>
                    <div class="card-footer bg-transparent border-dark">
                        <input
                            @keydown="sendTypingEvent"
                            @keyup.enter="sendMessage"
                            v-model="newMessage"
                            type="text"
                            name="message"
                            placeholder="Enter your message..."
                            class="form-control">
                    </div>
                    <span class="text-muted" v-if="activeUser" ><i class="fa fa-copy"></i>{{ activeUser.name }} is typing...</span>
                </div>
            </div>

            <div class="col-md-4">
                <div class="card border border-dark shadow-0 mb-3">
                    <div class="card-header bg-transparent border-dark">Active User</div>
                    <div class="card-body text-dark">
                        <ul class="list-unstyled" style="height:300px; overflow-y: scroll;">
                            <li class="py-2" v-for="(user, index) in users" :key="index">
                            <span class="heart p-1">
                                <i class="fa fa-circle fa-beat"></i>
                            </span>
                            {{ user.name }}
                            </li>
                        </ul>
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
import axios from 'axios';

    export default {
        props:['user'],
        data() {
            return {
                messages: [],
                newMessage: '',
                users:[],
                activeUser: false,
                typingTimer: false,
            }
        },

        created() {
            this.fetchMessages();
            Echo.join('chat')
                .here(user => {
                    this.users = user;
                })
                .joining(user => {
                    this.users.push(user);
                })
                .leaving(user => {
                    this.users = this.users.filter(u => u.id != user.id);
                })
                .listen('MessageSent',(event) => {
                    this.messages.push(event.message);
                })
                .listenForWhisper('typing', user => {
                   this.activeUser = user;
                    if(this.typingTimer) {
                        clearTimeout(this.typingTimer);
                    }
                   this.typingTimer = setTimeout(() => {
                       this.activeUser = false;
                   }, 3000);
                })
        },

        methods: {
            fetchMessages() {
                axios.get('messages').then(response => {
                    this.messages = response.data;
                })
            },
            sendMessage() {
                this.messages.push({
                    user: this.user,
                    message: this.newMessage
                });
                axios.post('messages', {message: this.newMessage});
                this.newMessage = '';
            },
            sendTypingEvent() {
                Echo.join('chat')
                    .whisper('typing', this.user);
            }
        }
    }
</script>
