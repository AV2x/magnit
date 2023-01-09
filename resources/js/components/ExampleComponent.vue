<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Чат</div>

                    <div class="card-body">
                        <div class="lead">
                            Список онлайн:
                        </div>
                        <div>
                            <ul>
                                <li v-for="user in users">{{user.name}}</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    data() {
        return {
            users: null,
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
    }
}
</script>
