<template>
  <div class="container">
    <el-row :gutter="20">
      <el-col :span="12">
        <div class="block">
          <span class="demonstration">上传比对库文件</span>
          <el-upload
            ref="thesisUpload"
            class="upload-demo margin-right-20"
            multiple
            list-type="picture-card"
            :auto-upload="false"
            :action="uploadUrl"
            :headers="uploadHeaders"
            :data="uploadData"
            :on-remove="handleRemove"
            :before-remove="beforeRemove"
            :before-upload="handleBeforeUpload"
            :on-success="handleFileUploadSuccess"
            :on-error="handleFileUploadError"
            :with-credentials="false"
            :on-change="handleOnfileChange"
            :on-exceed="handleExceed"
            :file-list="fileList"
            :accept="acceptFile"
            :limit="5"
          >
            <!-- <el-button slot="trigger" size="small" type="success">上传比对库文件</el-button> -->
            <i class="el-icon-plus"></i>
            <div slot="tip" class="el-upload__tip">
              请注意上传图片的命名格式：
              <font style="color:red;font-weight:bold;">用户组_图片标签</font>
              <br />选取好文件后点击确定即可
            </div>
          </el-upload>
          <el-button round style="margin-top:20px" type="primary" @click="submitUpload">确定</el-button>
          <el-button round style="margin-top:20px" type="success" @click="submitCapture">上传截屏图片</el-button>
        </div>
      </el-col>

      <el-col :span="12">
        <div class="block">
          <span class="demonstration">当前图片</span>
          <el-image :src="src">
            <div slot="placeholder" class="image-slot">
              加载中
              <span class="dot">...</span>
            </div>
            <div slot="error" class="image-slot">
              <i class="el-icon-picture-outline"></i>
            </div>
          </el-image>
          <div>
            <div class="button" style="margin-top:10px;">
              <el-button round type="primary" @click="getImage">刷新当前图片</el-button>
            </div>
            <div class="button" style="margin-top:10px;">
              <el-button round type="success" @click="getDetect">预测</el-button>
            </div>
            <div class="button" style="margin-top:10px;">
              <span class="detect-result">{{detectResult}}</span>
            </div>
          </div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>


<script>
export default {
  name: "Image",
  components: {},
  data() {
    return {
      src: "",
      imgBase64: "",
      acceptFile: ".jpg,.png",
      uploadUrl: "http://localhost:8080/image/insert",
      uploadHeaders: {},
      uploadData: {},
      fileList: [],
      detectResult: ""
    };
  },
  mounted() {
    this.getImage();
  },
  methods: {
    handleClose() {},
    handleRemove() {},
    beforeRemove() {},
    handleExceed() {},
    handleBeforeUpload(file) {},
    handleOnfileChange(file) {},
    handleFileUploadSuccess(res) {
      console.log(res);
      if (res.status == "200") {
        this.$message({
          type: "success",
          message: "上传成功"
        });
      } else {
        this.$message({
          type: "error",
          message: res.msg
        });
      }
    },
    handleFileUploadError(err) {
      console.log(err);
    },
    submitUpload() {
      this.uploadUrl = "http://localhost:8080/image/insert";
      this.$refs.thesisUpload.submit();
    },
    getImage() {
      this.axios
        .get(this.GLOBAL.BASE_URL + "/image/get", {
          responseType: "arraybuffer"
        })
        .then(response => {
          var result = response.data;
          console.log(result);
          let bytes = new Uint8Array(result);
          let storeData = "";
          let len = bytes.byteLength;
          for (let i = 0; i < len; i++) {
            storeData += String.fromCharCode(bytes[i]);
          }
          this.imgBase64 = window.btoa(storeData);
          this.src = "data:image/png;base64," + window.btoa(storeData);
        })
        .catch(error => {
          console.log(error);
          this.$message({
            type: "error",
            message: "错误发生"
          });
        });
    },
    getDetect() {
      this.axios
        .post(
          this.GLOBAL.BASE_URL + "/image/detect",
          {
            img: this.imgBase64
          },
          {
            headers: {
              "content-type": "application/json;charset=utf-8"
            }
          }
        )
        .then(response => {
          var result = response.data;
          console.log(result);
          if (result.status == "200") {
            this.detectResult = result.msg;
          } else {
            this.error(result.msg);
          }
        })
        .catch(error => {
          console.log(error);
          this.$message({
            type: "error",
            message: "错误发生"
          });
        });
    },
    submitCapture() {
      this.axios
        .post(
          this.GLOBAL.BASE_URL + "/image/capture/insert",
          {
            img: this.imgBase64
          },
          {
            headers: {
              "content-type": "application/json;charset=utf-8"
            }
          }
        )
        .then(response => {
          var result = response.data;
          console.log(result);
          if (result.status == "200") {
            this.$message({
              type: "success",
              message: "上传成功"
            });
          } else {
            this.error(result.msg);
          }
        })
        .catch(error => {
          console.log(error);
          this.$message({
            type: "error",
            message: "错误发生"
          });
        });
    }
  }
};
</script>

<style scoped>
.container {
  font-family: "Open Sans", sans-serif;
  color: #4a4a4a;
  text-align: center;
  height: 100%;
}
.block {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  height: 500px;
  width: 500px;
  background-color: #ffffff;
  margin-top: 10%;
  margin-left: 10%;
  border-radius: 15px;
  box-shadow: 2px 2px 5px gray;
}
.demonstration {
  margin-bottom: 20px;
  font-size: 15px;
}
.el-row,
.el-col {
  height: 100%;
}
.el-image {
  height: 300px;
  width: 300px;
}
.detect-result {
  color: red;
  font-size: 20px;
  font-weight: bold;
}
</style>