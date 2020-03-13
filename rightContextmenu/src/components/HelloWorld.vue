<template>
  <div class="hello">
    <div class="treesclass">
      <my-tree :data="theData" :name="menuName" :loading="loading" @getSubMenu="getSubMenu"></my-tree>
    </div>
    <ul class="dropdown-menu" id="msgRightMenu">
      <li>
        <a id="adddepart" style="text-align:center">添加</a>
        <!--// @click="addDepart" -->
      </li>
      <li>
        <a href="#" style="text-align:center">修改</a>
      </li>
      <li>
        <a href="#" style="text-align:center">删除</a>
      </li>
    </ul>
  </div>
</template>
<script>
const myData = [
  {
    id: "1",
    menuName: "基础管理",
    menuCode: "10"
  },
  {
    id: "2",
    menuName: "商品管理",
    menuCode: "20"
  },
  {
    id: "3",
    menuName: "订单管理",
    menuCode: "30",
    children: [
      {
        id: "31",
        menuName: "订单列表",
        menuCode: "31"
      },
      {
        id: "32",
        menuName: "退货列表",
        menuCode: "32",
        children: []
      }
    ]
  },
  {
    id: "4",
    menuName: "商家管理",
    menuCode: "",
    children: []
  }
];
import myTree from "./commonpage/treeMenu";
export default {
  name: "HelloWorld",
  components: {
    myTree
  },
  data() {
    return {
      theData: myData,
      menuName: "menuName", // 显示菜单名称的属性
      loading: false
    };
  },
  methods: {
    //获取到 树体传来的数据
    getSubMenu(item, index) {
      alert("item" + JSON.stringify(item));
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
