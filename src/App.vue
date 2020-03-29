<template>
  <div id="app">
    <h1>{{title}}</h1>
    <hr>

    <div>
      <h2>添加课程</h2>
      <div>
        <label for="courseName">课程名称:</label>
        <input id="courseName" type="text" v-model="courseInfo.name">
      </div>
      <div>
        <label for="coursePrice">课程价格:</label>
        <input id="coursePrice" type="text" v-model="courseInfo.price">
      </div>
      <div>
        <button id="addCourse" @click="addCourseToList">添加课程到列表</button>
      </div>
    </div>

    <hr>

    <div class="course-list">
      <h2>课程列表</h2>
      <table class="table">
        <tr>
          <th>课程名称</th>
          <th>课程价格</th>
          <th>操作</th>
        </tr>
        <tr v-for="(item,index) in courseList" :key="item.id">
          <td>{{item.name}}</td>
          <td>{{item.price}}</td>
          <td>
            <button @click="addCourseToCart(item.id)">添加到购物车</button>
          </td>
        </tr>
      </table>
    </div>
    <hr/>

    <cart class="cart" :courseItem="courseItem" @removeItem="remove" @minus="minus" @add="add"></cart>
  </div>
</template>

<script>
  import Cart from './components/Cart'

  export default {
    name: 'App',
    components: {
      Cart
    },
    data() {
      return {
        title: "购物车",
        courseInfo: {
          name: "",
          price: "",
          id: ""
        },
        courseList: [{
          id: 0,
          name: "web全栈构架师",
          price: 9999
        }, {
          id: 1,
          name: "python人工智能",
          price: 8888
        }],
        courseItem: []
      }
    },
    methods: {
      addCourseToList() {
        this.courseInfo.id = +new Date();
        this.courseList.push(this.courseInfo);
        this.courseInfo = {};
      },
      addCourseToCart(id) {
        let item = this.courseList.find(item => item.id == id);
        let isHasCourse = this.courseItem.find(val => val.id === item.id);
        if (isHasCourse) {
          isHasCourse.number++;
        } else {
          this.courseItem.push({
            ...item,
            number: 1,
            isActive: true
          })
        }
      },
      remove(id) {
        this.courseItem = this.courseItem.filter(item => item.id !== id);
      },
      minus(id) {
        this.courseItem.find(item => item.id == id).number--;
      },
      add(id) {
        this.courseItem.find(item => item.id == id).number++;
      }
    }
  }
</script>

<style scoped lang="less">
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;

    #addCourse {
      margin: 5px;
    }

    .course-list {
      display: flex;
      align-items: center;
      flex-direction: column;
      justify-content: center;
    }

    .cart {
      display: flex;
      align-items: center;
      flex-direction: column;
      justify-content: center;
    }
  }
</style>
