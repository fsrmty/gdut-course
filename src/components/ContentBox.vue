<template>
<div class="container">
    <div class="nav_box">
        <h2 class="title">目录导航</h2>
        <ul class="nav_ul">
            <li class="nav_li" :id="index" v-for="(item, index) in fileTree" :key="index" @click="handleClick_nav">
                {{ item.name }}
            </li>
        </ul>
    </div>
    <div class="file_list">
        <h2 class="title">文件列表</h2>
        <ul class="file_ul">
            <li class="firstline file_li">
                <div class="left">名称</div>
                <div class="right">最后更新时间</div>
            </li>
            <li class="file_li" v-for="(item, index) in nav_curr.children" :key="index">
                <span v-if="item.children instanceof Array" class="file_name" :id="index" v-on:click="handleClick_file">{{ item.name }}</span>
                <a v-else v-bind:href="item.url" class="file_name" target="_blank">{{ item.name }}</a>
                <span class="file_mtime">{{ item.mtime }}</span>
            </li>
            <li class="back_btn" v-on:click="handleClick_back">返回上一步</li>
        </ul>
    </div>
</div>
</template>

<script>
import fileTree from '../db.js';

export default {
    data() {
        return {
            fileTree,
            nav_curr: fileTree[2],
            nav_curr_back: fileTree[2],
        }
    },
    methods: {
        handleClick_nav(e) {
            this.nav_curr_back = this.nav_curr;
            this.nav_curr = fileTree[e.target.id];
        },
        handleClick_file(e) {
            this.nav_curr_back = this.nav_curr;
            this.nav_curr = this.nav_curr.children[e.target.id];
        },
        handleClick_back() {
            this.nav_curr = this.nav_curr_back;
        }
    },
}
</script>

<style scoped>
.container {
    width: 90%;
    margin: 1rem auto;
    font-size: 0.8rem;
    text-align: left;
}
.title {
    color: rgb(41, 180, 138);
    font-size: 1rem;
    padding-bottom: 1rem;
}
/********************** 目录导航的样式 **************************/
.nav_ul {
    overflow: hidden;
    padding-bottom: 1rem;
}
.nav_li {
    float: left;
    margin-left: 1rem;
}
.nav_li:hover {
    color: rgb(41, 180, 138);
    cursor: pointer;
}
/********************** 目录导航的样式 **************************/
/********************** 文件列表的样式 **************************/
.file_ul {
    margin-left: 1rem;
    padding-bottom: 1rem;
}
.firstline {
    overflow: hidden;
    border-bottom: 1px solid;
    margin-bottom: 0.4rem;
}
.firstline .left {
    float: left;
}
.firstline .right {
    float: right;
}
.file_li {
    padding: 0.4rem;
    padding-top: 0;
    overflow: hidden;
}
.file_li:hover {
    background: rgba(41, 180, 138, 0.4);
}
.file_name {
    cursor: pointer;
    float: left;
}
a.file_name {
    color: #09c;
}
.file_mtime {
    float: right;
}
.back_btn {
    font-size: 0.6rem;
    padding: 0.3rem;
    background: rgba(41, 180, 138, 0.4);
    display: inline-block;
    border-radius: 0.2rem;
    margin-left: 0.4rem;
    margin-top: 0.4rem;
}
.back_btn:hover {
    cursor: pointer;
    background: rgba(41, 180, 138, 0.8);
}
/********************** 文件列表的样式 **************************/
</style>
