<template>
  <div class="cart">
    <h2>购物车</h2>
    <table>
      <tr>
        <th>勾选</th>
        <th>课程名称</th>
        <th>课程价格</th>
        <th>数量</th>
        <th>价格</th>
      </tr>
      <tr v-for="(item,index) in courseItem" :key="item.id">
        <td>
          <input type="checkbox" v-model="item.isActive">
        </td>
        <td>{{item.name}}</td>
        <td>{{item.price}}</td>
        <td>
          <button @click="minus(item.id)">-</button>
          {{item.number}}
          <button @click="add(item.id)">+</button>
        </td>
        <td>{{item.price*item.number}}</td>
      </tr>
      <tr v-if="courseItem.length>0">
        <td></td>
        <td colspan="2">{{isActiveCourse}}/{{allCourseList}}</td>
        <td colspan="2">{{allPrice}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'Cart',
    // 遵守单项数据流
    props: ["courseItem"],
    data() {
      return {}
    },
    methods: {
      minus(id) {
        let item = this.courseItem.find(item => item.id === id);
        if (item.number > 1) {
          this.$emit('minus', id);
        } else {
          if (window.confirm("确定要删除吗？")) {
            this.$emit('removeItem', id);
          }
        }
      },
      add(id) {
        this.$emit('add', id);
      }
    },
    computed: {
      isActiveCourse() {
        return this.courseItem.filter(item => item.isActive).length
      },
      allCourseList() {
        return this.courseItem.length;
      },
      allPrice() {
        let num = 0;
        this.courseItem.forEach(item => {
          if (item.isActive) {
            num += item.price * item.number;
          }
        });
        return num;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
