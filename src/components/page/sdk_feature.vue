<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 项目管理</el-breadcrumb-item>
                <el-breadcrumb-item>项目列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-button type="primary" icon="el-icon-delete" class="handle-del mr10" @click="delAll">批量删除</el-button>

                <el-input v-model="select_word" placeholder="输入筛选关键词" class="handle-input mr10"></el-input>

            </div>
            <el-table :data="data" border class="table" ref="multipleTable" @selection-change="handleSelectionChange" stripe>
                <el-table-column type="selection" width="55" align="center"></el-table-column>

                <el-table-column type="index" label="序号" width="55" align="center"></el-table-column>

                <el-table-column prop="name" label="项目名称" width="250">
                    <template slot-scope="scope">
                        <el-popover trigger="hover" placement="top">
                            <p>项目名: {{ scope.row.name }}</p>
                            <p>接口数: {{ scope.row.interfaces }}</p>
                            <p>套件数: {{ scope.row.testsuits }}</p>
                            <p>用例数: {{ scope.row.testcases }}</p>
                            <p>配置数: {{ scope.row.configures }}</p>
                            <div slot="reference" class="name-wrapper">
                                <el-tag size="medium">{{ scope.row.name }}</el-tag>
                            </div>
                        </el-popover>
                    </template>
                </el-table-column>

                <el-table-column prop="leader" label="项目负责人" width="100" align="center">
                </el-table-column>

                <el-table-column prop="publish_app" label="应用名称" width="250">
                </el-table-column>

                <el-table-column prop="tester" label="测试人员" width="100" align="center">
                </el-table-column>

                <el-table-column prop="create_time" label="创建时间" sortable align="center">
                </el-table-column>

                <el-table-column label="操作" align="center">
                    <template slot-scope="scope">
                        <el-button type="text" icon="el-icon-edit" @click="handleRun(scope.$index, scope.row)">运行</el-button>
                        <el-button type="text" icon="el-icon-edit" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                        <el-button type="text" icon="el-icon-delete" class="red" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <div class="pagination">
                <el-pagination background @current-change="handleCurrentChange"
                               @size-change="handleSizeChange" :page-sizes="[4, 5, 8, 10, 20]"
                               layout="total, sizes, prev, pager, next, jumper" :total="total_nums" :page-size="page_size">
                </el-pagination>
            </div>
        </div>

        <!-- 编辑弹出框 -->
        <el-dialog title="编辑项目" :visible.sync="editVisible" width="30%" center>
            <el-form ref="form" :model="form" label-width="120px">

                <el-form-item label="项目名称">
                    <el-input v-model="form.name" clearable></el-input>
                </el-form-item>

                <el-form-item label="项目负责人">
                    <el-input v-model="form.leader" clearable></el-input>
                </el-form-item>

                <el-form-item label="测试人员">
                    <el-input v-model="form.tester" clearable></el-input>
                </el-form-item>

                <el-form-item label="开发人员">
                    <el-input v-model="form.programmer" clearable></el-input>
                </el-form-item>

                <el-form-item label="应用名称">
                    <el-input v-model="form.publish_app" clearable></el-input>
                </el-form-item>

                <el-form-item label="简要描述">
                    <el-input v-model="form.desc" clearable></el-input>
                </el-form-item>

            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveEdit">确 定</el-button>
            </span>
        </el-dialog>

        <!-- 删除提示框 -->
        <el-dialog title="删除项目" :visible.sync="delVisible" width="300px" center>
            <div class="del-dialog-cnt">删除不可恢复，是否确定删除？</div>
            <span slot="footer" class="dialog-footer">
                <el-button @click="delVisible = false">取 消</el-button>
                <el-button type="primary" @click="deleteRow">确 定</el-button>
            </span>
        </el-dialog>

        <!-- 运行项目弹出框 -->
        <el-dialog title="运行项目" :visible.sync="runVisible" width="30%" center>
            <el-form ref="form" :model="form" label-width="120px">
                <el-form-item label="运行环境">

                    <el-select v-model="env_id" clearable placeholder="请选择">
                        <el-option
                                v-for="item in envs_id_names"
                                :key="item.id"
                                :label="item.name"
                                :value="item.id">
                        </el-option>
                    </el-select>

                </el-form-item>

            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="runVisible = false">取 消</el-button>
                <el-button type="primary" @click="confirmRun">运 行</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        name: 'sdk_feature'
    };
</script>

<style scoped>

</style>
