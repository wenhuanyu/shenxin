<template>
    <div id="index">
        <img class="logo" src="../assets/images/logo.png" alt="">
        <div class="index-content">
            <router-link v-for="(item,index) in line.slice(0,3)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <div class="bbb">
<!--                    <div :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" v-html="item.name" class="aaa"></div>-->
<!--                    <div :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" v-html="item.introduce" class="ccc"></div>-->
                    <img :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="">
                </div>
            </router-link>
        </div>
        <div class="index-content content1">
            <router-link v-for="(item,index) in line.slice(3,6)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <div class="bbb">
<!--                    <div :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" v-html="item.name" class="aaa"></div>-->
<!--                    <div :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" v-html="item.introduce" class="ccc"></div>-->
                    <img :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="" >
                </div>
            </router-link>
        </div>

        <div class="index-content content2">
            <router-link v-for="(item,index) in line.slice(6,7)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <div class="bbb">
<!--                    <div :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" v-html="item.name" class="aaa"></div>-->
<!--                    <div :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" v-html="item.introduce" class="ccc"></div>-->
                    <img :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="" >
                </div>
            </router-link>
        </div>

        <div class="all_right">
            <el-popover
                placement="top"
                width="160"
                v-model="logoutShow">
                <p>?????????????????????</p>
                <div style="text-align: right; margin: 0">
                    <el-button size="mini" type="text" @click="logoutShow = false">??????</el-button>
                    <el-button type="primary" size="mini" @click="logout">??????</el-button>
                </div>
                <button slot="reference" class="all_right_top">
                    <img src="../assets/images/user_red.png" alt="">
                    <p>????????????</p>
                </button>
            </el-popover>
        </div>
    </div>
</template>

<script>
export default {
    name: "Index",
    data() {
        return {
            logoutShow: false,
            line: [
                {
                    img: require("@/assets/images/index_icon6.png"),
                    name: '????????????',
                    introduce:'??????????????????????????????',
                    to: '/TermMonitor'
                },
                {
                    img: require('@/assets/images/index_icon1.png'),
                    name: '????????????',
                    introduce:'??????????????????',
                    to: 'PatientManagement'
                },
                {
                    img: require("@/assets/images/index_icon2.png"),
                    name: '????????????',
                    introduce:'??????????????????',
                    to: '/Assess'
                },


                //line 2
                {
                    img: require("@/assets/images/index_icon4.png"),
                    name: '????????????',
                    introduce:'???????????????????????????',
                    to: '/Emotion'
                },
                {
                    img: require("@/assets/images/index_icon7.png"),
                    name: '????????????',
                    introduce:'??????????????????????????????',
                    to: '/DataAnalysis'
                },
                {
                    img: require("@/assets/images/index_icon5.png"),
                    name: '????????????',
                    introduce:'??????????????????',
                    to: '/ReportCenter'
                },

                //line 3
                {
                    img: require("@/assets/images/index_icon8.png"),
                    name: '????????????',
                    introduce:'?????????????????????',
                    to: '/SystemSetting'
                },

            ]
        };
    },
    computed: {
        check: function () {
            return (name) => {
                return this.$store.state.mainMenus.includes(name) ? 'all' : 'none'
            }
        }
    },
    watch: {
        /*'$store.state.mainMenus':(newVal)=>{
            console.log('mainMenus change',newVal)
        }*/
    },
    async mounted() {
        console.log('????????????', sessionStorage.getItem('usertype'))

        if (sessionStorage.getItem('usertype') !== 'center') {
            this.$router.push('/TerminalLogin')
        }
        if (!sessionStorage.getItem('usertype')) {
            this.$router.push('/Login')
        }
        this.getRules().then()
    },
    methods: {
        logout() {
            this.$store.commit('setUserInfo', {data: '', status: false});
            this.$router.push('/Login');
        },
        getRules() {
            let rule = new Promise((resolve, reject) => {
                //??????????????????
                this.$store.dispatch('GetAuth')
            })
            return rule
        }
    },
}
</script>

<style scoped lang="scss">
button {
    border: none;
}
.bbb {
    position: relative;
}
.aaa {
    z-index: 100;
    position: absolute;
    top: 34%;
    left: 38%;
    height: 35px;
    font-size: 30px;
    font-weight: 400;
    color: #FFFFFF;
    line-height: 35px;
}
.ccc {
    z-index: 100;
    position: absolute;
    top: 48%;
    left: 38%;
    height: 17px;
    font-size: 16px;
    font-weight: 400;
    color: #FFFFFF;
    line-height: 17px;
}
#index {
    width: 100%;
    height: 100vh;
    background: url('../assets/images/indexBG.png') no-repeat;
    background-size: 100% 100%;
}

.logo {
    display: block;
    //height: 16vh;
    position: absolute;
    top: 2vh;
    left: 10%;
}

.index-content {
    width: 1700px;
    display: flex;
    position: fixed;
    top: 22vh;
    left: 10%;
}

.content1 {
    top: 47vh;
    left: 10%;
}

.content2 {
    top: 72vh;
    left: 10%;
}

.index-content .img {
    display: block;
    //width: 461px;
    //height: 206px;
    transition: .3s all ease-in-out;
}

.index-content a {
    width: 30%;
    height: 22%;
}

.all_right_top p::after {
    border-top: 8px solid #1E87F0;
}
::v-deep .el-button--primary {
    background-color: #1E87F0;
}
::v-deep .el-button--text {
    color: #1E87F0;
}
.disabled {
    filter: grayscale(100%);
}
</style>
