<template>
    <div>
        <h3>Welcome</h3>
        <div>
            <h3>User Name : {{gettersAuthData.userName}}</h3>
        </div>
        <div>
            <h3>User Id : {{gettersAuthData.userId}}</h3>
        </div>
    </div>

</template>

<script>
import { mapGetters } from 'vuex'
export default {
    name:'DashboardCom',
    computed:{
        ...mapGetters('auth',{
            gettersAuthData:'getAuthData'
        })
    }
}
</script>

<style >
</style>
