<template>
    <div>
        <Table :tableTitle="tableTitle"
               :pageFooter="true"
               :InfoHref="InfoHref"
               :deleteUrl="'api/exam/delete'"
               :searchUrl="url"
               :statistics="true"
               :statistics_="true"
               :statis-href="`AssessStatistics`"
               :tableType = "1"
               :form="form"
               :scale_title="true"
               :add="false"/>

    </div>
</template>

<script>
import Table from "../components/Table";

export default {
    name: "TestReport",
    components: {Table},
    data() {
        return {
            tableTitle: [
                {
                    name: '编号',
                    value: 'num'
                },
                {
                    name: '测试时间',
                    value: 'create_at',

                },
                {
                    name: '量表名称',
                    value: 'test_paper_name'
                }],
            url: 'api/exam/index',
            InfoHref: 'InfoAssess',
            reportUrl: '',
            form:{
                type:''
            }
        };
    },
    computed: {},
    watch: {},
    methods: {
        add() {
        },
        removeBtn(ids) {
            console.log(ids)
        }
    },
    mounted() {
        if(this.$route.query.type) {
            this.form.type = this.$route.query.type
        }
        sessionStorage.setItem("detail_", true);
    }
}
</script>

<style scoped>

</style>
