<template>
    <div style="display: flex; ">
        <h2>{{ head }}</h2>
        <template >
        <n-icon> <ChevronRight16Filled /> </n-icon>
        </template>
    </div>
    <List v-for="item in music_info_list" :music_info="item" />
</template>

<script>
import { NCard, NEllipsis, NIcon } from 'naive-ui';
import { ChevronRight16Filled, Heart28Regular, TextBulletListAdd24Filled } from '@vicons/fluent/lib';
import List from './List.vue';
import querystring from 'querystring';

export default {
    name: 'FavoriteListPage',
    methods: {
        search_music(){
            // console.log(this.qurl);
            // console.log(this.uid);
            this.$axios.post(this.qurl, querystring.stringify(
                this.uid
            )).then(res => {
                var data =res.data
                // console.log(data);
                this.music_info_list = data;
            }).catch(function (error) {
                console.log(error);
            })
        }
    },
    mounted() {
        this.search_music()
    },
    // updated() {
    //     this.search_music(this.currentPage)
    // },
    data() {
        return{
            currentPage: 1,
            music_info_list: []
        }
    },
    props:{
        uid:{
            type: String,
            default: ""
        },
        qurl:{
            type: String,
            default: ""
        },
        head: {
            type: String,
            default: "搜索结果"
        }
    },
    components:{
        NCard,
        Heart28Regular,
        TextBulletListAdd24Filled,
        NEllipsis,
        List,
        ChevronRight16Filled,
        NIcon
    }
  }

</script>