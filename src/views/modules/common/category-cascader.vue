<template>
  <div>
    <el-cascader
      filterable
      clearable
      placeholder="试试搜索：手机"
      v-model="paths"
      :options="categorys"
      :props="setting"
    ></el-cascader>
  </div>
</template>

<script>
export default {
  //import 引入的组件需要注入到对象中才能使用
  components: {},
  props: {
    catelogPath: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    //这里存数据
    return {
      setting: {
        value: "catId",
        label: "name",
        children: "children",
      },
      categorys: [],
      paths: this.catelogPath,
    };
  },
  //计算属性
  computed: {},
  //监控data中数据变化
  watch: {
    catelogPath(v) {
      this.paths = this.catelogPath;
    },
    paths(v) {
      this.$emit("update:catelogPath", v);
      //还可以使用pubsub-js进行传值
      this.PubSub.publish("catPath", v);
    },
  },
  //方法
  methods: {
    getCategorys() {
      this.$http({
        url: this.$http.adornUrl("/product/category/list/tree"),
        method: "get",
      }).then(({ data }) => {
        this.categorys = data.data;
      });
    },
  },
  //声明周期 - 创建完成（可以访问当前this实例）
  created() {
    this.getCategorys();
  },
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //声明周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁之后
  activated() {}, //缓存keep-alive
};
</script>

<style scoped>
</style>