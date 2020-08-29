<template>
    <span v-html='html'></span>
</template>


<script>
    import { report_view } from '../../api/api';
    export default {
        beforeRouteEnter (to, from, next) {
            next(vm => {
                vm.report_id = vm.$route.params.id;
                vm.getData();
            });
            next()
        },
        beforeRouteUpdate (to, from, next) {
            this.report_id = to.params.id;
            // this.html = to .params.html;
            this.getData();
            next()
        },
        data() {
            return {
                report_id: null,
                html_report_name: '',
                html:''
            }
        },
    methods: {
        getData() {
            report_view(this.report_id)
                .then(response => {
                    // console.log("html report");
                    this.html_report_name = response.data.summary.html_report_name;
                    // console.log(response.data);
                    // console.log(response.data.summary.time);
                    this.time = response.data.summary.time;
                    this.stat = response.data.summary.stat;
                    this.platform = response.data.summary.platform;
                    this.details = response.data.summary.details;
                    this.html =response.data.html
                })
                .catch(error => {
                    if (typeof error === 'object' && error.hasOwnProperty('status_code')) {
                        if (error.status_code === 404) {
                            this.$router.push({ name: '404' })
                        }
                    } else {
                        this.$message.error('服务器错误');
                    }
                })}}}
</script>

<style scoped>

</style>
