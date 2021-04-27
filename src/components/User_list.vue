<template>
    <div class="contain">
        <p class="subtitle">DMs</p>
        <ul class="users">
            <Single_user v-for="(user,key) in users" :key="key" :user="user"></Single_user>
        </ul>
    </div>
</template>
<script>
    import Single_user from "./userlist/Singleuser";

    export default {
        name: "User_list",
        components: {
            Single_user
        },
        props: ["users"],
        data() {
            return {};
        }
    };
</script>
<style lang="scss" scoped>

    .contain{
        background-color: black;
        color: #d2d2d2;
    }
    .subtitle {
        padding: 6px 10px;
        font-weight: bold;
        font-size: 2rem;
    }

    .users {
        margin: 0px;
        padding: 0px;

        li {
            list-style: none;
            line-height: 40px;
            padding: 10px;
            font-size: 0.9rem;
            font-weight: bold;

            span {
                margin-left: 10px;
               
            }
        }
    }
</style>
