<template>

  <div class="image-uploader" ref="imgUploader">
    <el-upload list-type="picture-card"
               :disabled="readonly"
               :action="uploadUrl"
               :name="uploadName"
               :headers="uploadOption.headers"
               :data="uploadOption.data"
               :limit="limit"
               :file-list="fileList"
               :on-progress="handleProgress"
               :on-success="handleSuccess"
               :on-error="handleError"
               :on-preview="handlePreview"
               :on-change="handleChange"
               :before-upload="beforeUpload"
               :before-remove="beforeRemove"
               :on-remove="handleRemove"
               :on-exceed="handleExceed">
      <!--<el-button><i class="el-icon-plus"></i></el-button>-->
      <i class="el-icon-plus"></i>
      <div class="el-upload__tip" slot="tip" v-if="tips && !readonly">{{ tips }}</div>
    </el-upload>

    <el-dialog :visible.sync="dialogVisible"
               @close="handleClosePreview" center>
      <img width="100%"
           v-if="selectedImg"
           :src="selectedImg.url"
           :alt="selectedImg.name || selectedImg.url.split('/').pop()">
    </el-dialog>

  </div>

</template>
<style >
  .el-upload {
    background-color: #151a20 !important;
    border: none !important;
    line-height: 28px;
  }

  .el-upload::before {
    content: '';
    display: block;
    height: calc(50% - 14px);
  }
</style>

<script>
export default {
  props: {
    uploadUrl: {
      required: true,
      type: String
    },
    uploadName: {
      required: true,
      type: String
    },
    uploadOption: {
      required: false,
      type: Object,
      default: {
        headers: {},
        data: {}
      }
    },
    fileList: {
      required: true,
      type: Array
    },
    limit: {
      required: false,
      type: Number,
      default: 10
    },
    tips: {
      required: false,
      type: String,
      default: ''
    },
    readonly: {
      required: false,
      type: Boolean,
      default: false
    }
  },
  created () {},
  mounted () {
    if (this.readonly) {
      const uploadBtn = this.$refs.imgUploader.childNodes[0].getElementsByClassName('el-upload el-upload--picture-card')[0]
      uploadBtn.style = 'display: none;'
    }
  },
  data () {
    return {
      dialogVisible: false,
      selectedImg: null
    }
  },
  computed: {},
  methods: {
    handleSuccess (resp, file, fileList) {
      console.log('>>>>> 文件上传成功...', resp, file, fileList)
      this.$emit('success', resp, file, fileList)
    },
    handleError (err, file, fileList) {
      this.$message.error('上传图片大小不能超过2MB!')
      console.log('>>>>> 文件上传失败...', err, file, fileList)
      this.$emit('fail', err, file)
    },
    handleExceed (files, fileList) {
      console.log('>>>>> 文件超出个数限制...', files, fileList)
    },
    beforeUpload (file) {
      console.log('>>>>> 上传文件之前...', file)
      /*
       * TODO 限制上传处理: 1) 文件类型; 2) 文件大小;
       */
    },
    beforeRemove (file, fileList) {
      console.log('>>>>> 删除文件之前...', file, fileList)
    },
    handleRemove (file, fileList) {
      console.log('>>>>> 删除文件...', file, fileList)
      this.$emit('remove', file, fileList)
    },
    handlePreview (file) {
      console.log('>>>>> 点击已上传的文件链接...', file)
      this.selectedImg = file
      this.dialogVisible = true
    },
    handleProgress (event, file, fileList) {
      console.log('>>>>> 文件上传中...', event, file, fileList)
    },
    handleChange (file, fileList) {
      console.log('>>>>> 文件状态改变...', file, fileList)
    },
    handleClosePreview () {
      this.selectedImg = null
    }
  }
}
</script>

<style lang="scss" scoped>
  .image-uploader {
    .el-upload {
      width: 80px;
      height: 80px;
      line-height: 90px;
    }
  }
</style>
