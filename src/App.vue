<template>
  <div class="container vcenter">
    <div id="content" >
      <div id="edit" v-show="!showResult">
        <form v-on:submit.prevent="onSubmit">
              <h5>公交站点意见征求</h5>
            
                <p>公交线路走向是否科学？公交站点设置是否合理？首末时间、发车间隔如何确定？这些公交出行信息与群众的生活息息相关。如果您对我市公交线路走向、站点设置、发车间隔等有更好的建议，可以在下方给我们留言。
</p>
              <label for="phone">联系电话 <i class="fa fa-check-circle text-success" v-if="isPhone"></i> <i class="fa fa-info-circle text-error" v-else></i></label>
              <input class="u-full-width" type="text" placeholder="手机或者固话" id="phone" v-model="phone">
          <label for="comment">您的建议 <i class="fa fa-check-circle text-success" v-if="isBody"></i> <i class="fa fa-info-circle text-error" v-else></i></label>
          <textarea class="u-full-width" placeholder="最少10字最多500字" id="comment" v-model="comment"></textarea>
          <input class="button-primary" :disabled="!(isPhone&&isBody)" type="submit" value="提交">
          </form>
      </div>
      <div id="finish" class="text-center fadeInUp" v-show="showResult">
        <h1 class="text-success" v-if="isPersist"><i class="fa fa-check-circle"></i></h1>
        <h1 class="text-error" v-else><i class="fa fa-times-circle"></i></h1>
        <span class="tips">{{tips}}</span>
        <p style="margin-top:15px">{{result}}</p>
      </div>
    </div>
  </div>
  </template>

  <script>
    import * as validator from './validator'
    export default {
      data: function () {
        return {
          phone: '',
          comment: '',
          showResult: false,
          isPersist: false,
          result: '',
          tips: ''
        }
      },
      methods: {
        onSubmit: function () {
          this.$http.post('http://temp.app/api/guestbook', {phone: this.phone, comment: this.comment}).then((response) => {
            this.isPersist = true
            this.tips = response.body
            this.result = '感谢您对我们工作的关注与支持！我们会在第一时间认真研究您的反馈，并综合实际情况进行下一步的工作。再次感谢您！'
            this.showResult = true
          }, (response) => {
            this.isPersist = false
            this.result = '提交未能成功，可能是网络故障'
            this.showResult = true
            this.tips = response.body
          })
        }
      },
      computed: {
        isPhone: function () {
          return validator.isPhone(this.phone) || validator.isMobile(this.phone)
        },
        isBody: function () {
          return validator.minlength(this.comment, 10) && validator.maxlength(this.comment, 500)
        }
      }
    }
  </script>

  <style>
   /* .overlay{
      position: fixed;
      overflow-y: scroll;
      height: 100%;
      -webkit-box-pack: center;
      -ms-flex-pack: center;
      justify-content: center;
      left: 0;
      top: 0;
      width: 100%;
      background-image: url(http://jzjtj.qiniudn.com/images/banner/2.jpg);
      background-size: cover;
      background-attachment: fixed;
      background-position: bottom left;
      background-color: #38335f;
      z-index: 9;
    }*/
    /*.overlay_mask {
      position: fixed;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      z-index: 10;
      background: rgba(0,5,8,.75);
    }*/
    /*.overlay_content {
      position: relative;
      box-sizing: border-box;
      padding-left: 10px;
      padding-right: 10px;
      z-index: 22;
      -webkit-box-flex: 1;
      -ms-flex: 1;
      flex: 1;
    }*/
    #content {
    position: relative;
    margin: 50px auto;
    min-height: 200px;
    z-index: 100;
    padding: 30px;
    border: 1px solid #383838;
    background: #D1D1D1;
    background: -moz-repeating-linear-gradient(-45deg, #EFC1CB, #EFC1CB 30px, #F2F2F2 30px, #F2F2F2 40px, #C2E8F5 40px, #C2E8F5 70px, #F2F2F2 70px, #F2F2F2 80px);
    background: -webkit-repeating-linear-gradient(-45deg, #EFC1CB, #EFC1CB 30px, #F2F2F2 30px, #F2F2F2 40px, #C2E8F5 40px, #C2E8F5 70px, #F2F2F2 70px, #F2F2F2 80px);
    background: -o-repeating-linear-gradient(-45deg, #EFC1CB, #EFC1CB 30px, #F2F2F2 30px, #F2F2F2 40px, #C2E8F5 40px, #C2E8F5 70px, #F2F2F2 70px, #F2F2F2 80px);
    background: repeating-linear-gradient(-45deg, #EFC1CB, #EFC1CB 30px, #F2F2F2 30px, #F2F2F2 40px, #C2E8F5 40px, #C2E8F5 70px, #F2F2F2 70px, #F2F2F2 80px);
    -webkit-border-radius: 8px;
    -moz-border-radius: 8px;
    border-radius: 8px;
    -webkit-box-shadow: 0px 1px 6px #3F3F3F;
    -moz-box-shadow: 0px 1px 6px #3F3F3F;
    box-shadow: 0px 1px 6px #3F3F3F;
}
#content:after {
    background: #F9F9F9;
    margin: 10px;
    position: absolute;
    content: " ";
    bottom: 0;
    left: 0;
    right: 0;
    top: 0;
    z-index: -1;
    border: 1px #E5E5E5 solid;
    -webkit-border-radius: 8px;
    -moz-border-radius: 8px;
    border-radius: 8px;
}
h5 {
  text-align: center;
  border-bottom: 2px solid rgb(193, 83, 118);
}
h1 {
  margin-bottom: 0 !important;
}
p {
  line-height: 1.5em;
  text-indent: 2em;
  color:#ad605a;
}
.tips {
  color: #157b9b;
}
  </style>
}
