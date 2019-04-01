<template>
  <div id="app">
    <div class="main" id="main">
      <div class="header">
        <img src="./assets/touxiang.jpg" alt="无法显示头像" />
        <div class="header-info">
          <h1>{{ info.name }}</h1>
          <h3>{{ info.post }}</h3>
          <p>{{ info.motto }}</p>
        </div>
      </div>
      <div class="content">
        <section v-for="(item, index) in info.content" :key="index">
          <span>{{ item.title }}</span>
          <div
            class="item"
            v-for="(itemitem, indexindex) in item.items"
            :key="indexindex + 2019"
          >
            <h5 class="item-time">{{ itemitem.time }}</h5>
            <h5 class="item-name">{{ itemitem.name }}</h5>
            <h5 class="item-position">{{ itemitem.position }}</h5>
            <p class="item-description">{{ itemitem.description }}</p>
            <ul class="item-list">
              <li
                v-for="(list, listindex) in itemitem.list"
                :key="listindex + 1000"
              >
                <i></i>
                {{ list }}
              </li>
            </ul>
          </div>
        </section>
      </div>
    </div>
    <div class="footer">
      <button @click="download">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-PROFILE"></use>
        </svg>
        PDF简历
      </button>
      <p class="latest-update-time">最后更新：{{ info.latesttime }}</p>
      <p class="github">
        <a href="https://github.com/FangMingHong/resume" target="_blank">
          <svg class="icon" aria-hidden="true">
            <use xlink:href="#icon-github"></use>
          </svg>
        </a>
      </p>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import './css/iconfont.js'; // 阿里巴巴矢量图
import info from '../info.json';
import { constants } from 'fs';
import html2canvas from 'html2canvas';
import jspdf from 'jspdf';
export default {
  name: "app",
  data() {
    return {
      info:{}
    }
  },
  methods:{
    download() {
      var element = document.querySelector('#main');
      var w = element.clientWidth;    
      var h = element.clientHeight;    
      var canvas = document.createElement("canvas");
      var scale = 2
      canvas.width = w * scale;    
      canvas.height = h * scale;
      canvas.getContext("2d").scale(scale, scale);
      var opts = {
        scale: scale,
        canvas: canvas,
        width: w,
        height: h,
        useCORS: true
      }
      let that = this;   
      html2canvas(element,opts).then(function(canvas) {
        var contentWidth = canvas.width;
        var contentHeight = canvas.height;
        var pageHeight = contentWidth / 592.28 * 841.89;
        var leftHeight = contentHeight;
        var position = 0;
        var imgWidth = 595.28;
        var imgHeight = 592.28/contentWidth * contentHeight;
        var pageData = canvas.toDataURL('image/jpeg', 1.0);
        var pdf = new jspdf('', 'pt', 'a4');
        if (leftHeight < pageHeight) {
        pdf.addImage(pageData, 'JPEG', 0, 0, imgWidth, imgHeight);
        } else {    
            while(leftHeight > 0) {
                pdf.addImage(pageData, 'JPEG', 0, position, imgWidth, imgHeight)
                leftHeight -= pageHeight;
                position -= 841.89;
                if(leftHeight > 0) {
                  pdf.addPage();
                }
            }
        }
        pdf.save(that.info.name+'的简历.pdf');       
      })
    }
  },
  created() {
    this.info = info
  }
};
</script>

