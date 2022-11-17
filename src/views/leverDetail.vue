<template>
    <div>
        <Table
            :emotionId="true"
            :searchTime="false"
            :add="true"
            :edit="true"
            :searchInput ="false"
            :searchBtn="false"
            @edit="edit"
            :deleteUrl="deleteUrl"
            :searchUrl="url"
            :tableTitle="tableTitle"
            :delete-url="deleteUrl"
            :pageFooter="true"
            @add="add()"
            ref="table"
            :back="{}"
        >
        </Table>
        <!--弹出-->
        <el-dialog class="new-dialog" title="添加方案级别" :visible.sync="dialogFormVisible" @close="onDialogClose">
            <el-form :model="addForm">
                <el-form-item label="级别名称" label-width="120px"  >
                    <el-input v-model="addForm.name" autocomplete="off" placeholder="请输入级别名称"></el-input>
                </el-form-item>
                <el-form-item label="阈值设置" label-width="120px">
                    <el-select v-model="addForm.type" placeholder="请选择生理参数" style="width: 100%;" @change="thresholdChange($event)">
                        <el-option v-for="item in thresholdList" :key="item.id" :label="item.name"
                                   :value="item.id"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="起始值" label-width="120px"  >
                    <el-input v-model="addForm.start_value" autocomplete="off" placeholder="请输入起始值"></el-input>
                </el-form-item>
                <el-form-item label="结束值" label-width="120px"  >
                    <el-input v-model="addForm.end_value" autocomplete="off" placeholder="请输入结束值"></el-input>
                </el-form-item>
                <el-form-item label="备注" label-width="120px"  >
                    <el-input v-model="addForm.remark" autocomplete="off" placeholder="请输入备注"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false" round>取 消</el-button>
                <el-button type="primary" @click="onAdd" round style="background-color: #1E87F0 ; border-color: #1E87F0;">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>


<script>
import Table from "../components/Table";

export default {
    name: "Emotion",
    components: {
        Table
    },
    props:{
        width: {
            type: String,
            default:'75%'
        }
    },
    data() {
        return {
            Data:[],
            tableTitle: [
                {
                    name: '名称',
                    value: 'name',
                    width: 230
                },
                {
                    name: '生理参数',
                    value: 'type_text',
                    width: 230
                }, {
                    name: '起始值',
                    value: 'start_value',
                    width: 230
                },
                {
                    name: '截止值',
                    value: 'end_value',
                    width: 230
                },
                {
                    name: '备注',
                    value: 'remark',
                    width: 230
                }
            ],
            url: 'api/emotion_policy/emotion_level_list',
            deleteUrl: 'api/emotion_policy/delete_emotion_level',
            tableData: [],
            dialogFormVisible: false,
            form: {
            },
            addForm: {
                type:'',
                name: '',
                start_value:'',
                end_value:'',
                remark:''
            },
            type:'',
            thresholdList:[],
            level_id:'',
        };
    },
    computed: {},
    watch: {},
    mounted() {
        this.threshold()
    },
    methods: {
        edit(row) {
            console.log('row',row)
            this.dialogFormVisible = true
            this.addForm.name = row.name
            this.addForm.type = row.type
            this.addForm.start_value = row.start_value
            this.addForm.end_value = row.end_value
            this.addForm.remark = row.remark
            this.level_id = row.id
        },
        async onAdd() {
            let url = ''
            if(this.level_id !=='') {
                url = 'api/emotion_policy/edit_emotion_level'
                this.addForm.level_id = this.level_id
            } else {
                url = 'api/emotion_policy/add_emotion_level'
            }
            this.addForm.emotion_id = this.$route.query.id
            this.addForm.hospital_id = this.$store.state.hospital_id
            await this.$axios.post(url,this.$qs.stringify(this.addForm)).then(res => {
                this.dialogFormVisible = false
                this.$refs.table.onSubmit()
                this.addForm.name = ''
                this.addForm.type = ''
                this.addForm.start_value = ''
                this.addForm.end_value = ''
                this.addForm.remark = ''
                this.level_id = ''
            })
        },
        thresholdChange(eve) {
            this.addForm.type = eve
        },
         //生理参数
         async threshold() {
            await this.$axios.post('api/common/physiology_param').then(res => {
                if (res.data.code == 1) {
                    this.thresholdList = res.data.data
                }
            })
        },

        onDialogClose() {

        },
        async details(row) {
            await this.$axios.post('api/emotion/detail', {
                id:row.id
            }).then(res => {
                if (res.data.code == 1) {
                    this.Data = res.data.data
                }
            })
        },
        Refresh() {
            this.$refs.table.onRefresh()
        },
        add() {
            this.addForm.name = ''
            this.addForm.type = ''
            this.addForm.start_value = ''
            this.addForm.end_value = ''
            this.addForm.remark = ''
            this.level_id = ''
            this.dialogFormVisible = true
        },
        // 按钮弹窗
        dialog_btn() {
            let _that = this
            _that.dialogFormVisible = true;     // alert('cap')
        },
    },
}
</script>


<style scoped lang="scss">
::v-deep {
    .el-dialog {
        width: 30%;
    }
}
</style>
