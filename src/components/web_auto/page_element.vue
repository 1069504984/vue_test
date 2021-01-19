<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 自定义函数</el-breadcrumb-item>
                <el-breadcrumb-item>自定义函数列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-input v-model="select_word" placeholder="输入筛选关键词" class="handle-input mr10"></el-input>
            </div>
        </div>
        <el-radio-group v-model="direction">
            <el-radio label="ltr">从左往右开</el-radio>
            <el-radio label="rtl">从右往左开</el-radio>
            <el-radio label="ttb">从上往下开</el-radio>
            <el-radio label="btt">从下往上开</el-radio>
        </el-radio-group>

        <el-button @click="drawer = true" type="primary" style="margin-left: 20px;">
            点我查看函数
        </el-button>

        <el-drawer
                title="函数总览"
                :visible.sync="drawer"
                :direction="direction"
                :before-close="handleClose">
            <span>
    <li v-for="func in tableDataS" class="infinite-list-item" style="list-style:none;margin:10px;color: #2d8cf0;text-align:-moz-left">{{ func}}</li>
            </span>
        </el-drawer>

    </div>
</template>


<script>
    import { get_all_func } from '../../api/api';

    export default {
        name: 'page_element',
        data() {
            return {
                tableDataS: [],
                cur_page: 1,    // 当前页
                page_size: 10,  // 每页显示的数量
                total_nums: 1, // 数据总条数
                common: [],
                multipleSelection: [],
                select_word: '',
                del_list: [],
                id: 1,
                count: [1, 2, 3, 4, 5, 6],
                drawer: false,
                direction: 'rtl',

            };
        },
        created() {
            this.get_func(this.id);
        },

        methods: {
            search() {
                this.is_search = true;
            },
            // 获取函数
            get_func(id) {
                get_all_func(this.id).then(res => {
                    this.tableDataS = res.data.common;
                    console.log(this.tableDataS);
                });
            },
            handleClose(done){
                this.$confirm('确认关闭？').then(_ => {
                    done();
                }).catch(_ => {});
            }

        }

    };


</script>

<style scoped>
    .table {
        width: 100%;
        font-size: 14px;
    }

    .handle-input {
        width: 300px;
        display: inline-block;
    }

    .el-dropdown-link {
        cursor: pointer;
        color: #409EFF;
    }

    .el-icon-arrow-down {
        font-size: 12px;
    }
</style>
