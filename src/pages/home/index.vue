<template>
    <div>
        <!--搜索条-->
        <search-bar></search-bar>
    </div>
</template>

<script>
// 导入公共的search组件
import SearchBar from '../../components/search'
export default {
    data() {
        return {
            
        }
    },
    components:{
        'search-bar': SearchBar
    }
}
</script>

<style scoped>

</style>
