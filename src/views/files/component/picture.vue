<template>
  <div class="pic">
    <keep-alive>
      <el-image :src="imgUrl" class="preview" fit="cover"
        :preview-src-list="srcList" :initial-index="preImgIndex" infinite @close="closePre">
        <template #placeholder>
          <img :src="loadingGif" alt="" class="preview">
        </template>
        <template #error>
          <img :src="error" alt="加载失败" style="object-fit: fill; width: 100%; height: 100%" />
        </template>
      </el-image>
    </keep-alive>
  </div>
</template>

<script setup lang="ts">
import error from '@/assets/image/error.png'
import loadingGif from "@/assets/image/loadColor.gif"
import { fileRes } from '@/utils/useTypes';

// 父子组件传值
const props = defineProps<{
  imgUrl: string,
  srcList: string[]
  fileImg: fileRes
}>()

// 查找图片的索引
const preImgIndex = props.srcList.indexOf(props.imgUrl)

const closePre = () => {
  props.fileImg.showTips = false
}

// 判断是否存在图片预览按钮，添加下载按钮
const showImgPre = () => {
  setTimeout(() => {
    const imgPreBtns = document.querySelector('div.el-image-viewer__actions__inner') as HTMLDivElement
    console.log("imgPreBtns------", imgPreBtns);
    const downBtn = document.createElement("span")
    downBtn.innerHTML = "下载"
    imgPreBtns.appendChild(downBtn)
  }, 1)
}


</script>

<style lang="scss">
@import '@/style/index.scss';

.pic {
  height: $file-height;
  padding: 7px;
  text-align: center;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;

  .el-image-viewer__mask {
    opacity: .9;
  }

  .el-image-viewer__actions {
    width: unset;
    min-width: 282px;
  }

  .preview {
    width: $pre-width;
    height: $pre-height;
    border-radius: 5px;
    object-fit: cover;
  }

  // 预览图关闭按钮美化
  .el-image-viewer__close {
    width: 45px;
    height: 45px;
    top: unset;
    // right: 45px;
    background-color: unset !important;
  }

  .el-image-viewer__btn {
    background-color: var(--img-pre-btn-bg);
  }

  .el-image-viewer__btn:hover {
    background-color: var(--img-pre-btn-bg-hover);
  }
}
</style>
