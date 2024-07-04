<template>
  <div class="components-container">
    <el-row>
      <el-col :span="8" :offset="8">
        <pan-thumb :image="image + '?' + new Date().getTime()" />
        <image-cropper
          v-show="imagecropperShow"
          :key="imagecropperKey"
          :width="300"
          :height="300"
          url="https://httpbin.org/post"
          lang-type="en"
          @close="close"
          @crop-upload-success="cropSuccess"
        />
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="8" :offset="8">
        <el-button
          type="primary"
          icon="UploadFilled"
          @click="imagecropperShow = true"
        >
          更改头像
        </el-button>
      </el-col>
      <el-col :span="8" :offset="6" style="margin-top: 2%">
        <el-card class="user-info-card" style="width: 40vw">
          <template #header>
            <div class="card-header">
              <span class="ss">用户信息</span>
            </div>
          </template>
          <el-row>
            <el-col :span="6" :offset="1">
              <span class="ss">用户标识：</span>
            </el-col>
            <el-col :span="13" :offset="1">
              <span class="ss">{{ user.getId }}</span>
            </el-col>
          </el-row>
          <el-divider />
          <el-row>
            <el-col :span="6" :offset="1">
              <span class="ss">用户名：</span>
            </el-col>
            <el-col :span="13" :offset="1">
              <span class="ss">{{ user.getNickName }}</span>
            </el-col>
          </el-row>
          <el-divider />
          <el-row>
            <el-col :span="6" :offset="1">
              <span class="ss">手机号：</span>
            </el-col>
            <el-col :span="13" :offset="1">
              <span class="ss">{{ user.getPhone }}</span>
            </el-col>
          </el-row>
          <el-divider />
          <el-row>
            <el-col :span="6" :offset="1">
              <span class="ss">邮箱：</span>
            </el-col>
            <el-col :span="13" :offset="1">
              <span class="ss">{{ user.getEmail }}</span>
            </el-col>
          </el-row>
          <el-divider />
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import ImageCropper from "@/components/ImageCropper";
import PanThumb from "@/components/PanThumb";
import pinia from "@/store/store";
import { useUserStore } from "@/store/user";
import { storeToRefs } from "pinia/dist/pinia";
import { reactive } from "vue";
const user = useUserStore(pinia);
const { image } = storeToRefs(user);
const { userNickName } = storeToRefs(user);
export default {
  name: "AvatarUploadDemo",
  components: { ImageCropper, PanThumb },
  data() {
    return {
      imagecropperShow: false,
      imagecropperKey: 0,
      image: image,
      userNickName: userNickName,
    };
  },
  methods: {
    cropSuccess(resData) {
      this.imagecropperShow = false;
      this.imagecropperKey = this.imagecropperKey + 1;
      this.image = resData.files.avatar;
    },
    close() {
      this.imagecropperShow = false;
    },
  },
};
</script>
<script setup>
// eslint-disable-next-line no-redeclare
import { reactive } from "vue";
import ClipboardJS from "clipboard";
import utils from "@/utils/utils";
import { CopyDocument } from "@element-plus/icons-vue";

const userModel = reactive({
  id: user.getId,
  name: "",
  phone: user.getPhone,
  email: "",
  change: false,
  code: "",
  pwdOriginal: "",
  pwdNew: "",
  pwdNew2: "",
});
const copy = () => {
  let text = new ClipboardJS(".copyBtn", {
    text: function (trigger) {
      //alert("ok");
      return user.getId;
    },
  });
  text.on("success", () => {
    utils.showMessage(200, "复制成功！");
    text.destroy();
  });
  text.on("error", () => {
    utils.showMessage(400, "复制失败！");
    text.destroy();
  });
};
</script>

<style scoped lang="scss">
.components-container {
  margin-top: 5%;
  background-color: #ffffff;
  height: 200px;
  .el-button {
    margin-top: 10px;
  }
}
.copyBtn {
  --el-color-white: #000000;
}
.user-info-card {
  margin-top: 20px;
  background-color: #f5f7fa;
  padding: 20px;
}

.card-header {
  display: flex;
  align-items: center;
  justify-content: center;
}

.ss {
  font-size: 18px;
  font-weight: bold;
  margin-right: 10px;
}
</style>
