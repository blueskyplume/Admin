<template>
    <el-row class="h-screen bg-sky-400 " justify="center" align="middle">
        <el-col :span="8" class="flex justify-center">
            <el-form ref="ruleFormRef" style="height: 400px;" 
                :model="ruleForm" 
                status-icon 
                :rules="rules"
                label-width="auto" 
                class="flex flex-col justify-center w-96 bg-white py-3 px-6 rounded-sm shadow-md"
            >
                <h1 class="text-center mb-4">登录</h1>
                <el-form-item prop="pass">
                    <el-input v-model="ruleForm.pass" size="large" :prefix-icon="User" placeholder="账户" type="text" autocomplete="off" />
                </el-form-item>
                <el-form-item prop="checkPass">
                    <el-input v-model="ruleForm.checkPass" size="large" :prefix-icon="Lock" placeholder="密码" type="password" autocomplete="off" />
                </el-form-item>
                <el-form-item prop="age">
                    <el-input v-model.number="ruleForm.age" size="large" placeholder="验证码" />
                </el-form-item>
                <el-button  type="primary" @click="submitForm(ruleFormRef)">
                        登录
                </el-button>
            </el-form>
        </el-col>
    </el-row>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { User, Lock } from '@element-plus/icons-vue'

const ruleFormRef = ref<FormInstance>()

const checkAge = (rule: any, value: any, callback: any) => {
    if (!value) {
        return callback(new Error('Please input the age'))
    }
    setTimeout(() => {
        if (!Number.isInteger(value)) {
            callback(new Error('Please input digits'))
        } else {
            if (value < 18) {
                callback(new Error('Age must be greater than 18'))
            } else {
                callback()
            }
        }
    }, 1000)
}

const validatePass = (rule: any, value: any, callback: any) => {
    if (value === '') {
        callback(new Error('请输入账号'))
    } else {
        if (ruleForm.checkPass !== '') {
            if (!ruleFormRef.value) return
            ruleFormRef.value.validateField('checkPass')
        }
        callback()
    }
}
const validatePass2 = (rule: any, value: any, callback: any) => {
    if (value === '') {
        callback(new Error('请输入密码'))
    } 
    // else if (value !== ruleForm.pass) {
    //     callback(new Error("Two inputs don't match!"))
    // } else {
    //     callback()
    // }
}

const ruleForm = reactive({
    pass: '',
    checkPass: '',
    age: '',
})

const rules = reactive<FormRules<typeof ruleForm>>({
    pass: [{ validator: validatePass, trigger: 'blur' }],
    checkPass: [{ validator: validatePass2, trigger: 'blur' }],
    age: [{ validator: checkAge, trigger: 'blur' }],
})

const submitForm = (formEl: FormInstance | undefined) => {
    if (!formEl) return
    formEl.validate((valid) => {
        if (valid) {
            console.log('submit!')
        } else {
            console.log('error submit!')
        }
    })
}

const resetForm = (formEl: FormInstance | undefined) => {
    if (!formEl) return
    formEl.resetFields()
}
</script>