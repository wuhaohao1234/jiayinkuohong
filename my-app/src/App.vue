<template>
  <v-app>
    <v-content>
      <div class="header">
        <h1 class="text-center">面试题</h1>
        <v-btn class="mx-2" fab dark large color="cyan" @click="display=!display">
          <v-icon dark>mdi-pencil</v-icon>
        </v-btn>
      </div>
      <div class="fluid container">
        <div v-show="!display" class="col-md-10">
          <draggable
            class="list-group"
            tag="ul"
            v-model="list"
            v-bind="dragOptions"
            :move="onMove"
            @start="isDragging=true"
            @end="isDragging=false"
          >
            <transition-group type="transition" :name="'flip-list'">
              <v-list-item class="list-group-item" v-for="item in list" :key="item.order">
                <v-list-item-content>
                  <!-- <v-list-item-title>{{item.name}}</v-list-item-title> -->
                  <v-text-field v-model="item.name" required></v-text-field>
                </v-list-item-content>
              </v-list-item>
            </transition-group>
          </draggable>
        </div>
        <div v-show="display" class="col-md-10">
          <draggable
            class="list-group"
            tag="ul"
            v-model="list"
            v-bind="dragOptions"
            :move="onMove"
            @start="isDragging=true"
            @end="isDragging=false"
          >
            <transition-group type="transition" :name="'flip-list'">
              <v-list-item class="list-group-item" v-for="item in list" :key="item.order">
                <v-list-item-content>
                  <v-list-item-title>{{item.name}}</v-list-item-title>
                  <!-- <v-text-field v-model="item.name" required></v-text-field> -->
                </v-list-item-content>
              </v-list-item>
            </transition-group>
          </draggable>
        </div>
      </div>
    </v-content>
  </v-app>
</template>

<script>
import draggable from "vuedraggable";

const message = [
  "欢迎来到嘉印阔鸿网络科技有限公司",
  "这是一道简单的面试题目",
  "请用您最擅长的技术去实现它",
  "请勿造轮子",
  "要求：",
  "1. 实现以下效果,视觉可自行美化",
  "2. 点击编辑，所有内容可编辑，可考虑富文本编辑器或者contenteditable",
  "3. 将所有编辑信息按顺序存储在浏览器network中",
  "4. 选项可拖拽顺序",
  "5. 实现基本的CRUD(增删该查)",
  "加分项：",
  "1. 使用typescript加分",
  "2. 良好的命名规范与代码结构加分",
  "3. 单元测试全覆盖加分",
  "4. 代码部署在github可展示git提交记录加分",
  "5. git commit 规范加分",
  "6. 项目包含持续集成(travis-ci或circle-ci)加分,说明文件中显示",
  "7. 使用函数式编程加分",
  "8. 注释包含jsdoc加分",
  "9. npm换源加分",
  "10. vue.config.js设置路径别名加分"
];

export default {
  name: "App",

  components: {
    draggable
  },
  data: () => ({
    list: message.map((name, index) => {
      return { name, order: index + 1, fixed: false };
    }),
    list2: [],
    isDragging: false,
    delayedDragging: false,
    name: "",
    display: true
  }),
  methods: {
    orderList() {
      this.list = this.list.sort((one, two) => {
        return one.order - two.order;
      });
    },
    onMove({ relatedContext, draggedContext }) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      return (
        (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
      );
    }
  },
  computed: {
    dragOptions() {
      return {
        animation: 0,
        group: "description",
        ghostClass: "ghost"
      };
    },
    listString() {
      return JSON.stringify(this.list, null, 2);
    },
    list2String() {
      return JSON.stringify(this.list2, null, 2);
    }
  },
  watch: {
    isDragging(newValue) {
      if (newValue) {
        this.delayedDragging = true;
        return;
      }
      this.$nextTick(() => {
        this.delayedDragging = false;
      });
    }
  }
};
</script>
<style>
.header {
  display: flex;
  justify-content: space-around;
}
.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
/* .list-group {
  min-height: 20px;
} */
.list-group-item {
  cursor: move;
}
.list-group-item i {
  cursor: pointer;
}
</style>