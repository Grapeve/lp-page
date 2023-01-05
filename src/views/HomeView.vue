<script setup>
import { ref } from "vue";
import { Plus, Van } from "@element-plus/icons-vue";

const showIcon = ref(true);
const handleChange = (file) => {
  console.log(file); // file为上传的图片对象
  showIcon.value = false;
  // TODO: 调用后端接口将图片传过去，并接收返回结果
};

// 上传对象实例
const uploadEle = ref(null);
const reUploadImg = () => {
  showIcon.value = true;
  uploadEle.value.clearFiles();
};

// 车牌图片、车牌文本、车牌颜色
let licensePlateImg = ref("");
let licensePlateText = ref("");
let licensePlateColor = ref("white");
</script>

<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="80%" style="margin-right: 10px">
        <div class="upload-img-area">
          <el-upload
            ref="uploadEle"
            action="#"
            list-type="picture"
            :auto-upload="false"
            :limit="1"
            :on-change="handleChange"
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
            <img
              v-if="licensePlateImg"
              :src="licensePlateImg"
              class="license-plate-detection-img"
            />
            <div
              v-else
              style="width: 200px; height: 50px; border: 1px solid #409eff"
            ></div>
          </div>
          <el-divider style="margin-top: 10px" />
          <div class="license-plate-recognition">
            <span class="icon-text">
              <el-icon class="icon-margin"><Van /></el-icon>车牌识别
            </span>
            <text v-if="licensePlateText" class="lpt"
              >{{ licensePlateText }}
            </text>
            <div v-else class="lpt-empty">XXXXXX</div>
          </div>
          <el-divider style="margin-top: 10px" />
          <div class="license-plate-color">
            <span class="icon-text">
              <el-icon class="icon-margin"><Van /></el-icon>车牌颜色
            </span>
            <div v-if="licensePlateColor" class="lpc"></div>
            <div v-else class="lpc"></div>
          </div>
          <el-divider style="margin-top: 10px" />
        </div>
      </el-main>
    </el-container>
  </div>
</template>

<style scoped>
.common-layout {
  margin-top: 0.5rem;
}
.upload-img-area {
  height: 600px;
  border: 1px solid #409eff;
  border-radius: 20px 0 0 20px;
}
.result-area {
  height: 600px;
  border: 1px solid #409eff;
  border-radius: 0 20px 20px 0;
}

.el-upload-list__item-thumbnail {
  width: 100%;
  height: 100%;
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
  border: 1px solid #409eff;
  width: 200px;
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
  margin: 8px 0 3px 0;
}
.lpc {
  margin: 10px 0 6px 0;
  width: 200px;
  height: 25px;
  background-color: v-bind(licensePlateColor);
}
</style>
