<script setup>
import { ref } from "vue";
import { Plus, Van } from "@element-plus/icons-vue";
import { ElMessage } from "element-plus";

const timeConsuming = ref(0);
const showIcon = ref(true);

const handleChange = (res) => {
  // console.log("^^^^", res);
  showIcon.value = false;
};

// 上传对象实例
const uploadEle = ref(null);
const reUploadImg = () => {
  timeConsuming.value = 0;
  showIcon.value = true;
  licensePlateImg.value = "";
  licensePlateText.value = "";
  licensePlateColor.value = "white";
  imgsPreText.value = ["", "", "", ""];
  uploadEle.value.clearFiles();
};

const uploadSuccess = (response) => {
  if (response.status === "success") {
    ElMessage({
      message:
        "识别成功, 耗时" +
        (new Date().getTime() - timeConsuming.value) / 1000 +
        "s",
      type: "success",
    });
    licensePlateImg.value = response.fileSrc;
    licensePlateText.value = response.lptext;
    licensePlateColor.value = response.lpcolor;
    if (response.imgPres) {
      imgsPreText.value = response.imgPres;
      console.log(imgsPreText.value);
    }
  } else {
    ElMessage({
      message: "识别失败",
      type: "error",
    });
  }
};

const uploadError = () => {
  timeConsuming.value = 0;
  showIcon.value = true;
  ElMessage({
    message: "上传失败",
    type: "error",
  });
};

const uploadBefore = () => {
  timeConsuming.value = new Date().getTime();
};

// 车牌图片、车牌文本、车牌颜色
let licensePlateImg = ref("");
let licensePlateText = ref("");
let licensePlateColor = ref("white");

let nowTime = ref("");
setInterval(() => {
  const year = new Date().getFullYear();
  const month = new Date().getMonth() + 1;
  const day = new Date().getDate();
  const hour = new Date().getHours();
  const minute = new Date().getMinutes();
  const second = new Date().getSeconds();
  nowTime.value =
    year + "/" + month + "/" + day + " " + hour + ":" + minute + ":" + second;
}, 1);

// 图像预处理
const imgPreText = ref([
  "提取车牌颜色",
  "形态学处理",
  "二值化处理",
  "轮廓定位",
]);
const imgsPreText = ref(["", "", "", ""]);
</script>

<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="80%" style="margin-right: 0px">
        <div class="upload-img-area">
          <el-upload
            ref="uploadEle"
            action="http://120.76.203.186:6789/uploadImg"
            list-type="picture"
            :auto-upload="true"
            :limit="1"
            :on-change="handleChange"
            :on-success="uploadSuccess"
            accept=".jpg, .png"
            :on-error="uploadError"
            :before-upload="uploadBefore"
          >
            <el-icon :size="20" style="margin: 288px 400px" v-if="showIcon"
              ><Plus
            /></el-icon>

            <template #file="{ file }">
              <div>
                <img class="el-upload-list__item-thumbnail" :src="file.url" />
              </div>
            </template>
          </el-upload>
        </div>
      </el-aside>
      <el-main style="padding: 0">
        <div class="result-area">
          <el-button
            type="primary"
            style="margin: 10px 10px"
            @click="reUploadImg"
            >重新上传图片</el-button
          >
          <el-divider style="margin: 0" />
          <div class="license-plate-detection">
            <span class="icon-text">
              <el-icon class="icon-margin"><Van /></el-icon>车牌检测
            </span>
            <el-image
              v-if="licensePlateImg"
              :src="licensePlateImg"
              fit="cover"
              class="license-plate-detection-img"
            />
            <div v-else style="width: 200px; height: 50px"></div>
          </div>
          <el-divider style="margin-top: 5px" />
          <div class="license-plate-recognition">
            <span class="icon-text">
              <el-icon class="icon-margin"><Van /></el-icon>车牌识别
            </span>
            <text v-if="licensePlateText" class="lpt"
              >{{ licensePlateText }}
            </text>
            <div v-else class="lpt-empty"></div>
          </div>
          <el-divider style="margin-top: 5px" />
          <div class="license-plate-color">
            <span class="icon-text">
              <el-icon class="icon-margin"><Van /></el-icon>车牌颜色
            </span>
            <div v-if="licensePlateColor" class="lpc"></div>
            <div v-else class="lpc"></div>
          </div>
          <el-divider style="margin-top: 5px" />
          <div class="license-plate-pre">
            <span class="icon-text">
              <el-icon class="icon-margin"><Van /></el-icon>预处理图片
            </span>
            <div class="demo-image__placeholder">
              <div
                class="block"
                v-for="(item, index) in imgPreText"
                :key="index"
              >
                <span class="demonstration">{{ item }}</span>
                <el-image
                  v-if="imgsPreText[index]"
                  :zoom-rate="1.2"
                  :preview-src-list="imgsPreText"
                  :src="imgsPreText[index]"
                  :initial-index="index"
                />
                <div v-else style="width: 100%; height: 50px"></div>
              </div>
            </div>
          </div>
          <el-divider style="margin-top: 10px" />
        </div>
      </el-main>
    </el-container>
  </div>

  <div class="footer">{{ nowTime }}</div>
