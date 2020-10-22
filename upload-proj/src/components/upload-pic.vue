<template>
  <div>
    <h1>uploadPic</h1>
    <div class="upload">
      <div class="upload-title">
        <h3>上传图片</h3>
        <small><i style="color:coral;">点击图片可以浏览大图，双击关闭</i></small>
      </div>
      <div class="show-pic" v-if="imgUrl!=''">
        <img :src="imgUrl" id="img" v-on:click="view()" @dblclick="nodisplay()" />
        <img :src="imgUrl" id="bigImg" />
      </div>
      <div class="upload-btn">
        <input v-show="false" type="file" accept="image/jpeg,image/jpg,image/png" @change="tirggerFile($event)" ref="input" />
        <button @click="openImg()">选择图片</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        imgUrl: '',
        imgList: []
      };
    },
    methods: {
      tirggerFile(event) {
        // let 声明的变量只在 let 命令所在的代码块内有效。
        let file = event.target.files[0]; //不管选多少个图片，都只要第一个，限定只能上传一张
        var reader = new FileReader(); //解析器
        reader.readAsDataURL(file); //readAsDataURL方法会使用base-64进行编码，编码的资料由data字串开始，后面跟随的是MIME type，然后再加上base64字串，逗号之后就是编码过的图像文件的内容。
        // //console.log(this)
        let that = this;
        reader.onload = function(e) {
          that.imgUrl = this.result;
          //console.log(that.imgUrl)
        }
        let data = new FormData(); // 创建一个FormData对象,用来组装一组用 XMLHttpRequest发送请求的键/值对
        data.append("file", file, file.name);
        //console.log(data.get('file'));
        this.axios.post("http://127.0.0.1:3000/upload", data, {
          headers: {
            "content-type": "multipart/form-data"
          }
        }).then(function(res) {
          console.log(res.data);
        });
      },
      openImg() {
        this.$refs.input.click();
      },
      view() {
        document.getElementById('bigImg').setAttribute('style', 'display: block;')
      },
      nodisplay() {
        document.getElementById('bigImg').setAttribute('style', 'display: none')
      }
    }
  }
</script>

<style>
  .upload {
    width: 85%;
    border: #777 1px solid;
    border-radius: 10px;
    margin: auto;
  }

  .upload-title {
    padding: 0.5rem;
    background-color: #444;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }

  .upload-title h3 {
    color: aliceblue;
    margin: 0;
  }

  .show-pic #img {
    margin: 1rem;
    width: 100px;
    height: 100px;
    border-radius: 10%;
    /* position: absolute; */
  }

  .show-pic #img:hover .show-pic #bigImg {
    display: block;
  }

  .show-pic #bigImg {
    display: none;
    position: relative;
    left: 25%;
    margin: auto;
    width: 60%;
    margin: 0.5rem;
  }

  .upload-btn {
    padding: 1rem;
    text-align: end;
  }

  .upload-btn button {
    background-color: #444444;
    color: #F0F8FF;
    font-size: 14px;
    font-weight: bold;
    border-radius: 10px;
    padding: 0.5rem;
  }

  .upload-btn button:hover {
    background-color: darkslateblue;
    color: #F0F8FF;
  }
</style>
