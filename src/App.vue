<template>
  <div>
    <!--  header  -->
    <div class="header">
      <h2 class="header-title">todolist</h2>
      <input
        type="text"
        @keyup.enter="inputEnter"
        placeholder="按回车添加任务"
        class="header-input"
        v-model="inputText"
      />
      <button class="header-button" @click="addClick">添加</button>
    </div>
    <!--  main  -->
    <div class="main" v-if="task.length!==0">
      <h2 class="main-title">待完成</h2>
      <ul class="main-list">
        <li
          v-for="(item, index) in task"
          class="main-list-item"
          v-if="item.active===false"
          :key="index"
        >
          <input type="checkbox" @click="changeActive(index)" />
          <span>{{ item.text }}</span>
          <img src="./assets/img/lajitong.png" @click="removeTask(index)" alt=""/>
        </li>
      </ul>
      <!--        <h2 v-else>目前还没有待办，创建一个吧</h2>    -->
      <h2 class="main-title">已完成</h2>
      <ul class="main-list">
        <li
          v-for="(item, index) in task"
          v-if="item.active===true"
          class="main-list-item"
          :key="index"
        >
          <input type="checkbox" @click="changeActive(index)" checked/>
          <span style="text-decoration: line-through;">{{ item.text }}</span>
          <img src="./assets/img/lajitong.png" @click="removeTask(index)" />
        </li>
      </ul>
    </div>
    <tip v-else></tip>
    <!--  footer  -->
    <div class="footer"></div>

  </div>
</template>

<script>
import Tip from './components/Tip'
export default {
  name: "App.vue",
  data() {
    return {
      inputText:'',
      message: "Hello",
      task: []
    };
  },
  components:{
    Tip
  },
  methods: {
    //input内按下回车
    inputEnter() {
      this.addTask();
    },
    //点击添加按钮
    addClick() {
      this.addTask();
    },
    //添加任务 保存到本地 localStorage
    addTask() {
      if (this.inputText.trim() === "") {
        alert("你还没有输入内容");
      } else {
        let data = this.getTask();
        data.push({ text: this.inputText, active: false });
        localStorage.setItem("todolist", JSON.stringify(data));
      }
      this.inputText = ''
      this.loadTask();
    },
    //获取本地的数据 localStorage
    getTask() {
      let data = localStorage.getItem("todolist");
      if (data !== null) {
        return JSON.parse(data);
      } else {
        return [];
      }
    },
    //修改状态
    changeActive(index) {
      const data = this.getTask();
      data[index].active = !data[index].active;
      localStorage.setItem("todolist", JSON.stringify(data));
      this.loadTask();
    },
    //删除任务
    removeTask(index) {
      const data = this.getTask();
      data.splice(index, 1);
      localStorage.setItem("todolist", JSON.stringify(data));
      this.loadTask();
    },
    //页面渲染 重新获取数据
    loadTask() {
      this.task = this.getTask();
    },
  },
  created() {
    this.loadTask();
  },
  mounted() {
  }
};
</script>

<style lang="less" scoped>
.header {
  width: 50%;
  margin: 0 auto;
  // background-color: red;
  .header-title {
    display: inline-block;
    font-size: 45px;
    // line-height: 90px;
  }
  .header-input {
    height: 30px;
    width: 300px;
    margin-left: 100px;
    border-radius: 5px;
    border: none;
    outline: none;
  }
  .header-button {
    border-radius: 5px;
    height: 35px;
    border: none;
    width: 100px;
    margin-left: 5px;
    font-size: 20px;
  }
}
.main {
  // margin: 50px auto 0 auto;
  margin: 0 auto;
  width: 50%;
  .main-title {
  }
  .main-list {
    .main-list-item {
      position: relative;
      display: block;
      height: 45px;
      background-color: #ccc;
      border-bottom: 3px solid #eee;
      border-left: 4px solid #41b682;
      line-height: 45px;
      input {
        margin: 10px 15px 0 0;
        top: 10px;
        height: 25px;
        width: 25px;
      }
      span {
        // 加一个超出省略点显示，鼠标经过显示全部
        line-height: 45px;
        font-size: 25px;
      }
      img {
        position: absolute;
        width: 20px;
        top: 14px;
        right: 10px;
      }
    }
  }
}
</style>