<template>
    <div>
        <el-row>
            <el-col :span="5">
                <p style="font-size:16px;margin:10px 0;">北京清单</p>
                <el-upload
                    class="avatar-uploader"
                    action
                    :auto-upload="false"
                    :show-file-list="false"
                    :on-change="(file, fileList) => {onUploadChange(file, fileList, 'BJmedicalinsurance')}"
                >
                    <img v-if="imageUrl" :src="imageUrl" class="avatar" />
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
            </el-col>
            <el-col :span="5">
                <p style="font-size:16px;margin:10px 0;">其他清单</p>
                <el-upload
                    class="avatar-uploader"
                    action
                    :auto-upload="false"
                    :show-file-list="false"
                    :on-change="(file, fileList) => {onUploadChange(file, fileList, 'TencentInsurance')}"
                >
                    <img v-if="imageUrl" :src="imageUrl" class="avatar" />
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
            </el-col>
            <el-col :span="4">
                <p style="font-size:16px;margin:10px 0;">门诊</p>
                <el-upload
                    class="avatar-uploader"
                    action
                    :auto-upload="false"
                    :show-file-list="false"
                    :on-change="(file, fileList) => {onUploadChange(file, fileList, 'Generalmedicalbillclassify_deep')}"
                >
                    <img v-if="imageUrl" :src="imageUrl" class="avatar" />
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
            </el-col>
            <el-col :span="5">
                <p style="font-size:16px;margin:10px 0;">住院</p>
                <el-upload
                    class="avatar-uploader"
                    action
                    :auto-upload="false"
                    :show-file-list="false"
                    :on-change="(file, fileList) => {onUploadChange(file, fileList, 'Generalmedicalbillclassify_deep')}"
                >
                    <img v-if="imageUrl" :src="imageUrl" class="avatar" />
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
            </el-col>
            <el-col :span="5">
                <p style="font-size:16px;margin:10px 0;">小结检测</p>
                <el-upload
                    class="avatar-uploader"
                    action
                    :auto-upload="false"
                    :show-file-list="false"
                    :on-change="(file, fileList) => {onUploadChange(file, fileList, 'GeneralMedicalText')}"
                >
                    <img v-if="imageUrl" :src="imageUrl" class="avatar" />
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
            </el-col>
        </el-row>
        <el-input
            type="textarea"
            :autosize="{ minRows: 4, maxRows: 30}"
            placeholder="请输入内容"
            v-model="jsonstr"
            style="margin:20px;width:50%;"
        ></el-input>
        <div>
            <el-button
                style="margin-left: 10px;"
                size="small"
                type="success"
                @click="ModifyJsonStr"
            >修改提交</el-button>
        </div>
    </div>
</template>

<script>
export default {
    name: "back",
    components: {},
    data() {
        return {
            imageUrl: "",
            jsonstr: "",
            srcList: [],
            imageurl: "",
            jsonname: ""
        };
    },
    methods: {
        onUploadChange(file, fileList, type) {
            var that = this;
            console.log("file:", file);
            console.log("fileList:", fileList);
            console.log("type:", type);
            var reader = new FileReader();
            reader.readAsDataURL(file.raw);
            reader.onload = async function(e) {
                const base64 = this.result;
                let params = {
                    base64: base64,
                    filename: file.name,
                    type: type
                };
                let res = await window.axios.post(
                    "/api/ManageSystem/UploadToOcr",
                    params
                );
                console.log("res:", res);
                that.imageurl = res.data.imageurl;
                that.jsonstr = res.data.jsonstr;
                that.jsonname = res.data.jsonname;
                that.srcList = [this.imageUrl];
            };
        },
        async ModifyJsonStr() {
            let params = {
                jsonstr: this.jsonstr,
                jsonname: this.jsonname
            };
            let res = await window.axios.post(
                "/api/ManageSystem/ModifyJsonStr",
                params
            );
            console.log("res:", res);
            if (res.code == 200) {
                this.$message({
                    message: "修改成功",
                    type: "success"
                });
            } else {
                this.$message({
                    message: res.msg,
                    type: "warning"
                });
            }
        }
    },
    created() {}
};
</script>
<style>
.avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
}
.avatar-uploader .el-upload:hover {
    border-color: #409eff;
}
.avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
}
.avatar {
    width: 178px;
    height: 178px;
    display: block;
}
</style>
