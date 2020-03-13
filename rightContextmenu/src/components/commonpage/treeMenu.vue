<template>
  <ul class="tree-menu" style="padding-left:40px">
    <li v-for="(item, index) in data" :key="index">
      <span @click="toggle(item, index)" @contextmenu.prevent="rightBtn(item,index)">
        <i
          :class="['icon',
           item.children && item.children.length ? folderIconList[index] : 'file-text',
            loading ? loadingIconList[index] : ''
            ]"
        ></i>
        <!-- {{ item.self.menuName}} -->
       {{item[name] || item.menuName }} 
      </span>
      <tree-menu v-if="scope[index]" :data="item.children"></tree-menu>
    </li>
  </ul>
</template>
 
<script>
export default {
  name: "treeMenu",
  props: {
    data: Array,
    name: String,
    loading: Boolean
  },
  data() {
    return {
      folderIconList: [],
      loadingIconList: [],
      scope: {},
       menuName:""
    };
  },
  created () {
    this.data.forEach((item, index) => {
      if (item.children && item.children.length) {
        this.folderIconList[index] = 'folder';
      }
    });
  },
  mounted() {},
  methods: {
    doTask(index) {
      this.$set(this.scope, index, !this.scope[index]);
      this.folderIconList[index] = this.scope[index] ? "folder-open" : "folder";
    },
    //--向页面传数据
    toGetSubMenu(item) {
      var parent = this;
      var numlen = JSON.stringify(item);
      if(numlen.children==undefined||numlen.children==[]){
        for (var i = 1; i < item.id.length; i++) {
        parent.$parent.$emit("getSubMenu", item);
        parent = parent.$parent;
      }
       this.$emit("getSubMenu", item);
      }
       
    },
    toggle(item, index) {
      this.loadingIconList = [];
      if (item.children && item.children.length > 0) {
        item.children.forEach((item, index) => {
          this.folderIconList[index] = "folder";
        });
        this.doTask(index);
      } else {
        this.loadingIconList[index] = "loading";
      }
    },
    //右击出现功能菜单
    rightBtn(item, index) {
      // console.log(JSON.stringify(item));
      this.toGetSubMenu(item);
      //判断有子终节点的出现添加权限
      if (item.children && item.children.length > 0) {
        return;
      } else {
        //-- 右击内容  定位 --
        let menu = document.getElementById("msgRightMenu");
        var e = e || window.event;
        var marginWidths = 80;
        var marginHeight = 110;
        //鼠标点的坐标
        var oX = e.clientX - marginWidths;
        var oY = e.clientY - marginHeight;
        if (oX > 423) {
          oX = 423;
        }
        //菜单出现后的位置
        menu.style.display = "block";
        menu.style.left = oX + 320 + "px";
        menu.style.top = oY + 110 + "px";
        document.onclick = function() {
          //点击其他位置 右击内容消失
          document.getElementById("msgRightMenu").style.display = "none";
        };
        //阻止浏览器默认事件
        return false; //一般点击右键会出现浏览器默认的右键菜单，写了这句代码就可以阻止该默认事件
      }
    },
    // setFolder() {
    //   this.data.forEach((item, index) => {
    //     if (item.children && item.children.length) {
    //       this.folderIconList[index] = "folder";
    //     }
    //   });
    // }
  },
  // watch: {
  //   data(val) {
  //     this.setFolder();
  //   }
  // }
};
</script>
 
<style scoped>
.tree-menu {
  list-style: none;
}
.tree-menu li {
  line-height: 2;
}
.tree-menu li span {
  cursor: default;
}
.icon {
  display: inline-block;
  width: 15px;
  height: 15px;
  background-repeat: no-repeat;
  vertical-align: -2px;
}
.icon.folder {
  background-image: url(../../assets/img/myimg/folder.jpg);
  background-size: 14px 13px;
}
.icon.folder-open {
  background-image: url(../../assets/img/myimg/folder-open.jpg);
  background-size: 14px 13px;
}
.icon.file-text {
  background-image: url(../../assets/img/myimg/text.png);
  background-size: 12px 15px;
}
.icon .loading {
  background-image: url(/src/assets/loading.gif);
  background-size: 15px;
}
</style>