<template>
  <div>
    <el-tree
      :data="menus"
      :props="defaultProps"
      :expand-on-click-node="false"
      show-checkbox
      :default-expanded-keys="expandedKey"
      node-key="catId"
    >
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button v-if="node.level<=2" type="text" size="mini" @click="() => append(data)">添加</el-button>
          <el-button
            v-if="node.childNodes.length==0"
            type="text"
            size="mini"
            @click="() => remove(node, data)"
          >删除</el-button>
          <el-button type="text" size="mini" @click="() => edit(data)">编辑</el-button>
        </span>
      </span>
    </el-tree>

    <el-dialog :title="dialogTitle" :visible.sync="dialogVisible" width="30%" :close-on-click-modal="false">
      <el-form :model="category">
        <el-form-item label="分类名称">
          <el-input v-model="category.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="图标">
          <el-input v-model="category.icon" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="计量单位">
          <el-input v-model="category.productUnit" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitData">确 定</el-button>
      </span>
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
      category: {
        catId: null,
        name: "",
        parentCid: 0,
        catLevel: 0,
        showStatus: 1,
        sort: 0,
        icon: null,
        productUnit: null,
      },
      dialogTitle: "",
      dialogType: "",
      dialogVisible: false,
      expandedKey: [],
      menus: [],
      defaultProps: {
        children: "children",
        label: "name",
      },
    };
  },
  //计算属性
  computed: {},
  //监控data中数据变化
  watch: {},
  //方法
  methods: {
    handleNodeClick(data) {
      //   console.log("当前节点", data);
    },
    getMenus() {
      this.$http({
        url: this.$http.adornUrl("/product/category/list/tree"),
        method: "get",
        params: this.$http.adornParams({}),
      }).then(({ data }) => {
        this.menus = data.data;
      });
    },
    submitData() {
      if (this.dialogType == "add") {
        this.addCategory();
      } else if (this.dialogType == "edit") {
        this.editCategory();
      }
    },
    append(data) {
      console.log("添加数据", data);
      var emptyCategory = {
        catId: null,
        name: "",
        parentCid: 0,
        catLevel: 0,
        showStatus: 1,
        sort: 0,
        icon: null,
        productUnit: null,
      };
      this.category = emptyCategory;
      this.category.parentCid = data.catId;
      this.category.catLevel = data.catLevel * 1 + 1;
      this.dialogVisible = true;
      this.dialogType = "add";
      this.dialogTitle = "添加分类";
    },
    //添加三级你分类方法
    addCategory() {
      console.log("要提交的三级分类数据", this.category);
      this.$http({
        url: this.$http.adornUrl("/product/category/save"),
        method: "post",
        data: this.$http.adornData(this.category, false),
      }).then((data) => {
        this.$message({
          type: "success",
          message: "菜单保存成功!",
        });
        //刷新出新的菜单
        this.getMenus();
        this.expandedKey = [this.category.parentCid];
        this.dialogVisible = false;
      });
    },
    remove(node, data) {
      console.log(node, data);
      this.$confirm(`是否删除【${data.name}】菜单`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.$message({
            type: "success",
            message: "删除成功!",
          });
          var ids = [data.catId];
          this.$http({
            url: this.$http.adornUrl("/product/category/delete"),
            method: "post",
            data: this.$http.adornData(ids, false),
          }).then((data) => {
            //刷新出新的菜单
            this.getMenus();
            //设置需要默认展开的菜单
            this.expandedKey = [node.parent.data.catId];
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
    edit(data) {
      console.log("要修改的数据", data);
      this.dialogVisible = true;
      this.dialogType = "edit";
      this.dialogTitle = "修改分类";
      //发送请求获取当前节点最新的数据
      this.$http({
        url: this.$http.adornUrl(`/product/category/info/${data.catId}`),
        method: "get",
      }).then(({ data }) => {
        console.log("当前节点最新数据", data);
        this.category = data.data;
      });
      //   this.category.name = data.name;
      //   this.category.catId = data.catId;
      //   this.category.icon=data.icon;
      //   this.category.productUnit=data.productUnit;
    },
    //修改三级分类数据
    editCategory() {
      this.$http({
        url: this.$http.adornUrl("/product/category/update"),
        method: "post",
        data: this.$http.adornData(this.category, false),
      }).then((data) => {
        this.$message({
          type: "success",
          message: "菜单修改成功!",
        });
        //刷新出新的菜单
        this.getMenus();
        this.expandedKey = [this.category.parentCid];
        this.dialogVisible = false;
      });
    },
  },
  //声明周期 - 创建完成（可以访问当前this实例）
  created() {
    this.getMenus();
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