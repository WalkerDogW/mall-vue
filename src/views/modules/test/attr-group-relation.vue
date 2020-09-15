<template>
  <div>
    <el-dialog :visible.sync="dialogFormVisible" :close-on-click-modal="false">
      <el-button type="primary" @click="addRelation">新建关联</el-button>
      <el-button
        type="danger"
        @click="batchDeleteRelation"
        :disabled="dataListSelections.length <= 0"
      >批量删除</el-button>
      <el-table
        :data="relationAttrs"
        border
        style="width: 100%"
        @selection-change="selectionChangeHandle"
      >
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column prop="attrId" label="#"></el-table-column>
        <el-table-column prop="attrName" label="属性名"></el-table-column>
        <el-table-column prop="valueSelect" label="可选值"></el-table-column>
        <el-table-column fixed="right" header-align="center" align="center" label="操作"></el-table-column>
      </el-table>
      <!--  -->
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  //import 引入的组件需要注入到对象中才能使用
  components: {},
  props: {},
  data() {
    //这里存数据
    return {
      dialogFormVisible: true,
      dataListSelections: [],
      relationAttrs: [],
    };
  },
  //计算属性
  computed: {},
  //监控data中数据变化
  watch: {},
  //方法
  methods: {
    //初始化数据
    init() {},
    //新建关联
    addRelation() {},
    //批量删除
    batchDeleteRelation() {},
    //表格选择项发生变化
    selectionChangeHandle() {},
  },
  //声明周期 - 创建完成（可以访问当前this实例）
  created() {},
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