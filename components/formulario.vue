<template>
  <el-row type="flex" justify="center">
    <el-form model="form" rules="rules" ref="form">
      <el-form-item prop="sentiment">
        <el-col span="8">
          <span class="cardcss"> Escriba como se sintió</span>
        </el-col>
        <el-col span="12">
          <el-input v-model="form.sentiment" size="medium" class="sizebox">
            <el-button type="primary" @click="onSubmit" slot="append">
              Enviar
            </el-button>
          </el-input>
        </el-col>
      </el-form-item>
    </el-form>
  </el-row>
</template>
<script>
export default {
  data() {
    return {
      form: {
        sentiment: '',
      },
    }
  },
  methods
}
</script>
