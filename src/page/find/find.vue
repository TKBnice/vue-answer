<template>
    <div class="page">
        <div class="top">
            <!-- 轮播图 -->
            <div class="mt-swipe">
                <mt-swipe :auto="4000">
                    <mt-swipe-item class="silde1"><img v-bind:src="data.tu1" alt="图片1"></mt-swipe-item>
                    <mt-swipe-item class="silde2"><img v-bind:src="data.tu2" alt="图片2"></mt-swipe-item>
                    <mt-swipe-item class="silde3"><img v-bind:src="data.tu3" alt="图片3"></mt-swipe-item>
                </mt-swipe>
            </div>
            <div class="overflow">
                <div class="classification">
                    <ul>
                        <li>书籍精选</li>
                        <li>今日看点</li>
                        <li>本周热门</li>
                        <li>本月热门</li>
                        <li>最新上榜</li>
                        <li>总排行榜</li>
                        <li>已经出版</li>
                        <li>特约作者</li>
                    </ul>
                </div>
            </div>

        </div>
        <!-- 滚动列表 -->
        <div class="scroll">
            <h3>
                <span>推荐作者</span>
            </h3>
            <div id="wrapper">
                <div class="authors">
                    <ul>
                        <li v-for="author in data.authors">
                            <img :src="author.avatar" alt="author.name">
                            <div>
                                <p>{{author.name}}</p>
                                <button>关注</button>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        <!-- 信息展示 -->
        <div class="content" id="content">
            <ul ref="ul">
                <li v-for="author in data.authors">
                    <h5><img :src="author.avatar">
                        <span>{{author.name}}</span>
                    </h5>
                    <!-- <h3>{{author.editor_notes}}</h3> -->
                    <p>{{author.resume}}</p>
                </li>
            </ul>
        </div>
        <v-footer></v-footer>
    </div>
</template>

 <script type="text/ecmascript-6">
import { Swipe, SwipeItem } from 'mint-ui'
import Footer from '@/components/common/footer'
import axios from 'axios'

export default {
    data() {
        return {
            contentHeight: '16rem',
            data: {
                tu1: require('@/assets/aaa1.jpg'),
                tu2: require('@/assets/aaa2.jpg'),
                tu3: require('@/assets/aaa3.jpg'),
                authors: [
                    { avatar: '', name: '昵称', editor_notes: '详细介绍', resume: '个人说明' },
                    { avatar: '', name: '昵称', editor_notes: '详细介绍', resume: '个人说明' },
                    { avatar: '', name: '昵称', editor_notes: '详细介绍', resume: '个人说明' },
                    { avatar: '', name: '昵称', editor_notes: '详细介绍', resume: '个人说明' },
                    { avatar: '', name: '昵称', editor_notes: '详细介绍', resume: '个人说明' }

                ]
            }
        }
    },
    computed: {

    },
    components: {
        'v-footer': Footer,
        Swipe,
        SwipeItem
    },
    mounted() {
        this.aget();
        this.getContentHeight();
    },
    methods: {
        getContentHeight() {
            var authorHeight = this.$refs.ul.children[0].offsetHeight;
            // var n = this.data.authors.length;
            // this.contentHeight = (authorHeight * n / 16) + 'rem';
            // this.$refs.ul.style.height = this.contentHeight;
            console.log(this.$refs.ul.children[0].offsetHeight)
        },
        aget() {
            var _this = this
            axios.get('/api')
                .then(function(response) {
                    console.log(response);
                    // console.log(response.data.total);
                    for (var i = 0; i < response.data.total; i++) {
                        _this.data.authors[i].avatar = response.data.authors[i].avatar;
                        _this.data.authors[i].name = response.data.authors[i].name;
                        _this.data.authors[i].editor_notes = response.data.authors[i].editor_notes;
                        _this.data.authors[i].resume = response.data.authors[i].resume;
                    }
                    // console.log(_this.data.authors);
                    // _this.data.authors[0].alt = response.data.authors[0].alt;
                    // _this.author.avatar = response.data.authors[0].avatar;
                    // _this.author.editor_notes = response.data.authors[0].editor_notes;
                    // _this.author.large_avatar = response.data.authors[0].large_avatar;
                    // _this.author.last_post_time = response.data.authors[0].last_post_time;
                    // _this.author.name = response.data.authors[0].name;
                    // _this.author.resume = response.data.authors[0].resume;
                    // _this.author.url = response.data.authors[0].url;
                    // console.log(_this.author.large_avatar)
                })
                .catch(function(err) {
                    console.log(err);
                })
        }
    }

}
</script>

<style scoped lang="stylus">
.page{
    width:20rem;
    overflow hidden;
}
li,ul{
    margin 0;
    padding 0;
}
h1,h2 {
font-weight normal;
}
ul {
  list-style-type: none;
}
.scroll h3{
    height 1rem;
    line-height 1rem;
    font-size 0.4rem;
    background-color #d9d9d9;
    color #707274;
}
.scroll h3 span {
    border-left 2px solid #da2929;
    padding-left 3px;
    margin-left 5px;
}
#wrapper{
    overflow hidden;
    width 100%;
    height 7.25rem;
}
.authors{
    background-color #f0f0f0;
    overflow hidden;
    position relative;
    width 20rem;
    height 8.25rem;
    overflow-x: scroll;
    overflow-y: hidden;
}
.authors ul{
    width 31.25rem;
}
.authors::after{
    content '';
    display block;
    height 0 ;
    overflow hidden;
    clear both;
}
.authors li{
    height 6.625rem;
    float left;
    text-align center;
    font-size 1rem;
    margin 0.3rem;
    background-color #fafafa;
    border 1px solid #ddd;
    padding 0.3rem 0;
}
.authors li p{
    text-align center
    text-overflow ellipsis;
    overflow hidden;
    width 5.25rem;
    white-space nowrap;
    font-size 0.9rem;
    padding-top 0.25rem;
}
.authors li img{
    border-radius 50%;
}
.authors li button{
    margin-top 0.1875rem;
    list-style none;
    border none;
    background-color #0f88eb;
    color #fff
    font-size 0.9rem;
    width 4.3rem;
    height 1.2rem;
    line-height 1.2rem;
    border-radius 0.125rem;
}

.content{
 margin-bottom 3.8rem;
 background-color #d9d9d9;
 padding-top 3px;
}
.content li{
    margin 0.25rem 0;
    padding 0.25rem;
    background-color #fafafa;
}
.content li h5{
    width 100%;
    height 20px;
    line-height 20px;
}
.content li h5 img{
    border-radius 50%;
    width 20px;
    height 20px;
    vertical-align:middle;
}
.content li h5 span{
    font-size 0.065rem;
}
.content li h3{
    font-size 0.8rem;
}
.content li p{
    font-size 0.2rem;
    // overflow hidden;
    // white-space nowrap;
    // text-overflow ellipsis;
}

.mt-swipe {
  height 10rem;
  overflow hidden;
}

.silde1 img,
.silde2 img,
.silde3 img {
  width 100%;
  height 10rem;
}
.overflow{
    height 40px;
    overflow hidden;
    .classification{
        width 20rem;
        height 60px;
        overflow-x: scroll;
        overflow-y: hidden;
        ul{
            width 600px;
            height 40px;
            display:-webkit-flex;
            align-items: center;
            li{
                // float left;
                flex:1;
                width 60px;
                height 30px;
                line-height 30px;
                font-size 0.8rem;
                text-align center;
                background-color #dcb43c;
                margin-left 5px;
            }
        }  
    }
}
.classification ul li:last-child{
    margin-right 5px;
}
</style>