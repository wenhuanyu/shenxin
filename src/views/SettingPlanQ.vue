<template>
    <div class="setting-plan">
        <div class="programme">
            <div class="programme_title" @click="tab">
                测评方案
            </div>
            <div class="programme_title programme_action" @click="tabTwo">
                训练治疗方案
            </div>
            <div class="programme_title" @click="tabThree">
                PAD量表管理
            </div>
        </div>
        <Table :tableTitle="tableTitle_emotion" :searchBtn="true" :form="form"
               :searchTime="true"
               :addHref="addHrefTwo"
               :detailHref="detailHrefTwo" :deleteUrl="deleteUrl" :searchUrl="urlTwo" :page-footer="true"
               class="programme-right" levelUrl="leverDetail">

        </Table>

    </div>
</template>

<script>
import Table from '../components/Table';

export default {
    name: "SettingPlanQ",
    components: {
        Table
    },
    data() {
        return {
            qrDialog: false,
            tableTitle_emotion: [
                // {
                //     name: '编号'
                // },
                {
                    name: '名称',
                    value: 'name',
                },
                {
                    name: '已选内容',
                    value: 'detail',
                    formatter: (row) => {
                        let text = [];
                        row.detail.map((item, key) => {
                            if (key < 10) {
                                text.push(item.game_name)
                            }
                        });
                        return text.join('，') + (row.detail.length > text.length ? '...' : '')
                    },
                    align: 'left',
                    width: '300px'
                },
                {
                    name: '时间',
                    value: 'create_at',
                }
            ],
            form: {
                keyword: '',
                id: 0
            },
            urlTwo:'api/emotion_policy/lst',
            deleteUrl: 'api/emotion_policy/delete',
            addHrefTwo:'SettingPlanAddQ',
            detailHrefTwo: 'SettingPlanDetailQ',
            tableData: [],
            currentRow: '',
        };
    },
    computed: {},
    watch: {},
    mounted() {

    },
    methods: {
        tab(index) {
            this.$router.push('/SettingPlan')
        },
        tabTwo(index) {
            this.$router.push('/SettingPlanQ')
        },
        qrcode(row) {
            this.currentRow = row;
            this.qrDialog = true;
            console.log(row)
        },
        tabThree(index) {
            this.$router.push('/SettingPlanPad')
        },
        printPage() {
            this.$print(this.$refs.print)
        }
    },
}
</script>

<style scoped lang="scss">

::v-deep .el-dialog__header {
    padding: 0;
}

::v-deep .el-dialog__body {
    padding: 0;
}
::v-deep .el-table__body-wrapper{
    overflow: auto;
    height: 74vh;
}
//::v-deep .content_top{
//    margin-bottom: 8px;
//}
.dialog_body {
    padding: 30px 20px;
    position: relative;
}

.qrcode-bg {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    height: auto;
}

.button-wrap {
    display: flex;
    justify-content: flex-end;
    margin-bottom: 20px;
}

.logo-name {
    display: flex;
    justify-content: center;
    align-items: center;
    color: #333333;

    > img {
        width: 100px;
        height: 100px;
    }

    > div {
        margin-left: 20px;
        text-align: center;

        > h3 {
            font-size: 54px;
        }

        > p {
            font-size: 30px;
        }
    }
}

.plan-name {
    font-size: 60px;
    text-align: center;
    margin: 85px 0;
    color: #333333;

}

.qrcode-wrap {
    margin-left: auto;
    margin-right: auto;
    width: 478px;
    height: 478px;
    padding: 20px;
    border: 1px solid #979797;
    border-radius: 10px;

    > img {
        width: 100%;
        height: 100%;
        object-fit: contain;
    }
}

.explain-title {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 100px auto 40px;

    > span {
        padding: 0 10px;
        color: #333333;
        font-size: 24px;
    }

    > div {
        width: 24px;
        height: 1px;
        background-color: #1E86EF;
    }
}

.explain {
    margin-bottom: 200px;

    > p {
        text-align: center;
        color: #333333;
        font-weight: 600;
        margin-bottom: 0.5em;
        font-size: 30px;
    }
}

.setting-plan {
    display: flex;
}

.programme {
    width: 266px;
    height: 100%;
    background: #fff;
    padding: 10px;
}

.programme-right {
    min-width: 1284px;
    margin-left: 20px;
}

.programme_title {
    width: 246px;
    height: 58px;
    border-radius: 6px;
    font-size: 16px;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    display: flex;
    align-items: center;
    justify-content: center;
}

.programme_action {
    background: #1E86EF;
    color: #fff;
}

.hide {
    display: none;
}

.show {
    display: block;
}
</style>