<style lang="less">
@import url('./css/reset.css');
@color:#2d8cf0; // 主色
@background-color:#f8f8f9; // 背景颜色
@text-color:#657180; // 正文颜色
@content-title-color:#254665; // 内容标题颜色
@font-family:"Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif; // 正文字体
@font-family-title: 'Lucida Grande', 'Hiragino Sans GB', 'Hiragino Sans GB W3', @font-family; // 标题字体
body,html {
  background-color: @background-color;
  color:@text-color;
}
#app {
  font-family: @font-family;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
  color:@text-color;
  .main {
    box-shadow: 0 0 15px silver;
    -webkit-box-shadow: 0 0 15px silver;
    margin: 0 auto;
    background-color: #fff;
    .header {
      background-color: @color;
      width: 100%;
      padding: 20px;
      color:#fff;
      overflow: hidden;
      h1 {
        font-size: 50px;
        margin-bottom: 10px;
      }
      h3 {
        font-size: 30px;
        margin-bottom: 10px;
      }
      p {
        font-size: 16px;
      }
      img {
        display: none;
      }
    }
    .content {
      padding: 20px;
      section {
        width: 100%;
        margin-bottom: 10px;
        span {
          color:@content-title-color;
          border: 2px solid @content-title-color;
          padding: 4px;
          display: inline-block;
        }
        .item-time {
          margin: 6px 0;
          font-weight: bold;
        }
        .item-name {
          font-weight: bold;
        }
        .item-position {
          font-weight: 600;
          clear:both;
          margin: 6px 0;
        }
        .item-description {
          word-wrap:break-word;
          word-break:break-all;
          text-align: justify;
        }
        .item-list {
          width: 100%;
          // background-color: skyblue;
          li {
            line-height: 20px;
            i {
              display: inline-block;
              width: 4px;
              height: 4px;
              position: relative;
              top: -3px;
              background-color: @color;
              // border-radius: 50%;
              margin-right: 10px;
            }
          }
        } 
      }
    }
    @media screen and (min-width: 1200px) {
        width:76%;
        min-height: 600px;
        font-size: 20px;
        margin-top: 40px;
        .header {
          height: 200px;
          img {
            display: block;
            float: left;
            height: 100%;
          }
          .header-info {
            float: left;
            margin-left: 30px;
          }
        }
        .content {
          section {
            // min-height: 160px;
            .item {
              width: 100%;
              overflow: hidden;
              margin-bottom: 10px;
              font-size: 16px;
              .item-time {
                float: left;
              }
              .item-name {
                float: right;
              }
            }
          }
        }
    }
    @media screen and (min-width: 800px) and (max-width: 1199px) {
        width:76%;
        min-height: 600px;
        font-size: 20px;
        margin-top: 40px;
        .header {
          height: 200px;
          img {
            display: block;
            float: left;
            height: 100%;
          }
          .header-info {
            float: left;
            margin-left: 30px;
          }
        }
        .content {
          section {
            // min-height: 160px;
            .item {
              width: 100%;
              overflow: hidden;
              margin-bottom: 10px;
              font-size: 16px;
              .item-time {
                float: left;
              }
              .item-name {
                float: right;
              }
            }
          }
        }
    }
    @media screen and (min-width: 480px) and (max-width: 799px) {
        width:90%;
        min-height: 600px;
        font-size: 2rem;
        .header {
          margin-top: 20px;
          height: 200px;
          .header-info {
            text-align: center;
            h3 {
              margin: 2rem;
            }
          }
        }
        .content {
          section {
            // min-height: 160px;
            .item {
              width: 100%;
              overflow: hidden;
              margin-bottom: 10px;
              font-size: 16px;
              .item-time {
                float: left;
              }
              .item-name {
                float: right;
              }
            }
          }
        }
    }
    @media screen and (max-width: 479px) {
        width:100%;
        min-height: 600px;
        font-size: 20px;
        .header {
          height: 200px;
          .header-info {
            text-align: center;
            h1 {
              letter-spacing: 10px;
            }
            h3 {
              font-size: 20px;
              margin: 20px;
            }
          }
        }
        .content {
          font-size: 16px;
        }
    }
  }
  .footer {
    margin-top: 20px;
    text-align: center;
    font-size: 1.4rem;
    color:#808695;
    p {
      margin-bottom: .4rem;
    }
    a {
      text-decoration: none;
      color:#808695;
      font-size: 1.8rem;
    }
    button {
      padding: 8px 20px;
      color: @text-color;
      background-color: @background-color;
      border: none;
      cursor: pointer;
      border-radius: 4px;
      outline: none;
      &:hover {
        color:@color;
      }
      svg {
        font-size: 2rem;
      }
    }
  }
}
</style>
