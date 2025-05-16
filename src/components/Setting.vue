<template>
  <!--  <li class="element-title"><span>主页背景</span></li>-->
  <!--  <li><a>预设背景</a></li>-->
  <!--  <li><a>自定义</a></li>-->
  <li class="element-title"><span>自定义书签</span></li>
  <li><a @click="download_json()">备份Json</a></li>
  <li><label for="files"><a>上传Json</a></label></li>
  <input id="files" ref="refFile" style="display: none" type="file" v-on:change="upload_json">
  <li><a @click="recover_json()">恢复默认</a></li>


  <!--  <li><a>登录</a></li>-->
  <!--  <li><a>云端保存</a></li>-->
  <li class="element-title"><span>关于本项目</span></li>
  <div>
    <p style="color: red">无云端功能，本次构建版本为v3.0.1。</p>
    <h4>作者：</h4>
    <p>狐狸导航，www.foxccs.cn，<br>群 946392970</p>
    <h4>项目介绍：</h4>
    <p>本项目使用Vue3构建，推荐使用Chrome内核浏览器。</p>
    <h4>项目地址：</h4>
    <p>www.foxccs.cn</p>
    <h4>BUG反馈：</h4>
    <a href="https://www.foxccs.cn/">狐狸导航</a>
    <h4>版权声明：</h4>
    <p>本项目开源。</p></div>
</template>

<script>
import storage from "@/utils/storage";

export default {
  name: "Setting",
  data() {
    return {
      data: "",
      setting: ""
    };
  },
  methods: {
    download_json() {
      let json_data;
      json_data = storage.get("bookmarks")
      json_data = JSON.stringify(json_data, null, 4)
      const element = document.createElement('a')
      element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(json_data))
      element.setAttribute('download', 'bookmarks.json')
      element.style.display = 'none'
      element.click()
    },
    upload_json() {
      const selectedFile = this.$refs.refFile.files[0];
      let reader = new FileReader();
      reader.readAsText(selectedFile);
      reader.onload = function () {
        try {
          storage.set("bookmarks", JSON.parse(this.result))
        } catch (error) {
          console.log("解析失败，你的文件可能有问题。")
        }


      }
    },
    recover_json() {
      storage.set("bookmarks", "")
    }
  },
}
</script>

<style lang="less" scoped>

</style>
