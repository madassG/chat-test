<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="form-group">
                    <textarea readonly="" rows="6" class="form-control"> {{ dataMessages.join('\n') }} </textarea>
                </div>
                <div class="input-group mb-3">
                    <input type="text" class="form-control" placeholder="Type your message" v-model="message">
                    <div class="input-group-append">
                        <button @click="sendMessage" class="btn btn-outline-secondary" type="button">Send</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data: function(){
            return {
                dataMessages: [],
                message: "",
            }
        },
        mounted() {
            var socket = io('http://localhost:3000');
            socket.on("send-message-channel:App\\Events\\NewMessage", function (data) {
                this.dataMessages.push(data.message);
            }.bind(this));
        },
        methods:{
            sendMessage: function () {
                axios({
                    method: 'get',
                    url: '/chat/send-message',
                    params: {
                        message: this.message,
                    },
                }).then((response) => {
                    this.message = "";
                });
            }
        }
    }
</script>
