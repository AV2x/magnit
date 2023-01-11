<template>
    <div class="container">
        <section style="background-color: #eee;">
            <div class="container py-5">

                <div class="row">

                    <div class="col-md-6 col-lg-5 col-xl-4 mb-4 mb-md-0">

                        <h5 class="font-weight-bold mb-3 text-center text-lg-start">Online</h5>

                        <div class="card">
                            <div class="card-body">

                                <ul class="list-unstyled mb-0">
                                    <li v-for="user in users" class="p-2 border-bottom" style="background-color: #eee;">
                                        <a href="#!" class="d-flex justify-content-between">
                                            <div class="d-flex flex-row">
                                                <div class="pt-1">
                                                    <p class="fw-bold mb-0">{{user.name}}</p>
                                                </div>
                                            </div>

                                        </a>
                                    </li>
                                </ul>

                            </div>
                        </div>

                    </div>

                    <div class="col-md-6 col-lg-7 col-xl-8">

                        <ul class="list-unstyled">
                            <li class="d-flex justify-content-between mb-4" v-for="message in messages">
                                <div class="card">
                                    <div class="card-header d-flex justify-content-between p-3">
                                        <p class="fw-bold mb-0">{{message.user.name}}</p>
                                    </div>
                                    <div class="card-body">
                                        <p class="mb-0">
                                            {{message.message}}
                                        </p>
                                    </div>
                                </div>
                            </li>
                            <li class="bg-white mb-3">
                                <div class="form-outline">
                                    <textarea v-model="text" class="form-control" id="textAreaExample2" rows="4"></textarea>
                                    <label class="form-label" for="textAreaExample2">Message</label>
                                </div>
                            </li>
                            <button @click="send()" type="button" class="btn btn-info btn-rounded float-end">Send</button>
                        </ul>

                    </div>

                </div>

            </div>
        </section>
    </div>
</template>

<style>
.white {
    background-color: white;
    color: black;
}
</style>

<script>

export default {
    props: ['user'],
    data() {
        return {
            users: null,
            messages: null,
            text: null,
        }
    },
    methods : {
      send() {
          fetch('/api/send-message', {
              method: 'POST',
              body: JSON.stringify({text: this.text})
          });
      }
    },
    mounted() {
        Echo.join('users')
            .here((users) => {
                console.log(111);
                this.users = users
            })
            .joining((user) => {
                this.users.push(user)
            }).leaving((user) => {
            this.users.slice(this.users.indexOf(user), 1)
        })
        Echo.private('chat')
            .listen('MessageSent', (e) => {
                this.messages.push({
                    message: e.message,
                    user: e.user
                });
            });
    }
}
</script>
