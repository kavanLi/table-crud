<template>
  <div class="container">
    <input type="button" value="新增" class="add" @click="add">
    <div>
      <table>
        <thead>
          <tr>
            <th v-for="head in header" v-bind:key="head.id" v-cloak>{{ head }}</th>
          </tr>          
        </thead>
        <tbody>
          <tr v-for="(student, index) in students" v-bind:key="student.index" v-cloak>
            <td>{{index+1}}</td>
            <td v-for="value in student" v-bind:key="value.id">{{ value.toString() }}</td>
            <td><button v-on:click="edit(index)">修改</button><button @click="del(index)">删除</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    
    <!-- 外部组件,作为子组件,与Crud组件的关系是父子组件关系, modifylist用kebab-case(短横线分隔式命名),因为
    HTML 特性是不区分大小写的。所以，当使用的不是字符串模板时，camelCase (驼峰式命名) 的 prop 需要转换为相对应的 kebab-case (短横线分隔式命名)： -->
      <crud class="just-try-something-and-press-f12" data-would-be-add-to-root-component-attr="true" v-bind:modify-list="selectedList" v-bind:is-active="isActive" v-on:edit="editOne" v-on:cancel="foobar = arguments[0];isActive=!isActive" v-cloak></crud>
  </div>
</template>

<script>
// https://www.xiabingbao.com/vue/2017/07/10/vue-curd.html
import Crud from "./Crud";

export default {
  name: "Table",
  components: { Crud },
  data() {
    return {
      foobar: 0,
      selectedList: {},
      isActive: false,
      header: ["id", "用户名", "email", "性别", "省份", "爱好", "编辑"],
      students: [
        {
          username: "李明",
          email: "li@qq.com",
          sex: "男",
          province: "北京市",
          hobby: ["篮球", "编程"]
        },
        {
          username: "韩红",
          email: "han@163.com",
          sex: "女",
          province: "河北省",
          hobby: ["弹琴", "插画"]
        }
      ]
    };
  },
  filters: {
    filter: function(array) {
      // 数组的toString方法能输出
      return array.toString();
    }
  },
  methods: {
    del(index) {
      this.students.splice(index, 1);
    },
    add() {
      var init = {
        username: "",
        email: "",
        sex: "",
        province: "",
        hobby: []
      };
      this.selectedList = JSON.parse(JSON.stringify(init));
      this.toggleEdit();
    },
    editOne(student) {
      var dupe = 0;
      // 判断编辑行的username是否重复, 如果重复则覆盖, 通过dupe自增, 如果没有重复, 则students的元素个数会等于dupe, 那么再判断结果来是否进行增加一条数据.
      this.students.forEach((element, index) => {
        if (student.username == element.username) {
          this.students[index] = student;
        } else {
          dupe++;
        }
      });
      if (dupe === this.students.length) {
        this.students.push(student);
      }
      this.toggleEdit();
    },
    cancleOne() {
      this.toggleEdit();
    },
    edit(index) {
      // 一定要分别尝试这两断代码的区别!!! 两端代码要做同一件事就是传递表单的对象给子组件
      // 下面这一句通过转换以后, 传递的selectedList对象是持有内存中新的引用的student
      this.selectedList = JSON.parse(JSON.stringify(this.students[index]));

      // 而下面这句持有的引用和表单中的student的引用是一致的, 即传递给子组件的student对象在进行属性编辑的时候, 也会实时改变父组件表单中的内容, 就算点击取消按钮也不会还原修改, 运行实践.
      // this.selectedList = this.students[index]
      this.toggleEdit();
    },
    toggleEdit() {
      this.isActive = !this.isActive;
    }
  },
  beforeRouteEnter(to, from, next) {
    console.log("组件路由勾子：beforeRouteEnter, 下一行打印自身this");
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
  updated() {
    console.log("updated");
  },
  beforeDestroy: function() {
    console.log("beforeDestroy 销毁前状态===============》");
  },
  destroyed: function() {
    console.log("destroyed 销毁完成状态===============》");
  },
  beforeRouteLeave(to, from, next) {
    console.log("组件路由勾子：beforeRouteLeave, 下一行打印自身this");
    console.log(this); //可以访问vue实例
    next();
  }
};
</script>

<style>
  @import "../assets/css/main.css";
</style>