</template>

<style scoped>
.common-layout {
  width: 95%;
  background-color: rgba(255, 255, 255, 0.68);
  margin-top: 0.5rem;
  border: 1px solid #409eff;
  border-radius: 20px;
  box-shadow: 0px 12px 8px -12px #409eff;
  padding: 10px;
}
.upload-img-area {
  height: 600px;
  /* border: 1px solid #409eff; */
  border-right: none;
  border-radius: 20px 0 0 20px;
}
.result-area {
  height: 600px;
  /* border: 1px solid #409eff; */
  border-left: 1px solid #409eff;
  border-radius: 0 20px 20px 0;
}

.el-upload-list__item-thumbnail {
  width: 100% !important;
  height: 100% !important;
}

:deep(.upload-img-area) .el-upload-list {
  width: 100%;
  /* height: 540px; */
}

:deep(.upload-img-area) .el-upload--picture-card {
  width: 550px;
  height: 550px;
}
.el-upload-list__item-actions {
  width: 550px;
  height: 550px;
}
:deep(.upload-img-area) .el-upload-list__item {
  border: none;
  width: 100%;
  height: 100%;
  margin: -25px 0 0 0;
}
.icon-text {
  display: flex;
  align-items: center;
}
.icon-margin {
  margin-right: 3px;
}
.license-plate-detection {
  font-size: 20px;
  margin-top: 15px;
  color: #409eff;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.license-plate-detection-img {
  margin-top: 8px;
  width: 90%;
  height: 50px;
}

.license-plate-recognition {
  font-size: 20px;
  margin-top: 15px;
  color: #409eff;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}
.license-plate-color {
  font-size: 20px;
  margin-top: 15px;
  color: #409eff;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.lpt {
  color: #000000;
  font-size: 24px;
  margin: 8px 0 3px 0;
}
.lpt-empty {
  /* width: 200px;
  height: 50px; */
  margin: 40px 0 3px 0;
}
.lpc {
  margin: 10px 0 6px 0;
  width: 90%;
  height: 25px;
  background-color: v-bind(licensePlateColor);
}
.footer {
  margin-top: 10px;
  text-align: center;
}

.license-plate-pre {
  font-size: 20px;
  margin-top: 15px;
  color: #409eff;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* 预处理图片 */
.demo-image__placeholder {
  margin-top: 10px;
  width: 96%;
}
.demo-image__placeholder .block {
  display: inline-block;
  text-align: center;
  width: 50%;
  box-sizing: border-box;
  vertical-align: top;
}
.demo-image__placeholder .demonstration {
  align-items: center;
  display: block;
  color: #909399;
  font-size: 14px;
}
.demo-image__placeholder .el-image {
  padding: 0 5px;
}
</style>
