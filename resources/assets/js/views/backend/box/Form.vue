<template>
    <div class='contents'>
        <el-form ref="form" :model="form" :rules="rules" label-width="80px">
            <el-form-item :label="$t('fields.box_number')"  prop="box_number" >
                <el-input v-model.number="form.box_number"></el-input>
            </el-form-item>
            <el-form-item :label="$t('fields.sort')"  prop="sort" >
                <el-input v-model.number="form.sort"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="onSubmit('form')">{{ $t('form.submit') }}</el-button>
                <el-button @click="goBack">{{ $t('form.cancel')}}</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script type="text/ecmascript-6">
    import { boxView } from '../../../config/backend/views';
    import { notificationRedirect,redirect } from '../../../helps/helps';
    export default{
        props:{
            form:{
                type: Object,
                default(){
                    return {};
                }
            }
        },
        data(){
            return{
                url:'',
                method:'',
                message:'',
                submit:false,
                rules: {
                    sort:[
                        { type: 'number', message: '序号必须为数字值'}
                    ],
                    box_number:[
                        { type: 'number', message: '序号必须为数字值'}
                    ]
                }
            }
        },
        methods: {
            onSubmit(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        this.url = '/box'+(this.form.id ? '/' + this.form.id : '');
                        this.method = this.form.id ? 'put' : 'post';
                        this.message=this.$t('message.'+this.method);
                        if(this.submit==false){
                           this.submit=true;
                           let self=this;
                           this.$http[this.method](this.url, this.form).then((response) => {
                               if (response.status == 201 || response.status == 204) {
                                   notificationRedirect(self.message, function () {
                                       self.goBack();
                                   });
                               }
                           }).catch(({response}) => {
                               this.submit=false;
                           });
                        }
                        return false;
                    }
                    return false;
                });
            },
            goBack(){
                this.$router.go(-1);
            }

        }
    }
</script>

<style type="text/css" scoped>

</style>
