<template>
  <div class="overlay" v-if="isActive">
    <div class="con">
      <h2 class="title">新增 | 修改</h2>
      <div class="content">
        <table>
          <tr>
            <td>用户名</td>
            <td><input type="text" v-model="list.username"></td>
          </tr>
          <tr>
            <td>邮箱</td>
            <td><input type="text" v-model="list.email"></td>
          </tr>
          <tr>
            <td>性别</td>
            <td>
              <label><input type="radio" name="sex" value="男" v-model="list.sex">男</label>
              <label><input type="radio" name="sex" value="女" v-model="list.sex">女</label>
              <label><input type="radio" name="sex" value="未知" v-model="list.sex">未知</label>
            </td>
          </tr>
          <tr>
            <td>省份</td>
            <td>
              <select name="" id="" v-model="list.province">
                <option value="北京市">北京市</option>
                <option value="河北省">河北省</option>
                <option value="河南省">河南省</option>
                <option value="重庆市">重庆市</option>
                <option value="广东省">广东省</option>
                <option value="辽宁省">辽宁省</option>
              </select>
            </td>
          </tr>
          <tr>
            <td>爱好</td>
            <td>
              <label><input type="checkbox" v-model="list.hobby" value="篮球">篮球</label>
              <label><input type="checkbox" v-model="list.hobby" value="读书">读书</label>
              <label><input type="checkbox" v-model="list.hobby" value="插画">插画</label>
              <label><input type="checkbox" v-model="list.hobby" value="编程">编程</label>
              <label><input type="checkbox" v-model="list.hobby" value="弹琴">弹琴</label>
            </td>
          </tr>
        </table>
        <p>
          <input type="button" @click="cancelModify" value="取消">
          <input type="button" @click="modify" value="保存">
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Crud",
  data() {
    return {
      list: {}
    };
  },
  methods: {
    cancelModify() {
      this.$emit("cancel", 123);
    },
    modify() {
      this.$emit("edit", this.list);
    }
  },
  props: ["modifyList", "isActive"],
  computed: {
    selectedList: function() {
      return this.modifyList;
    }
  },
  updated() {
    console.log("updated");
    this.list = this.modifyList;
  },
  beforeRouteEnter(to, from, next) {
    console.log("组件路由勾子：beforeRouteEnter");
    console.log(this); //undefined，不能用this来获取vue实例
    next(vm => {
      console.log("组件路由勾子beforeRouteEnter的next");
      console.log(vm); //vm为vue的实例
    });
  },
  beforeCreate() {
    console.log("beforeCreate");
  },
  created() {
    this.$nextTick(() => {
      console.log("nextTick");
    });
    console.log("created");
  },
  beforeMount() {
    console.log("beforeMount");
  },
  mounted() {
    console.log("mounted");
  },
  beforeUpdate() {
    console.log("beforeUpdate");
  },
  beforeDestroy: function() {
    console.log("beforeDestroy 销毁前状态===============》");
  },
  destroyed: function() {
    console.log("destroyed 销毁完成状态===============》");
  },
  beforeRouteLeave(to, from, next) {
    console.log("组件路由勾子：beforeRouteLeave");
    console.log(this); //可以访问vue实例
    next();
  }
};
</script>

<style>

</style>
