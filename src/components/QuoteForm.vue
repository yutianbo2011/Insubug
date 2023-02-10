<template>
  <el-card>
    <el-form 
      :model="ruleForm" 
      :rules="rules" 
      ref="ruleForm" 
      label-width="230px" >
      <el-row>
        <el-col :span="12">
          <el-form-item label="Policy Maximum" prop="policyMax">
            <el-select v-model="ruleForm.policyMax" placeholder="Choose your policy maximum">
              <el-option label="Select" value="0"></el-option>
              <el-option label="50,000" value="50"></el-option>
              <el-option label="100,000" value="100"></el-option>
              <el-option label="250,000" value="250"></el-option>
              <el-option label="500,000" value="500"></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="Age" prop="age">
            <el-input v-model.number="ruleForm.age"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <el-row>
        <el-col :span="12">
          <el-form-item label="Travel Dates (mm/dd/yyyy)">
            <el-col :span="11">
              <el-form-item prop="startDate">
                <el-date-picker 
                  type="date" 
                  placeholder="Start Date" 
                  v-model="ruleForm.startDate" 
                  style="width: 100%;">
                </el-date-picker>
              </el-form-item>
            </el-col>
            <el-col class="line" :span="2">-</el-col>
            <el-col :span="11">
              <el-form-item prop="endDate">
                <el-date-picker 
                  placeholder="End Date" 
                  v-model="ruleForm.endDate" 
                  style="width: 100%;">
                </el-date-picker>
              </el-form-item>
            </el-col>
          </el-form-item>
        </el-col>
        
        <el-col :span="12">
          <el-form-item label="Citizenship" prop="citizenship">
            <el-input v-model="ruleForm.citizenship"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <el-row>
        <el-col :span="12">
          <el-form-item label="Mailing State" prop="mailingState">
            <el-input v-model="ruleForm.mailingState"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <el-form-item>
        <el-button type="primary" @click="onSubmit">GET QUOTES</el-button>
        <el-button type="danger" @click="resetForm">Reset From</el-button>
      </el-form-item>

    </el-form>
  </el-card>
</template>

<script>
export default {
  data() {
    var checkAge = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('Please input the age'))
      }
      if (!Number.isInteger(value)) {
        callback(new Error('Please input digits'));
      } else {
        if (value < 0 || value > 100) {
          callback(new Error('Please correct your age'))
        } else {
          callback()
        }
      }
    }
    var checkStartDate = (rule, value, callback) => {
      
      if (!value) {
        return callback(new Error('Please pick a start date'));
      }

      let startTime = new Date(value).getTime()
      let now = new Date().getTime()
      if(startTime < now){
        return callback(new Error('Please correct the start date'));
      }
      return callback()
    }

    var checkEndDate = (rule, value, callback) => {
      
      if (!value) {
        return callback(new Error('Please pick a start date'));
      }

      let endTime = new Date(value).getTime()
      let now = new Date().getTime()
      if(endTime < now){
        return callback(new Error('Please correct the start date'));
      }
      if(this.ruleForm.startDate){
        let startTime = new Date(this.ruleForm.startDate).getTime()
        if(startTime >= endTime){
          return callback(new Error('Please correct the start date'));
        }
      }
      return callback()
    }
    return {
      ruleForm:{
        startDate: '',
        endDate: '',
        policyMax: null,
        citizenship: '',
        age: '',
        mailingState: ''
      },
      rules: {
          startDate: [
            { validator: checkStartDate, trigger: 'change' }
          ],
          endDate: [
          { validator: checkEndDate, trigger: 'change' }
          ],
          policyMax: [
            { required: true, message: 'Please select policy max', trigger: 'change' },
          ],
          region: [
            { required: true, message: 'Please select citizenship', trigger: 'blur' }
          ],
          age: [
            { validator: checkAge, trigger: 'blur' },
          ],
          mailingState: [
            { type: 'string', required: true, message: 'Please input your mailing state', trigger: 'blur' }
          ]
        }
    }
  },
  methods:{
    onSubmit(){
      this.$refs['ruleForm'].validate((valid) => {
          if (valid) {
            console.log('form', this.ruleForm)
            this.$emit('submit-form')
          } else {
            console.log('error submit!!');
            this.$message({
              message: 'Please correct your info',
              type: 'error'
            })
            return false;
          }
        });
    },
    resetForm(){
      this.ruleForm = {
        startDate: '',
        endDate: '',
        policyMax: null,
        citizenship: '',
        age: '',
        mailingState: ''
      }
    }
  }
}
</script>