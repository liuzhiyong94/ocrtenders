<template>
  <div>
    <el-row>
      <el-col :span="8">
        <p style="font-size:16px;margin:20px 0;">文件上传</p>
        <el-upload
          class="upload-demo"
          ref="upload"
          action="/api/ManageSystem/Upload"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :file-list="fileList"
          :auto-upload="false"
          multiple
        >
          <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
          <el-button
            style="margin-left: 10px;"
            size="small"
            type="success"
            @click="submitUpload"
          >上传到服务器</el-button>
        </el-upload>
      </el-col>
      <el-col :span="8">
        <p style="font-size:16px;margin:20px 0;">初始识别数据</p>
        <p
          v-for="(item, index) in initData"
          :key="index"
          style="margin:10px;color:#409EFF;cursor:pointer;"
          @click="DownloadJson(item)"
        >{{item.split("/")[item.split("/").length-1]}}</p>
      </el-col>
      <el-col :span="8">
        <p style="font-size:16px;margin:20px 0;">精确识别数据</p>
        <el-button style="margin-left: 10px;" size="small" type="success" @click="DownloadZip">批量下载</el-button>
        <p
          v-for="(item, index) in modifyData"
          :key="index"
          style="margin:10px;color:#409EFF;cursor:pointer;"
          @click="DownloadJson(item)"
        >{{item.split("/")[item.split("/").length-1]}}</p>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "front",
  components: {},
  data() {
    return {
      fileList: [],
      initData: [],
      modifyData: []
    };
  },
  methods: {
    submitUpload() {
      this.$refs.upload.submit();
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    },
    async getInitData() {
      let params = {};
      let res = await window.axios.post(
        "/api/ManageSystem/GetInitData",
        params
      );
      this.initData = res.data;
    },
    async getModifyData() {
      let params = {};
      let res = await window.axios.post(
        "/api/ManageSystem/GetModifyData",
        params
      );
      this.modifyData = res.data;
    },
    DownloadJson(url) {
      var a = document.createElement("a");
      a.href = url;
      a.target = "_blank";
      a.download = url.split("/")[url.split("/").length - 1];
      document.body.appendChild(a);
      a.click();
    },
    async DownloadZip() {
      let params = {};
      let res = await window.axios.post(
        "/api/ManageSystem/DownloadZip",
        params
      );
      let zipsrc = res.data;
      var a = document.createElement("a");
      a.href = zipsrc;
      a.target = "_blank";
      a.download = zipsrc.split("/")[zipsrc.split("/").length - 1];
      document.body.appendChild(a);
      a.click();
    }
  },
  created() {
    this.getInitData();
    this.getModifyData();
    setInterval(this.getInitData, 10 * 1000);
    setInterval(this.getModifyData, 10 * 1000);
  }
};
</script>
<style>
</style>
