
<template>
    <div>
        <h2>小词台</h2>
        <div id="word-text-area">
            <el-input type="textarea" :rows="12"  placeholder="请输入内容,将生成词台"  resize="none" v-model="textarea">
            </el-input>
            <div id="word-img">
                <el-image :src="'data:image/png;base64,'+pic" :fit="fit">
                    <div slot="error" class="image-slot">
                        <i class="el-icon-picture-outline"></i>
                    </div>
                </el-image>
            </div>
            <div id="word-operation">
                <el-row>
                    <el-button type="primary" @click="onSubmit" round>生小词台</el-button>
                    <el-button type="success" @click="onDownload" round>下载图片</el-button>
                </el-row>
            </div>
        </div>
    </div>
</template>


<script>
     export default {
         name: 'Wordcloud',
         data() {
             return {
                 textarea: '',
                 pic: "",
                 pageTitle: 'Flask Vue Word Cloud',
             }
         },
         methods: {
             onSubmit() {
                 var param = {
                     "word": this.textarea
                 }
                 // 请求 地址，生成 图片
                 this.axios.post("/word/cloud/generate", param).then(
                     res => {
                         this.pic = res.data
                         console.log(res.data)
                     }
                 ).catch(res => {
                     console.log(res.data.res)
                 })
             },
             onDownload() {
                 const imgUrl = 'data:image/png;base64,' + this.pic
                 const a = document.createElement('a')
                 a.href = imgUrl
                 a.setAttribute('download', 'word-cloud')
                 a.click()
             }
         }
     }
 </script>


<style scoped>
    #word-text-area {
        margin-left: 20%;
        margin-right: 20%;
    }
    #word-operation {
        margin-top: 20px;
    }
    #word-img {
        width: 800px;
        height: 300px;
        margin: 20px;
    }
</style>
