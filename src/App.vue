
<template>
  <div id="app">
    <div id="container3"> <el-input v-model="innerstyle" type="textarea" :rows="15"></el-input></div>
    <div id="container1">
      <css-doodle click-to-update ref="doodle">{{ innerstyle }}</css-doodle>
    </div>

    <el-scrollbar id="container2">
      <el-form ref="form" :model="form" label-width="110px">
        <el-form-item label="布局背景色">
          <el-color-picker v-model="form.wholebackground"></el-color-picker>
          <el-input v-model="form.wholebackground" size="small"></el-input>
        </el-form-item>
        <el-form-item label="元素数目">
          <el-input v-model="form.itemnum"></el-input>
        </el-form-item>
        <el-form-item label="布局大小">
          <el-input v-model="form.wholesize"></el-input>
        </el-form-item>

        <el-form-item label="布局形状">
          <el-select v-model="form.wholeshape" placeholder="请选择活动区域">
            <el-option label="圆形" value="circle"></el-option>
            <el-option label="三角" value="triangle"></el-option>
            <el-option label="五边" value="pentagon"></el-option>
            <el-option label="六边" value="hexagon"></el-option>
            <el-option label="八边" value="octagon"></el-option>
            <el-option label="星星" value="star"></el-option>
            <el-option label="无穷" value="infinity"></el-option>
            <el-option label="心" value="heart"></el-option>
            <el-option label="内三角" value="(hypocycloid, 3)"></el-option>
            <el-option label="内四边" value="(hypocycloid, 4)"></el-option>
            <el-option label="内五边" value="(hypocycloid, 5)"></el-option>
            <el-option label="三叶草" value="(clover, 3)"></el-option>
            <el-option label="四叶草" value="(clover, 4)"></el-option>
            <el-option label="五叶草" value="(clover, 5)"></el-option>
            <el-option label="三圆角" value="(bud, 3)"></el-option>
            <el-option label="五圆角" value="(bud, 5)"></el-option>
            <el-option label="风车" value="windmill"></el-option>
            <el-option label="花瓶" value="vase"></el-option>
            <el-option label="水滴" value="drop"></el-option>
            <el-option label="鱼" value="fish"></el-option>
            <el-option label="豆子" value="bean"></el-option>
            <el-option label="鲸鱼" value="whale"></el-option>
            <el-option label="微笑" value="bicorn"></el-option>
             <el-option label="无" value=""></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="布局样式">
          <el-input
            type="textarea"
            v-model="form.wholestyle"
            placeholder="请输入其他布局样式"
          ></el-input>
        </el-form-item>

        <el-form-item label="click动画">
          <el-input
            v-model="form.itemtransition"
            type="textarea"
            :rows="2"
          ></el-input>
        </el-form-item>
        <el-form-item label="元素变化">
          <el-input
            v-model="form.itemtransform"
            style="width: 300px"
            type="textarea"
            placeholder="请输入元素样式，主要是变形"
          ></el-input>
        </el-form-item>
        <el-form-item label="元素样式">
          <el-input
            v-model="form.itemcolor"
            style="width: 300px"
            type="textarea"
            placeholder="请输入元素样式，主要是颜色"
            :rows="3"
          ></el-input>
        </el-form-item>
        <el-form-item label="子类元素选择">
          <el-input
            v-model="form.nth"
            placeholder="请输入数字或者表达式，如2n"
          ></el-input>
        </el-form-item>
        <el-form-item label="子类元素样式">
          <el-input
            type="textarea"
            v-model="form.nthinner"
            placeholder="请输入选中元素的样式"
          ></el-input>
        </el-form-item>
        <el-form-item label="伪类元素样式">
          <el-input
            type="textarea"
            v-model="form.after"
            placeholder="请输入伪元素的样式"
          ></el-input>
        </el-form-item>
        <el-form-item label="触发样式">
          <el-input
            type="textarea"
            v-model="form.hover"
            placeholder="请输入选中伪元素2的样式"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button ref='update' type="success" @click="update">刷新</el-button>
          <el-button @click="download" type="primary">保存</el-button>
          <el-button id="cancel" @click="cancelAll">清空</el-button>
        </el-form-item>
      </el-form>
    </el-scrollbar>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        itemnum: "7x7",
        wholesize: "500px",
        wholeshape: "circle",
        wholebackground: " #0a0c27",
        wholestyle: "",
        itemcolor: `background: hsla(calc(240 - 6 * @x * @y),70%, 68%, @r.8);`,
        itemtransform: "transform: scale(@r(.25, 1));",
        itemtransition: "transition: .2s ease-in-out @r(.6s);",
        nth: "",
        nthinner: "",
        after: "",
        hover: "",
      },
    };
  },
  computed: {
    innerstyle: function () {
      return `:doodle {
    @grid: ${this.form.itemnum}/${this.form.wholesize} ;
    @shape: ${this.form.wholeshape};
    background: ${this.form.wholebackground}; ${this.form.wholestyle}
  }
      :doodle(:hover){${this.form.hover}}
     @nth(${this.form.nth}) {${this.form.nthinner}}
     :after {${this.form.after}}  
${this.form.itemtransform}${this.form.itemcolor}
       ${this.form.itemtransition}`;
    },
  },
  methods: {
    cancelAll: function(){
      for (var key in this.form) {
        this.form[key] = "";
      }
    },
    update: function(){
      var doodle=this.$refs.doodle
      doodle.update(this.innerstyle);
    },
    download:async function(){
        var doodle=await this.$refs.doodle
        await doodle.export({
        scale: 6,
        download: true,
      });
    }
  },
};
</script>

<style>
body {
  background-image: linear-gradient(
    to right,
    #d9fa1cad,
    #0ea5e9,
    rgba(34, 211, 238, 0)
  );
  background-size: 400%;
  animation: back 30s linear infinite;
}
#app {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
#container1 {
  width: 50%;
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}
#container2 {
  width: 30%;
  position: relative;
  z-index: 3;
  background-color: white;
  height: 105%;
  display: flex;
  align-items: center;
  justify-content: center;
}
#container3{
  width: 20%;
}
.el-textarea__inner {
  font-family: "Microsoft";
  font-size: 20px;
}
@keyframes back {
  0% {
    background-position: 15%;
  }
  50% {
    background-position: 45%;
  }
  100% {
    background-position: 15%;
  }
}
</style>